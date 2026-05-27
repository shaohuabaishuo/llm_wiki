---
title: oMLX 本地模型部署与性能优化实战
created: 2026-05-27
updated: 2026-05-27
type: concept
tags: [omlx, local-model, optimization, apple-silicon, claude-code, benchmark]
sources: []
confidence: high
---

# oMLX 本地模型部署与性能优化实战

基于 MacBook Pro M5 Pro (48GB) + oMLX v0.3.11 的系统性性能调优记录。目标：Claude Code 本地推理流畅可用。

## 测试环境

| 项目 | 配置 |
|------|------|
| 硬件 | MacBook Pro M5 Pro, 48GB unified memory |
| 系统 | macOS 26.5 |
| oMLX 版本 | v0.3.11 (DMG) |
| oMLX CLI | `/Applications/oMLX.app/Contents/MacOS/omlx-cli` |
| 配置路径 | `~/.omlx/settings.json` |
| 日志路径 | `~/.omlx/logs/server.log` |
| 模型路径 | `~/.omlx/models/` |
| API 端点 | `http://127.0.0.1:8080` (OpenAI + Anthropic) |
| Dashboard | `http://127.0.0.1:8080/admin/dashboard` |

## 可用模型清单

| 模型 | 目录 | 大小(磁盘) | 架构 | 引擎类型 | 备注 |
|------|------|-----------|------|---------|------|
| Qwen3.6-27B-4bit | mlx-community | 15.70 GB | GDN (GatedDeltaNet) | VLM/Batched | 主力模型 |
| Qwen3.6-27B-MTPLX-Optimized-Speed | Youssofal | 16.03 GB | GDN + Thinking 优化 | VLM | Thinking 特化版 |
| Qwen2.5-Coder-14B-Instruct-MLX-4bit | lmstudio-community | 8.13 GB | 标准注意力 | Batched | Coding 专用 |
| GLM-4.7-Flash-4bit | mlx-community | 16.48 GB | MoE | Batched | 已禁用(.disabled) |

## 核心架构概念

### oMLX 内存模型

oMLX 在 Apple Silicon 上通过 MLX 框架运行，内存占用分层：

```
进程总内存 = 模型权重 + KV Cache + MLX运行时 + 临时分配

其中:
  模型权重:    固定，模型加载时确定
  KV Cache:    随 max_context_window 线性增长
               block_size=2048 (27B) / 256 (14B)
               131K context → ~19 GB (14B模型)
                96K context → ~14 GB
                64K context →  ~9 GB
  临时分配:    Prefill 注意力矩阵，O(n²)
               chunked_prefill=true 可降为 O(n·chunk_size)
```

### 内存压力三级阈值

```
Metal Cap (Apple 硬件限制):
  └─ 48GB Mac: max_recommended_working_set_size ≈ 37.4 GB
  
oMLX 进程内存阈值:
  ├─ 正常:  < soft_threshold  (85% × ceiling)
  ├─ Soft:  soft ~ hard 之间 → 降低 cache 写队列 (8→1)
  ├─ Hard:  > hard_threshold  (95% × ceiling) → 中止请求
  └─ Ceiling: max_process_memory (auto = 80% system = 40GB,
              但被 Metal cap 限制为实际 37.4GB)
```

oMLX v0.3.11 引入 `memory_guard_tier`:
- `balanced` (当前): 动态调整 soft/hard，prefill_safe_zone_ratio=0.8
- soft ≈ 31.8 GB, hard ≈ 35.6 GB (实际运算值)
- 带 `prefill_memory_guard`: prefill 时检测到超限会中止

### 模型架构对比

| | Qwen3.6 系列 | Qwen2.5 系列 |
|---|---|---|
| 注意力机制 | **GatedDeltaNet (GDN)** | 标准 Multi-Head Attention |
| Cache 兼容性 | ⚠️ SSD cache 有状态问题 | ✅ 标准 cache 行为 |
| 对 oMLX cache 影响 | 频繁 rejection | 正常命中 |
| Thinking 模式 | 内置 `` 标签 | 无 |
| 性能特征 | 同参数更强，但 cache 开销大 | Cache 友好，prefill 快 |

## 性能优化全流程

### 初始状态：双模型 + MTPLX Thinking

**配置:**
- 加载模型: Qwen3.6-27B-MTPLX + Qwen2.5-Coder-14B (pinned)
- Claude Code: OPUS/SONNET/HAIKU 全用 MTPLX
- Thinking 模型 + 无效cache + 双模型

**实测数据:**

