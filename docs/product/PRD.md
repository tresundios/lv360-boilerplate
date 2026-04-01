# LàmViệc360 — Product Requirements Document v1.2

> **Document ID:** LV360-PRD-001
> **Version:** 1.2 — Incorporating Client Feedback (Padmanabhan Raman, 29 March 2026)
> **Supersedes:** LV360-PRD-001 v1.1
> **Product:** LàmViệc360 — AI-powered multi-tenant recruitment SaaS platform
> **Market:** Vietnam — Mobile-first, Vietnamese primary, Decree 13/2023 compliant
> **Client:** Zebra Global Pte Ltd, Singapore (Sathish Xavier A / Padmanabhan Raman)
> **Prepared By:** tresundios Software (Navis Michael Bearly J)
> **Architecture:** React 18 + TypeScript · FastAPI · PostgreSQL · Google Gemini · cloudbase.vn
> **Dev UX Portal:** https://dev.lamviec360.com/ux/

---

## v1.2 Change Log

11 feedback items were extracted from Padmanabhan Raman's tracked-change review of PRD v1.1 (received 29 March 2026). All items have been assessed, assigned requirement IDs, and incorporated below. Typos in client text have been corrected throughout ("Canditate" → "Candidate", "releving" → "relieving", "canot" → "cannot", "passwordq" → "password", "langauge" → "language", "empoyee" → "employee", "Milion" → "Million"). Duplicate IDs used by the client have been resolved.

| Ref | Client Feedback Location | Type | Action Taken |
|---|---|---|---|
| FB-001 | Section 2.2 — Candidate Data Privacy row | New requirement | Added JSP-FR-015: candidate name privacy toggle |
| FB-002 | Section 3.1 — AUTH-FR-001 | UX design note | Dark background for auth screens — noted as design constraint, passed to UX team |
| FB-003 | Section 3.2 — JSP-FR-007 | UX design note | Application progress styling enhancement — noted, passed to UX team |
| FB-004 | Section 3.2 — after JSP-FR-012 | New requirement | Added JSP-FR-013: monthly events section on homepage |
| FB-005 | Section 3.2 — after JSP-FR-013 | New requirement | Added JSP-FR-014: employer company slider on landing page (client used duplicate ID JSP-FR-013 — corrected to JSP-FR-014) |
| FB-006 | Section 3.3.3 — after EMP-FR-015 | New requirement | Added EMP-FR-016b: employer opts in to feature brand on homepage (client used conflicting ID EMP-FR-016 — corrected to EMP-FR-016b) |
| FB-007 | Section 3.3.4 — after EMP-FR-025b | New requirement | Added EMP-FR-026b: recruiter document request workflow, view-only |
| FB-008 | Section 3.4 — after SAP-FR-008 | New requirement | Added SAP-FR-009: account lockout after 3 failed login attempts |
| FB-009 | Section 3.5 — NTF-FR-004 | Updated requirement | NTF-FR-004 updated: email templates must support both Vietnamese and English |
| FB-010 | Section 3.5 — after NTF-FR-006 | New requirement | Added NTF-FR-007: user-selectable notification language preference |
| FB-011 | Section 4 NFRs + Section 6 BR-009 | New NFR + BR update | Added NFR-019: VND million format, no decimals · BR-009 updated with format rule |

