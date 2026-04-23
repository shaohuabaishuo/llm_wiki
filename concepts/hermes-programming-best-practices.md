---
title: Hermes 编程最佳实践
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [agent, cli, tutorial, reference, pipeline]
sources: []
confidence: high
---

# Hermes 编程最佳实践

> 从零开始使用 Hermes Agent 进行高效编程的导航指南。
> 所有详细工作流和操作说明都在专用页面中，本文提供路线图和链接。

## 核心原则

1. **用对工具** — 读文件用 `read_file`，编辑用 `patch`，搜索用 `search_files`，写文件用 `write_file`。避免用 `cat/grep/sed/find` 等 shell 命令替代。
2. **先计划再动手** — 复杂任务先用 `writing-plans` skill 写计划。
3. **先测试再实现** — 遵循 `test-driven-development` skill 的红-绿-重构循环。
4. **先找根因再修复** — 遇到 bug 先调查根因（见 `systematic-debugging` skill）。
5. **提交前审查** — 见 `requesting-code-review` skill。

## 推荐的开发循环

```
需求理解 → 写计划 → todo 清单 → TDD 实现 → 代码审查 → 提交
```

每个环节的详细流程见对应 skill：

| 环节 | Skill/页面 |
|------|-----------|
| 写计划 | `writing-plans` skill |
| 任务跟踪 | `todo` 工具 |
| TDD 实现 | `test-driven-development` skill |
| 调试 | `systematic-debugging` skill |
| 代码审查 | `requesting-code-review` skill |
| 多模块并行实现 | `subagent-driven-development` skill |
| 工具清单 | [[hermes-tool-ecosystem]] |

## 工具选择速查

```
任务             → 工具
─────────────────────────────────
读取文件         → read_file
创建文件         → write_file
修改文件         → patch
搜索内容         → search_files
搜索文件名       → search_files(target="files")
运行命令         → terminal
启动服务器       → terminal(background=true)
并行任务         → delegate_task
定时任务         → cronjob
保存记忆         → memory
搜索历史会话     → session_search
保存可复用流程   → skill_manage
加载 skill       → skill_view / /skill <名称>
```

## 关键技巧与陷阱

### 终端
- 普通命令用前端模式（即使 timeout=300 也能即时返回）
- 长时间任务用 `background=True` + `notify_on_complete=True`
- 交互式 CLI 需要 `pty=True`（如 Python REPL、htop）
- 永远不要在非 PTY 下运行 vim/nano — 会挂起
- Git 输出管道到 `cat` 避免分页截断

### 文件
- `patch` 使用模糊匹配，不怕缩进差异，优先于 sed
- `write_file` 会自动创建父目录
- 大文件用 `offset`/`limit` 分页读取

### 上下文管理
- 跨会话偏好和事实存 `memory`，不要存任务进度
- 回忆过去的会话用 `session_search`（零 LLM 成本浏览）
- 可复用流程存为 skill（`skill_manage`），复杂任务后主动询问用户
- 使用过的 skill 如果有错误或过时，立即 `skill_manage(action="patch")` 更新

### Git
- 子代理使用 `-w`（worktree 模式）避免冲突
- 每个子代理独立提交

## 相关页面

- [[hermes-agent-installation]] — 安装与配置
- [[hermes-tool-ecosystem]] — 工具生态清单
- [[llm-wiki]] — 知识库模式
- 以下 inline-skills（在 session 中用 `/skill <名称>` 加载）：
  - `writing-plans` — 生成实现计划
  - `test-driven-development` — TDD 循环详解
  - `systematic-debugging` — 四阶段调试法
  - `subagent-driven-development` — 子代理并行开发
  - `requesting-code-review` — 提交前审查流水线