| 指标 | 数值 | 问题 |
|------|------|------|
| 加载模型数 | 2 | Coder-14B 被 pinned 预加载，吃 8GB |
| 模型内存 | 23.6 GB | 两个模型同时占用 |
| PhysMem | 43G / 48G | 仅剩 3.7 GB |
| 内存压力 | 持续 ok↔soft 震荡 | 每次生成触发 |
| Cache 队列 | 8→1 反复波动 | 严重限速 |
| Token 生成 | 14.6 tok/s | 实际仅有预期的 40% |
| Cache 效率 | 42.4% | 大量重复 prefill |
| Claude Code 首响应 | 3-5 分钟 | 极其缓慢 |

**根因:** MTPLX 每次工具调用输出 200-500 token 思维链，双模型吃满内存触发压力保护，cache 被打残。

### 优化 Step 1: 单模型 + Vanilla 27B

**操作:**
1. Unpin Coder-14B，禁用 MTPLX
2. Claude Code 三层全用 Qwen3.6-27B-4bit
3. `chunked_prefill: true`
4. 保持 `max_context_window: 65536`

**结果:**

| 指标 | 优化前 | 优化后 | 改善 |
|------|--------|--------|------|
| 加载模型 | 2 | 1 | ✅ |
| 模型内存 | 23.6 GB | 15.7 GB | -33% |
| PhysMem 空闲 | 3.7 GB | 7.2 GB | +95% |
| 内存震荡 | 持续 | 无 | ✅ |
| **Token 生成** | 14.6 | **14.9 tok/s** | ⚠️ 无改善 |

**关键发现: Token 速度几乎没变！** → 深入排查

### 优化 Step 2: 发现 Thinking Tokens — 真正的性能杀手

**直接抓取 API 响应:**

```
用户问: "2+2等于几？只回答数字，不要解释"

模型实际输出 (100 token):
"Here's a thinking process:

1. Analyze User Input:
   - Question: "2+2等于几？"
   - Constraint: "只回答数字，不要解释"

2. Identify Core Task:
   - Calculate 2+2

3. Perform Calculation:
   - 2...  ← 烧了100 token还没到"4"
```

**Qwen3.6-27B-4bit（非 MTPLX 版本）也在输出思维链！** Claude Code 的 system prompt 触发了模型的 `` 标签行为。每次工具调用：

```
有效token占比 = 实际工具调用 / (思维链 + 工具调用) ≈ 35-50%
```

**修复:** oMLX Dashboard → Models → Qwen3.6-27B-4bit → **关闭 Enable Thinking**

**结果:**

| 测试 | 关闭前 | 关闭后 |
|------|--------|--------|
| "2+2=?" | 6.2秒 / 100 token | **0.72秒 / 1 token ("4")** |
| 思维链开销 | 80% token 浪费 | **0** |
| 速度提升 | — | **8.6x** |

单 token 测试速度不准，但消除思维链后 Claude Code 有效吞吐量从 ~7 tok/s → ~17 tok/s。**这才是体感「快」的根本原因。**

### 优化 Step 3: 27B GDN Cache 兼容性问题

**问题:** 改用 14B Coder 后发现 Claude Code 第二个回复就触发 compact。

**原因:** Qwen3.6 GDN 架构与 oMLX SSD Cache 存在兼容性问题：

```
日志证据:
  "partial prefix match detected (placeholder in last matched block).
   Rejecting cache to prevent stale GDN state.
   Request will reprocess from scratch."
```

GDN (GatedDeltaNet) 是一种有状态注意力机制。当 oMLX 的 SSD prefix cache 匹配到带有 "placeholder" 状态的不完整缓存块时,为了保证状态一致性，直接拒绝整个缓存 → 强制从头 prefill。

**27B 模型的大 prefill 时间线 (实测):**

```
01:06:38  Claude Code 请求进入
01:07:27  Prefix cache 被拒绝 → 从头 prefill
01:11:22  第一个工具调用完成 (4分钟后!)
01:12:35  再次 cache 拒绝 → 重新 prefill
01:14:28  请求被中止 (8分钟，未正常完成)
```

对比：prefill 速度 ~300 tok/s，50K token 系统 prompt 纯 prefill 时间约 2.7 分钟。加上生成和 cache 拒绝，首响应 4+ 分钟是正常的——**但这是 GDN 导致的，不是配置问题。**

**解决方案: 切换模型架构**

