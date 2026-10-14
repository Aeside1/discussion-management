---
name: discussion-management
description: >
  将深度 AI 对话整理到项目的 discussion/ 工作空间，以纯 Markdown 形式维护
  PROJECT.md 以及 inquiries/、artifacts/、work-items/ 记录。当会话包含需要
  澄清、研究、结论或后续工作的问题时使用；初始化项目的讨论管理时使用；将当前或
  其他会话处理为项目文档时使用；提取询问、声明、证据、决策或任务时使用；更新项目
  概览时使用。区分同一问题的不同推理路径和独立的询问与任务。
---

# 讨论管理

将当前 AI 会话转化为项目 `discussion/` 工作空间内的可维护文档。

```text
<项目目录>/
├── discussion/                讨论管理工作空间及其唯一写入边界
│   ├── PROJECT.md             唯一的项目文件：概览、索引和当前状态
│   ├── inquiries/             独立问题
│   ├── work-items/            后续询问和可执行任务
│   └── artifacts/             声明、证据、结论、假设、决策
└── <项目内容>                 代码、研究、笔记和其他项目材料
```

本 skill 创建或更新的所有文件都在 `discussion/` 下。

当前会话是临时输入。`session init` 的输出是持久记录文件：Inquiry、WorkItem 和 Artifact 文档。

## 阅读相关参考

- 实体模型、项目根、文件命名、纯 Markdown 模板：[references/model.zh.md](references/model.zh.md)
- 自动分类、提取和更新工作流：[references/workflow.zh.md](references/workflow.zh.md)
- 使用 `session init` 处理当前或外部早期对话：[references/import.zh.md](references/import.zh.md)
- `PROJECT.md` 布局、排序、链接和快速阅读显示：[references/presentation.zh.md](references/presentation.zh.md)

在修改项目文件前阅读所需操作涉及的所有参考文档。

## 核心分类

对当前会话的每个实质性部分，将其分类为以下一种或多种：

```text
continuation | clarification | research | claim | evidence
conclusion | assumption | decision | path | inquiry | task
```

使用以下规则路由：

```text
相同问题 + 相同目标 + 相同完成标准
  → 当前 Inquiry

相同问题 + 替代推理、解决方案或验证路径
  → Inquiry 下记录的 Path

新问题 + 独立目标或完成标准
  → 新 Inquiry 文档

明确的可执行行动
  → Task 文档

可重用的陈述、来源、综合、假设或采纳的选择
  → Artifact 文档
```

一个新的独立 Inquiry 不是 Path。Path 是回答同一 Inquiry 的另一种方式。

## 命令

规范命令：

```text
project init
session init
session init import <file-or-directory>
status
map
new inquiry <title or question>
path <title>
capture <claim|evidence|conclusion|assumption|decision|task>
close
review
```

兼容性别名：

```text
init                    = project init
init import <path>      = session init import <path>
```

### `project init`

在当前项目目录中创建或规范化 `discussion/PROJECT.md` 以及 `discussion/inquiries/`、`discussion/work-items/` 和 `discussion/artifacts/` 文件夹。仅当用户明确要求 `session init` 时才摄入当前会话。

如果项目已在其他地方有讨论内容，将其移动到 `discussion/` 中。

### `session init`

解析当前项目目录，然后使用其 `discussion/` 文件夹作为工作空间。确保 `discussion/PROJECT.md` 和同级文件夹 `discussion/inquiries/`、`discussion/work-items/` 和 `discussion/artifacts/` 存在，然后将当前 AI 会话转换为持久的 Inquiry、WorkItem 和 Artifact 文档。阅读 `references/model.zh.md`、`references/workflow.zh.md` 和 `references/presentation.zh.md`。

单个当前会话可能产生多个独立的 Inquiry 和 WorkItem 文档。

### `session init import <file-or-directory>`

将早期对话文件或目录转换为当前项目的文档。在生成的纯 Markdown 文档中保留源路径或章节锚点（如果可用）。阅读 `references/import.zh.md`。

### 其他命令

- `status`：读取并呈现 `PROJECT.md` 和活动文档。
- `map`：呈现 `PROJECT.md` 和链接文档中记录的关系。
- `new inquiry`：在 `discussion/inquiries/` 下创建独立 Inquiry。
- `path`：在相关 Inquiry 中记录同一问题的推理路径。
- `capture`：创建或更新 Artifact 或 Task 及其源关系。
- `close`：将活动讨论整合到文档中并更新 `PROJECT.md`。
- `review`：查找缺失链接、未解决的询问、无支持的声明、过时的决策和孤立的工作项。

## 持久化规则

- `discussion/` 是写入边界。
- `discussion/PROJECT.md` 是项目级文件。它包含目标、当前焦点、活动询问、最近的结论和决策、开放验证项、下一步工作以及所有活动文档的链接。
- `discussion/` 下的 Inquiry、WorkItem 和 Artifact 文档是持久记录。
- 生成的项目文档使用普通 Markdown：标题、相对链接、列表、表格。
- 在可见的 Markdown 章节中保留不确定性和来源。
- AI 建议在用户采纳或基于它实施时成为已接受的 Decision。
- 为实质性问题、声明、决策和任务创建文档。

## 完成标准

操作完成的标志是：

```text
恰好一个 PROJECT.md 存在于 discussion/PROJECT.md；
PROJECT.md 包含当前概览和链接；
记录被初始化时 discussion/ 子文件夹存在；
当前会话材料已成为正确类型的文档；
同一问题的替代方案是 Paths；
独立问题是 Inquiries；
可执行行动是 Tasks；
持久文档有可见的源和关系章节；
生成的项目文档仅使用普通 Markdown；
PROJECT.md 反映了新的或更改的文档。
```
