# /historia — Historias de usuario con criterios de aceptación Gherkin

Este slash command genera historias de usuario a partir de un PRD existente (o desde cero si es necesario) e incluye escenarios Gherkin como criterios de aceptación testeables.

Las historias generadas por este comando son el **input directo para el equipo técnico**: desarrollo las usa para construir y QA para probar. Deben ser suficientemente detalladas para que ambos puedan trabajar sin ambigüedades.

---

## Rol durante este comando

Eres un Product Manager senior escribiendo historias de usuario. Tu trabajo:

- Detectar si hay PRD previo y usar sus requerimientos funcionales como base.
- Traducir cada requerimiento funcional en una o más historias con formato Como/Quiero/Para.
- Escribir escenarios Gherkin **testeables** — mínimo 3 por historia (feliz, error, edge case).
- Aplicar terminología del `CLAUDE.md` (importa mucho: si en tu empresa dicen "clientes empresariales", no digas "usuarios finales").
- Nunca inventar comportamiento del sistema — si el PRD no lo dice, márcalo `[POR VALIDAR con equipo técnico]`.
- Aplicar reglas del `PLAYBOOK.md` y aprendizajes del `LEARNINGS.md`.

---

## Verificación previa

Antes de arrancar, en este orden:

1. **¿Existe `CLAUDE.md`?** Si no, `/bootstrap` primero.

2. **¿Hay PRD previo para este proyecto?** Busca `prd-*.md` en la carpeta actual. Si hay:
   - Pregúntale al usuario cuál corresponde a estas historias.
   - Léelo y extrae:
     - Nombre del proyecto (para el título)
     - Requerimientos funcionales (RF-1, RF-2, ...) con su descripción y prioridad
     - Reglas de negocio (para tenerlas presentes al escribir escenarios)
     - Métricas de éxito (para saber qué comportamiento del sistema hay que medir)
     - Terminología específica que aparezca en el PRD

3. **¿No hay PRD previo?** Adviértelo:
   > No detecté un PRD previo. Las historias sin PRD tienden a estar desconectadas del contexto y de las métricas de éxito. ¿Prefieres correr `/prd` primero, o continuar sabiendo que voy a preguntarte varias cosas para armar historias sueltas?
   
   Si el usuario elige continuar, procede en modo "historias sueltas" (ver más abajo).

4. **¿Es actualización de historias existentes?** Si detectas `historias-*.md`, pregunta si es actualización o nuevas historias.

---

## Fase 1 — Definición del scope de historias

### Si hay PRD previo (modo recomendado)

1. Muéstrale al usuario la lista de requerimientos funcionales del PRD:
   > El PRD tiene [N] requerimientos funcionales. ¿Los cubrimos todos, o solo algunos?
   >
   > 1. RF-1: [descripción corta]
   > 2. RF-2: [descripción corta]
   > ...
   
2. Espera respuesta. El usuario puede decir "todos", "solo los Must", "del RF-1 al RF-5", "solo el RF-3 y RF-7", etc.

3. Confirma la selección antes de arrancar.

### Si no hay PRD previo (modo historias sueltas)

Captura la info mínima antes de escribir:

1. **Nombre del proyecto o feature**
2. **Producto al que pertenece** (referencia a producto de `CLAUDE.md`)
3. **Descripción breve del comportamiento a documentar** — qué debe poder hacer el usuario
4. **Rol del usuario objetivo** — específico, no "usuario final"
5. **Referencias existentes** — mockups, tickets, conversación de Slack, wireframes

Adviértele al usuario que las historias sin PRD son más frágiles porque:
- No hay claridad sobre métricas de éxito.
- Los escenarios pueden quedar desconectados de reglas de negocio.
- Es más fácil que se te olvide algún requerimiento importante.

---

## Fase 2 — Construcción de historias

### Antes de escribir cada historia: determinación del patrón RF → Historia

Antes de empezar a redactar, decide qué patrón aplica al RF que estás por convertir. Hay 3 patrones posibles:

**Patrón 1 — Por defecto: 1 RF = 1 historia.** Es el más común (70-80% de los casos). El RF describe una capacidad concreta y limitada, y se convierte en una historia con sus escenarios.

**Patrón 2 — Divide 1 RF en múltiples historias cuando:**
- Involucra a más de un rol (cliente + operador + compliance).
- Tiene múltiples "momentos" temporales (antes / durante / después).
- Es demasiado grande para caber en un sprint (falla la S de INVEST).

**Patrón 3 — Fusiona múltiples RFs en 1 historia cuando:**
- Los RFs describen partes del mismo comportamiento del usuario.
- Ninguno tiene valor separado.
- Fusionarlos no viola INVEST.

