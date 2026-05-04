# Requirements Document Template

Use for 要件定義書. Capture what the system must achieve and why, without locking in implementation details too early.

## Recommended Sections

1. **概要**
   - System or feature name.
   - One-paragraph purpose.
   - Primary users and operating context.
2. **背景・課題**
   - Current pain or opportunity.
   - Why this work matters now.
   - Existing manual work, system limits, or operational risks.
3. **目的・成功条件**
   - Business or user outcomes.
   - Observable success criteria.
   - Acceptance-level completion conditions.
4. **対象範囲**
   - In scope.
   - Out of scope.
   - Future candidates.
5. **機能要件**
   - User-facing requirements.
   - Admin/operator requirements.
   - Batch, notification, report, import/export, and audit requirements when relevant.
6. **非機能要件**
   - Security, privacy, performance, availability, maintainability, auditability, timezone/localization, accessibility, data retention.
7. **外部連携・制約**
   - External APIs, accounts, plans, credentials, rate limits, terms, delivery channels, infrastructure constraints.
8. **データ要件**
   - Key entities.
   - Required history/audit data.
   - Sensitive data and masking requirements.
9. **運用要件**
   - Schedules, manual rerun, monitoring, backup, restore, incident handling.
10. **決定事項・未決事項**
   - Confirmed decisions.
   - Assumptions/defaults.
   - Open questions with owner or next action.

## Quality Bar

- A developer can tell what must be built and what must not be built.
- Requirements avoid implementation names unless the existing system already makes them user-facing.
- Every high-risk assumption is visible.
- Non-goals are strong enough to stop accidental scope expansion.
