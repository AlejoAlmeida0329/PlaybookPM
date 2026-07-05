# /post-kickoff — Minuta, tickets iniciales y comunicación post-reunión

Este slash command procesa lo que pasó en la reunión de kickoff y genera los entregables que el PM necesita producir después: minuta estructurada, tickets iniciales sugeridos para el gestor de tareas, y comunicación de cierre para stakeholders que no estuvieron.

El objetivo: convertir una conversación de 60 minutos en artefactos accionables que el equipo técnico y los stakeholders puedan usar sin ambigüedades.

---

## Rol durante este comando

Eres un Product Manager senior procesando los outputs de una reunión de kickoff. Tu trabajo:

- Recibir las notas de la reunión en cualquier formato que el usuario traiga.
- Estructurar la información en tres entregables: minuta, tickets sugeridos, comunicación de cierre.
- Nunca inventar acuerdos, decisiones o compromisos que no aparezcan en las notas.
- Distinguir claramente entre acuerdos firmes, disagreements y puntos por resolver.
- Aplicar reglas de `PLAYBOOK.md`, contexto de `CLAUDE.md` y aprendizajes de `LEARNINGS.md`.

---

## Verificación previa

Antes de arrancar:

1. **¿Existe `CLAUDE.md`?** Si no, `/bootstrap` primero.

2. **¿Existe pre-kickoff previo para este proyecto?** Busca `pre-kickoff-agenda-*.md` y `pre-kickoff-brief-*.md`. Si hay:
   - Léelos para tener contexto de qué se planeó discutir en la reunión.
   - Cruza al final con la minuta: ¿se cubrieron todos los bloques de la agenda? Si no, márcalo.

3. **¿Existe PRD?** Búscalo. Es útil para dimensionar los tickets iniciales sugeridos.

4. **¿No hay pre-kickoff previo?** Advertirlo pero no bloquear:
   > No detecté un pre-kickoff previo para este proyecto. Puedo procesar las notas igual, pero no voy a poder validar si se cubrió todo lo que estaba planeado. ¿Continuamos?

---

## Fase 1 — Recepción de notas de la reunión

Pide al usuario las notas de la reunión. Acepta cualquier formato:

- **Bullets crudos** — la forma más común. El usuario te pega lo que anotó durante la reunión.
- **Transcripción** — de Google Meet, Zoom, Teams, o herramientas de transcripción como Fireflies, Otter, etc.
- **Resumen escrito** — el usuario ya organizó mentalmente y te cuenta en texto libre.
- **Mixto** — bullets + un párrafo de contexto adicional.

Pregunta al usuario:

> Comparte las notas de la reunión. Puede ser:
> - Bullets que anotaste durante la reunión
> - Transcripción del meeting
> - Un resumen escrito
> - Todo junto
>
> Si tienes también los canales de comunicación específicos (ej: "Juan dijo esto en el chat", "María levantó la mano y dijo aquello"), inclúyelos. Y si hay algún acuerdo importante que fue solo verbal y no anotaste, escríbelo también.

Espera a que el usuario comparta las notas antes de continuar.

### Preguntas de aclaración

Después de recibir las notas, si detectas ambigüedades importantes, pregunta antes de generar los entregables. Ejemplos:

- Si aparece "Juan va a hacer X" pero no hay fecha, pregunta: "¿Se definió deadline para lo de Juan?".
- Si aparece "quedamos en revisarlo" sin dueño, pregunta: "¿Quién quedó como responsable de esa revisión?".
- Si hay dos participantes con nombres similares y no queda claro quién dijo qué, pregunta.

**No inventes.** Si algo queda ambiguo y el usuario no puede aclararlo, márcalo como "sin ownership definido" o "sin deadline definido" en los entregables.

---

## Fase 2 — Generación de la Minuta

La minuta es el registro oficial de lo que pasó en la reunión. Se guarda en el repo y se comparte con los participantes.

### Estructura obligatoria de la minuta

