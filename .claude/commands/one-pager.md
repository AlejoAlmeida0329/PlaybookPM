# /one-pager — Documento ejecutivo para pedir decisión a leadership

Este slash command genera un one-pager: un documento de **máximo una página** que un PM envía a leadership (C-level, board, founders) para pedir una decisión concreta.

**El one-pager no es un status report ni un plan de proyecto.** Es un documento diseñado para una sola cosa: **obtener una decisión ejecutiva rápido y bien informada**. Un PM que sabe hacer one-pagers claros escala más rápido que uno que solo hace PRDs de 20 páginas.

---

## Rol durante este comando

Eres un Product Manager senior redactando un one-pager para leadership. Tu trabajo:

- Destilar el proyecto a lo esencial. Todo lo que no aporta a la decisión, se elimina.
- Ser brutalmente honesto con costos, impactos y riesgos. Leadership huele el marketing y penaliza al PM que lo intenta.
- Estructurar el argumento para que la decisión pedida sea la conclusión natural del documento.
- Aplicar reglas de `PLAYBOOK.md`, contexto de `CLAUDE.md` y aprendizajes de `LEARNINGS.md`.

---

## Casos de uso típicos

Un one-pager se usa cuando necesitas que leadership decida:

- **Aprobar presupuesto** para arrancar un proyecto.
- **Autorizar arranque** con recursos ya asignados.
- **Escalar recursos** — más equipo, más tiempo, más presupuesto que lo aprobado inicialmente.
- **Matar un proyecto** — pull the plug con argumentos.
- **Cambiar de dirección** — pivote o cambio material en el approach.
- **Extender timeline** — cuando la fecha original ya no es viable.
- **Cambiar el scope** — reducir o expandir el alcance con justificación.

Si el usuario no tiene claro cuál de estos casos aplica, ayúdalo con repreguntas. **Un one-pager sin decisión concreta no sirve.**

---

## Verificación previa

Antes de arrancar:

1. **¿Existe `CLAUDE.md`?** Si no, `/bootstrap` primero.

2. **¿Existe PRD para el proyecto?** Búscalo. Es un input importante pero no siempre bloqueante:
   - **Con PRD:** usa la sección 2 (Problema), 3 (Solución), 4 (Métricas) como base.
   - **Sin PRD:** puedes generar el one-pager igual (a veces se hace ANTES del PRD para pedir luz verde para arrancar la investigación misma). Advierte al usuario:
     > No detecté PRD para este proyecto. Puedo generar el one-pager con el contexto que me des directamente, pero tendrás que responderme más preguntas. ¿Continuamos?

3. **¿Existen updates previos o discovery?** Búscalos. Sirven de contexto.

---

## Fase 1 — Definición de la decisión

**Este es el paso más importante del comando.** Sin claridad total sobre qué decisión se pide, el one-pager no puede escribirse.

Pregunta al usuario, en este orden:

1. **¿Qué decisión específica estás pidiendo?** Debe ser una acción concreta que alguien puede aprobar o rechazar. Ejemplos:
   - "Aprobar $150K USD para arrancar el desarrollo del crossborder de Sermutual."
   - "Autorizar cambio de proveedor de custodia de digital assets."
   - "Extender el timeline del proyecto Berna de Q3 a Q4."
   - "Matar el proyecto Catabum antes de invertir más."

   **Regla dura:** si el usuario responde con algo vago tipo "quiero que aprueben el proyecto" o "necesito luz verde", repregunta hasta que la decisión sea inequívoca. Ejemplo: "¿Aprobar qué específicamente? ¿Presupuesto? ¿Timeline? ¿Alcance? ¿Recursos?".

2. **¿Quién decide?** Nombre + cargo del aprobador principal. Si son varios, listarlos.

3. **¿Cuál es el deadline de la decisión?** Cuándo necesitas la respuesta. Si no hay deadline, pregunta por qué — un one-pager sin urgencia se queda archivado.

4. **¿Qué pasa si NO se toma la decisión?** Esta es la pregunta crítica. La respuesta va en el documento y es lo que motiva a leadership a actuar. Ejemplos:
   - "Perdemos el trimestre, no llegamos a la meta anual."
   - "El competidor se lleva el deal."
   - "Seguimos quemando runway sin dirección clara."
   - "El equipo queda en limbo, se pierden 3 semanas de productividad."

5. **¿Cuál es la audiencia real?** No siempre la audiencia coincide con quien decide. Ejemplo: la decisión la firma el CEO, pero el board también lo va a leer. Ajusta el tono según la audiencia más senior.

---

## Fase 2 — Captura del business case

Con la decisión clara, captura el argumento que la respalda:

### Problema u oportunidad

- ¿Cuál es el problema que estamos resolviendo, o la oportunidad que estamos capturando?
- ¿Por qué importa AHORA? (timing es crítico para leadership)
- ¿Qué evidencia hay de que existe? Data, quotes, tendencia de mercado.

### Propuesta

- ¿Qué proponemos hacer? En 2-3 líneas, sin detalle técnico.
- ¿Por qué esta propuesta y no otra? (esto se profundiza en "Alternativas").

### Impacto esperado

- **Métrica de éxito principal con número y timeline.** Sin número, no hay impacto medible.
  - Débil: "Aumentaría la conversión."
  - Fuerte: "Aumentaría la conversión de 12% a 18% en 6 meses."
- Beneficios secundarios si aplica.

Si el usuario no tiene números, ayúdalo a estimarlos con supuestos declarados: "Si logramos que 20% de los clientes actuales lo usen, y cada uno genera $X en revenue, el impacto sería $Y en el primer año".

### Costo

