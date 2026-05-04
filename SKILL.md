---
name: software-design-docs
description: Create a consistent software design document set from a feature idea, conversation, existing codebase, existing documents, or requirements memo. Use when Claude Code or Codex needs to draft or revise requirements (要件定義書), basic design (基本設計書), detailed design (詳細設計書), DB design (DB設計), API design (API設計), batch design (バッチ設計), screen/UI design (画面設計), test viewpoints (試験観点), observability design (可観測性設計), impact scope (影響範囲), cautions (注意点), or implementation-ready pre-build documentation — especially before coding or when aligning multiple design documents.
---

# Software Design Docs

## Purpose

Create implementation-ready software design documents as a coherent, contradiction-resistant set. Prefer Markdown output; when the user asks for Google Docs, first produce stable Markdown content, then use the Google Drive/Docs workflow to publish or update Docs without changing the structure.

This skill is designed to work with both Claude Code and Codex. Frontmatter follows the Claude Code Skill convention; the workflow itself is agent-agnostic.

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
   5. Observability — produce as an independent document only when the feature has non-trivial operational, monitoring, alerting, or SLO requirements (most production APIs, batch jobs, and user-facing flows). For small internal utilities, the log/monitoring subsection inside detailed design is enough.
   6. Test viewpoints
   7. Impact scope
   8. Cautions and open issues
5. **Separate decisions from open items.** Every substantial document must include confirmed decisions, assumptions/defaults, and unresolved items.
6. **Cross-check consistency.** Before finalizing, verify that DB/API/batch/screen/observability/test/impact/caution documents do not contradict requirements, basic design, or detailed design.
   For split design docs, also verify that each INDEX, frontmatter block, related links, and bidirectional references agree.
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
- `references/observability.md` for 可観測性設計 (logs, metrics, traces, alerts, SLO).
- `references/split-design-docs.md` for scoped or split design docs, including one file per endpoint/table/batch/screen/detailed design item, dedicated common spec files, and INDEX navigation rules.
- `references/impact-and-cautions.md` for 影響範囲 and 注意点.
- `references/kabureka-patterns.md` only as an example of a well-structured project documentation set; never treat it as a universal rule.

## Output Rules

- Use the project's document location and naming rules when present.
- For a new project with no structure, propose a minimal `Documents/INDEX.md` and place docs under `Documents/`; for scoped split design docs, use the `Documents/design/` layout in `references/split-design-docs.md` unless the user or project rules say otherwise.
- When the user requests a single document type, endpoint, table, batch, screen, detailed design item, or common specification, produce only that scoped deliverable and mention related upstream/downstream items only as consistency context.
- Keep each document concise enough to be maintained, but complete enough for another engineer or agent to implement without guessing.
- Avoid duplicating the same facts across documents. Requirements define what and why; basic design defines system shape; detailed design defines implementation behavior; individual designs define contracts.
- Mark non-goals explicitly when they prevent likely scope creep.
- Mark estimates and unverified assumptions explicitly. Do not present guessed performance, cost, volume, dates, compatibility, or external service behavior as measured fact.
- Include a final consistency checklist with links or filenames when producing multiple documents.
- Do not duplicate test viewpoints between `screen-design.md` and `test-viewpoints.md`. Screen design holds only viewpoints scoped to that single screen (rendering, interaction, error/empty states); cross-cutting viewpoints (regression, non-functional, integration, migration) belong in `test-viewpoints.md`.
- Use a single shared format for the "決定事項・未決事項" section in every template. Recommended table:

  ```markdown
  | Item | Status | Owner | Notes |
  |---|---|---|---|
  | <decision or question> | decided / assumption / open | <name or role> | <rationale, deadline, or next action> |
  ```

  `Status` values are limited to `decided`, `assumption`, or `open`. Each template body only needs the section heading; refer back to this format.
