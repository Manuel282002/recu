# Navigation Map

> Defines the screen structure of the system, how screens connect to each other, and what routes
> exist. It is the reference when frontend and backend discuss what endpoints exist
> or how to reach a feature.

---

## Frontend route structure

/                           → Home / landing page (Huila Travel Expedition)├── /auth│   ├── /login              → Authentication form (Acceso a panel de agencia, viajero o administración)│   ├── /register           → New user registration (Opciones: Soy viajero / Tengo una agencia)│   └── /forgot-password    → Password recovery│├── /dashboard              → Main panel (authenticated)│   ├── /overview           → Summary and key metrics (Estadísticas según rol)│   └── /notifications      → Notification center│├── /planes                 → Planes turísticos list (Búsqueda y filtros por municipio, precio, duración)│   ├── /new                → Creation form (Solo rol: Agencia)│   └── /:id│       ├── /               → Resource detail (Detalle de experiencia, itinerario, tarifas y calendario)│       └── /edit           → Edit form (Solo rol: Agencia)│├── /reservas               → Historial de reservas list (Pasadas y activas)│   └── /:id                → Detail (Detalle completo del bono de reserva y estado)│├── /admin                  → Administration panel (role: Administrador de Plataforma)│   ├── /agencias           → Verificación de agencias (Validación de RNT y estado de aprobación)│   └── /reseñas            → Moderación manual de contenido y reseñas inapropiadas│└── /profile                → Authenticated user's profile (Información de contacto, redes sociales, sitio web)

## Screen map

| Screen | Route | Component | Minimum role | Backend service |
|--------|-------|-----------|--------------|----------------|
| Home | `/` | `HomePage` | Public | — |
| Login | `/auth/login` | `LoginPage` | Public | Módulo de Registro y Autenticación |
| Register | `/auth/register` | `RegisterPage` | Public | Módulo de Registro y Autenticación |
| Dashboard | `/dashboard` | `DashboardPage` | Turista / Viajero | Módulo de Reservas |
| Planes turísticos list | `/planes` | `PlanesListPage` | Public | Módulo de Búsqueda y Filtros |
| Planes turísticos detail | `/planes/:id` | `PlanesDetailPage` | Public | Módulo de Gestión de Planes Turísticos |
| Create Planes turísticos | `/planes/new` | `PlanesFormPage` | Agencia | Módulo de Gestión de Planes Turísticos |
| Admin panel | `/admin` | `AdminDashboard` | Administrador | Módulo de Administración y Reportes |

---

## Main user flows

### Flow 1 — Búsqueda, Selección y Solicitud de Reserva

Landing (/) o Filtros (/planes)│▼ Selecciona un plan específicoDetalle del Plan (/planes/:id)│▼ Completa formulario de reserva (Fecha, Personas, Tarifa) y acepta términos (RF20)Solicitud de Reserva│├── Cupo disponible en calendario ──► Solicitud Enviada (Estado: Pendiente)│└── Cupo lleno en calendario ──────► Alerta en interfaz (Evita sobreventas)

**Related HUs:** HU-11, HU-12, HU-16

### Flow 2 — Authentication

Landing (/)│▼ Click "Sign in" o "Iniciar sesión"Login (/auth/login)│├── Valid credentials ──► Dashboard (/dashboard) según rol (Administrador, Agencia, Turista)│└── Invalid credentials ► Login con mensaje de error en español (bloqueo al 5° intento por 15 min)
**Related HUs:** HU-02, HU-04

---

## Navigation rules

| Rule | Description |
|------|-------------|
| Authentication | Routes under `/dashboard`, `/planes/new`, `/planes/:id/edit`, `/reservas`, `/admin` redirect to `/auth/login` if no session |
| Authorization | Routes under `/admin` redirect to `/dashboard` if the user does not have Administrador role. Routes under `/planes/new` redirect to `/dashboard` if the user is not an Agencia. |
| 404 | Undefined routes show the 404 screen with a link to dashboard |
| Confirmation | Destructive actions (delete plan, cancel reservation) show a confirmation dialog modal before executing |

---

## Correlations

- Design system (visual components) → `12-ux-ui/design-system.md`
- Wireframes → `12-ux-ui/wireframes.md`
- Frontend API contracts → `07-api/contracts/openapi/`
- Roles and permissions → `00-governance/security-policy.md`

