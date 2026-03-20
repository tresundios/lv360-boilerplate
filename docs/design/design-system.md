# Design System — LàmViệc360
## Visual Language, Tokens & Component Library

---

## 1. Brand Identity

**Product name:** LàmViệc360 *(rendered as: `LàmViệc` + `360` in accent colour)*  
**Tagline:** Tuyển dụng thông minh — AI-powered hiring for Vietnam  
**Brand personality:** Trustworthy · Modern · Efficient · Vietnamese-first

---

## 2. Colour System

### 2.1 Primary Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `--color-primary` | `#2563EB` | Primary buttons, links, active states |
| `--color-primary-dark` | `#1D4ED8` | Button hover, pressed |
| `--color-primary-light` | `#EFF6FF` | Selected backgrounds, info tints |
| `--color-navy` | `#0F2044` | Employer sidebar, dark headers |
| `--color-navy-mid` | `#1A4A8A` | Sidebar items, secondary headings |
| `--color-navy-light` | `#DBEAFE` | Nav highlights |

### 2.2 Admin Palette (Super Admin panel only)

| Token | Hex | Usage |
|-------|-----|-------|
| `--color-admin-bg` | `#1A0A3A` | Admin sidebar background |
| `--color-admin-accent` | `#7C3AED` | Admin accent, active states |
| `--color-admin-light` | `#EDE9FE` | Admin tints |

### 2.3 Semantic Colours

| Token | Hex | Usage |
|-------|-----|-------|
| `--color-success` | `#15803D` | Success text, Verified badge |
| `--color-success-bg` | `#DCFCE7` | Success backgrounds |
| `--color-warning` | `#C8922A` | Warning text, Pending badge |
| `--color-warning-bg` | `#FEF9C3` | Warning backgrounds |
| `--color-danger` | `#B91C1C` | Error text, Rejected badge, destructive actions |
| `--color-danger-bg` | `#FEF2F2` | Error backgrounds |
| `--color-info` | `#0284C7` | Info text |
| `--color-info-bg` | `#E0F2FE` | Info backgrounds |

### 2.4 Neutral Ramp

| Token | Hex | Usage |
|-------|-----|-------|
| `--color-grey-900` | `#0F172A` | Primary text |
| `--color-grey-700` | `#334155` | Body text |
| `--color-grey-500` | `#64748B` | Secondary text, labels |
| `--color-grey-300` | `#CBD5E1` | Borders, dividers |
| `--color-grey-100` | `#F1F5F9` | Page background |
| `--color-white` | `#FFFFFF` | Card surfaces |

### 2.5 Colour Usage Rules
- **Never use colour alone** to convey meaning — always pair with text or icon
- **Contrast minimum:** 4.5:1 for body text, 3:1 for large text (WCAG AA — NFR-013)
- **VND currency** — always display in full, never abbreviated in salary fields

---

## 3. Typography

| Token | Size | Weight | Usage |
|-------|------|--------|-------|
| `--text-xs` | 11px | 400 | Metadata, timestamps, badges |
| `--text-sm` | 13px | 400/500 | Body secondary, table content |
| `--text-base` | 15px | 400 | Body primary, descriptions |
| `--text-lg` | 17px | 500 | Section labels, card titles |
| `--text-xl` | 20px | 600 | Page subtitles |
| `--text-2xl` | 24px | 700 | Page titles |
| `--text-3xl` | 30px | 700 | Hero headings |

**Font stack:** `'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`  
**Vietnamese characters:** Full Unicode support required — all fonts must render ă, â, đ, ê, ô, ơ, ư and all tone marks correctly.

---

## 4. Spacing (8pt Grid)

All spacing must use multiples of 4px:

```
4px   --space-1   Tight: icon-to-text, badge internal
8px   --space-2   Default gap: inline elements
12px  --space-3   Small gap: form field spacing
16px  --space-4   Default: card padding, section gaps
20px  --space-5   Medium: between components
24px  --space-6   Large: section padding
32px  --space-8   XL: major sections
48px  --space-12  Page-level padding
64px  --space-16  Hero sections
```

---

## 5. Component Library

### 5.1 Buttons

```
Size variants:  sm (28px height) · md (36px) · lg (44px — mobile default)
Style variants: primary · secondary · danger · ghost · outline
State variants: default · hover · active · disabled · loading
```

**Loading state:**
```html
<button class="btn btn-primary loading" disabled>
  <span class="spinner"></span> Generating...
</button>
```

### 5.2 Status Badges

| Badge | Colours | Usage |
|-------|---------|-------|
| `badge-pending` | Gold bg + text | Company pending approval |
| `badge-active` | Green bg + text | Active company/user |
| `badge-suspended` | Grey bg + text | Suspended account |
| `badge-rejected` | Red bg + text | Rejected application/company |
| `badge-draft` | Grey bg + text | Draft job posting |
| `badge-published` | Green bg + text | Published job |
| `badge-verified` | Green bg + ✓ icon | Verified company badge |
| `badge-ai` | Blue gradient + sparkle icon | AI-generated content |
| `badge-p1` | Blue bg + text | P1 Must Have |
| `badge-p2` | Amber bg + text | P2 Should Have |

### 5.3 Pipeline Stage Colours

