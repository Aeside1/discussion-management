# Discussion Management Model

## Storage boundary

The skill workspace is the `discussion/` folder inside the project directory where the user invokes the skill. Everything this skill owns lives there, so discussion records never mix with the surrounding project content.

The workspace has exactly one project-level document:

```text
PROJECT.md
```

`PROJECT.md` is the project overview, index, and current-state record. Detailed content lives in the sibling folders and links from here with ordinary relative Markdown links.

The workspace layout:

```text
<project directory>/
├── discussion/
│   ├── PROJECT.md
│   ├── inquiries/
│   ├── work-items/
│   └── artifacts/
└── <project content>
```

The current AI session is an input boundary. `session init` reads the current session and writes its durable results into the project documents.

## Entities

### Project

There is one Project per `discussion/` workspace. Its durable representation is `PROJECT.md`.

The Project records:

- project title and goal;
- current focus;
- active Inquiries;
- recent Conclusions and Decisions;
- assumptions needing validation;
- next WorkItems;
- links to durable documents;
- project change notes when useful.

The project is considered **discussion-managed** after `project init` has created or normalized `discussion/PROJECT.md` and its sibling record folders, and `session init` has established the document structure.

### Inquiry

An Inquiry is an independent question, research problem, or design problem with its own goal and completion criteria.

An Inquiry gets its own document when it can be understood without the surrounding transcript and has an independent next action or completion condition.

Suggested filename:

```text
<short-stable-id>-<slug>.md
```

Use ordinary Markdown sections:

```markdown
# Inquiry: How should AI discussions be managed?

## Main question

## Goal

## Completion criteria

## Current status

## Discussion summary

## Paths

## Artifacts

## Open questions

## Derived work

## Source and provenance

## Change notes
```

### Path

A Path is a same-question reasoning, solution, or validation route. It belongs inside the relevant Inquiry document.

Use a Path when the main question, goal, and completion criteria remain unchanged. Record competing Paths in a section such as:

```markdown
## Paths

### Path A: derive state from an evidence log

Status: active
Purpose: ...

### Path B: maintain state directly

Status: rejected
Reason: ...
```

### Artifact

An Artifact is a durable output of discussion. Use one of these types:

```text
Claim       a proposition that can be supported, challenged, or revised
Evidence    material that supports or contradicts a proposition
Conclusion  a current synthesis reached while addressing an Inquiry
Assumption  a temporary premise that still needs validation
Decision    an explicitly adopted project choice
```

Create an Artifact file when the item is referenced by multiple documents, affects a project decision, needs independent review, or creates follow-up work. Otherwise it may be recorded in the relevant Inquiry and summarized in `PROJECT.md`.

Artifact template:

```markdown
# Artifact: Title

Type: Claim | Evidence | Conclusion | Assumption | Decision
Status: provisional
Confidence: medium

## Statement

## Reasoning or evidence

## Counterevidence or limitations

## Relationships

- Supports: none
- Contradicts: none
- Supersedes: none
- Derived work: none

## Source and provenance

- Source session: current session, not retained as an entity
- Source location: message, heading, or imported file section
- Related Inquiry: [Inquiry title](../inquiries/example.md)

## Change notes
```

A Claim or Conclusion becomes a Decision when the user adopts it or implementation proceeds on that basis.

### WorkItem

A WorkItem is a durable follow-up. It has one of two types:

```text
Inquiry     a new independent question
Task        an executable action
```

Use `discussion/work-items/` for both. Make the type visible in the heading and body.

WorkItem template:

```markdown
# Task: Implement discussion-management skill

Type: Task
Status: proposed

## Objective

## Completion criteria

## Derived from

- [Artifact title](../artifacts/example.md)

## Depends on

- none

## Related documents

## Notes
```

For an Inquiry WorkItem, use:

```markdown
# Inquiry: How should discussion branches be displayed?

Type: Inquiry
Status: proposed

## Main question

## Goal

## Completion criteria

## Derived from

## Related documents

## Notes
```

When a WorkItem becomes an active independent Inquiry, move or rename it into `discussion/inquiries/`.

## Relationships

Use explicit ordinary Markdown links and labels:

```text
Derived from
Motivated by
Depends on
Related to
Supports
Contradicts
Supersedes
```

The primary structure is:

```text
PROJECT.md
  ├── Inquiry documents
  │     ├── Paths
  │     └── Artifacts
  └── WorkItem documents
        ├── derived from Artifacts
        └── depends on other WorkItems
```

A new independent question derived from a conclusion is a sibling Inquiry or proposed Inquiry WorkItem. A Path is an alternative route within one Inquiry.

## IDs and filenames

Use stable, readable IDs in filenames. Avoid dates as the only identity and avoid titles that change whenever wording is refined.

Examples:

```text
inquiry-manage-ai-discussion.md
artifact-evidence-log-as-source.md
task-implement-discussion-skill.md
```

If a document must be renamed, update all links in `discussion/PROJECT.md` and related documents in the same operation.

## Plain Markdown

Generated project documents use ordinary Markdown: headings, labels, relative links, lists, tables.
