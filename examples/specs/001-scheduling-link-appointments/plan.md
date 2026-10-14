# Implementation Plan: Citas de revisión anual por link de agenda

> Ejemplo inventado (ver [spec](spec.md)).

**Branch**: `main` | **Date**: 2026-01-12 | **Spec**: [spec.md](spec.md)

## Summary

Dos columnas nuevas en el pipeline "Annual Review" disparan dos workflows de envío (15 y 60 min) que mandan el link de agenda prellenado por el canal del cliente. Una copia del workflow de confirmación escucha los calendarios de la agenda externa y mueve la oportunidad a "Appointment Set". Todo en Draft; se prueba con "Demo Tester".

## Technical Context

- **Plataforma**: GoHighLevel (white-label), sub-cuenta única.
- **Acceso**: API de LeadConnector (lectura de workflows, pipelines, contactos; escritura con `dryRun` + `idempotencyKey`) y navegador con la sesión de la operadora para el builder de workflows (la API no expone pasos ni triggers).
- **Integración**: agenda externa → calendarios de GHL (uno por duración).
- **Testing**: manual, con un contacto de prueba con tag `test`. No hay tests automáticos.
- **Restricciones**: los workflows publicados están en vivo; no se tocan. Verificación en dos pasos solo por email.

## Constitution Check

| Regla | Cómo se cumple |
|---|---|
| Lectura primero | Línea base de versiones de los workflows antes de empezar (T003). |
| No tocar lo publicado | Se trabaja sobre copias en Draft. |
| Stop por pipeline | T014 agrega los workflows nuevos al Stop del pipeline. |
| Sin datos de salud | Los textos solo llevan el link de agenda. |

## Project Structure

```text
specs/001-scheduling-link-appointments/
├── spec.md
├── plan.md          # este archivo
├── tasks.md
├── execution-log.md # IDs creados y resultado de cada prueba
└── handoff.md       # entrega y lista de activación
```

## Decisiones

| Decisión | Alternativa descartada | Por qué |
|---|---|---|
| Un trigger por calendario | Un trigger con dos calendarios | El filtro "In calendar" acepta un solo valor. |
| Link prellenado con `{{contact.first_name}}%20{{contact.last_name}}` | `{{contact.name}}` | El espacio sin codificar rompe el link en SMS. |
| Copia del workflow de confirmación | Editar el original | El original está publicado y en uso. |
