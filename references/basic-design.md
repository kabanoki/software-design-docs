# Basic Design Template

Use for 基本設計書. Translate requirements into the system-level shape: components, data flow, integration points, and operational model.

## Recommended Sections

1. **設計概要**
   - Design goal.
   - Scope covered by this document.
   - Relationship to the requirements document.
2. **全体構成**
   - Major subsystems.
   - UI, API, batch, DB, external services, notifications, storage, authentication.
   - Deployment/runtime assumptions.
3. **主要フロー**
   - User flows.
   - Batch flows.
   - External integration flows.
   - Error and retry flows at a high level.
4. **データ設計方針**
   - Main entities and ownership.
   - Audit/history approach.
   - Sensitive data handling.
5. **API・外部IF方針**
   - Public/internal APIs.
   - External API usage.
   - Authentication and authorization boundaries.
6. **画面・UX方針**
   - Main screens.
   - Navigation.
   - Empty, error, loading, and permission states.
7. **バッチ・運用方針**
   - Schedules.
   - Idempotency.
   - Manual rerun.
   - Monitoring and alerting.
8. **非機能設計方針**
   - Security, performance, availability, backup/restore, logging, observability.
9. **決定事項・未決事項**
   - Confirmed architecture choices.
   - Assumptions/defaults.
   - Open questions.

## Quality Bar

- Requirements can be traced to a subsystem or flow.
- The document avoids code-level detail unless needed to prevent architectural ambiguity.
- External dependencies and operational responsibilities are clear.
- Another engineer can decide which detailed design documents are required.
