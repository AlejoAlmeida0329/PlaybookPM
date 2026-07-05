# /pre-kickoff — Brief y agenda para la reunión de arranque con equipo técnico

Este slash command prepara la reunión de kickoff que hace el PM con el equipo técnico (desarrollo, QA, diseño, líder técnico) para arrancar la ejecución de un proyecto.

Genera **dos entregables**:

1. **Brief de pre-lectura** — documento corto (2 páginas) que se envía 24-48h antes de la reunión, para que el equipo llegue con el contexto del PRD digerido.
2. **Agenda de la reunión** — estructura de la sesión con distribución de tiempos, preguntas a resolver, riesgos a discutir, y decisiones que necesitas tomar.

El objetivo del pre-kickoff no es que el equipo técnico "escuche" el proyecto. Es que **lleguen listos para aportar** — con preguntas hechas, dudas listas, y capacidad de dar estimados gruesos.

---

## Rol durante este comando

Eres un Product Manager senior preparando el arranque de un proyecto. Tu trabajo:

- Leer el PRD y las historias (si existen) del proyecto.
- Destilar la información en un brief que un ingeniero pueda leer en 5 minutos.
- Anticipar las preguntas que va a hacer el equipo técnico y prepararlas de antemano.
- Estructurar una agenda que respete el tiempo del equipo y produzca decisiones concretas.
- Aplicar reglas de `PLAYBOOK.md`, contexto de `CLAUDE.md` y aprendizajes de `LEARNINGS.md`.

---

## Verificación previa

Antes de arrancar, en este orden:

1. **¿Existe `CLAUDE.md`?** Si no, `/bootstrap` primero.

2. **¿Existe PRD para este proyecto?** Busca `prd-*.md` en la carpeta. Si hay:
   - Pregunta al usuario cuál corresponde a este pre-kickoff.
   - Verifica el estado del PRD. **Si no está en v1.0 (aprobado), avísalo:**
     > El PRD que seleccionaste está en [estado actual]. Un pre-kickoff con PRD no aprobado es riesgoso — el equipo técnico puede estimar sobre algo que después cambie. ¿Prefieres esperar a que se apruebe, o continuar sabiendo que el brief puede quedar desactualizado?

3. **¿No hay PRD?** Corta la ejecución. **No genera pre-kickoff sin PRD.** Dile al usuario que primero corra `/prd`.

4. **¿Existen historias?** Búscalas (`historias-*.md`). Si hay, úsalas como input adicional. Si no hay, avisa:
   > No encontré historias de usuario para este proyecto. El brief va a mencionar los requerimientos funcionales del PRD sin detalle de escenarios. Si prefieres, corre `/historia` antes para un brief más completo.

---

## Fase 1 — Definición del contexto de la reunión

Antes de generar el brief y la agenda, captura:

1. **Fecha y hora tentativa de la reunión.**
2. **Duración de la reunión.** Default sugerido: **60 minutos**. Si el proyecto es grande, considerar 90 min.
3. **Participantes esperados.** Roles concretos (líder técnico, dev backend, dev frontend, QA, diseño, PM). Cruza con la sección "Stakeholders y roles" del `CLAUDE.md` — si el usuario no menciona a alguien clave, sugiéreselo.
4. **Modalidad.** Presencial, remota o híbrida. Afecta cómo se estructura la sesión.
5. **Qué decisiones necesitas que salgan de la reunión.** Ejemplos:
   - Estimado grueso de esfuerzo (T-shirt sizing o sprints)
   - Definición de fases o milestones
   - Quién lidera técnicamente el proyecto
   - Identificación de bloqueos o dependencias no visibles
   - Priorización dentro del alcance si el timeline aprieta
6. **Restricciones conocidas.** Timeline, presupuesto, dependencias externas — cosas que el equipo técnico debe saber desde el inicio.

Si el usuario no tiene claras las decisiones que quiere que salgan, ayúdalo a construirlas con repreguntas.

---

## Fase 2 — Generación del Brief de pre-lectura

El brief se envía 24-48h antes de la reunión. Debe leerse en **máximo 5 minutos**. Estructura:

### Estructura obligatoria del brief

```markdown
# Brief de kickoff — [Nombre del proyecto]

**Reunión:** [fecha, hora, duración]
**Enviado por:** [PM]
**Documentos de referencia:** [prd-nombre.md, historias-nombre.md]

---

## Por qué estamos haciendo esto (2-3 líneas)

Contexto del problema y outcome deseado. Destilar la sección 2 del PRD (Problema) a la esencia. No copiar/pegar.

## Qué vamos a construir (bullets)

Destilar la sección 3 del PRD (Solución y alcance). Solo capacidades clave, sin detalle de implementación.

## Qué NO vamos a construir (bullets)

Lista corta del "out of scope" del PRD. Es tan importante como lo que sí. Protege de scope creep desde el arranque.

## Métricas de éxito

- North Star: [X]
- Guardrails críticos: [Y]

## Riesgos ya identificados

Solo los que el equipo técnico debe conocer antes de la reunión. Los más críticos del PRD (secciones 7 Riesgos) — máximo 3-4.

## Dependencias importantes

Cualquier equipo, proveedor o sistema del que dependa el proyecto y que el equipo técnico debe tener presente.

## Qué necesito de ustedes en la reunión

Bullets concretos con lo que el PM espera que el equipo llegue preparado:
- Preguntas técnicas leídas y con dudas listas
- Idea aproximada de esfuerzo (aunque sea t-shirt sizing)
- Riesgos técnicos que ustedes vean y que yo no
- Preferencias sobre cómo dividir el trabajo (fases, sprints, milestones)

## Referencias para profundizar

- PRD completo: [link o archivo]
- Historias de usuario: [link o archivo]
- Discovery si aplica: [link o archivo]
```

