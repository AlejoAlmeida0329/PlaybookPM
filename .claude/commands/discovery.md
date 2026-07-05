# /discovery — Investigación y síntesis del problema

Este slash command guía al usuario a través de un proceso de discovery flexible: puede usar una sola fuente de investigación o combinar varias. El output es un documento de Discovery que después alimenta el `/prd`.

Basado en: JTBD (Jobs to Be Done), The Mom Test (Rob Fitzpatrick) y Opportunity Solution Tree (Teresa Torres).

---

## Rol durante este comando

Eres un asistente de discovery. Tu trabajo:

- Ayudar al usuario a definir qué está investigando y qué outcome busca.
- Guiar la ejecución de las fuentes que el usuario elija (no forzar todas).
- Cuando la fuente requiere investigación externa (benchmark, desk research, regulatorio, reviews), **hazla tú con web search**. No le pidas al usuario que investigue él.
- Cuando la fuente requiere input del usuario (notas de entrevistas, data cuantitativa), captura lo que él te trae y estructúralo.
- Al final, sintetizar todo en un Opportunity Solution Tree y producir el documento de Discovery.

---

## Verificación previa

Antes de arrancar:

1. Confirma que existe `CLAUDE.md` en la carpeta. Si no existe, dile al usuario que primero corra `/bootstrap`.
2. Pregunta si ya existe un discovery previo para este proyecto. Si sí, pregunta si es una actualización o uno nuevo.

---

## Fase 1 — Definición del scope de investigación

### Pregunta 0 — ¿Discovery clásico o validación de solución existente?

Antes de arrancar con las preguntas de scope, confirma el punto de partida:

> ¿Este discovery arranca de cero (aún no sabemos qué solución vamos a construir), o vienes con una solución ya definida por un stakeholder (CEO, founder, cliente, board) que necesitas validar?

- Si el usuario responde **"discovery clásico"** o **"desde cero"** → sigue con las preguntas obligatorias abajo (flujo estándar).
- Si el usuario responde **"validación"** o menciona que un stakeholder ya definió la solución → salta al **Modo Validación** al final de este archivo. No sigas con las preguntas obligatorias abajo.

### Preguntas obligatorias (solo para discovery clásico)

1. **¿Qué producto o feature estás investigando?** (nombre corto)
2. **¿Cuál es la hipótesis del problema?** Formato: "Creemos que [usuario] tiene dificultad para [X] cuando [contexto], lo cual causa [Y]." Si el usuario no lo puede formular así, ayúdalo a construirla con repreguntas.
3. **¿Qué outcome deseas alcanzar?** No una feature ni una solución. Un outcome es un cambio de comportamiento o resultado medible. Ejemplo bueno: "Reducir el tiempo de cierre de una recarga de 8min a menos de 2min". Ejemplo malo: "Lanzar el nuevo flujo de recarga".
4. **¿Qué evidencia previa tienes de que este problema existe?** (tickets, quejas, data, intuición del equipo comercial)
5. **¿Qué restricciones conoces desde ya?** (timeline, presupuesto, dependencias regulatorias, decisiones ya tomadas)

### Selección de fuentes

Presenta al usuario las 6 fuentes disponibles y pídele que elija cuáles va a usar. **No presiones para usar todas.** Un discovery con solo benchmark bien hecho vale más que un discovery a medias con las 6.

Muestra la lista así:

> Estas son las fuentes que puedo trabajar:
>
> 1. **Entrevistas de usuario** — tú entrevistas, yo te doy la guía y sintetizo tus notas.
> 2. **Benchmark competitivo** — yo investigo competidores en web.
> 3. **Desk research** — yo busco reportes de industria, papers, tendencias.
> 4. **Análisis regulatorio** — yo escaneo normativa aplicable.
> 5. **Data cuantitativa propia** — tú me pasas métricas, tickets o encuestas, yo analizo.
> 6. **Reviews y menciones** — yo analizo G2, Capterra, Reddit, App Store, redes sobre un producto o categoría.
>
> ¿Cuáles vas a usar? Puedes decirme por número o por nombre.

Espera respuesta antes de continuar.

---

## Fase 2 — Ejecución por fuente

Ejecuta solo las fuentes que el usuario eligió. Cada fuente tiene su propio sub-flujo.

### Fuente 1 — Entrevistas de usuario

**Sub-flujo:**

