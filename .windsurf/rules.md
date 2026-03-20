# Windsurf Rules — LàmViệc360
## Master AI Coding Rules

**Project:** LàmViệc360 — AI-powered multi-tenant recruitment SaaS  
**Phase:** 1 MVP · Stage 1 Functional UI (HTML screens with flows)  
**Stack:** Vanilla HTML + CSS + JavaScript (Stage 1) → React 18 + TypeScript + Tailwind (Stage 2)  
**Doc ID:** LV360-PRD-002 v2.0

---

## 1. Project Context

You are building the **Stage 1 functional UI** for LàmViệc360 — a multi-tenant recruitment SaaS for the Vietnamese market. Stage 1 is **static HTML screens with realistic flows** that demonstrate every screen in the PRD before React development begins.

The platform has **five user roles** with completely separate UX layers:
- `job-seeker` → `/portal/*` routes, public-facing consumer feel
- `employer-admin` → `/employer/*` routes, SaaS dashboard feel
- `employer-hr` → `/employer/*` routes, same workspace, reduced permissions
- `employer-viewer` → `/employer/*` routes, read-only
- `super-admin` → `/admin/*` routes, control-plane feel

**Every screen you generate must be grounded in the PRD.** Reference `docs/product/PRD.md` before building any screen. If a requirement ID (e.g. `EMP-FR-016`) is mentioned in a task, look it up in the PRD first.

---

## 2. Global Screen Requirements

These two rules apply to EVERY screen without exception. 
Cascade must implement both before considering any screen complete.

### RULE G-001: Responsive Design (Mobile + Desktop)
Every screen must be fully responsive across three breakpoints:
- Mobile: 375px — primary design target (Vietnamese users are mobile-first)
- Tablet: 768px — secondary
- Desktop: 1280px — tertiary

Implementation checklist (verify before delivering any screen):
- [ ] No horizontal scroll at 375px
- [ ] All touch targets minimum 44×44px at mobile width
- [ ] Sidebar collapses to hamburger menu or bottom nav at mobile
- [ ] Kanban columns collapse to tabbed list view on mobile (EMP-FR-017)
- [ ] Filter panels open as bottom sheet on mobile (not side panel)
- [ ] Tables become card stacks on mobile
- [ ] Font size never below 14px on mobile
- [ ] Images and logos scale with CSS (max-width: 100%)

CSS breakpoint pattern to use in every screen:
```css
/* Mobile first — base styles target 375px */
/* Tablet */
@media (min-width: 768px)  { ... }
/* Desktop */
@media (min-width: 1280px) { ... }
```

### RULE G-002: Multilingual — English + Vietnamese
Every screen must support both languages with a toggle switch.

Implementation:
- Language toggle in the top navigation bar: [EN | VI] switcher
- Default language: Vietnamese (VI) — primary market
- All visible text elements must have both English and Vietnamese versions 
  defined in a data object at the top of each HTML file's <script> block
- Switching language must update all text instantly without page reload
- Selected language persisted in localStorage key "lv360_lang"

Pattern to implement in every screen:
```javascript
const LANG = {
  en: {
    pageTitle:     "Job Search",
    searchLabel:   "Search jobs...",
    applyBtn:      "Apply Now",
    salaryLabel:   "Salary",
    // ... all text strings for this screen
  },
  vi: {
    pageTitle:     "Tìm kiếm việc làm",
    searchLabel:   "Tìm kiếm việc làm...",
    applyBtn:      "Ứng tuyển ngay",
    salaryLabel:   "Mức lương",
    // ... all text strings for this screen
  }
};

function setLang(lang) {
  localStorage.setItem("lv360_lang", lang);
  document.querySelectorAll("[data-i18n]").forEach(el => {
    const key = el.getAttribute("data-i18n");
    if (LANG[lang][key]) el.textContent = LANG[lang][key];
  });
  document.querySelectorAll("[data-i18n-placeholder]").forEach(el => {
    const key = el.getAttribute("data-i18n-placeholder");
    if (LANG[lang][key]) el.placeholder = LANG[lang][key];
  });
  // Update toggle button states
  document.getElementById("lang-en").classList.toggle("active", lang === "en");
  document.getElementById("lang-vi").classList.toggle("active", lang === "vi");
}

// Init on load
const currentLang = localStorage.getItem("lv360_lang") || "vi";
setLang(currentLang);
```

