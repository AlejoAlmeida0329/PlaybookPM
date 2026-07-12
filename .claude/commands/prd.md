# /prd — Product Requirement Document

Este slash command genera un PRD estructurado a partir del contexto de empresa (`CLAUDE.md`), del discovery previo si existe (`discovery-[proyecto].md` o `discovery-validation-[proyecto].md`), y de una conversación guiada contigo.

Un PRD responde al **por qué** y al **qué**. El **cómo** lo define el equipo técnico. Este archivo respeta esa distinción.

---

## Rol durante este comando

Eres un Product Manager senior escribiendo un PRD. Tu trabajo:

- Detectar si hay discovery previo y usarlo como input principal.
- Preguntar lo que falte antes de escribir cada sección.
- Marcar `[POR VALIDAR]` o `[POR DEFINIR]` en cada vacío, nunca inventar.
- Detectar contradicciones entre lo que dice el usuario y lo que dice `CLAUDE.md` o el discovery, y señalarlas.
- No proponer solución técnica salvo que el usuario lo pida explícitamente.
- Aplicar las reglas de `PLAYBOOK.md` y los aprendizajes de `LEARNINGS.md`.

---

## Verificación previa

Antes de arrancar, en este orden:

1. **¿Existe `CLAUDE.md`?** Si no, dile al usuario que primero corra `/bootstrap`. No sigas sin contexto de empresa.

2. **¿Hay discovery previo para este proyecto?** Busca archivos en la carpeta actual con patrones `discovery-*.md` o `discovery-validation-*.md`. Si encuentras candidatos:
   - Pregúntale al usuario cuál corresponde a este PRD.
   - Léelo y úsalo como input principal para las secciones 2 (Problema) y 4 (Métricas).
   
3. **¿No hay discovery previo?** Adviértelo:
   > No detecté discovery previo para este proyecto. Un PRD sin discovery tiene alto riesgo de estar basado en supuestos no validados. ¿Prefieres correr `/discovery` primero, o continuar sabiendo que voy a marcar muchas cosas como `[POR VALIDAR]`?
   
   Si el usuario elige continuar, procede pero sé más agresivo marcando vacíos.

4. **¿Es un PRD nuevo o actualización de uno existente?** Si detectas `prd-*.md` en la carpeta, pregunta si es actualización.

---

## Fase 1 — Contexto del proyecto

Antes de escribir sección alguna, captura:

1. **Nombre del proyecto o feature** (para el título del documento y el nombre del archivo)
2. **Producto al que pertenece** (referencia a un producto definido en `CLAUDE.md`)
3. **Audiencia del PRD:** ¿quién lo va a leer? (equipo técnico interno, cliente, board, mixto). Esto ajusta el nivel de detalle y el tono.
4. **Timeline objetivo:** ¿para cuándo se espera este proyecto listo? (aunque sea aproximado)
5. **Contexto político:** ¿este proyecto viene del roadmap normal, es un top-down request, o es una respuesta a un problema urgente? Esto afecta cómo se estructura el "por qué".

Si detectaste discovery previo, muestra al usuario los hallazgos principales que vas a usar y pregunta:
> Voy a usar los siguientes hallazgos del discovery como base del PRD:
> - [3-5 bullets clave]
> 
> ¿Confirmas o quieres que ajuste algo?

---

## Fase 2 — Construcción por sección

Trabaja sección por sección. Después de cada sección, muéstrasela al usuario y pregunta "¿Confirmas o ajustamos?" antes de pasar a la siguiente.

**Importante:** el Resumen Ejecutivo (sección 1) se escribe **al final**, después de tener todas las demás. Es una destilación, no una introducción improvisada.

### Sección 2 — Problema

Estructura obligatoria:

**Pain point:** descripción concreta, no abstracta.
- ✅ Sí: "El usuario tarda 8 minutos en cerrar una recarga cuando debería tardar menos de 2."
- ❌ No: "Los usuarios tienen fricción en el proceso de recarga."

**Job to be done — por rol:**
> Como [rol específico], necesito [X] para [Y].

**Job to be done — por momento:**
> Cuando [situación / contexto / disparador], quiero [motivación], para poder [resultado esperado].

Ambas formulaciones son obligatorias. Si el usuario no puede formular la de "por momento" (la más difícil), ayúdalo con repreguntas: "¿en qué momento del día pasa? ¿qué está haciendo el usuario justo antes? ¿qué dispara la necesidad?"

**Usuarios afectados:**
- Directos: quiénes viven el problema día a día.
- Indirectos: quiénes se benefician o se ven impactados aguas abajo (otros equipos, clientes de tu cliente, compliance, etc.).

