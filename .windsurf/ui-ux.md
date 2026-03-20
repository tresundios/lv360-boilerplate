# UI/UX Rules — LàmViệc360
## Screen-by-Screen UX Guidance for Stage 1

---

## 1. Core UX Principles

Derived from UX research (25 HR professionals, Vietnamese market context):

| Principle | How it manifests in UI |
|-----------|----------------------|
| **Flexibility over rigidity** | Pipeline stages must be customisable. Dropdowns not fixed lists. |
| **Clarity reduces errors** | Every action has a visible confirmation. Destructive actions need double-confirm. |
| **Speed improves productivity** | Onboarding < 5 min. Job post < 2 min. Max 3 clicks to move a candidate. |
| **Visibility improves decisions** | Candidate cards show all critical info without opening profile. KPI widgets above the fold. |
| **Control builds trust** | AI output always needs human confirmation (BR-003). Never auto-publish. |
| **Mobile-first always** | 60–70% of Vietnamese users prefer mobile. Design 375px first. |

---

## 2. Three Visual Layers — UX Differentiation

These three layers must feel like **different products** side by side:

### Layer 1: Public Job Portal
- **Feel:** Consumer app — open, approachable, fast
- **Navigation:** Top bar only. No sidebar.
- **Typography:** Larger, friendlier. Hero search bar prominent.
- **Whitespace:** Generous. Card-based layout.
- **Key screens:** p01, p02, p03, js01–js13

### Layer 2: Employer Workspace
- **Feel:** Professional SaaS dashboard — structured, data-dense
- **Navigation:** 240px left sidebar with navy background
- **Header:** Company name + role badge + notification bell
- **KPI widgets:** Always above the fold on dashboard
- **Key screens:** e01–e19, ow01–ow04

### Layer 3: Super Admin Panel
- **Feel:** Control-plane — analytical, high information density
- **Navigation:** 200px dark sidebar (`#1A0A3A`) with purple accents
- **Always visible:** Pending approvals alert banner
- **Key screens:** sa01–sa11

---

## 3. Screen-by-Screen UX Requirements

### PUBLIC PORTAL

#### p01 — Homepage
- Hero search bar is the #1 element — vertically centred, full-width on mobile
- Featured job cards below (3 on mobile, 6 on desktop)
- "For Employers" CTA section with onboarding benefit highlights
- Advertisement banner slot visible (ADV — top of page, clearly labelled in dev)
- No login required to browse

#### p02 — Job Search
- Filter sidebar: keyword, location (province dropdown), industry, salary range slider (VND), job type
- Job cards: title, company + logo, location, salary, date posted, job type badge
- "Similar Jobs" recommendation strip at bottom (JSP-FR-010, BR-018)
- Mobile: filter panel opens as bottom sheet
- Sort: Date posted (default), Relevance

#### p03 — Job Detail
- Full JD above the fold on desktop, scrollable on mobile
- Sticky "Apply Now" button on mobile (floats at bottom)
- Company card with Verified badge if approved (BR-008)
- Salary in VND, negotiable = "Thương lượng" (BR-009)
- "Similar Jobs" section (JSP-FR-010)
- "Apply Now" requires login — redirect to a01-login if not authenticated

---

### AUTH FLOWS

#### a01 — Login
- Email + password primary method
- Google button (prominent)
- Zalo button (prominent — Vietnamese users prioritise this)
- "Forgot password?" link
- Redirect after login based on role

#### a02 — Register: Role Select
- Two large cards: "I'm an Employer / HR" + "I'm a Job Seeker"
- Step indicator: Step 1 of 4
- Employer card notes: "Company workspace will be set up"

#### a03 — Register: Profile
- Full name, email, phone (+84 format), password + confirm
- Password strength meter (real-time)
- Phone field: required, noted as used for OTP

#### a04 — OTP Verify
- 6-box OTP input, large touch targets
- Countdown timer: "Expires in 09:42"
- "Resend OTP" (max 3 attempts visible)
- Confirm which phone/email received the OTP

#### a05 — Forgot Password
- Two states on same page: (1) email entry (2) success confirmation
- Reset form: new password + confirm + strength meter

---

### ONBOARDING WIZARD

#### ow01 — Company Info
- Progress stepper: Step 1 / 3
- Fields: Company Name*, Business Reg No*, Industry*, Size*, Province*, Website, Description*, Logo upload
- Live character count on Description (50–1000 chars)

#### ow02 — Plan Confirm
- Phase 1: Freemium auto-confirmed — show "Your plan: Free (Phase 1 — No limits)"
- Job quota: 999 displayed
- No payment fields