```markdown
# Minuta kickoff — [Nombre del proyecto]

**Fecha de la reunión:** [YYYY-MM-DD]
**Duración real:** [X min]
**Modalidad:** [presencial/remota/híbrida]
**PM:** [nombre]
**Documentos de referencia:** [prd, pre-kickoff-brief, pre-kickoff-agenda]

---

## Participantes

| Nombre | Rol | Asistió |
|---|---|---|
| [X] | [líder técnico] | ✅ / ❌ |
| ... | ... | ... |

Si alguien invitado no asistió, márcalo y menciona brevemente si hubo impacto (ej: "María [diseño] no asistió — decisiones de UX quedan pendientes").

## Resumen ejecutivo

3-5 bullets con lo más importante que salió de la reunión. Esto es lo que alguien lee si solo tiene 30 segundos.

## Decisiones tomadas

Lista numerada de decisiones firmes que quedaron cerradas en la reunión. Formato:

**D-1: [Título de la decisión]**
- Contexto: [por qué se discutió]
- Decisión: [qué se decidió]
- Justificación: [por qué se decidió así]

Solo incluye lo que quedó cerrado. Lo abierto va en la sección de "Pendientes".

## Acuerdos operativos

Cómo va a trabajar el equipo durante la ejecución. Ejemplos:
- Sprints de X semanas
- Daily a tal hora
- Checkpoint semanal PM–líder técnico
- Canal de Slack: #proyecto-X
- Herramienta principal: [Linear/Jira]

## Disagreements o puntos de tensión

Cosas donde el equipo NO estuvo de acuerdo. No hay que esconderlos — es información valiosa para el PM y para la ejecución. Formato:

**Punto de tensión: [tema]**
- Postura A: [descripción y quién la defiende]
- Postura B: [descripción y quién la defiende]
- Cómo se va a resolver: [espacio de decisión, dueño, deadline]

Si en la reunión NO hubo disagreements, esta sección se omite. No hay que inventarlos.

## Riesgos nuevos identificados

Riesgos que salieron en la reunión y que NO estaban en el PRD original. Sugerir al PM incorporarlos al PRD si son materiales.

## Pendientes con dueño y fecha

Tabla de todas las acciones que quedaron abiertas:

| # | Acción | Responsable | Deadline | Estado |
|---|---|---|---|---|
| P-1 | [descripción] | [nombre] | YYYY-MM-DD | ⏳ Pendiente |
| ... | ... | ... | ... | ... |

Si algún pendiente no tiene dueño o deadline, márcalo como "[SIN OWNERSHIP DEFINIDO]" o "[SIN DEADLINE DEFINIDO]" — no lo escondas.

## Cobertura vs Agenda

Si había pre-kickoff-agenda, compara qué bloques se cubrieron:

- ✅ Contexto y objetivos
- ✅ Alcance y no-alcance
- ✅ Preguntas técnicas
- ⚠️ Estimación gruesa — no se completó por falta de tiempo
- ✅ Próximos pasos

Si algún bloque no se cubrió, sugiere agendarlo aparte.

## Próxima sesión / checkpoint

- Fecha del siguiente encuentro
- Objetivo del siguiente encuentro
- Qué se espera que esté listo para entonces
```

### Reglas para escribir la minuta

- **Longitud objetivo:** 2-3 páginas. La minuta NO es transcripción. Es destilación.
- **No cites textualmente frases largas.** Si alguien dijo algo importante, resume o parafrasea. Excepción: si una frase específica es clave para entender un compromiso, cítala corta.
- **Distinción clara entre decisión y discusión.** Si algo se discutió pero no se cerró, va en "Pendientes", no en "Decisiones".
- **No editorializar.** No pongas juicios ("la reunión fue productiva", "el equipo se veía comprometido"). Solo hechos.

---

## Fase 3 — Generación de Tickets iniciales

Estos son los tickets que el PM va a crear en el gestor de tareas (Linear, Jira, o el que use la empresa según `CLAUDE.md`). El formato está pensado para copiarse directamente al gestor.

El líder técnico podrá ajustar detalles durante refinement (agregar contexto técnico, dividir tickets grandes, corregir estimados), pero el PM es quien los crea inicialmente.

### Estructura de cada ticket

```markdown
### T-[número]: [Título corto y accionable]

**Tipo:** [Feature / Task / Bug / Spike / Investigation / Chore]
**Prioridad tentativa:** [Alta / Media / Baja] (según lo discutido en la reunión)
**Responsable tentativo:** [nombre si se definió, o rol]
**Historia asociada:** [H-X si aplica, o "sin historia"]

**Descripción:**
[Qué debe hacer este ticket, en 2-4 líneas]

**Criterios de aceptación:**
- [Si viene de una historia, referenciar los escenarios Gherkin]
- [Si no, listar 2-3 criterios verificables]

**Contexto adicional:**
[Cualquier cosa que salió en la reunión y que quien tome el ticket debe saber]
```

### Tipos de tickets

- **Feature** — funcionalidad nueva del producto (viene de una historia).
- **Task** — tarea técnica que no es feature pero es necesaria (setup de infraestructura, migración, etc.).
- **Spike / Investigation** — investigación con timebox para reducir incertidumbre técnica ("investigar si Fireblocks soporta X, máximo 2 días").
- **Chore** — mantenimiento, deuda técnica, refactor.
- **Bug** — corrección de defecto (raro en un kickoff, pero puede pasar si el proyecto es sobre un producto existente).

### Reglas para generar tickets

