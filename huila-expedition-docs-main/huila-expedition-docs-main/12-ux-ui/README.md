# 12 — UX/UI

> **What is this?** The user experience design: how the system looks, how it is navigated,
> and how it behaves from the end user's perspective.

## Why design comes before code

Changing a wireframe takes 5 minutes. Changing the code takes hours.
Changing the code in production with real users can cost days and reputation.

**Design first → implement later.**

---

## What is here and how to fill it in

### `navigation-map.md` ⭐ (Start here)
The map of all screens/pages and how they connect.

## Navigation map

### Public area (no authentication)
- / (Huila Travel Expedition Home)
  - /auth/login
  - /auth/register (Opciones: Viajero / Agencia)
  - /planes (Resultados de búsqueda con filtros por municipio, precio y duración)
  - /planes/{id} (Ficha de detalle de la experiencia turística e itinerario)

### Private area — Role: Turista / Viajero
- /dashboard
  - /reservas
    - /reservas/list (Historial de consultas pasadas y activas)
    - /reservas/{id} (Detalle del bono de solicitud y estado de confirmación)
  - /profile (Gestión de cuenta personal)

### Private area — Role: Agencia Local (Proveedor)
- /dashboard
  - /planes
    - /planes/new (Formulario en línea para la creación de un plan turístico)
    - /planes/{id}/edit (Formulario de edición y actualización)
  - /reservas
    - /reservas/list (Panel de gestión de solicitudes recibidas para aprobación o cancelación)
  - /profile (Actualización de información de contacto, redes sociales y enlace a sitio web propio)

### Private area — Role: Administrador de Plataforma
- /admin
  - /agencias (Verificación y validación manual del Registro Nacional de Turismo - RNT)
  - /reseñas (Módulo de moderación manual de calificaciones y comentarios recibidos)
  - /overview (Panel administrativo con estadísticas generales del mes)

## Access matrix

| Screen | Turista / Viajero | Agencia Local | Administrador | Public |
|--------|:---:|:---:|:---:|:---:|
| `/` (Home) | ✅ | ✅ | ✅ | ✅ |
| `/auth/login` / `/auth/register` | ❌ | ❌ | ❌ | ✅ |
| `/planes/{id}` (Detalle del Plan) | ✅ | ✅ | ✅ | ✅ |
| `/dashboard` (General) | ✅ | ✅ | ✅ | ❌ |
| `/planes/new` / `/edit` | ❌ | ✅ | ✅ | ❌ |
| `/admin/*` (Módulos de Control) | ❌ | ❌ | ✅ | ❌ |

---

### `wireframes.md`
Low-fidelity designs of the main screens.
Contiene la estructura visual responsiva y en formato mobile-first (para pantallas desde 320px de ancho) de los componentes clave: el buscador de municipios (Villavieja, San Agustín, Neiva, Rivera, Pitalito, La Plata), la sección de planes destacados y el formulario de reserva con aceptación explícita de la Ley 1581 de 2012.

### `design-system.md`
The project's design system: tokens, components, patterns.
Contiene la paleta corporativa basada en el Verde Institucional del SENA (`#39A900`), la tipografía adaptada a los frameworks Bootstrap y Tailwind CSS, y los estados semánticos del sistema (Disponible, Pocas plazas, Sin cupo).

---

## Correlations with other sections

| This section is fed by... | And feeds into... |
|---------------------------|-------------------|
| `04-requirements/user-stories.md` → what flows exist (HU-01 a HU-20) | Screens implementing each HU |
| `02-domain/entities-and-rules.md` → what data to display (Agencias, Planes, Calendarios, Reservas, Reseñas) | Fields in wireframes and tables |
| `09-microservices/` → what APIs the frontend consumes (Laravel 10+ Backend REST API) | What data arrives at each screen |

---

## Questions this section must answer

- **How many screens does the system have?** Posee la página principal (Home), el flujo de autenticación (Login/Registro/Recuperación), el flujo público de búsqueda y detalle de planes, los paneles privados de autogestión para las Agencias/Turistas y el panel centralizado de control para el Administrador.
- **How does each type of user navigate?** Los viajeros buscan de forma libre y solicitan reservas con un checkout optimizado; las agencias navegan por formularios privados de inventario en tiempo real; y el administrador gestiona solicitudes mediante tablas de datos y paneles estadísticos generales.
- **What visual components are repeated?** Se repiten los botones con estados de carga síncronos/asíncronos, las tarjetas descriptivas de planes turísticos con fotos, los mensajes de error en color rojo debajo de los campos de los formularios, y los modales con fondo oscurecido para confirmaciones destructivas.
- **What is the system's visual language?** Un diseño limpio, accesible y responsivo (mobile-first), regido por la identidad de la región del Huila ("desierto de estrellas y montaña verde") y alineado cromáticamente con los lineamientos del SENA.
