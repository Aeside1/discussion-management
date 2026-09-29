# discussion-management

A skill for turning long AI discussions into maintainable project documents.

Most AI conversations are not wasted — they contain real questions, real reasoning, and real conclusions. But the output stays trapped in a transcript. A week later you remember that you decided something, not why, and the question that produced it is gone.

This skill treats a discussion as a small knowledge project: it identifies the questions being asked, separates genuine new questions from alternative routes through the same question, extracts the durable claims and decisions, and keeps a single readable project file that links to everything.

---

## The problem

Long discussions branch. A single question gets clarified, turns into two, produces a conclusion, and the conclusion creates three follow-up problems.

Two failures follow from that:

- **Everything collapses into one thread.** Different questions get mixed into one pile, and nothing can be found later.
- **Everything gets saved as a summary.** A wall of prose where facts, opinions, evidence, and decisions are indistinguishable.

Both make the discussion unusable as a record.

## What this skill does

It maintains four kinds of durable object:

| Object | Meaning |
|---|---|
| **Inquiry** | An independent question with its own goal and completion criteria |
| **Path** | An alternative reasoning, solution, or validation route through the *same* Inquiry |
| **Artifact** | A Claim, Evidence, Conclusion, Assumption, or Decision produced by the discussion |
| **WorkItem** | A follow-up: either a new proposed Inquiry, or an executable Task |

The critical distinction is between a **Path** and a new **Inquiry**:

```text
same question + different route   → Path
new question + its own goal       → sibling Inquiry
clear executable action           → Task
```

A conclusion that produces new questions does not create child threads. It produces **sibling** Inquiries linked back to the Artifact that caused them.

## What it deliberately does not do

- **It does not save conversations.** The AI session is temporary input. There is no session entity, no session registry, no transcript archive.
- **It does not write into your project.** Everything lives in a single `discussion/` folder, so discussion records never mix with code, research, or notes.
- **It does not use Obsidian syntax.** Generated documents are plain Markdown: no YAML frontmatter, no `[[wikilinks]]`, no vault properties. Relative Markdown links only.
- **It does not promote suggestions to decisions.** An AI proposal stays `provisional` until you adopt it or build on it.

---

## Output layout

Everything this skill owns lives under one folder, parallel to your real project content:

```text
<project directory>/
├── discussion/                 ← the skill's only write boundary
│   ├── PROJECT.md              the one project file: overview, index, current state
│   ├── inquiries/              independent questions
│   ├── work-items/             follow-up inquiries and executable tasks
│   └── artifacts/              claims, evidence, conclusions, assumptions, decisions
└── <project content>           code, research, notes, and other project material
```

There is exactly one `PROJECT.md` per discussion workspace. It carries the goal, the current focus, active inquiries, recent conclusions and decisions, open validation items, next work, and links to every active document. Detailed content stays in the record files and is reached by relative link.

---

## Commands

Canonical:

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
init                 = project init
init import <path>   = session init import <path>
```

### `project init`

Creates or normalizes the workspace boundary: `discussion/PROJECT.md` plus the `inquiries/`, `work-items/`, and `artifacts/` folders.

It does **not** ingest the current session. Establishing the workspace and processing a conversation are separate operations, so several sessions can be attached to one project one at a time.

### `session init`

Resolves the project directory, ensures the workspace exists, then transforms the current AI session into durable Inquiry, WorkItem, and Artifact documents.

One session can produce many documents. A session that touches three questions produces three Inquiries — it is never flattened into one.

### `session init import <path>`

Does the same for earlier dialogue from a Markdown, JSON, or plain-text transcript, or a directory of them. Source paths and section anchors are preserved as visible provenance inside the generated documents.

---

## Installing

Clone the repository somewhere stable, then link it into the project where you want the skill available.

On Windows, a directory junction works without elevation:

```powershell
New-Item -ItemType Junction `
  -Path "<project>\.pi\skills\discussion-management" `
  -Target "G:\Github\discussion-management"
```

The skill is then discovered as `<project>\.pi\skills\discussion-management\SKILL.md`, while the actual files stay in this repository — so one clone can be linked into any number of projects and updates propagate immediately.

To link it globally instead, place the junction under your pi agent skills directory.

---

## Repository layout

```text
discussion-management/
├── SKILL.md                    entry point: model summary, routing, commands
└── references/
    ├── model.md                entities, storage boundary, IDs, file templates
    ├── workflow.md             classification, extraction, update order
    ├── import.md               processing current and earlier dialogue
    └── presentation.md         PROJECT.md layout, ordering, links, status output
```

`SKILL.md` stays short and routes to a reference only when the operation needs it. The reference files carry the detail: entity fields, templates, decision trees, and completion criteria.

---

## Design principles

- **Separate the boundary from the content.** One `discussion/` folder is the entire write scope. Nothing else in the project is touched.
- **Separate the session from the record.** Conversations are input; documents are output. Keeping transcripts out avoids a second, competing source of truth.
- **Separate the question from the route.** Paths belong inside an Inquiry. A real change of question creates a peer document instead.
- **Separate claims from decisions.** Conclusions and Decisions are different objects with different status, and evidence is never the same thing as the thing it supports.
- **Preserve uncertainty visibly.** `provisional`, confidence levels, and counterevidence are part of the document, not metadata to be stripped.
- **Keep the overview shallow.** `PROJECT.md` shows status, next action, and links. Detail lives one hop away.
