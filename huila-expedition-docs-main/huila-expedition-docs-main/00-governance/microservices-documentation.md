# Per-Module Documentation Standard - HUILA TRAVEL EXPEDITION

> Defines exactly what documents each platform subsystem/module must have, who writes them,
> when they are created, and when they must be updated. Non-compliance blocks the merge.

---

## Required structure for each subsystem module

As defined in the SRS, HUILA TRAVEL EXPEDITION is developed as a modular monolith using Laravel 10+. Each major logic subsystem resides in its corresponding directory or module space and MUST have:

09-microservices/modules/NN-subsystem-name/├── README.md          REQUIRED from Sprint 1├── data-model.md      REQUIRED before creating MySQL migrations├── decisions.md       RECOMMENDED — internal technical decisions of the module└── runbook.md         REQUIRED before first deploy to local/staging instance
And its API contract in:
07-api/contracts/openapi/travel-platform.yaml    REQUIRED as it exposes central REST endpoints
---

## README.md — Subsystem technical sheet

**When to create it:** At the start of the sprint where the module logic is created.  
**Owner:** Developer assigned to the subsystem module.  
**Update when:** Scope, routing parameters, or internal Eloquent dependencies change.  

Minimum content (use `09-microservices/_template/module/README.md`):

| Section | What it must say |
|---------|-----------------|
| Responsibility | One sentence: what the subsystem does and what data models it authoritatively handles. |
| Architecture location | Associated Laravel Controllers, Models, and MySQL relational tables. |
| Responsibilities (what it DOES) | List of concrete functional requirements (e.g., Subsystem 3: Agency registration and RNT verification). |
| Out of scope (what it does NOT do) | What it delegated (e.g., the Booking module delegates financial transactions to Wompi/PayU APIs). |
| How to run locally | Exact artisan commands and local environment variables (`.env` configurations). |
| Related documents | Links to the other files and views of the subsystem. |

---

## data-model.md — Subsystem data model

**When to create it:** Before executing the first Laravel database migration script.  
**Owner:** Developer assigned to the subsystem.  
**Update when:** A MySQL table structure or model relationship is modified.  

Minimum content:
- ER diagram (Mermaid) of the subsystem's specific tables (Agencias, Servicios, Calendarios, Reservas, Pagos, Reseñas).
- Description of each table with its columns, types, constraints, and relational integrity.
- Justification of the central DB engine (MySQL 8.0 with InnoDB to ensure ACID transactions for booking concurrency).
- Migration strategy (Laravel native migrations).

**Rule:** A field whose reason for existing is not obvious (like tracking timestamps for Law 1581 compliance) MUST have a comment in the diagram.

---

## decisions.md — Subsystem technical decisions

**When to create it:** When the team makes a non-obvious technical decision about the specific module.  
**Owner:** Whoever made the technical choice.  
**Update when:** A new decision is made or a previous one is revoked.  

Recommended format: miniADR (without the full rigor of an architecture ADR):
```markdown
### Decision: [short name]
**Date:** [date]
**Context:** [what problem was being solved, e.g., using Laravel native validation rules for RNT fields]
**Decision:** [what was decided]
**Consequences:** [known trade-offs]
```

---

## runbook.md — Subsystem operations manual

**When to create it:** Before the first deployment to the execution instance.  
**Owner:** Responsible developer (Luisa, Juan, Natalia, or Manuel).  
**Update when:** A new operational issue is discovered or a local environment procedure changes.  

Minimum content:
- How to verify the subsystem is healthy (Laravel health logs, checking connection to port 3306 for MySQL).
- Known symptoms and their causes: "If you see a 500 error on booking submit, check the CSRF token validation or SMTP mail credentials."
- How to roll back database migrations (`php artisan migrate:rollback`).
- How to run seeders for sample Huila travel plans (`php artisan db:seed`).

---

## OpenAPI Contract

**When to create it:** Before implementing the platform's first API endpoint (API-First rule).  
**Owner:** Developer assigned to the module REST logic.  
**Update when:** An endpoint (e.g., searching plans by municipality, updating agency info) is added or modified.  

**API-First Rule:** The contract is written BEFORE the route code. Contract schemas validate that the Laravel API responses fulfill the contract, not the other way around.

---

## How to add a new logic subsystem

1. Copy `09-microservices/_template/module/` → `09-microservices/modules/NN-name/`
2. Update the system catalog with the new module entry.
3. Update the data relationship map.
4. Update the central OpenAPI documentation contract at `07-api/contracts/openapi/travel-platform.yaml`.
5. Create a PR with at least the subsystem README.md and the sketched API definitions.

---

## Correlations

- Subsystem templates → `09-microservices/_template/module/`
- API contracts → `07-api/contracts/openapi/`
- General documentation rules → `00-governance/documentation-rules.md`
