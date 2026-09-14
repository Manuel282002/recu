# Definition of Ready (DoR) - HUILA TRAVEL EXPEDITION

> A User Story is **Ready** when the entire team can start it in the next sprint
> without needing to resolve fundamental questions mid-sprint.
> If a story doesn't meet this DoR, it goes back to refinement.

---

## DoR checklist

Before moving a User Story to "Ready for Sprint", verify:

### Clarity

- [ ] The story is written in the exact format used in the SRS: **As [role], I want [action], so that [benefit]** (e.g., HU-01 to HU-20).
- [ ] The role is specific based on our defined roles: **Administrador de Plataforma**, **Agencia Local (Proveedor)**, or **Viajero (Usuario Final)**.
- [ ] The expected benefit is clear, verifiable, and aligned with regional tourism promotion.

### Acceptance Criteria

- [ ] There are clear acceptance criteria written in the user story card (matching the criteria listed in our SRS document).
- [ ] The criteria cover the happy path (e.g., successful image compression under 500 KB for HU-06) AND error cases (e.g., system alert when trying to delete a plan with active bookings for HU-05).
- [ ] The criteria are testable via manual validation or automated PHPUnit tests within Laravel.
- [ ] There are no ambiguous criteria (e.g., instead of "the response should be fast", it must state "page loads in maximum 3 seconds under normal conditions" to comply with RNF1).

### Dependencies

- [ ] All external API dependencies are identified, such as the payment gateway (**Wompi / PayU / MercadoPago**) or **WhatsApp Business API** (Section 9.3).
- [ ] Blocking dependencies are resolved (e.g., the MySQL 8.0 base schema must be ready before building the Agency Registration views).
- [ ] Legal compliance requirements (like validating the **Registro Nacional de Turismo - RNT** for RF1 or explicit acceptance of **Ley 1581 de 2012** for RF20) are explicitly referenced.

### Estimation

- [ ] The team (Manuel, Luisa, Juan, and Natalia) has estimated the story using our established Story Points scale (1 to 13 SP).
- [ ] There is agreement that the story fits inside our 2-week sprint capacity (~20 SP total capacity).
- [ ] If the story is too large (> 5 SP, like the complete Booking Request Engine HU-12), it has been broken down into smaller sub-tasks.

### Technical readiness

- [ ] Local development environments (XAMPP/Laragon running PHP 8.2+ and MySQL 8.0) are available and synchronized for all team members.
- [ ] Database requirements, migrations, and ORM Eloquent relationships are defined if there are database changes.
- [ ] The impact on other application modules is fully identified.

### Non-functional requirements

- [ ] Performance constraints are specified (e.g., complying with RNF2 image compression limits).
- [ ] Security requirements are considered (such as native Laravel bcrypt hashing for passwords according to RNF8, or role-based access control for RF16).
- [ ] UI responsiveness requirements are met following the Mobile-First approach using Bootstrap or Tailwind CSS (RNF4).

---

## Common reasons a story is NOT ready

| Problem | What to do |
|---------|-----------|
| Unclear requirements / Ambiguous filters | Schedule a 30-min refinement session with the team and Instructor Karol Daniela Correa. |
| Missing RNT or Law 1581 validation fields | Update the acceptance criteria to include specific Colombian legal fields before the sprint starts. |
| Unknown payment gateway API parameters | Project Leader (Manuel) reviews and documents the Wompi/PayU integration guide. |
| Too large (> 5 SP) | Break the story down into smaller frontend components or specific backend endpoints. |
| No database connection or environment crash | Team collaborates to restore local database configurations using migration files. |

---

## DoR vs DoD

| | Definition of Ready (DoR) | Definition of Done (DoD) |
|-|--------------------------|--------------------------|
| **When** | Before starting the story (during refinement/planning) | After finishing the story (before closing the task) |
| **Who verifies** | ADSO Team during planning sessions | ADSO Team members during peer review / PR audit |
| **Purpose** | Ensure the team can start development without blockers | Ensure the increment is shippable, secure, and compliant |

---

## Correlations

- Full DoD → `00-governance/definition-of-done.md`
- User Stories backlog → `04-requirements/user-stories.md`
- System Architecture → `05-architecture/architecture-specs.md`
