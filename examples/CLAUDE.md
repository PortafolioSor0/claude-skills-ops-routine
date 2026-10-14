# CLAUDE.md (ejemplo)

> Versión inventada y resumida de la memoria de proyecto que usan las skills. Nombres e IDs son ficticios.

## Trato con la operadora
Empieza cada respuesta con el nombre de la operadora. Habla en español; lo que ve el cliente va en inglés.

## Todo vive en este repo
- Documentos, plantillas, notas y listas se guardan aquí, nunca fuera.
- Commits al terminar cada trabajo, en `main`, con la operadora como autora y sin atribución a IA. Los push los hace la operadora.

## Rutina diaria
1. `/daily-intake` → 2. `/daily-plan` → 3. `/client-update`. `docs/pendientes.md` es la lista única de tareas.

## Conexión con el CRM (GoHighLevel)
- Conector `leadconnector`; sub-cuenta `demo-location-0001` en cada `execute_operation`.
- Verificación en dos pasos **siempre por email**: el teléfono de la cuenta no es de la operadora.
- Contraseñas y códigos los escribe la operadora.
- Lectura primero. Toda escritura: confirmar, `dryRun: true` e `idempotencyKey`.
- Los workflows publicados están en vivo: probar con "Demo Tester" (`demo-contact-0001`).
- **Regla Stop:** cada pipeline tiene su workflow "<Pipeline>: Won / Lost / Abandoned ➡️ Stop campaign" que saca solo de los workflows de ese pipeline. Todo workflow nuevo se agrega el mismo día a su Stop.
- La API no expone pasos, triggers ni logs de workflows: para eso, navegador con la sesión de la operadora.
- Nunca guardar tokens ni claves en el repo (`.env` en `.gitignore`).

## Reglas de contenido (cliente y compliance)
- Nada de "free": "no-obligation" o "no cost to you".
- "Medicare Annual Enrollment Period", nunca "Open Enrollment".
- Nunca pedir datos de salud por email o SMS; se usa el link de la revisión segura en línea.
- Sin nombres de aseguradoras en los posts.
- En cada pieza: teléfono con TTY, horario, línea de licencia y el aviso de organizaciones/productos.
- Todo pasa por compliance unas 2 semanas antes de publicarse.

## Reportes al cliente
Se muestran en la reunión diaria: página nueva por reunión, solo lo nuevo, en primera persona, sin mencionar IA.

## Lecciones de herramientas
- Granola: leer resumen **y** transcripción.
- Gmail: el conector no lee adjuntos.
- Builder de workflows: zoom solo con + / −; desplegables con teclado; escribir solo con el campo en foco.
