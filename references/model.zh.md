# 讨论管理模型

## 存储边界

skill 工作空间是用户调用 skill 的项目目录内的 `discussion/` 文件夹。这个 skill 拥有的所有内容都在那里。

工作空间有一个项目级文档：

```text
PROJECT.md
```

`PROJECT.md` 是项目概览、索引和当前状态记录。详细内容在同级文件夹中，通过普通相对 Markdown 链接从这里链接。

工作空间布局：

```text
<项目目录>/
├── discussion/
│   ├── PROJECT.md
│   ├── inquiries/
│   ├── work-items/
│   └── artifacts/
└── <项目内容>
```

当前 AI 会话是输入边界。`session init` 读取当前会话并将其持久化结果写入项目文档。

## 实体

### Project

每个 `discussion/` 工作空间有一个 Project。其持久表示是 `PROJECT.md`。

Project 记录：

- 项目标题和目标；
- 当前焦点；
- 活动 Inquiries；
- 最近的 Conclusions 和 Decisions；
- 需要验证的假设；
- 下一步 WorkItems；
- 持久文档链接；
- 有用时的项目变更注释。

项目在 `project init` 创建或规范化 `discussion/PROJECT.md` 及其同级记录文件夹，且 `session init` 建立文档结构后，被视为**讨论管理的**。

### Inquiry

Inquiry 是一个独立问题、研究问题或设计问题，有自己的目标和完成标准。

当 Inquiry 可以在没有周围对话记录的情况下被理解，并且有独立的下一步行动或完成条件时，它获得自己的文档。

建议的文件名：

```text
<short-stable-id>-<slug>.md
```

使用普通 Markdown 章节：

```markdown
# Inquiry: AI 讨论应该如何管理？

## 主要问题

## 目标

## 完成标准

## 当前状态

## 讨论摘要

## Paths

## Artifacts

## 开放问题

## 派生工作

## 来源和出处

## 变更注释
```

### Path

Path 是同一问题的推理、解决方案或验证路径。它属于相关 Inquiry 文档内部。

当主要问题、目标和完成标准保持不变时使用 Path。在如下章节中记录竞争的 Paths：

```markdown
## Paths

### Path A: 从证据日志派生状态

状态：活动
目的：...

### Path B: 直接维护状态

状态：拒绝
原因：...
```

### Artifact

Artifact 是讨论的持久输出。使用以下类型之一：

```text
Claim       可以被支持、挑战或修订的命题
Evidence    支持或反驳命题的材料
Conclusion  在处理 Inquiry 时达成的当前综合
Assumption  仍需验证的临时前提
Decision    明确采纳的项目选择
```

当项目被多个文档引用、影响项目决策、需要独立审查或创建后续工作时，创建 Artifact 文件。否则可以在相关 Inquiry 中记录并在 `PROJECT.md` 中总结。

Artifact 模板：

```markdown
# Artifact: 标题

类型: Claim | Evidence | Conclusion | Assumption | Decision
状态: provisional
置信度: medium

## 陈述

## 推理或证据

## 反证或局限性

## 关系

- 支持: none
- 反驳: none
- 取代: none
- 派生工作: none

## 来源和出处

- 源会话: 当前会话
- 源位置: 消息、标题或导入的文件章节
- 相关 Inquiry: [Inquiry 标题](../inquiries/example.md)

## 变更注释
```

Claim 或 Conclusion 在用户采纳或基于它实施时成为 Decision。

### WorkItem

WorkItem 是持久的后续工作。它有两种类型之一：

```text
Inquiry     新的独立问题
Task        可执行行动
```

两者都使用 `discussion/work-items/`。在标题和正文中显示类型。

WorkItem 模板：

```markdown
# Task: 实施讨论管理 skill

类型: Task
状态: proposed

## 目标

## 完成标准

## 派生自

- [Artifact 标题](../artifacts/example.md)

## 依赖于

- none

## 相关文档

## 注释
```

对于 Inquiry WorkItem，使用：

```markdown
# Inquiry: 讨论分支应该如何显示？

类型: Inquiry
状态: proposed

## 主要问题

## 目标

## 完成标准

## 派生自

## 相关文档

## 注释
```

当 WorkItem 成为活动的独立 Inquiry 时，将其移动或重命名到 `discussion/inquiries/`。

## 关系

使用显式的普通 Markdown 链接和标签：

```text
派生自
动机来自
依赖于
相关于
支持
反驳
取代
```

主要结构是：

```text
PROJECT.md
  ├── Inquiry 文档
  │     ├── Paths
  │     └── Artifacts
  └── WorkItem 文档
        ├── 派生自 Artifacts
        └── 依赖于其他 WorkItems
```

从结论派生的新独立问题是同级 Inquiry 或提议的 Inquiry WorkItem。Path 是一个 Inquiry 内的替代路径。

## ID 和文件名

在文件名中使用稳定、可读的 ID。

示例：

```text
inquiry-manage-ai-discussion.md
artifact-evidence-log-as-source.md
task-implement-discussion-skill.md
```

如果必须重命名文档，在同一操作中更新 `discussion/PROJECT.md` 和相关文档中的所有链接。

## 纯 Markdown

生成的项目文档使用普通 Markdown：标题、标签、相对链接、列表、表格。