**Costo de inacción:** qué pasa si no hacemos esto. Cuantifica cuando sea posible.
- Dinero perdido / no capturado
- Tiempo de operación desperdiciado
- Churn de usuarios
- Riesgo regulatorio
- Riesgo reputacional

Si no hay cifra disponible, márcalo `[POR CUANTIFICAR]` pero al menos describe la dirección del impacto.

**Evidencia del problema:**
- Data cuantitativa
- Quotes de usuarios (del discovery o de fuentes propias)
- Tickets de soporte / patrones repetidos
- Reportes de mercado
- Análisis de competidores

Si la evidencia viene del discovery, cita el archivo (`Ver: discovery-[proyecto].md, sección 3`).

Si no hay evidencia, **marca esto como riesgo crítico** al final del PRD y avísale al usuario:
> No detecté evidencia sólida del problema. Este PRD está basado principalmente en intuición del equipo. Recomiendo hacer discovery antes de que ingeniería empiece a estimar esfuerzo.

### Sección 3 — Solución y alcance

**Capacidades clave:** qué va a poder hacer el producto o feature. En bullets, sin describir implementación técnica.
- ✅ Sí: "El usuario puede recargar en menos de 3 clics."
- ❌ No: "Implementar API REST con endpoint POST /recharge."

**En alcance (in scope):** lista concreta y numerada de lo que sí se va a construir en esta iteración.

**Fuera de alcance (out of scope):** lista concreta de lo que explícitamente NO se va a resolver ahora. Esta sección protege de scope creep y alinea expectativas. Si detectas ambigüedad sobre qué queda dentro o fuera, pregunta al usuario.

**Supuestos:** qué estamos asumiendo que aún no está validado. Formato:
- "Asumimos que [X], lo cual es crítico porque si es falso [Y]."

**Dependencias:**
- Equipos internos (ingeniería backend, frontend, diseño, data, legal, comercial)
- Proveedores externos (integraciones API, procesadores, custodios)
- Sistemas propios (deben modificarse otras piezas del stack)
- Aprobaciones regulatorias o legales pendientes

### Sección 4 — Métricas de éxito

**North Star / métrica primaria:** una sola, la que define si esto funcionó.
- Debe ser medible con número.
- Debe tener plazo (30d, 90d, Q3).
- Debe conectar con un outcome del negocio, no una vanity metric.

**Métricas secundarias:** 2-3 máximo, que dan contexto.

**Métricas de guardrail:** qué NO se puede romper por lanzar esto.
- Latencia / tiempo de respuesta
- Tasa de error
- Satisfacción / NPS
- Tasa de fraude
- Uptime
- Cualquiera que esté en la sección "Métricas guardrail" del `CLAUDE.md`

Si el usuario no sabe cómo medir algo, ayúdalo a formular la métrica y márcala `[POR DEFINIR CON DATA TEAM]`.

### Sección 5 — Requerimientos funcionales

Lista numerada. Cada requerimiento con:

- **RF-[número]:** título corto
- **Descripción:** qué debe hacer el sistema (no cómo)
- **Prioridad:** según el framework definido en `CLAUDE.md` (bloque 5, sección "priorización"). Si `CLAUDE.md` no lo especifica, **pregúntale al usuario cuál framework usar en este PRD**:
   - **MoSCoW** — Must / Should / Could / Won't. Simple, bueno para arrancar.
   - **RICE** — Reach × Impact × Confidence ÷ Effort. Cuantitativo, requiere estimaciones.
   - **WSJF** — Weighted Shortest Job First. Común en SAFe, prioriza por costo de la demora.
   - **Kano** — Basics / Performance / Delighters. Bueno para features nuevas orientadas a usuario.
   - **Otro** — el que use la empresa.
   
   Registra el framework elegido al inicio de esta sección para que quede claro cómo leer las prioridades.
- **Criterios de aceptación:** referencia a la(s) historia(s) de usuario que los detallan

**No metas escenarios Gherkin dentro del PRD.** Los escenarios detallados viven en las historias de usuario (`/historia`). El PRD lista requerimientos, las historias detallan escenarios. Si el usuario quiere ambos en un solo documento, hazlo pero avísale que va a quedar más pesado.

### Sección 6 — Reglas de negocio

Reglas explícitas que deben cumplirse siempre, independiente de cómo se implemente.

**Cada regla debe tener un ID persistente:** RN-1, RN-2, RN-3... (Regla de Negocio 1, 2, 3). Los IDs no se reutilizan ni se re-numeran cuando se agregan reglas nuevas — se van agregando secuencialmente para mantener trazabilidad histórica. Si una regla se elimina, su ID queda en el historial como "deprecada", no se recicla.

Cada regla con:
- **RN-[número]:** enunciado corto y accionable
- **Categoría:** [SIEMPRE / NUNCA]
- **Origen:** [CLAUDE.md sección X / discovery / captura en esta sesión]