| Stage | Background | Text | Border |
|-------|-----------|------|--------|
| Applied | `#F1F5F9` | `#334155` | `#CBD5E1` |
| Screening | `#EFF6FF` | `#1D4ED8` | `#BFDBFE` |
| Shortlisted | `#FEFCE8` | `#A16207` | `#FDE68A` |
| Interview Scheduled | `#F0F9FF` | `#0369A1` | `#BAE6FD` |
| Offer Sent | `#F0FDF4` | `#15803D` | `#BBF7D0` |
| Hired | `#DCFCE7` | `#166534` | `#86EFAC` |
| Rejected | `#FEF2F2` | `#B91C1C` | `#FECACA` |

### 5.4 Form Fields

```
Default:   1px solid #CBD5E1, white background
Focus:     1px solid #2563EB, light blue shadow
Error:     1px solid #B91C1C, red tint background
Disabled:  1px solid #E2E8F0, grey background
```

All inputs: `min-height: 44px` (mobile touch target — WCAG)

### 5.5 Cards

```
Card:        white bg · 1px solid #E2E8F0 · border-radius 12px · box-shadow sm
Card hover:  border-color #CBD5E1 · box-shadow md
Card active: border-color #2563EB
```

### 5.6 KPI / Metric Cards (Employer Dashboard)

```html
<div class="kpi-card">
  <div class="kpi-label">Active Jobs</div>
  <div class="kpi-value">12</div>
  <div class="kpi-delta positive">↑ 3 this week</div>
</div>
```

### 5.7 AI Output Wrapper (BR-003)

Every AI-generated content block must use this wrapper — never omit it:

```html
<div class="ai-output-wrapper">
  <div class="ai-badge">
    ✦ AI Generated · Review before saving
  </div>
  <div class="ai-content-body">
    <!-- AI content here -->
  </div>
  <div class="ai-session-counter">
    Session: 1 / 5 AI requests used
  </div>
  <div class="ai-actions">
    <button class="btn btn-primary">✓ Confirm & Use</button>
    <button class="btn btn-secondary">↺ Regenerate (1 retry left)</button>
    <button class="btn btn-ghost">✎ Edit Manually</button>
  </div>
</div>
```

### 5.8 Consent Block (BR-011)

```html
<div class="consent-block" role="group" aria-labelledby="consent-heading">
  <p id="consent-heading" class="consent-title">Đồng ý xử lý dữ liệu</p>
  <label class="consent-label">
    <input type="checkbox" id="data-consent" required
           aria-required="true" aria-describedby="consent-error">
    <span>
      Tôi đồng ý để <strong>{{company_name}}</strong> lưu trữ và xử lý
      thông tin cá nhân của tôi cho mục đích tuyển dụng vị trí này,
      theo <a href="/privacy">Chính sách Bảo vệ Dữ liệu Cá nhân</a>
      (Nghị định 13/2023/NĐ-CP).
    </span>
  </label>
  <p id="consent-error" class="field-error" role="alert" hidden>
    Bạn phải đồng ý với điều khoản để tiếp tục nộp đơn.
  </p>
</div>
```

---

## 6. Layout System

### 6.1 Breakpoints

```css
/* Mobile first */
/* xs: 0–374px  — small phones */
/* sm: 375px    — iPhone SE, standard mobile */
/* md: 768px    — tablet */
/* lg: 1024px   — small desktop */
/* xl: 1280px   — standard desktop */
/* 2xl: 1536px  — large desktop */
```

### 6.2 Page Layouts

**Public Portal:**
```
[Header — fixed top, 64px]
[Content — max-width 1200px, centred]
[Footer]
```

**Employer Workspace:**
```
[Sidebar — fixed left, 240px, navy]
[Main content — flex-1, grey-100 bg]
  [Top bar — 56px, white, company name + notifications]
  [Page content — padding 24px]
```

**Super Admin:**
```
[Sidebar — fixed left, 200px, admin-bg (#1A0A3A)]
[Main content — flex-1, white]
  [Top bar — 48px]
  [Page content — padding 20px]
```

### 6.3 Grid

```css
.grid-4  { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }
.grid-3  { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
.grid-2  { display: grid; grid-template-columns: repeat(2, 1fr); gap: 16px; }

/* Mobile collapse */
@media (max-width: 767px) {
  .grid-4, .grid-3, .grid-2 { grid-template-columns: 1fr; }
}
```

---

## 7. Icon System

Use **SVG icons** inline. Recommended set: Heroicons (MIT license).

Standard sizes: `16px` (inline), `20px` (buttons), `24px` (nav items), `32px` (feature icons)

Key icons needed:
- `briefcase` — Jobs
- `users` — Pipeline / Team
- `chart-bar` — Dashboard / Analytics
- `bell` — Notifications
- `sparkles` — AI features
- `shield-check` — Verified badge
- `document-text` — Documents / JD
- `check-circle` — Success / Hired
- `x-circle` — Rejected
- `clock` — Pending / Scheduled
- `eye` — View / Visible
- `eye-slash` — Hidden / Invisible (contact gating)

---

## 8. Motion & Animation

Stage 1 uses **CSS transitions only**:

```css
/* Standard transition */
transition: all 0.15s ease;

/* Hover on cards */
.card:hover { transform: translateY(-1px); }

/* Modal appearance */
@keyframes slideUp {
  from { transform: translateY(20px); opacity: 0; }
  to   { transform: translateY(0);    opacity: 1; }
}

/* Skeleton shimmer */
@keyframes shimmer {
  0%, 100% { opacity: 0.5; }
  50%       { opacity: 1;   }
}
```

**Respect prefers-reduced-motion:**
```css
@media (prefers-reduced-motion: reduce) {
  * { transition: none !important; animation: none !important; }
}
```

---

*LV360-DS-001 · v1.0 · March 2026 · tresundios Software*
