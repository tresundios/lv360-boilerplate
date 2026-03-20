# AI Rules — LàmViệc360
## Instructions for Windsurf AI (Cascade)

---

## 1. How to Use These Rules

When Windsurf Cascade generates code for LàmViệc360, it must:

1. **Always read `docs/product/PRD.md` first** when given any feature task
2. **Always read `.windsurf/frontend.md`** before writing any HTML/CSS
3. **Always read `.windsurf/ui-ux.md`** before designing any screen layout
4. **Cite requirement IDs** in comments — e.g. `<!-- EMP-FR-016: Kanban pipeline -->`
5. **Never invent features** not in the PRD without flagging them as `<!-- ADDITION: not in PRD -->`

---

## 2. Prompt Patterns for Common Tasks

### Build a screen from scratch

```
Build [screen-id] — [screen-name] as a self-contained HTML file.

Requirements from PRD:
- [requirement-id]: [description]
- [requirement-id]: [description]

Follow .windsurf/frontend.md for CSS variables and component patterns.
Follow .windsurf/ui-ux.md for this screen's UX spec.
Include the dev navigator in the bottom-right corner.
Use Vietnamese mock data.
```

### Add a component to an existing screen

```
Add a [component name] to [screen-file].html.
This implements [requirement-id] from PRD Section [X.X].
Follow the component pattern in .windsurf/frontend.md Section 3.
```

### Connect screens with navigation

```
Add navigation links between these screens:
[screen-a] → [screen-b] on [action]
[screen-b] → [screen-c] on [action]

Use the dev navigator pattern from .windsurf/frontend.md Section 4.3.
```

---

## 3. PRD Requirement Lookup Rules

When a task mentions a screen or feature, Cascade must look up the corresponding PRD requirement:

| Task involves | Look up in PRD |
|--------------|---------------|
| Login / Registration | Section 3.1 AUTH-FR-001 to 009 |
| Job search / listing | Section 3.2 JSP-FR-001 to 003 |
| Job seeker profile | Section 3.2 JSP-FR-004, 011 |
| Job application | Section 3.2 JSP-FR-005, BR-011 |
| Application tracker | Section 3.2 JSP-FR-007 |
| AI Interview Prep | Section 3.2 JSP-FR-009 |
| Document archive | Section 3.2 JSP-FR-012 |
| Company onboarding | Section 3.3.1 EMP-FR-001 to 004 |
| Employer dashboard | Section 3.3.2 EMP-FR-005 to 008 |
| Create/edit job | Section 3.3.3 EMP-FR-009 to 015 |
| AI JD generator | Section 3.3.3 EMP-FR-010, BR-003 |
| Candidate pipeline | Section 3.3.4 EMP-FR-016 to 025b |
| Bulk actions / undo | Section 3.3.4 EMP-FR-019b, 020 |
| AI screening | Section 3.3.4 EMP-FR-023, 023b, BR-013 |
| Rejection templates | Section 3.3.4 EMP-FR-025b |
| Team management | Section 3.3.5 EMP-FR-026 to 029 |
| Super Admin companies | Section 3.4 SAP-FR-001 to 003 |
| Super Admin analytics | Section 3.4 SAP-FR-004 |
| Notifications | Section 3.5 NTF-FR-001 to 006 |

---

## 4. Business Rules Cascade Must Never Violate

These are absolute constraints in any generated code:

```javascript
// BR-001: Locked workspace banner
if (company.status === 'pending') showPendingBanner();

// BR-002: Job quota always 999 in Phase 1
const jobQuotaDisplay = 999;

// BR-003: AI output always needs confirmation gate
function showAIOutput(content) {
  renderWithBadge(content); // Never auto-save
  requireUserConfirmation(); // Always show confirm button
}

// BR-007: Invite link expiry
const INVITE_EXPIRY_HOURS = 72;

// BR-009: Salary display
function displaySalary(job) {
  if (job.salaryNegotiable) return 'Thương lượng';
  return `${job.salaryMin.toLocaleString('vi-VN')} – ${job.salaryMax.toLocaleString('vi-VN')} VND/tháng`;
}

// BR-011: Consent required on every application
function validateApplicationForm(form) {
  if (!form.consentChecked) {
    showError('Bạn phải đồng ý với điều khoản để nộp đơn.');
    return false;
  }
  return true;
}

// BR-013: AI screening always needs override capability
function renderAIScreeningResults(results) {
  results.forEach(r => {
    renderWithTransparencyBadge(r);
    renderOverrideButton(r); // Always present
  });
  // Never allow auto-rejection based on AI alone
}

// BR-014: Contact visibility gated by stage
function canViewContact(candidate, currentStage, minStage) {
  const stageOrder = ['applied','screening','shortlisted','interview','offer','hired'];
  return stageOrder.indexOf(currentStage) >= stageOrder.indexOf(minStage);
}
```

---

## 5. Accessibility Checklist (NFR-013)

Cascade must verify these before completing any screen:

- [ ] All images have `alt` text (or `alt=""` for decorative images)
- [ ] All interactive elements have `aria-label` or visible label
- [ ] Form inputs have associated `<label for="...">` elements
- [ ] Colour is never the only way to convey information (use text + icon too)
- [ ] Focus order is logical (matches visual reading order)
- [ ] Skip navigation link at top: `<a href="#main" class="skip-link">Skip to content</a>`
- [ ] Error messages are associated with fields via `aria-describedby`
- [ ] Modal dialogs trap focus and restore it on close

---

## 6. What Cascade Must NOT Generate

These are out of scope for Stage 1. If asked, generate a placeholder with a comment:

```html
<!-- OUT OF SCOPE (Phase 2): Drag-and-drop pipeline -->
<div class="feature-placeholder">
  <p>Drag-and-drop will be available in Phase 2.</p>
</div>
```

Out of scope items:
- Drag-and-drop Kanban (`<!-- Phase 2 -->`)
- Subscription / billing screens (`<!-- Phase 2 -->`)
- Company subdomains (`<!-- Phase 2–3 -->`)
- In-app messaging (`<!-- Phase 2 -->`)
- Native mobile app (`<!-- Post-MVP -->`)
- Real API calls (`<!-- Use mock data only in Stage 1 -->`)
- Real authentication (`<!-- Simulated in Stage 1 -->`)

---

## 7. Code Review Checklist for Generated Screens

After generating a screen, verify:

1. ✅ Screen ID comment on line 2
2. ✅ PRD requirement IDs listed in comment
3. ✅ CSS variables from `frontend.md` imported
4. ✅ Vietnamese mock data used
5. ✅ Dev navigator in bottom-right
6. ✅ Mobile-first (375px works without horizontal scroll)
7. ✅ All business rules reflected in UI (BR-001 to BR-014)
8. ✅ No Phase 2 features implemented
9. ✅ Consent checkbox on any application form (BR-011)
10. ✅ AI output always shows badge + confirm gate (BR-003)

---

*LV360-AI-001 · v1.0 · March 2026 · tresundios Software*