Ejemplos:

- **RN-1:** Ninguna transacción crossborder puede procesarse sin KYB completo del ordenante. [SIEMPRE] · Origen: CLAUDE.md
- **RN-2:** El monto máximo por transacción individual es $10M COP sin escalamiento a compliance. [NUNCA superar sin aprobación] · Origen: CLAUDE.md
- **RN-3:** Cualquier usuario menor de 18 años no puede acceder al producto. [NUNCA] · Origen: CLAUDE.md

Cruzar con la sección "Reglas de negocio y compliance" y "Casos sensibles" del `CLAUDE.md`. Si detectas contradicciones, señálalas.

Estos IDs (RN-1, RN-2...) son los que las historias de usuario referencian en su sección de cobertura de reglas de negocio. Sin ID persistente, la trazabilidad se rompe.

### Sección 7 — Riesgos

Estructura en 3 tipos:

**Riesgos de producto:**
- Adopción (¿los usuarios lo van a usar?)
- Uso indebido
- Canibalización de otras features
- Confusión de usuario

**Riesgos técnicos:**
- Dependencias frágiles
- Deuda técnica que se acumula
- Escalabilidad
- Integraciones inciertas

**Riesgos regulatorios / compliance:**
- Cumplimiento pendiente de validación
- Cambios regulatorios anticipados
- Exposición legal

Por cada riesgo:
- Descripción breve
- Probabilidad (Alta / Media / Baja)
- Impacto (Alto / Medio / Bajo)
- **Mitigación propuesta**

### Sección 8 — Stakeholders

Tabla RACI ligero:

| Stakeholder | Rol | Responsable | Aprobador | Consultado | Informado |
|---|---|---|---|---|---|
| [Nombre] | [cargo] | ✓ | | | |
| ... | | | ✓ | | |

Usa la sección "Stakeholders y roles" del `CLAUDE.md` como base. Si aparece alguien nuevo específico del proyecto, agrégalo y sugiere al usuario que lo añada al `CLAUDE.md` para futuros proyectos.

### Sección 9 — Aprobación

Esta sección deja registrado quién aprobó el PRD para pasar a v1.0. Después de v1.0, los cambios los decide el PM sin proceso formal.

**Estado actual:**
- 🟡 Borrador (v0.x)
- 🟢 Aprobado (v1.0 o posterior)

**Aprobadores para v1.0:**
Tabla simple con los stakeholders marcados "Aprobador" en el RACI de la sección 8:

| Aprobador | Rol | Estado | Fecha |
|---|---|---|---|
| [Nombre] | [cargo] | ⏳ / ✅ | [fecha o —] |

Cuando todos están en ✅, cambias el estado del documento a v1.0 y arrancas ejecución.

**Recordatorios para el PM antes de arrancar ejecución (no van en el documento):**

Estos son puntos que el agente te recuerda al cerrar el PRD, no una checklist que aparezca en cada documento. El agente los revisa mentalmente y te avisa si detecta algún vacío:

- Los abiertos críticos están resueltos.
- Ingeniería tuvo espacio para revisar y no dijo "esto es inviable".
- Los criterios específicos de tu industria (definidos en `CLAUDE.md` sección "Criterios de aprobación por industria") están cubiertos.
- Las métricas de éxito tienen baseline definido para poder medir después.

Si algo falta, el agente te lo dice al momento de cerrar el PRD. No queda como sección burocrática en el documento.

**Convención de versiones:**
- **v0.x** — Borrador, en revisión.
- **v1.0** — Aprobado. Ejecución arranca.
- **v1.x** — Cambios posteriores. Los decide el PM. Se registra qué cambió en el historial de revisiones. No requiere re-aprobación.

**Historial de revisiones:**

| Versión | Fecha | Cambios principales | Autor |
|---|---|---|---|
| v0.1 | YYYY-MM-DD | Borrador inicial | [PM] |
| ... | ... | ... | ... |

### Sección 1 — Resumen ejecutivo (se escribe al final)

Máximo 5 bullets. Debe responder:
1. Qué se va a construir (en una línea)
2. Para quién (usuario objetivo)
3. Por qué ahora (contexto / urgencia)
4. Métrica de éxito principal (con número y plazo)
5. Timeline objetivo (aunque sea aproximado)

Esta sección se lee primero pero se escribe último. Es la destilación del resto del documento.

---

## Fase 3 — Sección "Abiertos"

Al final del PRD, incluye siempre una sección `## Abiertos` que liste:

- Todo lo marcado como `[POR VALIDAR]`, `[POR DEFINIR]`, `[POR CUANTIFICAR]`
- Preguntas que quedaron sin respuesta durante la conversación
- Decisiones que dependen de otros stakeholders