#### ow03 — Team Invite
- Email + Role dropdown (HR/Recruiter, Viewer)
- Pending invites list with "72h remaining" countdown (BR-007)
- "Skip for now" option

#### ow04 — Pending Approval
- Full-screen friendly waiting state
- "Your company is under review" message
- Estimated time: "Usually within 1 business day"
- What happens next: clear step list
- Lock icon — workspace locked until approved (BR-001)

---

### EMPLOYER WORKSPACE

#### e01 — Dashboard
- 4 KPI cards: Active Jobs, Applications, Interviews This Week, Positions Filled
- Job quota shown as 999 (BR-002)
- Recent applications activity feed
- Quick actions: Post Job, View Pipeline, Manage Team
- Advertisement banner (Slot C — clearly labelled)

#### e03 — Create Job
- Multi-section form with progress indicator (25/50/75/100% — EMP-FR-015)
- "AI Generate JD" prominent button — calls mock AI, shows AI output wrapper (BR-003)
- Required fields starred
- Save as Draft / Publish toggle

#### e05 — AI JD Generator
- Input: Job Title + Skills tags
- Loading state: spinner + "Generating your JD..."
- Output: full JD text with "AI Generated" badge (BR-003)
- Session counter: "1 of 5 AI requests used this session"
- Actions: Confirm & Insert / Regenerate (max 2 retries shown) / Edit Manually
- Confirmed draft preserved in session (NFR-003b)

#### e06 — Pipeline Kanban
- Visual columns for each stage (Applied, Screening, Shortlisted, Interview Scheduled, Offer Sent, Hired, Rejected)
- Each candidate card: name, experience, AI score badge, stage dropdown
- Stage move dropdown fires confirmation modal
- Bulk select checkboxes → bulk action bar appears
- Filter bar above: stage, date, AI score, experience level (EMP-FR-024)
- Mobile: list view with stage filter tabs

#### e07 — Candidate Detail
- Slide-out panel (desktop) / full page (mobile)
- Tabs: Overview, Resume, Notes, Documents, History
- Documents tab: locked if below minimum stage (BR-014)
- Contact details: shown/hidden based on stage threshold (BR-014)
- "Move Stage" dropdown + confirmation
- "Add Note" inline

#### e09 — Reject & Notify
- Template selector dropdown (EMP-FR-025b)
- Preview of rejection email
- "Suppress notification" toggle (NTF-FR-006)
- "Schedule send" option (delayed rejection)
- Double-confirm modal for bulk rejection (EMP-FR-019b)

---

### SUPER ADMIN

#### sa01 — Dashboard
- Platform-wide KPIs: Total Companies, Users, Active Jobs, Pending Approvals
- Pending approvals alert banner (yellow, always visible if count > 0)
- Recent platform activity feed
- System health indicators

#### sa02 — Company List
- Filter pills: All / Pending / Active / Suspended
- Per-row actions: Approve / Reject / Suspend
- Pending badge count shown in nav

#### sa03 — Company Detail
- Full company profile view
- Approval actions with reason field (required for rejection)
- Company's job listings preview
- Team member list

---

## 4. Modals and Overlays

All modals must:
- Have a visible close button (×) and Escape key listener
- Dim the background with `rgba(0,0,0,0.5)` overlay
- Be centred vertically on desktop, bottom-sheet on mobile
- Show action buttons: primary action left, Cancel right
- Destructive actions use red primary button

Confirm dialogs for destructive actions (bulk reject, suspend company):
```
"Are you sure you want to reject 8 candidates?
This cannot be undone within 5 minutes.
[Cancel]  [Confirm Rejection]"
```

---

## 5. Empty States

Every list/table must have a designed empty state:

```html
<div class="empty-state">
  <img src="assets/empty-pipeline.svg" alt="" aria-hidden="true">
  <h3>No candidates yet</h3>
  <p>Applications for this job will appear here once submitted.</p>
  <a class="btn btn-primary" href="p01-homepage.html">View Job Listing</a>
</div>
```

---

## 6. Loading / Skeleton States

For screens that would fetch data, show skeleton loaders:

```html
<div class="skeleton-card">
  <div class="skeleton skeleton-title"></div>
  <div class="skeleton skeleton-text"></div>
  <div class="skeleton skeleton-text short"></div>
</div>
```

```css
.skeleton { background: #E2E8F0; border-radius: 4px; animation: shimmer 1.5s infinite; }
@keyframes shimmer { 0%,100%{opacity:.5} 50%{opacity:1} }
```

---

*LV360-UIUX-001 · v1.0 · March 2026 · tresundios Software*