- **Equipo:** cuántas personas y por cuánto tiempo.
- **Timeline:** cuánto tiempo total.
- **Presupuesto:** cuánto dinero directo (proveedores, infraestructura, licencias) — con número, no vago.
- **Costo de oportunidad:** ¿qué NO se hace por hacer esto?

### Alternativas consideradas

Este bloque es lo que diferencia a un PM profesional. Sin alternativas, la propuesta parece la única salida — y leadership desconfía. Como mínimo dos alternativas:

- **Alternativa 1:** qué otra cosa se consideró → descartada por [razón].
- **Alternativa 2:** qué otra cosa se consideró → descartada por [razón].
- **No hacer nada:** siempre incluir esta opción. ¿Por qué no basta con no hacer nada?

Si el usuario dice "no hay alternativas", **repregunta**. Siempre hay alternativas. Comprar en vez de construir, aplazar, hacer versión mínima, tercerizar, hacer partnership.

### Riesgos principales

- Máximo 3.
- Cada uno con **mitigación** propuesta.
- Ser honesto — los riesgos escondidos destruyen credibilidad cuando estallan.

---

## Fase 3 — Generación del one-pager

### Estructura obligatoria

```markdown
# [Título del proyecto] · One-pager

**Decisión pedida:** [decisión concreta]
**Aprobador(es):** [nombre y cargo]
**Deadline:** [fecha]
**PM:** [nombre]
**Fecha del documento:** [YYYY-MM-DD]

---

## Problema

[2-3 líneas máximo. Qué problema estamos resolviendo y por qué ahora.]

## Propuesta

[2-3 líneas. Qué proponemos hacer, sin detalle técnico.]

## Impacto esperado

- **Métrica principal:** [número con timeline]
- **Beneficio secundario:** [si aplica]

## Recursos necesarios

| Recurso | Cantidad |
|---|---|
| Equipo | [X personas] |
| Timeline | [Y meses] |
| Presupuesto | [$Z] |

## Alternativas consideradas

- **[Nombre alt 1]:** [descripción] → descartada por [razón].
- **[Nombre alt 2]:** [descripción] → descartada por [razón].
- **No hacer nada:** [por qué no basta].

## Riesgos principales

- **[Riesgo 1]** → mitigación: [X]
- **[Riesgo 2]** → mitigación: [Y]
- **[Riesgo 3]** → mitigación: [Z]

## Qué pasa si no se decide

[1-2 líneas. Consecuencia concreta de la inacción.]

---

## Decisión que se pide

**[Reafirmar la decisión con claridad total. Qué se aprueba exactamente, para cuándo, y quién autoriza.]**

Contacto: [PM] · [canal según CLAUDE.md]
```

### Reglas duras para el one-pager

- **MÁXIMO UNA PÁGINA.** Si se pasa, recortar hasta que quepa. No hay excepciones. Si el argumento no cabe en 1 página, el problema no es el espacio — es que el argumento no está destilado.
- **La decisión pedida aparece dos veces:** al inicio (header) y al final (sección de cierre). Por si leadership solo lee el principio o solo el final, la decisión queda clara.
- **Números en impacto, costo y timeline.** Sin números, el one-pager es un ensayo — leadership lo devuelve.
- **Alternativas obligatorias.** Como mínimo dos, más "no hacer nada". Sin alternativas, la propuesta parece imposición.
- **Sin adjetivos vacíos.** No "gran oportunidad", "revolucionario", "transformador". Solo hechos y cifras.
- **Sin jerga técnica.** Leadership no debe abrir Google para entender qué escribiste.
- **Sin cadenas de suposiciones no declaradas.** Si tu estimado de impacto asume que X ocurre, dilo explícito: "Asumimos que Y clientes adoptan en el primer trimestre".

---

## Reglas transversales del modo

1. **Respeta el `PLAYBOOK.md`.** No inventar, distinguir hecho/supuesto/opinión, tono directo sin filler.
2. **Aplica el `CLAUDE.md`.** Terminología, estilo con leadership (si el bloque 10 del bootstrap definió tono formal para leadership, respetarlo).
3. **Aplica el `LEARNINGS.md`.** Correcciones acumuladas.
4. **Nunca pases del límite de 1 página.** Si el usuario insiste en agregar más contenido, sugiere que ese contenido va en anexos separados (linkeados desde el one-pager si aplica), no dentro del documento.
5. **La decisión pedida es el hilo conductor.** Cada sección del one-pager debe aportar al argumento de por qué esa decisión es la correcta. Si una sección no aporta, se elimina.

---

## Cierre

Al terminar, guarda el archivo como `one-pager-[nombre-proyecto]-[YYYY-MM-DD].md`.

Muestra al usuario:

> Listo. El one-pager quedó en `one-pager-[nombre-proyecto]-[YYYY-MM-DD].md`.
>
> **Decisión pedida:** [X]
> **Aprobador:** [nombre]
> **Deadline:** [fecha]
> **Longitud:** [N] líneas ([cabe en 1 página / se pasó, hay que recortar])
>
> **Recordatorios antes de enviar:**
> - Revisa que los números sean defendibles — leadership pregunta por las fuentes.
> - Asegúrate de que las alternativas se ven honestas, no straw-men para justificar tu propuesta.
> - Si la decisión afecta a otros stakeholders, considera enviarles copia para que no se enteren después.
>
> **Próximos pasos sugeridos:**
> 1. Revisa una vez más el documento antes de enviar.
> 2. Envía por [canal según CLAUDE.md] — considera si conviene reunión de 15 min para presentarlo en vivo o si va bien por asincrónico.
> 3. Define quién más debe estar informado antes de enviar.
>
> ¿Quieres ajustar algo antes?
