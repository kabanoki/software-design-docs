# Impact Scope and Cautions Template

Use for 影響範囲 and 注意点. Capture what may break, what must be communicated, and what needs special care.

## Impact Scope Sections

1. **変更概要**
   - What changes.
   - Why it changes.
   - Deployment or migration timing.
2. **影響対象**
   - Users/roles.
   - Screens.
   - APIs.
   - Batches/jobs.
   - DB/tables.
   - Reports/exports.
   - Notifications.
   - External services.
   - Infrastructure/operations.
3. **互換性**
   - Existing data.
   - Existing API clients.
   - Backward compatibility.
   - Rollback constraints.
4. **リスク**
   - Functional risk.
   - Data risk.
   - Security/privacy risk.
   - Operational risk.
   - Cost/vendor risk.
5. **緩和策**
   - Tests.
   - Feature flags.
   - Backup.
   - Monitoring.
   - Manual rollback or rerun.
6. **リリース前確認**
   - Required checks.
   - Owner.
   - Evidence.

## Cautions Sections

1. **実装注意**
   - Ordering constraints.
   - Doc-first requirements.
   - Existing patterns to preserve.
2. **運用注意**
   - Schedules.
   - Credentials.
   - Manual steps.
   - Failure handling.
3. **セキュリティ注意**
   - Secrets.
   - PII.
   - Auth/access control.
   - Logs and DB persistence.
4. **未決事項**
   - Decisions needed before implementation.
   - Decisions that may be deferred.

## Quality Bar

- Impact is concrete enough to guide review and rollout.
- Cautions are actionable, not vague warnings.
- Security and operations are not afterthoughts.
- Deferred items have explicit consequences.
