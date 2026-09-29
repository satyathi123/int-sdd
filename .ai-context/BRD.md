# Business Requirements Document (BRD)

## Project Name
Employee Internal Transfer (MobilityHub)

## Document Metadata
- **Version:** 1.3 (Comprehensive Baseline — RBAC & Super Admin Governance Incorporated)
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
6. **Centralized Administration & Governance:** Robust Role-Based Access Control (RBAC) and Super Admin workflow capabilities for system configuration, delegation, and operational oversight.

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
- **Auto-Escalation:** If unapproved after Day 5, an auto-escalation alert is dispatched to HR Operations and Super Admin for administrative intervention or delegation.

### BRD-005: Downstream Handover & Transition Orchestration
Upon final HR approval and reaching the scheduled effective date, the portal orchestrates downstream operational tasks:
- **HRMS Record Update:** Primary point-of-no-return; updates department, job title, and manager hierarchy.
- **Payroll & Cost Center Alignment:** Synchronizes cost-center codes, tax locations, and pay element changes at the start of the next pay cycle.
- **IT Identity & Access Management:** Adjusts software access, system permissions, and hardware requisitions.
- **Facilities & Workplace Management:** Coordinates physical seat allocation, access badges, and regional asset logistics.
- **Resilience & Exception Triage:** If any downstream task fails, the transfer is not aborted; an exception ticket is flagged to HR Operations with full diagnostic context.

### BRD-006: Consolidated Transparency & Status Tracking
- Employees and managers have a real-time progress dashboard displaying current status, completed steps, pending action holder (by role, preserving manager confidentiality), and milestone timestamps.
- Reason for transfer is classified as sensitive data and is visible exclusively to HR Operations and Super Admin.

---

## 4. Role-Based Access Control (RBAC) Architecture & Access Matrix

### 4.1 User Roles & Definitions

The MobilityHub platform enforces fine-grained Role-Based Access Control (RBAC) across 7 distinct user personas:

1. **`EMPLOYEE` (Request Subject / Initiator):**
   - Full-time employees browsing internal opportunities, initiating transfer requests, tracking their request progress, and withdrawing active requests prior to the point-of-no-return.
2. **`RELEASING_MANAGER` (Current Reporting Manager):**
   - Current hierarchical manager of the employee. Authorized to review release details, evaluate team release timelines, and approve/reject Stage 1 requests for direct reports. Restricted from viewing sensitive reason text.
3. **`RECEIVING_MANAGER` (Target Department Manager):**
   - Target manager owning the destination position/headcount. Authorized to evaluate candidate role fit, confirm position availability, and approve/reject Stage 2 requests. Restricted from viewing sensitive reason text.
4. **`HR_BUSINESS_PARTNER` (HRBP):**
   - Departmental HR representative authorized to view all departmental requests, inspect sensitive reason fields, validate policy compliance, and participate in HR reviews.
5. **`HR_OPERATIONS_ADMIN` (Central HR Operations):**
   - Central HR administrator authorized to perform final Stage 3 validation, override stalled SLAs, reassign pending approvers, handle downstream exception tickets, and cancel requests prior to HRMS update.
6. **`SUPER_ADMIN` (System & Platform Administrator):**
   - Global administrative authority possessing complete system oversight. Authorized to configure system parameters, manage RBAC roles, delegate approver authority, trigger manual workflow overrides, access system-wide audit logs, and monitor integration health.
7. **`DOWNSTREAM_FULFILLERS` (Payroll, IT IAM, Facilities Operations):**
   - Functional fulfillment operators (or system service accounts) authorized to view and update specific assigned execution tasks only. Restricted from viewing approval history or sensitive employee reasons.

---

### 4.2 RBAC Permissions Matrix

