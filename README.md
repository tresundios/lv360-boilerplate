# LàmViệc360 — Stage 1 UI Prototype

AI-powered recruitment SaaS for Vietnam.
Stage 1: Functional HTML screens with complete flows.

## Project Structure
~~~
lv360-boilerplate/
├── .windsurf/
│   ├── rules.md          # Master AI coding rules
│   ├── frontend.md       # CSS tokens + components
│   ├── ui-ux.md          # Screen UX specifications
│   └── ai.md             # Windsurf Cascade prompts
│
├── docs/
│   ├── product/
│   │   ├── PRD.md              # Full PRD v2.0
│   │   ├── user-personas.md    # 5 user personas
│   │   └── user-journeys.md    # 5 end-to-end flows
│   │
│   └── design/
│       ├── design-system.md    # Tokens + components
│       ├── wireframes.md       # Layout specs
│       └── ux-guidelines.md    # Interaction patterns
│
├── screens/
│   ├── index.html              # All 54 screens index
│   ├── flow-map.html           # Visual flow diagram
│   ├── verification-report.md  # Cross-link report
│   ├── public/                 # p01–p05 (5 screens)
│   ├── auth/                   # a01–a07 (7 screens)
│   ├── onboarding/             # ow01–ow04 (4 screens)
│   ├── employer/               # e01–e19 (19 screens)
│   ├── admin/                  # sa01–sa11 (11 screens)
│   └── job-seeker/             # js01–js13 (11 screens)
│
└── assets/
    └── images/
        └── logo.png            # Product logo
~~~

## Quick Start

1. Open lv360-boilerplate/ in Windsurf IDE
2. Open screens/index.html in browser to navigate 
   all screens
3. Open screens/flow-map.html to see user journeys

## Screen Count

| Module          | Screens | IDs              |
|-----------------|---------|------------------|
| Public Portal   | 5       | p01–p05          |
| Authentication  | 7       | a01–a07          |
| Onboarding      | 4       | ow01–ow04        |
| Employer        | 19      | e01–e19          |
| Super Admin     | 11      | sa01–sa11        |
| Job Seeker      | 11      | js01,js03–js07,  |
|                 |         | js09–js13        |
| Total           | 57      |                  |

Note: js02 and js08 are intentionally skipped.
index.html and flow-map.html are dev tools (not product).

## Key Rules

All screens follow:
- RULE G-001: Fully responsive (375px / 768px / 1280px)
- RULE G-002: EN/VI bilingual toggle (default Vietnamese)
- RULE G-003: Logo from assets/images/logo.png

All screens enforce:
- BR-011: Consent checkbox mandatory on applications
- BR-003: AI output always needs human confirmation
- BR-005: Pipeline history is immutable
- BR-014: Documents and contacts gated by pipeline stage

## PRD Reference

Product Requirements: docs/product/PRD.md
Document ID: LV360-PRD-002 v2.0
Client: Zebra Global Pte Ltd
Dev Partner: tresundios Software
Go-Live: 19 June 2026 (M9)

## Tech Stack (Stage 1)

Stage 1: Vanilla HTML + CSS Variables + Vanilla JS
Stage 2: React 18 + TypeScript + Tailwind CSS + Vite
Stage 3: + FastAPI + PostgreSQL + Google Gemini API
Hosting: cloudbase.vn
