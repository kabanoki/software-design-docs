# Test Viewpoints Template

Use for 試験観点. Create acceptance and verification viewpoints, not just low-level test cases.

## Recommended Sections

1. **試験方針**
   - Objective.
   - Test levels: unit, integration, feature, E2E, operation, security.
   - What counts as pass/fail.
2. **要件別観点**
   - Requirement ID or section.
   - Scenario.
   - Expected observable result.
   - Test data.
3. **機能別観点**
   - UI.
   - API.
   - Batch.
   - DB/migration.
   - Report/export.
   - Notification.
4. **異常系・境界値**
   - Invalid input.
   - Missing data.
   - Duplicate data.
   - External service errors.
   - Permission errors.
   - Timezone/date boundaries.
5. **非機能観点**
   - Performance.
   - Security.
   - Accessibility.
   - Observability.
   - Backup/restore.
6. **回帰観点**
   - Existing behavior that must not change.
   - Compatibility.
   - Data migration safety.
7. **受け入れ確認表**
   - Check item.
   - Status.
   - Evidence command or manual check.
   - Notes.
8. **未確認・保留**

## Quality Bar

- Tests validate external behavior rather than implementation details.
- Each high-risk requirement has at least one positive and one failure/edge viewpoint.
- Manual checks include observable evidence.
- Regression risk is explicit.
