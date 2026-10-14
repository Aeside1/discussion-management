# 讨论工作流

## 操作循环

`session init` 是主要转换操作：

```text
当前 AI 会话
  → 识别主题和问题
  → 分类陈述和来源
  → 提取持久输出
  → 创建或更新文档
  → 更新 PROJECT.md
```

写入前：

1. 解析当前项目目录；
2. 使用其 `discussion/` 文件夹作为工作空间；
3. 确保 `discussion/PROJECT.md` 存在，必要时通过 `project init` 创建；
4. 确保 `discussion/inquiries/`、`discussion/work-items/` 和 `discussion/artifacts/` 存在；
5. 读取当前 `discussion/PROJECT.md` 和相关现有文档；
6. 将当前会话作为临时源材料处理。

只在 `discussion/` 内写入。

## 分类每个实质性片段

```text
continuation      继续现有 Inquiry
clarification     使 Inquiry 更精确
research          添加来源或调查结果
claim             提出可重用命题
evidence          提供支持或反驳命题的材料
conclusion        综合当前状态
assumption        陈述需要验证的前提
decision          明确采纳项目选择
path              提出回答同一 Inquiry 的另一路径
inquiry           开始独立问题
task              陈述可执行行动
```

记录实质性片段：延续、澄清、研究、声明、证据、结论、假设、决策、路径、询问和任务。

## Path 还是新 Inquiry

使用此决策树：

```text
内容是否仍在回答相同的主要问题？
│
├── 是
│   ├── 解释、细化或证据 → 现有 Inquiry
│   ├── 替代解决方案或验证路径 → 该 Inquiry 中的 Path
│   └── 综合或采纳的答案 → Artifact
│
└── 否
    ├── 独立目标和完成标准 → Inquiry 文档
    ├── 可执行行动 → Task 文档
    └── 可能的未来方向 → 提议的 WorkItem
```

Path 保留在 Inquiry 文档内。新 Inquiry 是 `discussion/inquiries/` 下的同级文档，或当尚未被接受为活动工作时是 `discussion/work-items/` 下的提议 Inquiry WorkItem。

用可见的 `派生自` 链接将它链接到引起它的 Claim 或 Conclusion。

## 提取持久输出

使用最小的合适类型：

```text
Claim       可重用命题
Evidence    来源、实验、观察行为或对话记录支持的材料
Conclusion  当前综合
Assumption  临时前提
Decision    采纳的选择
```

当项目被跨文档重用、影响 Decision、需要审查、有独立证据或产生后续工作时，创建独立 Artifact 文件。否则在 Inquiry 中记录并在 `PROJECT.md` 中总结。

AI 建议在用户采纳或基于它实施之前保持临时状态。

## 更新顺序

分类当前会话后：

1. 更新现有 Inquiry 文档；
2. 为独立活动问题创建新 Inquiry 文档；
3. 为尚未开始的后续工作创建提议的 Inquiry 或 Task WorkItems；
4. 创建或更新 Artifact 文档；
5. 在所有受影响的文档之间添加相对链接；
6. 更新 `discussion/PROJECT.md` 当前焦点、活动询问、最近输出、验证需求和下一步工作；
7. 检查断开的相对链接；
8. 报告已创建、已更新和不明确的项目。

## 项目初始化

`project init` 创建或规范化工作空间边界：

```text
discussion/PROJECT.md
discussion/inquiries/
discussion/work-items/
discussion/artifacts/
```

它在项目约定需要时创建记录文件夹，但仅当用户运行 `session init` 时才处理当前会话。

如果 `discussion/PROJECT.md` 已存在，保留其有用内容并规范化其章节。如果讨论记录存在于项目的其他地方，将它们合并到 `discussion/` 中。

## 会话初始化

`session init` 处理当前会话一次。它可以在稍后的另一个会话中或在额外讨论之后再次运行。

每次运行：

1. 读取 `discussion/PROJECT.md` 和现有文档；
2. 检测当前主题是否匹配现有 Inquiry；
3. 将新推理追加到匹配的 Inquiry 而不是创建重复项；
4. 仅当问题、目标或完成标准独立时才创建新 Inquiry；
5. 当会话修订它们时更新现有 Artifacts；
6. 仅当它们在实质上不同时才创建新 Artifacts；
7. 更新 `discussion/PROJECT.md`。

该命令在文档内容级别是幂等的：当用户再次运行 `session init` 时避免重复文档。使用内容、标题和现有链接来匹配文档。

## 通知

保持输出简短：

```text
已更新 Inquiry：……
已记录 Artifact：……
检测到新的并列 Inquiry：……
检测到 Task：……
以下边界仍需确认：……
```

仅当 Path、新 Inquiry 和 Task 之间的区别会实质性改变项目结构时才询问。

## 关闭和审查

当要求关闭或整合时：

1. 分离事实、Claims、Evidence、Conclusions、Assumptions、Decisions 和 WorkItems；
2. 可见地标记未解决项目；
3. 更新文档状态；
4. 更新 `discussion/PROJECT.md`。

审查以下内容：

```text
断开的相对链接；
无支持的 Claims；
未验证的 Assumptions；
过时或被取代的 Decisions；
没有来源的 WorkItems；
重复的 Inquiry 文档；
实际上是独立问题的 Paths；
错误嵌入为 Paths 的独立问题；
当前状态缺失的 discussion/PROJECT.md 章节；
discussion/ 外的文件。
```
