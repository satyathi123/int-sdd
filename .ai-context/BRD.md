# Business Requirements Document (BRD)

## Project Name
Employee Internal Transfer (MobilityHub)

## Document Metadata
- **Version:** 1.2 (Comprehensive Baseline — Gate 0 Revisions Incorporated)
- **Status:** In Review (Pending Gate 0 BRD PR Review Approval)
- **Process Owner:** HR Operations & People Operations Team
- **Product Owner:** One-Point Employee Portal Product Team
- **Technical Author:** Anand Satyarthi (`anand.satyarthi@indusnet.co.in`)
- **Assigned Reviewer (Gate 0 / Gate 1):** Supratim Jetty (`supratim.jetty@intglobal.com`)

---

## 1. Executive Summary & Business Need

Today, an employee requesting an internal department or location transfer must manually coordinate with multiple departments—current reporting manager, receiving department manager, HR business partners, payroll, IT identity & access, and facilities. This decentralized, manual process results in lack of end-to-end visibility, extended turnaround times, handover gaps, compliance risks, and operational errors during workforce transition.

The **Employee Internal Transfer (MobilityHub)** platform digitizes and orchestrates the end-to-end internal transfer journey into a single unified self-service experience on the One-Point Employee Portal.

### Core Business Objectives:
1. **Unified Self-Service Experience:** A single digital portal for employees to discover opportunities, initiate requests, and track real-time progress.
2. **Multi-Category Mobility Support:** Comprehensive support for all transfer categories including Departmental Transfers, Location/Relocation Shifts, Lateral Career Moves, and Project-Driven Reallocations (beyond appraisal-linked transfers).
3. **Automated Policy Governance:** Automated pre-submission eligibility verification to enforce tenure, performance, and disciplinary standards.
4. **Structured Multi-Tier Workflow:** Transparent, SLA-governed 3-stage approval hierarchy (Releasing Manager → Receiving Manager → HR Operations) with automated reminders and escalations.
5. **Resilient Downstream Integration:** Orchestrated automated handoffs to HRMS, Payroll, IT Access, and Facilities systems with HR exception triage.

---

## 2. Comprehensive Transfer Categories & Business Use Cases

The MobilityHub platform supports four distinct internal transfer categories:

### 2.1 Departmental / Cross-Functional Transfers
- **Description:** Employee transitions from their current business unit/department to a different department (e.g., Engineering to Product Management, or Customer Support to Technical Operations).
- **Business Driver:** Career progression, skill diversification, or strategic internal staffing.
- **Workflow Variation:** Requires technical capability assessment by Receiving Manager and resource release planning by Releasing Manager.

### 2.2 Location & Relocation Shifts
- **Description:** Employee shifts physical working location (e.g., Mumbai office to Bengaluru hub, or regional office to corporate headquarters).
- **Business Driver:** Personal relocation request, office consolidation, or regional headcount demand.
- **Workflow Variation:** Triggers regional HR compliance verification, local tax/payroll cost-center realignment, physical seat allocation, and IT asset logistics.

### 2.3 Lateral Career & Role-Based Moves
- **Description:** Employee transitions to a different role or position at the same job grade within the same or different department (e.g., QA Lead to Scrum Master).
- **Business Driver:** Requisition-backed internal job posting (IJP) application or internal mobility program.
- **Workflow Variation:** Requires verification of position requisition ID, grade alignment, and headcount availability in target team.

### 2.4 Project-Driven & Operational Reallocations
- **Description:** Reallocation driven by organizational restructuring, project completion, or strategic resource rebalancing.
- **Business Driver:** Business operational necessity or project redeployment.
- **Workflow Variation:** Accelerated approval workflow with pre-authorized HR Operations endorsement and flexible notice periods.

---

## 3. Detailed Business Requirements

### BRD-001: Employee Self-Service Transfer Initiation
- **Description:** Eligible employees can initiate an official Internal Transfer Request via the One-Point Employee Portal.
- **Transfer Types Supported:** Departmental, Location Shift, Lateral Move, and Project Reallocation.
- **Data Captured:**
  - Transfer Category / Reason Code
  - Target Business Unit / Department
  - Target Physical Location
  - Target Requisition / Position ID (for IJP & Lateral moves)
  - Proposed Effective Date ($\ge 30$ calendar days from submission, aligned with upcoming payroll cycle)
  - Optional Statement of Interest / Justification (classified as sensitive data)
- **Constraints:** Maximum of 1 active (non-terminal) transfer request per employee at any given time.