| Operation / Feature | `EMPLOYEE` | `RELEASING_MGR` | `RECEIVING_MGR` | `HRBP` | `HR_OPS_ADMIN` | `SUPER_ADMIN` | `FULFILLERS` |
|---|---|---|---|---|---|---|---|
| **Browse IJPs / Vacancies** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **Initiate Draft Request** | ✅ (Self) | ❌ | ❌ | ❌ | ❌ | ✅ (On-Behalf) | ❌ |
| **Submit Request** | ✅ (Self) | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **View Own Request Status** | ✅ (Self) | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **Stage 1 Decision (Release)** | ❌ | ✅ (Directs) | ❌ | ❌ | ❌ | ✅ (Override) | ❌ |
| **Stage 2 Decision (Accept)** | ❌ | ❌ | ✅ (Target) | ❌ | ❌ | ✅ (Override) | ❌ |
| **Stage 3 Decision (HR Ops)** | ❌ | ❌ | ❌ | ✅ (Review) | ✅ (Final) | ✅ (Override) | ❌ |
| **View Sensitive Reason Field**| ✅ (Self) | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ |
| **Withdraw Active Request** | ✅ (Pre-HR) | ❌ | ❌ | ❌ | ❌ | ✅ (Override) | ❌ |
| **Cancel Request (Admin)** | ❌ | ❌ | ❌ | ❌ | ✅ (Pre-HRMS) | ✅ (Pre-HRMS) | ❌ |
| **Reassign Pending Approver** | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ |
| **Manage Downstream Tasks** | ❌ | ❌ | ❌ | ❌ | ✅ (Exception) | ✅ | ✅ (Own Task) |
| **Configure System Policies** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| **View System Audit Logs** | ❌ | ❌ | ❌ | ❌ | ✅ (Dept) | ✅ (Global) | ❌ |

---

## 5. Super Admin Workflow & Operational Capabilities

### 5.1 Super Admin Persona & Authority Scope
The **Super Admin** role represents the highest level of administrative governance within the MobilityHub platform. Super Admins possess full platform visibility and intervention authority, ensuring continuous operational flow, system adaptability, security compliance, and emergency fault resolution.

---

### 5.2 Complete Super Admin End-to-End Workflow

```text
                                 ┌──────────────────────────────────────────────┐
                                 │       SUPER ADMIN GOVERNANCE PORTAL          │
                                 └──────────────────────┬───────────────────────┘
                                                        │
         ┌──────────────────────────────┬───────────────┴───────────────┬──────────────────────────────┐
         ▼                              ▼                               ▼                              ▼
┌─────────────────┐           ┌──────────────────┐            ┌───────────────────┐          ┌───────────────────┐
│ 1. SYSTEM POLICY│           │ 2. USER & ROLE   │            │ 3. WORKFLOW       │          │ 4. INTEGRATION &  │
│   CONFIGURATION │           │   GOVERNANCE     │            │   OVERRIDE        │          │   AUDIT LOGGING   │
└────────┬────────┘           └────────┬─────────┘            └─────────┬─────────┘          └─────────┬─────────┘
         │                             │                                │                              │
  - SLA Days (5d)               - RBAC Assignment                - Reassign Approver            - View Audit Logs
  - Notice Period (30d)         - Temporary Delegation           - SLA Auto-Escalation          - Retry Failed Task
  - Tenure (6m)                 - Proxy Management               - Pre-HRMS Cancellation        - Telemetry Health
  - Rating (3.0+)               - Security Scopes                - Manual State Reset           - PII Inspection
```

---

### 5.3 Core Super Admin Workflow Modules

#### Module 1: System Parameter & Policy Configuration
- **SLA Parameters:** Configure global approval SLA thresholds (default: 5 business days), reminder intervals (default: Day 3), and escalation targets.
- **Notice Period Rules:** Configure minimum effective date notice periods per transfer category (default: $\ge 30$ calendar days).
- **Eligibility Policy Rules:** Adjust pre-check policy parameters including minimum tenure (default: 6 months), performance rating threshold (default: $\ge 3.0/5.0$), and PIP lookback windows.

#### Module 2: User Role & Approver Delegation Governance
- **Role Assignment:** Assign and modify user permissions across all 7 RBAC roles.
- **Approver Delegation Management:** Configure temporary approver delegation / proxy authority for managers on official leave, long-term absence, or organizational transition.
- **On-Behalf Initiation:** Capability to initiate or assist transfer submissions on behalf of employees during exceptional HR circumstances.

#### Module 3: Global Workflow Override & Operational Interventions
- **Approver Reassignment:** Dynamically reassign a stuck or inactive approval stage to an alternate manager or HR BP.
- **SLA Escalation Intervention:** Intervene in auto-escalated requests (Day 5+) to grant administrative approval or return for revision.
- **Administrative Cancellation:** Cancel active requests prior to the point-of-no-return (HRMS execution) with mandatory administrative justification.
- **State Reset & Recovery:** Reset requests stuck in transient network/system errors back to the nearest valid workflow state.