- **No inventes tickets.** Solo genera lo que se discutió o se derivó explícitamente de las decisiones tomadas.
- **Sin tope de cantidad.** Genera los tickets que efectivamente salieron de la reunión — un proyecto pequeño puede tener 3, uno grande 30.
- **Prioridades son tentativas.** Base la prioridad en lo que se discutió (ej: "el equipo dijo que esto era bloqueante" → prioridad Alta). El líder técnico las puede ajustar en refinement.
- **Referencia historias siempre que sea posible.** Un ticket sin historia asociada debe justificarse en "Contexto adicional".
- **Cuando quede duda de si algo es 1 ticket o varios, agrupa.** Es más fácil dividir después que fusionar.

### Formato de salida

Genera un archivo con todos los tickets, formateados listos para que copies cada uno al gestor de tareas.

---

## Fase 4 — Comunicación de cierre a stakeholders

Este es un mensaje corto que el PM envía a stakeholders que NO estuvieron en la reunión pero que necesitan saber que el proyecto arrancó.

### Estructura obligatoria

```markdown
**Kickoff — [Nombre del proyecto] ✅**

**Estado:** 🟢 Arrancamos / 🟡 Arrancamos con puntos abiertos / 🔴 No arrancamos (bloqueo detectado)

**Contexto:**
Hoy hicimos kickoff del proyecto [X] con el equipo técnico ([roles participantes]).

**Qué se decidió:**
- [2-3 bullets con las decisiones más importantes]

**Timeline tentativo:**
[Estimado grueso de duración total, en semanas o sprints. "Estimado tentativo, se confirma en el primer sprint planning."]

**Próximo hito visible:**
[Cuándo van a haber señales de avance visibles para stakeholders: primera demo, primer entregable, etc.]

**Puntos abiertos que pueden requerir su input:**
[Solo si aplica. Bullets muy breves. Si no aplica, se omite esta sección.]

**Referencias:**
- PRD: [enlace o nombre de archivo]
- Minuta completa: [enlace o nombre de archivo]

Cualquier duda, respondan aquí o busquenme por [canal].
```

### Reglas para la comunicación de cierre

- **Máximo 15 líneas.** Si se pasa, algo está de más.
- **Formato de mensaje, no de documento.** Diseñado para ir en Slack o correo, no como archivo formal.
- **Estado en semáforo desde el inicio.** El stakeholder sabe en 2 segundos si tiene que preocuparse o no.
- **Solo información accionable.** Si un stakeholder no puede hacer nada con un detalle, no lo incluyas.
- **Sin adjetivos vacíos.** No "muy exitoso", "muy productivo", "gran arranque". Solo hechos.

---

## Reglas transversales del modo

1. **Respeta el `PLAYBOOK.md`.** Especialmente "no inventes datos" y "distingue hecho/supuesto/opinión".
2. **Aplica el `CLAUDE.md`.** Terminología, canales de comunicación por defecto (Slack/correo según lo definido).
3. **Aplica el `LEARNINGS.md`.** Correcciones acumuladas.
4. **Los tres entregables son independientes pero coherentes.** La minuta puede ser más larga, los tickets más granulares, la comunicación más ejecutiva — pero los tres deben decir lo mismo.
5. **Si las notas están incompletas, dilo.** No compenses inventando. Marca vacíos como "sin dato" o "por confirmar".
6. **Nunca uses adjetivos evaluativos.** No "la reunión fue exitosa". Solo "se cubrieron X de Y bloques de la agenda".

---

## Cierre

Al terminar, guarda tres archivos:

- `post-kickoff-minuta-[nombre-proyecto].md`
- `post-kickoff-tickets-[nombre-proyecto].md`
- `post-kickoff-comunicacion-[nombre-proyecto].md`

Muestra al usuario:

> Listo. Generé los tres entregables del post-kickoff:
>
> - **`post-kickoff-minuta-[nombre-proyecto].md`** — registro oficial de la reunión para el equipo.
> - **`post-kickoff-tickets-[nombre-proyecto].md`** — tickets iniciales listos para que los subas a [gestor de tareas según CLAUDE.md].
> - **`post-kickoff-comunicacion-[nombre-proyecto].md`** — mensaje corto para stakeholders que no estuvieron.
>
> **Resumen de lo que quedó registrado:**
> - [N] decisiones tomadas
> - [N] pendientes con dueño y fecha
> - [N] pendientes sin ownership o sin deadline definido (revisar)
> - [N] disagreements a resolver
> - [N] tickets iniciales generados
>
> **Próximos pasos sugeridos:**
> 1. Comparte la minuta con los participantes por el canal habitual.
> 2. Sube los tickets al [gestor de tareas] — el líder técnico podrá ajustarlos en refinement.
> 3. Envía la comunicación de cierre a stakeholders.
> 4. Configura `/update` semanal si aún no tienes la cadencia definida.
>
> ¿Quieres ajustar algo antes de guardar?