HTML pattern for translatable elements:
```html
<!-- Text content -->
<button data-i18n="applyBtn">Ứng tuyển ngay</button>
<label data-i18n="salaryLabel">Mức lương</label>

<!-- Placeholder text -->
<input data-i18n-placeholder="searchLabel" placeholder="Tìm kiếm việc làm...">

<!-- Language toggle (place in every header/navbar) -->
<div class="lang-toggle">
  <button id="lang-vi" class="lang-btn active" onclick="setLang('vi')">VI</button>
  <span>|</span>
  <button id="lang-en" class="lang-btn" onclick="setLang('en')">EN</button>
</div>
```

Vietnamese-specific formatting rules (always apply regardless of language setting):
- Salary: "25.000.000 – 40.000.000 VND/tháng" (full format, never abbreviated)
- Dates: "15 Tháng 3, 2026" in VI · "March 15, 2026" in EN
- Phone: "+84 912 345 678" format throughout
- Currency symbol: VND (not ₫ abbreviation) in all salary displays

### RULE G-003: Product Logo
The official LàmViệc360 logo is located at:

  assets/images/logo.png

Rules for logo usage across all screens:

- Every screen that shows the product name or brand must use this logo file
- Use as an <img> tag — never render the brand name as plain text
- Always include alt="LàmViệc360" on the img tag
- Never hardcode the logo as inline SVG or CSS background

Recommended HTML pattern (copy into every header/navbar):
```html
<a href="../../screens/public/p01-homepage.html" class="logo-link">
  <img src="../../assets/images/logo.png" 
       alt="LàmViệc360" 
       class="logo-img">
</a>
```

Sizing by context:
- Public portal header:       height 36px, width auto
- Employer sidebar (top):     height 32px, width auto
- Super Admin sidebar (top):  height 28px, width auto
- Auth screens (centred):     height 44px, width auto
- Mobile (all contexts):      height 28px, width auto

CSS to include in every screen's <style> block:
```css
.logo-img {
  height: 36px;
  width: auto;
  display: block;
  object-fit: contain;
}
@media (max-width: 767px) {
  .logo-img { height: 28px; }
}
```

Fallback: If logo.png does not exist yet during development, 
render this placeholder instead — do NOT use plain text as fallback:
```html
<div class="logo-placeholder" aria-label="LàmViệc360">
  <span style="color:#2563EB;font-weight:700;font-size:20px;">
    LàmViệc<span style="color:#0F2044;">360</span>
  </span>
</div>
```

---

## 3. Code Quality Rules

### 3.1 HTML / Stage 1
- Every HTML file must be **fully self-contained** — all CSS inline in `<style>`, all JS inline in `<script>`.
- No external CDN dependencies. No `<link>` to external stylesheets.
- Use the shared design tokens defined in `docs/design/design-system.md`. Copy the CSS variables block into every screen's `<style>` tag.
- Every screen file must have a `<!-- SCREEN: [screen-id] [screen-name] -->` comment on line 2.
- Every screen must have a `<!-- PRD: [requirement-ids] -->` comment listing the requirements it implements.
- Every screen must have a visible **screen navigator** (dev overlay) in the bottom-right corner showing all screens in the flow.

### 3.2 Accessibility (WCAG 2.1 AA — NFR-013)
- All interactive elements must have `aria-label` or visible label.
- Colour contrast ratio ≥ 4.5:1 for all body text.
- Focus states visible on all focusable elements (`outline: 2px solid var(--color-primary)`).
- Form inputs must have associated `<label>` elements — never placeholder-only labels.
- Touch targets minimum 44×44px on mobile layouts.

### 3.3 Responsive (NFR-011, AC-011)
- Mobile-first: design for 375px width first, then 768px, then 1280px.
- Use CSS Grid and Flexbox — no fixed pixel widths on layout containers.
- Test every screen at 375px (mobile), 768px (tablet), 1280px (desktop).

