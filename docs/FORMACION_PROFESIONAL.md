# Formación profesional

Competencias que demuestra este proyecto, con evidencia en el repo. El contexto es laboral: operación diaria y automatización del CRM de una agencia de seguros de Medicare (anonimizada), septiembre–octubre de 2026.

## Competencias

| Competencia | Evidencia |
|---|---|
| **Automatización de CRM (GoHighLevel)**: pipelines, workflows, triggers, If/Else, esperas por fecha y dinámicas, custom fields y custom values, trigger links, formularios, Smart Lists, Bulk Actions | [docs/AUTOMATIZACIONES.md](AUTOMATIZACIONES.md), secciones 1–4 |
| **Diseño de reglas de negocio reutilizables** (un Stop por pipeline, canal preferido con prioridad SMS, copias en Draft en lugar de editar lo publicado) | AUTOMATIZACIONES §1–3; [examples/CLAUDE.md](../examples/CLAUDE.md) |
| **Integración de APIs y servicios** (LeadConnector API, agenda externa, Zoom, webhook → Google Apps Script) | AUTOMATIZACIONES §2, §4, §5; [skills/ghl-connect](../skills/ghl-connect/SKILL.md) |
| **Desarrollo guiado por especificaciones** (spec → plan → tasks → execution log → handoff) aplicado a no-code | [examples/specs/](../examples/specs/001-scheduling-link-appointments/) |
| **Pruebas y control de cambios en producción**: línea base de versiones, contacto y tag de prueba, pruebas por rama, limpieza, lista de activación | handoff y tasks del ejemplo; AUTOMATIZACIONES "Método común" |
| **Ingeniería de agentes de IA / prompt engineering**: skills de Claude Code con disparadores, pasos, límites y formato de salida | [skills/](../skills/) |
| **Seguridad operativa con IA**: 2FA solo por email, credenciales que escribe la persona, OAuth con confirmación, `dryRun` + `idempotencyKey`, contenido externo como dato y no como instrucción | [skills/ghl-connect](../skills/ghl-connect/SKILL.md) |
| **Manejo de datos personales y de salud** con criterio (nada de datos de salud por email/SMS, listas solo en repo privado, anonimización para publicar) | examples/CLAUDE.md; este repo |
| **Compliance en marketing regulado** (Medicare): vocabulario permitido, avisos obligatorios, paquetes de revisión | [skills/client-update](../skills/client-update/SKILL.md); AUTOMATIZACIONES §6 |
| **Automatización de documentos** (Python estándar → HTML → PDF/DOCX) | AUTOMATIZACIONES §6 |
| **Desarrollo web** (HTML/CSS responsive, modo oscuro, SEO local, JSON-LD) dentro de un builder | AUTOMATIZACIONES §7 |
| **Gestión del trabajo y comunicación con el cliente**: lista única de tareas, priorización por fechas fijas, reportes en primera persona | [skills/daily-plan](../skills/daily-plan/SKILL.md); [examples/pendientes.md](../examples/pendientes.md) |
| **Documentación para continuidad** (memoria de proyecto, handoffs, runbooks) | examples/CLAUDE.md; handoff del ejemplo |

## Retos y aprendizajes

- **La API no lo cubre todo.** LeadConnector no expone pasos, triggers ni logs de los workflows. Aprendí a dividir el trabajo: la API para leer y verificar versiones, y el navegador para construir en el builder, con un runbook de trucos para un editor lento y en iframe.
- **Cambiar algo que está en vivo.** Con cientos de clientes esperando en pasos de fecha fija, editar el workflow original era arriesgado. Lo resolví con copias en Draft, filtros de prueba y un plan de en qué paso entra cada cliente al migrar.
- **Limitaciones del producto.** Los filtros de trigger aceptan un solo valor y un nombre con espacio rompe un link en SMS. Cada hallazgo quedó escrito como regla para no repetir el error.
- **Efectos secundarios entre campañas.** "Remove from All Workflows" sacaba a los clientes también del newsletter y de las reseñas. De ahí salió la regla del Stop por pipeline.
- **Trabajar con IA de forma segura y verificable.** Las skills ponen límites explícitos (qué no hace la IA, qué confirma la persona) y dejan rastro en git de cada cambio.
- **Escribir para un público regulado.** Aprendí qué palabras evita compliance en Medicare y a armar paquetes de revisión con anticipación.

## Impacto

- **Rutina diaria:** me ahorra más de 8 horas a la semana de organizar notas, correos y archivos y de armar reportes (estimación propia).
- **Paquetes de compliance:** pasaron de ~1 día de armado a mano a ~1 hora con los scripts (estimación propia).
- **Campaña de revisión anual:** las automatizaciones cubren una campaña de cientos de clientes. Las confirmaciones salen por el canal que usó cada cliente y las citas se piden moviendo una sola columna.
- **Regla Stop:** los clientes cerrados (ganados o perdidos) salen solos de la campaña, sin mensajes de más.
- **Cambios en producción sin incidentes:** todo se construyó en Draft y se probó con un contacto de prueba antes de activarse; no se editó ningún workflow en vivo.
