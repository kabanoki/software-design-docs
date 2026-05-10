# Monolith Design Splitting

Use when updating or substantially extending an existing canonical design document that may have grown too large to maintain. Use this reference before approval to detect oversized documents and propose a split, and after approval to perform the migration. This is different from creating new split design docs: monolith splitting is a migration step whose first priority is preserving information and switching the canonical entry point safely.

Do not split existing monolithic documents automatically. First report that splitting is recommended, explain why, and wait for explicit approval.

## Split Triggers

Use these line counts as heuristics, not hard rules. Project rules and user instructions override them.

| Document | Consider Split | Strongly Recommend Split |
|---|---:|---:|
| Requirements | over 1,200 lines | over 2,000 lines |
| Basic design | over 700 lines | over 1,000 lines |
| Detailed design | over 1,200 lines | over 2,000 lines |
| DB design | over 800 lines | split by table or schema area |
| Batch design | over 800 lines | split by batch or command |

Before a substantial update to an existing design document, check the approximate line count. Treat updates that touch multiple major sections, add or rewrite roughly 100+ lines, or require repeated loading of unrelated sections as substantial. If the document is above the "Consider Split" line, propose a split before continuing the update.

## Safe Split Procedure

1. Read the root document index, such as `Documents/INDEX.md`, and any project-specific documentation rules.
2. Inspect the target monolith: line count, heading outline, section numbers, existing links, and frontmatter if present.
3. Decide the first split level before editing files. For a first migration, prefer section or domain-area splits over very fine endpoint/table/screen splits unless the user asked for fine-grained output.
4. Create or update the destination folder and `INDEX.md` first.
5. Copy or extract content by section or area without summarizing away details. Keep the old monolith body intact unless the user explicitly asks for a stub and accepts the loss of full-text history.
6. Add a notice near the top of the old monolith saying it is no longer canonical and points to the new canonical index.
7. If frontmatter is present, change the old monolith away from `status: canonical` (usually to `status: obsolete`) and set the new split index or split files to the project's canonical status.
8. Update the root index so the canonical link points to the split index or new canonical files.
9. Search for known links to the old monolith and update them to the new canonical destination, unless a link is intentionally historical and labeled as such.
10. Run link, section coverage, canonical-link, old-monolith-notice, frontmatter-status, inbound-link, and whitespace checks.
11. Record acceptance notes or review evidence when the split is large enough to need reviewer confidence.

## Old Monolith Notice

Keep the old monolith in place when existing links or review references may still point to it. Do not move it to `archive/` immediately unless the project rules or user request say so.

Use a notice like this near the top:

```markdown
> Legacy monolithic design document. Since YYYY-MM-DD, the canonical documents are under `Documents/design/.../INDEX.md`.
> This file is kept as the migration source, history, and full-text search aid. Do not use it as the current canonical source.
```

If the project language is Japanese, use:

```markdown
> 旧モノリス設計書。YYYY-MM-DD以降の正本は `Documents/design/.../INDEX.md`。
> このファイルは移行元、履歴、全文検索用として残す。現在の正本としては使用しない。
```

## Destination INDEX

The destination index should do more than list files. It should tell agents what to read and what not to read.

```markdown
# <Design Area> INDEX

## Canonical Documents

| Area | Document | Read When |
|---|---|---|
| Database | [database.md](database.md) | changing schema, migrations, or data constraints |
| Batches | [batches.md](batches.md) | changing scheduled jobs or rerun behavior |

## Reference Scope

| Task | Read | Usually Do Not Read |
|---|---|---|
| DB change | `database.md`, related `db/*.md` | old monolith full text |
| Batch change | `batches.md`, related `batch/*.md` | unrelated screen design |
```

## Frontmatter Additions

Use these fields when they help agents distinguish old sources from new canonical split files.

```yaml
---
source_monolith: ../../specs/old_design.md
canonical_since: YYYY-MM-DD
split_from_sections:
  - "6.12"
  - "6.16"
read_when:
  - changing notification behavior
do_not_use_for:
  - historical comparison
---
```

Keep frontmatter practical. Do not add fields that the project will not maintain.

## Verification Checks

Run or manually verify these before finalizing a split.

| Check | Purpose |
|---|---|
| Link check | Every relative Markdown link in split files and indexes points to an existing file or intentional anchor. |
| Section coverage | Each source heading or numbered section is represented in the destination files or explicitly marked out of scope. |
| Diff check | `git diff --check` has no whitespace errors. |
| Canonical link check | The root index points to the new split index or canonical files. |
| Old monolith notice | The old monolith clearly says it is source/history, not current canonical design. |
| Frontmatter status | The old monolith is no longer `status: canonical`, and the new canonical index or files carry the project's canonical status. |
| Inbound link check | Known links to the old monolith are updated to the new canonical destination or labeled as intentionally historical. |

For section coverage, prefer a lightweight table when the source has numbered headings:

```markdown
| Source Section | Destination | Status |
|---|---|---|
| 6.12 Notification rules | [notifications.md](notifications.md) | migrated |
| 6.16 Retry behavior | [batches.md](batches.md) | migrated |
```

`Status` here is migration status, not the `decided / assumption / open` status used in decision tables.

## Review Packet Evidence

When preparing a review packet for a document split, include:

- Original target file and line count.
- Destination file list and line counts.
- Link check result.
- Section coverage result.
- Whether the old monolith notice was added.
- Whether frontmatter status was updated on old and new files.
- Whether the root index canonical link changed.
- Whether inbound links to the old monolith were updated or intentionally preserved as historical links.
- Explicit non-scope, especially "no intentional content change" and "no further fine-grained split".

## Phased Splitting

Do not over-split the first migration. Prefer this order:

1. Split the monolith by major section or domain area.
2. Further split high-traffic areas by table, endpoint, batch, screen, or detailed process only when that improves maintenance or the user asks.
3. Convert parent documents into indexes or common specs when their detailed content has moved elsewhere.

For new documents or explicit scoped requests, use the finer one-file-per-endpoint/table/batch/screen rules in `references/split-design-docs.md`. For existing monolith migrations, prioritize safe canonical transition first.