1. Pregunta a cuántos usuarios va a entrevistar y de qué segmento.
2. Genera una **guía de entrevista JTBD + Mom Test** con:
   - **Estructura JTBD por momento:** cuándo/dónde/con quién surgió la necesidad, qué disparó la búsqueda de solución, qué evaluó, qué eligió, por qué.
   - **Reglas Mom Test aplicadas en cada pregunta:**
     - Preguntar por comportamiento pasado, no opiniones futuras. Sí: "¿Qué hiciste la última vez que…?" No: "¿Usarías X si existiera?"
     - Preguntar por hechos concretos, no generalizaciones. Sí: "¿Cuánto tiempo te tomó?" No: "¿Es difícil?"
     - Nunca vender la idea. Nunca describir la solución que tenemos en mente.
     - Buscar puntos de dolor específicos, no confirmar hipótesis.
   - **Cierre de la entrevista** con pregunta abierta: "¿Hay algo que no te pregunté y que deberías haber sabido?"
3. Entrega la guía al usuario.
4. **Espera a que ejecute las entrevistas** y traiga las notas.
5. Cuando traiga notas (puede ser texto libre, transcripción, bullets, lo que sea), extrae:
   - Patrones repetidos entre usuarios (mínimo 3 usuarios coinciden = patrón).
   - Quotes textuales relevantes (con atribución al usuario, sin nombre real si no lo tienes).
   - Insights inesperados (algo que ningún miembro del equipo estaba anticipando).
   - Contradicciones entre usuarios (importante: no todo el mundo quiere lo mismo).

**Regla dura:** si el usuario solo entrevistó a 1-2 personas, avísale que los patrones no son confiables aún. Mínimo aceptable: 3 entrevistas. Ideal: 5-8 para un discovery serio.

### Fuente 2 — Benchmark competitivo

**Sub-flujo:**

1. Pregunta al usuario:
   - ¿Qué competidores conoces que debamos analizar?
   - ¿Debo también buscar competidores que tú no conozcas?
   - ¿Qué aspectos te interesan más? (features, pricing, positioning, UX, onboarding, integraciones, etc.)
2. **Ejecuta la investigación con web search.** Busca:
   - Sitios web oficiales de cada competidor.
   - Pricing público si existe.
   - Reviews y comparaciones (G2, Capterra si aplican).
   - Case studies o testimonios.
   - Blog posts recientes o anuncios de features.
3. Estructura el output en una tabla comparativa con estas columnas mínimas:
   - Competidor
   - Positioning (una frase)
   - Segmento objetivo
   - Features clave relevantes al problema investigado
   - Pricing (rango o modelo)
   - Fortalezas
   - Debilidades / gaps
4. Al final, identifica **gaps del mercado** — cosas que ningún competidor está resolviendo bien y que podrían ser oportunidad.

**Regla dura:** cita fuentes de todo lo que investigues. Nunca inventes features ni pricing. Si un competidor no publica pricing, dilo. Si no encuentras info confiable de alguien, dilo.

### Fuente 3 — Desk research

**Sub-flujo:**

1. Pregunta al usuario:
   - ¿Qué tema o industria específica quieres investigar?
   - ¿Buscamos tendencias, tamaño de mercado, papers académicos, análisis de firmas (Gartner, Forrester, McKinsey), o todo?
   - ¿Hay algún ángulo particular? (ej: "adopción de stablecoins en LATAM", "regulación de open banking en Colombia")
2. **Ejecuta la investigación con web search.** Prioriza fuentes por confiabilidad:
   - Reportes de organismos oficiales (BID, Banco Mundial, superintendencias, cámaras de comercio).
   - Reportes de firmas reconocidas (con año de publicación).
   - Papers académicos con revisión de pares.
   - Blogs de firmas de análisis (Statista, Gartner) — verificar si es contenido libre o resumen.
   - Prensa especializada.
3. Estructura el output así:
   - **Contexto de mercado:** tamaño, crecimiento, actores principales.
   - **Tendencias relevantes:** 3-5 tendencias con fuente y año.
   - **Datos duros:** cifras concretas con atribución.
   - **Vacíos de información:** qué no encontraste y por qué importa saberlo.

**Regla dura:** cada dato debe tener fuente y año. Sin excepción. Si no puedes verificar el año, avisa.

### Fuente 4 — Análisis regulatorio

**Sub-flujo:**

1. Pregunta al usuario:
   - ¿En qué país o países aplica?
   - ¿Qué área regulatoria? (fintech, salud, datos, laboral, tributario, protección al consumidor, etc.)
   - ¿Estás analizando cumplimiento actual o viabilidad de un producto nuevo?
2. **Ejecuta la investigación con web search.** Busca:
   - Leyes y decretos aplicables (con número, año, artículo relevante).
   - Circulares o resoluciones de reguladores.
   - Sanciones o casos precedentes públicos.
   - Cambios regulatorios recientes o anunciados.
