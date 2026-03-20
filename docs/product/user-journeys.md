# User Journeys — LàmViệc360
## Phase 1 MVP · End-to-End Flows

---

## Journey 1 — Company Onboarding (Haanh / Company Admin)
**PRD refs:** AUTH-FR-001, AUTH-FR-008, EMP-FR-001–004, BR-001, BR-008  
**Success metric:** Completed in < 5 minutes on mobile (AC-001)

```
[Discovers platform] → p01-homepage
  ↓ Clicks "For Employers" CTA
[Registers as Employer] → a02-register-role
  ↓ Selects "Employer / HR"
[Fills profile] → a03-register-profile
  · Full name, email, phone (+84), password
  · Phone used for OTP
  ↓
[OTP Verification] → a04-otp-verify
  · 6-digit code, 10-min expiry
  · AUTH-FR-003
  ↓
[Company Info Wizard Step 1] → ow01-company-info
  · Company name, Reg No, Industry, Size, Province, Website, Description, Logo
  · EMP-FR-001
  ↓
[Plan Confirmation Step 2] → ow02-plan-confirm
  · Phase 1: Free auto-confirmed
  · Quota: 999 jobs (BR-002)
  ↓
[Team Invite Step 3] → ow03-team-invite (optional)
  · Invite HR by email, assign role
  · Invite expires 72h (BR-007)
  ↓
[Pending Approval] → ow04-pending-approval ←── BR-001 gate
  · Workspace locked
  · SA reviews → approves
  ↓
[Employer Dashboard] → e01-dashboard
  · Verified badge granted (BR-008)
  · KPIs visible
  · ✅ Journey complete
```

**Failure paths:**
- Email already registered → "Account exists, sign in instead"
- Company registration number already in system → inline error
- SA rejects company → email with reason, re-submission link

---

## Journey 2 — Post a Job with AI JD (Minh / HR Recruiter)
**PRD refs:** EMP-FR-009, EMP-FR-010, EMP-FR-011, EMP-FR-013, BR-003, NFR-003  
**Success metric:** Job published in < 2 minutes (AC-002)

```
[Employer Dashboard] → e01-dashboard
  ↓ Clicks "Post New Job"
[Create Job Form] → e03-create-job
  · Job Title*, Department, Type, Location, Salary (VND), Skills tags
  · JD progress indicator: 25% → 50% → 75% → 100%
  ↓ Clicks "AI Generate JD"
[AI JD Generator] → e05-ai-jd-generator
  · Loading: "Generating your JD..." (< 10 sec — NFR-003)
  · Output: Full JD with "AI Generated" badge
  · Session counter: "1 of 5 requests used" (BR-003)
  ↓ Reviews content
  · Confirm & Insert  ← always required (BR-003)
    OR Regenerate (max 2 retries)
    OR Edit Manually
  ↓ Clicks "Confirm & Insert"
[Back on Create Job — JD populated]
  · Remaining fields: Vacancies, Deadline, Selection Criteria
  ↓
[Save Draft or Publish]
  · Draft → saved, not visible publicly (EMP-FR-011)
  · Publish → appears on portal immediately
  ↓
[Job List] → e02-job-list
  · New job card visible
  · Status: Published / Draft
  · ✅ Journey complete
```

**Failure paths:**
- AI API timeout → retry button shown (max 2 retries — NFR-003)
- After 2 retries fail → "AI unavailable, please write manually"
- Session-cached draft preserved on timeout (NFR-003b)

---

## Journey 3 — Job Seeker Applies for a Job (Thuy)
**PRD refs:** JSP-FR-001–005, JSP-FR-007, BR-009, BR-011, AC-003, AC-004  
**Success metric:** Apply in < 60 seconds (AC-004)

```
[Homepage] → p01-homepage
  ↓ Types keyword in hero search
[Job Search] → p02-job-search
  · Results: title, company + logo, location, salary (VND — BR-009)
  · Verified badge on approved companies (BR-008)
  · Filters: location, industry, salary range, job type
  ↓ Clicks a job card
[Job Detail] → p03-job-detail
  · Full JD, company profile, salary, deadline
  · "Similar Jobs" strip (JSP-FR-010)
  ↓ Clicks "Apply Now" (requires login)
[Login / Register] → a01-login or a07-social-login
  · Zalo login (one-click for returning users)
  ↓ Authenticated
[Apply Form] → js03-apply
  · Pre-populated from saved profile (JSP-FR-005)
  · Cover letter (optional)
  · Document selection from archive (JSP-FR-012)
  · CONSENT CHECKBOX ← mandatory (BR-011)
    "Tôi đồng ý để [Company] xử lý dữ liệu..."
    Cannot submit without checking
  ↓ Submits
[Confirmation screen]
  · "Application submitted!"
  · NTF-FR-001: confirmation email sent
  ↓
[Application Tracker] → js04-tracker
  · New application appears: Status = "Applied"
  · JSP-FR-007
  · ✅ Journey complete
```

