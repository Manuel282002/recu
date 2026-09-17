# Non-Functional Requirements (NFR) — Huila Travel Expedition

NFRs define the qualities of the system. For the Huila Travel Expedition platform, these metrics are tightly aligned with our initial shared hosting infrastructure restrictions (1 vCPU, 1 GB RAM, 5-10 GB SSD) as defined in the SRS (RNF3).

---

## NFR-001: Performance

| Attribute | Metric | Test condition |
|-----------|--------|---------------|
| P95 latency — main page and tour plans | < 3000ms (3s) | Under normal load (RNF1) |
| P99 latency — critical endpoints | < 3000ms (3s) | Under 30-50 concurrent users (RNF3) |
| Image compression target | < 500 KB | Automatically upon upload (RNF2) |
| Report generation time | < 10 seconds | PDF compilation for booking history (RF18) |
| Email delivery time | < 120 seconds (2m) | Dynamic transactional SMTP queues (RF12) |

**Defined critical endpoints:**
- `GET /` — Crucial for tourist conversion and loading the 6 featured tour plans (HU-10).
- `GET /paquetes` — Main catalog for applying multi-variable real-time search filters (RF7).
- `POST /reservas` — Process incoming tourist reservation requests securely while evaluating calendar capacity (RF10).

**Load testing tools:**
- k6, Apache JMeter

**Where is it validated?** Evaluated locally and verified via automated automated staging scripts before deployments.

---

## NFR-002: Availability

| Environment | SLO | Maintenance window | Max downtime/month |
|------------|-----|-------------------|-------------------|
| Production | 99.0% | Scheduled low-traffic windows | 7.2 hours (RNF10) |
| Staging | 95.0% | No restriction | 36 hours |

**Monthly error budget in production:** 7.2 hours.
**Error Budget policy:** Maintenance windows must be announced beforehand on the platform. If the monthly error budget drops below 99% uptime, deployments are frozen to secure standard reliability.

**Health checks:**
- `GET /health` — Verifies that the core Laravel application process is running.
- `GET /health/ready` — Confirms active connectivity with the MySQL 8.0 local database engine.

---

## NFR-003: Scalability

| Scenario | Expected behavior |
|---------|------------------|
| Gradual load growth | Vertical migration to a VPS architecture (2 vCPU, 4 GB RAM, 20 GB SSD) without rewriting code (RNF12). |
| Traffic spike limits | Platform must safely hold 30 to 50 concurrent travelers during high tourist season peaks (RNF3). |
| Concurrency capacity | Supported up to 500 users at baseline infrastructure boundaries (RT05). |

**Strategy:** Clean code architecture using Laravel's standard optimization features and Eloquent queries to support infrastructure vertical scaling in less than 8 hours of technical migration work.

---

## NFR-004: Security

### Authentication and Authorization
- Secure login mechanism with failed attempts lockdown (locks account for 15 minutes after 5 consecutive failures) (HU-02).
- Automatic session expiration after 30 minutes of user inactivity (HU-02).
- Role-Based Access Control (RBAC) establishing 3 distinct permission scopes: Administrador, Agencia, and Turista (RF16, HU-04).

### Data transmission
- HTTPS is mandatory for all production traffic using valid SSL certificates (RNF7).
- Automatic server-side redirection from HTTP to HTTPS (RNF7).

### Sensitive data
- Passwords: Encrypted via Laravel's native hashing system utilizing `bcrypt` algorithms (RNF8).
- Zero storage of cleartext credentials across database tables.
- Compliance with PCI-DSS guidelines: No sensitive payment card data is saved on the system's local storage (Section 5).

### Regulatory compliance
- **Ley 1581 de 2012 (Habeas Data):** Explict acceptance of terms and privacy policies is mandatory before registering or submitting a booking request, saving the exact timestamp of consent (RF20, RNF9, HU-16).
- **Ley 2068 de 2020 (Colombia General Tourism Law):** Mandatory validation and physical verification of the National Tourism Register (RNT) code for travel agencies (Section 5).

---

## NFR-005: Observability

| Pillar | Requirement | Tool |
|--------|------------|------|
| Logs | Error auditing in standard files with contextual user arrays | Laravel Log / Monolog |
| Metrics | Execution time markers for heavy search filter database queries | Built-in Profilers / PageSpeed |
| Alerts | Automated email notification to the Admin user on system failures | SMTP Alert Routing |

---

## NFR-006: Maintainability

| Metric | Target |
|--------|--------|
| Clean Code Standards | Monolith code separation following Modular MVC patterns for clean maintenance |
| Backup Routine | Automated weekly database backup routines (RNF11) |
| Recovery Time (RTO) | System recovery and total backup restoration complete in less than 4 hours (RNF11) |
| Local Setup Onboarding | A new developer can replicate the Laravel environment locally in less than 1 hour |

---

## NFR-007: Portability

- The backend architecture runs seamlessly on multi-platform PHP servers (PHP 8.2+).
- Fully compatible with standard Linux distributions (Ubuntu Server / CentOS) for easy hosting relocations.
- Database schemas are structured under versioned native migration files ensuring portability to any environment running MySQL 8.0.

---

## NFR priority matrix

| NFR | Priority (P1/P2/P3) | Validated in CI? | Owner |
|-----|---------------------|-----------------|-------|
| Performance | P1 | Yes (RNF1 Page Loading) | Manuel Caviedes (Project Lead) |
| Availability | P1 | Yes (Uptime Tracker) | Development Team |
| Security | P1 | Yes (Role Validation Middleware) | Manuel Caviedes / Full Stack Team |
| Maintainability | P2 | Yes (Weekly Backups Setup) | Development Team |

---

## Correlations

- Detailed functional constraints → `02-domain/stories/`
- Relational mapping constraints → `06-data/models.md`
- Code execution logic guidelines → `Especificación de Requisitos (SRS)`