### Reglas para escribir el brief

- **Longitud:** máximo 2 páginas. Si se pasa, hay que recortar, no expandir.
- **No copiar/pegar del PRD.** Destilar y simplificar. Si el brief es igual al PRD, no sirve.
- **Lenguaje del equipo técnico.** Sin excesos de jerga de producto ni corporativo.
- **Priorizar lo accionable.** El equipo debe leer y saber qué hacer.

---

## Fase 3 — Generación de la Agenda de la reunión

La agenda estructura la reunión y sirve como guía del PM durante la sesión. Se comparte con el equipo al inicio de la reunión.

### Estructura obligatoria de la agenda

Distribución de tiempo por defecto (para reunión de 60 min):

```markdown
# Agenda kickoff — [Nombre del proyecto]

**Fecha:** [X] · **Duración:** 60 min · **Modalidad:** [presencial/remota/híbrida]
**PM:** [nombre] · **Participantes:** [lista de roles]

---

## 1. Contexto y objetivos (10 min)

- Repaso rápido del problema y outcome (asumiendo que leyeron el brief).
- Confirmación de que todos están alineados.
- Espacio para preguntas de aclaración sobre el "por qué".

## 2. Alcance y no-alcance (10 min)

- Repaso de capacidades clave.
- Revisión explícita del "fuera de alcance".
- Preguntas del equipo sobre entendimiento del scope.

## 3. Preguntas técnicas y dudas (15 min)

- El equipo trae sus preguntas del brief.
- PM responde o marca "por resolver" con dueño y fecha.
- Objetivo: que ninguna pregunta técnica quede sin ownership al final de la reunión.

## 4. Estimación gruesa y fases (15 min)

- Discusión de esfuerzo aproximado (T-shirt sizing, sprints, u otro).
- Propuesta de fases o milestones.
- Identificación de dependencias no visibles antes.
- Objetivo: salir con un rango de esfuerzo y una estructura tentativa de fases.

## 5. Decisiones pendientes y próximos pasos (10 min)

- Lista de decisiones que quedaron abiertas.
- Asignación de dueños y fechas para resolver cada una.
- Definición del próximo checkpoint (siguiente reunión, sprint planning, etc.).
- Confirmación de canales de comunicación durante la ejecución.

---

## Preguntas que el PM quiere resolver hoy

[Lista concreta de las decisiones que el usuario necesita que salgan de esta reunión — capturadas en Fase 1.]

## Riesgos a poner sobre la mesa

[Lista de riesgos identificados en el PRD que el PM quiere validar o mitigar con el equipo técnico.]
```

### Ajustes de tiempo según duración

Si la reunión es de **90 min**, agregar 15 min a "Estimación y fases" y 15 min a "Preguntas técnicas".

Si la reunión es de **30 min** (kickoff express para proyectos pequeños):
- Contexto (5) · Alcance (5) · Preguntas (10) · Estimación (5) · Próximos pasos (5).
- Advertir al usuario que 30 min es apretado y solo funciona para proyectos muy claros.

### Reglas para la agenda

- **Cada bloque tiene un objetivo, no solo un tema.** "Discusión de esfuerzo" es tema; "Salir con rango de esfuerzo tentativo" es objetivo.
- **Las preguntas del PM van explícitas al final.** No las escondas dentro de los bloques.
- **Los riesgos van explícitos al final.** El PM los saca al momento correcto durante la reunión, pero deben estar listados de antemano.

---

## Reglas transversales del modo

1. **Respeta el `PLAYBOOK.md`.** Tono, no inventar, marcar supuestos.
2. **Aplica el `CLAUDE.md`.** Terminología, estilo de comunicación con equipos internos.
3. **Aplica el `LEARNINGS.md`.** Correcciones acumuladas.
4. **No propongas soluciones técnicas** en el brief ni en la agenda. Ese es el trabajo del equipo técnico en la reunión.
5. **El brief y la agenda son documentos distintos.** El brief se envía antes; la agenda es la guía de la reunión. No los fusiones a menos que el usuario lo pida explícitamente.
6. **Longitudes objetivo:** brief máximo 2 páginas, agenda máximo 1 página. Si se pasan, recortar.

---

## Cierre

Al terminar, guarda dos archivos:

- `pre-kickoff-brief-[nombre-proyecto].md`
- `pre-kickoff-agenda-[nombre-proyecto].md`

Muestra al usuario:

> Listo. Generé dos documentos para el pre-kickoff:
>
> - **`pre-kickoff-brief-[nombre-proyecto].md`** — para enviar al equipo técnico 24-48h antes de la reunión.
> - **`pre-kickoff-agenda-[nombre-proyecto].md`** — para guiar la sesión.
>
> **Próximos pasos sugeridos:**
> 1. Revisa los documentos y ajusta si algo no cuadra con la dinámica de tu equipo.
> 2. Envía el brief con al menos 24h de anticipación por el canal que uses ([Slack/correo] según `CLAUDE.md`).
> 3. Prepárate para las decisiones que capturaste — tenlas listas.
> 4. Después de la reunión, corre `/post-kickoff` para generar la minuta y siguientes pasos.
>
> ¿Quieres ajustar algo antes?
