# discussion-management

把 AI 长讨论整理成可维护的项目文档的 skill。

大多数 AI 对话并没有浪费——里面有真实的问题、真实的推理、真实的结论。但这些产出被困在了聊天记录里。一周之后，你只记得自己做过某个决定，却不记得为什么，而产生它的那个问题已经找不到了。

这个 skill 把一次讨论当成一个小型知识项目来处理：识别正在被追问的问题，把真正的新问题和「同一问题的不同路线」区分开，抽取可长期保留的观点和决策，并维护一份可读的项目文件来索引全部内容。

---

## 要解决的问题

长讨论会分叉。一个问题被澄清后变成两个，其中一个产出结论，而这个结论又带出三个后续问题。

由此产生两种典型失败：

- **所有东西塌缩成一条线。** 不同问题混在一堆里，之后什么都找不回来。
- **所有东西被存成一段总结。** 一大段文字里，事实、观点、证据、决策完全分不清。

两者都会让讨论失去作为记录的价值。

## 这个 skill 做什么

它维护四类可长期保留的对象：

| 对象 | 含义 |
|---|---|
| **Inquiry** | 一个独立问题，有自己的目标和完成标准 |
| **Path** | 通往**同一个** Inquiry 的另一条推理、方案或验证路线 |
| **Artifact** | 讨论产出的 Claim、Evidence、Conclusion、Assumption 或 Decision |
| **WorkItem** | 后续工作：要么是一个待确认的新 Inquiry，要么是一个可执行的 Task |

最关键的一条区分，是 **Path** 和新的 **Inquiry**：

```text
同一个问题 + 不同路线   → Path
新问题 + 自己的目标     → 并列的 sibling Inquiry
明确的执行动作          → Task
```

一个结论带出新问题时，不会生成「子线程」，而是生成与原 Inquiry **并列**的 Inquiry，并链接回产生它的那个 Artifact。

## 刻意不做的事

- **不保存对话。** AI session 只是临时输入。没有 session 实体、没有 session 登记表、没有聊天记录归档。
- **不写入你的项目别处。** 所有内容都放在单一的 `discussion/` 目录里，讨论记录永远不会和代码、调研、笔记混在一起。
- **不使用 Obsidian 语法。** 生成的文档是普通 Markdown：没有 YAML frontmatter、没有 `[[wikilink]]`、没有库属性。只用相对 Markdown 链接。
- **不把建议升级成决策。** AI 的提议会一直保持 `provisional`，直到你采纳它，或者开始基于它做事。

---

## 产物布局

这个 skill 拥有的所有东西都在同一个目录下，与项目真实内容平行：

```text
<项目目录>/
├── discussion/                 ← 本 skill 唯一的写入边界
│   ├── PROJECT.md              唯一的项目文件：概览、索引、当前状态
│   ├── inquiries/              独立问题
│   ├── work-items/             后续问题与可执行任务
│   └── artifacts/              观点、证据、结论、假设、决策
└── <项目内容>                  代码、调研、笔记及其他项目材料
```

每个 discussion 工作区只有一个 `PROJECT.md`。它承载项目目标、当前重点、活跃 Inquiry、最近的结论与决策、待验证事项、下一步工作，以及指向所有活跃文档的链接。详细内容留在各自的记录文件里，通过相对链接抵达。

---

## 命令

正式命令：

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

兼容别名：

```text
init                 = project init
init import <path>   = session init import <path>
```

### `project init`

创建或规范化工作区边界：`discussion/PROJECT.md`，以及 `inquiries/`、`work-items/`、`artifacts/` 三个目录。

它**不会**顺手处理当前 session。建立工作区和处理一次对话是两件独立的事，因此可以把多个 session 逐个接入同一个项目。

### `session init`

定位项目目录，确保工作区存在，然后把当前 AI session 转换成可长期保留的 Inquiry、WorkItem 和 Artifact 文档。

一个 session 可以产出很多份文档。一个 session 里讨论了三件事，就产出三个 Inquiry——绝不会被压成一个。

### `session init import <path>`

对更早的对话做同样的事，来源可以是 Markdown、JSON、纯文本 transcript，或包含这些文件的目录。源文件路径和章节锚点会作为可见的来源信息保留在生成的文档中。

---

## 安装

把仓库 clone 到一个固定位置，然后在需要启用该 skill 的项目里建立链接。

在 Windows 上，目录 junction 不需要管理员权限：

```powershell
New-Item -ItemType Junction `
  -Path "<项目>\.pi\skills\discussion-management" `
  -Target "G:\Github\discussion-management"
```

之后 skill 会以 `<项目>\.pi\skills\discussion-management\SKILL.md` 的形式被发现，而文件本体留在本仓库中——所以一份 clone 可以挂到任意多个项目上，修改会立即同步到全部项目。

如果想全局启用，把 junction 放到你的 pi agent skills 目录下即可。

---

## 仓库结构

```text
discussion-management/
├── SKILL.md                    入口：模型摘要、路由规则、命令
└── references/
    ├── model.md                实体、存储边界、标识、文件模板
    ├── workflow.md             分类、抽取、更新顺序
    ├── import.md               处理当前对话与历史对话
    └── presentation.md         PROJECT.md 布局、排序、链接、状态输出
```

`SKILL.md` 保持简短，只在操作真正需要时才路由到某个 reference。细节都在 reference 里：实体字段、模板、判定树和完成标准。

---

## 设计原则

- **边界与内容分离。** 一个 `discussion/` 目录就是全部写入范围，项目其他部分一概不碰。
- **会话与记录分离。** 对话是输入，文档是输出。不保留聊天记录，就避免了第二个互相竞争的事实来源。
- **问题与路线分离。** Path 属于 Inquiry 内部；真正换了问题就生成同级文档，而不是继续往下挂子节点。
- **主张与决策分离。** Conclusion 和 Decision 是状态不同的两类对象；证据也永远不等同于它所支持的东西。
- **不确定性与来源要显式保留。** `provisional`、置信度、反证都是文档的一部分，不是可以抹掉的元数据。
- **概览保持浅层。** `PROJECT.md` 只呈现状态、下一步动作和链接，细节都在一跳之外。
