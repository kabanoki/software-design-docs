# API Design Template

Use for API設計. Define HTTP/internal API contracts, authentication, validation, errors, and compatibility.

## Recommended Sections

1. **API概要**
   - Public/internal/admin API classification.
   - Consumers.
   - Versioning and compatibility policy.
2. **認証・認可**
   - Auth method.
   - Roles/scopes.
   - Access control by resource.
3. **Endpoint一覧**
   - Method and path.
   - Purpose.
   - Request source.
   - Response summary.
4. **Endpoint詳細**
   - Request path/query/body.
   - Validation.
   - Response schema.
   - Status codes.
   - Error body.
   - Pagination/sorting/filtering.
   - 日時表現規約: ISO 8601 (e.g. `2026-05-04T12:34:56Z` or `+09:00`). State whether the API accepts/returns UTC, a fixed timezone, or client-local, and how naive datetimes are rejected.

   Per endpoint, document with concrete schema and examples. Use this layout (outer fence is four backticks so JSON examples can stay inside):

````markdown
### POST /v1/orders

**Auth**: Bearer token, scope `orders:write`
**Idempotency**: required via `Idempotency-Key` header

**Request body**

| Field | Type | Required | Notes |
|---|---|---|---|
| customer_id | string (uuid) | yes | must exist |
| items | array<Item> | yes | min 1, max 100 |
| items[].sku | string | yes | `^[A-Z0-9-]{1,32}$` |
| items[].qty | integer | yes | >= 1 |
| note | string | no | max 500 chars |

```json
{
  "customer_id": "0b7a...e3",
  "items": [{ "sku": "ABC-001", "qty": 2 }],
  "note": "gift wrap"
}
```

**Response 201**

```json
{
  "id": "ord_01HYZ...",
  "status": "created",
  "created_at": "2026-05-04T12:34:56Z"
}
```

**Errors**

| Status | code | when |
|---|---|---|
| 400 | `invalid_request` | schema/validation failure |
| 401 | `unauthorized` | missing or invalid token |
| 403 | `forbidden` | scope mismatch |
| 409 | `idempotency_conflict` | same key, different payload |
| 422 | `out_of_stock` | item unavailable |
````
5. **副作用**
   - DB writes.
   - External calls.
   - Notifications/events/jobs.
6. **制限事項**
   - Rate limits.
   - Size limits.
   - Idempotency keys.
   - Caching.
7. **セキュリティ**

   For each item, document the threat surface and the chosen mitigation. When an item is not applicable, write `該当なし: <reason>` rather than omitting.

   - **Injection**: query/command builders used, parameterization strategy, allowlist for dynamic identifiers.
   - **XSS**: response content type, escaping policy for HTML/JSON/SVG, untrusted-field allowlist.
   - **CSRF**: cookie/session usage, SameSite settings, anti-CSRF token mechanism, exempted endpoints.
   - **SSRF**: outbound URL validation, allowed schemes/hosts, metadata-IP blocking.
   - **Secret exposure**: where secrets live, masking in logs/errors/traces, rotation policy.
   - **PII handling**: which fields are PII, masking in logs/responses, retention/deletion path, audit log scope.
8. **決定事項・未決事項**

## Quality Bar

- Clients can implement without guessing field names or error handling.
- Error and authorization behavior is explicit.
- API side effects are visible.
- Existing API compatibility risks are called out.
