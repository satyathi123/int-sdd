# BRD Change Log

## Baseline Initialized
- **Date:** 2026-09-17
- **Version:** 1.0.0-init
- **Summary:** Initial project workspace setup.

## Version 1.1 — Customized Business Rules & Differentiation
- **Date:** 2026-09-17
- **Version:** 1.1.0
- **Summary of Business Changes:**
  1. **Formalized 3-Tier Approval Workflow:** Releasing Manager → Receiving Manager → HR Operations (BRD-003).
  2. **Concrete Eligibility Policy Gate:** Mandated 6-month continuous tenure, performance rating ≥ "Meets Expectations" (≥ 3.0/5.0), and zero active PIP/disciplinary warnings (BRD-002).
  3. **Strict Requisition-Linked IJP Model:** Bound transfer requests to approved Open Internal Job Postings / vacancy requisitions (BRD-001).
  4. **Operational SLA & Auto-Escalation:** Defined 5-day review SLA with Day 3 reminder and Day 5 auto-escalation to HR Operations (BRD-004).
  5. **Effective Date Rules:** Minimum 30 calendar days notice aligned with upcoming pay cycle (BRD-001).
  6. **Downstream Resilient Orchestration:** HRMS as point-of-no-return, accompanied by Payroll, IT, and Facilities asynchronous tasks with automated HR remediation alerts on partial failure (BRD-005).

## Version 1.2 — Comprehensive BRD Baseline (Gate 0 Revisions Incorporated)
- **Date:** 2026-09-26
- **Version:** 1.2.0
- **Gate Review Reference:** `.ai-context/pr_reviews/GATE0-brd-baseline-20260925-182500.md` (Reviewer: Supratim Jetty)
- **Summary of Business Changes:**
  1. **Expanded Transfer Categories (§2):** Incorporated Departmental/Cross-Functional Transfers, Location & Relocation Shifts, Lateral Role Moves, and Project-Driven Reallocations beyond performance/appraisal-linked transfers.
  2. **Explicit Functional Requirements (§4):** Added FR-001 through FR-009 covering Initiation, Eligibility Engine, Workflow Engine, SLA Engine, Decision Capture, Downstream Integration, Dashboard Tracking, Withdrawal/Cancellation, and Exception Triage.
  3. **Comprehensive Non-Functional Requirements (§5):** Added NFR-001 through NFR-005 covering Performance & Latencies ($< 500$ms API, $< 1.5$s UI), Security & PII Protection (RBAC, PII-free logs, TLS 1.3 / AES-256), Scalability ($99.9\%$ uptime, 10k users), Auditability (7-year append-only audit trail), and Resilience (idempotency, fault tolerance).
  4. **Business Assumptions, Dependencies & Constraints (§6):** Documented HRMS/ATS assumptions, external system dependencies (HRMS, Payroll, IAM, Facilities), and operational constraints.

