# Tasks: Citas de revisión anual por link de agenda

> Ejemplo inventado (ver [spec](spec.md) y [plan](plan.md)).

**Tests**: no hay tests automáticos. La validación es manual con "Demo Tester" (`demo-contact-0001`) y está dentro de cada historia.

**Reglas para todas las tareas**:
- Todo se construye en **Draft**.
- No se editan workflows publicados.
- En "Test workflow" solo se elige al contacto de prueba.
- Cada ID nuevo se anota en `execution-log.md`.
- Commit al cerrar cada fase.

## Format: `[ID] [P?] [Story] Description`

## Phase 1: Setup

- [X] T001 Crear en el pipeline "Annual Review" las columnas "Request Appointment 15 min" (`stage-demo-15`) y "60 min" (`stage-demo-60`). Por navegador (la API no crea etapas); comprobado con `get-pipelines`.
- [X] T002 Redactar los textos de SMS y email en `contracts/messages.md` siguiendo el estilo de los mensajes existentes.
- [X] T003 Crear `execution-log.md` con la foto previa (versión) de cada workflow que se va a copiar.
- [X] T004 OK de la operadora a los textos de T002. No se escribe ningún mensaje en GHL antes.

## Phase 2: Foundational

- [X] T005 Comprobar que "Demo Tester" tiene el tag `test` y su oportunidad está en una columna sin trigger.

**Checkpoint**: textos aprobados y línea base guardada.

## Phase 3: User Story 1 (P1) 🎯 MVP

- [X] T006 [US1] Crear en Draft "Annual Review: Request Appointment 15 min ➡️ Scheduling link" (trigger "Pipeline stage changed", Allow re-entry).
- [X] T007 [US1] Paso "Remove from Workflow" con la secuencia de la encuesta (original y copia).
- [X] T008 [US1] If/Else "Canal": Solo SMS / Solo Email / Ambos, según `Review Channel` y si hay teléfono o email.
- [X] T009 [US1] Probar las tres ramas con Test workflow y anotar el resultado.
- [X] T010 [P] [US1] Duplicar para 60 min y cambiar link y etapa.

## Phase 4: User Story 2 (P2)

- [X] T011 [US2] Copia en Draft del workflow de confirmación, con un trigger por calendario de la agenda externa y filtro `Has tag = test`.
- [X] T012 [US2] Reserva real con "Demo Tester" (con OK de la operadora): la oportunidad pasa a "Appointment Set" en ~1 min.

## Phase 5: User Story 3 (P3)

- [ ] T013 [US3] Wait 2 días → recordatorio por el mismo canal; Wait 2 días → tarea para Jordan. Probar con esperas cortas.

## Phase 6: Polish

- [X] T014 Agregar los workflows nuevos al Stop "Annual Review: Won / Lost / Abandoned ➡️ Stop campaign".
- [X] T015 Limpieza: cancelar reservas de prueba, devolver al contacto de prueba a su etapa y dejar las copias en Draft.
- [X] T016 Escribir `handoff.md` con IDs, resultados y lista de activación.