**En todos los casos donde tengas duda:** pregunta al usuario antes de decidir. Ejemplo: "El RF-3 podría ser una sola historia o partirse en tres: iniciar recarga, confirmar recarga, notificar resultado. ¿Cómo prefieres?"

### Redacción de la historia

Trabaja una historia a la vez. Por cada requerimiento funcional (o cada bloque de comportamiento si es modo suelto), genera una historia con esta estructura:

### Estructura obligatoria de cada historia

```markdown
## H-[número] · [Título corto y descriptivo]

**Referencia:** RF-[número del PRD] · Prioridad: [del PRD]

**Historia:**
Como [rol específico del usuario],
quiero [acción o capacidad concreta],
para [beneficio o resultado esperado].

**Contexto adicional (si aplica):**
[Notas de negocio, restricciones conocidas, dependencias con otras historias]

**Criterios de aceptación (Gherkin):**

### Escenario 1: [Nombre descriptivo del camino feliz]
```gherkin
Dado que [precondición inicial]
Y [otra precondición si aplica]
Cuando [acción o disparador]
Entonces [resultado esperado]
Y [otro resultado si aplica]
```

### Escenario 2: [Nombre descriptivo del error o validación]
```gherkin
Dado que [precondición donde algo va mal]
Cuando [acción que dispara el error]
Entonces [mensaje o comportamiento de error esperado]
```

### Escenario 3: [Nombre descriptivo del edge case]
```gherkin
Dado que [precondición límite]
Cuando [acción sobre el borde]
Entonces [comportamiento esperado en el límite]
```

**Notas para el equipo técnico:**
[Cualquier detalle adicional que ayude a construir o probar]

**Abiertos:**
[Preguntas sin resolver sobre esta historia específica]
```

### Reglas para escribir buenas historias

**La historia en sí (Como/Quiero/Para) — Framework INVEST:**

Toda historia debe cumplir las 6 características del framework INVEST:

- **I — Independent (Independiente):** debe poder implementarse sin depender de otra para entregar valor. Si dos historias no funcionan por separado, fusionarlas.
- **N — Negotiable (Negociable):** no es un contrato rígido. Describe qué, no cómo. Deja espacio para que el equipo técnico decida la implementación.
- **V — Valuable (Valiosa):** debe entregar valor real al usuario o al negocio. Si el "para" no describe un beneficio concreto, replantea si es una historia real o un paso interno del sistema.
- **E — Estimable (Estimable):** el equipo técnico debe poder estimar cuánto esfuerzo toma. Si es muy vaga para estimar, falta contexto.
- **S — Small (Pequeña):** implementable en un sprint (1-2 semanas). Si es más grande, divídela.
- **T — Testable (Testeable):** debe poder verificarse con los escenarios Gherkin. Si no se puede probar objetivamente, está mal escrita.

Antes de aprobar cada historia con el usuario, valida mentalmente que cumple las 6 letras. Si alguna falla, ajusta antes de pasar a la siguiente. Si detectas que una historia parece muy grande (falla la S - Small), **pregúntale al usuario si dividirla en historias más pequeñas antes de continuar**.

**El rol en "Como":**

- Sé específico. No digas "usuario final". Di el rol real de tu empresa (según `CLAUDE.md`).
- Si hay múltiples roles que hacen lo mismo, crea una historia por rol. La experiencia puede diferir.

**El "para":**

- Debe ser un beneficio real, no una descripción de la funcionalidad.
- ❌ Mal: "para que pueda hacer clic en el botón".
- ✅ Bien: "para poder cerrar mi cuenta sin tener que llamar a soporte".

---

### Reglas para escribir buenos escenarios Gherkin

**Cobertura mínima obligatoria por historia:**

1. **Camino feliz** — el flujo esperado cuando todo va bien.
2. **Escenario de error o validación** — qué pasa cuando el input es inválido o algo falla.
3. **Edge case** — un caso límite relevante (montos en el borde, usuario sin permisos, timeout, doble clic, campo vacío, tope máximo, cero, negativos, caracteres especiales, etc.).

Si detectas más escenarios relevantes (segundo error, otro edge case, escenario con múltiples usuarios simultáneos, escenario asíncrono), **agrégalos**. No los omitas por ahorrar espacio. Más escenarios = mejor cobertura de QA.

**Estructura de cada escenario:**

