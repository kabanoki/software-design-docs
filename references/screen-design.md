# Screen Design Template

Use for 画面設計. Define screen purpose, layout, data, interactions, permissions, and states.

## Recommended Sections

1. **画面概要**
   - Screen name.
   - User role.
   - Purpose and primary task.
   - Route/navigation entry.
2. **表示項目**
   - Main sections.
   - Fields, labels, units, formatting.
   - Empty, loading, error, no-permission states.
3. **操作**
   - Buttons, filters, forms, modals, tooltips, navigation.
   - Confirmation requirements.
   - Keyboard/mobile considerations when relevant.
4. **データ取得・更新**
   - Source API/query.
   - Refresh timing.
   - Mutations and side effects.
5. **権限・セキュリティ**
   - Who can view.
   - Who can operate.
   - Sensitive data display/masking.
6. **UI/UX注意**
   - Domain language.
   - Accessibility.
   - Localization.
   - Explanation text, tooltips, and help for ambiguous labels/icons.
7. **試験観点**
   - Rendering.
   - Interaction.
   - Error states.
   - Responsive behavior.
8. **決定事項・未決事項**

## Quality Bar

- A frontend engineer can build the screen without inventing labels or states.
- Ambiguous icons, codes, ranks, and flags have explanations.
- Empty and error states are first-class.
- Permissions and sensitive data are clear.
