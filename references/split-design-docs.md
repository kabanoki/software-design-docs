# Split Design Docs

Use when creating or updating scoped design documents, especially one file per API endpoint, DB table, batch, screen, or detailed design item. Batch design is split per batch in the same way API design is split per endpoint. Detailed design is process-first: split by use case or process by default, and split complex implementation responsibilities only when they are worth unit testing. Shared rules and conventions are not embedded in individual files; write them as dedicated common specification files. Do not split existing monolithic documents automatically; use this only for new documents, explicit split requests, or additive updates where split docs are already the project pattern. For approved migrations from oversized monolithic documents, use `references/monolith-design-splitting.md`.

## Contents

- [Precedence](#precedence)
- [Standard Layout](#standard-layout)
- [Existing Monoliths](#existing-monoliths)
- [INDEX Tables](#index-tables)
- [File Naming](#file-naming)
- [Frontmatter](#frontmatter)
- [Existing File Matching](#existing-file-matching)
- [Detailed Design Items](#detailed-design-items)
- [Common Specs](#common-specs)
- [Links and Updates](#links-and-updates)
- [Quality Bar](#quality-bar)

## Precedence

Follow document rules in this order:

1. User's explicit instruction.
2. `AGENTS.md`, `CLAUDE.md`, or `CODEX.md`.
3. Existing `INDEX.md` or `README.md` under the docs tree.
4. Existing design document structure, naming, headings, table columns, and slug pattern.
5. This reference's standard rules.

If existing endpoint, table, batch, or screen design files already exist, match their heading structure, table order, naming, and slug style. This includes non-REST styles such as gRPC or command/event design documents.

## Standard Layout

Prefer this layout for split design docs when the project has no stronger rule. Keep split docs under the same `Documents/` root as other new-project design documents.

```text
Documents/design/
|-- api/
|   |-- INDEX.md
|   `-- <endpoint-slug>.md
|-- db/
|   |-- INDEX.md
|   `-- <table-name>.md
|-- batch/
|   |-- INDEX.md
|   `-- <batch-name>.md
|-- screen/
|   |-- INDEX.md
|   `-- <screen-name>.md
|-- detailed/
|   |-- INDEX.md
|   |-- processes/
|   |-- components/
|   `-- jobs/
`-- common/
    |-- INDEX.md
    `-- <common-spec>.md
```

Create one file per API endpoint, one file per DB table, one file per batch, one file per screen, and one file per detailed design item. For detailed design, use `processes/` for use cases and flows, `components/` for complex services/repositories/validators/components/commands, and `jobs/` for detailed job internals. Keep one flat `detailed/INDEX.md` so processes, components, and jobs can be reviewed together. Create common specification files only under `common/`; do not inline shared environment, authentication, authorization, response, validation, error, or logging rules into individual endpoint, table, batch, screen, or detailed files.

For monorepos or multiple services, add a service layer such as `Documents/design/api/<service>/...` only when the user or project rules require it. Otherwise assume a single-service layout.

## Existing Monoliths

For new documents and explicit scoped requests, use the fine-grained one-file-per-endpoint/table/batch/screen rules above. For a first split of an existing oversized monolith, do not jump straight to maximal granularity unless the user asks; follow `references/monolith-design-splitting.md`, preserve source content, switch canonical links safely, and only then consider further fine-grained splits for high-traffic areas.

## INDEX Tables

Use the existing INDEX format when present. For new split design directories, use these columns:

### API INDEX

| Endpoint | Purpose | Auth | Related DB | Related Common | design_status | File |
|---|---|---|---|---|---|---|
| `POST /orders` | Create an order | required | `orders`, `order_items` | `logging.md`, `error-levels.md` | draft | [post-orders.md](post-orders.md) |

### DB INDEX

| Table | Purpose | Key Columns | Related API/Batch | Migration | Backfill | design_status | File |
|---|---|---|---|---|---|---|---|
| `orders` | Order header | `id`, `customer_id`, `status` | `../api/post-orders.md` | yes | no | draft | [orders.md](orders.md) |

### Batch INDEX

| Batch | Purpose | Schedule | Related Tables | Related API | design_status | File |
|---|---|---|---|---|---|---|
| `sync-orders` | Sync external orders | daily 02:00 JST | `orders` | `../api/post-orders.md` | draft | [sync-orders.md](sync-orders.md) |

### Screen INDEX

| Screen | Route | Role | Purpose | Related API | design_status | File |
|---|---|---|---|---|---|---|
| Order list | `/orders` | operator | Search and inspect orders | `../api/get-orders.md` | draft | [order-list.md](order-list.md) |

### Detailed INDEX

| Target | Type | Purpose | Parent Process | Related API | Related DB | Related Batch/Screen | unit_test_required | design_status | File |
|---|---|---|---|---|---|---|---|---|---|
| Order creation | process | Create an order | - | `../api/post-orders.md` | `orders`, `order_items` | `../screen/order-create.md` | true | draft | [processes/order-creation.md](processes/order-creation.md) |

### Common INDEX

| Spec | Purpose | Referenced By | design_status | File |
|---|---|---|---|---|
| Logging | Log format and forbidden fields | `../api/post-orders.md`, `../batch/sync-orders.md` | draft | [logging.md](logging.md) |

Allowed `design_status` values are defined in [Frontmatter](#frontmatter). In Detailed INDEX, use `-` in `Parent Process` for process rows; frontmatter uses `parent_process: null` or omits it for the same case. The `Related Batch/Screen` INDEX column is combined for readability, while frontmatter keeps `related_batches` and `related_screens` separate for machine matching.

## File Naming

Endpoint slugs:

- Lowercase the HTTP method.
- Convert `/` path separators to `-`.
- Remove `{}` from path parameters and keep the parameter name.
- Preserve `snake_case`.
- Exclude query parameters from the filename; document them inside the endpoint file.
- Include API versions when they are part of the path.

Examples:

- `GET /users/{user_id}` -> `get-users-user_id.md`
- `POST /orders` -> `post-orders.md`
- `POST /v1/orders/{order_id}/items/{item_id}` -> `post-v1-orders-order_id-items-item_id.md`

If the normal endpoint slug exceeds 120 characters, shorten it to `<method>-<first-3-segments>-<last-2-segments>-<hash8>.md`. `hash8` is the first 8 lowercase hex characters of SHA-256 over normalized `METHOD path`, where `METHOD` is uppercase and `path` excludes query parameters, has one leading slash, collapses repeated slashes, and removes a trailing slash unless the path is `/`.

DB table filenames preserve the table name, including prefixes and `snake_case`: `tenant_orders` -> `tenant_orders.md`. For schema-qualified tables, use `<schema>__<table>.md`, for example `auth.users` -> `auth__users.md`.

Batch filenames preserve the batch or job name when it is already filesystem-safe. Otherwise normalize only separators and spaces to `-`, for example `daily order sync` -> `daily-order-sync.md`.

Screen filenames prefer the route when available: convert `/` separators to `-`, remove route parameter markers such as `[id]`, `{id}`, and `:id` while keeping the name, and exclude query parameters. Example: `/users/[user_id]/edit` -> `users-user_id-edit.md`. If no route exists, normalize the screen name like a batch name.

Detailed process filenames use an English noun phrase in kebab-case when the project has no stronger rule, for example `Order creation` -> `order-creation.md`. Detailed component filenames use `<target_type>-<target-name>.md` for all component-style types, such as `component-stock-ranking-table.md`, `service-stock-scoring.md`, or `validator-order-input.md`. Detailed job filenames use the batch/job name with a `-job` suffix unless already present, for example `sync-orders-job.md`; this keeps them distinct from `Documents/design/batch/sync-orders.md`.

## Frontmatter

Use frontmatter for new split design files. `status` describes document lifecycle and uses `canonical`, `draft`, `example`, `review_output`, or `obsolete`. `design_status` describes the design item's progress and uses `draft`, `reviewing`, `approved`, `implemented`, or `deprecated`. For detailed design, `target_type` uses `process`, `component`, `service`, `repository`, `validator`, `job`, or `command`; selection rules are in [Detailed Design Items](#detailed-design-items). These are different from the `Status` column in "決定事項・未決事項", which is limited to `decided`, `assumption`, or `open`.

API:

```yaml
---
doc_type: api_design
status: draft
design_status: draft
endpoint: "POST /orders"
related_tables:
  - orders
  - order_items
related_common:
  - ../common/logging.md
  - ../common/error-levels.md
related_detailed:
  - ../detailed/processes/order-creation.md
---
```

DB:

```yaml
---
doc_type: db_design
status: draft
design_status: draft
table: orders
related_apis:
  - ../api/post-orders.md
related_common:
  - ../common/logging.md
related_detailed:
  - ../detailed/processes/order-creation.md
migration_required: true
backfill_required: false
---
```

Batch:

```yaml
---
doc_type: batch_design
status: draft
design_status: draft
batch_name: sync-orders
schedule: "daily 02:00 JST"
related_apis:
  - ../api/post-orders.md
related_tables:
  - orders
related_common:
  - ../common/logging.md
related_detailed:
  - ../detailed/jobs/sync-orders-job.md
idempotent: true
---
```

Screen:

```yaml
---
doc_type: screen_design
status: draft
design_status: draft
screen_name: Order list
route: /orders
role: operator
related_apis:
  - ../api/get-orders.md
related_common:
  - ../common/authorization.md
related_detailed:
  - ../detailed/processes/order-listing.md
---
```

Detailed:

```yaml
---
doc_type: detailed_design
status: draft
design_status: draft
target_type: process
target_name: Order creation
parent_process: null
related_apis:
  - ../../api/post-orders.md
related_tables:
  - orders
  - order_items
related_batches: []
related_screens:
  - ../../screen/order-create.md
related_common:
  - ../../common/logging.md
unit_test_required: true
---
```

Common:

```yaml
---
doc_type: common_spec
status: draft
design_status: draft
spec_name: Logging
referenced_by:
  - ../api/post-orders.md
  - ../batch/sync-orders.md
---
```

## Existing File Matching

Before creating a new split design file, search for an existing file and update it when found:

- API: frontmatter `endpoint` -> endpoint slug filename -> main heading -> API INDEX entry.
- DB: frontmatter `table` -> table filename -> main heading -> DB INDEX entry.
- Batch: frontmatter `batch_name` -> batch filename -> main heading -> Batch INDEX entry.
- Screen: frontmatter `screen_name` or `route` -> screen filename -> main heading -> Screen INDEX entry.
- Detailed: frontmatter `target_type` + `target_name` -> detailed filename -> main heading -> Detailed INDEX entry.

Do not create duplicates such as `post-orders-2.md` unless the user explicitly asks for a separate variant and explains why. If two targets normalize to the same filename, prefer a meaningful suffix from the business action or variant, such as `post-orders-bulk.md` or `get-orders-search.md`.

## Detailed Design Items

Use detailed design for behavior: process flow, state transitions, branch conditions, transaction boundaries, external dependency handling, and implementation-level error behavior. Use `process` as the default `target_type`. Allowed `target_type` values are `process`, `component`, `service`, `repository`, `validator`, `job`, and `command`.

`parent_process` is `null` or omitted for `process` files. For `component`, `service`, `repository`, `validator`, `job`, and `command` files, `parent_process` is required and must point to the parent process file.

Create individual detailed files only for responsibilities that are worth unit testing. Set `unit_test_required: true` when the target has branching, calculation, DB writes, state transitions, external API interpretation, exception/retry/idempotency behavior, or authorization/permission rules. Treat simple DTOs, enums, thin controllers, static configuration, simple presenters, and simple type definitions as `unit_test_required: false`; do not create individual detailed files for them unless the user explicitly asks.

`batch/` documents describe the contract and operations of a batch: trigger, schedule, inputs, outputs, idempotency, rerun, and monitoring. `detailed/jobs/` documents describe the internal implementation logic of a job: branches, state transitions, data updates, and error handling. Link both directions when both exist.

Apply `references/detailed-design.md` by target type. Process files should use all relevant sections, especially processing flow, abnormal paths, data updates, and transaction/concurrency behavior. Component, service, repository, validator, job, and command files should focus on scope, inputs/outputs, processing details, validation/error handling, security, logs, related design links, and unit test viewpoints without copying code.

## Common Specs

Create only common specs that are needed, and always create them as dedicated files under `common/`. Common candidates include `environment-variables.md`, `error-levels.md`, `logging.md`, `authentication.md`, `authorization.md`, `pagination.md`, `validation.md`, and `response-format.md`.

Do not duplicate common specification details inside individual API, DB, batch, screen, or detailed files. Each individual design file must include a `関連共通仕様` section with links to the common specs it depends on. If no common spec applies, write `該当なし: <reason>`.

Keep `common/logging.md` limited to shared log format, log levels, field naming, correlation IDs, masking, and forbidden fields. For feature-specific SLO, metrics, traces, alerts, and operational drilldown, use `references/observability.md` and create a separate observability design document when needed.

## Links and Updates

Maintain bidirectional navigation:

- API design links to related DB, batch, screen, and common specs.
- DB design links to related API, batch, and common specs.
- Batch design links to related DB, API, and common specs.
- Screen design links to related API and common specs.
- Detailed design links to related API, DB, batch, screen, and common specs.

Whenever a split design file is created or updated, update the related `INDEX.md` in the same change. When detailed design links to API, DB, batch, or screen files, add or update a `関連詳細設計` section or `related_detailed` frontmatter entry on the structural design file. Verify that frontmatter, links, INDEX related fields, and `design_status` agree.

For deprecation, do not delete files unless the user explicitly requests deletion. Set `design_status: deprecated` in the file and INDEX, keep the INDEX row for history, and add a note explaining the replacement or reason. If the user explicitly requests deletion, follow the archive rules in `references/document_indexing.md` instead of hard-deleting by default.

## Quality Bar

- A future agent can find the exact endpoint, table, batch, screen, detailed design item, or common spec without loading unrelated documents.
- Scoped requests do not generate unrelated document sets.
- INDEX rows, frontmatter, and bidirectional links agree.
- Common specs are linked from individual docs but not duplicated.
