# 项目呈现

## 目的

`PROJECT.md` 位于项目 `discussion/` 工作空间的根，是项目级概览和索引。它是快速阅读入口和当前状态记录。详细内容属于同级文件夹。

生成的项目文档是普通 Markdown：标题、相对链接、列表、表格。

## PROJECT.md 布局

创建或规范化 `discussion/PROJECT.md` 使用此顺序：

```markdown
# 项目标题

## 目标

## 当前焦点

## 活动询问

## 最近的结论和决策

## 需要验证

## 下一步工作

## 文档索引

## 变更注释
```

`PROJECT.md` 应快速回答：

```text
这个项目试图实现什么？
现在在讨论什么？
哪些 Inquiries 是开放的？
得出了什么结论或做出了什么决策？
什么仍然不确定？
接下来应该发生什么？
详细文档在哪里？
```

## PROJECT.md 模板

```markdown
# 项目标题

管理: discussion-management

## 目标

用一两段描述项目目标。

## 当前焦点

- Inquiry: [标题](inquiries/inquiry-example.md)
- 为什么重要: ...
- 下一步决策或行动: ...

## 活动询问

### Inquiry: 标题

状态: active  
问题: ...  
下一步: ...  
文档: [Inquiry 文档](inquiries/inquiry-example.md)

## 最近的结论和决策

- Conclusion: ... — [Artifact](artifacts/artifact-example.md)
- Decision: ... — [Artifact](artifacts/decision-example.md)

## 需要验证

- Assumption: ... — [Artifact](artifacts/assumption-example.md)

## 下一步工作

- [ ] Task: ... — [WorkItem](work-items/task-example.md)
- 提议的 Inquiry: ... — [WorkItem](work-items/inquiry-example.md)

## 文档索引

### Inquiries

- [Inquiry 标题](inquiries/inquiry-example.md) — 状态 — 简短描述

### Work items

- [Task 标题](work-items/task-example.md) — 状态 — 派生自 [Artifact](artifacts/artifact-example.md)

### Artifacts

- [Artifact 标题](artifacts/artifact-example.md) — 类型 — 状态

## 变更注释

- YYYY-MM-DD: ...
```

只包含状态、标题、最相关的关系和链接。

## 排序规则

按可操作性排序概览：

1. 目标和当前焦点；
2. 活动 Inquiries；
3. 最近的 Conclusions 和已接受或提议的 Decisions；
4. 假设和验证需求；
5. 活动和提议的 WorkItems；
6. 文档索引；
7. 变更注释。

在每个组内，首先放置活动或阻塞项目。在索引中保留已解决和归档的文档及其状态，或在索引变得太大时省略它们。

## 关系显示

使用 Inquiry 作为主要阅读单元。在 Inquiry 文档内显示 Paths。通过标记的相对链接显示派生工作：

```markdown
### Inquiry: AI 讨论应该如何管理？

状态: active  
问题: 如何在不丢失上下文的情况下管理讨论分支和结果？  
文档: [Inquiry](inquiries/inquiry-manage-ai-discussion.md)

当前结论: [将讨论结构与讨论结果分离](artifacts/conclusion-separate-structure-and-outcomes.md)

派生工作:

- [Inquiry: 讨论分支应该如何显示？](work-items/inquiry-discussion-branches.md)
  - 派生自: [结论](artifacts/conclusion-separate-structure-and-outcomes.md)
- [Task: 实施讨论管理 skill](work-items/task-implement-discussion-skill.md)
  - 派生自: [结论](artifacts/conclusion-separate-structure-and-outcomes.md)
```

使用可见标签：

```text
源
派生自
动机来自
依赖于
相关于
支持
反驳
取代
```

文档索引和标记的链接保留图，而第一部分保持可读。

## `status` 输出

首先读取 `PROJECT.md` 并呈现：

```text
项目目标
当前焦点
活动 Inquiries
最近的 Conclusions 和 Decisions
验证需求
下一步 WorkItems
文档根
```

只显示标题、状态、下一步行动和最相关的链接。如果 `PROJECT.md` 缺失，路由到 `project init`。

## `map` 输出

从 Project 文档和活动文档显示紧凑的关系图：

```text
Project
├── 当前 Inquiry
│   ├── Path A / Path B
│   ├── 当前 Conclusion 或 Decision
│   └── 派生的 Inquiry / Task
├── 其他活动 Inquiries
└── 未解决的验证项
```

使用文档链接和关系标签进行交叉链接。显示结构，而不是每个细节。

## 更新 PROJECT.md

在以下情况后更新 `discussion/PROJECT.md`：

```text
project init；
session init 或 session init import；
Inquiry、WorkItem 或 Artifact 的创建或状态变化；
新的或修订的 Decision；
新的依赖或派生自关系；
关闭或审查操作。
```

更新前：

1. 保留有用的现有内容；
2. 删除重复或过时的索引条目；
3. 确保每个活动文档有一个相对链接；
4. 确保每个显示的关系在详细文档中有匹配的关系；
5. 保持当前焦点简短且可操作。

## 呈现完成标准

呈现更新完成的标志是：

```text
恰好一个 PROJECT.md 存在于 discussion/PROJECT.md；
当前焦点出现在开头附近；
活动 Inquiries 和下一步工作在不打开每个文档的情况下可见；
Paths 显示在其 Inquiry 下；
派生的 WorkItems 显示其源 Artifact；
所有显示的链接都是普通相对 Markdown 链接；
生成的项目文档使用普通 Markdown；
已解决的细节保持可访问。
```
