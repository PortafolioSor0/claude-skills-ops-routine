# Entrega: citas por link de agenda (spec 001)

> Ejemplo inventado. Los IDs son ficticios.

Construido y probado el 2026-01-14. Falta la activación de la operadora.

## Qué hay en el CRM

| Pieza | ID | Estado |
|---|---|---|
| Columna "Request Appointment 15 min" | `stage-demo-15` | Creada |
| Columna "Request Appointment 60 min" | `stage-demo-60` | Creada |
| "Annual Review: Request Appointment 15 min ➡️ Scheduling link" | `wf-demo-0015` | Draft |
| "Annual Review: Request Appointment 60 min ➡️ Scheduling link" | `wf-demo-0060` | Draft |
| "Appointment Set ➡️ Reminders (copia)" | `wf-demo-0106` | Draft, con el filtro `test` |

Los workflows publicados **no se tocaron**; mantienen la misma versión que en la línea base.

## Resultados de las pruebas (con Demo Tester)

| Prueba | Resultado |
|---|---|
| Envío 15 min, canal SMS / Email / vacío | ✅ Cada caso toma la rama correcta. |
| Link prellenado | ✅ Corregido: el nombre va con `%20`. |
| Reserva en 15 min | ✅ Pasa a Appointment Set en ~1 min. |
| Cancelación | ✅ A los 5 min pasa a Cancelation. |
| Recordatorio y tarea (T013) | ⏳ Pendiente de autorizar. |

## Lista de activación (operadora)

1. En la copia del workflow de confirmación, cambiar en **cada trigger** el filtro `Has tag` de `test` al tag real de la campaña y publicar.
2. Publicar los envíos de 15 y 60 min.
3. Confirmar que los dos aparecen en el Stop del pipeline.
4. Pasar el workflow de confirmación original a Draft solo si ya no escucha ningún calendario en uso.

## Cosas a saber

- "In calendar" y "Has tag" aceptan **un valor** por trigger.
- Para repetir una prueba con Test workflow, primero sacar al contacto desde Enrollment history; si no, sale "Skipped".
