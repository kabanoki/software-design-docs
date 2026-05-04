# Software Design Docs

`software-design-docs` is a Codex skill for creating implementation-ready software design documents from feature ideas, conversations, existing code, existing documents, or requirements memos.

This is not a generic PRD skill. Use it when product intent needs to become engineering-ready documentation: requirements, basic design, detailed design, DB/API/batch/screen design, test viewpoints, impact scope, cautions, and open issues.

## When to Use

Use this skill when you need to:

- Draft or revise a consistent set of software design documents.
- Turn an idea or discussion into implementation planning material.
- Align requirements, system design, detailed behavior, data contracts, screens, jobs, APIs, tests, and operational cautions.
- Prepare documents before coding in a doc-first project.
- Check that downstream design docs do not contradict upstream requirements or architecture decisions.

## How It Works

Start with [`SKILL.md`](SKILL.md). It defines the workflow, output rules, and reference-loading policy.

The usual document order is:

1. Requirements
2. Basic design
3. Detailed design
4. DB/API/batch/screen design
5. Test viewpoints
6. Impact scope
7. Cautions and open issues

For existing projects, read project rules first, especially `AGENTS.md` and any document index such as `Documents/INDEX.md` or `docs/INDEX.md`. Do not overwrite existing canonical documents silently.

## Reference Files

Load only the files needed for the requested deliverables.

| Need | Reference |
|---|---|
| Requirements document | [`references/requirements.md`](references/requirements.md) |
| Document placement, index, archive, and metadata rules | [`references/document_indexing.md`](references/document_indexing.md) |
| Basic design | [`references/basic-design.md`](references/basic-design.md) |
| Detailed design | [`references/detailed-design.md`](references/detailed-design.md) |
| DB design | [`references/db-design.md`](references/db-design.md) |
| API design | [`references/api-design.md`](references/api-design.md) |
| Batch design | [`references/batch-design.md`](references/batch-design.md) |
| Screen design | [`references/screen-design.md`](references/screen-design.md) |
| Test viewpoints | [`references/test-viewpoints.md`](references/test-viewpoints.md) |
| Impact scope and cautions | [`references/impact-and-cautions.md`](references/impact-and-cautions.md) |
| Example documentation patterns | [`references/kabureka-patterns.md`](references/kabureka-patterns.md) |

`references/kabureka-patterns.md` is an example pattern library, not a universal rulebook.

## Quality Rules

- Ground documents in source material: code, schemas, routes, jobs, tests, existing docs, and project rules.
- Mark assumptions, estimates, and unresolved items explicitly.
- Keep decisions separate from open questions.
- Avoid duplicating the same facts across documents.
- Use code as evidence for implemented behavior, but call out mismatches instead of silently rewriting design intent.
- Maintain navigation by creating or updating document indexes when adding or moving project documents.

## Directory Layout

```text
.
|-- SKILL.md
|-- README.md
|-- agents/
|   `-- openai.yaml
`-- references/
    |-- api-design.md
    |-- basic-design.md
    |-- batch-design.md
    |-- db-design.md
    |-- detailed-design.md
    |-- document_indexing.md
    |-- impact-and-cautions.md
    |-- kabureka-patterns.md
    |-- requirements.md
    |-- screen-design.md
    `-- test-viewpoints.md
```
