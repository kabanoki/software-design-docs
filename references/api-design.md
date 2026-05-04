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
   - Injection, XSS, CSRF, SSRF, secret exposure, PII handling.
8. **決定事項・未決事項**

## Quality Bar

- Clients can implement without guessing field names or error handling.
- Error and authorization behavior is explicit.
- API side effects are visible.
- Existing API compatibility risks are called out.
