# Business Requirements Document (BRD)

## Project Name
Employee Internal Transfer (MobilityHub)

## Document Metadata
- **Version:** 1.1 (Customized Baseline)
- **Status:** Changes Requested (Gate 0 BRD PR Review - Revisions Required)
- **Process Owner:** HR Operations & People Operations Team
- **Product Owner:** One-Point Employee Portal Product Team
- **Technical Author:** Anand Satyarthi (`anand.satyarthi@indusnet.co.in`)
- **Assigned Reviewer (Gate 0 / Gate 1):** Supratim Jetty (`supratim.jetty@intglobal.com`)

---

## 1. Executive Summary & Business Need
Today, an employee requesting an internal department or location transfer must manually coordinate with multiple departments—current manager, receiving manager, HR business partners, payroll, IT identity & access, and facilities. This decentralized process results in lack of visibility, extended turnaround times, handover gaps, and operational errors during transition.

The **Employee Internal Transfer (MobilityHub)** project digitizes and orchestrates the end-to-end internal transfer journey into a single unified self-service experience on the One-Point Employee Portal.

---

## 2. Business Requirements

### BRD-001: Employee Self-Service Transfer Initiation
- **Description:** Eligible employees can browse approved internal job postings (IJP) and submit an official Internal Transfer Request.
- **Data Captured:** Target Business Unit/Department, Target Location, Target Requisition/Position ID, Proposed Effective Date (≥ 30 calendar days from submission, aligned with payroll cycle), and optional statement of interest/reason.
- **Constraints:** Maximum of 1 active transfer request per employee at any given time.

### BRD-002: Automated Policy-Based Eligibility Gate
- **Description:** System enforces strict eligibility pre-checks prior to submission:
  1. **Tenure Requirement:** Minimum 6 months continuous service in the employee's current role and department.
  2. **Performance Rating:** Latest appraisal rating must be ≥ "Meets Expectations" (or equivalent score ≥ 3.0 / 5.0).
  3. **Disciplinary & Performance Standing:** No active Performance Improvement Plan (PIP) or active formal disciplinary warnings on record.
- **Outcome:** Ineligible employees receive clear explanatory feedback; submission is blocked.

### BRD-003: Three-Stage Sequential Approval Workflow
The transfer request traverses a strict 3-tier approval hierarchy:
1. **Stage 1 — Releasing Manager Approval:** Current reporting manager evaluates team impact, project release timeline, and knowledge transfer plan.
2. **Stage 2 — Receiving Manager Approval:** Target department manager confirms candidate technical/role fit and position headcount availability.
3. **Stage 3 — HR Operations Validation:** Central HR verifies compensation grade, statutory compliance, benefits alignment, and gives final operational approval.
- **Rejection Policy:** A rejection at any stage terminates the workflow, records reasons, and notifies the employee.
- **Withdrawal Policy:** The employee may voluntarily withdraw the request at any time prior to final HR Operations approval.

### BRD-004: SLA Management & Automated Escalations
- **Review SLA:** 5 business days allotted for each approval stage.
- **Reminders:** Automated notifications sent at Day 3 of inactivity.
- **Escalation:** If unapproved after Day 5, an auto-escalation alert is dispatched to HR Operations for administrative intervention.

### BRD-005: Downstream Handover & Transition Orchestration
Upon final HR approval and reaching the scheduled effective date, the portal orchestrates downstream operational tasks:
- **HRMS Record Update:** Primary point-of-no-return; updates department, job title, and manager hierarchy.
- **Payroll & Cost Center Alignment:** Synchronizes cost-center codes, tax locations, and pay element changes at the start of the next pay cycle.
- **IT Identity & Access Management:** Adjusts software access, system permissions, and hardware requisitions.
- **Facilities & Workplace Management:** Coordinates physical seat allocation, access badges, and regional asset logistics.
- **Resilience:** If any downstream operational task fails, the transfer is not aborted; an exception ticket is flagged to HR Operations with full diagnostic context.

### BRD-006: Consolidated Transparency & Status Tracking
- Employees and managers have a real-time progress dashboard displaying current status, completed steps, pending action holder (by role, preserving manager confidentiality), and milestone timestamps.
- Reason for transfer is classified as sensitive data and is visible exclusively to HR Operations.

---

## 3. Scope Boundaries

### In-Scope
- Full-time employee-initiated domestic internal transfers.
- Requisition-backed internal mobility applications (IJP).
- 3-tier approval workflow with SLA enforcement.
- Orchestration adapters for HRMS, Payroll, IT Access, and Facilities.
- Role-based audit logging of all state transitions.

### Out-of-Scope (Deferred / Excluded)
- External recruitment and new hire onboarding.
- Position or job requisition creation (managed in ATS/HRMS).
- Cross-border / international transfers requiring immigration/visa handling.
- Contractor, intern, or third-party vendor transitions.
- Management-directed / involuntary reassignments.
