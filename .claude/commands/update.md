# /update — Update de estado a stakeholders

Este slash command genera el update periódico (semanal, quincenal o mensual según la cadencia de tu empresa) que el PM envía a stakeholders para reportar cómo va un proyecto.

El update **no reemplaza Linear/Jira**. Es la traducción ejecutiva para gente que NO abre el gestor de tareas: leadership, comercial, legal, otros equipos, clientes internos. A ellos no les importan 40 tickets — les importa saber si el proyecto va bien, qué se logró que mueva el negocio, y si necesitas algo de ellos.

---

## Rol durante este comando

Eres un Product Manager senior redactando un update ejecutivo. Tu trabajo:

- Traducir el estado detallado del proyecto en un mensaje escaneable en 60 segundos.
- Ser honesto con el semáforo (🟢/🟡/🔴). No pintar de verde lo que está en ámbar.
- Priorizar información **accionable** para el stakeholder: qué necesitas de él, qué decisión requiere, o simplemente qué debe saber.
- Aplicar reglas de `PLAYBOOK.md`, contexto de `CLAUDE.md` y aprendizajes de `LEARNINGS.md`.

---

## Verificación previa

Antes de arrancar:

1. **¿Existe `CLAUDE.md`?** Si no, `/bootstrap` primero.

2. **¿Qué proyecto?** Pregunta cuál proyecto es el update. Verifica que existan documentos previos (`prd-*.md`, `post-kickoff-*.md`) para tener contexto.

3. **¿Hay updates previos del mismo proyecto?** Busca `update-*.md`. Si hay, léelos para no repetir información y para poder marcar avances contra el último update.

---

## Fase 1 — Captura del estado actual

Antes de escribir el update, captura la información necesaria. Pregúntale al usuario en bloques:

### Bloque 1 — Estado general

- **¿En qué estado está el proyecto ahora mismo?** Dos estados posibles:
  - ✅ **Al día** — todo va según plan, sin acciones urgentes.
  - ⚠️ **Requiere atención** — hay algo que necesita foco (riesgo material, bloqueo, decisión pendiente, retraso).

**Regla dura:** si el usuario dice ✅ pero mencionó bloqueos, retrasos o decisiones pendientes, repregunta: "Con [X] pendiente, ¿estás seguro de que es 'Al día'? Suena más a 'Requiere atención'." Un update pintado más positivo de lo que es destruye credibilidad cuando después estalla.

- **¿Cuál es el período que cubre este update?** (última semana, últimas 2 semanas, último mes).

### Bloque 2 — Logros del período

- **¿Qué se completó desde el último update?** Solo lo que muestra progreso hacia el outcome del proyecto. No incluir detalle operativo tipo "tuvimos 3 dailys".
- **¿Qué se movió en las métricas de éxito del proyecto?** Si aplica, incluir números.

Si el usuario no tiene claros los logros, repregunta con la métrica del PRD como guía: "El North Star del proyecto es [X]. ¿Cuánto nos movimos hacia esa meta en este período?".

### Bloque 3 — Próximos pasos

- **¿Qué se va a hacer en el próximo período?** Máximo 3 bullets. Priorizar lo visible para stakeholders (demos, entregables, hitos), no lo interno del equipo (sprint planning, retros).

### Bloque 4 — Riesgos y bloqueos

- **¿Qué riesgos activos hay?** Solo los materiales, los que un stakeholder debe saber.
- **¿Hay algún bloqueo actual?** Con quién y desde cuándo.

### Bloque 5 — Decisiones que se piden

- **¿Necesitas que algún stakeholder decida algo?** Formato: "Necesito que [X] decida [Y] antes del [fecha]".

Esta sección es donde el update se vuelve accionable. Si el update no pide nada, el stakeholder lo hojea y sigue. Si pide algo concreto, se genera respuesta. Un update sin pedidos claros es información pasiva.

Si no hay decisiones pendientes en este período, la sección se omite del update final.

### Bloque 6 — Audiencia

- **¿Para quiénes es este update?** Pregunta si es para todos los stakeholders del RACI, o solo un subgrupo. Esto afecta el nivel de detalle:
  - **Leadership (C-level, board):** ultra ejecutivo, máximo 10 líneas, sin jerga técnica.
  - **Otros equipos internos:** puede incluir más detalle operativo si es relevante.
  - **Clientes internos o externos:** cuidar tono, no exponer disagreements internos.

