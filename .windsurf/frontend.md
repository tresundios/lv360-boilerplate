# Frontend Standards — LàmViệc360
## Stage 1: HTML Prototype Rules

---

## 1. Stage 1 Tech Stack

Stage 1 is **vanilla HTML + CSS + JavaScript** — no build tools, no frameworks.  
Every file must run by opening directly in a browser (file:// protocol).

```
Stage 1 (current)   → Vanilla HTML · CSS Variables · Vanilla JS
Stage 2 (next)      → React 18 · TypeScript · Tailwind CSS · Vite
Stage 3 (production)→ Same + FastAPI backend · PostgreSQL · Gemini API
```

---

## 2. CSS Architecture

### 2.1 CSS Variables (copy into every screen)

All screens must import the design tokens as CSS variables in their `<style>` block:

```css
:root {
  /* Brand */
  --color-primary:       #2563EB;
  --color-primary-dark:  #1D4ED8;
  --color-primary-light: #EFF6FF;

  /* Employer workspace (navy) */
  --color-navy:          #0F2044;
  --color-navy-mid:      #1A4A8A;
  --color-navy-light:    #DBEAFE;

  /* Super Admin (deep purple-navy) */
  --color-admin-bg:      #1A0A3A;
  --color-admin-accent:  #7C3AED;
  --color-admin-light:   #EDE9FE;

  /* Semantic */
  --color-success:       #15803D;
  --color-success-bg:    #DCFCE7;
  --color-warning:       #C8922A;
  --color-warning-bg:    #FEF9C3;
  --color-danger:        #B91C1C;
  --color-danger-bg:     #FEF2F2;
  --color-info:          #0284C7;
  --color-info-bg:       #E0F2FE;

  /* Neutrals */
  --color-grey-900:      #0F172A;
  --color-grey-700:      #334155;
  --color-grey-500:      #64748B;
  --color-grey-300:      #CBD5E1;
  --color-grey-100:      #F1F5F9;
  --color-white:         #FFFFFF;

  /* Typography */
  --font-sans:  'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --font-mono:  'Fira Code', 'Cascadia Code', monospace;
  --text-xs:    11px;
  --text-sm:    13px;
  --text-base:  15px;
  --text-lg:    17px;
  --text-xl:    20px;
  --text-2xl:   24px;
  --text-3xl:   30px;

  /* Spacing (8pt grid) */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-12: 48px;
  --space-16: 64px;

  /* Border radius */
  --radius-sm:  4px;
  --radius-md:  8px;
  --radius-lg:  12px;
  --radius-xl:  16px;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px -1px rgba(0,0,0,0.07), 0 2px 4px -2px rgba(0,0,0,0.05);
  --shadow-lg: 0 10px 15px -3px rgba(0,0,0,0.08), 0 4px 6px -4px rgba(0,0,0,0.05);

  /* Border */
  --border-default: 1px solid #E2E8F0;
  --border-strong:  1px solid #CBD5E1;
}
```

### 2.2 Layer Visual Identity

Each platform layer has a distinct visual identity:

| Layer | Nav style | Primary bg | Accent | Tone |
|-------|-----------|-----------|--------|------|
| Public portal | Top bar | White | `--color-primary` | Consumer, open |
| Employer workspace | Left sidebar 240px | `--color-navy` | `--color-primary` | Professional, dashboard |
| Super Admin | Left sidebar 200px | `--color-admin-bg` | `--color-admin-accent` | Control-plane, analytical |

---

## 3. Component Patterns

### 3.1 Button System

```html
<!-- Primary -->
<button class="btn btn-primary">Post New Job</button>

<!-- Secondary -->
<button class="btn btn-secondary">Save Draft</button>

<!-- Danger -->
<button class="btn btn-danger">Reject</button>

<!-- Ghost -->
<button class="btn btn-ghost">Cancel</button>

<!-- Icon + label -->
<button class="btn btn-primary btn-icon">
  <svg>...</svg> Generate JD
</button>
```

```css
.btn {
  display: inline-flex; align-items: center; gap: var(--space-2);
  padding: 8px 16px; border-radius: var(--radius-md);
  font-size: var(--text-sm); font-weight: 500; cursor: pointer;
  border: 1px solid transparent; transition: all 0.15s;
  min-height: 36px;
}
.btn-primary  { background: var(--color-primary); color: white; }
.btn-primary:hover { background: var(--color-primary-dark); }
.btn-secondary { background: white; color: var(--color-grey-700); border-color: var(--color-grey-300); }
.btn-danger   { background: var(--color-danger); color: white; }
.btn-ghost    { background: transparent; color: var(--color-grey-500); }
```

### 3.2 Badge / Status System

```html
<span class="badge badge-pending">Pending</span>
<span class="badge badge-active">Active</span>
<span class="badge badge-rejected">Rejected</span>
<span class="badge badge-ai">AI Generated</span>
<span class="badge badge-verified">✓ Verified</span>
```

### 3.3 Form Fields

```html
<div class="field-group">
  <label class="field-label" for="job-title">Job Title *</label>
  <input class="field-input" id="job-title" type="text"
         placeholder="e.g. Senior Backend Engineer">
  <span class="field-hint">Will appear exactly as entered on the public listing.</span>
</div>
```

### 3.4 AI Components

Always wrap AI-generated content with the transparency badge:

```html
<div class="ai-output-wrapper">
  <div class="ai-badge">
    <svg><!-- sparkle icon --></svg>
    AI Generated — Review before saving
  </div>
  <div class="ai-content">
    <!-- AI output here -->
  </div>
  <div class="ai-actions">
    <button class="btn btn-primary">Confirm & Save</button>
    <button class="btn btn-ghost">Regenerate</button>
    <button class="btn btn-ghost">Edit Manually</button>
  </div>
</div>
```

### 3.5 Consent Checkbox (BR-011)

Every application form must include:

```html
<div class="consent-block">
  <label class="consent-label">
    <input type="checkbox" id="consent" required>
    <span>
      Tôi đồng ý để <strong>[Company Name]</strong> xử lý dữ liệu cá nhân
      của tôi cho mục đích tuyển dụng này, theo
      <a href="#">Chính sách Bảo vệ Dữ liệu</a>.
      (Decree 13/2023)
    </span>
  </label>
  <p class="consent-note">Required. Application cannot be submitted without consent.</p>
</div>
```

---

## 4. Navigation Patterns

### 4.1 Public Portal — Top Bar

```html
<header class="portal-header">
  <div class="header-inner">
    <a class="logo" href="p01-homepage.html">LàmViệc<span>360</span></a>
    <nav class="header-nav">
      <a href="p02-job-search.html">Find Jobs</a>
      <a href="#">For Employers</a>
    </nav>
    <div class="header-actions">
      <a class="btn btn-ghost" href="a01-login.html">Sign In</a>
      <a class="btn btn-primary" href="a02-register-role.html">Sign Up</a>
    </div>
  </div>
</header>
```

### 4.2 Employer Workspace — Left Sidebar

```html
<aside class="sidebar">
  <div class="sidebar-logo">LàmViệc<span>360</span></div>
  <div class="sidebar-company">TechCorp Vietnam JSC</div>
  <nav class="sidebar-nav">
    <a class="nav-item active" href="e01-dashboard.html">
      <svg><!-- icon --></svg> Dashboard
    </a>
    <a class="nav-item" href="e02-job-list.html">
      <svg><!-- icon --></svg> Job Management
    </a>
    <!-- ... -->
  </nav>
</aside>
```

### 4.3 Dev Screen Navigator (required on every screen)

```html
<!-- DEV NAVIGATOR — remove in production -->
<div id="dev-nav" style="position:fixed;bottom:16px;right:16px;z-index:9999;
  background:#0F2044;border-radius:8px;padding:12px;font-size:11px;color:white;
  max-height:300px;overflow-y:auto;min-width:180px;">
  <div style="font-weight:bold;margin-bottom:8px;color:#93C5FD;">
    📍 Current: [SCREEN-ID]
  </div>
  <div style="font-weight:bold;margin-bottom:4px;opacity:.7;">Next screens:</div>
  <a href="[next-screen].html" style="display:block;color:#5B9BD5;padding:3px 0;">→ [Next Screen Name]</a>
</div>
```

---

## 5. Kanban Pipeline Pattern (EMP-FR-016/017)

Phase 1 Kanban: **visual columns + dropdown movement** (no drag-and-drop):

```html
<div class="pipeline-board">
  <div class="pipeline-column">
    <div class="column-header">
      <span class="column-title">Applied</span>
      <span class="column-count">12</span>
    </div>
    <div class="column-cards">
      <div class="candidate-card">
        <div class="card-name">Nguyễn Thị Thu</div>
        <div class="card-exp">3 years exp</div>
        <div class="card-score ai-badge-sm">AI 82%</div>
        <select class="stage-move-dropdown" onchange="moveCandidate(this)">
          <option value="">Move to stage...</option>
          <option value="screening">Screening</option>
          <option value="shortlisted">Shortlisted</option>
          <!-- ... -->
        </select>
      </div>
    </div>
  </div>
  <!-- more columns -->
</div>
```

---

## 6. Mock Data Standards

All mock data must be realistic and Vietnamese-contextualised:

```javascript
const MOCK_JOBS = [
  {
    id: 'JOB-001',
    title: 'Senior Backend Engineer',
    company: 'TechCorp Vietnam JSC',
    location: 'Hà Nội',
    salary: '25.000.000 – 40.000.000 VND/tháng',
    type: 'Full-time',
    posted: '2 ngày trước',
    verified: true,
    stage: 'published'
  }
];

const MOCK_CANDIDATES = [
  {
    id: 'CAND-001',
    name: 'Nguyễn Thị Thu',
    email: 'thu@gmail.com',
    phone: '+84 912 345 678',
    stage: 'shortlisted',
    aiScore: 82,
    experience: '3 năm',
    appliedDate: '10 Tháng 3, 2026'
  }
];
```

---

*LV360-FRONTEND-001 · v1.0 · March 2026 · tresundios Software*
