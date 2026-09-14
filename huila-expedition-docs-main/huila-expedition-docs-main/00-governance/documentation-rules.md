# Documentation Rules - HUILA TRAVEL EXPEDITION

> These rules determine how documentation is written, organized, and maintained in this project.
> Documentation that does not follow these rules may be rejected in code review.

---

## Core principle

> **"Documentation is code. If it's not up to date, it's broken."**

Every User Story (HU-01 to HU-20) that modifies the system behavior or its database structures in MySQL MUST include updating the affected markdown documents.
The Definition of Done (DoD) requires it.

---

## Language

| Artifact | Language |
|----------|----------|
| Source code (variables, PHP functions, classes, Eloquent models) | English |
| Code comments (inline PHP documentation) | English |
| Commits | English (Conventional Commits, e.g., `docs(governance):`) |
| Branch names | English (e.g., `docs/inicializacion`) |
| Markdown documentation | Spanish / English (System spec in Spanish, tech framework in English) |
| OpenAPI contracts (descriptions) | English |
| Error messages returned to frontend | Spanish (Clear messages as requested by RNF6) |
| Internal system logs (Laravel logs) | English |

> **Rule:** Once the language for each category is chosen, it is binding for the entire project.
> Mixing languages in the same category is grounds for PR rejection.

---

## File structure

---

## What to document and what NOT to

### DO document

| What | Where |
|------|-------|
| Non-obvious architectural decisions (e.g., using Laravel native hashing bcrypt) | `05-architecture/decisions/records/ADR-NNN.md` |
| Business rules and domain invariants (e.g., checking RNT validation for RF1) | `02-domain/entities-and-rules.md` |
| API contracts for the travel platform | `07-api/contracts/openapi/travel-platform.yaml` |
| Data model changes (MySQL 8.0 migrations, schemas, relationships) | `06-data/models.md` |
| Operational procedures (Local deployment environment setup - XAMPP/Laragon) | `13-operations/` |
| Identified risks (e.g., booking concurrency or overbooking risks) | `15-project-control/risks.md` |

### DO NOT document

- What the PHP/Laravel code already says clearly (do not repeat in comments what can be read in the code).
- Temporary database tables or experiments that will be reverted.
- Implementation details of external libraries (like Tailwind CSS internal framework components).
- Change history (that's what git log is for).

---

## Owners per section

| Section | Owner | Review frequency |
|---------|-------|-----------------|
| `00-governance/` | Manuel Felipe Caviedes (Tech Lead) | Start of each sprint |
| `02-domain/` | Manuel Felipe Caviedes (Tech Lead) | When the domain rules change |
| `04-requirements/` | ADSO Team (Manuel, Luisa, Juan, Natalia) | Each sprint planning |
| `05-architecture/` | Manuel Felipe Caviedes (Tech Lead) | Each design decision |
| `07-api/contracts/` | Luisa Ortega / Juan Oteca (Devs) | Each API endpoint change |
| `06-data/` | Manuel Caviedes / Natalia Trujillo (Devs) | Each MySQL schema change |
| `13-operations/` | ADSO Development Team | After each local deployment sprint |
| `15-project-control/` | Manuel Felipe Caviedes (Tech Lead) | Weekly review |

---

## Document format

### Headings
- `# H1` — only one per file; it is the title.
- `## H2` — main sections.
- `### H3` — subsections.
- Do not use H4 or deeper; if you need it, the document has too much hierarchy.

### Tables
Use tables for comparisons, registers, matrices, and roles. Do not use tables for simple lists.

### Code
Always use code blocks with the language specified (e.g., php, sql, json, markdown):

public function registerAgency(Request $request) { ... }

### Template instructions
Blocks marked `> [!NOTE] INSTRUCTIONS` indicate the document is an unfilled template.
Remove them when the document is complete.

---

## Update process

1. The developer identifies which documents or database models their change affects.
2. Updates the documentation files together with the Laravel code (same PR).
3. The reviewer (Manuel Caviedes or another team member) verifies the documentation is up to date.
4. If the PR closes a HU that had API impact (like payment gateways for Wompi/PayU) → the OpenAPI contract must be updated.

---

## Correlations

- Git conventions → `00-governance/git-conventions.md`
- System Architecture specifications → `05-architecture/architecture-specs.md`
- Definition of Done (docs as part of DoD) → `00-governance/definition-of-done.md`
```

---

### Cómo debes guardar este archivo ahora mismo en GitHub:

1. **Nombre del archivo (Arriba):** `00-governance/documentation-rules.md` (Asegúrate de seguir parado en la rama `docs/inicializacion`).
2. **Caja del código (Centro):** Pega todo el bloque de texto que te acabo de dar.
3. **Commit changes (Abajo):** 
   * En el título corto escribe el prefijo y mensaje exacto exigido por tu profesor:
     ```text
     docs(governance): estructurar reglas de documentacion para el proyecto hte
     ```
   * En la descripción opcional puedes escribir:
     ```text
     Se completan las normas de formato, idioma y asignación de propietarios por sección para el proyecto Huila Travel Expedition, alineando los requerimientos de la ficha ADSO al framework de desarrollo.
     ```
4. Dale clic al botón verde de **Commit changes**.

---

<FollowUp>
¡Con esto ya tenemos **4 archivos completados con información real** en tu rama de inicialización! Superamos el mínimo de 3 secciones que te pidió el profesor para abrir el PR. 

¿Prefieres que te enseñe ahora mismo a **abrir el Pull Request (PR)** formalmente en GitHub para enviárselo al instructor, o quieres que completemos el archivo de **`git-conventions.md`** antes de cerrar la rama?
</FollowUp>