### BRD-002: Automated Policy-Based Eligibility Gate
- **Description:** System enforces strict eligibility pre-checks prior to request submission:
  1. **Tenure Requirement:** Minimum 6 months continuous service in current role and department (configurable by transfer type).
  2. **Performance Standing:** Latest appraisal rating $\ge$ "Meets Expectations" (or equivalent score $\ge 3.0 / 5.0$).
  3. **Disciplinary & PIP Standing:** No active Performance Improvement Plan (PIP) or active formal disciplinary warnings on record.
  4. **Active Request Constraint:** Zero existing active transfer requests.
- **Outcome:** If pre-check passes, submission is enabled. If pre-check fails, submission is blocked and clear explanatory policy feedback is presented to the employee.

### BRD-003: Three-Stage Sequential Approval Workflow
The transfer request traverses a strict 3-tier approval hierarchy:
1. **Stage 1 — Releasing Manager Approval:** Current reporting manager evaluates team impact, project release timeline, and knowledge transfer plan.
2. **Stage 2 — Receiving Manager Approval:** Target department manager confirms candidate technical/role fit, team capacity, and position headcount availability.
3. **Stage 3 — HR Operations Validation:** Central HR verifies compensation grade, statutory compliance, benefits alignment, and grants final operational approval.
- **Rejection Policy:** Rejection at any stage terminates the workflow, records structured rejection reasons, and notifies the employee.
- **Withdrawal Policy:** Employee may voluntarily withdraw the request at any time prior to final HR Operations approval (point-of-no-return).

### BRD-004: SLA Management & Automated Escalations
- **Review SLA:** 5 business days allotted for each approval stage.
- **Automated Reminders:** Automated notification sent to pending action holder at Day 3 of inactivity.
- **Auto-Escalation:** If unapproved after Day 5, an auto-escalation alert is dispatched to HR Operations for administrative intervention or delegation.

### BRD-005: Downstream Handover & Transition Orchestration
Upon final HR approval and reaching the scheduled effective date, the portal orchestrates downstream operational tasks:
- **HRMS Record Update:** Primary point-of-no-return; updates department, job title, and manager hierarchy.
- **Payroll & Cost Center Alignment:** Synchronizes cost-center codes, tax locations, and pay element changes at the start of the next pay cycle.
- **IT Identity & Access Management:** Adjusts software access, system permissions, and hardware requisitions.
- **Facilities & Workplace Management:** Coordinates physical seat allocation, access badges, and regional asset logistics.
- **Resilience & Exception Triage:** If any downstream task fails, the transfer is not aborted; an exception ticket is flagged to HR Operations with full diagnostic context.

### BRD-006: Consolidated Transparency & Status Tracking
- Employees and managers have a real-time progress dashboard displaying current status, completed steps, pending action holder (by role, preserving manager confidentiality), and milestone timestamps.
- Reason for transfer is classified as sensitive data and is visible exclusively to HR Operations.

---

## 4. Functional Requirements (FRs)

| FR ID | Module / Area | Functional Requirement Description | Related BRD |
|---|---|---|---|
| **FR-001** | Initiation | The system shall provide a multi-step self-service wizard for employees to select transfer category (Departmental, Location, Lateral, Project) and input target attributes. | BRD-001 |
| **FR-002** | Eligibility Engine | The system shall automatically query HRMS and Appraisal databases to validate tenure ($\ge 6$ months), performance ($\ge 3.0$), and PIP status before allowing submission. | BRD-002 |
| **FR-003** | Workflow Engine | The system shall route submitted requests through sequential 3-tier approvals (Releasing Mgr $\rightarrow$ Receiving Mgr $\rightarrow$ HR Ops) and maintain state immutability. | BRD-003 |
| **FR-004** | SLA Engine | The system shall track 5-day SLAs per stage, trigger Day 3 inactivity reminders, and fire Day 5 auto-escalations to HR Operations. | BRD-004 |
| **FR-005** | Decision Capture | The system shall capture approver decisions (Approve / Reject) with mandatory comments on rejection, updating the status in real time. | BRD-003 |
| **FR-006** | Downstream Integration | The system shall execute asynchronous handoffs to HRMS, Payroll, IT IAM, and Facilities upon reaching the effective date, tracking task completion states. | BRD-005 |
| **FR-007** | Dashboard & Visibility | The system shall render a real-time tracking dashboard showing stage progress and pending roles while masking approver personal details from employees. | BRD-006 |
| **FR-008** | Withdrawal & Cancellation | The system shall allow employees to withdraw requests prior to HR approval and permit HR Admins to cancel requests before HRMS execution. | BRD-003 |
| **FR-009** | Exception Triage | The system shall catch downstream task failures, flag remediation alerts to HR Operations, and prevent unauthorized request rollbacks post-HRMS update. | BRD-005 |

---

## 5. Non-Functional Requirements (NFRs)

