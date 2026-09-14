# Agile Team Conventions - HUILA TRAVEL EXPEDITION

> Defines how the team works through its development cycles. Agree on and sign off
> with the entire team before the first sprint. Update when the team decides to change something.

---

## Sprint structure

| Field | Value |
|-------|-------|
| Duration | 2 weeks |
| Sprint start | Monday |
| Sprint end | Friday of week 2 |
| Current sprint | Sprint 1 — March 2, 2026 to March 13, 2026 |
| Estimated capacity | 20 Story Points (SP) per sprint |

---

## Ceremonies

### Sprint Planning
- **When:** First day of the sprint — 08:00 AM
- **Duration:** Maximum 2 hours
- **Who:** Entire ADSO Team: Manuel Felipe Caviedes (Líder/Dev), Luisa Fernanda Ortega (Dev), Juan Diego Oteca (Dev), Derly Natalia Trujillo (Dev).
- **Goal:** Select and commit to the SRS user stories, break down into tasks (Laravel backend / Frontend components).
- **Output artifact:** Sprint Backlog updated in GitHub Projects.

### Daily Stand-up
- **When:** Monday to Friday — 08:15 AM
- **Duration:** Maximum 15 minutes
- **Format:**
  1. What did I do yesterday for HTE?
  2. What will I do today (e.g., database design, view creation)?
  3. Is anything blocking me (e.g., API integration, image compression)?
- **Rule:** Technical discussions about Laravel or MySQL happen after the daily, not during it.

### Sprint Review
- **When:** Last Friday of the sprint — 10:00 AM
- **Duration:** Maximum 45 minutes
- **Who:** ADSO Team + Instructor Karol Daniela Correa Trujillo.
- **Goal:** Show the functional software increment (e.g., functional filters, agency panel) and collect feedback.

### Sprint Retrospective
- **When:** Last Friday of the sprint — 11:00 AM (after the review)
- **Duration:** Maximum 45 minutes
- **Format:** What went well / What to improve / Action commitments (e.g., improve code reviews).
- **Rule:** Each retro produces at least 1 improvement action with an owner and due date.

### Backlog Refinement
- **When:** Wednesday of the second week — 02:00 PM
- **Duration:** Maximum 1 hour
- **Goal:** Detail and estimate user stories for the next sprint from the SRS catalog.
- **Exit criterion:** The user story meets the Definition of Ready.

---

## Estimation

### Scale

| Points | Meaning | HTE Project Examples (Based on SRS) |
|--------|---------|----------------------------------------------------|
| 1 | Trivial — done in hours | RF20 Acceptance of Terms, HU-20 Contact Form. |
| 2 | Small — done in one day | HU-02 Secure Login, RNF7 HTTPS Setup, RF3 Profile Update. |
| 3 | Medium — takes 2–3 days | HU-01 Agency Registration (with RNT fields), HU-11 Filters by Municipality. |
| 5 | Large — takes almost a full sprint | HU-09 Availability Calendar, HU-06 Automatic Image Compression. |
| 8 | Very large — should be split | HU-12 Booking Request Engine, PDF Report Generation. |
| 13 | Epic — MUST be split | Full Frontend + Backend integration of Wompi/PayU. |

**Technique:** Planning Poker  
**Tool:** GitHub Projects Integration  

### Estimation rule
- If there is disagreement of 2+ levels (e.g., someone says 3 and another says 8 on the Calendar setup), discuss architecture before voting again.
- If a story is estimated at 8 or 13, it must be split into smaller sub-tasks before entering the sprint.

---

## Backlog tool

**Tool:** GitHub Projects  
**Board URL:** https://github.com  

### Board columns

| Column | Meaning |
|--------|---------|
| Backlog | Pending refinement (All SRS User Stories HU-01 to HU-20). |
| Ready | Ready to enter the sprint (meets DoR) |
| In Progress | Someone is actively working on the task (assigned to Manuel, Luisa, Juan, or Natalia). |
| In Review | In Pull Request / code review on GitHub (Main branch protection) |
| Done | Meets DoD (Functional in Laravel, responsive with Bootstrap/Tailwind, tested). |

---

## Team velocity

*(Dado que el proyecto tiene un esfuerzo estimado total de ~59 SP en tu SRS, lo dividiremos equitativamente en 3 Sprints de desarrollo)*

| Sprint | Story points completed | Notes |
|--------|----------------------|-------|
| Sprint 1 | 20 | Auth Módulo, Gestión de Planes (HU-01 to HU-07). |
| Sprint 2 | 18 | Módulo de Búsqueda, Filtros y Calendarios (HU-08 to HU-11). |
| Sprint 3 | 21 | Motor de Reservas, PDF, Reseñas y Cierre (HU-12 to HU-20). |
| **Average** | **19.6** | **Baseline team velocity for ADSO 3239188**. |

---

## Related documents

- Definition of Ready → `00-governance/definition-of-ready.md`
- Definition of Done → `00-governance/definition-of-done.md`
- Risk management → `15-project-control/risks.md`
- Technical debt backlog → `15-project-control/tech-backlog.md`
