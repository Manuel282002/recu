# User Stories — Backlog — Huila Travel Expedition

This artifact lists the functional backlog for the **Huila Travel Expedition** platform. It categorizes requirements into epics and details refined user stories aligned with the development team and sprint delivery targets.

---

## Backlog status

| Cut | Sprint | Total HUs | Refined | In progress | Completed |
|-----|--------|-----------|---------|-------------|-----------|
| Cut 1 | Sprint 1 | 4 | 4 | 0 | 4 |
| Cut 1 | Sprint 2 | 3 | 3 | 3 | 0 |
| Cut 2 | Sprint 3-4 | 13 | 0 | 0 | 0 |

---

## Epics

| ID | Epic | Description |
|----|------|-------------|
| EP-001 | Identity and Security | Manages authentication, access control via RBAC, and secure registration for administrative roles and local agencies. |
| EP-002 | Catalog and Inventory Control | Handles the creation, categorization, and calendar availability of adventure and ecotourism packages. |
| EP-003 | Booking Engine and Quality Assurance | Processes user reservations, accepts terms, tracks history, and manages moderated traveler reviews. |

---

## User Stories

### HU-01 — Online Agency Registration {#HU-001}

**Epic:** EP-001

> **As** an unregistered Travel Agency
> **I want** to fill in a registration form with my legal identifiers (NIT and RNT)
> **so that** I can gain a digital workspace to promote my regional tour services.

**Acceptance Criteria:**

```gherkin
Scenario 1: Successful form submission (Happy Path)
  Given that an agency is on the public online registration interface
  When  they input a valid corporate name, NIT, RNT, email, and password
  Then  the system stores the account record inside the MySQL database
  And   the initial account status is set to 'PENDIENTE' pending admin review.

Scenario 2: Submission failure due to duplicate legal records
  Given that an agency tries to register on the platform
  When  they input an RNT or NIT number that already exists in the system
  Then  the application blocks the transaction
  And   an error message stating 'The legal registration credentials already exist' is displayed.
```

**Definition of Done:**
- [ ] Monolith architecture code reviewed and approved.
- [ ] Database validation migrations executed successfully.
- [ ] Acceptance criteria verified via custom PHPUnit assertions.
- [ ] Form interface verified on responsive screens.

| Field | Value |
|-------|-------|
| Story Points | 3 |
| Priority | Must Have |
| Target sprint | Sprint 1 |
| Assigned to | Manuel Felipe Caviedes Cordero |
| Status | Done |
| Dependencies | None |
| Affected service(s) | Monolith (AgencyModule) |

---

### HU-05 — Tour Plan Lifecycle CRUD {#HU-005}

**Epic:** EP-002

> **As** an approved Travel Agency
> **I want** to create, edit, and delete my tour plans from a central panel
> **so that** I can keep my travel packages accurate and updated according to market demands.

**Acceptance Criteria:**

```gherkin
Scenario 1: Creating a package with proper attributes
  Given that an agency has an 'APROBADA' account status and is logged in
  When  they submit the tour creation form with title, description, itinerary, price, duration, and type
  Then  the new plan is persisted immediately in the database
  And   it becomes available for search inside the public catalog interface.

Scenario 2: Warning when deleting a package with active bookings
  Given that an agency triggers a delete command on an existing package
  When  the selected package has active or pending bookings attached to it
  Then  the application stops the hard deletion process
  And   it displays an administrative alert blocking the destruction of active reservation paths.
```

| Field | Value |
|-------|-------|
| Story Points | 5 |
| Priority | Must Have |
| Target sprint | Sprint 2 |
| Assigned to | Luisa Fernanda / Juan Diego / Natalia Trujillo |
| Status | In Progress |
| Dependencies | HU-01, HU-02 |
| Affected service(s) | Monolith (TourPlanModule) |

---

## Rules for writing HUs

### 1. The role matters
Do not write "As a user" — that says nothing. Use the specific role:
- **As a system administrator** (Administrador de Plataforma)
- **As an approved travel agency** (Agencia Local)
- **As a traveler** (Viajero / Turista)

### 2. The benefit justifies the work
The "so that" must describe a business benefit, not redescribe the action:
- **so that** I can avoid manual communications and process reservation slots autonomously.

### 3. ACs are verifiable
Each AC must be verifiable manually or automatable as a test:
- **Then** the application blocks the registration and returns a clear text validation error.

### 4. One HU = one unit of value
Stories must fit completely inside a 2-week sprint window. If a story grows beyond 8 Story Points, it is automatically split.

---

## Ready-to-copy HU template

```markdown
### HU-00X — [Name] {#HU-00X}

**Epic:** EP-00X

> **As** [role]
> **I want** [action]
> **so that** [benefit]

**Acceptance Criteria:**

\```gherkin
Scenario 1: [name]
  Given [context]
  When  [action]
  Then  [result]
\```

| Field | Value |
|-------|-------|
| Story Points | |
| Priority | |
| Target sprint | |
| Status | Backlog |
| Dependencies | |
```

---

## Correlations

- Team DoD checklists → `00-governance/definition-of-done.md`
- Non-functional metric validation rules → `02-domain/requirements/non-functional.md`
- Core relational traceability mapping → `02-domain/requirements/traceability-matrix.md`
