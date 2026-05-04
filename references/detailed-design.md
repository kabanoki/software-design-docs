# Detailed Design Template

Use for 詳細設計書. Define implementation behavior precisely enough that another engineer can build without inventing contracts.

## Recommended Sections

1. **対象範囲**
   - Feature/module boundary.
   - Related requirement and basic design sections.
2. **処理詳細**
   - Commands, services, controllers, jobs, screens, importers/exporters, validators.
   - Important sequence and state transitions.
   - Idempotency and concurrency rules.
3. **入力・出力**
   - Parameters, request fields, files, events, schedules.
   - Response, report, notification, DB output, logs.
4. **データ更新仕様**
   - Tables/collections touched.
   - Create/update/delete/upsert behavior.
   - History/audit records.
5. **バリデーション・エラー処理**
   - Invalid input.
   - Missing data.
   - External API failure.
   - Partial success.
   - Retry/skip/fail behavior.
6. **権限・セキュリティ**
   - Roles/permissions.
   - Secret handling.
   - PII masking.
   - Injection/XSS/SSRF/path traversal concerns where relevant.
7. **ログ・監視**
   - Logs, metrics, audit trails, alerts, operator-visible states.
8. **試験観点リンク**
   - Unit, feature, integration, E2E, migration, security, operational checks.
9. **決定事項・未決事項**
   - Confirmed implementation choices.
   - Assumptions/defaults.
   - Open questions.

## Quality Bar

- Interfaces and state transitions are decision-complete.
- Failure behavior is explicit for every external dependency and destructive operation.
- Data writes are clear enough to derive migration and test work.
- Detailed design does not contradict higher-level documents.
