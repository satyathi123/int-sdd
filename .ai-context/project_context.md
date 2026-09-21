# Project Context — MobilityHub (Employee Internal Transfer)

## Objective

**MobilityHub (Employee Internal Transfer Digital Journey)** delivers a centralized, transparent, self-service internal mobility experience within the organisation's One-Point Employee Portal.

The platform streamlines internal mobility by orchestrating employee transfer requests through a structured 3-stage approval hierarchy (Releasing Manager → Receiving Manager → HR Operations), automated eligibility policy checks, and automated handoffs to HRMS, Payroll, IT Access, and Facilities.

**Outcome:** One employee request → One orchestrated multi-stage workflow → One transparent progress dashboard.

## Architecture Summary

**Modular Monolith (Microservice-Ready with domain boundaries)**
The application is structured with domain boundaries separating Portal Request Management, Workflow Orchestration, Eligibility Evaluation, and Downstream Integration Adapters. 

See `.ai-context/architecture.md` and `.ai-context/constitution.md`.

## Key Business Policies & Rules

1. **Eligibility Baseline:** Minimum 6-month continuous tenure, rating ≥ Meets Expectations, no active PIP.
2. **Approval Chain:** 3-Tier sequential approvals (Releasing Manager → Receiving Manager → HR Operations) with a 5-day review SLA.
3. **Vacancy Mapping:** Applications must reference an approved open internal job posting (IJP).
4. **Effective Date:** At least 30 calendar days from submission, aligned to pay period boundary.
5. **Downstream Integration:** Asynchronous handoffs to HRMS, Payroll, IT IAM, and Facilities with automated exception triage.

## Stakeholders

| Role | Interest |
|---|---|
| Employee | Browses IJPs, submits, tracks, and manages transfer request |
| Releasing Manager | Evaluates release timeline, project continuity, and knowledge transfer |
| Receiving Manager | Assesses candidate fit, team capacity, and accepts transfer |
| HR Business Partner / Ops | Validates policy compliance, grade alignment, compensation, and grants final approval |
| Payroll Operations | Updates cost centers, tax locations, and pay element adjustments |
| IT Operations / IAM | Provisions target system access, reclaims source permissions |
| Facilities | Arranges seat allocations, badges, and workspace logistics |
| Portal Product Owner | Oversees journey user experience and operational metrics |

## Tech Stack

- **Frontend:** React (Vite) + Tailwind CSS
- **Backend:** Node.js (Express / TypeScript)
- **Database:** PostgreSQL
- **ORM / Data Access:** Prisma ORM
- **Authentication:** JWT Bearer Tokens (RBAC enabled)
- **Testing:** Jest / React Testing Library

## Governance & Reviewer Roster

- **Project Manager / Product Owner (Gate 1 Reviewer):** Supratim Jetty (`supratim.jetty@intglobal.com`)
- **Technical Lead / Architect (Gate 2 Reviewer):** Supratim Jetty (`supratim.jetty@intglobal.com`)
- **Senior Software Engineer / Spec Author:** Anand Satyarthi (`anand.satyarthi@indusnet.co.in`)
- **Authenticated Developer Git Email:** `anand.satyarthi@indusnet.co.in`

## Core Artifacts

1. `.ai-context/BRD.md` — Authoritative business requirements document (v1.1)
2. `.ai-context/brd-change-log.md` — Change audit log for BRD
3. `.ai-context/constitution.md` — Technical constitution & engineering constraints
4. `.ai-context/architecture.md` — System architecture and execution layout
5. `.ai-context/status.md` — Project lifecycle status board
