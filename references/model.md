# Discussion Management Model

## Storage boundary

The skill workspace is the `discussion/` folder inside the project directory where the user invokes the skill. Everything this skill owns lives there, so discussion records never mix with the surrounding project content.

The workspace has exactly one project-level document:

```text
PROJECT.md
```

`PROJECT.md` is the project overview, index, and current-state record. It is not a database dump. Keep detailed content in the sibling folders and link to it with ordinary relative Markdown links.

The durable layout is:

```text
<project directory>/
├── discussion/
│   ├── PROJECT.md
│   ├── inquiries/
│   ├── work-items/
│   └── artifacts/
└── <project content>
```

Do not create `projects/`, `sessions/`, `index.md`, or `overview.md` for this skill. Do not write discussion records anywhere outside `discussion/`.

The current AI session is an input boundary, not a durable entity. `session init` reads the current session and writes its durable results into the project docs. It does not create a Session document, session registry, transcript archive, or Session-to-Inquiry join file.

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
- links to durable docs;
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

A Path is a same-question reasoning, solution, or validation route. It belongs inside the relevant Inquiry document; it is not normally a separate file.

Use a Path only when the main question, goal, and completion criteria remain unchanged. Record competing Paths in a section such as:

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

Create an Artifact file when the item is referenced by multiple docs, affects a project decision, needs independent review, or creates follow-up work. Otherwise it may be recorded in the relevant Inquiry and summarized in `PROJECT.md`.

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

A Claim or Conclusion is not automatically a Decision. An AI suggestion is not an accepted Decision unless the user adopts it or implementation proceeds on that basis.

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

## Related docs

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

## Related docs

## Notes
```

When a WorkItem becomes an active independent Inquiry, move or rename it into `discussion/inquiries/` and leave a short redirect note in the old document if needed. Do not duplicate its substantive content.

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
  ├── Inquiry docs
  │     ├── Paths
  │     └── Artifacts
  └── WorkItem docs
        ├── derived from Artifacts
        └── depends on other WorkItems
```

A new independent question derived from a conclusion is a sibling Inquiry or proposed Inquiry WorkItem, not a child Path. A Path is only an alternative route within one Inquiry.

## IDs and filenames

Use stable, readable IDs in filenames. Avoid dates as the only identity and avoid titles that change whenever wording is refined.

Examples:

```text
inquiry-manage-ai-discussion.md
artifact-evidence-log-as-source.md
task-implement-discussion-skill.md
```

If a document must be renamed, update all links in `discussion/PROJECT.md` and related docs in the same operation.

## Plain Markdown requirement

Generated project docs must contain no YAML frontmatter, Obsidian properties, Wikilinks, or Obsidian-specific syntax. Use headings, labels, relative Markdown links, checklists, and tables only.
