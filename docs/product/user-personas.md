# User Personas — LàmViệc360
## Phase 1 MVP · Derived from PRD v2.0 + UX Research

---

## Persona 1 — Nguyễn Thị Haanh · Company Admin

> *"I need to onboard my company, post jobs, and see the full hiring picture — all from my phone while commuting."*

| Attribute | Detail |
|-----------|--------|
| **Role** | HR Director / Company Admin |
| **Age** | 40 years old |
| **Location** | Đà Nẵng |
| **Tech level** | Comfortable with SaaS tools |
| **Device** | Desktop (work) + Mobile (check notifications) |
| **Company** | Mid-sized tech firm, 150 employees |
| **PRD reference** | User Class: Company Admin · EMP-FR-001–029 |

### Goals
- Set up the company workspace quickly (< 5 minutes — AC-001)
- Have full visibility of all open roles and candidate pipelines at a glance
- Manage team members and assign appropriate roles (RBAC)
- Ensure company data is protected (Decree 13/2023 compliance)
- Get the "Verified Company" badge to build candidate trust (BR-008)

### Pain Points (from research)
- Current tools have rigid workflows — can't customise hiring stages per role
- Poor data visibility — scattered spreadsheets and email threads
- No AI assistance for job descriptions — writes everything from scratch
- Difficult to onboard new HR staff onto existing tools

### Behaviours
- Checks dashboard KPIs every morning
- Manages 3–5 active job postings simultaneously
- Uses desktop for pipeline management, mobile for notifications
- Values data-driven insights over gut feeling

### Key Screens
`ow01` → `ow02` → `ow03` → `e01` → `e02` → `e14` → `e18` → `e19`

---

## Persona 2 — Trần Minh Đức · HR / Recruiter

> *"I'm juggling 8 open roles at once. I need to move candidates through stages fast and draft job descriptions in minutes, not hours."*

| Attribute | Detail |
|-----------|--------|
| **Role** | Hiring Manager / HR Recruiter |
| **Age** | 28 years old |
| **Location** | TP. Hồ Chí Minh |
| **Tech level** | Mobile-first, quick learner |
| **Device** | Mobile (primary) + Desktop |
| **Company** | E-commerce startup, 50 employees |
| **PRD reference** | User Class: HR/Recruiter · EMP-FR-009–025b |

### Goals
- Post a job in under 2 minutes using AI assistance (AC-002)
- Quickly scan candidate cards without opening every profile
- Move multiple candidates between stages in bulk
- Send rejection emails with pre-defined templates
- Avoid accidental data loss (session caching — NFR-003b)

### Pain Points (from research)
- Spends 30–60 min writing each JD from scratch
- Interview scheduling is a manual back-and-forth nightmare
- Candidates go "in limbo" — no status visibility for applicants
- Bulk rejection without a structured reason feels unprofessional

### Behaviours
- Manages 6–10 roles simultaneously (research: 44% of HR users)
- Needs candidate name, current stage, experience at a glance (research: 100%)
- Open to AI suggestions but always wants manual review (research: 68%)
- Uses mobile to review pipeline between meetings

### Key Screens
`e02` → `e03` → `e05` → `e06` → `e07` → `e09` → `e10` → `e13`

---

## Persona 3 — Lê Thị Thuy · Job Seeker

> *"I apply from my phone on the bus. I hate filling the same form 20 times and never hearing back."*

| Attribute | Detail |
|-----------|--------|
| **Role** | Fresh Graduate / Junior Developer |
| **Age** | 24 years old |
| **Location** | TP. Hồ Chí Minh |
| **Tech level** | Mobile-native, Zalo power user |
| **Device** | Android smartphone (primary) |
| **PRD reference** | User Class: Job Seeker · JSP-FR-001–012 |

### Goals
- Search and apply for jobs in under 60 seconds (AC-004)
- Track application status without having to email each company
- Upload documents securely without worrying about data leaks (JSP-FR-012)
- Get AI-generated interview prep questions for shortlisted roles (JSP-FR-009)
- Know when an employer views her documents (BR-014 transparency)

### Pain Points (from research)
- "Apply and forget" — never hears back from most applications
- Fills the same personal info on every platform (repetitive forms)
- Can't tell if her CV is even being read
- Worried about sensitive documents (payslips, contracts) being shared without consent

### Behaviours
- Uses Zalo login exclusively — never creates new passwords
- Checks phone 5–10 times a day during job search period
- Most active job searching during commute (3G/4G connection — NFR-001)
- Applies to 3–5 jobs per week
- Saves interesting jobs to review later

### Key Screens
`p01` → `p02` → `p03` → `a07` → `js01` → `js03` → `js04` → `js07` → `js09`

---

## Persona 4 — Bùi Văn Khoa · Viewer (Read-Only)

> *"I just need to see where the hiring stands for my department. I don't need to do anything — just check the pipeline."*

| Attribute | Detail |
|-----------|--------|
| **Role** | Department Head / Line Manager (Viewer role) |
| **Age** | 45 years old |
| **Location** | Hà Nội |
| **Tech level** | Basic — prefers simple interfaces |
| **Device** | Desktop |
| **PRD reference** | User Class: Viewer · EMP-FR-026–029 |

### Goals
- See the current state of hiring for his department at a glance
- Know which candidates are at interview stage
- No need to take actions — just observe

### Constraints (RBAC — EMP-FR-027)
- Cannot post jobs
- Cannot move candidates between stages
- Cannot access billing or team management
- Cannot see internal HR notes (BR-006)
- Cannot see candidate contact details below minimum stage (BR-014)

### Key Screens
`e01` (read-only view) → `e06` (read-only Kanban)

---

## Persona 5 — Platform Super Admin

> *"I approve companies, manage the platform, and make sure no bad actors abuse the system."*

| Attribute | Detail |
|-----------|--------|
| **Role** | Platform Super Administrator |
| **Age** | — |
| **Tech level** | High — power user |
| **Device** | Desktop |
| **PRD reference** | User Class: Super Admin · SAP-FR-001–008 |

### Goals
- Review and approve / reject company registrations (SAP-FR-002/003)
- Monitor platform-wide analytics (SAP-FR-004)
- Manage advertisement banner slots (SAP-FR-008)
- Configure data retention policies (SAP-FR-007)
- Suspend abusive companies or users (SAP-FR-002/006)

### Key Screens
`sa01` → `sa02` → `sa03` → `sa04` → `sa06` → `sa07` → `sa08` → `sa09` → `sa10` → `sa11`

---

## Persona Quick Reference

| Persona | Role | Primary flow | Key PRD section |
|---------|------|-------------|----------------|
| Haanh | Company Admin | Onboarding → Dashboard → Team | 3.3.1, 3.3.2, 3.3.5 |
| Minh | HR / Recruiter | Post Job → Pipeline → Reject | 3.3.3, 3.3.4 |
| Thuy | Job Seeker | Search → Apply → Track | 3.2 JSP |
| Khoa | Viewer | Dashboard (read-only) | 3.3.5 RBAC |
| Admin | Super Admin | Approve → Monitor → Configure | 3.4 SAP |

---

*LV360-PERSONAS-001 · v1.0 · March 2026 · tresundios Software · Derived from PRD v2.0 + UX Research (25 HR professionals)*
