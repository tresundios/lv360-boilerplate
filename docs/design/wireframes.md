# Wireframes — LàmViệc360
## Layout Specification · Stage 1 HTML Screens

---

## How to Use This Document

For each screen, this document describes:
- **Layout** — structural arrangement
- **Key zones** — what must appear and where
- **PRD refs** — which requirements to implement
- **Connected screens** — where links go

Windsurf should build exactly what is described here, referencing `design-system.md` for visual tokens.

---

## PUBLIC PORTAL SCREENS

---

### p01 — Homepage

**Layout:** Full-width · Top nav · Hero section · Features · Job cards · Footer

```
┌─────────────────────────────────────────┐
│ HEADER: Logo | Find Jobs | For Employers | Sign In | Sign Up │
├─────────────────────────────────────────┤
│ HERO (gradient bg):                     │
│   "Find your next opportunity in Vietnam"│
│   [Search bar: keyword + location + 🔍] │
│   Popular: IT · Finance · Marketing     │
├─────────────────────────────────────────┤
│ AD BANNER SLOT (labelled in dev)        │
├─────────────────────────────────────────┤
│ FEATURED JOBS (6 cards — 3 on mobile)  │
│ [Job Card] [Job Card] [Job Card]        │
│ [Job Card] [Job Card] [Job Card]        │
│ [See all jobs →]                        │
├─────────────────────────────────────────┤
│ FOR EMPLOYERS section                   │
│ "Post jobs, manage pipeline, hire fast" │
│ [Start Free →] (→ a02-register-role)   │
├─────────────────────────────────────────┤
│ FOOTER                                  │
└─────────────────────────────────────────┘
```

**PRD:** JSP-FR-001, JSP-FR-002, BR-008 (verified badge on cards)  
**Links:** Search → p02 · Apply → a01 (if not logged in) · For Employers → a02

---

### p02 — Job Search

**Layout:** Left filter panel (260px) + Right results grid

```
┌──────────────┬──────────────────────────────┐
│ FILTERS      │ RESULTS (n jobs found)       │
│              │ Sort: Date | Relevance ▾     │
│ Keyword      │                              │
│ [___________]│ ┌──────────────────────────┐ │
│              │ │ JOB CARD                  │ │
│ Location ▾   │ │ Title · Company Logo+Name │ │
│              │ │ 📍 Location  💰 Salary VND │ │
│ Industry ▾   │ │ 🏷 Full-time  📅 2 days ago│ │
│              │ │ ✓ Verified         [Save] │ │
│ Salary range │ └──────────────────────────┘ │
│ [slider VND] │ (repeat × n)                 │
│              │                              │
│ Job Type     │ SIMILAR JOBS strip           │
│ ☐ Full-time  │ (JSP-FR-010)                 │
│ ☐ Part-time  │                              │
│ ☐ Remote     └──────────────────────────────┘
│ ☐ Contract   
└──────────────
```

**Mobile:** Filter as bottom sheet triggered by "Filter" button  
**PRD:** JSP-FR-001, JSP-FR-002, JSP-FR-006, JSP-FR-010  
**Links:** Job card → p03 · Save → js06 (if logged in)

---

### p03 — Job Detail

**Layout:** Single column (max 800px centred)

```
┌──────────────────────────────────────┐
│ ← Back to results                    │
│                                      │
│ [Company Logo] TechCorp Vietnam JSC  │
│               ✓ Verified             │
│                                      │
│ Senior Backend Engineer              │
│ 📍 Hà Nội  💰 25–40tr VND  🏷 Full-time│
│ 📅 Deadline: 31 Tháng 3, 2026        │
│                                      │
│ [Apply Now] ← sticky on mobile      │
│                                      │
│ Job Description                      │
│ [full JD text]                       │
│                                      │
│ Requirements                         │
│ [list]                               │
│                                      │
│ About the Company                    │
│ [description]                        │
│                                      │
│ ──── Similar Jobs ────               │
│ [3 job cards] (JSP-FR-010)           │
└──────────────────────────────────────┘
```

**PRD:** JSP-FR-003, JSP-FR-010, BR-008, BR-009  
**Links:** Apply Now → a01-login (if not authenticated) OR js03-apply

---

## AUTH SCREENS

---

### a01 — Login

**Layout:** Centred card (max 400px) on white/light background