#### Module 4: Downstream Integration & Exception Management
- **Failed Task Retry & Remediation:** Inspect failed downstream adapter tasks (Payroll, IT, Facilities), manually retry adapter calls, or mark tasks as manually fulfilled with resolution notes.
- **Integration Health Telemetry:** Monitor real-time status, error rates, and API response latencies for downstream HRMS, Payroll, IAM, and Facilities integrations.

#### Module 5: Security Audit & System Observability
- **Global Audit Log Access:** Search, filter, and inspect the immutable append-only audit trail capturing every state transition, approver decision, delegation, and override event across the entire system.
- **PII Compliance Verification:** Audit system logs to ensure strict compliance with PII privacy rules (zero PII in logs).

---

## 6. Functional Requirements (FRs)

| FR ID | Module / Area | Functional Requirement Description | Related BRD |
|---|---|---|---|
| **FR-001** | Initiation | The system shall provide a multi-step self-service wizard for employees to select transfer category (Departmental, Location, Lateral, Project) and input target attributes. | BRD-001 |
| **FR-002** | Eligibility Engine | The system shall automatically query HRMS and Appraisal databases to validate tenure ($\ge 6$ months), performance ($\ge 3.0$), and PIP status before allowing submission. | BRD-002 |
| **FR-003** | Workflow Engine | The system shall route submitted requests through sequential 3-tier approvals (Releasing Mgr $\rightarrow$ Receiving Mgr $\rightarrow$ HR Ops) and maintain state immutability. | BRD-003 |
| **FR-004** | SLA Engine | The system shall track 5-day SLAs per stage, trigger Day 3 inactivity reminders, and fire Day 5 auto-escalations to HR Operations and Super Admin. | BRD-004 |
| **FR-005** | Decision Capture | The system shall capture approver decisions (Approve / Reject) with mandatory comments on rejection, updating the status in real time. | BRD-003 |
| **FR-006** | Downstream Integration | The system shall execute asynchronous handoffs to HRMS, Payroll, IT IAM, and Facilities upon reaching the effective date, tracking task completion states. | BRD-005 |
| **FR-007** | Dashboard & Visibility | The system shall render a real-time tracking dashboard showing stage progress and pending roles while masking approver personal details from employees. | BRD-006 |
| **FR-008** | Withdrawal & Cancellation | The system shall allow employees to withdraw requests prior to HR approval and permit HR Admins / Super Admins to cancel requests before HRMS execution. | BRD-003 |
| **FR-009** | Exception Triage | The system shall catch downstream task failures, flag remediation alerts to HR Operations and Super Admin, and prevent unauthorized request rollbacks post-HRMS update. | BRD-005 |
| **FR-010** | RBAC Enforcement | The system shall strictly enforce role-based access rules across all API endpoints and UI components per the RBAC Permissions Matrix. | BRD-001–006 |
| **FR-011** | Super Admin Management | The system shall provide Super Admins with dedicated UI portals for system parameter configuration, approver delegation, workflow overrides, task retries, and audit log inspection. | BRD-001–006 |

---

## 7. Non-Functional Requirements (NFRs)

### 7.1 Performance & Responsiveness (NFR-001)
- **API Response Time:** $95\%$ of API requests must complete in $< 500$ ms under normal load.
- **Dashboard Load Time:** Self-service portal dashboard must render in $< 1.5$ seconds.
- **Batch SLA Processing:** Scheduled SLA evaluation and downstream task dispatch jobs must complete within 15 minutes of trigger time.

### 7.2 Security, Privacy & PII Protection (NFR-002)
- **Role-Based Access Control (RBAC):** Strict enforcement of 7 defined roles per §4.
- **Data Confidentiality:** Transfer statement of interest / reason is classified as sensitive data and MUST be visible exclusively to HR Operations and Super Admin.
- **PII Protection in Logs:** Application logs MUST NOT log employee names, email addresses, salary data, performance ratings, or reason text.
- **Encryption Standards:** Data in transit protected via TLS 1.3; data at rest encrypted using AES-256.