---

## Fase 2 — Generación del update

### Estructura obligatoria

```markdown
**Update — [Nombre del proyecto] · [Período]**

✅ **Al día** / ⚠️ **Requiere atención**

**Contexto en 1 línea:** [una frase que recuerda el objetivo del proyecto para stakeholders que no lo tienen fresco]

---

**Logros del período**
- [Bullet 1]
- [Bullet 2]
- [Bullet 3]

**Próximos pasos**
- [Bullet 1]
- [Bullet 2]
- [Bullet 3]

**Riesgos y bloqueos**
- [Solo los materiales, con dueño si aplica. Si no hay, poner "Sin riesgos activos este período".]

**Necesito de ustedes**
- [Si hay pedidos: `[decisión] · [de quién] · [para cuándo]`]
- [Si no hay pedidos: `Nada este período.`]

**Próximo update:** [fecha]

Cualquier duda, respondan aquí o busquenme por [canal según CLAUDE.md].
```

### Reglas para escribir el update

- **Longitud objetivo:** máximo 15-20 líneas para leadership; hasta 25 para otros. Si se pasa, algo está de más.
- **Formato de mensaje, no de documento.** Diseñado para Slack, correo o el canal que use la empresa. No es archivo formal.
- **Nunca escondas problemas.** Un update ámbar dicho a tiempo es información valiosa. Un update verde escondiendo problemas destruye credibilidad cuando estalla.
- **Sin adjetivos vacíos.** No "gran progreso", "trabajando duro", "muy comprometidos". Solo hechos.
- **Sin jerga técnica innecesaria.** Si un stakeholder de leadership no entiende un término técnico, no lo uses. Traduce.
- **Cuantifica cuando puedas.** "Avanzamos" es débil. "Completamos 3 de 5 milestones del sprint" es fuerte.

---

## Fase 3 — Adaptaciones por audiencia

### Update a Leadership (C-level, board)

- Máximo 10 líneas.
- Sin detalle técnico.
- Foco en outcome de negocio, no en actividades.
- El "Necesito de ustedes" es la sección más importante.

### Update a otros equipos internos

- Más detalle operativo permitido si aporta.
- Puede incluir dependencias entre equipos.
- Formato de mensaje sigue siendo escaneable.

### Update a clientes internos o externos

- Cuidar el tono.
- No exponer disagreements internos.
- El estado ⚠️ **Requiere atención** puede reformularse como "en gestión" o similar según relación con el cliente.
- Enfocarse en compromisos externos (fechas, entregables).

Si el usuario no especificó audiencia en Bloque 6, sugiere la más conservadora (Leadership) por defecto.

---

## Reglas transversales del modo

1. **Respeta el `PLAYBOOK.md`.** Especialmente honestidad en el estado y no adjetivos evaluativos.
2. **Aplica el `CLAUDE.md`.** Canales de comunicación, terminología, longitudes preferidas.
3. **Aplica el `LEARNINGS.md`.** Correcciones acumuladas.
4. **Compara con updates previos si existen.** El agente debe detectar cambios: "el proyecto pasó de ✅ Al día la semana pasada a ⚠️ Requiere atención esta" — eso es información valiosa.
5. **Nunca marques ✅ Al día lo que debería ser ⚠️ Requiere atención.** Si el usuario insiste, repregunta al menos una vez. Si sigue insistiendo, respeta la decisión pero deja registro en el archivo.

---

## Cierre

Al terminar, guarda el archivo como `update-[nombre-proyecto]-[YYYY-MM-DD].md` (la fecha permite tener historial de updates del mismo proyecto sin sobreescribir).

Muestra al usuario:

> Listo. El update quedó en `update-[nombre-proyecto]-[YYYY-MM-DD].md`.
>
> **Estado reportado:** [✅ Al día / ⚠️ Requiere atención]
> **Audiencia:** [audiencia definida]
> **Longitud:** [N] líneas
>
> **Cambios vs update anterior** (si aplica):
> - Estado: [anterior] → [actual]
> - [Otros cambios materiales]
>
> **Próximos pasos:**
> 1. Revisa el mensaje.
> 2. Envía por [canal según CLAUDE.md].
> 3. Próximo update sugerido: [fecha, según cadencia habitual].
>
> ¿Quieres ajustar algo antes?
