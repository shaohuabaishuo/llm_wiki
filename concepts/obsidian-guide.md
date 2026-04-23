---
title: Obsidian 入门使用说明书
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [obsidian, tutorial, reference, wiki]
sources: []
confidence: high
---

# Obsidian 入门使用说明书

> Obsidian 是一款强大的本地优先的笔记软件，基于 Markdown 文件，支持双向链接、图谱视图和丰富的插件生态。本文档从零开始，带你掌握 Obsidian 的核心用法。

---

## 一、Obsidian 是什么？

Obsidian 是一款**基于本地 Markdown 文件的笔记应用**。与 Notion、Evernote 等云端笔记不同，Obsidian 的所有笔记都是纯文本的 `.md` 文件，存放在你自己电脑上的一个文件夹中（这个文件夹称为 **仓库 / Vault**）。

**核心特点：**

| 特性 | 说明 |
|------|------|
| 本地优先 | 笔记存在你的硬盘上，无需联网，永不丢失 |
| Markdown 原生 | 使用纯文本格式，未来兼容性极强 |
| 双向链接 | `[[笔记名]]` 即可链接，自动生成反向链接面板 |
| 图谱视图 | 可视化你的知识网络，发现意想不到的连接 |
| 插件系统 | 超过 2000 个社区插件，可扩展任何功能 |
| 完全免费 | 个人使用全部免费，同步和发布需付费 |

---

## 二、安装与设置

### 2.1 下载安装

**桌面端（Windows / macOS / Linux）：**

