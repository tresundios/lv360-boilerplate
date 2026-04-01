# LàmViệc360
## Product Requirements Document — v1.1

> **Document ID:** LV360-PRD-001
> **Version:** 1.1 — Added Section 8: Requirement-to-Screen Traceability Matrix
> **Supersedes:** LV360-PRD-001 v1.0
> **Product:** LàmViệc360 — AI-powered multi-tenant recruitment SaaS platform
> **Market:** Vietnam — Mobile-first, Vietnamese primary, Decree 13/2023 compliant
> **Client:** Zebra Global Pte Ltd, Singapore (Sathish Xavier A / Padmanabhan Raman)
> **Prepared By:** tresundios Software (Navis Michael Bearly J)
> **Architecture:** React 18 + TypeScript · FastAPI · PostgreSQL · Google Gemini · cloudbase.vn
> **Modules Covered:** AUTH · JSP · EMP · SAP · NTF · Non-Functional Requirements
> **Dev UX Portal:** https://dev.lamviec360.com/ux/
> **v1.1 Change:** Section 8 added — full requirement-to-screen traceability matrix with live dev portal links. No Figma references. Dev portal is the single source of truth for UX review.

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [Specific Requirements](#3-specific-requirements)
   - [3.1 Authentication & Account Management](#31-authentication--account-management-auth)
   - [3.2 Job Seeker Portal](#32-job-seeker-portal-jsp)
   - [3.3 Employer / Company Workspace](#33-employer--company-workspace-emp)
   - [3.4 Super Admin Panel](#34-super-admin-panel-sap)
   - [3.5 Notifications & Communication](#35-notifications--communication-ntf)
4. [Non-Functional Requirements](#4-non-functional-requirements)
5. [Data Model (Core Entities)](#5-data-model-core-entities)
6. [Business Rules](#6-business-rules)
7. [Acceptance Criteria](#7-acceptance-criteria)
8. [Requirement-to-Screen Traceability Matrix](#8-requirement-to-screen-traceability-matrix) ← NEW in v1.1
   - [8.1 Screen Inventory](#81-screen-inventory--54-screens)
   - [8.2 AUTH Requirements Traceability](#82-auth--authentication--account-management)
   - [8.3 JSP Requirements Traceability](#83-jsp--job-seeker-portal)
   - [8.4 EMP Requirements Traceability](#84-emp--employer--company-workspace)
   - [8.5 SAP Requirements Traceability](#85-sap--super-admin-panel)
   - [8.6 NTF Requirements Traceability](#86-ntf--notifications--communication)
   - [8.7 Business Rules Traceability](#87-business-rules-traceability)
   - [8.8 Acceptance Criteria Traceability](#88-acceptance-criteria-traceability)

---

## 1. Introduction

### 1.1 Purpose

This Product Requirements Document (PRD) v1.1 defines the complete functional requirements, user roles, business workflows, data model, business rules, and acceptance criteria for LàmViệc360 Phase 1 MVP.

Version 1.1 adds Section 8 — Requirement-to-Screen Traceability Matrix — in response to client feedback (Padmanabhan Raman, 22 March 2026) requesting point-by-point linkage between PRD requirement IDs and built UX screens. Every requirement ID, business rule, and acceptance criterion is now mapped to its live screen on the development portal at https://dev.lamviec360.com/ux/.

Sections 1–7 are unchanged from v1.0. All 63 requirements, 14 business rules, 18 NFRs, and 19 acceptance criteria from v1.0 remain in force.

**Figma is no longer used as a design reference.** The dev.lamviec360.com/ux/ portal is the single source of truth for all UX screen review going forward.

This document is intended for:
- Client stakeholders (Sathish & Padu) — for review and formal sign-off
- Solution Architect and Backend Lead (Navis) — as updated system design inputs
- Development team — as the implementation reference
- QA Team (Josephine) and 3rd Party HR SME — for test case derivation

### 1.2 Scope

LàmViệc360 is an AI-enhanced recruitment ecosystem serving three distinct user groups: Job Seekers, Employers/HR Teams, and Platform Administrators. The platform operates as a multi-tenant SaaS where each registered company receives an isolated workspace. Phase 1 delivers the complete public job portal, employer workspace, candidate pipeline, AI tooling, Super Admin panel, and compliance controls — all free for Phase 1 users.

| Scope | Items |
| --- | --- |
| **IN SCOPE — Phase 1** | Public job portal (search, apply, track, product promotions, advertisement banners). Company registration and onboarding. Employer workspace (job posting, Kanban-style pipeline with dropdown stage movement, team management). AI JD generation (Gemini API with human review gate). Rule-based job recommendations. Notifications with HR suppression controls. Candidate data consent and retention controls (Decree 13/2023). Super Admin panel (tenant approval, moderation, analytics). |
| **OUT OF SCOPE — Phase 2** | Full AI-powered job recommendations (ML engine). Drag-and-drop Kanban. Company subdomains. Custom company theme/colours. Internal multi-stage approval workflow (Line Manager). In-app messaging. Subscription billing and payment gateway. |
| **OUT OF SCOPE — Phase 3+** | HRMS/payroll integration. Skill assessment module. White-label. Custom domain. Native iOS/Android apps. Salary negotiation engine. |
| **Permanently Out of Scope** | Legal employment certifications. Involvement in salary negotiation or labour contracts. Acting as a recruitment agency. Direct financial transactions between employers and candidates. |

### 1.3 Definitions and Abbreviations

| Term | Definition |
| --- | --- |
| LàmViệc360 | The product — AI-powered recruitment platform ('WorkRound360' in English) |
| SaaS | Software as a Service — cloud-based software delivery model |
| Multi-Tenant | Architecture where a single platform serves multiple client organisations with data isolation |
| Tenant | A registered company operating within its own isolated workspace on the platform |
| RBAC | Role-Based Access Control — permissions governed by assigned user roles |
| JD | Job Description |
| ATS | Applicant Tracking System |
| 2FA / OTP | Two-Factor Authentication / One-Time Password |
| JWT | JSON Web Token — stateless authentication mechanism |
| VND | Vietnamese Dong — local currency for all salary and pricing display |
| AI Transparency | Obligation to disclose when content is AI-generated and provide HR override capability |
| Data Retention Policy | Configurable rule for automated deletion of candidate data after a defined period of inactivity |
| Decree 13/2023 | Vietnam's Personal Data Protection Decree — governs candidate consent and data handling |
| WCAG | Web Content Accessibility Guidelines — target Level AA for core flows |
| BPMN | Business Process Model and Notation — standard diagram notation for process flows |

---

## 2. Overall Description

### 2.1 Product Perspective

LàmViệc360 is a new, independently developed, cloud-hosted SaaS platform. It fills a gap in the Vietnamese recruitment technology market — a modern, AI-enhanced, affordable, and trust-driven hiring ecosystem that is missing from the current landscape. The platform operates across five architectural layers:

- **Client Layer** — React 18 + TypeScript + Tailwind CSS (Progressive Web App, mobile-first)
- **API Layer** — FastAPI (Python), REST + OpenAPI, JWT-based authentication and RBAC enforced at API level
- **AI Layer** — Google Gemini API for JD drafting, candidate screening, and interview prep — with mandatory human review gate
- **Data Layer** — PostgreSQL managed database, minimum 4 vCPU / 16GB RAM / 100GB SSD on cloudbase.vn
- **External Services** — Email (SendGrid), Social sharing links, Google and Zalo OAuth

### 2.2 Product Functions Summary

| Capability Group | Description | Phase |
| --- | --- | --- |
| Public Job Portal | Public-facing job search, job detail, employer directory, candidate registration. Product promotion display and advertisement banners. | Phase 1 |
| Company Onboarding & Workspace | Multi-step registration, workspace setup, team management. Freemium plan auto-confirmed in Phase 1. | Phase 1 |
| Job Management | Job posting with AI-assisted JD drafting (Gemini), publishing, editing, and full lifecycle management. | Phase 1 |
| Candidate Pipeline | Kanban-column visual layout with dropdown-based stage movement. Drag-and-drop deferred to Phase 2. | Phase 1 |
| AI JD Generation | Gemini API generates JD drafts. Mandatory human review. AI badge indicator. Max 2 retries. Session caching of confirmed draft. | Phase 1 |
| AI Candidate Screening | Gemini ranks candidates vs. job requirements. HR must confirm before stage changes. Transparency badge + HR override mandatory. | Phase 1 (P2 priority) |
| AI Interview Prep | AI-generated practice questions from JD for job seekers. | Phase 1 (P2 priority) |
| Rule-Based Recommendations | Same category/location job recommendations on dashboard and job detail page. Full ML engine deferred to Phase 2. | Phase 1 |
| Notifications & Communication | Email + in-app notifications with HR suppression controls and configurable triggers. | Phase 1 |
| Candidate Data Privacy | Consent checkbox on application. Configurable data retention policy. Decree 13/2023 compliant. | Phase 1 |
| Super Admin Panel | Tenant approval, suspension, content moderation, platform analytics, data retention management. | Phase 1 |
| Drag-and-Drop Kanban | Full drag-and-drop pipeline board. | Phase 2 |
| In-App Messaging | Real-time messaging between employer and candidate. | Phase 2 |
| Full AI Recommendations | ML-powered personalised recommendations based on profile, history, and behaviour. | Phase 2 |
| Company Subdomains | Per-company subdomain (companyname.lamviec360.com). | Phase 2–3 |
| HRMS / Payroll Integration | Integration with external HR management systems. | Phase 3 |
| Native Mobile App | Android / iOS native application. | Post-MVP |

### 2.3 User Roles and Characteristics

| User Class | Description | Persona | Tech Level | Frequency |
| --- | --- | --- | --- | --- |
| Super Admin | Platform operator — manages all companies, approvals, moderation, analytics | — | High | Low–Medium |
| Company Admin | HR Director, CEO, or business owner (age 30–50). Sets up and oversees full company workspace. | 'Haanh' — HR Director | Comfortable | High |
| HR / Recruiter | Internal company staff managing 6–10 roles simultaneously (age 25–45). Posts jobs and manages pipeline. | 'Minh' — Hiring Manager | Mobile-first | Very High |
| Job Seeker | Vietnamese job market participant (age 22–35). Mobile-first user searching and applying via smartphone. | 'Thuy' — New Graduate | Mobile-first | Medium–High |
| Line Manager | [Phase 2 only] Internal approver for offer sign-off. Not in Phase 1 scope. | — | Phase 2 | — |

### 2.4 Operating Environment

| Attribute | Detail |
| --- | --- |
| Platform Type | Web-based SaaS — Progressive Web App / Responsive Web |
| Primary Platform | Mobile-first — iOS Safari, Android Chrome (latest 2 versions) |
| Secondary Platform | Desktop — Chrome, Firefox, Edge, Safari (latest 2 versions) |
| Backend | FastAPI (Python) — REST + OpenAPI 3.0 |
| Frontend | React 18 + TypeScript + Tailwind CSS |
| Database | PostgreSQL — Managed, min. 4 vCPU / 16GB RAM / 100GB SSD on cloudbase.vn |
| AI Integration | Google Gemini API |
| Hosting | cloudbase.vn — Production + Staging + Development tiers |
| Authentication | JWT + 2FA (OTP via Email / SMS / Zalo) |
| Email Service | SendGrid (or equivalent) |
| Min. Bandwidth | Optimised for 3G/4G mobile connections (Vietnam market) |
| Language | Vietnamese (primary) · English (secondary, switchable) |
| Currency | Vietnamese Dong (VND) — all salary and pricing display |

### 2.5 Design and Implementation Constraints

- Platform must scale to 25,000+ tenant companies and 1,000,000+ job seekers within 3 years of go-live.
- Company subdomains are NOT in Phase 1 scope — reserved for Phase 2–3.
- Kanban drag-and-drop is NOT in Phase 1. Phase 1 delivers Kanban-style column layout with dropdown-based stage movement only.
- Company-specific primary colour theming and custom banners are NOT in Phase 1. Company logo and description are supported.
- AI-generated content requires explicit human confirmation before publishing. The system must never auto-publish AI output.
- AI screening output must display a transparency badge, allow HR override, and must not be used as the sole basis for rejection.
- Platform must comply with Vietnam's Decree 13/2023 on personal data protection — including candidate consent checkbox and configurable data retention.
- Design must achieve WCAG 2.1 Level AA accessibility compliance for all core user flows.
- All monetary values displayed in Vietnamese Dong (VND). Negotiable salary hides exact range from public listing.
- All API calls to AI services are rate-limited: maximum 5 requests per item per session. Users who exceed 3 AI abuse events in a session have their session terminated.

---

## 3. Specific Requirements

Requirement IDs follow the convention: `[Module]-FR-[###]`. Items marked ✦ are new or updated in v1.0.
Priority: **P1** Must Have | **P2** Should Have | **P3** Could Have

### 3.1 Authentication & Account Management (AUTH)

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| AUTH-FR-001 | Multi-step registration wizard: Step 1 Account Type (Company/Individual), Step 2 Profile details including phone number, Step 3 Email/SMS OTP verification, Step 4 First-login onboarding wizard. | P1 Must Have | R-02, R-07 |
| AUTH-FR-002 | Support email/password authentication. Google social login and Zalo social login for all user types. | P1 Must Have | R-06 |
| AUTH-FR-003 | OTP-based verification for all user roles at account creation and subsequent logins. Company Admin additionally enforces 2FA for workspace access. | P1 Must Have | R-02, R-12 |
| AUTH-FR-004 | Forgot Password flow: email verification link → secure reset form with real-time password strength indicator. | P1 Must Have | R-07 |
| AUTH-FR-005 | Role assignment and enforcement at registration: Super Admin, Company Admin, HR/Recruiter, Job Seeker. Role determines navigation, accessible features, and onboarding path throughout the session. | P1 Must Have | R-02, R-04 |
| AUTH-FR-006 | Secure JWT sessions with configurable expiry. Sessions invalidated on logout, password change, or AI abuse threshold breach. | P1 Must Have | R-03 |
| AUTH-FR-007 | Company Admin invites team members by email. Invitees receive an activation link valid for 72 hours that links them to the company workspace without requiring full company re-registration. | P1 Must Have | R-02 |
| AUTH-FR-008 | Role-differentiated first-login onboarding wizard: Company Admin — company profile setup + subscription plan + team invite; Job Seeker — guided profile completion steps. | P1 Must Have | R-06, R-07 |
| AUTH-FR-009 | System stores password credentials securely. Simplified OTP login for subsequent sessions. Roles requiring high security use OTP via email or SMS at each login. | P1 Must Have | R-02 |

### 3.2 Job Seeker Portal (JSP)

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| JSP-FR-001 | Public-facing job search accessible without login. Search by keyword, job title, location (province/city), industry, salary range (VND), and job type (full-time, part-time, remote, contract). | P1 Must Have | R-01, R-05 |
| JSP-FR-002 | Job search results show job cards: Job Title, Company Name, Logo, Location, Salary Range (VND), Date Posted, Job Type tag. Results sortable by date and relevance. | P1 Must Have | R-05, R-06 |
| JSP-FR-003 | Job Detail Page: full JD, qualifications, company description, deadline, location, salary range, and 'Apply Now' button (login required to apply). | P1 Must Have | R-01 |
| JSP-FR-004 ✦ | Job Seeker profile: personal details, work experience, education, skills, languages, desired salary range (VND), resume/document upload. Accepted formats: PDF, DOC, DOCX, JPG, PNG. Maximum file size: 10MB. | P1 Must Have | R-05, R-06 |
| JSP-FR-005 ✦ | One-click apply using saved profile. Optional cover letter. System pre-populates application form from profile. Mandatory candidate data consent checkbox displayed before final submission. Applications cannot be submitted without explicit consent. | P1 Must Have | R-05, R-06 |
| JSP-FR-006 | Job seekers can save listings to a personal 'Saved Jobs' list for later review. | P2 Should Have | R-06 |
| JSP-FR-007 | Application Tracker view showing all submitted applications with current status: Applied, Under Review, Shortlisted, Interview Scheduled, Offer Sent, Hired, Rejected. | P1 Must Have | R-01, R-05 |
| JSP-FR-007b ✦ | Application Tracker optionally displays an estimated stage duration indicator per stage, based on the hiring company's average processing time. Indicator sourced from company pipeline configuration. | P2 Should Have | R-09 |
| JSP-FR-008 | Push/email notifications on: application confirmed, CV viewed, status change, interview invitation, offer received. | P1 Must Have | R-01, R-06 |
| JSP-FR-009 | AI Interview Preparation: job seeker selects a saved application and receives AI-generated practice interview questions relevant to the job description. (Phase 1 P2 priority) | P2 Should Have | R-06 |
| JSP-FR-010 ✦ | Rule-based job recommendations on Job Seeker Dashboard and Job Detail page ('Similar Jobs') — same category/location as applied/saved/browsed jobs. Full ML recommendations are Phase 2. | P1 Must Have | R-10 |
| JSP-FR-011 | Guided profile completion prompts for any missing required fields post-login. | P1 Must Have | R-06 |
| JSP-FR-012 | Candidate Document Archive: password-protected encrypted storage for previous employment contracts, relieving letters, payslips, and certificates. Documents uploaded by the job seeker, accessible only to the job seeker and (from Shortlisted stage) the employer. | P1 Must Have | R-06 |

### 3.3 Employer / Company Workspace (EMP)

#### 3.3.1 Company Onboarding

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| EMP-FR-001 | Company Registration form: Company Name, Business Registration Number, Industry, Company Size, Website URL, Description, Logo upload, and Admin user details. | P1 Must Have | R-02 |
| EMP-FR-002 | Post email verification, company placed in 'Pending Approval' status until approved by Super Admin. Company Admin receives notification on approval or rejection. | P1 Must Have | R-02 |
| EMP-FR-003 ✦ | Post-approval onboarding wizard guides Company Admin through: profile completion, plan confirmation (Phase 1: Freemium auto-confirmed), and first team member invitation. | P1 Must Have | R-02 |
| EMP-FR-004 | Clear progress indicator throughout onboarding wizard. Target: company onboarding completed in under 5 minutes on a mobile device. | P1 Must Have | R-02 |

#### 3.3.2 Company Dashboard

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| EMP-FR-005 ✦ | Employer Dashboard displays KPIs: Total Active Job Postings, Remaining Job Quota (displayed as 999 in Phase 1 to reflect unlimited), Total Applications Received, Interviews Scheduled This Week, Positions Filled This Month. Advertisement banner slot displayed. | P1 Must Have | R-02 |
| EMP-FR-006 | Quick-action bar on dashboard: 'Post New Job', 'View Applications', 'Manage Team'. | P1 Must Have | R-02, R-06 |
| EMP-FR-007 | Sidebar navigation: Overview, Job Management, Candidate Pipeline, Team Management, Settings. | P1 Must Have | R-02 |
| EMP-FR-008 | 'Verified Company' badge displayed on employer profile and all job listings after Super Admin completes manual verification. | P1 Must Have | R-02 |

#### 3.3.3 Job Management

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| EMP-FR-009 ✦ | Create Job form: Job Title, Department, Job Type (dropdown), Location, Salary Range (VND, negotiable option), Job Description (rich text editor), Required Skills (tag input), Experience (years), Education Level (dropdown), Vacancies, Application Deadline, Selection Criteria (Attitude type, Social type, Personality type, Overtime willingness, Project work requirements). | P1 Must Have | R-02, R-06 |
| EMP-FR-010 ✦ | 'AI Generate JD' button: sends Job Title + Skills to Google Gemini API, returns a draft JD with 'AI Generated' indicator. User must review and confirm before saving. Confirmed draft is session-cached to prevent data loss on timeout. Max 2 retries per session. | P1 Must Have | R-02, R-06 |
| EMP-FR-011 | HR/Recruiter can save a job as 'Draft' before publishing. Published jobs appear on the public portal. Draft jobs visible only within employer workspace. | P1 Must Have | R-02 |
| EMP-FR-012 | HR/Recruiter can edit, close, pause, or duplicate existing job postings. A closed job is removed from the public portal but all application data is retained. | P1 Must Have | R-01 |
| EMP-FR-013 | Job posting completion time target: HR can post a job in under 2 minutes using the AI-assisted form. | P1 Must Have | R-02 |
| EMP-FR-014 | Social sharing links (LinkedIn, Facebook) on published job detail pages. | P2 Should Have | R-03 |
| EMP-FR-015 | JD completion progress indicator (25% / 50% / 75% / 100%) based on field fill rate. | P2 Should Have | R-03 |

> **HR Expert Note:** Phase 1 delivers Kanban-style visual column layout with dropdown-based stage movement. Drag-and-drop interaction is deferred to Phase 2. Confirmed by UX Team.

#### 3.3.4 Candidate Pipeline Management

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| EMP-FR-016 ✦ | Candidate Pipeline displays all applications per job by stage using Kanban-style visual columns. Default stages: Applied, Screening, Shortlisted, Interview Scheduled, Offer Sent, Hired, Rejected. Company Admin can add, rename, or reorder stages. | P1 Must Have | R-02, R-09 |
| EMP-FR-017 ✦ | Phase 1: Kanban-style column layout (visual) with dropdown-based stage movement (no drag-and-drop). Mobile view: list/card view with stage filter. Drag-and-drop deferred to Phase 2. | P1 Must Have | R-10 |
| EMP-FR-018 | Candidate card displays: Name (100%), Current Stage (100%), Experience (88%), Resume Snapshot link (84%), Interview Feedback summary (72%), Source channel (60%), AI Match Score (52%) — based on HR research data. | P1 Must Have | R-05, R-06 |
| EMP-FR-019 ✦ | Stage selector dropdown to move candidates between stages. System logs each transition with timestamp, acting user ID, and note (if added). Audit trail is immutable — stage history cannot be deleted. Stage IDs preserved even if stages are renamed. | P1 Must Have | R-02, R-09 |
| EMP-FR-019b ✦ | Undo mechanism for bulk stage movements: HR can undo a bulk stage change within 5 minutes of execution. Double-confirmation modal required when bulk moving candidates to 'Rejected' stage. | P1 Must Have | R-09 |
| EMP-FR-020 ✦ | Bulk stage movement for selected candidates. Bulk actions require confirmation. Undo within 5 minutes available. Double-confirmation required when bulk moving to Rejected. | P1 Must Have | R-02, R-09 |
| EMP-FR-021 | Full candidate profile (resume, work history, cover letter, documents) viewable within pipeline without navigating away — slide-out panel or modal. | P1 Must Have | R-06 |
| EMP-FR-022 ✦ | HR/Recruiter can add internal notes/feedback to a candidate record. Notes visible only to Company Admin and HR. Note edits logged in audit trail with timestamp and editor ID. | P1 Must Have | R-06, R-09 |
| EMP-FR-023 | 'AI Screen Candidates' action per job: Gemini API analyses all resumes against job requirements, returns ranked list with match scores and rationale. HR must confirm before any stage changes occur. | P1 Must Have | R-02, R-06 |
| EMP-FR-023b ✦ | AI Screening Safeguards: (1) AI Transparency Badge displayed on screening results panel. (2) HR can override any AI ranking decision. (3) System must not auto-reject any candidate based solely on AI score. (4) AI bias disclaimer visible to HR before running screening. | P1 Must Have | R-09 |
| EMP-FR-024 | Filter and sort candidate pipeline by: Stage, Application Date, AI Match Score, Experience Level, Education Level. | P1 Must Have | R-06 |
| EMP-FR-025 ✦ | Send email notifications to individual or bulk candidates from pipeline: Application Received, Shortlisted, Interview Invitation (with date/time/format), Offer Letter, Rejection. HR can suppress individual stage-change notifications at the time of movement. | P1 Must Have | R-02, R-09 |
| EMP-FR-025b ✦ | Structured rejection reason template library: HR selects from pre-defined rejection reasons (e.g. 'Role filled internally', 'Profile does not match requirements'). Optional: HR can schedule a delayed send for rejection emails. | P1 Must Have | R-09 |

#### 3.3.5 Team Management (RBAC)

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| EMP-FR-026 | Company Admin invites internal employees by email. Each invitee is assigned a role: Admin, HR/Recruiter, or Viewer. | P1 Must Have | R-02 |
| EMP-FR-027 | Role enforcement: Admin — all features including settings and team management; HR/Recruiter — post jobs, manage candidates, communicate; Viewer — read-only access to jobs and pipeline. | P1 Must Have | R-02 |
| EMP-FR-028 | Company Admin can modify a team member's role or revoke access at any time. Revoked users lose access immediately. | P1 Must Have | R-02 |
| EMP-FR-029 | Team Management screen shows all members with: assigned role, join date, last login, and access status (Active/Inactive). | P1 Must Have | R-02 |

### 3.4 Super Admin Panel (SAP)

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| SAP-FR-001 | Global Company List: all registered companies with status (Pending/Active/Suspended), registration date, subscription plan, and active job posting count. | P1 Must Have | R-02, R-04 |
| SAP-FR-002 | Super Admin can approve, reject (with reason), enable, or suspend any company account. | P1 Must Have | R-02, R-04 |
| SAP-FR-003 | Tenant Approval Panel: companies in 'Pending Approval' state with submitted details for review. | P1 Must Have | R-02, R-03 |
| SAP-FR-004 ✦ | Platform Analytics Dashboard: total registered companies, job seekers, active job postings, applications, user growth trends. Revenue dashboard for Phase 2. | P1 Must Have | R-02, R-04 |
| SAP-FR-005 | Content moderation: review flagged job listings, remove inappropriate content, mark listings as verified or suspicious. | P1 Must Have | R-04 |
| SAP-FR-006 | User account management: search, view, disable, or suspend any user account. | P1 Must Have | R-04 |
| SAP-FR-007 ✦ | Data Retention Management: configure platform-wide default data retention period. Override per plan tier. Trigger automated deletion workflows for inactive candidate data after configured period. | P1 Must Have | R-09 |
| SAP-FR-008 | Advertisement banner slot management: create, schedule, activate, and deactivate promotional banners displayed on the public portal and employer dashboard. | P1 Must Have | R-02 |

### 3.5 Notifications & Communication (NTF)

> **HR Expert Note:** Blanket automated notifications can cause email fatigue. Version 1.0 introduces HR-configurable notification suppression controls available on all plans. HR can suppress candidate notification for any individual stage movement at the time of the action.

| Req ID | Requirement Description | Priority | Source |
| --- | --- | --- | --- |
| NTF-FR-001 ✦ | Automated email to job seekers on: registration confirmation, application acknowledgement, status changes, interview invitation, offer issued, rejection. Rejection emails can be delayed by HR. | P1 Must Have | R-01, R-09 |
| NTF-FR-002 | Automated email to HR/Recruiters on: new application received, application marked urgent, interview scheduled. | P1 Must Have | R-06 |
| NTF-FR-003 | In-app notification bell with unread badge count. Notification panel shows: type icon, message, timestamp, and action link. | P1 Must Have | R-06 |
| NTF-FR-004 | All email templates use parameterised tokens (candidate name, job title, company name, date). Default language: Vietnamese. | P1 Must Have | R-01 |
| NTF-FR-005 | Job seekers can manage notification preferences (email on/off per event type) from profile settings. | P2 Should Have | R-06 |
| NTF-FR-006 ✦ | HR can suppress candidate notification for individual stage changes at the time of the stage movement (e.g. move to Screening without notifying candidate). | P1 Must Have | R-09 |

---

## 4. Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
| --- | --- | --- | --- |
| NFR-001 | Performance | Page load time — public job search | < 3 seconds on 4G |
| NFR-002 | Performance | Job posting form submission response | < 2 seconds |
| NFR-003 | Performance | AI JD generation response (Gemini API) | < 10 seconds with loading indicator. Max 2 retries per session. |
| NFR-003b | Performance | AI JD draft session caching | Last confirmed AI draft cached in session; not lost on timeout or page refresh. |
| NFR-004 | Scalability | Concurrent users at go-live | Min. 1,000 concurrent users |
| NFR-005 | Scalability | Company / tenant capacity | 1,000+ companies without architecture change |
| NFR-006 | Availability | Platform uptime SLA | 99.5% (Production environment) |
| NFR-007 | Security | Data encryption | TLS 1.2+ in transit · AES-256 at rest |
| NFR-008 | Security | Authentication | JWT + OTP/2FA for all roles; Company Admin 2FA on workspace access |
| NFR-009 | Security | Role enforcement | RBAC enforced at API level — not client-side only |
| NFR-010 | Usability | Company onboarding benchmark | < 5 minutes end-to-end on mobile |
| NFR-011 | Usability | Job posting benchmark | < 2 minutes (standard form with AI assistance) |
| NFR-012 | Usability | Task completion rate (core flows) | ≥ 90% in usability testing |
| NFR-013 | Accessibility | Web accessibility compliance | WCAG 2.1 Level AA — all core user flows |
| NFR-014 | Compatibility | Browser support | Chrome, Firefox, Edge, Safari — latest 2 versions |
| NFR-015 | Compatibility | Mobile OS support | iOS 15+ (Safari) · Android 10+ (Chrome) |
| NFR-016 | Maintainability | Backend unit test coverage | ≥ 70% (Phase 1) · ≥ 80% target by go-live |
| NFR-017 | Localisation | Language support | Vietnamese (primary) · English (secondary, switchable) |
| NFR-018 | Compliance | Data privacy | Vietnam Decree 13/2023 — mandatory candidate consent checkbox + configurable data retention with auto-delete |

---

## 5. Data Model (Core Entities)

The following entities form the Phase 1 data model. All monetary values stored in VND.

| Entity | Key Attributes |
| --- | --- |
| User Account | User ID, Email, Password (hashed), Role, Phone Number, Account Type, Status, Created Date, Last Login |
| Company Profile | Company ID, Name, Business Reg. No., Industry, Size, Logo URL, Description, Website, Verification Status, Plan, Admin User ID |
| Job Posting | Job ID, Company ID, Title, Department, Type, Location, Salary Range, Description, Skills[], Deadline, Vacancies, Status, AI Flag, Posted Date |
| Candidate Application | Application ID, Job ID, Candidate User ID, Applied Date, Current Stage, Cover Letter, Documents[], AI Match Score, Source, Consent Flag, Consent Timestamp |
| Candidate Profile | Profile ID, User ID, Full Name, Phone, Location, Work Experience[], Education[], Skills[], Desired Salary, Resume URL (max 10MB; PDF/DOC/DOCX/JPG/PNG), Completeness % |
| Pipeline Stage History | History ID, Application ID, From Stage, To Stage, Changed By, Timestamp, Notes, Stage ID (immutable — preserved on stage rename) |
| Audit Log | Log ID, Entity Type (Application/Note/Download), Entity ID, Action, Actor ID, Timestamp |
| Candidate Note | Note ID, Application ID, Note Text, Created By, Created Date, Last Edited By, Last Edited Timestamp |
| Notification | Notification ID, User ID, Type, Message, Read Status, Timestamp, Action URL, Suppressed Flag, Scheduled Send Time |
| Team Member | Member ID, Company ID, User ID, Role, Invited By, Joined Date, Status |
| Data Retention Policy | Policy ID, Company ID, Retention Months, Auto-Delete Enabled, Last Run Timestamp |
| Rejection Template | Template ID, Company ID, Title, Body Text, Is Default |

---

## 6. Business Rules

| Rule ID | Business Rule | Status |
| --- | --- | --- |
| BR-001 | A company must have Super Admin approval before its workspace becomes active and job postings become visible to the public. | Unchanged |
| BR-002 | Phase 1 is 100% free for all employers. Job quota displayed as 999 (unlimited). No payment gateway in Phase 1. | Phase 1 |
| BR-003 | AI-generated content (JD drafts, candidate screening results) must be explicitly confirmed by a human user before any data is saved or published. The system must never auto-publish AI output. | Unchanged |
| BR-004 | A candidate application is tied to the specific job posting. If the job is closed, existing applications remain accessible in the pipeline but no new applications are accepted. | Unchanged |
| BR-005 | Pipeline stage history is immutable — logged stage transitions cannot be deleted. Stage IDs are preserved in the audit log even if the stage is renamed by Company Admin. | Updated v1.0 |
| BR-006 | Internal candidate notes are visible only to Company Admin and HR/Recruiter of the same company. Viewers and external users cannot access notes. Note edits are logged in the audit trail. | Updated v1.0 |
| BR-007 | A team member invitation link expires after 72 hours. After expiry, Company Admin must re-send the invitation. | Unchanged |
| BR-008 | The 'Verified Company' badge is granted exclusively by Super Admin after manual verification. Self-registration does not grant the badge. | Unchanged |
| BR-009 | Salary data is stored and displayed in VND. If marked 'Negotiable', the exact range is hidden from the public listing. | Unchanged |
| BR-010 | All dropdown list items (job types, industries, education levels, provinces/cities) are configurable via the Super Admin panel — not hardcoded in the application. | Unchanged |
| BR-011 | Candidate data consent checkbox is mandatory on the application form. Applications cannot be submitted without explicit consent. Consent flag and timestamp are stored with the application record. | New v1.0 |
| BR-012 | Candidate personal data (profile, resume, application data) is subject to the company's configured data retention policy. After the retention period of inactivity, an automated deletion job removes the data. Audit logs are exempt from deletion. | New v1.0 |
| BR-013 | AI candidate screening must not be the sole basis for rejection. HR must review AI screening output and manually confirm any stage changes. AI bias disclaimer must be visible to HR before running screening. | New v1.0 |
| BR-014 | Candidate contact details (email, phone) visibility is configurable by Company Admin: minimum stage required to view contact details. Default: HR/Recruiters can view contact from Screening stage onward. | New v1.0 |

---

## 7. Acceptance Criteria

| AC ID | Feature | Acceptance Criteria | Priority |
| --- | --- | --- | --- |
| AC-001 | Company Onboarding | A new company completes registration, email verification, and basic profile setup within 5 minutes on a mobile device. | P1 |
| AC-002 | Job Posting | HR user creates and publishes a job posting (using AI JD feature) within 2 minutes. | P1 |
| AC-003 | Job Search | Job seeker searches using ≥ 3 simultaneous filters and receives results within 3 seconds. | P1 |
| AC-004 | Application Submission | Logged-in job seeker applies to a job using saved profile (one-click apply) in under 60 seconds. Consent checkbox must be presented and checked before submission. | P1 |
| AC-005 | Application Tracking | Job seeker can view current stage of all submitted applications with correct status in Application Tracker. | P1 |
| AC-006 | Pipeline Management | HR can view all applications for a job, filter by stage, and change a candidate's stage in under 3 clicks with confirmation. | P1 |
| AC-007 | AI JD Generation | AI JD generation returns a relevant draft in < 10 seconds. Max 2 retries allowed. Confirmed draft is session-cached. Requires explicit user confirmation before saving. | P1 |
| AC-008 | Notifications | Job seeker receives email notification within 5 minutes of their application status being updated by HR (unless notification is suppressed by HR). | P1 |
| AC-009 | RBAC Enforcement | A 'Viewer' role team member cannot access job creation, candidate management, or settings — verified by direct URL access attempt. | P1 |
| AC-010 | Admin Approval Gate | A company pending approval cannot post jobs or appear in public job search until Super Admin grants approval. | P1 |
| AC-011 | Mobile Responsiveness | Core flows fully usable on a 375px-wide mobile screen without horizontal scrolling. | P1 |
| AC-012 | Verified Badge | 'Verified Company' badge visible only after Super Admin approval — not before. | P1 |
| AC-013 | Contact Visibility | Default: HR/Recruiters can view candidate contact details from Screening stage onward. Company Admin can reconfigure minimum visibility stage. | P1 |
| AC-014 | Bulk Undo | HR can undo a bulk stage change within 5 minutes. Double-confirmation modal appears when bulk moving to Rejected. Verify undo removes stage changes and restores previous state. | P1 |
| AC-015 | AI Screening Safeguards | AI screening results panel shows AI Transparency Badge. HR override button present. No candidate is moved to Rejected based on AI score alone without explicit HR confirmation. | P1 |
| AC-016 | Data Consent | Application form cannot be submitted without the candidate data consent checkbox being checked. Consent flag + timestamp stored with application record. | P1 |
| AC-017 | Rejection Templates | HR can select a structured rejection reason from a pre-defined template library. Optional delayed send available. | P1 |
| AC-018 | Job Recommendations | Job Seeker Dashboard and Job Detail page show rule-based similar job recommendations (same category/location). No ML recommendations in Phase 1. | P1 |
| AC-019 | AI Session Limit | System tracks AI calls per session. After 5 calls: user is warned. After 3 abuse events in a session: session is terminated. | P1 |

---

## 8. Requirement-to-Screen Traceability Matrix

> **Purpose:** This section maps every PRD requirement ID, business rule, and acceptance criterion to the screen(s) that implement it on the LàmViệc360 development UX portal. Every link below points to a live screen at `https://dev.lamviec360.com/ux/`. Use this section to verify point-by-point that each requirement has a corresponding built screen.
>
> **Dev UX Portal base URL:** https://dev.lamviec360.com/ux/
>
> **Note:** Screen IDs `js02` and `js08` are intentionally skipped — those IDs were reserved during planning and not assigned to any Phase 1 screen.

---

### 8.1 Screen Inventory — 54 Screens

| Screen ID | Screen Name | PRD Requirements | Dev Portal URL | Module |
| --- | --- | --- | --- | --- |
| p01 | Homepage | JSP-FR-001, JSP-FR-002, JSP-FR-010, BR-008, BR-009 | [p01-homepage](https://dev.lamviec360.com/ux/public/p01-homepage.html) | Public |
| p02 | Job Search | JSP-FR-001, JSP-FR-002, JSP-FR-006, JSP-FR-010, BR-008, BR-009 | [p02-job-search](https://dev.lamviec360.com/ux/public/p02-job-search.html) | Public |
| p03 | Job Detail | JSP-FR-003, JSP-FR-010, BR-008, BR-009 | [p03-job-detail](https://dev.lamviec360.com/ux/public/p03-job-detail.html) | Public |
| p04 | Employer Landing | EMP-FR-001, EMP-FR-004, BR-002 | [p04-employer-landing](https://dev.lamviec360.com/ux/public/p04-employer-landing.html) | Public |
| p05 | Company Directory | JSP-FR-001, BR-008 | [p05-company-directory](https://dev.lamviec360.com/ux/public/p05-company-directory.html) | Public |
| a01 | Login | AUTH-FR-001, AUTH-FR-002, AUTH-FR-003, AUTH-FR-006 | [a01-login](https://dev.lamviec360.com/ux/auth/a01-login.html) | Auth |
| a02 | Register — Role Select | AUTH-FR-001, AUTH-FR-005 | [a02-register-role](https://dev.lamviec360.com/ux/auth/a02-register-role.html) | Auth |
| a03 | Register — Profile | AUTH-FR-001, AUTH-FR-004 | [a03-register-profile](https://dev.lamviec360.com/ux/auth/a03-register-profile.html) | Auth |
| a04 | OTP Verify | AUTH-FR-003 | [a04-otp-verify](https://dev.lamviec360.com/ux/auth/a04-otp-verify.html) | Auth |
| a05 | Forgot Password | AUTH-FR-004, AUTH-FR-006 | [a05-forgot-password](https://dev.lamviec360.com/ux/auth/a05-forgot-password.html) | Auth |
| a06 | First-Login Onboarding | AUTH-FR-001, AUTH-FR-008 | [a06-first-login-onboarding](https://dev.lamviec360.com/ux/auth/a06-first-login-onboarding.html) | Auth |
| a07 | Social Login | AUTH-FR-002, AUTH-FR-003, AUTH-FR-005 | [a07-social-login](https://dev.lamviec360.com/ux/auth/a07-social-login.html) | Auth |
| ow01 | Company Info Wizard | EMP-FR-001, EMP-FR-004, AUTH-FR-008 | [ow01-company-info](https://dev.lamviec360.com/ux/onboarding/ow01-company-info.html) | Onboarding |
| ow02 | Plan Confirmation | EMP-FR-003, BR-002 | [ow02-plan-confirm](https://dev.lamviec360.com/ux/onboarding/ow02-plan-confirm.html) | Onboarding |
| ow03 | Team Invite | EMP-FR-026, AUTH-FR-007, BR-007 | [ow03-team-invite](https://dev.lamviec360.com/ux/onboarding/ow03-team-invite.html) | Onboarding |
| ow04 | Pending Approval | EMP-FR-002, BR-001, BR-008 | [ow04-pending-approval](https://dev.lamviec360.com/ux/onboarding/ow04-pending-approval.html) | Onboarding |
| e01 | Employer Dashboard | EMP-FR-005, EMP-FR-006, EMP-FR-007, EMP-FR-008, BR-001, BR-002 | [e01-dashboard](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) | Employer |
| e02 | Job List | EMP-FR-011, EMP-FR-012, BR-002, BR-004 | [e02-job-list](https://dev.lamviec360.com/ux/employer/e02-job-list.html) | Employer |
| e03 | Create Job | EMP-FR-009, EMP-FR-010, EMP-FR-011, EMP-FR-013, EMP-FR-015, BR-003 | [e03-create-job](https://dev.lamviec360.com/ux/employer/e03-create-job.html) | Employer |
| e04 | Edit Job | EMP-FR-009, EMP-FR-012, BR-004 | [e04-edit-job](https://dev.lamviec360.com/ux/employer/e04-edit-job.html) | Employer |
| e05 | AI JD Generator | EMP-FR-010, BR-003, NFR-003, NFR-003b | [e05-ai-jd-generator](https://dev.lamviec360.com/ux/employer/e05-ai-jd-generator.html) | Employer |
| e06 | Pipeline Kanban | EMP-FR-016, EMP-FR-017, EMP-FR-018, EMP-FR-019, EMP-FR-019b, EMP-FR-020, EMP-FR-023, EMP-FR-023b, EMP-FR-024, BR-005, BR-013, BR-014 | [e06-pipeline-kanban](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) | Employer |
| e07 | Candidate Detail | EMP-FR-018, EMP-FR-021, EMP-FR-022, BR-005, BR-006, BR-013, BR-014 | [e07-candidate-detail](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) | Employer |
| e08 | Move Stage Modal | EMP-FR-019, EMP-FR-019b, EMP-FR-020, NTF-FR-006, BR-005 | [e08-move-stage-modal](https://dev.lamviec360.com/ux/employer/e08-move-stage-modal.html) | Employer |
| e09 | Reject & Notify | EMP-FR-025, EMP-FR-025b, NTF-FR-001, NTF-FR-004, NTF-FR-006, BR-005 | [e09-reject-notify](https://dev.lamviec360.com/ux/employer/e09-reject-notify.html) | Employer |
| e10 | Candidate Notes | EMP-FR-022, BR-005, BR-006 | [e10-candidate-notes](https://dev.lamviec360.com/ux/employer/e10-candidate-notes.html) | Employer |
| e11 | View Documents | JSP-FR-012, EMP-FR-021, BR-005, BR-014 | [e11-view-documents](https://dev.lamviec360.com/ux/employer/e11-view-documents.html) | Employer |
| e12 | Stage Config | EMP-FR-016, BR-005, BR-010 | [e12-stage-config](https://dev.lamviec360.com/ux/employer/e12-stage-config.html) | Employer |
| e13 | Rejection Templates | EMP-FR-025b, NTF-FR-004, BR-010 | [e13-rejection-templates](https://dev.lamviec360.com/ux/employer/e13-rejection-templates.html) | Employer |
| e14 | Team List | EMP-FR-026, EMP-FR-027, EMP-FR-028, EMP-FR-029, BR-007 | [e14-team-list](https://dev.lamviec360.com/ux/employer/e14-team-list.html) | Employer |
| e15 | Invite Member | EMP-FR-026, AUTH-FR-007, BR-007 | [e15-invite-member](https://dev.lamviec360.com/ux/employer/e15-invite-member.html) | Employer |
| e16 | Edit Role | EMP-FR-027, EMP-FR-028 | [e16-edit-role](https://dev.lamviec360.com/ux/employer/e16-edit-role.html) | Employer |
| e17 | Employer Notifications | NTF-FR-002, NTF-FR-003 | [e17-notifications](https://dev.lamviec360.com/ux/employer/e17-notifications.html) | Employer |
| e18 | Account Settings | AUTH-FR-006, AUTH-FR-009, NTF-FR-005 | [e18-account-settings](https://dev.lamviec360.com/ux/employer/e18-account-settings.html) | Employer |
| e19 | Company Profile | EMP-FR-001, EMP-FR-008, BR-008, BR-010 | [e19-company-profile](https://dev.lamviec360.com/ux/employer/e19-company-profile.html) | Employer |
| sa01 | Admin Dashboard | SAP-FR-001, SAP-FR-004, BR-001 | [sa01-dashboard](https://dev.lamviec360.com/ux/admin/sa01-dashboard.html) | Admin |
| sa02 | Company List | SAP-FR-001, SAP-FR-002, SAP-FR-003, BR-001, BR-008 | [sa02-companies](https://dev.lamviec360.com/ux/admin/sa02-companies.html) | Admin |
| sa03 | Company Detail | SAP-FR-002, SAP-FR-003, BR-001, BR-008 | [sa03-company-detail](https://dev.lamviec360.com/ux/admin/sa03-company-detail.html) | Admin |
| sa04 | User Management | SAP-FR-006, AUTH-FR-005 | [sa04-users](https://dev.lamviec360.com/ux/admin/sa04-users.html) | Admin |
| sa05 | User Detail | SAP-FR-006, AUTH-FR-006 | [sa05-user-detail](https://dev.lamviec360.com/ux/admin/sa05-user-detail.html) | Admin |
| sa06 | Advertisement Mgmt | SAP-FR-008 | [sa06-ads](https://dev.lamviec360.com/ux/admin/sa06-ads.html) | Admin |
| sa07 | Global Templates | EMP-FR-025b, NTF-FR-004, BR-010 | [sa07-templates](https://dev.lamviec360.com/ux/admin/sa07-templates.html) | Admin |
| sa08 | Data Retention | SAP-FR-007, BR-012, NFR-018 | [sa08-retention](https://dev.lamviec360.com/ux/admin/sa08-retention.html) | Admin |
| sa09 | Audit Log | BR-005, BR-012, SAP-FR-006 | [sa09-audit-log](https://dev.lamviec360.com/ux/admin/sa09-audit-log.html) | Admin |
| sa10 | Platform Reports | SAP-FR-004, SAP-FR-005 | [sa10-reports](https://dev.lamviec360.com/ux/admin/sa10-reports.html) | Admin |
| sa11 | System Settings | BR-010, AUTH-FR-006, SAP-FR-007, NFR-003 | [sa11-settings](https://dev.lamviec360.com/ux/admin/sa11-settings.html) | Admin |
| js01 | JS Dashboard | JSP-FR-007, JSP-FR-010, JSP-FR-011, JSP-FR-012, NTF-FR-003 | [js01-dashboard](https://dev.lamviec360.com/ux/job-seeker/js01-dashboard.html) | Job Seeker |
| js03 | Apply Form | JSP-FR-004, JSP-FR-005, JSP-FR-012, BR-009, BR-011 | [js03-apply](https://dev.lamviec360.com/ux/job-seeker/js03-apply.html) | Job Seeker |
| js04 | Application Tracker | JSP-FR-007, JSP-FR-007b, JSP-FR-008, NTF-FR-001 | [js04-tracker](https://dev.lamviec360.com/ux/job-seeker/js04-tracker.html) | Job Seeker |
| js05 | Application Detail | JSP-FR-007, JSP-FR-007b, JSP-FR-008, JSP-FR-009, JSP-FR-012, BR-014 | [js05-app-detail](https://dev.lamviec360.com/ux/job-seeker/js05-app-detail.html) | Job Seeker |
| js06 | Saved Jobs | JSP-FR-006, JSP-FR-010 | [js06-saved-jobs](https://dev.lamviec360.com/ux/job-seeker/js06-saved-jobs.html) | Job Seeker |
| js07 | Profile Editor | JSP-FR-004, JSP-FR-011, AUTH-FR-008 | [js07-profile](https://dev.lamviec360.com/ux/job-seeker/js07-profile.html) | Job Seeker |
| js09 | Document Archive | JSP-FR-012, BR-005, BR-014 | [js09-documents](https://dev.lamviec360.com/ux/job-seeker/js09-documents.html) | Job Seeker |
| js10 | Upload Document | JSP-FR-004, JSP-FR-012 | [js10-upload-doc](https://dev.lamviec360.com/ux/job-seeker/js10-upload-doc.html) | Job Seeker |
| js11 | AI Interview Prep | JSP-FR-009, BR-003, AC-019, NFR-003 | [js11-ai-prep](https://dev.lamviec360.com/ux/job-seeker/js11-ai-prep.html) | Job Seeker |
| js12 | Notifications | JSP-FR-008, NTF-FR-001, NTF-FR-003, NTF-FR-005 | [js12-notifications](https://dev.lamviec360.com/ux/job-seeker/js12-notifications.html) | Job Seeker |
| js13 | Account Settings | JSP-FR-004, AUTH-FR-006, AUTH-FR-009, NTF-FR-005, BR-011, BR-012 | [js13-settings](https://dev.lamviec360.com/ux/job-seeker/js13-settings.html) | Job Seeker |

---

### 8.2 AUTH — Authentication & Account Management

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| AUTH-FR-001 | Multi-step registration wizard Steps 1–4 | P1 | a02, a03, a04, a06 | [a02](https://dev.lamviec360.com/ux/auth/a02-register-role.html) · [a03](https://dev.lamviec360.com/ux/auth/a03-register-profile.html) · [a04](https://dev.lamviec360.com/ux/auth/a04-otp-verify.html) · [a06](https://dev.lamviec360.com/ux/auth/a06-first-login-onboarding.html) |
| AUTH-FR-002 | Email + password · Google OAuth · Zalo OAuth | P1 | a01, a07 | [a01](https://dev.lamviec360.com/ux/auth/a01-login.html) · [a07](https://dev.lamviec360.com/ux/auth/a07-social-login.html) |
| AUTH-FR-003 | OTP verification for all roles · 2FA for Company Admin | P1 | a04 | [a04](https://dev.lamviec360.com/ux/auth/a04-otp-verify.html) |
| AUTH-FR-004 | Forgot password with strength indicator | P1 | a05 | [a05](https://dev.lamviec360.com/ux/auth/a05-forgot-password.html) |
| AUTH-FR-005 | Role assignment at registration | P1 | a02 | [a02](https://dev.lamviec360.com/ux/auth/a02-register-role.html) |
| AUTH-FR-006 | Secure JWT sessions — invalidate on logout/password change | P1 | a05, e18, js13 | [a05](https://dev.lamviec360.com/ux/auth/a05-forgot-password.html) · [e18](https://dev.lamviec360.com/ux/employer/e18-account-settings.html) · [js13](https://dev.lamviec360.com/ux/job-seeker/js13-settings.html) |
| AUTH-FR-007 | Team invite email — 72hr activation link | P1 | ow03, e15 | [ow03](https://dev.lamviec360.com/ux/onboarding/ow03-team-invite.html) · [e15](https://dev.lamviec360.com/ux/employer/e15-invite-member.html) |
| AUTH-FR-008 | Role-differentiated first-login onboarding wizard | P1 | a06 | [a06](https://dev.lamviec360.com/ux/auth/a06-first-login-onboarding.html) |
| AUTH-FR-009 | Simplified OTP login for subsequent sessions | P1 | a01, a04 | [a01](https://dev.lamviec360.com/ux/auth/a01-login.html) · [a04](https://dev.lamviec360.com/ux/auth/a04-otp-verify.html) |

---

### 8.3 JSP — Job Seeker Portal

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| JSP-FR-001 | Public job search without login | P1 | p01, p02 | [p01](https://dev.lamviec360.com/ux/public/p01-homepage.html) · [p02](https://dev.lamviec360.com/ux/public/p02-job-search.html) |
| JSP-FR-002 | Job cards — title, company, salary VND, location, date, type | P1 | p02 | [p02](https://dev.lamviec360.com/ux/public/p02-job-search.html) |
| JSP-FR-003 | Job detail page with Apply Now button | P1 | p03 | [p03](https://dev.lamviec360.com/ux/public/p03-job-detail.html) |
| JSP-FR-004 | Job seeker profile — resume upload max 10MB | P1 | js07, js10 | [js07](https://dev.lamviec360.com/ux/job-seeker/js07-profile.html) · [js10](https://dev.lamviec360.com/ux/job-seeker/js10-upload-doc.html) |
| JSP-FR-005 | One-click apply with consent checkbox (BR-011) | P1 | js03 | [js03](https://dev.lamviec360.com/ux/job-seeker/js03-apply.html) |
| JSP-FR-006 | Save jobs to personal Saved Jobs list | P2 | js06 | [js06](https://dev.lamviec360.com/ux/job-seeker/js06-saved-jobs.html) |
| JSP-FR-007 | Application tracker — all statuses | P1 | js04 | [js04](https://dev.lamviec360.com/ux/job-seeker/js04-tracker.html) |
| JSP-FR-007b | Estimated stage duration indicator | P2 | js04, js05 | [js04](https://dev.lamviec360.com/ux/job-seeker/js04-tracker.html) · [js05](https://dev.lamviec360.com/ux/job-seeker/js05-app-detail.html) |
| JSP-FR-008 | Push/email notifications on status changes | P1 | js12 | [js12](https://dev.lamviec360.com/ux/job-seeker/js12-notifications.html) |
| JSP-FR-009 | AI interview preparation questions | P2 | js11 | [js11](https://dev.lamviec360.com/ux/job-seeker/js11-ai-prep.html) |
| JSP-FR-010 | Rule-based job recommendations | P1 | p02, p03, js01 | [p02](https://dev.lamviec360.com/ux/public/p02-job-search.html) · [p03](https://dev.lamviec360.com/ux/public/p03-job-detail.html) · [js01](https://dev.lamviec360.com/ux/job-seeker/js01-dashboard.html) |
| JSP-FR-011 | Guided profile completion prompts | P1 | js01, js07 | [js01](https://dev.lamviec360.com/ux/job-seeker/js01-dashboard.html) · [js07](https://dev.lamviec360.com/ux/job-seeker/js07-profile.html) |
| JSP-FR-012 | Document archive — encrypted storage, stage-gated access | P1 | js09, js10, e11 | [js09](https://dev.lamviec360.com/ux/job-seeker/js09-documents.html) · [js10](https://dev.lamviec360.com/ux/job-seeker/js10-upload-doc.html) · [e11](https://dev.lamviec360.com/ux/employer/e11-view-documents.html) |

---

### 8.4 EMP — Employer / Company Workspace

#### 8.4.1 Company Onboarding

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| EMP-FR-001 | Company registration form | P1 | ow01, e19 | [ow01](https://dev.lamviec360.com/ux/onboarding/ow01-company-info.html) · [e19](https://dev.lamviec360.com/ux/employer/e19-company-profile.html) |
| EMP-FR-002 | Pending approval status (BR-001) | P1 | ow04 | [ow04](https://dev.lamviec360.com/ux/onboarding/ow04-pending-approval.html) |
| EMP-FR-003 | Post-approval onboarding wizard | P1 | ow01, ow02, ow03 | [ow01](https://dev.lamviec360.com/ux/onboarding/ow01-company-info.html) · [ow02](https://dev.lamviec360.com/ux/onboarding/ow02-plan-confirm.html) · [ow03](https://dev.lamviec360.com/ux/onboarding/ow03-team-invite.html) |
| EMP-FR-004 | Onboarding progress indicator — under 5 min target | P1 | ow01, ow02, ow03 | [ow01](https://dev.lamviec360.com/ux/onboarding/ow01-company-info.html) |

#### 8.4.2 Company Dashboard

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| EMP-FR-005 | Dashboard KPIs — jobs, applications, interviews, quota 999 | P1 | e01 | [e01](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) |
| EMP-FR-006 | Quick-action bar | P1 | e01 | [e01](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) |
| EMP-FR-007 | Sidebar navigation | P1 | e01 | [e01](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) |
| EMP-FR-008 | Verified Company badge (BR-008) | P1 | e01, e19 | [e01](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) · [e19](https://dev.lamviec360.com/ux/employer/e19-company-profile.html) |

#### 8.4.3 Job Management

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| EMP-FR-009 | Create job form — all fields | P1 | e03 | [e03](https://dev.lamviec360.com/ux/employer/e03-create-job.html) |
| EMP-FR-010 | AI Generate JD button — Gemini, review gate, 2 retries | P1 | e03, e05 | [e03](https://dev.lamviec360.com/ux/employer/e03-create-job.html) · [e05](https://dev.lamviec360.com/ux/employer/e05-ai-jd-generator.html) |
| EMP-FR-011 | Save job as Draft | P1 | e02, e03 | [e02](https://dev.lamviec360.com/ux/employer/e02-job-list.html) · [e03](https://dev.lamviec360.com/ux/employer/e03-create-job.html) |
| EMP-FR-012 | Edit, close, pause, duplicate job (BR-004) | P1 | e02, e04 | [e02](https://dev.lamviec360.com/ux/employer/e02-job-list.html) · [e04](https://dev.lamviec360.com/ux/employer/e04-edit-job.html) |
| EMP-FR-013 | Job posting under 2 minutes target | P1 | e03 | [e03](https://dev.lamviec360.com/ux/employer/e03-create-job.html) |
| EMP-FR-014 | Social sharing links | P2 | e04, p03 | [e04](https://dev.lamviec360.com/ux/employer/e04-edit-job.html) · [p03](https://dev.lamviec360.com/ux/public/p03-job-detail.html) |
| EMP-FR-015 | JD completion progress indicator | P2 | e03 | [e03](https://dev.lamviec360.com/ux/employer/e03-create-job.html) |

#### 8.4.4 Candidate Pipeline Management

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| EMP-FR-016 | Kanban pipeline — configurable stages | P1 | e06, e12 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e12](https://dev.lamviec360.com/ux/employer/e12-stage-config.html) |
| EMP-FR-017 | Phase 1: visual columns + dropdown movement, no drag-and-drop | P1 | e06 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) |
| EMP-FR-018 | Candidate card information display | P1 | e06, e07 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e07](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) |
| EMP-FR-019 | Stage selector with immutable audit trail (BR-005) | P1 | e06, e08 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e08](https://dev.lamviec360.com/ux/employer/e08-move-stage-modal.html) |
| EMP-FR-019b | Bulk undo within 5 minutes | P1 | e06, e08 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e08](https://dev.lamviec360.com/ux/employer/e08-move-stage-modal.html) |
| EMP-FR-020 | Bulk stage movement with confirmation | P1 | e06, e08 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e08](https://dev.lamviec360.com/ux/employer/e08-move-stage-modal.html) |
| EMP-FR-021 | Full candidate profile in slide-out panel | P1 | e07 | [e07](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) |
| EMP-FR-022 | Internal notes — Admin + HR only (BR-006) | P1 | e07, e10 | [e07](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) · [e10](https://dev.lamviec360.com/ux/employer/e10-candidate-notes.html) |
| EMP-FR-023 | AI Screen Candidates — Gemini ranked list | P1 | e06 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) |
| EMP-FR-023b | AI screening safeguards — badge, override, no auto-reject | P1 | e06, e07 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e07](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) |
| EMP-FR-024 | Filter and sort pipeline | P1 | e06 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) |
| EMP-FR-025 | Send email notifications from pipeline | P1 | e09 | [e09](https://dev.lamviec360.com/ux/employer/e09-reject-notify.html) |
| EMP-FR-025b | Structured rejection template library | P1 | e09, e13 | [e09](https://dev.lamviec360.com/ux/employer/e09-reject-notify.html) · [e13](https://dev.lamviec360.com/ux/employer/e13-rejection-templates.html) |

#### 8.4.5 Team Management (RBAC)

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| EMP-FR-026 | Invite team members by email with role | P1 | e14, e15 | [e14](https://dev.lamviec360.com/ux/employer/e14-team-list.html) · [e15](https://dev.lamviec360.com/ux/employer/e15-invite-member.html) |
| EMP-FR-027 | Role enforcement — Admin, HR, Viewer | P1 | e14, e16 | [e14](https://dev.lamviec360.com/ux/employer/e14-team-list.html) · [e16](https://dev.lamviec360.com/ux/employer/e16-edit-role.html) |
| EMP-FR-028 | Modify or revoke access — immediate effect | P1 | e14, e16 | [e14](https://dev.lamviec360.com/ux/employer/e14-team-list.html) · [e16](https://dev.lamviec360.com/ux/employer/e16-edit-role.html) |
| EMP-FR-029 | Team management screen — role, join date, last login, status | P1 | e14 | [e14](https://dev.lamviec360.com/ux/employer/e14-team-list.html) |

---

### 8.5 SAP — Super Admin Panel

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| SAP-FR-001 | Global company list with status | P1 | sa01, sa02 | [sa01](https://dev.lamviec360.com/ux/admin/sa01-dashboard.html) · [sa02](https://dev.lamviec360.com/ux/admin/sa02-companies.html) |
| SAP-FR-002 | Approve, reject, suspend companies | P1 | sa02, sa03 | [sa02](https://dev.lamviec360.com/ux/admin/sa02-companies.html) · [sa03](https://dev.lamviec360.com/ux/admin/sa03-company-detail.html) |
| SAP-FR-003 | Tenant approval panel | P1 | sa02, sa03 | [sa02](https://dev.lamviec360.com/ux/admin/sa02-companies.html) · [sa03](https://dev.lamviec360.com/ux/admin/sa03-company-detail.html) |
| SAP-FR-004 | Platform analytics dashboard | P1 | sa01, sa10 | [sa01](https://dev.lamviec360.com/ux/admin/sa01-dashboard.html) · [sa10](https://dev.lamviec360.com/ux/admin/sa10-reports.html) |
| SAP-FR-005 | Content moderation | P1 | sa02 | [sa02](https://dev.lamviec360.com/ux/admin/sa02-companies.html) |
| SAP-FR-006 | User account management | P1 | sa04, sa05 | [sa04](https://dev.lamviec360.com/ux/admin/sa04-users.html) · [sa05](https://dev.lamviec360.com/ux/admin/sa05-user-detail.html) |
| SAP-FR-007 | Data retention management (BR-012) | P1 | sa08, sa11 | [sa08](https://dev.lamviec360.com/ux/admin/sa08-retention.html) · [sa11](https://dev.lamviec360.com/ux/admin/sa11-settings.html) |
| SAP-FR-008 | Advertisement banner slot management | P1 | sa06 | [sa06](https://dev.lamviec360.com/ux/admin/sa06-ads.html) |

---

### 8.6 NTF — Notifications & Communication

| Req ID | Requirement Summary | Priority | Screen IDs | Dev Portal |
| --- | --- | --- | --- | --- |
| NTF-FR-001 | Automated emails to job seekers — status changes, rejection | P1 | js12, e09 | [js12](https://dev.lamviec360.com/ux/job-seeker/js12-notifications.html) · [e09](https://dev.lamviec360.com/ux/employer/e09-reject-notify.html) |
| NTF-FR-002 | Automated emails to HR — new application, interview | P1 | e17 | [e17](https://dev.lamviec360.com/ux/employer/e17-notifications.html) |
| NTF-FR-003 | In-app notification bell | P1 | e17, js12 | [e17](https://dev.lamviec360.com/ux/employer/e17-notifications.html) · [js12](https://dev.lamviec360.com/ux/job-seeker/js12-notifications.html) |
| NTF-FR-004 | Parameterised email templates — Vietnamese default | P1 | e13, sa07 | [e13](https://dev.lamviec360.com/ux/employer/e13-rejection-templates.html) · [sa07](https://dev.lamviec360.com/ux/admin/sa07-templates.html) |
| NTF-FR-005 | Job seeker notification preferences | P2 | js13 | [js13](https://dev.lamviec360.com/ux/job-seeker/js13-settings.html) |
| NTF-FR-006 | HR suppresses individual stage-change notifications | P1 | e08, e09 | [e08](https://dev.lamviec360.com/ux/employer/e08-move-stage-modal.html) · [e09](https://dev.lamviec360.com/ux/employer/e09-reject-notify.html) |

---

### 8.7 Business Rules Traceability

| Rule ID | Business Rule Summary | Implemented In | Dev Portal |
| --- | --- | --- | --- |
| BR-001 | Company locked until SA approval — pending banner shown | ow04, e01, sa01, sa02, sa03 | [ow04](https://dev.lamviec360.com/ux/onboarding/ow04-pending-approval.html) · [e01](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) · [sa02](https://dev.lamviec360.com/ux/admin/sa02-companies.html) |
| BR-002 | Job quota displayed as 999 in Phase 1 (unlimited) | e01, e02, e03, ow02 | [e01](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) · [ow02](https://dev.lamviec360.com/ux/onboarding/ow02-plan-confirm.html) |
| BR-003 | AI output requires human confirmation — never auto-published | e05, e06, js11 | [e05](https://dev.lamviec360.com/ux/employer/e05-ai-jd-generator.html) · [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [js11](https://dev.lamviec360.com/ux/job-seeker/js11-ai-prep.html) |
| BR-004 | Closed job retains all application data | e02, e04 | [e02](https://dev.lamviec360.com/ux/employer/e02-job-list.html) · [e04](https://dev.lamviec360.com/ux/employer/e04-edit-job.html) |
| BR-005 | Pipeline stage history immutable — no delete on audit trail | e06, e07, e08, e10, e12, sa09 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e08](https://dev.lamviec360.com/ux/employer/e08-move-stage-modal.html) · [sa09](https://dev.lamviec360.com/ux/admin/sa09-audit-log.html) |
| BR-006 | Candidate notes visible to Admin + HR only — not Viewer | e07, e10 | [e07](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) · [e10](https://dev.lamviec360.com/ux/employer/e10-candidate-notes.html) |
| BR-007 | Team invite link expires after 72 hours | ow03, e14, e15, sa11 | [ow03](https://dev.lamviec360.com/ux/onboarding/ow03-team-invite.html) · [e15](https://dev.lamviec360.com/ux/employer/e15-invite-member.html) |
| BR-008 | Verified badge granted by SA only — not on self-registration | p02, p03, e01, e19, ow04, sa02, sa03 | [e01](https://dev.lamviec360.com/ux/employer/e01-dashboard.html) · [ow04](https://dev.lamviec360.com/ux/onboarding/ow04-pending-approval.html) · [sa02](https://dev.lamviec360.com/ux/admin/sa02-companies.html) |
| BR-009 | Salary in VND — negotiable hides exact range | p02, p03, e03, js07 | [p02](https://dev.lamviec360.com/ux/public/p02-job-search.html) · [e03](https://dev.lamviec360.com/ux/employer/e03-create-job.html) |
| BR-010 | All dropdown lists configurable via SA panel — not hardcoded | sa07, sa11 | [sa07](https://dev.lamviec360.com/ux/admin/sa07-templates.html) · [sa11](https://dev.lamviec360.com/ux/admin/sa11-settings.html) |
| BR-011 | Consent checkbox mandatory — cannot submit without | js03, js13 | [js03](https://dev.lamviec360.com/ux/job-seeker/js03-apply.html) · [js13](https://dev.lamviec360.com/ux/job-seeker/js13-settings.html) |
| BR-012 | Data retention auto-delete — audit logs exempt | sa08, sa11, js13 | [sa08](https://dev.lamviec360.com/ux/admin/sa08-retention.html) · [sa11](https://dev.lamviec360.com/ux/admin/sa11-settings.html) · [js13](https://dev.lamviec360.com/ux/job-seeker/js13-settings.html) |
| BR-013 | AI screening — transparency badge + HR override + no auto-reject | e06, e07 | [e06](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) · [e07](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) |
| BR-014 | Contact details and documents gated by pipeline stage | e07, e11, js05, js09 | [e07](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) · [e11](https://dev.lamviec360.com/ux/employer/e11-view-documents.html) · [js09](https://dev.lamviec360.com/ux/job-seeker/js09-documents.html) |

---

### 8.8 Acceptance Criteria Traceability

| AC ID | Feature | Acceptance Criteria | Dev Portal Screen |
| --- | --- | --- | --- |
| AC-001 | Company Onboarding | Complete registration, verify, profile setup under 5 min on mobile. | [ow01-company-info](https://dev.lamviec360.com/ux/onboarding/ow01-company-info.html) |
| AC-002 | Job Posting | Create and publish job using AI JD within 2 minutes. | [e03-create-job](https://dev.lamviec360.com/ux/employer/e03-create-job.html) |
| AC-003 | Job Search | Search with ≥3 filters, results within 3 seconds. | [p02-job-search](https://dev.lamviec360.com/ux/public/p02-job-search.html) |
| AC-004 | Application Submission | One-click apply under 60 seconds. Consent mandatory (BR-011). | [js03-apply](https://dev.lamviec360.com/ux/job-seeker/js03-apply.html) |
| AC-005 | Application Tracking | Job seeker sees correct status for all applications. | [js04-tracker](https://dev.lamviec360.com/ux/job-seeker/js04-tracker.html) |
| AC-006 | Pipeline Management | HR filters, views, moves candidate in under 3 clicks. | [e06-pipeline-kanban](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) |
| AC-007 | AI JD Generation | AI returns draft under 10 sec. 2 retries. Session cached. | [e05-ai-jd-generator](https://dev.lamviec360.com/ux/employer/e05-ai-jd-generator.html) |
| AC-008 | Notifications | Job seeker receives email within 5 min of status update. | [js12-notifications](https://dev.lamviec360.com/ux/job-seeker/js12-notifications.html) |
| AC-009 | RBAC Enforcement | Viewer cannot access job creation, pipeline, or settings. | [e16-edit-role](https://dev.lamviec360.com/ux/employer/e16-edit-role.html) |
| AC-010 | Admin Approval Gate | Pending company cannot post jobs or appear in search (BR-001). | [ow04-pending-approval](https://dev.lamviec360.com/ux/onboarding/ow04-pending-approval.html) |
| AC-011 | Mobile Responsiveness | Core flows usable at 375px without horizontal scroll. | [p01-homepage](https://dev.lamviec360.com/ux/public/p01-homepage.html) |
| AC-012 | Verified Badge | Verified badge shown only after SA approval (BR-008). | [sa03-company-detail](https://dev.lamviec360.com/ux/admin/sa03-company-detail.html) |
| AC-013 | Contact Visibility | Contact visible from Screening stage onward by default (BR-014). | [e07-candidate-detail](https://dev.lamviec360.com/ux/employer/e07-candidate-detail.html) |
| AC-014 | Bulk Undo | Undo bulk move within 5 min. Double-confirm on bulk reject. | [e08-move-stage-modal](https://dev.lamviec360.com/ux/employer/e08-move-stage-modal.html) |
| AC-015 | AI Screening Safeguards | AI badge shown, HR override present, no auto-reject (BR-013). | [e06-pipeline-kanban](https://dev.lamviec360.com/ux/employer/e06-pipeline-kanban.html) |
| AC-016 | Data Consent | Application cannot submit without consent checked (BR-011). | [js03-apply](https://dev.lamviec360.com/ux/job-seeker/js03-apply.html) |
| AC-017 | Rejection Templates | HR selects from template library. Optional delayed send. | [e09-reject-notify](https://dev.lamviec360.com/ux/employer/e09-reject-notify.html) |
| AC-018 | Job Recommendations | Rule-based similar jobs on dashboard and detail page. | [js01-dashboard](https://dev.lamviec360.com/ux/job-seeker/js01-dashboard.html) |
| AC-019 | AI Session Limit | After 5 AI calls: warned. After 3 abuse events: session terminated. | [e05-ai-jd-generator](https://dev.lamviec360.com/ux/employer/e05-ai-jd-generator.html) |

---

*LV360-PRD-001 · v1.1 · March 2026 · © tresundios Software · Zebra Global Pte Ltd · Confidential*
