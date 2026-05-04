# Batch Design Template

Use for バッチ設計. Define scheduled, manual, and internal batch jobs with dependencies, idempotency, observability, and recovery.

## Recommended Sections

1. **バッチ概要**
   - Batch name.
   - Purpose.
   - Trigger: cron, scheduler, manual, event, internal helper.
   - Standard runtime and timezone.
2. **実行条件**
   - Inputs and options.
   - Environment variables.
   - Required prior data.
   - External services.
3. **処理順序**
   - Step-by-step sequence.
   - Child jobs or commands.
   - Transaction boundaries when important.
4. **入出力**
   - DB reads/writes.
   - Files/reports.
   - Notifications.
   - Logs/audit records.
5. **冪等性・再実行**
   - Skip conditions.
   - Force/rebuild behavior.
   - Locking and concurrency.
   - Partial success handling.
6. **失敗時対応**
   - Expected failure modes.
   - Retry policy.
   - Manual recovery.
   - Monitoring/alerting.
7. **運用確認**
   - Success checks.
   - Dashboard/admin visibility.
   - Log locations.
8. **決定事項・未決事項**

## Quality Bar

- An operator can safely rerun the batch.
- Dependencies and destructive behaviors are clear.
- Monitoring can distinguish running, success, partial, failed, skipped, and stale states where applicable.
- External API constraints and schedules are visible.
