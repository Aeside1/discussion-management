# Project Presentation

## Purpose

`PROJECT.md`, at the root of the project's `discussion/` workspace, is the project-level overview and index. It is the fast-reading entry point and the current-state record. Detailed content belongs in the sibling folders.

Generated project documents are ordinary Markdown: headings, relative links, lists, tables.

## PROJECT.md layout

Create or normalize `discussion/PROJECT.md` with this order:

```markdown
# Project title

## Goal

## Current focus

## Active inquiries

## Recent conclusions and decisions

## Needs validation

## Next work

## Document index

## Change notes
```

`PROJECT.md` should answer quickly:

```text
What is this project trying to achieve?
What is being discussed now?
Which Inquiries are open?
What has been concluded or decided?
What remains uncertain?
What should happen next?
Where are the detailed documents?
```

## PROJECT.md template

```markdown
# Project title

Management: discussion-management

## Goal

Describe the project goal in one or two paragraphs.

## Current focus

- Inquiry: [title](inquiries/inquiry-example.md)
- Why it matters: ...
- Next decision or action: ...

## Active inquiries

### Inquiry: Title

Status: active  
Question: ...  
Next step: ...  
Document: [Inquiry document](inquiries/inquiry-example.md)

## Recent conclusions and decisions

- Conclusion: ... — [Artifact](artifacts/artifact-example.md)
- Decision: ... — [Artifact](artifacts/decision-example.md)

## Needs validation

- Assumption: ... — [Artifact](artifacts/assumption-example.md)

## Next work

- [ ] Task: ... — [WorkItem](work-items/task-example.md)
- Proposed Inquiry: ... — [WorkItem](work-items/inquiry-example.md)

## Document index

### Inquiries

- [Inquiry title](inquiries/inquiry-example.md) — status — short description

### Work items

- [Task title](work-items/task-example.md) — status — derived from [Artifact](artifacts/artifact-example.md)

### Artifacts

- [Artifact title](artifacts/artifact-example.md) — type — status

## Change notes

- YYYY-MM-DD: ...
```

Include only status, title, the most relevant relation, and a link.

## Ordering rules

Order the overview by actionability:

1. goal and current focus;
2. active Inquiries;
3. recent Conclusions and accepted or proposed Decisions;
4. assumptions and validation needs;
5. active and proposed WorkItems;
6. document index;
7. change notes.

Within each group, put active or blocking items first. Keep resolved and archived documents in the index with their status, or omit them when the index becomes too large.

## Relationship display

Use the Inquiry as the primary reading unit. Show Paths inside the Inquiry document, not as unrelated top-level items. Show derived work through labeled relative links:

```markdown
### Inquiry: How should AI discussions be managed?

Status: active  
Question: How can discussion branches and outcomes be managed without losing context?  
Document: [Inquiry](inquiries/inquiry-manage-ai-discussion.md)

Current conclusion: [Separate discussion structure from discussion outcomes](artifacts/conclusion-separate-structure-and-outcomes.md)

Derived work:

- [Inquiry: How should discussion branches be displayed?](work-items/inquiry-discussion-branches.md)
  - Derived from: [conclusion](artifacts/conclusion-separate-structure-and-outcomes.md)
- [Task: Implement the discussion-management skill](work-items/task-implement-discussion-skill.md)
  - Derived from: [conclusion](artifacts/conclusion-separate-structure-and-outcomes.md)
```

Use visible labels:

```text
Source
Derived from
Motivated by
Depends on
Related to
Supports
Contradicts
Supersedes
```

The document index and labeled links preserve the graph while the first sections remain readable.

## `status` output

Read `PROJECT.md` first and present:

```text
Project goal
Current focus
Active Inquiries
Recent Conclusions and Decisions
Validation needs
Next WorkItems
Document root
```

Show only title, status, next action, and the most relevant link. If `PROJECT.md` is missing, route to `project init`.

## `map` output

Show a compact relationship map from the Project document and active documents:

```text
Project
├── Current Inquiry
│   ├── Path A / Path B
│   ├── Current Conclusion or Decision
│   └── Derived Inquiry / Task
├── Other active Inquiries
└── Unresolved validation items
```

Use document links and relationship labels for cross-links. Show the structure, not every detail.

## Updating PROJECT.md

Update `discussion/PROJECT.md` after:

```text
project init;
session init or session init import;
creation or status change of an Inquiry, WorkItem, or Artifact;
new or revised Decision;
new dependency or derived-from relation;
close or review operation.
```

Before updating:

1. preserve useful existing content;
2. remove duplicate or stale index entries;
3. ensure every active document has one relative link;
4. ensure each displayed relation has a matching relation in the detailed document;
5. keep the current focus short and actionable.

## Presentation completion criteria

A presentation update is complete when:

```text
exactly one PROJECT.md exists at discussion/PROJECT.md;
current focus appears near the beginning;
active Inquiries and next work are visible without opening every document;
Paths are shown under their Inquiry;
derived WorkItems show their source Artifact;
all displayed links are ordinary relative Markdown links;
generated project documents use ordinary Markdown;
resolved detail remains accessible.
```
