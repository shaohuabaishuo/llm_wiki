---
title: Hermes Agent 接入本地 DeepSeek 模型完整配置指南
created: 2026-04-24
updated: 2026-04-24
type: concept
tags: [configuration, troubleshooting, ollama, wsl, local, deepseek, hermes, tutorial]
sources: []
confidence: high
---

# Hermes Agent 接入本地 DeepSeek 模型完整配置指南

从零开始，将 Hermes Agent 接入本地 Ollama 运行的 DeepSeek 模型，包含 WSL2 网络配置、Hermes 配置、模型切换的完整操作流程。

> **适用场景：** Ollama 运行在 Windows 宿主机，Hermes Agent 运行在 WSL2 中。纯 Linux 用户可跳过 WSL 网络配置步骤。

---

## 1. 环境概览

### 硬件配置（参考）

| 组件 | 配置 |
|------|------|
| GPU | NVIDIA GeForce RTX 2080 Ti 11GB VRAM |
| 系统内存 | 8GB DDR4 |
| OS | Windows 11 + WSL2 (kernel 2.6.3) |
| CUDA | 13.2 |

### 软件栈

```
┌─────────────────────────────────────┐
│  Windows 宿主机                      │
│  ┌───────────┐  ┌─────────────────┐ │
│  │  Ollama   │  │  Ollama Models  │ │
│  │ :11434    │  │  deepseek-r1:8b │ │
│  │ (GPU推理) │  │  qwen2.5-coder  │ │
│  └─────┬─────┘  └─────────────────┘ │
├────────┼────────────────────────────┤
│  WSL2  │  mirrored networking       │
│  ┌─────┴──────────────────────────┐ │
│  │  Hermes Agent                  │ │
│  │  ~/.hermes/config.yaml         │ │
│  │  custom_providers:             │ │
│  │    local-deepseek              │ │
│  └────────────────────────────────┘ │
└─────────────────────────────────────┘
```

---

## 2. 前提条件

| 组件 | 要求 | 验证命令 |
|------|------|----------|
| Ollama | Windows 安装，至少拉取一个模型 | `ollama list` |
| Hermes Agent | WSL2 中已安装 | `hermes --version` |
| WSL2 | 已配置 GPU 直通 | `nvidia-smi` |

---

## 3. WSL2 网络配置

### 3.1 推荐方案：Mirrored 网络模式

创建/编辑 `%USERPROFILE%\.wslconfig`（Windows 侧，`C:\Users\<用户名>\.wslconfig`）：

```ini
[wsl2]
networkingMode=mirrored
memory=4GB
processors=4
```

> **优点：** WSL2 内的 `localhost` 直接指向 Windows 宿主机，无需手动查找 IP。支持 Windows 11 22H2+。

**使配置生效：** 在 PowerShell 中执行 `wsl --shutdown`，然后重新打开 WSL2 终端。

### 3.2 备选方案：NAT 模式 + 主机 IP

如果无法使用 mirrored 模式（Windows 10 / 旧版），需使用 NAT 模式的 Windows 宿主机 IP。

**让 Ollama 监听所有接口**（Windows 环境变量）：
- 变量名: `OLLAMA_HOST`
- 变量值: `0.0.0.0`

**从 WSL2 获取 Windows 宿主 IP：**
```bash
ip route show | grep -i default | awk '{print $3}'
# 通常返回 172.x.x.1
```

> ⚠️ **注意：** NAT 模式下，WSL2 每次重启后 IP 可能变化，需重新获取。推荐使用 mirrored 模式避免此问题。

---

## 4. Ollama 关键配置

### 4.1 上下文长度（CRITICAL）

Ollama 根据 VRAM 自动设置上下文长度，但默认值对 agent 使用严重不足：

| 可用 VRAM | Ollama 默认 context | Agent 最低需求 |
|-----------|---------------------|----------------|
| < 24 GB | **4,096 tokens** | ≥ 32,768 tokens |
| 24–48 GB | 32,768 tokens | ✓ 可用 |
| 48+ GB | 256,000 tokens | ✓ 充足 |

**必须手动设置！** 在 Windows 系统环境变量中添加：

- 变量名: `OLLAMA_CONTEXT_LENGTH`
- 变量值: `32768`

