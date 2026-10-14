# 处理早期对话

## 目的

`session init import <file-or-directory>` 将早期对话处理到当前项目的 `discussion/` 工作空间。源是临时输入；持久结果是 `discussion/inquiries/`、`discussion/work-items/` 和 `discussion/artifacts/` 下的普通 Markdown，以及对 `discussion/PROJECT.md` 的更新。

兼容性别名：

```text
init import <path> = session init import <path>
```

## 前提条件

首先解析项目目录，然后使用其 `discussion/` 文件夹作为工作空间。

如果 `discussion/PROJECT.md` 不存在，运行或推荐 `project init` 并确保用户已选择项目目录。

确保这些目录存在：

```text
discussion/inquiries/
discussion/work-items/
discussion/artifacts/
```

## 支持的源

```text
带标题或说话人标签的 Markdown
包含消息、回合或对话记录内容的 JSON
具有可识别回合或章节边界的纯文本
用户明确提供的目录
```

当用户明确提供目录时，目录遍历是递归的。

## 导入步骤

1. 在创建任何内容之前读取 `discussion/PROJECT.md` 和现有文档。
2. 枚举指定的源文件。
3. 解析消息、回合或章节边界。
4. 当主要问题、独立目标、完成标准、预期输出或证据要求改变时分段内容。
5. 将每个片段与现有 Inquiries 匹配。
6. 将同一问题的替代方案作为 Paths 保留在现有 Inquiry 内。
7. 为独立问题创建同级 Inquiry 文档。
8. 为明确的可执行行动创建 Task 文档。
9. 提取持久的 Claims、Evidence、Conclusions、Assumptions 和 Decisions。
10. 在可见的 `来源和出处` 章节中保留源文件路径和章节、消息或行锚点。
11. 更新 `discussion/PROJECT.md`。
12. 报告不明确的片段以供审查。

一个源文件可以产生多个 Inquiry 文档。

## 出处

出处被写为普通文本：

```markdown
## 来源和出处

- 源: `notes/old-discussion.md`
- 位置: `## Evidence model`，大约第 80-120 行
- 处理: `session init import`
```

对于当前会话：

```markdown
## 来源和出处

- 源: 当前 AI 会话
- 位置: 从 "..." 开始的讨论片段
- 处理: `session init`
```

## 项目推断

项目是包含或为 `discussion/` 工作空间选择的目录。

如果导入的材料建议与现有 Project 不同的目标，将不匹配记录为开放问题。

## 导入期间的 Artifact 规则

根据来源处理陈述：

```text
用户声明或提议       → Claim 或 Assumption，直到得到支持
AI 建议              → 临时 Claim 或 Conclusion
引用的主要来源        → 带引用/位置的 Evidence
明确的用户采纳        → Decision
稍后的矛盾           → 更新状态并添加反证
```

简洁的 AI 摘要是 Conclusion，不是 Evidence。

## 幂等性

通过以下方式避免重复：

- 首先读取当前文档；
- 根据含义匹配问题；
- 当源添加支持或反证时更新现有 Artifact；
- 当它贡献不同证据时保留新源位置；
- 在替换冲突 Decision 之前询问。

## 导入输出

导入后，报告：

```text
已更新 discussion/PROJECT.md
已创建或更新 Inquiry 文档：...
已创建或更新 WorkItem 文档：...
已创建或更新 Artifact 文档：...
需要审查的不明确片段：...
断开或未解决的源位置：...
```

## 完成标准

导入完成的标志是：

```text
所有可访问的源材料都已考虑；
独立主题有单独的 Inquiry 文档；
同一问题的替代方案是 Paths；
明确行动是 Tasks；
持久输出有可见的源位置；
discussion/PROJECT.md 反映导入的文档；
不明确的分类是可见的。
```