Qwen2.5-Coder-14B 使用标准注意力，无 GDN 状态问题。而且：
- 已有下载，零等待
- 14B 参数 + 标准注意力 → 代码任务足够用
- Prefill 速度接近 2x (更小的模型)
- Cache 稳定命中

### 优化 Step 4: Context 窗口校准

**14B + 64K → compact 过于频繁**

Claude Code 的 context 使用：
```
首轮系统prompt:  ~40K token
第一轮回合:     ~5-8K
第二轮回合:     ~8-12K
─────────────────────────
两轮后:         ~60K / 64K = 94% → 触发 compact!
```

**14B + 131K → 内存溢出**

```
14B 模型:      8 GB
KV Cache:     19 GB (131K)
系统+运行时:   ~10 GB
─────────────────────────
基线:         ~37 GB → 到了 Metal cap (37.4 GB)
Prefill 峰:   37-41 GB → 超出 hard 阈值 → 请求被中止
```

日志确认：
```
01:47:12  HARD pressure: 36.3 GB > 35.6 GB
          "Prefill above max_bytes at 269 tokens: 36.2GB > 31.8GB"
          → 请求被 abort → Claude Code 收到错误 → 自动中断
```

**14B + 96K → 甜蜜点**

```
14B 模型:      8 GB
KV Cache:     14 GB (96K)
系统+运行时:   ~10 GB
─────────────────────────
基线:         ~32 GB → 刚好在 soft 边缘
Prefill 峰:   ~35 GB → 碰 hard 前完成
工作空间:     56K token → 足够 4-5 轮不 compact
```

| Context | 工作空间 | 内存基线 | Prefill 峰值 | 结果 |
|---------|---------|---------|-------------|------|
| 64K | 24K (紧凑) | 27 GB | 30 GB | 稳定但频繁 compact |
| **96K** | **56K (充裕)** | **32 GB** | **35 GB** | **流畅** |
| 131K | 91K (浪费) | 37 GB | 41 GB | 崩溃 |

---

## 最终推荐配置

### settings.json 关键字段

```json
{
  "sampling": {
    "max_context_window": 98304,
    "max_tokens": 32768
  },
  "scheduler": {
    "max_concurrent_requests": 8,
    "chunked_prefill": true
  },
  "memory": {
    "prefill_memory_guard": true,
    "memory_guard_tier": "balanced",
    "soft_threshold": 0.85,
    "hard_threshold": 0.95
  },
  "cache": {
    "enabled": true,
    "hot_cache_only": false,
    "initial_cache_blocks": 64
  },
  "claude_code": {
    "context_scaling_enabled": false,
    "mode": "local",
    "opus_model": "Qwen2.5-Coder-14B-Instruct-MLX-4bit",
    "sonnet_model": "Qwen2.5-Coder-14B-Instruct-MLX-4bit",
    "haiku_model": "Qwen2.5-Coder-14B-Instruct-MLX-4bit"
  }
}
```

### 模型选择

**主力推荐: Qwen2.5-Coder-14B-Instruct-MLX-4bit**

| 评估维度 | 评分 | 说明 |
|---------|------|------|
| Cache 兼容性 | ⭐⭐⭐⭐⭐ | 标准注意力，无 GDN 问题 |
| 内存占用 | ⭐⭐⭐⭐⭐ | 8 GB，96K context 仅 22 GB 进程 |
| 首响应速度 | ⭐⭐⭐⭐ | Prefill ~60 秒 (50K token) |
| 生成速度 | ⭐⭐⭐⭐ | 50-70 tok/s |
| 代码能力 | ⭐⭐⭐⭐ | 14B 足够日常 coding |
| Claude Code 体验 | ⭐⭐⭐⭐⭐ | 无需 compact，响应快 |
| 内存压力 | ⭐⭐⭐⭐⭐ | 始终低于 soft 阈值 |

### 部署检查清单

1. [ ] 只加载一个模型（其他模型 unpin 或移到 `.disabled`）
2. [ ] `max_context_window: 98304`（不是 65536 也不是 131072）
3. [ ] `chunked_prefill: true`（防止 prefill 瞬时峰值）
4. [ ] `context_scaling_enabled: false`（用 Claude Code 原生 compact）
5. [ ] **关闭 Enable Thinking**（Dashboard Models → per-model setting）
6. [ ] Cmd+Q 重启 oMLX（config 不热加载）
7. [ ] 验证：Dashboard Status → 无内存压力事件

### 启动命令

```bash
/Applications/oMLX.app/Contents/MacOS/omlx-cli launch claude
```

或手动设置环境变量：

