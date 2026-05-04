# DB Design Template

Use for DB設計. Define persistent data structures, relationships, constraints, indexes, and lifecycle.

## Recommended Sections

1. **設計方針**
   - DB engine assumptions.
   - Naming conventions.
   - Date/time/timezone handling.
   - JSON/default value policy.
2. **ER概要**
   - Main entities and relationships.
   - Ownership and cardinality.
3. **テーブル定義**
   - Table purpose.
   - Columns, types, nullability, defaults.
   - Primary keys, foreign keys, unique constraints.
   - Indexes and expected query patterns.
4. **データライフサイクル**
   - Insert/update/delete rules.
   - Retention.
   - Archive/cleanup.
   - Rebuild/reimport behavior.
5. **移行・互換性**
   - Migration order.
   - Backfill.
   - Rollback constraints.
   - Existing data impact.
6. **セキュリティ・監査**
   - Sensitive columns.
   - Masking/encryption.
   - Audit/history tables.
7. **決定事項・未決事項**

## Quality Bar

- Every table has a clear owner and purpose.
- Indexes map to actual query patterns.
- Defaults and nullability are intentional.
- Data lifecycle is explicit for operationally important tables.
