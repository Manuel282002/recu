# Traceability Matrix — Huila Travel Expedition

This matrix connects every single software requirement and business rule from the SRS directly to its corresponding user story, technical service module, and validation status.

---

## FR → HU → Test → Service Matrix

| FR ID | FR Description | HU(s) | Tests / Verification Method | Service Module | Status |
|-------|---------------|-------|----------------------------|----------------|--------|
| RF1   | Registro de agencias mediante formulario en línea | HU-01 | `AgencyRegistrationTest.php` | Monolith (AgencyModule) | ✅ Done |
| RF2   | Inicio de sesión con autenticación segura | HU-02 | `AuthLoginTest.php` | Monolith (AgencyModule) | ✅ Done |
| RF3   | Actualización de información de contacto y redes | HU-03 | `AgencyProfileTest.php` | Monolith (AgencyModule) | ✅ Done |
| RF4   | Creación, edición y eliminación de planes turísticos | HU-05 | `TourPlanCrudTest.php` | Monolith (TourPlanModule) | 🟡 In progress |
| RF5   | Carga y administración de imágenes de destinos | HU-06 | `ImageUploadTest.php` | Monolith (TourPlanModule) | 🟡 In progress |
| RF6   | Clasificación de planes por tipo de turismo | HU-07 | `CategoryFilterTest.php` | Monolith (TourPlanModule) | 🟡 In progress |
| RF7   | Filtros de búsqueda por municipio, precio y duración | HU-11 | `SearchFiltersTest.php` | Monolith (TourPlanModule) | 🟡 In progress |
| RF8   | Configuración de tarifas diferenciadas | HU-08 | `PricingTiersTest.php` | Monolith (TourPlanModule) | 🔴 Pending |
| RF9   | Calendario para mostrar fechas disponibles | HU-09 | `AvailabilityCalendarTest.php` | Monolith (TourPlanModule) | 🟡 In progress |
| RF10  | Solicitud de reservas por parte de los turistas | HU-12 | `BookingRequestTest.php` | Monolith (ReservationModule) | 🔴 Pending |
| RF11  | Gestión de aprobación o cancelación de reservas | HU-13 | `BookingApprovalTest.php` | Monolith (ReservationModule) | 🔴 Pending |
| RF12  | Envío automático de correos de confirmación | HU-14 | `MailNotificationTest.php` | Monolith (ReservationModule) | 🔴 Pending |
| RF13  | Consulta de historial de reservas | HU-15 | `BookingHistoryTest.php` | Monolith (ReservationModule) | 🔴 Pending |
| RF14  | Sistema de calificaciones y reseñas | HU-17 | `ReviewsModerationTest.php` | Monolith (ReservationModule) | 🔴 Pending |
| RF15  | Panel administrativo con estadísticas generales | HU-18 | `AdminDashboardTest.php` | Monolith (AdminModule) | 🔴 Pending |
| RF16  | Control de acceso según roles definidos | HU-04 | `RoleMiddlewareTest.php` | Monolith (AdminModule) | ✅ Done |
| RF17  | Visualización de planes destacados en la página principal | HU-10 | `FeaturedPlansTest.php` | Monolith (TourPlanModule) | 🟡 In progress |
| RF18  | Generación de reportes en PDF | HU-19 | `PdfReportTest.php` | Monolith (AdminModule) | 🔴 Pending |
| RF19  | Formulario de contacto para soporte | HU-20 | `SupportFormTest.php` | Monolith (AdminModule) | 🔴 Pending |
| RF20  | Aceptación de términos y condiciones antes de reservar | HU-16 | `TermsAcceptanceTest.php` | Monolith (ReservationModule) | 🔴 Pending |

---

## NFR → Validation Matrix

| NFR ID | Description | How it is validated | Tool | Status |
|--------|-------------|-------------------|------|--------|
| RNF1   | Page load < 3 seconds | Frontend load speed checks | Google PageSpeed / Lighthouse | 🟡 Monitoring |
| RNF2   | Auto image compression < 500 KB | Intervention Image backend testing | PHPUnit Integration | 🟡 Monitoring |
| RNF3   | Support 30-50 concurrent users | Load testing under baseline limits | Apache JMeter / k6 | 🟡 Monitoring |
| RNF4   | Responsive (Mobile-First) layout | Cross-device UI inspections | Chrome DevTools | 🟡 Monitoring |
| RNF7   | Mandatory HTTPS encryption | SSL certificate validity checks | Qualys SSL Labs | ✅ Validated |
| RNF8   | Password hashing via Bcrypt | Cryptographic model security assertions | PHPUnit Assertions | ✅ Validated |
| RNF9   | Habeas Data (Ley 1581) compliance | DB timestamp auditing for RF20 | MySQL Query Logs | 🔴 Pending |
| RNF11  | Weekly database backups | Backup automation and restore drills | Cron Tasks / Bash Scripting | 🟡 Monitoring |

---

## Inverse Traceability: HU → FR

| HU ID | Title / Target Scope | FR(s) it implements | Sprint |
|------|----------------------|---------------------|--------|
| HU-01 | Online Agency Registration | RF1 | Sprint 1 |
| HU-02 | Secure Auth Login Session | RF2 | Sprint 1 |
| HU-03 | Agency Profile Customization | RF3 | Sprint 1 |
| HU-04 | RBAC Permissions Middleware | RF16 | Sprint 1 |
| HU-05 | Tour Plan Lifecycle Crud | RF4 | Sprint 2 |
| HU-09 | Availability Calendar Control | RF9 | Sprint 2 |
| HU-11 | Real-time Search Catalog Filters | RF7 | Sprint 2 |

---

## Status Legend

| Status | Meaning |
|--------|---------|
| ✅ Done | Implemented, verified, and merged into main codebase |
| 🟡 In progress | Active task in development under the current sprint |
| 🔴 Pending | Backlog item waiting for sprint planning allocation |

---

## Identified Gaps (Requirements without coverage)

No structural requirement gaps were found during the analysis. Every single functional requirement (RF1 to RF20) and non-functional metric outlined in the original SRS documentation maps directly to an established User Story and an automated verification target.

---

## How to Maintain this Matrix

1. When a new system feature or requirement adjustment is requested by the instructor: Add its relational row to the main matrix.
2. When a PHPUnit test file is written in Laravel: Update the "Tests / Verification Method" field.
3. At the end of every sprint demo: Shift the status flags accordingly to maintain absolute alignment with the repository codebase.
