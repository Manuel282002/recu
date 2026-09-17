# HU-AGENCY-001: Registro de Agencias mediante Formulario en Línea

> **Convención de ID:** `HU-AGENCY-001` (Vinculada directamente al Requisito Funcional: RF1 del SRS)

---

## Historia de Usuario

**Como** Agencia turística
**Quiero** registrarme en la plataforma mediante un formulario en línea
**Para** poder publicar mis planes turísticos y ser visible para los turistas del Huila.

---

## Criterios de aceptación

> Formato: "Dado que [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]"

- [ ] **AC1: Formulario Completo y Estado Inicial.** Dado que una agencia de viajes se encuentra en el formulario de registro, cuando ingresa todos los campos obligatorios (nombre, NIT, RNT, correo, teléfono y contraseña), entonces el sistema valida los datos en tiempo real, guarda el registro en la base de datos MySQL y la cuenta queda en estado 'pendiente de aprobación' hasta que el Administrador la verifique.
- [ ] **AC2: Envío de Correo Automatizado.** Dado que el formulario de registro se ha completado y guardado con éxito, cuando el sistema procesa el registro, entonces envía un correo de confirmación de manera automática a la dirección registrada en un tiempo máximo de 2 minutos.
- [ ] **AC3: Control de Duplicados Legales.** Dado que una agencia intenta registrarse ingresando un NIT o un RNT que ya existe en la plataforma, cuando presiona el botón de enviar, entonces el sistema bloquea el proceso y muestra un mensaje de error claro en español indicando que los identificadores ya se encuentran en uso.

---

## Notas técnicas

> Restricciones de implementación, consideraciones de rendimiento e integraciones requeridas según las especificaciones del SRS.

**Servicio(s) responsable(s):** `monolith-laravel (Modulo: Agencias)`
**Endpoint(s) implementado(s):** `POST /api/v1/agencias/registro`
**Eventos generados:** `AgencyRegistered`
**Permisos requeridos:** Público / Usuario no autenticado

---

## Definición de Hecho (Definition of Done - DoD)

> Esta Historia de Usuario solo puede cerrarse cuando cumple con el DoD completo del equipo.
> Ver: `00-governance/definition-of-done.md`

**Verificaciones adicionales específicas para esta HU:**
- [ ] Migración de base de datos ejecutada y verificada para la tabla `agencias`.
- [ ] Validaciones de formato y longitud en tiempo real para los campos NIT y RNT activadas en el frontend (Bootstrap/Tailwind).
- [ ] Conexión con el servidor de correo SMTP configurada correctamente en el entorno de desarrollo para el despacho de alertas.

---

## Estimación y prioridad

| Campo | Valor |
|-------|-------|
| Story Points | 3 SP |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 |
| Dependencias | Ninguna |