### 3.4 Vietnamese Market
- All sample/placeholder text must use Vietnamese names and content:
  - Names: Nguyễn Thị Haanh, Trần Minh Đức, Lê Thị Thu, Bùi Văn Khoa
  - Companies: TechCorp Vietnam JSC, DataVN Ltd, FinApp Vietnam
  - Locations: Hà Nội, TP. Hồ Chí Minh, Đà Nẵng
  - Salary: displayed in VND (e.g. `15.000.000 – 25.000.000 VND/tháng`)
  - Phone: `+84 9xx xxx xxx` format
- UI labels can be English for Stage 1 but note where Vietnamese translation is needed.

---

## 4. File Naming Conventions

```
screens/
  public/           → p01-homepage.html, p02-job-search.html, p03-job-detail.html
  auth/             → a01-login.html, a02-register-role.html, a03-register-profile.html
                      a04-otp-verify.html, a05-forgot-password.html, a07-social-login.html
  onboarding/       → ow01-company-info.html, ow02-plan-confirm.html
                      ow03-team-invite.html, ow04-pending-approval.html
  job-seeker/       → js01-dashboard.html, js03-apply.html, js04-tracker.html
                      js05-app-detail.html, js06-saved-jobs.html, js07-profile.html
                      js09-documents.html, js10-upload-doc.html, js11-ai-prep.html
                      js12-notifications.html, js13-settings.html
  employer/         → e01-dashboard.html, e02-job-list.html, e03-create-job.html
                      e04-edit-job.html, e05-ai-jd-generator.html
                      e06-pipeline-kanban.html, e07-candidate-detail.html
                      e08-move-stage-modal.html, e09-reject-notify.html
                      e10-candidate-notes.html, e11-view-documents.html
                      e12-stage-config.html, e13-rejection-templates.html
                      e14-team-list.html, e15-invite-member.html, e16-edit-role.html
                      e17-notifications.html, e18-account-settings.html
                      e19-company-profile.html
  admin/            → sa01-dashboard.html, sa02-companies.html, sa03-company-detail.html
                      sa04-users.html, sa05-user-detail.html, sa06-ads.html
                      sa07-templates.html, sa08-retention.html, sa09-audit-log.html
                      sa10-reports.html, sa11-settings.html
```

---

## 5. Business Rules You Must Always Enforce

These rules from the PRD must be reflected in every relevant screen:

| Rule | What to show in UI |
|------|--------------------|
| BR-001 | Companies in "Pending" state — workspace locked, show pending banner |
| BR-002 | Job quota always shows 999 in Phase 1 (unlimited) |
| BR-003 | AI output always shows "AI Generated" badge + "Review & Confirm" gate |
| BR-005 | Pipeline history is immutable — no delete button on stage history |
| BR-007 | Invite links: show "Expires in 72 hours" countdown |
| BR-008 | Verified badge only after SA approval — not during pending |
| BR-009 | VND salary format throughout; negotiable = hidden range |
| BR-011 | Consent checkbox on every application form — cannot submit without |
| BR-013 | AI screening: transparency badge + HR override button always present |
| BR-014 | Contact details hidden below minimum stage threshold |

---

## 6. What NOT to Build in Stage 1

- ❌ No drag-and-drop (Phase 2)
- ❌ No subscription billing / payment screens (Phase 2)
- ❌ No company subdomains (Phase 2)
- ❌ No in-app messaging (Phase 2)
- ❌ No native mobile app (Post-MVP)
- ❌ No backend API calls — all data is hardcoded/mocked
- ❌ No authentication logic — simulate logged-in state with mock data

---

## 7. When You Are Unsure

1. Check `docs/product/PRD.md` first — the answer is almost always there.
2. Check `docs/design/design-system.md` for colours, spacing, and components.
3. Check `docs/design/wireframes.md` for layout guidance per screen.
4. If genuinely ambiguous, build the most conservative interpretation and add a `<!-- DECISION: [description] -->` comment.

---

*LV360-RULES-001 · v1.0 · March 2026 · tresundios Software*