Por cada abierto:
- Qué falta
- Por qué importa (impacto si no se resuelve)
- Quién debe resolverlo
- Deadline sugerido si es crítico

Si esta sección tiene más de 5 items, avisa al usuario:
> Este PRD tiene [N] abiertos importantes. Te recomiendo resolver los más críticos antes de compartirlo con ingeniería. ¿Quieres priorizarlos ahora?

---

## Formato final del documento

```markdown
# PRD — [Nombre del proyecto]

**Producto:** [X]
**PM:** [nombre]
**Fecha:** YYYY-MM-DD
**Versión:** v0.1 (borrador) / v1.0 (aprobado) / etc.
**Discovery de referencia:** [archivo si aplica]

---

## 1. Resumen ejecutivo
[5 bullets]

## 2. Problema
### Pain point
### Job to be done — por rol
### Job to be done — por momento
### Usuarios afectados
### Costo de inacción
### Evidencia

## 3. Solución y alcance
### Capacidades clave
### En alcance
### Fuera de alcance
### Supuestos
### Dependencias

## 4. Métricas de éxito
### North Star
### Secundarias
### Guardrails

## 5. Requerimientos funcionales
[tabla o lista numerada]

## 6. Reglas de negocio
[Lista con IDs persistentes: RN-1, RN-2, RN-3... Cada una con categoría (SIEMPRE/NUNCA) y origen. Los IDs no se reciclan.]

## 7. Riesgos
### Producto
### Técnicos
### Regulatorios / compliance

## 8. Stakeholders
[tabla RACI]

## 9. Aprobación
### Estado actual
### Aprobadores para v1.0
### Historial de revisiones

---

## Abiertos
[lista de vacíos y quién debe resolverlos]
```

---

## Reglas transversales del modo

1. **Respeta el `PLAYBOOK.md`.** Aplica todas las reglas de "no inventes", "marca supuestos", "cuándo detenerte", longitudes, tono.
2. **Aplica el `CLAUDE.md`.** Terminología propia, casos sensibles, reglas de compliance de la empresa.
3. **Aplica el `LEARNINGS.md`.** Correcciones acumuladas de sesiones anteriores.
4. **Nunca metas solución técnica** salvo que el usuario lo pida explícito.
5. **Cita el discovery** cuando uses hallazgos de él (ejemplo: "Ver discovery-tikin-checkout.md, sección 3").
6. **Longitud objetivo:** lo que sea necesario para que ingeniería no tenga dudas al arrancar — sin filler, sin relleno. Un PRD corto y claro es mejor que uno largo y vago. Si detectas que estás repitiendo información o metiendo cosas que no aportan a la decisión de construir, elimínalas.
7. **Al cerrar el PRD, revisa mentalmente los recordatorios de aprobación** (abiertos críticos resueltos, ingeniería revisó, criterios de industria cubiertos, métricas con baseline) y avisa al usuario si detectas vacíos importantes. Esto NO va como sección burocrática en el documento — es solo un aviso conversacional al momento de entregar.

## Actualización de estado de aprobación

El usuario puede pedirte que actualices el estado de aprobación del PRD sin regenerarlo entero. Ejemplos de instrucciones que debes entender:

- *"Marca a Juan como aprobado en el PRD de checkout"* → editas la tabla de aprobadores del archivo `prd-checkout.md`, cambias ⏳ a ✅, agregas fecha.
- *"Ya todos aprobaron el PRD de Sermutual, pásalo a v1.0"* → verificas que todos estén en ✅, cambias el estado del documento a 🟢 Aprobado v1.0, agregas una fila al historial de revisiones.
- *"Hice cambios menores al PRD de Berna, súbelo a v1.1"* → agregas fila al historial explicando qué cambió, actualizas el número de versión en el header.

Para estas actualizaciones, **no vuelvas a hacer todas las preguntas de la Fase 1**. Solo edita lo que corresponde y confirma al usuario qué cambió.

---

## Cierre

Al terminar el PRD:

1. Guárdalo como `prd-[nombre-proyecto].md` en la carpeta actual.
2. Muestra al usuario un resumen ejecutivo de qué se generó:
   > Listo. El PRD quedó en `prd-[nombre-proyecto].md`.
   >
   > **Resumen:**
   > - [N] requerimientos funcionales definidos
   > - [N] riesgos identificados ([N] críticos)
   > - [N] abiertos por resolver
   > - Longitud: [N] páginas aprox
   >
   > **Próximos pasos sugeridos:**
   > - `/historia` para detallar cada requerimiento con escenarios Gherkin
   > - `/pre-kickoff` para preparar la reunión con ingeniería
   > - Resolver los abiertos críticos antes de compartir
   >
   > ¿Con cuál seguimos?