1. 访问 [obsidian.md](https://obsidian.md) 点击 Download
2. 选择对应系统的安装包
3. 按提示完成安装

**移动端（iOS / Android）：**

- App Store 或 Google Play 搜索 "Obsidian" 下载
- 移动端与桌面端使用同一仓库可通过 Obsidian Sync 或第三方同步（iCloud / Syncthing）连接

### 2.2 创建第一个仓库

1. 打开 Obsidian，点击 **"Create new vault"**
2. 选择 **"Create new vault"**（或打开已有文件夹）
3. 输入仓库名称和存放路径
4. 点击 **"Create"** 即可进入

> 你的 LLM Wiki 仓库就是一个现成的 Obsidian 仓库！在创建仓库时可选择 **"Open folder as vault"** 指向 `~/llm_wiki`（WSL 下路径为 `/home/jingzhong/llm_wiki`）。

### 2.3 界面语言设置

1. 点击左下角 **齿轮图标 ⚙️** → **Settings**
2. 选择 **"About"** → **"Language"**
3. 选择 **"简体中文"**
4. 重启 Obsidian 生效

---

## 三、界面导航

默认界面分为三个区域：

```
┌──────────┬──────────────────────┬──────────┐
│ 侧边栏    │     编辑区            │ 右侧栏   │
│ (文件列表) │   (笔记内容)          │ (反向链接 │
│          │                      │  图谱等)  │
├──────────┴──────────────────────┴──────────┤
│                 状态栏                      │
└────────────────────────────────────────────┘
```

- **左侧边栏：** 文件浏览器、搜索、标签面板等
- **中间编辑区：** 笔记的编辑与预览（支持分屏）
- **右侧边栏：** 反向链接面板、图谱视图、大纲等
- **状态栏：** 底部显示字数、插件状态等

**快捷键（常用）：**

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+N` | 新建笔记 |
| `Ctrl+O` | 快速打开笔记 |
| `Ctrl+P` | 命令面板（Obsidian 的万能入口） |
| `Ctrl+E` | 切换编辑/预览模式 |
| `Ctrl+Shift+F` | 全文搜索 |
| `Ctrl+,` | 打开设置 |
| `Ctrl+[` / `Ctrl+]` | 后退/前进导航 |

---

## 四、核心概念

### 4.1 仓库（Vault）

仓库是一个**文件夹**，里面所有的 `.md` 文件都是你的笔记。你可以：
- 一个 Obsidian 窗口只能打开一个仓库
- 可以创建多个仓库，用不同窗口打开
- 仓库目录可以随意移动、备份（就是个文件夹）

### 4.2 笔记（Note）

每篇笔记就是一个 `.md` 文件。Obsidian 支持两种模式：
- **编辑模式：** 直接写 Markdown 源码
- **预览模式：** 渲染后的阅读视图

**推荐的笔记文件名规则：**
- 使用小写英文 + 连字符：`transformer-architecture.md`
- 或直接使用中文文件名：`Transformer 架构笔记.md`
- 保持一致即可

### 4.3 Markdown 基础

Obsidian 支持标准 Markdown 语法，以下是最常用的：

```markdown
# 一级标题
## 二级标题
### 三级标题

**加粗文字**
*斜体文字*
~~删除线~~
==高亮==（Obsidian 特有）

- 无序列表项
- 另一个列表项

1. 有序列表项
2. 另一个有序项

> 引用内容

`行内代码`

```代码块（三个反引号）

[链接文字](https://example.com)

![图片描述](路径/图片.png)
```

---

## 五、链接系统——最核心的功能

Obsidian 的链接系统是其灵魂所在。

### 5.1 内部链接（Wikilinks）

使用双括号创建笔记之间的链接：

```markdown
[[笔记名称]]
[[笔记名称|显示文字]]        ← 自定义链接文字
[[笔记名称#标题]]           ← 链接到笔记内的某个标题
[[笔记名称#^锚点]]          ← 链接到笔记内的某个锚点
```

**效果：**
- 点击链接跳转到目标笔记
- 目标笔记自动显示 **"反向链接"**（哪些笔记链接了它）
- 如果目标笔记不存在，链接显示为灰色（点击可创建）

### 5.2 反向链接（Backlinks）

这是 Obsidian 最强大的功能之一：
- 当你打开一篇笔记，右侧面板自动显示所有链接到此处的笔记
- 让你追踪一个想法的"引用来源"
- 点击反向链接即可跳转回源笔记

> 这就是 [[LLM Wiki]] 使用 `[[wikilinks]]` 的原因——整个知识库自动变为一个互联的网络。

### 5.3 链接图谱

所有 `[[wikilinks]]` 自动构成一个图结构——每篇笔记是一个节点，每个链接是一条边。Obsidian 的 **图谱视图（Graph View）** 就是这个图的视觉呈现。

---

## 六、标签系统

标签用于分类和过滤，语法为 `#标签名`：

```markdown
#markdown
#obsidian/插件
#项目/进行中
```

**标签面板**会自动收集所有使用的标签并显示数量，点击标签即可筛选出所有包含该标签的笔记。

> 在 LLM Wiki 中，标签遵循 [[LLM Wiki|SCHEMA.md]] 定义的分类体系，新标签需先添加再使用。

---

## 七、核心插件详解

Obsidian 内置了数十个核心插件，默认已启用最常用的。以下推荐启用并掌握：

### 7.1 文件浏览器（File Explorer）
默认开启。显示仓库的文件树，支持拖拽移动、右键菜单创建新文件。

### 7.2 搜索（Search）
`Ctrl+Shift+F` 全局搜索，支持：
- 正则表达式匹配
- 搜索文件名与内容
- 限定搜索路径
- 保存搜索结果为书签

### 7.3 图谱视图（Graph View）
点击左侧栏的**图谱图标**打开。两种模式：
- **全局图谱：** 显示整个仓库的笔记网络
  - 过滤：只显示带特定标签的笔记
  - 缩放：滚轮缩放，拖拽旋转
  - 点击节点跳转到对应笔记
- **局部图谱：** 在笔记中 `Ctrl+Click` 链接后选择 "打开图谱视图"

### 7.4 反向链接（Backlinks）
默认在右侧面板显示。展示当前笔记被哪些笔记引用，以及当前笔记内未链接的潜在引用（通过关键词匹配建议）。

### 7.5 大纲（Outline）
自动解析当前笔记的标题结构，显示为可点击的目录树。适合长文档导航。

### 7.6 模板（Templates）
**启用方法：** 设置 → 核心插件 → Templates → 启用

**设置步骤：**
1. 指定模板文件夹（如 `_templates/`）
2. 创建模板文件，如 `_templates/entity-template.md`：

```markdown
---
title: {{title}}
created: {{date}}
updated: {{date}}
type: entity
tags: []
sources: []
---

# {{title}}

## 概述

## 关键信息

## 相关链接
```

3. 在新笔记中按 `Ctrl+P` → "Template: Insert template" → 选择模板

### 7.7 日记（Daily Notes）
每天自动创建一篇日记笔记，适合记录日常。快捷键：`Ctrl+Shift+D`。

### 7.8 幻灯片（Slides）
将 Markdown 笔记以幻灯片形式展示。用 `---` 分隔每页幻灯片，点击状态栏的幻灯片图标即可演示。

---

## 八、社区插件——无限扩展

### 8.1 如何安装

1. 打开设置 ⚙️ → **Community plugins**
2. 点击 **"Turn on community plugins"**（首次需确认）
3. 点击 **"Browse"** 搜索插件
4. 找到插件后点击 **"Install"** → **"Enable"**

> 由于网络原因，部分中国大陆用户可能需要配置代理才能加载社区插件列表。

### 8.2 必装插件推荐

| 插件 | 用途 |
|------|------|
| **Dataview** | 用类似 SQL 的语法查询笔记元数据，自动生成索引、统计表 |
| **Templater** | 比内置模板更强大的模板引擎（支持脚本） |
| **Calendar** | 日历视图，与 Daily Notes 配合使用 |
| **Excalidraw** | 在笔记中嵌入手绘风格图表 |
| **Kanban** | 将笔记转为看板视图 |
| **Paste URL into selection** | 选中文字后粘贴 URL 自动生成 `[文字](链接)` |
| **Omnisearch** | 比内置搜索更快更准的全文搜索 |
| **Image Toolkit** | 增强图片查看功能（缩放、旋转等） |

### 8.3 Dataview 快速入门

Dataview 让你像操作数据库一样查询笔记。以下是一些常用查询：

```dataview
TABLE tags, created, confidence
FROM "entities"
WHERE contains(tags, "agent")
SORT created DESC
```

```dataview
LIST rows.file.link
FROM "concepts"
GROUP BY type
```

使用 Dataview 可以自动维护索引页面，无需手动更新。

---

## 九、图谱视图深度使用

### 9.1 过滤和搜索

在图谱视图中，你可以：
- 输入关键词仅显示匹配的节点
- 按标签筛选（标签下拉菜单）
- 点击空白处取消所有筛选

### 9.2 分组着色

设置 → Graph View → **Groups**：
- 添加分组规则，根据路径、标签等条件为节点着色
- 例如：`path: entities/` 显示蓝色，`path: concepts/` 显示红色

### 9.3 局部图谱

每篇笔记右上角点击 **"Open local graph"**，只显示与该笔记直接连接的一层或两层节点，方便聚焦。

---

## 十、实用技巧与工作流

### 10.1 快速操作

| 操作 | 方法 |
|------|------|
| 快速切换笔记 | `Ctrl+O` 输入文件名 |
| 拆分编辑窗口 | `Ctrl+Click` 笔记标题或 `Ctrl+\|` |
| 在文件中搜索替换 | `Ctrl+H` |
| 插入当前时间 | `Ctrl+T` |
| 合并笔记 | 同时打开两篇笔记，拖拽内容 |
| 导出 PDF | `Ctrl+Shift+P` → "Export to PDF" |

### 10.2 同步方案

| 方案 | 费用 | 特点 |
|------|------|------|
| **Obsidian Sync** | 付费 | 官方端到端加密，跨平台，历史版本 |
| **iCloud Drive** | 免费（有存储空间） | macOS/iOS 之间无缝同步 |
| **Syncthing** | 免费开源 | 多平台 P2P 同步，无中央服务器 |
| **Git** | 免费 | 版本控制，适合开发者，LLM Wiki 即用此方式 |

### 10.3 备份建议

- 定期 `git push` 到远程仓库（LLM Wiki 已配置）
- 或使用 Obsidian Sync 自动备份
- 重要笔记可导出为 PDF / HTML

### 10.4 写作效率建议

1. **只写链接，不急着写内容**——先创建骨架，再慢慢填充
2. **使用模板减少重复工作**——实体页、概念页都可预设模板
3. **每天写日记**——Daily Notes 是积累想法的起点
4. **定期回顾反向链接**——发现笔记之间的新联系
5. **不用过度分类**——链接比文件夹更重要，用 `[[wikilinks]]` 组织比用树形目录更灵活

---

## 十一、Obsidian + LLM Wiki 最佳实践

这个 Wiki 本身就是 Obsidian 仓库，以下是与 Obsidian 配合的关键点：

1. **在 Obsidian 中打开这个仓库**：打开 Obsidian → "Open folder as vault" → 选择 `~/wiki`（或 `~/llm_wiki`）
2. **[[Wikilinks]] 自动生效**：所有页面之间的链接在 Obsidian 中可点击跳转
3. **图谱视图可视化**：查看整个知识库的知识网络
4. **Dataview 查询**：可用 `FROM "entities"` 等查询自动生成动态列表
5. **设置附件路径**：Obsidian 设置 → Files & Links → Default location for attachments → 设为 `raw/assets/`
6. **开启 Wikilinks 格式**：设置 → Files & Links → Use [[Wikilinks]] → 开启

---

## 十二、常见问题

### Q: Obsidian 免费吗？
A: 个人使用完全免费。付费功能为 Obsidian Sync（云同步）和 Obsidian Publish（发布网站），非必需。

### Q: 笔记会丢失吗？
A: 笔记是本地纯文本文件，只要你的硬盘没问题就不会丢失。建议配合 Git 或云盘自动备份。

### Q: 如何在 WSL 中使用 Obsidian 打开 LLM Wiki？
A: Obsidian 运行在 Windows 上，LLM Wiki 在 WSL 中。Windows 可通过 `\\wsl$\Ubuntu\home\jingzhong\llm_wiki` 路径访问。在 Obsidian 中打开仓库时粘贴此路径即可。

### Q: 可以和别人协作编辑同一个仓库吗？
A: 可以，使用 Git 同步是最佳方案（LLM Wiki 已配置 GitHub 远程仓库），或使用 Obsidian Sync 共享仓库。

### Q: 移动端和桌面端如何同步？
A: 推荐 Obsidian Sync（官方解决方案），或使用 Syncthing（免费开源）同步整个仓库文件夹。

---

## 相关页面

- [[LLM Wiki]] — 本 Wiki 的完整使用模式说明
- [[Hermes Tool Ecosystem]] — Hermes Agent 工具生态全景

> 本文档的部分内容参考了 Obsidian 官方文档和社区最佳实践。