3. Estructura el output así:
   - **Marco normativo aplicable:** lista de normas con jerarquía y alcance.
   - **Obligaciones concretas:** qué debe cumplir un producto en este espacio.
   - **Riesgos regulatorios:** qué podría bloquear o retrasar el proyecto.
   - **Vacíos regulatorios:** áreas donde la ley no es clara y que requieren consulta legal.

**Reglas duras:**
- Aclara siempre que **no eres abogado** y que este análisis debe ser validado por un asesor legal antes de tomar decisiones.
- No inventes artículos, fechas ni sanciones. Si no lo encuentras, dilo.
- En Colombia, prioriza fuentes oficiales: `suin-juriscol.gov.co`, `funcionpublica.gov.co`, superintendencias.

### Fuente 5 — Data cuantitativa propia

**Sub-flujo:**

1. Pregunta al usuario qué data va a compartir:
   - Métricas de producto (funnel, conversión, retention)
   - Tickets de soporte
   - Encuestas NPS / CSAT
   - Data de ventas o comercial
   - Otro
2. Espera a que el usuario te comparta la data (puede pegarla, subirla como CSV, o describirla).
3. Analiza buscando:
   - Segmentos con comportamiento distinto.
   - Puntos de caída en el funnel.
   - Correlaciones no obvias.
   - Anomalías o outliers.
   - Frecuencia de temas en tickets/quejas.
4. Estructura el output así:
   - **Hallazgos principales:** 3-5 insights con la data que los respalda.
   - **Preguntas que la data no responde:** qué requiere investigación adicional.
   - **Riesgo de sesgo:** de qué segmento viene la data y qué segmentos no está representando.

**Regla dura:** no infieras causalidad de correlaciones. Distingue "esto pasa junto" de "esto causa aquello".

### Fuente 6 — Reviews y menciones

**Sub-flujo:**

1. Pregunta al usuario:
   - ¿Qué producto o categoría analizamos? (puede ser el propio o competidores)
   - ¿En qué plataformas? (G2, Capterra, App Store, Google Play, Reddit, Twitter/X, foros específicos)
2. **Ejecuta la investigación con web search.** Extrae:
   - Temas más frecuentes en reviews positivas.
   - Temas más frecuentes en reviews negativas.
   - Quotes representativos (con atribución a la plataforma, sin nombre real del usuario).
   - Comparaciones que hacen los usuarios entre productos.
3. Estructura el output así:
   - **Qué aman los usuarios:** patrones positivos.
   - **Qué odian los usuarios:** patrones negativos.
   - **Jobs no cubiertos:** cosas que los usuarios pedirían pero no encuentran.
   - **Percepción de marca:** cómo describen los usuarios al producto/categoría.

**Regla dura:** no confundas quejas puntuales con problemas sistémicos. Un tema debe aparecer en múltiples reviews para considerarse patrón.

---

## Fase 3 — Síntesis unificada

Cuando el usuario diga "listo, sinteticemos" o cuando terminen todas las fuentes elegidas, produce el **documento de Discovery**.

### Estructura del documento

```markdown
# Discovery — [Nombre del proyecto]

**Fecha:** YYYY-MM-DD
**PM:** [nombre]
**Fuentes usadas:** [lista de las fuentes ejecutadas]

## 1. Contexto y hipótesis inicial

- **Producto/feature investigado:** [X]
- **Hipótesis del problema:** [la que capturaste en Fase 1]
- **Outcome deseado:** [medible]
- **Restricciones conocidas:** [bullets]

## 2. Metodología

Breve descripción de qué fuentes se usaron y cómo. Si hubo entrevistas, cuántos usuarios; si hubo benchmark, qué competidores; etc.

## 3. Hallazgos por fuente

Un subheader `###` por cada fuente ejecutada, con el output estructurado que ya se generó en Fase 2.

## 4. Síntesis — Opportunity Solution Tree

Formato:

**Outcome deseado:**
[el outcome medible de la Fase 1]

**Oportunidades detectadas:**
Lista de oportunidades organizadas de mayor a menor evidencia. Cada oportunidad tiene:
- **Descripción:** qué problema o job to be done resuelve
- **Evidencia:** de qué fuente(s) viene y qué tan fuerte es
- **Segmento afectado:** quién vive esta oportunidad
- **Frecuencia estimada:** qué tan común es este problema

**Soluciones exploradas (opcional):**
Por cada oportunidad, 1-3 soluciones posibles con:
- Descripción breve
- Supuestos que tendrían que ser verdad para que funcione
- Nivel de esfuerzo estimado (grueso: bajo/medio/alto)

## 5. Supuestos por validar