```
┌──────────────────────────────┐
│  LàmViệc360                  │
│                              │
│  Welcome back                │
│                              │
│  Email                       │
│  [________________]          │
│  Password                    │
│  [________________] 👁        │
│  [Forgot password?]          │
│                              │
│  [Sign In — full width btn]  │
│                              │
│  ─── or ───                  │
│  [G Continue with Google]    │
│  [Z Continue with Zalo]      │
│                              │
│  Don't have an account?      │
│  [Create account →]          │
└──────────────────────────────┘
```

**PRD:** AUTH-FR-001, AUTH-FR-002, AUTH-FR-003  
**Links:** Forgot password → a05 · Create account → a02 · Google/Zalo → a07

---

### a04 — OTP Verify

**Layout:** Centred card, compact

```
┌──────────────────────────────┐
│  🔒 Verify your identity     │
│                              │
│  Enter the 6-digit code sent │
│  to +84 9xx xxx 678          │
│                              │
│  [_] [_] [_] [_] [_] [_]    │
│  (large touch targets)       │
│                              │
│  Expires in 09:42            │
│                              │
│  [Verify]                    │
│                              │
│  [Resend OTP] (2 remaining)  │
└──────────────────────────────┘
```

**PRD:** AUTH-FR-003  
**Links:** On verify → role-based redirect (employer → ow01, seeker → js01)

---

## ONBOARDING WIZARD

---

### ow01 — Company Info Wizard

**Layout:** Centred form card · Progress stepper at top

```
┌──────────────────────────────────────┐
│ Step 1 of 3 ●──○──○                  │
│ Company Information                  │
├──────────────────────────────────────┤
│ Company Name *                       │
│ [____________________________]       │
│                                      │
│ Business Registration No. *          │
│ [____________________________]       │
│                                      │
│ Industry *    Company Size *         │
│ [▾_________]  [▾_____________]       │
│                                      │
│ Province *    Website                │
│ [▾_________]  [____________________] │
│                                      │
│ Description * (50–1000 chars)        │
│ [________________________]           │
│ [________________________] 120/1000  │
│                                      │
│ Logo Upload                          │
│ [📎 Upload logo (PNG/JPG max 2MB)]   │
│                                      │
│              [Continue →]           │
└──────────────────────────────────────┘
```

**PRD:** EMP-FR-001, EMP-FR-004  
**Links:** Continue → ow02

---

### ow04 — Pending Approval

**Layout:** Full-width centred · Friendly illustration placeholder

```
┌──────────────────────────────────────┐
│                                      │
│        🕐  (clock illustration)      │
│                                      │
│  Your company is under review        │
│                                      │
│  We're verifying TechCorp Vietnam JSC│
│  Business reg: 0101234567            │
│                                      │
│  What happens next:                  │
│  ✓ 1. Our team reviews your details  │
│  ○ 2. You receive an email decision  │
│  ○ 3. Your workspace is activated    │
│                                      │
│  Usually within 1 business day.      │
│                                      │
│  [Check my email instead]            │
│                                      │
│  ⚠ Your workspace is locked until   │
│    approval is granted.              │
└──────────────────────────────────────┘
```

**PRD:** EMP-FR-002, BR-001  

---

## EMPLOYER WORKSPACE SCREENS

---

### e01 — Employer Dashboard

**Layout:** Sidebar (240px navy) + Main content

```
SIDEBAR:
LàmViệc360
TechCorp Vietnam JSC ✓
──────────────────────
📊 Dashboard        ← active
💼 Jobs
👥 Pipeline
👤 Team
⚙  Settings

MAIN CONTENT:
┌─────────────────────────────────────────┐
│ [Top bar: Company | Role | 🔔 Bell | Avatar]│
├─────────────────────────────────────────┤
│ Good morning, Haanh 👋                  │
│                                         │
│ KPI CARDS (4-col grid, 2-col on tablet) │
│ ┌────────┐ ┌────────┐ ┌────────┐ ┌────┐│
│ │Active  │ │Apps    │ │Interviews│ │Hired││
│ │Jobs    │ │Total   │ │This Week │ │Month││
│ │  12    │ │  847   │ │    5     │ │  3  ││
│ │999 max │ │        │ │          │ │     ││
│ └────────┘ └────────┘ └────────┘ └────┘│
│                                         │
│ AD BANNER SLOT (Slot C — ADV-FR-003)   │
│                                         │
│ QUICK ACTIONS                           │
│ [Post New Job] [View Pipeline] [Team]   │
│                                         │
│ RECENT ACTIVITY                         │
│ · Nguyễn Thu applied for Backend Eng.  │
│ · Trần Minh moved to Shortlisted       │
└─────────────────────────────────────────┘
```

