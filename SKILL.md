---
name: software-design-docs
description: Create a consistent software design document set from a feature idea, conversation, existing codebase, existing documents, or requirements memo. Use when Codex needs to draft or revise requirements, basic design, detailed design, DB design, API design, batch design, screen/UI design, test viewpoints, impact scope, cautions, or implementation-ready pre-build documentation, especially before coding or when aligning multiple design documents.
---

# Software Design Docs

## Purpose

Create implementation-ready software design documents as a coherent, contradiction-resistant set. Prefer Markdown output; when the user asks for Google Docs, first produce stable Markdown content, then use the Google Drive/Docs workflow to publish or update Docs without changing the structure.

Do not use this as a generic PRD skill. This skill starts where product intent becomes implementation planning: requirements, architecture, data contracts, screens, batches, tests, impact, and operational cautions.

The main job is document consistency, not template filling. Requirements define what and why; basic design defines system shape; detailed design defines implementation behavior; DB/API/batch/screen/test/impact/caution documents must align with those upstream decisions.

## Workflow

1. **Ground in source material.** Read the user's request, attached files, existing docs, repository structure, schemas, routes, jobs, tests, and project rules. In existing projects, read `AGENTS.md` and any document index such as `Documents/INDEX.md` before deciding output paths or reference scope.
2. **Clarify only high-impact unknowns.** Ask about audience, scope, business rules, constraints, and unresolved tradeoffs only when they cannot be discovered from the repo or documents. Record unanswered items as unresolved, not as invented facts.
3. **Protect source truth.** Do not overwrite existing docs silently. Verify implemented behavior from code. Write measured values only when actually measured; otherwise mark values as assumptions, estimates, or unverified.
4. **Create documents in this order.**
   1. Requirements
   2. Basic design
   3. Detailed design
   4. DB/API/batch/screen design
   5. Test viewpoints
   6. Impact scope
   7. Cautions and open issues
5. **Separate decisions from open items.** Every substantial document must include confirmed decisions, assumptions/defaults, and unresolved items.
6. **Cross-check consistency.** Before finalizing, verify that DB/API/batch/screen designs do not contradict requirements, basic design, or detailed design.
7. **Maintain navigation.** Update the document index after creating, moving, or archiving docs. Archive old documents instead of deleting them when history may matter.
8. **Respect doc-first rules.** If the project requires specification updates before implementation, update or draft the relevant documents before proposing code changes.

## Reference Loading

Load only the reference files needed for the requested deliverables:

- `references/requirements.md` for 要件定義書.
- `references/document_indexing.md` for document placement, index, archive, reference-scope, and AI-readable metadata rules.
- `references/basic-design.md` for 基本設計書.
- `references/detailed-design.md` for 詳細設計書.
- `references/db-design.md` for DB設計.
- `references/api-design.md` for API設計.
- `references/batch-design.md` for バッチ設計.
- `references/screen-design.md` for 画面設計.
- `references/test-viewpoints.md` for 試験観点.
- `references/impact-and-cautions.md` for 影響範囲 and 注意点.
- `references/kabureka-patterns.md` only as an example of a well-structured project documentation set; never treat it as a universal rule.

## Output Rules

- Use the project's document location and naming rules when present.
- For a new project with no structure, propose a minimal `Documents/INDEX.md` and place docs under `Documents/`.
- Keep each document concise enough to be maintained, but complete enough for another engineer or agent to implement without guessing.
- Avoid duplicating the same facts across documents. Requirements define what and why; basic design defines system shape; detailed design defines implementation behavior; individual designs define contracts.
- Mark non-goals explicitly when they prevent likely scope creep.
- Mark estimates and unverified assumptions explicitly. Do not present guessed performance, cost, volume, dates, compatibility, or external service behavior as measured fact.
- Include a final consistency checklist with links or filenames when producing multiple documents.
