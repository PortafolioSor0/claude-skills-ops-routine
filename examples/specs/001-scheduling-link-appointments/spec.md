# Feature Specification: Citas de revisión anual por link de agenda

> Ejemplo inventado. Agencia, personas, IDs y datos son ficticios; reproduce la estructura de una spec real de spec-kit adaptada a un CRM (GoHighLevel).

**Feature Branch**: `main` (el repo se trabaja siempre sobre `main`)

**Created**: 2026-01-12

**Status**: Draft

**Input**: User description: "Dos columnas nuevas en el pipeline 'Annual Review' ('Request Appointment 15 min' y 'Request Appointment 60 min'). Cuando Jordan mueve a un cliente a una de ellas, el cliente recibe el link de agenda de esa duración, prellenado con su nombre y email, por el canal que usó (campo `Review Channel`; si está vacío, SMS y email), con un recordatorio si no reserva. Al reservar, la oportunidad pasa a 'Appointment Set' y el cliente sale de los workflows de envío. Al cancelar, pasa a 'Cancelation'. Todo se construye en Draft y se prueba con el contacto de prueba; la operadora activa."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Pedirle al cliente que reserve con un solo movimiento (Priority: P1)

Cuando Jordan revisa a un cliente y decide que necesita una llamada, lo mueve a "Request Appointment 15 min" o "60 min". Sin escribir nada más, el cliente recibe el link de esa duración, con su nombre y email ya puestos, por el canal que usó.

**Why this priority**: hoy Jordan lo hace a mano, cliente por cliente. Por sí sola ya le ahorra ese trabajo.

**Independent Test**: con el contacto "Demo Tester" lanzado en el workflow de envío de 15 min (en Draft, con "Test workflow"), comprobar que le llega el link por el canal de su campo `Review Channel` y que la página de agenda muestra su nombre y email ya escritos.

**Acceptance Scenarios**:

1. **Given** un cliente con canal "SMS", **When** Jordan lo mueve a "Request Appointment 15 min", **Then** recibe solo por SMS el link de 15 min.
2. **Given** un cliente con canal "Email", **When** lo mueve a "Request Appointment 60 min", **Then** recibe solo por email el link de 60 min.
3. **Given** un cliente sin canal, **When** lo mueve a cualquiera de las dos columnas, **Then** recibe el link por SMS y por email.
4. **Given** un cliente que seguía en la secuencia de recordatorios de la encuesta, **When** lo mueven a una columna "Request Appointment", **Then** sale de esa secuencia.

---

### User Story 2 - La reserva mueve al cliente sola (Priority: P2)

Cuando el cliente reserva, su oportunidad pasa sola a "Appointment Set" y deja de recibir los mensajes de "reserva tu cita".

**Independent Test**: reservar con los datos de "Demo Tester" y comprobar que la oportunidad pasa a "Appointment Set" y sale de los workflows de envío. Dispara un workflow publicado: solo con el OK de la operadora.

**Acceptance Scenarios**:

1. **Given** un cliente en "Request Appointment 15 min", **When** reserva, **Then** pasa a "Appointment Set" y no recibe el recordatorio de reserva.
2. **Given** una cita que llega duplicada por Google Calendar a otro calendario, **When** entra, **Then** los workflows nuevos no hacen nada con ella.

---

### User Story 3 - Recordatorio si no reserva (Priority: P3)

Si el cliente no reserva en 2 días, recibe un recordatorio. Si tampoco reserva en los 2 días siguientes, Jordan recibe una tarea para llamarlo.

**Acceptance Scenarios**:

1. **Given** un cliente que no reservó, **When** pasa el plazo, **Then** recibe el recordatorio por el mismo canal que el primer mensaje.
2. **Given** un cliente que reservó antes del plazo, **When** llega el plazo, **Then** no recibe nada.

### Edge Cases

- Nombre con espacio interno ("Mary Ann"): el parámetro del link debe ir codificado (`%20`).
- Reprogramación: el sistema de agenda la envía como actualización de la misma cita, no como cancelar + reservar.
- Cliente sin email ni teléfono válidos: no se envía nada y se crea una tarea para Jordan.

## Requirements *(mandatory)*

- **FR-001**: El envío se dispara solo por cambio de etapa del pipeline "Annual Review".
- **FR-002**: El canal se elige con el campo `Review Channel` (SMS, Email o vacío).
- **FR-003**: Ningún workflow publicado existente se edita; donde hace falta cambiar uno, se trabaja sobre una copia en Draft.
- **FR-004**: Todo workflow nuevo del pipeline se agrega ese mismo día a la lista del workflow "Won / Lost / Abandoned ➡️ Stop campaign" de ese pipeline.
- **FR-005**: No se pide ningún dato de salud por SMS ni email.

## Success Criteria *(mandatory)*

- **SC-001**: Jordan pide una cita con un solo movimiento de columna, sin escribir mensajes.
- **SC-002**: Ningún cliente con cita reservada recibe un recordatorio de "reserva tu cita".
- **SC-003**: Las pruebas con el contacto de prueba cubren las tres ramas de canal.
