---
name: discussion-management
description: >
  Organize substantial AI discussions into a project's discussion/ workspace,
  keeping PROJECT.md plus inquiries/, artifacts/, and work-items/ as plain
  Markdown records. Use when a session contains one or more questions that need
  to be clarified, researched, concluded, or turned into follow-up work; when
  initializing discussion-management for a project; when processing another
  session into the project's documents; when extracting inquiries, claims,
  evidence, decisions, or tasks; or when updating the project overview.
  Distinguish same-question reasoning paths from independent inquiries and tasks.
---

# Discussion Management

Turn the current AI session into maintainable documents inside the project's `discussion/` workspace.

```text
<project directory>/
├── discussion/                the discussion-management workspace and its only write boundary
│   ├── PROJECT.md             the one project file: overview, index, and current state
│   ├── inquiries/             independent questions
│   ├── work-items/            follow-up inquiries and executable tasks
│   └── artifacts/             claims, evidence, conclusions, assumptions, decisions
└── <project content>          code, research, notes, and other project material
```

All files this skill creates or updates live under `discussion/`.

The current session is temporary input. The output of `session init` is the durable record files: Inquiry, WorkItem, and Artifact documents.

## Read the relevant reference

- Entity model, project root, file naming, and plain Markdown templates: [references/model.md](references/model.md)
- Automatic classification, extraction, and update workflow: [references/workflow.md](references/workflow.md)
- Processing current or external earlier dialogue with `session init`: [references/import.md](references/import.md)
- `PROJECT.md` layout, ordering, links, and fast-reading display: [references/presentation.md](references/presentation.md)

Read all references needed by the requested operation before changing project files.

## Core classification

For each substantial part of the current session, classify it as one or more of:

```text
continuation | clarification | research | claim | evidence
conclusion | assumption | decision | path | inquiry | task
```

Route it using these rules:

```text
same question + same goal + same completion criteria
  → current Inquiry

same question + alternative reasoning, solution, or validation route
  → Path recorded under the Inquiry

new question + independent goal or completion criteria
  → new Inquiry document

clear executable action
  → Task document

reusable statement, source, synthesis, assumption, or adopted choice
  → Artifact document
```

A Path is another way to answer the same Inquiry. A new independent Inquiry has its own question, goal, and completion criteria.

## Commands

Canonical commands:

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

Compatibility aliases:

```text
init                    = project init
init import <path>      = session init import <path>
```

### `project init`

Create or normalize `discussion/PROJECT.md` and the `discussion/inquiries/`, `discussion/work-items/`, and `discussion/artifacts/` folders in the current project directory. Ingest the current session only when the user asks for `session init`.

If the project already has discussion content elsewhere, move it into `discussion/` rather than leaving two competing locations.

### `session init`

Resolve the current project directory, then use its `discussion/` folder as the workspace. Ensure `discussion/PROJECT.md` and the sibling folders `discussion/inquiries/`, `discussion/work-items/`, and `discussion/artifacts/` exist, then transform the current AI session into durable Inquiry, WorkItem, and Artifact documents. Read `references/model.md`, `references/workflow.md`, and `references/presentation.md`.

A single current session may produce multiple independent Inquiry and WorkItem documents.

### `session init import <file-or-directory>`

Transform an earlier dialogue file or directory into the current project's documents. Preserve source paths or section anchors inside the resulting plain Markdown documents where available. Read `references/import.md`.

### Other commands

- `status`: read and present `PROJECT.md` and active documents.
- `map`: present the relationships recorded in `PROJECT.md` and linked documents.
- `new inquiry`: create an independent Inquiry under `discussion/inquiries/`.
- `path`: record a same-question reasoning path in the relevant Inquiry.
- `capture`: create or update an Artifact or Task with its source relationship.
- `close`: consolidate the active discussion into documents and update `PROJECT.md`.
- `review`: find missing links, unresolved inquiries, unsupported claims, stale decisions, and orphaned work-items.

## Persistence rules

- `discussion/` is the write boundary.
- `discussion/PROJECT.md` is the project-level file. It contains the goal, current focus, active inquiries, recent conclusions and decisions, open validation items, next work, and links to all active documents.
- Inquiry, WorkItem, and Artifact documents under `discussion/` are the durable records.
- Generated project documents use ordinary Markdown: headings, relative links, lists, tables.
- Preserve uncertainty and provenance in visible Markdown sections.
- An AI suggestion becomes an accepted Decision when the user adopts it or implementation proceeds on that basis.
- Create documents for substantial questions, claims, decisions, and tasks.

## Completion criteria

An operation is complete when:

```text
exactly one PROJECT.md exists at discussion/PROJECT.md;
PROJECT.md contains the current overview and links;
discussion/ subfolders exist when records have been initialized;
nothing was created or modified outside discussion/;
current-session material has become correctly typed documents;
same-question alternatives are Paths;
independent questions are Inquiries;
executable actions are Tasks;
durable documents have visible source and relationship sections;
generated project documents use ordinary Markdown only;
PROJECT.md reflects the new or changed documents.
```