**Failure paths:**
- Not logged in → redirect to a01-login with return URL
- Profile incomplete → JSP-FR-011: prompt to complete before applying
- Consent not checked → inline error, cannot submit (BR-011)
- Already applied → "You've already applied" with tracker link

---

## Journey 4 — HR Manages Candidate Pipeline (Minh)
**PRD refs:** EMP-FR-016–025b, BR-005, BR-006, BR-013, BR-014  
**Success metric:** Move candidate in < 3 clicks (AC-006)

```
[Employer Dashboard] → e01-dashboard
  ↓ Clicks "View Pipeline" for a job
[Pipeline Kanban] → e06-pipeline-kanban
  · Columns: Applied | Screening | Shortlisted | Interview | Offer | Hired | Rejected
  · Candidate cards: name, exp, AI score, stage dropdown
  ↓ Clicks candidate card
[Candidate Detail] → e07-candidate-detail
  · Tabs: Overview / Resume / Notes / Documents / History
  · Documents tab locked if below Screening stage (BR-014)
  · Contact details shown/hidden by stage threshold (BR-014)
  ↓ Moves candidate via dropdown
[Stage Move Confirmation modal] → e08-move-stage-modal
  · "Move Nguyễn Thị Thu from Applied → Screening?"
  · Notification toggle: "Notify candidate" (NTF-FR-006)
  ↓ Confirms
  · Audit log updated (EMP-FR-019, BR-005)
  · Candidate notified (if not suppressed)
  ↓ Wants to add a note
[Candidate Notes] → e10-candidate-notes (or inline)
  · Note visible to Admin + HR only (BR-006)
  · Edit history logged (EMP-FR-022)
  ↓ Wants to run AI screening
[AI Screening results panel]
  · Transparency badge displayed (EMP-FR-023b, BR-013)
  · AI bias disclaimer visible before running
  · HR override button present
  · No auto-rejection — HR confirms any stage change
  ↓ Wants to reject candidates in bulk
[Bulk select → Reject & Notify] → e09-reject-notify
  · Template selector (EMP-FR-025b)
  · Double-confirmation modal (EMP-FR-019b)
  · "Suppress notification" toggle (NTF-FR-006)
  · ✅ Journey complete (undo available within 5 min — EMP-FR-019b)
```

---

## Journey 5 — Super Admin Approves a Company (Admin)
**PRD refs:** SAP-FR-001–003, BR-001, BR-008  

```
[Admin Dashboard] → sa01-dashboard
  · Pending approval count badge (e.g. "3 pending")
  ↓ Clicks "Review Pending"
[Company List — Pending filter] → sa02-companies
  · Companies in "Pending" state listed
  ↓ Clicks a company
[Company Detail] → sa03-company-detail
  · Full company profile submitted during onboarding
  · Business Registration Number displayed
  ↓ Reviews legitimacy
  
  PATH A — Approve:
  · Clicks "Approve"
  · Company status → Active
  · Verified badge granted (BR-008)
  · Company Admin notified via email (NTF-FR-001)
  · Workspace unlocked (BR-001)

  PATH B — Reject:
  · Clicks "Reject"
  · Must enter rejection reason (required field)
  · Reason included in notification email
  · Company Admin notified
  
  · ✅ Journey complete
```

---

## Flow Connectivity Map

```
p01 ──────────────────────────────────→ p02 → p03
                                              ↓ (login required)
a01 ← a02 ← a03 ← a04 ← a05            a07 (Zalo/Google)
 ↓                                        ↓
ow01 → ow02 → ow03 → ow04 (pending)
                           ↓ (SA approves)
              ┌────────────────────────────────────┐
              │         EMPLOYER WORKSPACE          │
              │  e01 → e02 → e03 → e05             │
              │  e06 → e07 → e08 → e09 → e10       │
              │  e12 → e13 → e14 → e15 → e16       │
              │  e17 → e18 → e19                   │
              └────────────────────────────────────┘

              ┌────────────────────────────────────┐
              │         JOB SEEKER PORTAL           │
              │  js01 → js03 → js04 → js05         │
              │  js06 → js07 → js09 → js10 → js11  │
              │  js12 → js13                       │
              └────────────────────────────────────┘

              ┌────────────────────────────────────┐
              │          SUPER ADMIN PANEL          │
              │  sa01 → sa02 → sa03                │
              │  sa04 → sa05                       │
              │  sa06 → sa07 → sa08 → sa09         │
              │  sa10 → sa11                       │
              └────────────────────────────────────┘
```

---

*LV360-JOURNEYS-001 · v1.0 · March 2026 · tresundios Software · Based on PRD v2.0*
