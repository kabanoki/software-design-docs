# Document Indexing and Reference Scope

Use when creating, moving, archiving, or organizing project documents. The goal is to help humans and AI agents find the right source quickly without loading obsolete or irrelevant material.

## Core Rules

1. **Read the existing index first.** Look for `Documents/INDEX.md`, `docs/INDEX.md`, `README.md`, `AGENTS.md`, `CLAUDE.md`, `CODEX.md`, or project-specific documentation maps before creating files.
2. **Respect project placement rules.** If the project defines folders for specs, operations, infrastructure, reviews, or archive, follow them. If no structure exists, propose a minimal `Documents/INDEX.md`.
3. **Update the index after changes.** Any new document set, renamed document, moved document, or archived document must be reachable from the index.
4. **Respect precedence.** When rules conflict, use this order: explicit user instruction, `AGENTS.md` / `CLAUDE.md` / `CODEX.md`, existing index or README, existing document structure and naming, then skill defaults.
5. **Do not delete old documents by default.** Move obsolete but potentially useful docs to `archive/`, `archive/old/`, or the project's established historical folder.
6. **Limit AI reference scope.** The index should say which documents to read for each task and which folders are normally historical only, such as `reviews/` and `archive/`.
7. **Record source status.** Distinguish canonical docs, drafts, examples, review outputs, generated packets, and obsolete documents.

## Recommended Minimal Index

```markdown
# Project Documents Index

## Start Here

| Purpose | Document |
|---|---|
| Requirements | [Requirements](specs/requirements.md) |
| Basic design | [Basic Design](specs/basic-design.md) |
| Detailed design | [Detailed Design](specs/detailed-design.md) |

## Reference Scope

| Task | Read | Usually Do Not Read |
|---|---|---|
| Requirements/design change | `specs/`, relevant `acceptance/` | `reviews/`, `archive/` |
| Implementation | Relevant specs and tests | Historical reviews |
| Operations | `operations/`, relevant `infrastructure/` | Full spec history |

## Folders

| Folder | Purpose |
|---|---|
| `specs/` | Canonical requirements and design docs |
| `acceptance/` | Acceptance checks and test viewpoints |
| `operations/` | Runbooks and operational procedures |
| `infrastructure/` | Server, network, deployment, security docs |
| `reviews/` | Review packets and review results |
| `archive/` | Obsolete or historical documents |
```

## AI-Readable Metadata

Use frontmatter only when the project already uses it or when establishing a new documentation system. Keep it small and practical.

```yaml
---
doc_type: requirements
status: canonical
audience: engineering
read_when:
  - changing requirements
  - checking implementation scope
do_not_use_for:
  - historical comparison
---
```

Useful fields:

- `doc_type`: `requirements`, `basic_design`, `detailed_design`, `db_design`, `api_design`, `batch_design`, `screen_design`, `common_spec`, `test_viewpoints`, `impact_analysis`, `cautions`, `runbook`, `review`, `archive`.
- `status`: `canonical`, `draft`, `example`, `review_output`, `obsolete`. This describes the document lifecycle, not a design item's implementation progress and not the `decided` / `assumption` / `open` status used in decision tables.
- `audience`: `engineering`, `operations`, `product`, `qa`, `ai_agent`.
- `read_when`: short conditions for loading the document.
- `do_not_use_for`: situations where the document is likely misleading.

## Source Truth Rules

- Existing canonical docs outrank generated summaries.
- Code outranks stale detailed design when documenting implemented behavior, but do not silently rewrite the design; call out the mismatch.
- Measured values require evidence: command output, logs, dashboard data, benchmark result, external quote, or dated source.
- Guesses must be labeled as `assumption`, `estimate`, or `unverified`.
- Review outputs are evidence of feedback, not canonical design unless a resolution document adopts them.

## Quality Bar

- A new contributor can find the canonical docs in under a minute.
- AI agents can avoid loading historical noise by following the index.
- Obsolete docs remain recoverable but are clearly not canonical.
- Every generated or moved document has a clear home and status.