**设置后重启 Ollama**（系统托盘右键 → Quit，再重新打开）。

**验证：**
```bash
ollama ps
# 查看 CONTEXT 列，应显示 32768
```

> Agent 使用中，仅系统 prompt + 工具 schema 就占用 4k–8k tokens。4096 上下文意味着模型会丢失对话历史，产生不连贯的响应。

### 4.2 推荐模型

| 模型 | 参数 | 大小 | 工具调用 | 适用场景 |
|------|------|------|----------|----------|
| `deepseek-r1:8b` | 8.2B Q4_K_M | 5.2GB | ❌ 不支持 | 推理、对话、内容生成 |
| `deepseek-r1:7b` | 7B Q4_K_M | 4.7GB | ⚠️ 视模板 | 备选（需测试） |
| `qwen2.5-coder:7b-instruct` | 7.6B Q4_K_M | 4.7GB | ⚠️ 文本输出 | 备选编程模型 |

**拉取模型：**
```powershell
# Windows PowerShell
ollama pull deepseek-r1:8b
ollama pull qwen2.5-coder:7b-instruct
```

---

## 5. Hermes 配置

### 5.1 编辑 `~/.hermes/config.yaml`

在配置文件中添加 `custom_providers` 段（使用 **named custom provider** 模式，可在云端和本地间自由切换）：

```yaml
model:
  default: deepseek-v4-pro        # 保持云端 DeepSeek 为默认
  provider: deepseek
  base_url: https://api.deepseek.com/v1

# ... 其他现有配置 ...

custom_providers:
  - name: local-deepseek
    base_url: http://localhost:11434/v1
    api_mode: chat_completions
    models:
      deepseek-r1:8b:
        context_length: 32768
      qwen2.5-coder:7b-instruct:
        context_length: 32768
```

### 5.2 各字段说明

| 字段 | 值 | 说明 |
|------|-----|------|
| `name` | `local-deepseek` | 自定义 provider 名称，用于 `/model` 切换 |
| `base_url` | `http://localhost:11434/v1` | Ollama OpenAI 兼容端点（mirrored 模式） |
| `api_mode` | `chat_completions` | 标准 OpenAI 格式 |
| `models.<id>.context_length` | `32768` | 与 `OLLAMA_CONTEXT_LENGTH` 保持一致 |

> **NAT 模式：** 将 `localhost` 替换为 Windows 宿主 IP（如 `http://172.27.16.1:11434/v1`）。

---

## 6. 模型切换

### 6.1 在 Hermes 会话中切换

```
# 切换到本地 DeepSeek
/model custom:local-deepseek:deepseek-r1:8b

# 切换回云端 DeepSeek
/model deepseek:deepseek-v4-pro

# 切换本地备选模型
/model custom:local-deepseek:qwen2.5-coder:7b-instruct
```

### 6.2 在终端中切换

```bash
# 退出当前会话
Ctrl+C

# 进入交互式设置
hermes model
# → 选择 "Custom endpoint"
# → 选择已配置的 "local-deepseek"
# → 选择模型
```

### 6.3 作为会话默认

修改 `config.yaml` 中的 `model` 段：

```yaml
model:
  default: deepseek-r1:8b
  provider: custom
  base_url: http://localhost:11434/v1
  context_length: 32768
```

> ⚠️ 这样设置后所有会话默认使用本地模型。推荐使用 named custom provider 方式，需要时临时切换。

---

## 7. 工具调用限制说明

### 7.1 当前状态

| 模型 | Ollama 工具调用 | 影响 |
|------|----------------|------|
| `deepseek-r1:8b` | ❌ 不支持 | Hermes 的 `web_search`、`terminal`、`file` 等工具无法使用 |
| `qwen2.5-coder:7b-instruct` | ⚠️ 文本格式 | 模型输出 JSON 工具调用文本，但不会被识别为结构化 tool_calls |

### 7.2 根本原因

Ollama 的工具调用依赖模型的 chat template 中包含 function calling 定义。部分模型（特别是 DeepSeek R1 distill 系列）的 Ollama 模板缺少此支持。

### 7.3 解决方案

**方案 A：使用替代推理引擎（推荐用于 agent 场景）**