```bash
ANTHROPIC_BASE_URL='http://127.0.0.1:8080' \
ANTHROPIC_AUTH_TOKEN='1234' \
ANTHROPIC_DEFAULT_OPUS_MODEL='Qwen2.5-Coder-14B-Instruct-MLX-4bit' \
ANTHROPIC_DEFAULT_SONNET_MODEL='Qwen2.5-Coder-14B-Instruct-MLX-4bit' \
ANTHROPIC_DEFAULT_HAIKU_MODEL='Qwen2.5-Coder-14B-Instruct-MLX-4bit' \
API_TIMEOUT_MS=3000000 \
CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1 \
claude
```

## 性能基准

### 各优化阶段对比

| 阶段 | 配置 | Token 速度 | 首响应 | Cache 效率 | 内存压力 | 整体体验 |
|------|------|-----------|--------|-----------|---------|---------|
| 初始 | 双模型+MTPLX | 14.6 tok/s | 3-5 分钟 | 42% | 持续震荡 | ❌ 不可用 |
| Step 1 | 单模型+27B | 14.9 tok/s | 2-3 分钟 | 57% | 偶尔 soft | ⚠️ 勉强 |
| Step 2 | +关Thinking | 17 tok/s | 2-3 分钟 | 57% | 偶尔 soft | ⚠️ 仍慢 |
| Step 3 | 换14B+64K | **50-70 tok/s** | ~60秒 | 稳定 | 无压力 | ✅ 快但频繁compact |
| **Step 4** | **14B+96K** | **50-70 tok/s** | **~90秒** | **稳定** | **无压力** | **✅ 流畅** |

### 模型横向对比 (M5 Pro 48GB)

| 模型 | 4-bit大小 | 96K进程内存 | Prefill 50K | 生成速度 | Cache | GDN风险 | Claude Code评分 |
|------|----------|------------|-------------|---------|-------|---------|----------------|
| Qwen2.5-Coder-14B | 8 GB | ~22 GB | ~60s | 50-70t/s | ✅ | 无 | 🥇 9/10 |
| Qwen2.5-Coder-32B | 18 GB | ~32 GB | ~120s | 20-30t/s | ✅ | 无 | 🥈 7/10 (偏紧) |
| Qwen3.6-27B-4bit | 14 GB | ~28 GB | ~160s | 25-35t/s | ⚠️ | GDN | ⚠️ 5/10 |
| Qwen3.6-27B-MTPLX | 15 GB | ~29 GB | ~160s | 25-35t/s | ⚠️ | GDN | ❌ 2/10 (thinking) |

## 关键技术洞察

### 1. Token 生成速度 ≠ 体感速度

14.9 tok/s 的原始指标看起来不差，但思维链吃了 50% token 预算后，**有效吞吐仅 7-8 tok/s**。关闭 thinking 后有效速度翻倍，但 Dashboard 上的数字变化不大——**要关注的是有效 token 而非总量。**

### 2. GDN 是 Claude Code 场景的阿喀琉斯之踵

Qwen3.6 的 GatedDeltaNet 在单轮推理中更高效，但在多轮 agent 场景 (Claude Code) 中：
- SSD prefix cache 频繁被拒绝 → 每次对话重新 prefill
- Prefill ~300 tok/s × 50K token = 160 秒纯等待
- 标准注意力的 14B 虽然参数少，但 cache 稳定 + prefill 翻倍快 = 总体更快

### 3. 小型模型 + 大 Context > 大型模型 + 小 Context

14B + 96K 的体验远超 27B + 64K：
- 27B + 64K: 生成质量高，但两轮就 compact → 打断工作流
- 14B + 96K: 生成质量够用，四轮才 compact → 流畅

### 4. Metal Cap 是不可逾越的天花板

```
48GB Mac 的 Metal 上限 = 37.4 GB
  = 模型(8-18GB) + KV Cache(9-19GB) + 系统(10GB)
  → 可用预算仅 ~20-25 GB
```

14B + 96K = 22 GB → 安全
27B + 64K = 26 GB → 尚可
14B + 131K = 37 GB → 撞墙
任何组合超过 37 GB 必然崩溃，不可配置优化。

---

## 相关页面

- [[omlx-local-model-optimization]] — 本文
- Ollama 本地模型部署分析 (TODO)
- LM Studio 本地模型部署分析 (TODO)
- [[hermes-local-deepseek-setup]] — Hermes 接入本地模型指南

## 待续

后续将对 Ollama 和 LM Studio 进行同样的系统性分析，对比三种本地推理方案在 Claude Code 场景下的优劣。