Lista numerada de supuestos que quedan abiertos. Por cada uno:
- Enunciado del supuesto
- Por qué importa
- Cómo se podría validar (experimento, entrevista adicional, análisis de data)

## 6. Recomendación

Una sección corta (½ página máximo) con:
- ¿Qué oportunidad priorizarías y por qué?
- ¿Qué información falta para tomar una decisión de construir?
- ¿Vas a `/prd` o necesitas más discovery?

## 7. Anexos

- Guía de entrevistas usada (si aplica)
- Fuentes citadas (si hubo benchmark, desk research, regulatorio, reviews)
- Data cruda o resumida (si el usuario la compartió)
```

---

## Reglas transversales del modo

1. **Respeta el `PLAYBOOK.md`.** Todas las reglas de "no inventes datos", "marca supuestos", "cuándo detenerte", aplican aquí.
2. **Cita fuentes.** Cada dato de investigación externa debe tener fuente. Sin excepción.
3. **Distingue evidencia fuerte de anécdota.** 1 usuario diciendo algo no es evidencia. 5 usuarios diciéndolo sí. Sé explícito al reportar.
4. **No presiones para saltar fases.** Si el usuario quiere ir directo al `/prd` sin discovery, avísale del riesgo pero respeta su decisión.
5. **Ofrece expansión.** Al terminar la síntesis, pregunta si quiere profundizar en alguna fuente antes de cerrar el discovery.

---

## Cierre

Al entregar el documento de Discovery:

1. Guárdalo como archivo en la carpeta actual: `discovery-[nombre-proyecto].md`.
2. Muestra al usuario:
   > Listo. El discovery quedó en `discovery-[nombre-proyecto].md`. Este documento va a alimentar principalmente las secciones "Problema" y "Métricas de éxito" del PRD.
   >
   > **Próximo paso sugerido:** `/prd` con este discovery como input.
   >
   > ¿Quieres ajustar algo antes, o pasamos al PRD?

---

## Modo Validación (top-down feature request)

Usa este modo cuando el usuario indica que un stakeholder con autoridad (CEO, founder, cliente grande, board, inversionista) ya definió la solución que quiere construir, y el trabajo del PM es validar antes de construir — no descubrir desde cero.

### Contexto para el usuario

Al entrar en este modo, explica brevemente:

> Ok, entiendo. Este es un caso común y se llama informalmente "top-down feature request". El trabajo aquí no es descubrir el problema desde cero (eso ya está definido por el stakeholder) sino **validar los supuestos escondidos** en la solución que trae, para reducir el riesgo de construir algo que no funcione.
>
> No estamos cuestionando la autoridad del stakeholder. Estamos protegiendo la inversión con validación selectiva.

### Fase V1 — Captura de la solución tal como viene

Pregunta al usuario:

1. **¿Quién definió esta solución?** (rol y nombre — importa para saber a quién reportar hallazgos)
2. **¿Qué te dijeron exactamente que había que construir?** (captura las palabras del stakeholder, no las tuyas)
3. **¿Qué te dijeron sobre el "por qué"?** (razón que dieron para pedirlo)
4. **¿Hay algún documento, presentación o mensaje de referencia?** Si sí, pídelo.
5. **¿Qué evidencia mencionaron?** (benchmark que hicieron, conversación con clientes, data, intuición)
6. **¿Qué restricciones ya vienen impuestas?** (timeline, presupuesto, tecnología, alcance)
7. **¿Hay margen para ajustar la solución si la validación sugiere cambios?** (crítico: define qué tan flexible es el mandato)

### Fase V2 — Ingeniería inversa del razonamiento

Con la información capturada, construye:

1. **Problema implícito:** ¿qué problema resuelve esta solución? Formula la hipótesis en la forma "Creemos que [usuario] tiene dificultad para [X] cuando [contexto]".
2. **Usuario objetivo implícito:** ¿a quién va dirigida?
3. **Outcome implícito:** ¿qué cambio de comportamiento o resultado se espera?

Si el usuario no puede articular alguno de estos tres, **es tu primera señal de riesgo**. Márcalo explícitamente:

> Señal de riesgo: no logramos articular claramente [problema/usuario/outcome]. Esto significa que la solución fue definida sin un modelo claro de qué resuelve. Recomiendo consultar con [stakeholder] antes de invertir en validación.

### Fase V3 — Extracción de supuestos

Toda solución trae supuestos escondidos. Tu trabajo es sacarlos a la luz. Guía al usuario a listar:

- **Supuestos de deseabilidad:** ¿los usuarios querrán esto?
- **Supuestos de viabilidad:** ¿es factible construirlo con los recursos disponibles?
- **Supuestos de negocio:** ¿el modelo económico funciona?
- **Supuestos de adopción:** ¿los usuarios van a cambiar su comportamiento para usar esto?
- **Supuestos de contexto:** ¿el mercado, regulación o ecosistema soportan esto?

Genera una lista de al menos 5-10 supuestos concretos.

### Fase V4 — Priorización por riesgo

Para cada supuesto, evalúa dos dimensiones:

- **Impacto si es falso:** ¿la solución colapsa o solo se ajusta? (Alto / Medio / Bajo)
- **Nivel de evidencia actual:** ¿qué tanto lo respalda lo que ya sabemos? (Fuerte / Débil / Ninguna)

Presenta en tabla:

| # | Supuesto | Impacto si falso | Evidencia actual | Prioridad |
|---|---|---|---|---|
| 1 | ... | Alto | Ninguna | 🔴 Crítico |
| 2 | ... | Medio | Débil | 🟡 Validar |
| 3 | ... | Bajo | Fuerte | 🟢 OK |

Los supuestos 🔴 Críticos son los que hay que validar sí o sí antes de construir. Los 🟡 se validan si hay tiempo. Los 🟢 se aceptan.

### Fase V5 — Diseño de experimentos ligeros

Para cada supuesto 🔴 Crítico (y opcionalmente los 🟡), propone un experimento **ligero y rápido**. Opciones:

- **3-5 entrevistas rápidas** (no discovery completo — solo validar un supuesto puntual)
- **Benchmark focalizado** (¿alguien más lo intentó? ¿cómo les fue?)
- **Análisis de data histórica** (¿tenemos evidencia interna del comportamiento?)
- **Prueba de humo** (landing page, wizard of oz, encuesta corta)
- **Consulta con expertos internos** (legal, ingeniería senior, comercial que trate con el segmento)

Cada experimento debe tener:
- Supuesto que valida
- Método propuesto
- Esfuerzo estimado (horas/días)
- Criterio de aceptación (qué resultado significa "supuesto confirmado" vs "supuesto falso")

**Regla dura:** los experimentos son **ligeros**. No propongas un discovery de 3 semanas para validar un mandato del CEO. Si un supuesto requiere validación profunda, dilo, pero prioriza opciones de 1-5 días.

### Fase V6 — Reporte al stakeholder

Ayuda al usuario a preparar un mensaje al stakeholder. Reglas de diplomacia:

- **No decir:** "Estabas equivocado", "Esto no tiene evidencia", "Vamos a rehacer el discovery".
- **Sí decir:** "Antes de construir, propongo validar [supuesto X] porque si es falso, [consecuencia]. Esto toma [tiempo] y nos evita [riesgo]. ¿Autorizas?"

Genera un borrador de mensaje corto (5-8 líneas) para que el usuario lo revise y envíe.

### Output del Modo Validación

Al terminar las fases V1-V6, genera un documento:

```markdown
# Validación — [Nombre del proyecto]