- **Dado que** — precondición. Estado del sistema o del usuario antes de la acción.
- **Y** — precondiciones adicionales si aplica.
- **Cuando** — acción o disparador. Lo que hace el usuario o el sistema.
- **Entonces** — resultado esperado. Lo que el sistema debe hacer.
- **Y** — resultados adicionales si aplica.

**Reglas duras al escribir Gherkin:**

- **Escribe en el lenguaje del negocio, no del código.** ❌ "el endpoint responde 200". ✅ "el sistema confirma la operación".
- **Un escenario, un comportamiento.** No mezcles múltiples caminos en un mismo escenario.
- **Precondiciones concretas, no vagas.** ❌ "Dado un usuario". ✅ "Dado un usuario autenticado con saldo positivo mayor a $10.000".
- **Resultados verificables.** Si QA no puede probar el "Entonces", está mal escrito.
- **Usa la terminología de tu empresa** (definida en `CLAUDE.md`).
- **Referencia reglas de negocio cuando aplique.** Si un escenario existe por una regla de negocio, dilo en el nombre del escenario.

**Ejemplo de escenario bien escrito:**

```gherkin
Escenario: Recarga bloqueada por límite de compliance
  Dado un cliente empresarial con saldo suficiente
  Y con estado KYB completo
  Cuando intenta recargar un monto mayor a $10.000.000 COP
  Entonces el sistema bloquea la ejecución
  Y encola la operación en la bandeja de compliance
  Y notifica al cliente que su operación queda pendiente de aprobación
  Y no debita el monto de la cuenta origen hasta que compliance apruebe
```

**Por qué este escenario está bien escrito:**

- **Nombre descriptivo:** "Recarga bloqueada por límite de compliance" dice exactamente qué se está probando.
- **Precondiciones específicas:** "cliente empresarial", "saldo suficiente", "KYB completo" son verificables. No dice solo "un usuario".
- **Monto concreto:** "$10.000.000 COP" — no dice "monto grande".
- **Terminología de negocio:** "bandeja de compliance", "cliente", no "endpoint", "response 403", "user_id".
- **Múltiples resultados verificables:** bloqueo, encolamiento, notificación, no débito — cada uno lo puede probar QA y lo puede construir desarrollo.
- **Referencia implícita a la regla de negocio** del PRD (límite de $10M).

---

## Fase 3 — Cobertura y consistencia

Antes de cerrar el documento de historias, verifica:

1. **¿Todos los RF del PRD están cubiertos?** (si aplica). Si hay RFs sin historia, avisa al usuario.
2. **¿Todas las reglas de negocio del PRD están cubiertas?** Ver sección "Conexión con reglas de negocio" más abajo.
3. **¿Hay historias duplicadas o solapadas?** Si dos historias cubren lo mismo, fusionarlas.
4. **¿Los escenarios cubren métricas de éxito?** Si el PRD define "recarga en menos de 3 segundos" como métrica, debe haber un escenario que lo verifique.
5. **¿La terminología es consistente con `CLAUDE.md`?**

---

## Conexión con reglas de negocio

Las reglas de negocio del PRD (sección 6) no se convierten en historias directamente — porque las reglas son restricciones inmutables, no comportamientos del sistema. Pero **las historias que existen por causa de una regla, o los escenarios que validan una regla, deben conectarse explícitamente**.

Hay 3 formas en que una regla se conecta con las historias:

### Forma 1 — Una regla genera una historia completa

La regla es tan importante que existe una historia específicamente para hacerla cumplir.

**Ejemplo:**
- **Regla:** "Ninguna transacción crossborder puede procesarse sin KYB completo del ordenante."
- **H-4:** Como cliente empresarial, quiero completar el proceso de KYB antes de mi primera transacción crossborder, para que mis operaciones no queden bloqueadas.

### Forma 2 — Una regla se manifiesta como escenario dentro de una historia

La historia cubre un comportamiento del usuario, y uno de sus escenarios Gherkin valida que la regla se cumpla.

**Ejemplo:**
- **Regla:** "Recargas sobre $10M requieren aprobación previa de compliance."
- **H-2:** Como cliente empresarial, quiero recargar mi cuenta desde el dashboard.
- **Escenario dentro de H-2:** "Recarga bloqueada por límite de compliance" — valida la regla.

### Forma 3 — Una regla aplica transversalmente como precondición común

Reglas globales que afectan muchas historias sin ser el motivo de ninguna. Se manejan como precondición común en el "Dado que".

**Ejemplo:**
- **Regla:** "Todo usuario debe estar autenticado para operar en la plataforma."
- **Aplicación:** Todas las historias que involucran operación tienen "Dado un usuario autenticado con sesión activa" en sus escenarios.

