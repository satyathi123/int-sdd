---
name: int-reverse-engine-workflow
description: Reverse engineer an existing, ongoing, or legacy codebase to generate authoritative .ai-context/BRD.md and .ai-context/assumptions.md baselines for Gate 0 PR review approval.
---

# INT Reverse Engineering Workflow

## 1. Purpose

This workflow is responsible for analyzing an **existing, ongoing, or legacy project** (`src/`, `database/`, `migrations/`, `routes/`, `controllers/`, `services/`, `config/`, `tests/`) and reverse-engineering its current implementation into an SDD-ready state.

It MUST:

- Analyze existing codebase, folder structure, APIs, routes, controllers, services, database models, schemas, and configurations
- Extract functional requirements, non-functional constraints, user roles, business rules, dependencies, and open questions
- Perform Source & Evidence Classification (`Confirmed Code`, `Confirmed DB`, `Inferred`, `Unknown / Human Confirmation Required`)
- Generate or update `.ai-context/BRD.md` with all mandatory sections (`Objectives`, `Scope`, `Actors`, `Functional Requirements`, `NFRs`, `Business Rules`, `Dependencies`, `Assumptions`, `Out of Scope`, `Open Questions`, `Acceptance Criteria`)
- Generate or update `.ai-context/assumptions.md` with technical, business, integration, and environmental uncertainties
- Hand over to existing BRD Ingestion (`int-brd-ingestion`) and log changes in `.ai-context/decisions/brd-change-log.md`
- Trigger **Gate 0 BRD PR Review** requiring the assigned reviewer to answer all pending open questions before approval
- Stop before business architecture modifications or feature code implementation

> [!CRITICAL]
> **Strict Non-Destructive Boundary**:
> - This workflow NEVER modifies, deletes, or refactors existing application code (`src/`), test code (`tests/`), or database schema files.
> - This workflow NEVER modifies `int-project-setup` or alters clean project initialization logic.
> - Reused BRD generation and ingestion mechanisms remain the standard processing pipeline.

---

# 2. Prerequisites & Trigger Condition

Use this workflow when:

- Bringing an existing, ongoing, or legacy codebase into the INT SDD ecosystem.
- `BRD.md` or `assumptions.md` is missing, incomplete, or outdated relative to actual implementation.
- Executed via slash command `/int-reverse-engine-workflow` or context menu.

---

# 3. Two-Tier Minimum-Token Scan Protocol

To minimize token usage and prevent context window exhaustion on large repositories:

```text
┌────────────────────────────────────────────────────────────────────────┐
│ TIER 1: Programmatic Deterministic Pre-Scan (0 LLM Tokens)             │
│ - Local file inspection (list_dir, grep_search)                         │
│ - Excludes: node_modules/, dist/, build/, coverage/, .git/, *.min.*,  │
│   binary assets, and lockfiles                                         │
│ - Extracts AST symbols: API routes, controller methods, DB models,      │
│   schema tables, package dependencies, environment keys                │
│ - Generates compact Repository Inventory Summary                       │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ TIER 2: Targeted LLM Semantic Extraction (~3,000 LLM Tokens)           │
│ - Receives ONLY Tier 1 Summary + primary entry points                  │
│ - Extracts business intent, business rules, actors, and workflows       │
│ - Populates mandatory BRD.md & assumptions.md sections                 │
└────────────────────────────────────────────────────────────────────────┘
```

---

# 4. Source & Evidence Classification Matrix

Every extracted requirement and assumption MUST be classified by evidence source:

| Classification | Definition & Evidence Criteria | Target Artifact |
|---|---|---|
| **Confirmed from Code** | Directly verified in active controllers, routes, or services in `src/`. | `.ai-context/BRD.md` |
| **Confirmed from Database** | Directly verified in active migrations, SQL DDL, or ORM models. | `.ai-context/BRD.md` |
| **Confirmed from APIs/Tests** | Verified via OpenAPI contracts or passing automated tests in `tests/`. | `.ai-context/BRD.md` |
| **Inferred from Implementation** | Derived from multi-source code patterns or naming conventions. | `.ai-context/BRD.md` (Flagged) |
| **Unknown / Human Confirmation Required** | Ambiguous, unconfirmed, or hardcoded parameters requiring human decision. | `.ai-context/assumptions.md` |

---

# 5. Reverse Engineering Execution Flow

```text
Existing Codebase & DB Structure
       │
       ▼
TIER 1: Deterministic Inventory Scan (0 LLM Tokens)
       │
       ▼
TIER 2: Evidence Classifier & Semantic Extraction
       │
       ▼
Generate / Update .ai-context/BRD.md & .ai-context/assumptions.md
       │
       ▼
Handover to Existing BRD Ingestion (int-brd-ingestion)
       │
       ▼
Gate 0 BRD PR Review (Reviewer Answers Open Questions)
       │
       ▼
SDD-Ready Baseline Approved
```

### Step-by-Step Execution:

1. **Deterministic Inventory Scan**:
   - Inspect folder structure using `list_dir` and `grep_search`.
   - Exclude non-source files (`node_modules/`, `dist/`, `build/`, `.git/`, lockfiles, binaries).
   - Identify frontend routes, pages, components.
   - Identify backend routes, controllers, services, middleware, ORM models, SQL scripts.
   - Identify environment configs (`.env.example`), package dependencies, integration contracts.

2. **Semantic Requirements Synthesis**:
   - Map routes and controller endpoints to candidate business domains and functional requirement IDs (`BRD-001`, `BRD-002`).
   - Extract measurable Non-Functional Requirements (NFRs) from configs and system setup.
   - Extract mandatory Dependencies (databases, external APIs, cloud services).
   - Extract mandatory Out-of-Scope boundaries based on missing or un-implemented capabilities.

3. **Baseline File Generation**:
   - Write `.ai-context/BRD.md` with all 11 mandatory sections.
   - Write `.ai-context/assumptions.md` recording technical/business uncertainties and unconfirmed items.
   - Log baseline entry in `.ai-context/decisions/brd-change-log.md`.

4. **Gate 0 BRD PR Review Handoff**:
   - Set BRD status to `Pending Review`.
   - Present `.ai-context/BRD.md` and `.ai-context/assumptions.md` for Gate 0 PR review.
   - The assigned PM/TL reviewer MUST review and provide explicit answers to all pending items listed under `Open Questions` during review.
   - Reviewer answers are recorded in `.ai-context/pr_reviews/BRD-<timestamp>.md` and updated into `.ai-context/BRD.md` before approval.
   - **HALT & END TURN** waiting for explicit Gate 0 PR review approval.

---

# 6. Compatibility & Consolidation Rules

- **Integration Target**: Consolidates exclusively with existing BRD Generation / Ingestion (`int-brd-ingestion` and `int-project-from-brd`).
- **No Parallel System**: Reuses existing BRD structure, naming conventions, metadata, and Gate 0 review pipeline.
- **Unrelated Workflows**: Spec Generation, Plan, Tasks, Test Cases, Release Management, and `int-project-resume` remain 100% separate and unmodified.

---

# 7. Stop Conditions

STOP after completing baseline generation and presenting for Gate 0 PR Review.

Do NOT:

- Modify existing application code (`src/`).
- Modify existing test suites (`tests/`).
- Draft feature specs (`.spec.md`).
- Generate plans (`.plan.md`) or tasks (`.tasks.md`).
- Approve Gate 0 automatically.