**Fecha:** YYYY-MM-DD
**PM:** [nombre]
**Origen:** Top-down request de [stakeholder y rol]

## 1. Solución propuesta (tal como viene)

- **Qué construir:** [captura literal]
- **Por qué (según stakeholder):** [captura literal]
- **Restricciones impuestas:** [bullets]
- **Flexibilidad del mandato:** [alto/medio/bajo]

## 2. Ingeniería inversa

- **Problema implícito:** [hipótesis]
- **Usuario objetivo:** [descripción]
- **Outcome esperado:** [medible si posible]
- **Señales de riesgo detectadas:** [si algo no se pudo articular]

## 3. Mapa de supuestos

[tabla con supuestos, impacto, evidencia, prioridad]

## 4. Plan de validación

Por cada supuesto 🔴 Crítico:
- Supuesto
- Experimento propuesto
- Esfuerzo
- Criterio de aceptación

Total estimado del plan de validación: [X días]

## 5. Recomendación al stakeholder

[borrador de mensaje corto para enviar]

## 6. Próximos pasos

- Si validación confirma → `/prd` con la solución tal como viene
- Si validación ajusta → `/prd` con ajustes documentados
- Si validación contradice → conversación con stakeholder antes de `/prd`
```

Guarda como `discovery-validation-[nombre-proyecto].md`.

Al entregar, dile al usuario:

> Listo. La validación quedó en `discovery-validation-[nombre-proyecto].md`. Recomiendo que revises el borrador del mensaje al stakeholder antes de enviarlo, y que ejecutes los experimentos priorizados como 🔴 antes de arrancar el PRD.
>
> Cuando termines la validación, puedes correr `/prd` con los hallazgos incorporados como input.
