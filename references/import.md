# Processing Earlier Dialogue

## Purpose

`session init import <file-or-directory>` processes earlier dialogue into the current project's `discussion/` workspace. The source is temporary input; the durable result is ordinary Markdown under `discussion/inquiries/`, `discussion/work-items/`, and `discussion/artifacts/`, plus updates to `discussion/PROJECT.md`.

Compatibility alias:

```text
init import <path> = session init import <path>
```

## Preconditions

Resolve the project directory first, then use its `discussion/` folder as the workspace.

If `discussion/PROJECT.md` does not exist, run or recommend `project init` and ensure the user has selected the project directory.

Ensure these directories exist:

```text
discussion/inquiries/
discussion/work-items/
discussion/artifacts/
```

## Supported sources

```text
Markdown with headings or speaker labels
JSON containing messages, turns, or transcript content
plain text with recognizable turn or section boundaries
a directory explicitly provided by the user
```

Directory traversal is recursive when the user explicitly supplies a directory.

## Import steps

1. Read `discussion/PROJECT.md` and existing documents before creating anything.
2. Enumerate the specified source files without modifying them.
3. Parse message, turn, or section boundaries.
4. Segment content when the main question, independent goal, completion criteria, expected output, or evidence requirements change.
5. Match each segment against existing Inquiries.
6. Keep same-question alternatives as Paths inside an existing Inquiry.
7. Create peer Inquiry documents for independent questions.
8. Create Task documents for explicit executable actions.
9. Extract durable Claims, Evidence, Conclusions, Assumptions, and Decisions.
10. Preserve source file paths and section, message, or line anchors in visible `Source and provenance` sections.
11. Update `discussion/PROJECT.md`.
12. Report ambiguous segments for review.

One source file can produce multiple Inquiry documents. Do not flatten all topics into one document.

## Provenance

Provenance is written as ordinary text:

```markdown
## Source and provenance

- Source: `notes/old-discussion.md`
- Location: `## Evidence model`, approximately lines 80-120
- Processing: `session init import`
```

For the current session:

```markdown
## Source and provenance

- Source: current AI session
- Location: discussion segment beginning with "..."
- Processing: `session init`
```

## Project inference

The project is the directory containing or selected for the `discussion/` workspace.

If the imported material suggests a different goal from the existing Project, record the mismatch as an open question.

## Artifact rules during import

Treat statements according to their source:

```text
user claim or proposal       → Claim or Assumption until supported
AI suggestion                → provisional Claim or Conclusion
cited primary source         → Evidence with citation/location
explicit user adoption      → Decision
later contradiction          → update status and add counterevidence
```

A concise AI summary is a Conclusion, not Evidence.

## Idempotence

Avoid duplicates by:

- reading current documents first;
- matching questions by meaning;
- updating an existing Artifact when the source adds support or counterevidence;
- preserving a new source location when it contributes distinct evidence;
- asking before replacing a conflicting Decision.

## Import output

After import, report:

```text
Updated discussion/PROJECT.md
Created or updated Inquiry documents: ...
Created or updated WorkItem documents: ...
Created or updated Artifact documents: ...
Ambiguous segments requiring review: ...
Broken or unresolved source locations: ...
```

## Completion criteria

Import is complete when:

```text
all accessible source material was considered;
independent topics have separate Inquiry documents;
same-question alternatives are Paths;
explicit actions are Tasks;
durable outputs have visible source locations;
discussion/PROJECT.md reflects the imported documents;
ambiguous classification is visible.
```
