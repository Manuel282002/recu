# Definition of Done (DoD) - HUILA TRAVEL EXPEDITION

> A User Story is **DONE** when it meets ALL criteria on this checklist.
> If even one is missing, the story is NOT done — it goes back to In Progress.

## Mandatory checklist

### Code
- [ ] Code implements all acceptance criteria of the user story (HU-01 to HU-20).
- [ ] Code was reviewed and approved by at least 1 team member (Luisa, Juan, Natalia, or Manuel via PR review).
- [ ] Code follows project standards (Laravel Pint / PSR-12 coding standard formatting pass).
- [ ] No technical debt introduced without registering it in `15-project-control/technical-backlog.md`.
- [ ] Database structures follow MySQL 8.0 integrity standards (ACID transactions implemented for booking concurrency to prevent overbooking as stated in SRS section 5.1).

### UI & Usability
- [ ] Frontend views are fully responsive (Mobile-First approach with Bootstrap or Tailwind CSS) and functional on screens down to 320px (Compliance with RNF4).
- [ ] Form validation functions correctly in real-time with clear error messages in Spanish (Compliance with RNF6).
- [ ] Layout is fully compatible and tested in Google Chrome, Mozilla Firefox, Safari, and Microsoft Edge (Compliance with RNF5).

### Tests
- [ ] Unit tests written for new business logic using PHPUnit in Laravel (e.g., pricing rules, availability arithmetic).
- [ ] Test coverage does not decrease from the project baseline.
- [ ] All tests pass successfully in the local execution environment.
- [ ] Acceptance criteria verified manually (e.g., verifying image compression doesn't exceed 500 KB for HU-06).

### Integration & Security
- [ ] Changes do not break other platform modules (Authentication, Agency Management, Booking Engine).
- [ ] All data traffic and routing run strictly over HTTPS (SSL certificate validated as per RNF7).
- [ ] Passwords and authentication processes utilize native Laravel hashing (bcrypt) to ensure security compliance (RNF8).
- [ ] Data handling routines strictly enforce Colombian Law 1581 of 2012 (Habeas Data) with explicit terms acceptance before booking requests (RF20 / RNF9).

### Deployment
- [ ] Code is mergeable to the `docs/inicializacion` or development branch with zero conflicts.
- [ ] Local environment validation is successful on Apache/Nginx web server stacks running PHP 8.2+.
- [ ] Basic smoke tests pass cleanly on the execution instance.

### Documentation
- [ ] Main project documentation updated if structural parameters or logic paths change.
- [ ] Database relational schemas or changes are logged in the technical log if MySQL migrations were altered.

---

## Allowed exceptions

The following exceptions must be explicitly agreed to by the Leader (Manuel Felipe Caviedes):
- Automated testing omitted for UI components due to immediate iteration constraints (document manual pass).
- Documentation adjustments deferred due to urgent sprint delivery requirements (must create a tech-debt ticket).

---

## What is NOT a Done criterion

- "El código está en mi máquina" — it must be pushed to the GitHub repository branch.
- "Funciona en mi entorno local" — it must build cleanly without breakages for other teammates.
- "La instructora Karol Daniela ya vio la pantalla" — that is product acceptance; the technical requirements checklist must still be completely ticked.
