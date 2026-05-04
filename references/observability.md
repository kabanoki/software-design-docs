# Observability Template

Use for 可観測性設計. Define logs, metrics, traces, and alerts so that operators can answer "is it working, what changed, who is affected" without reading source code.

## Recommended Sections

1. **設計方針**
   - Goals (debuggability, SLO tracking, audit, security forensics).
   - Tooling (log sink, metrics backend, tracing system, alert routing).
   - Retention and access control.
2. **ログ**
   - Format (structured JSON, key naming, timestamp/timezone).
   - Correlation IDs (request_id, trace_id, user_id) and propagation rules.
   - **ログイベント一覧**: event name, level (debug/info/warn/error), included fields, PII masking policy, sampling/rate, destination.
   - PII / secret masking rules and forbidden fields.
3. **メトリクス**
   - RED / USE / business metrics that matter.
   - Naming convention, labels/cardinality budget.
   - Aggregation window and dashboard ownership.
4. **トレース**
   - Spans of interest, parent/child relationships, baggage.
   - Sampling strategy (head/tail, rate by route).
   - PII handling in span attributes.
5. **アラート**
   - Alert name, condition, severity, runbook link, on-call rotation.
   - Suppression / dependency rules to avoid alert storms.
   - Test/drill plan.
6. **SLO / 可用性指標**
   - SLI definition, target SLO, error budget policy.
   - Reporting cadence and stakeholders.
7. **運用導線**
   - Where operators look first (dashboard URL).
   - Drilldown path: alert → dashboard → log query → trace.
   - Audit/security log access procedure.
8. **決定事項・未決事項**

## Quality Bar

- An on-call engineer can go from alert to root-cause query in under 5 minutes using only documents in this template.
- Cardinality budget is explicit; no unbounded labels (raw user_id, request body) in metrics.
- PII / secret handling is documented for every signal type (log, metric label, trace attribute).
- Each alert has a runbook and an owner. No "fires but nobody knows what to do" alerts.
