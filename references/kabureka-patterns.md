# Kabureka-Inspired Documentation Patterns

Use this only as a reference example. Do not treat Kabureka-specific finance, J-Quants, Cloudflare, or Laravel decisions as universal defaults.

## Patterns Worth Reusing

- Maintain a document index that tells agents which files to read first and which archives/reviews to ignore unless needed.
- Keep requirements, basic design, detailed design, acceptance checks, operations, infrastructure, process, prompts, reviews, and archives in separate folders.
- Require specification/design updates before implementation when behavior, data contracts, APIs, batch flows, reports, or UI/UX change.
- Store phase review outputs under a timestamped `reviews/` folder and include a resolution note when findings are addressed or deferred.
- For batch-heavy systems, maintain a batch-specific documentation index plus separate pages for parent batch flow, data fetch jobs, screening/report/notification jobs, analysis/manual jobs, and monitoring/internal jobs.
- In operational docs, include commands, target dates/timezones, logs, idempotency, rerun behavior, failure handling, and dashboard/admin visibility.
- In user-facing reports and notifications, explain domain-specific flags/ranks and include required caution/disclaimer language.

## Anti-Patterns to Avoid

- Loading every historical review or archived design into context by default.
- Mixing requirements, implementation details, and operations in one long document.
- Recording generated review output without a concise resolution file.
- Saying a batch is rerunnable without documenting what it skips, overwrites, or re-sends.
- Letting UI labels expose internal English codes without tooltips or explanations.