**New requirements in v1.2:** JSP-FR-013 · JSP-FR-014 · JSP-FR-015 · EMP-FR-016b · EMP-FR-026b · SAP-FR-009 · NTF-FR-007
**Updated requirements in v1.2:** NTF-FR-004 · BR-009
**New NFR in v1.2:** NFR-019
**New business rules in v1.2:** BR-015 · BR-016 · BR-017
**New acceptance criteria in v1.2:** AC-020 · AC-021 · AC-022 · AC-023 · AC-024
**New data model entities in v1.2:** DocumentRequest · PlatformEvent
**New screens required in v1.2:** p06 · js15 · js16 · e20 · sa12

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [Specific Requirements](#3-specific-requirements)
   - [3.1 Authentication & Account Management (AUTH)](#31-authentication--account-management-auth)
   - [3.2 Job Seeker Portal (JSP)](#32-job-seeker-portal-jsp)
   - [3.3 Employer / Company Workspace (EMP)](#33-employer--company-workspace-emp)
   - [3.4 Super Admin Panel (SAP)](#34-super-admin-panel-sap)
   - [3.5 Notifications & Communication (NTF)](#35-notifications--communication-ntf)
4. [Non-Functional Requirements](#4-non-functional-requirements)
5. [Data Model (Core Entities)](#5-data-model-core-entities)
6. [Business Rules](#6-business-rules)
7. [Acceptance Criteria](#7-acceptance-criteria)
8. [Requirement-to-Screen Traceability Matrix](#8-requirement-to-screen-traceability-matrix)

---

## 1. Introduction

### 1.1 Purpose

This PRD v1.2 defines the complete functional requirements for LàmViệc360 Phase 1 MVP, incorporating all feedback from Padmanabhan Raman's review of v1.1 (29 March 2026). Sections 1–7 carry forward all content from v1.1 with the specific changes noted in the change log. Section 8 is updated to include the five new screens required by v1.2 requirements.

This document is intended for: Client stakeholders (Sathish & Padu) for review and formal sign-off · Solution Architect and Backend Lead (Navis) as updated system design inputs · Development team as the implementation reference · QA Team (Josephine) and 3rd Party HR SME for test case derivation.

### 1.2 Scope

LàmViệc360 is an AI-enhanced recruitment ecosystem serving three distinct user groups: Job Seekers, Employers/HR Teams, and Platform Administrators. The platform operates as a multi-tenant SaaS where each registered company receives an isolated workspace. Phase 1 delivers the complete public job portal, employer workspace, candidate pipeline, AI tooling, Super Admin panel, and compliance controls — all free for Phase 1 users.

| | |
|---|---|
| **IN SCOPE — Phase 1** | Public job portal (search, apply, track, promotions, advertisement banners, monthly events section, employer slider). Company registration and onboarding. Employer workspace (job posting, Kanban pipeline with dropdown stage movement, team management, document request workflow, homepage brand feature). AI JD generation (Gemini). Rule-based recommendations. Bilingual (VI/EN) notifications with user language preference. Candidate name privacy toggle. Data consent and retention (Decree 13/2023). Super Admin panel (tenant approval, moderation, analytics, account lockout management). |
| **OUT OF SCOPE — Phase 2** | Full AI recommendations. Drag-and-drop Kanban. Company subdomains. Custom theming. Line Manager approval. In-app messaging. Subscription billing. |
| **OUT OF SCOPE — Phase 3+** | HRMS/payroll integration. Skill assessment. White-label. Custom domain. Native apps. |
| **Permanently Out of Scope** | Legal employment certifications. Salary negotiation. Acting as recruitment agency. Direct financial transactions. |

### 1.3 Definitions and Abbreviations

| Term | Definition |
|---|---|
| LàmViệc360 | The product — AI-powered recruitment platform ('WorkRound360' in English) |
| SaaS | Software as a Service |
| Multi-Tenant | Architecture where one platform serves multiple organisations with data isolation |
| RBAC | Role-Based Access Control |
| JD | Job Description |
| 2FA / OTP | Two-Factor Authentication / One-Time Password |
| JWT | JSON Web Token |
| VND | Vietnamese Dong — all salary and pricing in million format, no decimals |
| Decree 13/2023 | Vietnam's Personal Data Protection Decree |
| WCAG | Web Content Accessibility Guidelines |
| Privacy Toggle | *(New v1.2)* Candidate setting to show or hide their real name to recruiters |

---

## 2. Overall Description

### 2.1 Product Perspective

LàmViệc360 is a new, independently developed, cloud-hosted SaaS platform. Five architectural layers:

- **Client Layer** — React 18 + TypeScript + Tailwind CSS (PWA, mobile-first)
- **API Layer** — FastAPI (Python), REST + OpenAPI, JWT + RBAC at API level
- **AI Layer** — Google Gemini API for JD drafting, screening, interview prep — mandatory human review gate
- **Data Layer** — PostgreSQL, min. 4 vCPU / 16GB RAM / 100GB SSD on cloudbase.vn
- **External Services** — SendGrid (bilingual VI/EN email), Google OAuth, Zalo OAuth

### 2.2 Product Functions Summary

| Capability Group | Description | Phase |
|---|---|---|
| Public Job Portal | Job search, job detail, employer directory, registration. Promotions, advertisement banners, monthly events section, employer company slider. | Phase 1 |
| Company Onboarding & Workspace | Registration, workspace setup, team management. Employer homepage brand opt-in. Freemium auto-confirmed Phase 1. | Phase 1 |
| Job Management | AI-assisted JD drafting (Gemini), publishing, editing, lifecycle management. | Phase 1 |
| Candidate Pipeline | Kanban-column layout with dropdown stage movement. Document request workflow (view-only). Drag-and-drop Phase 2. | Phase 1 |
| AI JD Generation | Gemini drafts JD. Mandatory human review. AI badge. Max 2 retries. Session caching. | Phase 1 |
| AI Candidate Screening | Gemini ranks candidates. HR confirms before stage changes. Transparency badge + override mandatory. | Phase 1 (P2 priority) |
| AI Interview Prep | AI-generated practice questions from JD for job seekers. | Phase 1 (P2 priority) |
| Rule-Based Recommendations | Same category/location recommendations. Full ML Phase 2. | Phase 1 |
| Notifications & Communication | Email + in-app. Bilingual VI/EN templates. User-selectable notification language. HR suppression controls. | Phase 1 |
| Candidate Data Privacy | Consent checkbox. Name privacy toggle (hide real name from recruiters). Data retention. Decree 13/2023. | Phase 1 |
| Super Admin Panel | Tenant approval, moderation, analytics, data retention, account lockout management. | Phase 1 |
| Drag-and-Drop Kanban | Full drag-and-drop pipeline. | Phase 2 |
| In-App Messaging | Real-time messaging between employer and candidate. | Phase 2 |
| Company Subdomains | Per-company subdomain. | Phase 2–3 |
| Native Mobile App | Android / iOS. | Post-MVP |

### 2.3 User Roles and Characteristics

| User Class | Description | Persona | Tech Level | Frequency |
|---|---|---|---|---|
| Super Admin | Platform operator — approvals, moderation, analytics, account lockout management | — | High | Low–Medium |
| Company Admin | HR Director, CEO, business owner (age 30–50). Full workspace control. | 'Haanh' | Comfortable | High |
| HR / Recruiter | Internal staff managing 6–10 roles (age 25–45). Jobs, pipeline, document requests. | 'Minh' | Mobile-first | Very High |
| Job Seeker | Vietnamese job market participant (age 22–35). Mobile-first. Controls name privacy. | 'Thuy' | Mobile-first | Medium–High |
| Line Manager | [Phase 2 only] | — | Phase 2 | — |

### 2.4 Operating Environment

| Attribute | Detail |
|---|---|
| Platform Type | Web-based SaaS — Progressive Web App / Responsive Web |
| Primary Platform | Mobile-first — iOS Safari, Android Chrome (latest 2 versions) |
| Secondary Platform | Desktop — Chrome, Firefox, Edge, Safari (latest 2 versions) |
| Backend | FastAPI (Python) — REST + OpenAPI 3.0 |
| Frontend | React 18 + TypeScript + Tailwind CSS |
| Database | PostgreSQL — min. 4 vCPU / 16GB RAM / 100GB SSD on cloudbase.vn |
| AI Integration | Google Gemini API |
| Email Service | SendGrid — bilingual Vietnamese and English templates |
| Language | Vietnamese (primary) · English (secondary, user-switchable) |
| Currency | Vietnamese Dong (VND) — million format, no decimal places |

### 2.5 Design and Implementation Constraints

- Platform must scale to 25,000+ tenant companies and 1,000,000+ job seekers within 3 years.
- Kanban drag-and-drop is NOT in Phase 1 — dropdown stage movement only.
- All monetary values displayed in VND million format with no decimals: format `999,999,999,999` (comma thousands separator, zero decimal places). *(Updated FB-011)*
- AI-generated content requires explicit human confirmation before publishing. Never auto-publish.
- Platform must comply with Decree 13/2023 — consent checkbox and configurable data retention.
- WCAG 2.1 Level AA for all core user flows.
- Account lockout enforced after 3 consecutive failed login attempts. Super Admin reviews and issues reset link. *(New FB-008)*
- *(Design note FB-002):* Auth screens (login, register, OTP, forgot password) should use a dark navy/black background. UX team to apply.

---

## 3. Specific Requirements

Requirement IDs: `[Module]-FR-[###]`. Items marked ✦ are new or updated in v1.2.
Priority: **P1** Must Have | **P2** Should Have | **P3** Could Have

### 3.1 Authentication & Account Management (AUTH)

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| AUTH-FR-001 | Multi-step registration wizard: Step 1 Account Type (Company/Individual), Step 2 Profile details including phone number, Step 3 Email/SMS OTP verification, Step 4 First-login onboarding wizard. *(Design note FB-002: auth screen backgrounds to use dark navy/black theme — UX team action.)* | P1 | R-02, R-07 |
| AUTH-FR-002 | Email/password authentication. Google social login. Zalo social login for all user types. | P1 | R-06 |
| AUTH-FR-003 | OTP verification for all roles at account creation. Company Admin enforces 2FA on every workspace login. | P1 | R-02, R-12 |
| AUTH-FR-004 | Forgot Password: email verification link → secure reset form with real-time password strength indicator. | P1 | R-07 |
| AUTH-FR-005 | Role assignment at registration: Super Admin, Company Admin, HR/Recruiter, Job Seeker. Role determines navigation, features, and onboarding path. | P1 | R-02, R-04 |
| AUTH-FR-006 | Secure JWT sessions with configurable expiry. Sessions invalidated on logout, password change, or AI abuse threshold breach. | P1 | R-03 |
| AUTH-FR-007 | Company Admin invites team members by email. Activation link valid for 72 hours, links invitee to company workspace. | P1 | R-02 |
| AUTH-FR-008 | Role-differentiated first-login onboarding: Company Admin — profile + plan + team invite; Job Seeker — guided profile completion. | P1 | R-06, R-07 |
| AUTH-FR-009 | Secure password storage. Simplified OTP login for subsequent sessions. High-security roles (Company Admin) use OTP at each login. | P1 | R-02 |

### 3.2 Job Seeker Portal (JSP)

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| JSP-FR-001 | Public job search without login. Search by keyword, title, location, industry, salary range (VND), job type. | P1 | R-01, R-05 |
| JSP-FR-002 | Job cards: Title, Company Name, Logo, Location, Salary (VND), Date Posted, Job Type tag. Sortable by date and relevance. | P1 | R-05, R-06 |
| JSP-FR-003 | Job Detail Page: full JD, qualifications, company description, deadline, salary range, Apply Now button (login required). | P1 | R-01 |
| JSP-FR-004 ✦ | Job Seeker profile: personal details, work experience, education, skills, languages, desired salary (VND), resume/document upload. Max 10MB. Formats: PDF, DOC, DOCX, JPG, PNG. | P1 | R-05, R-06 |
| JSP-FR-005 ✦ | One-click apply using saved profile. Optional cover letter. Pre-populated form. Mandatory consent checkbox before submission. | P1 | R-05, R-06 |
| JSP-FR-006 | Save listings to personal Saved Jobs list. | P2 | R-06 |
| JSP-FR-007 | Application Tracker: all applications with status — Applied, Under Review, Shortlisted, Interview Scheduled, Offer Sent, Hired, Rejected. *(Design note FB-003: progress indicator styling to be visually enhanced — UX team action.)* | P1 | R-01, R-05 |
| JSP-FR-007b ✦ | Application Tracker optionally shows estimated stage duration indicator per stage, sourced from company pipeline configuration. | P2 | R-09 |
| JSP-FR-008 | Email/push notifications: application confirmed, CV viewed, status change, interview invitation, offer received. | P1 | R-01, R-06 |
| JSP-FR-009 | AI Interview Preparation: AI-generated practice questions from JD for a selected application. | P2 | R-06 |
| JSP-FR-010 ✦ | Rule-based job recommendations on Job Seeker Dashboard and Job Detail page — same category/location. Full ML Phase 2. | P1 | R-10 |
| JSP-FR-011 | Guided profile completion prompts for missing required fields post-login. | P1 | R-06 |
| JSP-FR-012 | Candidate Document Archive: encrypted storage for employment contracts, relieving letters, payslips, certificates. Accessible to employer from Shortlisted stage. | P1 | R-06 |
| JSP-FR-013 ✦ | **[NEW — FB-004]** Monthly Events Section: The public homepage displays a monthly events section. Events include seminars, webinars, exhibitions, and other employment-related events for both employers and employees. Visitors can browse and click through to event details without logging in. | P2 | FB-004 |
| JSP-FR-014 ✦ | **[NEW — FB-005]** Employer Company Slider: The public landing page displays a horizontal scrollable carousel of verified employer companies, each showing company logo, name, and number of current open job postings. Clicking a card navigates to that company's listings. | P2 | FB-005 |
| JSP-FR-015 ✦ | **[NEW — FB-001]** Candidate Name Privacy Toggle: Job seekers can set a privacy preference in their profile to control whether their real name is visible to recruiters. When enabled, the candidate's name is replaced with an anonymised label (e.g. "Candidate #A1047") in all employer-facing pipeline views. Company Admin can configure the minimum pipeline stage at which the candidate's real name is automatically revealed. Candidate can change this setting at any time. | P1 | FB-001 |

### 3.3 Employer / Company Workspace (EMP)

#### 3.3.1 Company Onboarding

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| EMP-FR-001 | Company Registration form: Company Name, Business Registration Number, Industry, Size, Website URL, Description, Logo upload, Admin user details. | P1 | R-02 |
| EMP-FR-002 | Post email verification, company placed in Pending Approval until Super Admin approves. Admin notified on approval or rejection. | P1 | R-02 |
| EMP-FR-003 ✦ | Post-approval onboarding wizard: profile completion, Freemium plan confirmation, first team member invitation. | P1 | R-02 |
| EMP-FR-004 | Progress indicator throughout wizard. Target: onboarding under 5 minutes on mobile. | P1 | R-02 |

#### 3.3.2 Company Dashboard

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| EMP-FR-005 ✦ | Dashboard KPIs: Active Job Postings, Job Quota (999 in Phase 1), Total Applications, Interviews Scheduled This Week, Positions Filled This Month. Advertisement banner slot displayed. | P1 | R-02 |
| EMP-FR-006 | Quick-action bar: Post New Job, View Applications, Manage Team. | P1 | R-02, R-06 |
| EMP-FR-007 | Sidebar navigation: Overview, Job Management, Candidate Pipeline, Team Management, Settings. | P1 | R-02 |
| EMP-FR-008 | Verified Company badge after Super Admin manual verification. | P1 | R-02 |

#### 3.3.3 Job Management

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| EMP-FR-009 ✦ | Create Job form: Title, Department, Type, Location, Salary Range (VND, negotiable option), JD (rich text), Required Skills, Experience, Education, Vacancies, Deadline, Selection Criteria. | P1 | R-02, R-06 |
| EMP-FR-010 ✦ | AI Generate JD button: Gemini API returns draft with AI indicator. User reviews and confirms before saving. Session-cached. Max 2 retries. | P1 | R-02, R-06 |
| EMP-FR-011 | Save job as Draft. Published jobs appear on public portal. | P1 | R-02 |
| EMP-FR-012 | Edit, close, pause, or duplicate job postings. Closed job removed from portal but applications retained. | P1 | R-01 |
| EMP-FR-013 | Job posting target: under 2 minutes using AI-assisted form. | P1 | R-02 |
| EMP-FR-014 | Social sharing links (LinkedIn, Facebook) on published job detail pages. | P2 | R-03 |
| EMP-FR-015 | JD completion progress indicator (25% / 50% / 75% / 100%). | P2 | R-03 |
| EMP-FR-016b ✦ | **[NEW — FB-006]** Homepage Brand Feature Opt-In: Employer can select in their company profile settings to feature their company brand and latest open job postings on the LàmViệc360 public homepage. The feature displays the company logo, name, Verified badge (if applicable), and up to 3 current open positions. Controlled by Company Admin, toggled on/off at any time. | P2 | FB-006 |

> **HR Expert Note:** Phase 1 delivers Kanban-style visual column layout with dropdown-based stage movement. Drag-and-drop deferred to Phase 2. Confirmed by UX Team.

#### 3.3.4 Candidate Pipeline Management

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| EMP-FR-016 ✦ | Kanban pipeline with visual columns per stage. Default stages: Applied, Screening, Shortlisted, Interview Scheduled, Offer Sent, Hired, Rejected. Company Admin can add, rename, reorder stages. | P1 | R-02, R-09 |
| EMP-FR-017 ✦ | Phase 1: Visual columns + dropdown stage movement only. No drag-and-drop. Mobile: list/card with stage filter. | P1 | R-10 |
| EMP-FR-018 | Candidate card shows: Name or anonymised label (respecting privacy toggle — JSP-FR-015), Stage, Experience, Resume link, Interview Feedback, Source, AI Match Score. | P1 | R-05, R-06 |
| EMP-FR-019 ✦ | Stage dropdown to move candidates. Each transition logged: timestamp, actor user ID, optional note. Audit trail immutable. Stage IDs preserved on rename. | P1 | R-02, R-09 |
| EMP-FR-019b ✦ | Bulk undo within 5 minutes. Double-confirmation required when bulk moving to Rejected. | P1 | R-09 |
| EMP-FR-020 ✦ | Bulk stage movement with confirmation. Undo within 5 minutes. Double-confirm for Rejected. | P1 | R-02, R-09 |
| EMP-FR-021 | Full candidate profile viewable within pipeline — slide-out panel or modal. | P1 | R-06 |
| EMP-FR-022 ✦ | Internal candidate notes: visible only to Company Admin and HR. Edits logged in audit trail. | P1 | R-06, R-09 |
| EMP-FR-023 | AI Screen Candidates: Gemini ranked list with match scores. HR must confirm before any stage changes. | P1 | R-02, R-06 |
| EMP-FR-023b ✦ | AI Screening Safeguards: transparency badge, HR override, no auto-reject, bias disclaimer before running. | P1 | R-09 |
| EMP-FR-024 | Filter/sort pipeline by: Stage, Application Date, AI Match Score, Experience Level, Education Level. | P1 | R-06 |
| EMP-FR-025 ✦ | Email notifications to individual or bulk candidates from pipeline. HR can suppress per stage movement. | P1 | R-02, R-09 |
| EMP-FR-025b ✦ | Structured rejection template library. Optional delayed send. | P1 | R-09 |
| EMP-FR-026b ✦ | **[NEW — FB-007]** Recruiter Document Request: HR/Recruiter can formally request that a candidate shares specific documents from their Document Archive (JSP-FR-012). Request specifies document type: payslips (last 3 months or last 6 months), relieving letter, previous employment contract. Candidate receives a notification and can approve or decline. If approved, the recruiter can view the documents within the platform but **cannot download** them. All document view events are logged in the audit trail (BR-005). | P1 | FB-007 |

#### 3.3.5 Team Management (RBAC)

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| EMP-FR-026 | Company Admin invites employees by email. Role assigned: Admin, HR/Recruiter, or Viewer. | P1 | R-02 |
| EMP-FR-027 | Role enforcement: Admin — all features; HR/Recruiter — post jobs, manage pipeline, communicate; Viewer — read-only. | P1 | R-02 |
| EMP-FR-028 | Modify or revoke access at any time. Revoked users lose access immediately. | P1 | R-02 |
| EMP-FR-029 | Team Management screen: role, join date, last login, access status (Active/Inactive). | P1 | R-02 |

### 3.4 Super Admin Panel (SAP)

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| SAP-FR-001 | Global Company List: status (Pending/Active/Suspended), registration date, plan, active job count. | P1 | R-02, R-04 |
| SAP-FR-002 | Approve, reject (with reason), enable, or suspend any company. | P1 | R-02, R-04 |
| SAP-FR-003 | Tenant Approval Panel: Pending companies with submitted details for review. | P1 | R-02, R-03 |
| SAP-FR-004 ✦ | Platform Analytics: companies, job seekers, active jobs, applications, growth trends. | P1 | R-02, R-04 |
| SAP-FR-005 | Content moderation: flagged listings, remove inappropriate content. | P1 | R-04 |
| SAP-FR-006 | User management: search, view, disable, suspend any user. Includes Locked status filter. | P1 | R-04 |
| SAP-FR-007 ✦ | Data Retention Management: configure retention period, trigger automated deletion. | P1 | R-09 |
| SAP-FR-008 | Advertisement banner slot management: create, schedule, activate, deactivate. | P1 | R-02 |
| SAP-FR-009 ✦ | **[NEW — FB-008]** Account Lockout Management: Any user (candidate or recruiter) who fails login authentication 3 or more consecutive times is automatically locked and cannot attempt further logins. The system notifies the Super Admin. The Super Admin reviews the case and sends a secure password reset link to restore access. Locked accounts visible in User Management with a "Locked" status filter. | P1 | FB-008, R-02 |

### 3.5 Notifications & Communication (NTF)

> **HR Expert Note:** Blanket automated notifications can cause email fatigue. HR-configurable suppression controls are available. HR can suppress candidate notification for any individual stage movement at the time of the action.

| Req ID | Requirement Description | Priority | Source |
|---|---|---|---|
| NTF-FR-001 ✦ | Automated email to job seekers: registration confirmation, application acknowledgement, status changes, interview invitation, offer, rejection. Rejection emails can be delayed by HR. | P1 | R-01, R-09 |
| NTF-FR-002 | Automated email to HR/Recruiters: new application, application marked urgent, interview scheduled. | P1 | R-06 |
| NTF-FR-003 | In-app notification bell with unread badge count. Panel: type icon, message, timestamp, action link. | P1 | R-06 |
| NTF-FR-004 ✦ | **[UPDATED — FB-009]** All email templates use parameterised tokens (candidate name, job title, company name, date). Templates must be maintained in **both Vietnamese and English**. Default outbound language is Vietnamese. English version sent when recipient's notification language preference is set to English (see NTF-FR-007). | P1 | R-01, FB-009 |
| NTF-FR-005 | Job seekers can manage notification preferences (on/off per event type) from profile settings. | P2 | R-06 |
| NTF-FR-006 ✦ | HR can suppress candidate notification for individual stage movements at time of action. | P1 | R-09 |
| NTF-FR-007 ✦ | **[NEW — FB-010]** User Notification Language Preference: Both candidates and recruiters can select their preferred language for all system communications and notifications — Vietnamese (default) or English. This preference is set in account settings and applied to all outbound emails, in-app notifications, and system messages. Platform defaults to Vietnamese for all users unless explicitly changed. | P1 | FB-010, R-09 |

---

## 4. Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|---|---|---|---|
| NFR-001 | Performance | Page load — public job search | < 3 seconds on 4G |
| NFR-002 | Performance | Job posting form submission | < 2 seconds |
| NFR-003 | Performance | AI JD generation (Gemini) | < 10 seconds with loading indicator. Max 2 retries. |
| NFR-003b | Performance | AI JD draft session caching | Cached in session; not lost on timeout or refresh. |
| NFR-004 | Scalability | Concurrent users at go-live | Min. 1,000 concurrent users |
| NFR-005 | Scalability | Company / tenant capacity | 1,000+ companies without architecture change |
| NFR-006 | Availability | Uptime SLA | 99.5% (Production) |
| NFR-007 | Security | Data encryption | TLS 1.2+ in transit · AES-256 at rest |
| NFR-008 | Security | Authentication | JWT + OTP/2FA for all roles; Company Admin 2FA on workspace access |
| NFR-009 | Security | Role enforcement | RBAC enforced at API level — not client-side only |
| NFR-010 | Usability | Company onboarding | < 5 minutes end-to-end on mobile |
| NFR-011 | Usability | Job posting | < 2 minutes with AI assistance |
| NFR-012 | Usability | Task completion rate | ≥ 90% in usability testing |
| NFR-013 | Accessibility | Web accessibility | WCAG 2.1 Level AA — all core flows |
| NFR-014 | Compatibility | Browser support | Chrome, Firefox, Edge, Safari — latest 2 versions |
| NFR-015 | Compatibility | Mobile OS | iOS 15+ (Safari) · Android 10+ (Chrome) |
| NFR-016 | Maintainability | Backend test coverage | ≥ 70% Phase 1 · ≥ 80% by go-live |
| NFR-017 | Localisation | Language support | Vietnamese (primary) · English (secondary, user-switchable via NTF-FR-007) |
| NFR-018 | Compliance | Data privacy | Vietnam Decree 13/2023 — consent checkbox + configurable data retention with auto-delete |
| NFR-019 ✦ | **[NEW — FB-011]** Localisation | VND number format | All monetary values displayed in Vietnamese million format: comma as thousands separator, zero decimal places. Format: `999,999,999,999`. Example: `25,000,000` VND not `25000000` or `25.000.000,00`. Applies to all screens, email templates, and exported reports. |

---

## 5. Data Model (Core Entities)

All monetary values stored in VND as integers (no decimals). New fields added in v1.2 marked ✦.

| Entity | Key Attributes |
|---|---|
| User Account | User ID, Email, Password (hashed), Role, Phone, Account Type, Status, Created Date, Last Login, Notification Language Preference (vi/en) ✦, Account Locked (bool) ✦, Failed Login Count ✦ |
| Company Profile | Company ID, Name, Business Reg No, Industry, Size, Logo URL, Description, Website, Verification Status, Plan, Admin User ID, Homepage Feature Opt-In (bool) ✦ |
| Job Posting | Job ID, Company ID, Title, Department, Type, Location, Salary Range (VND integer), Description, Skills[], Deadline, Vacancies, Status, AI Flag, Posted Date |
| Candidate Application | Application ID, Job ID, Candidate User ID, Applied Date, Current Stage, Cover Letter, Documents[], AI Match Score, Source, Consent Flag, Consent Timestamp, Name Privacy Enabled (bool) ✦ |
| Candidate Profile | Profile ID, User ID, Full Name, Phone, Location, Work Experience[], Education[], Skills[], Desired Salary (VND integer), Resume URL, Completeness %, Name Privacy Toggle (bool) ✦ |
| Pipeline Stage History | History ID, Application ID, From Stage, To Stage, Changed By, Timestamp, Notes, Stage ID (immutable) |
| Audit Log | Log ID, Entity Type (Application/Note/Download/DocumentView/DocumentRequest ✦), Entity ID, Action, Actor ID, Timestamp |
| Candidate Note | Note ID, Application ID, Note Text, Created By, Created Date, Last Edited By, Last Edited Timestamp |
| Notification | Notification ID, User ID, Type, Message, Read Status, Timestamp, Action URL, Suppressed Flag, Scheduled Send Time, Language (vi/en) ✦ |
| Team Member | Member ID, Company ID, User ID, Role, Invited By, Joined Date, Status |
| Data Retention Policy | Policy ID, Company ID, Retention Months, Auto-Delete Enabled, Last Run Timestamp |
| Rejection Template | Template ID, Company ID, Title, Body Text Vietnamese, Body Text English ✦, Is Default |
| Document Request ✦ | Request ID, Application ID, Recruiter User ID, Candidate User ID, Document Types Requested[], Request Date, Status (Pending/Approved/Declined), Response Date |
| Platform Event ✦ | Event ID, Title, Description, Event Type (seminar/webinar/exhibition/other), Event Date, External URL, Created By, Active (bool) |

---

## 6. Business Rules

| Rule ID | Business Rule | Status |
|---|---|---|
| BR-001 | Company must have Super Admin approval before workspace becomes active and jobs are publicly visible. | Unchanged |
| BR-002 | Phase 1 is 100% free. Job quota displayed as 999. No payment gateway. | Phase 1 |
| BR-003 | AI-generated content requires explicit human confirmation before saving or publishing. Never auto-publish. | Unchanged |
| BR-004 | Closed job retains all existing applications but accepts no new ones. | Unchanged |
| BR-005 | Pipeline stage history is immutable. Document view and request events are also logged and immutable. | Updated v1.2 |
| BR-006 | Internal candidate notes visible only to Company Admin and HR/Recruiter. Edits logged in audit trail. | Unchanged |
| BR-007 | Team invitation link expires after 72 hours. Company Admin must re-send after expiry. | Unchanged |
| BR-008 | Verified Company badge granted exclusively by Super Admin after manual verification. | Unchanged |
| BR-009 ✦ | **[UPDATED — FB-011]** Salary stored as VND integers (no decimals). All salary and monetary values displayed in Vietnamese million format: comma as thousands separator, zero decimal places (e.g. `25,000,000` VND). If marked Negotiable, exact range hidden from public listing. | Updated v1.2 |
| BR-010 | All dropdown list items configurable via Super Admin panel — not hardcoded. | Unchanged |
| BR-011 | Consent checkbox mandatory on application form. Cannot submit without explicit consent. Consent flag + timestamp stored. | Unchanged |
| BR-012 | Candidate personal data subject to configured retention policy. Auto-delete after inactivity period. Audit logs exempt. | Unchanged |
| BR-013 | AI screening must not be sole basis for rejection. HR must confirm all stage changes. Bias disclaimer required before running. | Unchanged |
| BR-014 | Candidate contact details visibility configurable by Company Admin. Default: visible from Screening stage onward. | Unchanged |
| BR-015 ✦ | **[NEW — FB-008]** Account Lockout: Any user account failing login authentication 3 or more consecutive times is automatically locked. No further login attempts permitted until Super Admin issues a secure password reset link. Failed login count resets to zero on successful authentication. | New v1.2 |
| BR-016 ✦ | **[NEW — FB-007]** Document Request View-Only: Recruiter granted document access via EMP-FR-026b may view documents within the platform only. Download is not permitted under any circumstance. All document view events are logged in the audit trail. | New v1.2 |
| BR-017 ✦ | **[NEW — FB-001]** Candidate Name Privacy: When a candidate enables the privacy toggle (JSP-FR-015), their real name is replaced with an anonymised identifier in all employer-facing views. Company Admin configures the minimum pipeline stage at which the real name is revealed, regardless of the candidate's toggle setting. | New v1.2 |

---

## 7. Acceptance Criteria

| AC ID | Feature | Acceptance Criteria | Priority |
|---|---|---|---|
| AC-001 | Company Onboarding | New company completes registration, email verification, and profile setup within 5 minutes on mobile. | P1 |
| AC-002 | Job Posting | HR creates and publishes a job using AI JD within 2 minutes. | P1 |
| AC-003 | Job Search | Job seeker searches with ≥ 3 simultaneous filters and receives results within 3 seconds. | P1 |
| AC-004 | Application Submission | Logged-in job seeker applies using saved profile in under 60 seconds. Consent checkbox presented and checked. | P1 |
| AC-005 | Application Tracking | Job seeker sees correct status for all submitted applications in Application Tracker. | P1 |
| AC-006 | Pipeline Management | HR filters, views, and moves a candidate stage in under 3 clicks with confirmation. | P1 |
| AC-007 | AI JD Generation | AI returns draft in < 10 seconds. Max 2 retries. Session-cached. Requires explicit confirmation. | P1 |
| AC-008 | Notifications | Job seeker receives email within 5 minutes of status update (unless suppressed by HR). | P1 |
| AC-009 | RBAC Enforcement | Viewer cannot access job creation, pipeline management, or settings via direct URL. | P1 |
| AC-010 | Admin Approval Gate | Pending company cannot post jobs or appear in public search until Super Admin approves. | P1 |
| AC-011 | Mobile Responsiveness | Core flows fully usable on 375px-wide screen without horizontal scrolling. | P1 |
| AC-012 | Verified Badge | Verified badge visible only after Super Admin approval. | P1 |
| AC-013 | Contact Visibility | HR sees candidate contact details from Screening stage onward by default. Admin can reconfigure. | P1 |
| AC-014 | Bulk Undo | HR can undo bulk stage change within 5 minutes. Double-confirmation modal appears for bulk Rejected. | P1 |
| AC-015 | AI Screening Safeguards | AI badge shown. HR override present. No auto-reject without explicit HR confirmation. | P1 |
| AC-016 | Data Consent | Application cannot submit without consent checkbox checked. Flag + timestamp stored. | P1 |
| AC-017 | Rejection Templates | HR selects from template library. Optional delayed send available. | P1 |
| AC-018 | Job Recommendations | Rule-based similar jobs shown on dashboard and detail page. No ML in Phase 1. | P1 |
| AC-019 | AI Session Limit | After 5 AI calls: user warned. After 3 abuse events: session terminated. | P1 |
| AC-020 ✦ | Account Lockout | After 3 consecutive failed logins the account is locked. Super Admin notified. SA sends password reset link. Locked status visible in User Management. *(FB-008)* | P1 |
| AC-021 ✦ | Candidate Name Privacy | When privacy toggle is enabled, real name not visible to recruiter in pipeline — anonymised label shown. Candidate can toggle at any time. Company Admin can configure reveal stage. *(FB-001)* | P1 |
| AC-022 ✦ | Document Request View-Only | Recruiter can request specific documents. Candidate approves or declines. Approved: recruiter views in-platform only, cannot download. All view events logged. *(FB-007)* | P1 |
| AC-023 ✦ | VND Number Format | All monetary values display in comma-separated million format with zero decimal places throughout all screens, emails, and reports. *(FB-011)* | P1 |
| AC-024 ✦ | Notification Language | User selects VI or EN in account settings. All subsequent emails and notifications delivered in selected language. Default Vietnamese. *(FB-010)* | P1 |

---

## 8. Requirement-to-Screen Traceability Matrix

> **Dev UX Portal:** https://dev.lamviec360.com/ux/
> Screen IDs `js02` and `js08` are intentionally skipped.
> All 54 screens from v1.1 are unchanged. The 5 new screens below must be built as part of v1.2 implementation.

### 8.1 New Screens Required by v1.2 Requirements

| Screen ID | Screen Name | Req IDs | Module | Status |
|---|---|---|---|---|
| p06 | Monthly Events | JSP-FR-013 | Public | To be built |
| js15 | Privacy Settings | JSP-FR-015, BR-017 | Job Seeker | To be built |
| js16 | Document Request — Candidate Response | EMP-FR-026b, BR-016 | Job Seeker | To be built |
| e20 | Document Request — Recruiter View | EMP-FR-026b, BR-005, BR-016 | Employer | To be built |
| sa12 | Account Lockout Management | SAP-FR-009, BR-015 | Admin | To be built |

### 8.2 Existing Screens Updated by v1.2 Requirements

| Screen ID | Screen Name | New Requirements Added | Dev Portal URL |
|---|---|---|---|
| p01 | Homepage | JSP-FR-013, JSP-FR-014, EMP-FR-016b | [p01-homepage](https://dev.lamviec360.com/ux/public/p01-homepage.html) |
| e19 | Company Profile | EMP-FR-016b | [e19-company-profile](https://dev.lamviec360.com/ux/employer/e19-company-profile.html) |
| e06 | Pipeline Kanban | EMP-FR-018 (privacy label in candidate card) | [e06-pipeline-kanban](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) |
| js07 | Profile Editor | JSP-FR-015, NTF-FR-007 | [js07-profile](https://dev.lamviec360.com/ux/job-seeker/js07-profile.html) |
| js13 | Account Settings | NTF-FR-007, BR-011, BR-012 | [js13-settings](https://dev.lamviec360.com/ux/job-seeker/js13-settings.html) |
| sa04 | User Management | SAP-FR-009, BR-015 (Locked status filter) | [sa04-users](https://dev.lamviec360.com/ux/admin/sa04-users.html) |
| sa11 | System Settings | NFR-019, BR-009 (VND format config) | [sa11-settings](https://dev.lamviec360.com/ux/admin/sa11-settings.html) |
| e18 | Account Settings (Employer) | NTF-FR-007 | [e18-account-settings](https://dev.lamviec360.com/ux/employer/e18-account-settings.html) |

---

*LV360-PRD-001 · v1.2 · March 2026 · © tresundios Software · Zebra Global Pte Ltd · Confidential*
