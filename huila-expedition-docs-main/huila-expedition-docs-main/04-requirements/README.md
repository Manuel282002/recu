# 04 — Requirements — Huila Travel Expedition

This module centralizes the formal requirements specification for the **Huila Travel Expedition** platform. It structures what the system must do (Functional Requirements) and how well it must do it (Non-Functional Requirements), serving as the foundational contract for the development team and the instructor's validation (Section 4).

---

## Why this section exists

Requirements are the technical contract between the project team and stakeholders. In this system:
- They establish a clear baseline to verify that all 20 core functionalities from the SRS are successfully met.
- They set success criteria for PHPUnit implementation and manual interface validations.
- They ensure that quality metrics remain stable under baseline hosting constraints (1 vCPU, 1 GB RAM).

---

## Types of requirements

### Functional (FR)
Describe **what the system does**: user roles behaviors, catalog updates, and transactional booking status mutations.
*Example: "The system must allow travel agencies to register and validate their National Tourism Register (RNT) code."*

### Non-functional (NFR)
Describe **how it does it**: performance constraints, usability rules, responsive screen adaptation, and legal compliance.
*Example: "The main page and tour plans details must load in maximum 3 seconds under normal traffic conditions."*

---

## What is here and how to fill it in

### `functional.md` ⭐
A numbered specification of all the system's functional requirements from the SRS.
**Content:** Mapped directly across our three core subsystems: Administration (`RF15`, `RF16`), Agencias (`RF1`, `RF3`, `RF4`, `RF5`, `RF8`, `RF9`, `RF11`), and Turistas (`RF6`, `RF7`, `RF10`, `RF12`, `RF13`, `RF14`, `RF17`, `RF18`, `RF19`, `RF20`).

### `non-functional.md` ⭐
Quality, performance, security, and technical infrastructure constraints.
**Content:** Documented metrics according to SRS guidelines, detailing the 3-second maximum page load (`RNF1`), 500 KB auto-image compression (`RNF2`), 30-50 concurrent users target (`RNF3`), and secure `bcrypt` credential hashing (`RNF8`).

### `user-stories.md`
Formalized product backlog stories for sprint allocation.
**Content:** Detailed agile cards utilizing the *As/I want/So that* format, backed by verifiable acceptance criteria written in Gherkin syntax (such as `HU-AGENCY-001` for online agency registration).

### `traceability-matrix.md` ⭐
The relational grid linking user stories, requirements, and test scripts.
**Content:** An end-to-end matrix mapping requirements to their technical verification components within the Laravel layout, proving 100% requirement coverage.

### `_template-hu.md`
Standardized team template for creating uniform User Stories with Gherkin scenarios.

### `_template-nfr.md`
Standardized team template for defining measurable quality metrics and verification tools.

---

## Correlations with other sections

| This section feeds... | Why |
|-----------------------|-----|
| `05-architecture/` | Latency, data consistency, and hosting constraints guide our Laravel monolithic modular layout. |
| `06-data/models.md` | Required database structures, data dictionaries, and field nullabilities are derived from FRs. |
| `07-api/` | Functional requirements define the route endpoints, parameters, and payloads exposed by the system. |

---

## Common mistakes to avoid

❌ **"The platform must be fast"** → Not measurable. We use: "Main pages must load in less than 3 seconds (RNF1)."

❌ **"The system must be secure"** → Too vague. We use: "Mandatory HTTPS traffic with active SSL and password encription via Laravel's native bcrypt hashing (RNF7, RNF8)."

❌ Writing requirements that limit code structure instead of defining the core business problem.

---

## Questions this section answers

- **What must the system do for each type of user?** It allows travelers to filter and request bookings, agencies to manage regional tour package calendars, and administrators to verify RNT certificates and moderate reviews.
- **With what speed, availability, and security?** Under a 3-second response boundary, 99.0% monthly uptime target, and secure role-based access control (RBAC).
- **Which requirement originates each test case?** Every validation rule links back to an explicit SRS requirement index (RF1 to RF20) inside our traceability grid.
