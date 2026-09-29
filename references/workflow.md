# Discussion Workflow

## Operating loop

`session init` is the main conversion operation:

```text
current AI session
  → identify topics and questions
  → classify statements and sources
  → extract durable outputs
  → create or update documents
  → update PROJECT.md
```

Before writing:

1. resolve the current project directory;
2. use its `discussion/` folder as the workspace;
3. ensure `discussion/PROJECT.md` exists, creating it through `project init` if necessary;
4. ensure `discussion/inquiries/`, `discussion/work-items/`, and `discussion/artifacts/` exist;
5. read the current `discussion/PROJECT.md` and relevant existing documents;
6. process the current session as temporary source material.

Write only inside `discussion/`. Never place discussion records in the surrounding project content.

Never create a Session entity or retain a transcript merely because the session was processed.

## Classify each substantial segment

```text
continuation      continues an existing Inquiry
clarification     makes an Inquiry more precise
research          adds a source or investigation result
claim             proposes a reusable proposition
evidence          provides material supporting or contradicting a proposition
conclusion        synthesizes the current state
assumption        states a premise needing validation
decision          explicitly adopts a project choice
path              proposes another route to the same Inquiry
inquiry           starts an independent question
task              states an executable action
```

Do not record ordinary conversational filler or every assistant response.

## Path or new Inquiry

Use this decision tree:

```text
Does the content still answer the same main question?
│
├── yes
│   ├── explanation, refinement, or evidence → existing Inquiry
│   ├── alternative solution or validation route → Path in that Inquiry
│   └── synthesis or adopted answer → Artifact
│
└── no
    ├── independent goal and completion criteria → Inquiry document
    ├── executable action → Task document
    └── possible future direction → proposed WorkItem
```

A Path remains inside an Inquiry document. A new Inquiry is a peer document under `discussion/inquiries/` or a proposed Inquiry WorkItem under `discussion/work-items/` when it has not yet been accepted as active work.

Do not call a new independent question a fork. Link it to the Claim or Conclusion that caused it with a visible `Derived from` link.

## Extract durable outputs

Use the smallest suitable type:

```text
Claim       reusable proposition
Evidence    source, experiment, observed behavior, or transcript-supported material
Conclusion  current synthesis
Assumption  temporary premise
Decision    adopted choice
```

Create an independent Artifact file when it is reused across documents, affects a Decision, needs review, has independent evidence, or produces follow-up work. Otherwise record it in the Inquiry and summarize it in `PROJECT.md`.

An AI suggestion remains provisional until the user adopts it or implementation proceeds on it.

## Update order

After classifying the current session:

1. update existing Inquiry documents;
2. create new Inquiry documents for independent active questions;
3. create proposed Inquiry or Task WorkItems for follow-up work not yet started;
4. create or update Artifact documents;
5. add relative links among all affected documents;
6. update `discussion/PROJECT.md` current focus, active inquiries, recent outputs, validation needs, and next work;
7. check for broken relative links;
8. report the created, updated, and ambiguous items.

## Project initialization

`project init` creates or normalizes only the workspace boundary:

```text
discussion/PROJECT.md
discussion/inquiries/
discussion/work-items/
discussion/artifacts/
```

It may create the record folders if the project convention requires it, but it does not process the current session. `session init` performs the conversion.

If `discussion/PROJECT.md` already exists, preserve its useful content and normalize its sections rather than overwriting it. If discussion records exist elsewhere in the project, consolidate them into `discussion/` and leave no competing location.

## Session initialization

`session init` processes the current session once. It may be run again later in another session or after additional discussion.

On each run:

1. read `discussion/PROJECT.md` and existing documents;
2. detect whether a current topic matches an existing Inquiry;
3. append new reasoning to the matching Inquiry instead of creating duplicates;
4. create a new Inquiry only when question, goal, or completion criteria are independent;
5. update existing Artifacts when the session revises them;
6. create new Artifacts only when they are substantively distinct;
7. update `discussion/PROJECT.md`.

The command is idempotent at the document-content level: do not create duplicate documents merely because the user runs `session init` again. Exact transcript deduplication is not required because the transcript itself is not persisted; use content, title, and existing links to avoid obvious duplicates.

## Notifications

Keep output short:

```text
已更新 Inquiry：……
已记录 Artifact：……
检测到新的并列 Inquiry：……
检测到 Task：……
以下边界仍需确认：……
```

Ask only when the distinction between a Path, new Inquiry, and Task would materially change the project structure.

## Close and review

When asked to close or consolidate:

1. separate facts, Claims, Evidence, Conclusions, Assumptions, Decisions, and WorkItems;
2. mark unresolved items visibly;
3. update document status;
4. update `discussion/PROJECT.md`;
5. do not create a Session record.

Review for:

```text
broken relative links;
unsupported Claims;
unvalidated Assumptions;
stale or superseded Decisions;
WorkItems without sources;
duplicate Inquiry documents;
Paths that are actually independent questions;
independent questions incorrectly embedded as Paths;
discussion/PROJECT.md sections missing from the current state;
files written outside discussion/.
```