- **llama.cpp `llama-server`**（加 `--jinja` 标志）：原生工具调用支持更好
- **vLLM**（加 `--enable-auto-tool-choice --tool-call-parser hermes`）：完整工具调用
- **LM Studio 0.3.6+**：GUI 配置，部分模型支持

**方案 B：接受限制，按场景使用**

| 场景 | 推荐模型 | 工具需求 |
|------|----------|----------|
| 代码审查、分析 | 云端 `deepseek-v4-pro` | 需要工具 |
| 内容生成、翻译 | 本地 `deepseek-r1:8b` | 不需要工具 |
| 推理、决策辅助 | 本地 `deepseek-r1:8b` | 不需要工具 |
| 编程辅助 | 云端或本地 `qwen2.5-coder` | 按需 |

---

## 8. 验证流程

### 8.1 验证 Ollama 连通性

```bash
# WSL2 内执行
curl -s http://localhost:11434/v1/models | python3 -m json.tool | grep '"id"'
```

预期输出：
```
"id": "deepseek-r1:8b",
"id": "qwen2.5-coder:7b-instruct",
```

### 8.2 验证模型推理

```bash
curl -s http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "deepseek-r1:8b",
    "messages": [{"role": "user", "content": "用一句中文打招呼"}],
    "max_tokens": 200
  }' | python3 -c "import sys,json;d=json.load(sys.stdin);print(d['choices'][0]['message']['content'])"
```

### 8.3 验证 Hermes 连接

```bash
# 启动 Hermes 并切换模型
hermes
# → /model custom:local-deepseek:deepseek-r1:8b
# → 输入测试消息：你好，请用中文回复
```

---

## 9. 常见问题

### Q: `Connection refused`？

- Ollama 未运行 → Windows 上启动 Ollama
- mirrored 模式未生效 → 执行 `wsl --shutdown` 后重新进入
- NAT 模式下 IP 变化 → 重新获取 IP：`ip route show | grep -i default | awk '{print $3}'`
- Windows 防火墙阻止 → 添加 11434 端口入站规则

### Q: 模型回复不连贯或忘记上文？

上下文长度过低。检查：
```bash
ollama ps   # 查看 CONTEXT 列
```
如低于 32768，按 4.1 章节设置 `OLLAMA_CONTEXT_LENGTH`。

### Q: deepseek-r1:8b 的 `reasoning` 字段是什么？

DeepSeek R1 是推理模型，响应包含两个部分：
- `reasoning`（思考过程）— 模型内部推理链条
- `content`（最终回答）— 实际输出

这导致每个回复消耗更多 tokens。对话中大约 80-90% 的输出 tokens 用于推理过程。

### Q: 如何在云端和本地间快速切换？

在 Hermes 会话中使用：
```
/model deepseek:deepseek-v4-pro          # 切换到云端
/model custom:local-deepseek:deepseek-r1:8b  # 切换到本地
```

切换即时生效，对话历史保留。

### Q: 系统重启后连接失败？

- **mirrored 模式：** `localhost` 始终有效，无需额外配置
- **NAT 模式：** Windows 宿主 IP 可能变化，需更新 `config.yaml` 中的 `base_url`

---

## 10. 配置检查清单

- [ ] Ollama 在 Windows 上正常运行（`ollama list` 有输出）
- [ ] `OLLAMA_CONTEXT_LENGTH=32768` 已设为 Windows 环境变量
- [ ] `.wslconfig` 已配置 `networkingMode=mirrored`
- [ ] WSL2 内 `curl localhost:11434/v1/models` 返回模型列表
- [ ] `~/.hermes/config.yaml` 中 `custom_providers` 段配置正确
- [ ] `hermes model` 可看到 `local-deepseek` provider
- [ ] `/model custom:local-deepseek:deepseek-r1:8b` 可切换成功

---

## 相关页面

- [[hermes-agent-installation]] — Hermes Agent 安装指南
- [[hermes-programming-best-practices]] — Hermes 编程最佳实践
- [[hermes-tool-ecosystem]] — Hermes 工具生态系统
- [[inkos-ollama-local-setup]] — Inkos 的类似配置（可参考 WSL2 网络部分）
- [[llm-wiki]] — LLM Wiki 知识库模式