### 7.3 Availability & Scalability (NFR-003)
- **System Availability:** Service uptime target of $99.9\%$ during core business hours.
- **Scalability:** System architecture must support up to 10,000 active employees with peak concurrent load of 500 active sessions.

### 7.4 Auditability & Compliance (NFR-004)
- **Immutable Audit Trail:** Every state transition, approver decision, withdrawal, delegation, and Super Admin override MUST write an append-only audit log entry capturing timestamp, actor reference, source state, target state, and correlation ID.
- **Retention:** Audit records retained for a minimum of 7 years for statutory compliance.

### 7.5 Resilience & Interoperability (NFR-005)
- **Fault Tolerance:** Downstream adapter failures MUST NOT corrupt request state or crash the portal workflow.
- **Idempotency:** State-changing API endpoints MUST require `Idempotency-Key` headers to prevent duplicate processing on retries.

---

## 8. Business Assumptions, External Dependencies & Constraints

### 8.1 Business Assumptions
1. Employee organizational structures, reporting lines, and tenure records are accurately maintained in the primary HRMS.
2. Target job positions and vacancy headcount are published and maintained in the ATS / Internal Job Posting portal.
3. Approvers have access to the One-Point Employee Portal or corporate email for SLA notifications.

### 8.2 External System Dependencies
1. **HRMS Core API:** Source of employee profile, tenure, manager hierarchy, and target for organizational updates.
2. **Appraisal & Performance System:** Source of latest performance ratings and PIP / disciplinary standing.
3. **Payroll & Cost Center Service:** Destination for cost-center alignment and tax location updates.
4. **IT IAM & ITSM System:** Destination for user account provisioning and access reclamation.
5. **Facilities Management System:** Destination for physical workspace and badge allocation.

### 8.3 Business & System Constraints
1. **Single Request Constraint:** An employee cannot have more than 1 active transfer request concurrently.
2. **Notice Period Constraint:** Effective date must be at least 30 calendar days after submission and aligned with payroll boundaries.
3. **Point of No Return:** Once the effective date is reached and HRMS record update begins, the request cannot be cancelled or withdrawn via self-service.

---

## 9. Scope Boundaries

### In-Scope
- Full-time employee-initiated domestic internal transfers across all categories (Departmental, Location, Lateral, Project).
- Automated pre-submission eligibility pre-checks.
- 3-tier sequential approval workflow with 5-day SLAs and auto-escalations.
- Complete Role-Based Access Control (RBAC) across 7 user roles.
- Dedicated Super Admin workflow, parameter configuration, approver delegation, and workflow override modules.
- Orchestration adapters for HRMS, Payroll, IT Access, and Facilities systems.
- Append-only audit logging and real-time dashboard status tracking.

### Out-of-Scope (Deferred / Excluded)
- External candidate recruitment and new hire onboarding.
- Creation or modification of job requisitions (managed in ATS/HRMS).
- Cross-border / international transfers requiring immigration, visa, or multi-tax entity handling.
- Third-party contractors, interns, or vendor staff transitions.
- Involuntary / disciplinary management reassignments.

---

## 10. Revision & Approval History

| Version | Date | Author / Reviewer | Changes / Comments |
|---|---|---|---|
| 1.0 | 2026-09-17 | Anand Satyarthi | Initial baseline baseline setup. |
| 1.1 | 2026-09-17 | Anand Satyarthi | Customized baseline with 3-tier approvals, eligibility rules, and downstream orchestration. |
| 1.2 | 2026-09-26 | Anand Satyarthi / Supratim Jetty | Comprehensive revision addressing Gate 0 PR review (`GATE0-brd-baseline-20260925-182500.md`): Added explicit Functional Requirements (FR-001–FR-009), Non-Functional Requirements (NFR-001–NFR-005), Assumptions/Dependencies/Constraints, and expanded Transfer Categories (Departmental, Location, Lateral, Project). |
| 1.3 | 2026-09-29 | Anand Satyarthi / Supratim Jetty | Comprehensive revision incorporating Role-Based Access Control (RBAC) architecture & access matrix (§4) across 7 user roles, and Super Admin workflow, policy configuration, delegation, and override governance (§5). Resubmitted for Gate 0 approval. |