### 5.1 Performance & Responsiveness (NFR-001)
- **API Response Time:** $95\%$ of API requests must complete in $< 500$ ms under normal load.
- **Dashboard Load Time:** Self-service portal dashboard must render in $< 1.5$ seconds.
- **Batch SLA Processing:** Scheduled SLA evaluation and downstream task dispatch jobs must complete within 15 minutes of trigger time.

### 5.2 Security, Privacy & PII Protection (NFR-002)
- **Role-Based Access Control (RBAC):** Strict enforcement of Employee, Releasing Manager, Receiving Manager, HR BP, HR Admin, and Downstream Fulfiller roles.
- **Data Confidentiality:** Transfer statement of interest / reason is classified as sensitive data and MUST be visible exclusively to HR Operations.
- **PII Protection in Logs:** Application logs MUST NOT log employee names, email addresses, salary data, performance ratings, or reason text.
- **Encryption Standards:** Data in transit protected via TLS 1.3; data at rest encrypted using AES-256.

### 5.3 Availability & Scalability (NFR-003)
- **System Availability:** Service uptime target of $99.9\%$ during core business hours.
- **Scalability:** System architecture must support up to 10,000 active employees with peak concurrent load of 500 active sessions.

### 5.4 Auditability & Compliance (NFR-004)
- **Immutable Audit Trail:** Every state transition, approver decision, withdrawal, and downstream trigger MUST write an append-only audit log entry capturing timestamp, actor reference, source state, target state, and correlation ID.
- **Retention:** Audit records retained for a minimum of 7 years for statutory compliance.

### 5.5 Resilience & Interoperability (NFR-005)
- **Fault Tolerance:** Downstream adapter failures MUST NOT corrupt request state or crash the portal workflow.
- **Idempotency:** State-changing API endpoints MUST require `Idempotency-Key` headers to prevent duplicate processing on retries.

---

## 6. Business Assumptions, External Dependencies & Constraints

### 6.1 Business Assumptions
1. Employee organizational structures, reporting lines, and tenure records are accurately maintained in the primary HRMS.
2. Target job positions and vacancy headcount are published and maintained in the ATS / Internal Job Posting portal.
3. Approvers have access to the One-Point Employee Portal or corporate email for SLA notifications.

### 6.2 External System Dependencies
1. **HRMS Core API:** Source of employee profile, tenure, manager hierarchy, and target for organizational updates.
2. **Appraisal & Performance System:** Source of latest performance ratings and PIP / disciplinary standing.
3. **Payroll & Cost Center Service:** Destination for cost-center alignment and tax location updates.
4. **IT IAM & ITSM System:** Destination for user account provisioning and access reclamation.
5. **Facilities Management System:** Destination for physical workspace and badge allocation.

### 6.3 Business & System Constraints
1. **Single Request Constraint:** An employee cannot have more than 1 active transfer request concurrently.
2. **Notice Period Constraint:** Effective date must be at least 30 calendar days after submission and aligned with payroll boundaries.
3. **Point of No Return:** Once the effective date is reached and HRMS record update begins, the request cannot be cancelled or withdrawn via self-service.

---

## 7. Scope Boundaries

### In-Scope
- Full-time employee-initiated domestic internal transfers across all categories (Departmental, Location, Lateral, Project).
- Automated pre-submission eligibility pre-checks.
- 3-tier sequential approval workflow with 5-day SLAs and auto-escalations.
- Orchestration adapters for HRMS, Payroll, IT Access, and Facilities systems.
- Append-only audit logging and real-time dashboard status tracking.

### Out-of-Scope (Deferred / Excluded)
- External candidate recruitment and new hire onboarding.
- Creation or modification of job requisitions (managed in ATS/HRMS).
- Cross-border / international transfers requiring immigration, visa, or multi-tax entity handling.
- Third-party contractors, interns, or vendor staff transitions.
- Involuntary / disciplinary management reassignments.

---

## 8. Revision & Approval History

| Version | Date | Author / Reviewer | Changes / Comments |
|---|---|---|---|
| 1.0 | 2026-09-17 | Anand Satyarthi | Initial baseline baseline setup. |
| 1.1 | 2026-09-17 | Anand Satyarthi | Customized baseline with 3-tier approvals, eligibility rules, and downstream orchestration. |
| 1.2 | 2026-09-26 | Anand Satyarthi / Supratim Jetty | Comprehensive revision addressing Gate 0 PR review (`GATE0-brd-baseline-20260925-182500.md`): Added explicit Functional Requirements (FR-001–FR-009), Non-Functional Requirements (NFR-001–NFR-005), Assumptions/Dependencies/Constraints, and expanded Transfer Categories (Departmental, Location, Lateral, Project). Resubmitted for Gate 0 approval. |