**PRD:** EMP-FR-005, EMP-FR-006, EMP-FR-007, EMP-FR-008, BR-002

---

### e06 — Pipeline Kanban

**Layout:** Horizontal scroll columns (min 200px each)

```
JOB: Senior Backend Engineer ▾  [Filters ▾]  [AI Screen] [+ Add Stage]

┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────┐ ...
│ Applied  (12)│ │Screening  (8)│ │Shortlisted(4)│ │Interview │
│              │ │              │ │              │ │   (2)    │
│ ┌──────────┐ │ │ ┌──────────┐ │ │ ┌──────────┐ │ │          │
│ │Ng. Thị   │ │ │ │Tr. Minh  │ │ │ │Lê Thị    │ │ │          │
│ │Thu       │ │ │ │Đức       │ │ │ │Thuy      │ │ │          │
│ │3 yrs exp │ │ │ │5 yrs exp │ │ │ │AI: 91%   │ │ │          │
│ │AI: 82%   │ │ │ │AI: 75%   │ │ │ │          │ │ │          │
│ │[Move ▾]  │ │ │ │[Move ▾]  │ │ │ │[Move ▾]  │ │ │          │
│ └──────────┘ │ │ └──────────┘ │ │ └──────────┘ │ │          │
│ ...          │ │ ...          │ │              │ │          │
└──────────────┘ └──────────────┘ └──────────────┘ └──────────┘
```

**PRD:** EMP-FR-016, EMP-FR-017, EMP-FR-018, EMP-FR-023, EMP-FR-024

---

### e09 — Reject & Notify Modal

**Layout:** Modal overlay · centred · max 520px

```
┌──────────────────────────────────┐
│ Reject & Notify Candidate        │  ×
├──────────────────────────────────┤
│ Nguyễn Thị Thu — Backend Eng.   │
│                                  │
│ Rejection Reason *               │
│ [Select template ▾]              │
│ ○ Role filled internally         │
│ ○ Profile does not match         │
│ ○ Overqualified for this role    │
│ ○ Write custom reason            │
│                                  │
│ Email Preview:                   │
│ ┌──────────────────────────────┐ │
│ │ Subject: Your application... │ │
│ │ Dear Nguyễn Thị Thu,        │ │
│ │ Thank you for applying...   │ │
│ └──────────────────────────────┘ │
│                                  │
│ 🔕 Suppress notification         │
│    [toggle OFF]                  │
│                                  │
│ Schedule send: [Immediately ▾]   │
│                                  │
│       [Cancel] [Send Rejection]  │
└──────────────────────────────────┘
```

**PRD:** EMP-FR-025, EMP-FR-025b, NTF-FR-006, BR-005

---

## SUPER ADMIN SCREENS

---

### sa01 — Admin Dashboard

**Layout:** Dark sidebar (200px, `#1A0A3A`) + Main content

```
SIDEBAR (dark purple-navy):
LàmViệc360 ADMIN
──────────────────
📊 Dashboard     ← active
🏢 Companies
👤 Users
📢 Ads
📋 Templates
🗑 Retention
🔍 Audit Log
📈 Reports
⚙  Settings

MAIN CONTENT:
┌──────────────────────────────────┐
│ Platform Dashboard               │
│                                  │
│ ⚠ 3 companies pending approval  │  ← yellow alert
│   [Review Now →]                │
│                                  │
│ KPIs: Companies | Users | Jobs | │
│       Applications               │
│                                  │
│ Recent Platform Activity         │
│ [audit log feed]                 │
│                                  │
│ System Health                    │
│ API ✓ · DB ✓ · AI ✓ · Email ✓  │
└──────────────────────────────────┘
```

**PRD:** SAP-FR-001, SAP-FR-004, BR-001

---

## SCREEN INVENTORY SUMMARY

| Group | Count | Screen IDs |
|-------|-------|-----------|
| Public | 3 | p01, p02, p03 |
| Auth | 6 | a01, a02, a03, a04, a05, a07 |
| Onboarding | 4 | ow01, ow02, ow03, ow04 |
| Job Seeker | 11 | js01, js03–js07, js09–js13 |
| Employer | 19 | e01–e19 |
| Super Admin | 11 | sa01–sa11 |
| **Total** | **54** | |

---

*LV360-WF-001 · v1.0 · March 2026 · tresundios Software*