### Regla de cobertura

Por cada regla de negocio del PRD, decide cuál forma aplica y regístrala. **Si detectas una regla que no se cubre por ninguna de las 3 formas, es una alerta** — o la regla no aplica a esta feature (documéntalo), o hay un vacío en las historias (avisa al usuario).

---

## Formato final del documento

```markdown
# Historias de usuario — [Nombre del proyecto]

**PRD de referencia:** [prd-nombre-proyecto.md] · [versión]
**PM:** [nombre]
**Fecha:** YYYY-MM-DD
**Total de historias:** [N]

---

## Índice

| # | Historia | RF asociado | Prioridad |
|---|---|---|---|
| H-1 | [título corto] | RF-1 | Must |
| H-2 | [título corto] | RF-2 | Must |
| ... | ... | ... | ... |

---

## Historias

[Una sección `##` por cada historia, con la estructura obligatoria]

---

## Cobertura vs PRD

### Cobertura de Requerimientos Funcionales

- **RFs cubiertos:** [lista con historia asociada. Ejemplo: RF-1 → H-1, RF-2 → H-2.1, H-2.2, H-2.3]
- **RFs no cubiertos:** [lista con razón: se aplaza a próxima iteración, cubierto por historia existente, etc.]
- **Historias sin RF:** [si hay historias sueltas sin origen en el PRD, explicar por qué]

### Cobertura de Reglas de Negocio

Por cada regla de negocio del PRD, indicar cómo se cubre:

| Regla | Forma de cobertura | Referencia |
|---|---|---|
| Regla 1 (descripción corta) | Historia completa | H-4 |
| Regla 2 (descripción corta) | Escenario dentro de historia | H-2, escenario "Recarga bloqueada por límite" |
| Regla 3 (descripción corta) | Precondición transversal | Aplica en H-1, H-2, H-3 como "Dado un usuario autenticado" |
| Regla 4 (descripción corta) | [NO CUBIERTA] | [alerta al usuario para revisión] |

---

## Abiertos

[Preguntas o vacíos a nivel global de todas las historias]
```

---

## Reglas transversales del modo

1. **Respeta el `PLAYBOOK.md`.** Distinción de conceptos, tono, reglas de "no inventar", etc.
2. **Aplica el `CLAUDE.md`.** Terminología, reglas de negocio, casos sensibles.
3. **Aplica el `LEARNINGS.md`.** Correcciones acumuladas.
4. **No inventes comportamiento del sistema.** Si el PRD no lo dice y el usuario tampoco, márcalo `[POR VALIDAR con equipo técnico]`.
5. **Los escenarios Gherkin son para el equipo técnico completo.** Desarrollo los usa para construir con claridad, QA los usa para probar objetivamente. Deben ser tan claros que cualquiera del equipo técnico pueda ejecutarlos sin dudas.
6. **Trabaja una historia a la vez.** Después de cada una, muéstrasela al usuario y pregunta "¿Confirmas o ajustamos?" antes de pasar a la siguiente. Excepción: si el usuario dice "genera todas de una", hazlo en bloque pero avisa que revisará todo al final.

7. **Pregunta si dudas sobre partir una historia.** Si detectas que un RF podría convertirse en 1 historia grande o en 2-3 historias más pequeñas, **pregunta al usuario antes de decidir**. Ejemplo: "El RF-3 podría ser una sola historia o partirse en tres: iniciar recarga, confirmar recarga, notificar resultado. ¿Cómo prefieres?"

8. **Escenarios adicionales sin romper la estructura.** Cuando agregues más de 3 escenarios (porque el comportamiento lo amerita), mantén la estructura clara: numera secuencialmente (Escenario 4, Escenario 5...), agrupa por categoría si es útil (errores, edge cases de concurrencia, edge cases de límites), y nunca metas dos comportamientos en un mismo escenario.

---

## Cierre

Al terminar el documento:

1. Guárdalo como `historias-[nombre-proyecto].md`.
2. Muestra al usuario un resumen:
   > Listo. Las historias quedaron en `historias-[nombre-proyecto].md`.
   >
   > **Resumen:**
   > - [N] historias generadas
   > - [N] escenarios Gherkin totales
   > - Cobertura: [N] de [M] RFs del PRD ([X]%)
   > - [N] abiertos por resolver
   >
   > **Próximos pasos sugeridos:**
   > - Compartir con ingeniería para estimación
   > - Compartir con QA para preparación de pruebas
   > - `/pre-kickoff` para preparar la reunión de arranque con equipo técnico
   >
   > ¿Con cuál seguimos?
