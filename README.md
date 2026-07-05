# PM Playbook
 
> Un libro de jugadas para Product Managers, ejecutable como slash commands en Claude Code. Cada jugada produce un artefacto profesional: discovery, PRDs, historias de usuario, kickoffs, updates a stakeholders y one-pagers ejecutivos.
 
Este repo es portable: se clona en cualquier empresa, se corre `/bootstrap` una vez, y queda listo para usar. El contexto de cada empresa vive en `CLAUDE.md`; las reglas del sistema no cambian entre empresas.
 
---
 
## Tabla de contenidos
 
1. [Qué es esto y para quién](#qué-es-esto-y-para-quién)
2. [Cómo funciona (arquitectura)](#cómo-funciona-arquitectura)
3. [Instalación en una empresa nueva](#instalación-en-una-empresa-nueva)
4. [Los slash commands disponibles](#los-slash-commands-disponibles)
5. [Flujos de trabajo típicos](#flujos-de-trabajo-típicos)
6. [Estructura del repo](#estructura-del-repo)
7. [Cambiar de empresa](#cambiar-de-empresa)
8. [Cómo actualizar el sistema](#cómo-actualizar-el-sistema)
9. [Preguntas frecuentes](#preguntas-frecuentes)
10. [Glosario para PMs nuevos](#glosario-para-pms-nuevos)
---
 
## Qué es esto y para quién
 
### El problema
 
Un Product Manager escribe muchos documentos: PRDs, discovery, historias de usuario, minutas de reunión, updates a stakeholders, one-pagers para leadership. Cada uno tiene su estructura, su audiencia y sus reglas.
 
Los PMs junior tienden a producir documentos vagos, sin trazabilidad, sin métricas claras, sin evidencia. Los PMs senior producen documentos claros pero gastan mucho tiempo en formato y estructura.
 
Este sistema resuelve ambas cosas: **te da estructura para no olvidar nada, y te ayuda a producir rápido sin bajar la calidad.**
 
### Para quién es
 
- Product Managers, especialmente los que están arrancando la carrera.
- Fundadores que hacen product management sin equipo dedicado.
- PMs con experiencia que quieren estandarizar su forma de trabajar.
- Equipos de producto que quieren un baseline consistente de documentación.
### Qué NO es
 
- No reemplaza tu criterio. Todas las decisiones importantes las tomas tú.
- No reemplaza las herramientas de gestión (Linear, Jira, Notion). Genera documentos que después subes o compartes.
- No reemplaza las conversaciones. Un buen PM habla con usuarios, con ingeniería y con leadership — el sistema solo ayuda a documentar y estructurar lo que sale de esas conversaciones.
---
 
## Cómo funciona (arquitectura)
 
El sistema tiene **tres capas**:
 
### Capa 1: Reglas transversales del sistema
 
Archivo: `PLAYBOOK.md`
 
Define el comportamiento base del sistema PM. Tono, estilo, cuándo detenerse, distinción entre conceptos (reglas de negocio vs requerimientos vs criterios de aceptación), reglas de honestidad. **No cambia entre empresas.**
 
### Capa 2: Contexto de la empresa actual
 
Archivo: `CLAUDE.md` (generado por `/bootstrap`)
 
Contexto específico de la empresa donde trabajas: entidades legales, productos, stakeholders, terminología, reglas de compliance, herramientas, métricas, estilo de comunicación. **Cambia por empresa.**
 
### Capa 3: Aprendizajes acumulados
 
Archivo: `LEARNINGS.md`
 
Correcciones que le hiciste al sistema y que quedaron como reglas persistentes. Se llena solo con el tiempo — cada vez que corriges al sistema, él te pregunta si guardar el aprendizaje. **Cambia por empresa.**
 
### Cómo se cargan
 
Claude Code lee automáticamente `CLAUDE.md` al iniciar cada sesión. `CLAUDE.md` importa los otros dos con la sintaxis `@PLAYBOOK.md` y `@LEARNINGS.md`. Todo se inyecta en el contexto antes de que empieces a hablar.
 
Los slash commands (`/prd`, `/historia`, etc.) viven en `.claude/commands/` y se ejecutan al escribir su nombre. Cada uno tiene su propio prompt con instrucciones específicas.
 
---
 
## Instalación en una empresa nueva
 
### Prerrequisitos
 
- Tener [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) instalado. Se instala con:
```bash
  npm install -g @anthropic-ai/claude-code
```
- Tener una terminal cualquiera. Este repo funciona igual en Ghostty, iTerm, Warp, Terminal, la terminal integrada de Cursor, VS Code o Antigravity.
### Pasos
 
1. **Clonar el repo** en la carpeta donde vayas a trabajar:
```bash
   git clone [url-del-repo] pm-playbook
   cd pm-playbook
```
 
2. **Abrir Claude Code:**
```bash
   claude
```
 
3. **Correr el bootstrap:**
```
   /bootstrap
```
   El sistema te entrevistará durante 20-30 minutos por 11 bloques temáticos. Al final generará dos archivos: `CLAUDE.md` con el contexto de tu empresa, y `LEARNINGS.md` vacío para acumular aprendizajes.
 
4. **Cerrar y volver a abrir Claude Code.** Esto es necesario para que cargue el `CLAUDE.md` recién creado.
5. **Empezar a usar los slash commands.** Ver la sección siguiente.
### Verificación de que quedó bien
 
Al abrir Claude Code después del bootstrap, escribí algo como *"¿qué sabes sobre mi empresa?"*. El sistema debería responder con información específica de la empresa que capturaste. Si responde de forma genérica, el `CLAUDE.md` no se cargó bien — revisa que exista en la raíz de la carpeta.
 
---
 
## Los slash commands disponibles
 
### `/bootstrap`
Configuración inicial. Se corre una sola vez por empresa para generar el `CLAUDE.md`.
 
### `/discovery`
Investigación y síntesis del problema antes de escribir un PRD. Soporta 6 fuentes de investigación (entrevistas, benchmark competitivo, desk research, análisis regulatorio, data cuantitativa, reviews y menciones) que se pueden usar por separado o combinadas. También soporta un **modo validación** para casos donde el CEO o un stakeholder ya definió la solución y hay que validarla.
 
**Output:** `discovery-[proyecto].md` o `discovery-validation-[proyecto].md`.
 
### `/prd`
Product Requirement Document. Documento de 8 secciones que responde al por qué y al qué (nunca al cómo). Detecta automáticamente si existe discovery previo y lo usa como input.
 
**Output:** `prd-[proyecto].md`.
 
### `/historia`
Historias de usuario con criterios de aceptación en formato Gherkin. Detecta automáticamente el PRD del proyecto y traduce cada requerimiento funcional en una o más historias. Aplica el framework INVEST y garantiza mínimo 3 escenarios por historia (feliz, error, edge case).
 
**Output:** `historias-[proyecto].md`.
 
### `/pre-kickoff`
Prepara la reunión de arranque con el equipo técnico. Genera dos entregables: un brief de pre-lectura (2 páginas) que se envía 24-48h antes de la reunión, y una agenda estructurada por bloques con distribución de tiempos.
 
**Output:** `pre-kickoff-brief-[proyecto].md` + `pre-kickoff-agenda-[proyecto].md`.
 
### `/post-kickoff`
Procesa las notas de la reunión y genera tres entregables: minuta estructurada, tickets iniciales listos para subir a Linear/Jira, y comunicación de cierre para stakeholders que no estuvieron.
 
**Output:** `post-kickoff-minuta-[proyecto].md` + `post-kickoff-tickets-[proyecto].md` + `post-kickoff-comunicacion-[proyecto].md`.
 
### `/update`
Update periódico de estado a stakeholders (semanal, quincenal o mensual). Formato de mensaje corto para Slack/correo con estado binario (Al día / Requiere atención). Adapta el tono según audiencia (leadership, otros equipos internos, clientes).
 
**Output:** `update-[proyecto]-[YYYY-MM-DD].md`.
 
### `/one-pager`
Documento ejecutivo de máximo 1 página para pedir una decisión a leadership. Sirve para aprobar presupuesto, autorizar arranque, escalar recursos, matar un proyecto, cambiar dirección, extender timeline o cambiar scope.
 
**Output:** `one-pager-[proyecto]-[YYYY-MM-DD].md`.
 
---
 
## Flujos de trabajo típicos
 
### Flujo A — Proyecto nuevo con discovery clásico
 
Este es el flujo ideal cuando arranca un proyecto sin dirección predefinida.
 
```
/discovery       → investigar el problema
/prd             → escribir el PRD basado en el discovery
/historia        → generar historias de usuario del PRD
/pre-kickoff     → preparar reunión con equipo técnico
[reunión]
/post-kickoff    → procesar minuta y generar tickets
/update          → cada semana o quincena durante ejecución
```
 
### Flujo B — Proyecto top-down (el CEO ya definió qué construir)
 
Cuando un stakeholder de alto nivel llega con la solución cocinada.
 
```
/discovery       → modo validación (validar supuestos, no descubrir)
/prd             → escribir el PRD con los hallazgos de validación
/historia        → generar historias
/pre-kickoff     → preparar reunión con equipo técnico
[reunión]
/post-kickoff    → procesar minuta y generar tickets
/update          → cada semana o quincena
```
 
### Flujo C — Pedir presupuesto o aprobación antes de arrancar
 
Cuando necesitas luz verde de leadership antes de invertir tiempo en el proyecto.
 
```
/discovery       → investigación mínima para tener argumentos
/one-pager       → pedir decisión a leadership
[si aprueban]
/prd             → arrancar el proyecto formalmente
[seguir flujo A o B]
```
 
### Flujo D — Solo update de proyecto en ejecución
 
Si el proyecto ya está corriendo, solo usas `/update` en cadencia.
 
```
/update          → semanal
/update          → semanal
/update          → semanal
[al final del proyecto]
/one-pager       → si necesitas escalar recursos o extender timeline
```
 
### Flujo E — Actualizar contexto de la empresa
 
Cuando algo cambia (nuevo producto, nuevo stakeholder, cambio regulatorio):
 
```
Editar CLAUDE.md directamente
    o
/bootstrap       → si es un cambio grande, correr de nuevo
```
 
---
 
## Estructura del repo
 
```
pm-playbook/
├── PLAYBOOK.md                    ← reglas transversales del sistema
├── CLAUDE.md                     ← contexto de la empresa (generado por /bootstrap)
├── LEARNINGS.md                  ← aprendizajes acumulados (se llena con el tiempo)
├── README.md                     ← este archivo
├── .claude/
│   └── commands/
│       ├── bootstrap.md          → /bootstrap
│       ├── discovery.md          → /discovery
│       ├── prd.md                → /prd
│       ├── historia.md           → /historia
│       ├── pre-kickoff.md        → /pre-kickoff
│       ├── post-kickoff.md       → /post-kickoff
│       ├── update.md             → /update
│       └── one-pager.md          → /one-pager
└── [archivos generados por los commands]
    ├── discovery-*.md
    ├── prd-*.md
    ├── historias-*.md
    ├── pre-kickoff-*.md
    ├── post-kickoff-*.md
    ├── update-*.md
    └── one-pager-*.md
```
 
### Qué versionar en git y qué no
 
**Sí versionar:**
- `PLAYBOOK.md`
- `README.md`
- Toda la carpeta `.claude/commands/`
**Sí versionar (depende de sensibilidad):**
- `CLAUDE.md` — contiene contexto de la empresa. Si el repo es privado y solo tú lo usas, sí. Si es compartido, considera separar la info sensible.
- `LEARNINGS.md` — aprendizajes. Igual criterio.
**Considerar según caso:**
- Documentos generados (`prd-*.md`, `discovery-*.md`, etc.) — contienen info del proyecto. Depende de la política de tu empresa.
Ejemplo de `.gitignore` conservador si versionas solo el sistema:
```
# No versionar contexto de empresa ni documentos generados
CLAUDE.md
LEARNINGS.md
CLAUDE.md.draft
discovery-*.md
prd-*.md
historias-*.md
pre-kickoff-*.md
post-kickoff-*.md
update-*.md
one-pager-*.md
```
 
---
 
## Cambiar de empresa
 
El repo es portable. Cuando cambies de empresa:
 
1. **Opción A — Carpeta nueva por empresa:**
```bash
   # Clonar el repo en una carpeta específica de la nueva empresa
   git clone [url-del-repo] pm-playbook-nueva-empresa
   cd pm-playbook-nueva-empresa
   claude
   /bootstrap
```
   Ventaja: contextos completamente aislados.
 
2. **Opción B — Rama por empresa en el mismo repo:**
```bash
   cd pm-playbook
   git checkout -b nueva-empresa
   rm CLAUDE.md LEARNINGS.md
   claude
   /bootstrap
```
   Ventaja: un solo repo, historia en git.
 
Ambas funcionan. La Opción A es más limpia si vas a mantener varias empresas simultáneamente (freelance, consultoría). La Opción B es más práctica si solo estás en una empresa a la vez.
 
---
 
## Cómo actualizar el sistema
 
Cuando mejoras un slash command o añades reglas al `PLAYBOOK.md`, esos cambios los quieres tener disponibles en todas las empresas donde uses el repo.
 
### Si usas la Opción A (carpeta por empresa)
 
Cada empresa es un clon independiente. Para propagar mejoras:
 
```bash
# En la empresa donde trabajes actualmente, actualizas el sistema:
cd pm-playbook-empresa-actual
git pull origin main   # si hay cambios remotos
# edita archivos, prueba, commit y push
git add .
git commit -m "mejora: nuevo bloque en discovery"
git push
 
# En otra empresa donde uses el repo:
cd pm-playbook-otra-empresa
git fetch origin
git checkout main -- PLAYBOOK.md .claude/commands/
```
 
Este último comando actualiza SOLO el sistema (reglas + commands) sin tocar el `CLAUDE.md` ni el `LEARNINGS.md` de esa empresa.
 
### Si usas la Opción B (rama por empresa)
 
```bash
# Merger cambios del main a la rama de la empresa:
git checkout empresa-actual
git merge main
```
 
---
 
## Preguntas frecuentes
 
### ¿Puedo editar los slash commands para adaptarlos a mi estilo?
 
Sí. Todos los archivos en `.claude/commands/` son editables. Ajusta lo que necesites. Si crees que tu ajuste podría servirle a otros, considera compartirlo.
 
### ¿Qué pasa si el sistema no sigue las reglas que definí?
 
Tres cosas por revisar, en orden:
1. **¿El `CLAUDE.md` está cargado?** Verifica que el archivo exista en la raíz de la carpeta actual. Al iniciar Claude Code, corre `/memory` para ver qué está cargado.
2. **¿La regla es específica?** "Sé más directo" no funciona. "No uses la palabra X" sí. Reglas vagas producen adherencia baja.
3. **¿La regla contradice otra?** Si en `CLAUDE.md` dices una cosa y en `LEARNINGS.md` dices otra, el sistema puede confundirse. Revisa consistencia.
### ¿Cómo actualizo el estado de un PRD sin regenerarlo?
 
Dile al sistema en lenguaje natural. Ejemplos:
- *"Marca a Juan como aprobado en el PRD de Sermutual."*
- *"Ya todos aprobaron el PRD de checkout, pásalo a v1.0."*
- *"Cambia la métrica principal del PRD de Berna, ahora es X en vez de Y."*
El sistema edita el archivo directamente. No hay que correr `/prd` de nuevo.
 
### ¿Puedo usar esto en Cursor o Antigravity en vez de Claude Code?
 
Claude Code es la herramienta más integrada porque lee `CLAUDE.md` automáticamente. En Cursor o Antigravity puedes usar la extensión de Claude Code (que corre en la terminal integrada), o pegar el contenido de los archivos manualmente en cada conversación. La segunda opción es tediosa; recomendado el primero.
 
### ¿Qué pasa si un documento generado quedó mal?
 
Corrige al sistema. Si es una corrección puntual, se ajusta ese documento. Si es una corrección que aplica siempre (ej: "en mi empresa se dice X, no Y"), el sistema te va a preguntar si guardar el aprendizaje en `LEARNINGS.md` para no volver a cometer el error.
 
### ¿El sistema aprende con el tiempo?
 
Sí, pero de forma controlada. No hay aprendizaje automático en segundo plano. Cada aprendizaje se guarda solo si tú confirmas. Los aprendizajes viven en `LEARNINGS.md`, un archivo que puedes leer, editar o borrar en cualquier momento.
 
### ¿Los documentos generados se pueden usar como input de otros documentos?
 
Sí, y es el patrón principal del sistema. `/discovery` alimenta a `/prd`, `/prd` alimenta a `/historia` y a `/pre-kickoff`, `/post-kickoff` produce input para `/update`. Los slash commands detectan automáticamente los documentos previos del mismo proyecto y los usan.
 
### ¿Cómo comparto un documento con el equipo si vive en mi carpeta local?
 
Los archivos generados son markdown. Puedes:
- Subirlos a Notion / Confluence / Google Docs (copiando el contenido).
- Compartir el archivo directo si tu equipo trabaja con markdown.
- Convertirlos a PDF con herramientas como Pandoc.
Este sistema no publica automáticamente en ninguna plataforma — genera los archivos en tu computador y tú decides cómo distribuirlos.
 
### ¿Qué hago si el bootstrap se interrumpió?
 
Empezar de nuevo. El bootstrap no soporta pausa (fue decisión de diseño para mantener el sistema simple). Toma 20-30 minutos si respondes de corrido, así que reserva ese tiempo antes de arrancar.
 
---
 
## Glosario para PMs nuevos
 
Términos que aparecen en el sistema. Si algo no lo conoces, aquí va la explicación corta:
 
- **PRD (Product Requirement Document):** Documento que describe qué se va a construir y por qué. No cómo — eso lo define ingeniería.
- **RF (Requerimiento Funcional):** Un elemento individual del PRD que describe una capacidad del sistema. Se numeran RF-1, RF-2, etc.
- **Historia de usuario:** Descripción corta de una necesidad del usuario en formato "Como [rol], quiero [acción], para [beneficio]".
- **Gherkin:** Sintaxis estándar para escribir criterios de aceptación. Estructura Dado / Cuando / Entonces. Ayuda a que ingeniería y QA sepan exactamente qué se espera del sistema.
- **INVEST:** Framework para validar historias de usuario. Independent, Negotiable, Valuable, Estimable, Small, Testable.
- **JTBD (Jobs to Be Done):** Enfoque de descubrimiento que se centra en el "trabajo" que el usuario intenta hacer, no en las features. Dos formatos: por rol ("como [rol] necesito X") y por momento ("cuando [contexto] quiero X").
- **Discovery:** Fase de investigación antes de decidir qué construir. Sirve para validar el problema y las oportunidades.
- **Kickoff:** Reunión donde el PM entrega oficialmente un proyecto al equipo técnico para arranque.
- **Sprint:** Ciclo de trabajo del equipo técnico, típicamente 1-2 semanas.
- **Stakeholder:** Cualquier persona con interés en el proyecto — leadership, clientes internos, otros equipos, inversionistas.
- **RACI:** Framework para clarificar roles en un proyecto. Responsable, Aprobador, Consultado, Informado.
- **MoSCoW:** Framework de priorización. Must have, Should have, Could have, Won't have.
- **RICE:** Framework de priorización cuantitativo. Reach × Impact × Confidence ÷ Effort.
- **One-pager:** Documento ejecutivo de máximo una página para pedir una decisión a leadership.
- **North Star metric:** La métrica única que define si un producto o proyecto está funcionando.
- **Guardrail metric:** Métrica que NO se puede romper por lanzar una feature. Ejemplo: latencia, tasa de fraude, uptime.
---
 
## Créditos y contexto
 
Este repo fue construido de forma iterativa entre un Product Manager en formación y Claude (Anthropic), a través de conversaciones diseñadas para producir un sistema práctico, no académico. Cada slash command, cada regla, cada decisión de diseño responde a una preocupación real: cómo un PM produce documentación de calidad sin ahogarse en formato.
 
Se inspira en:
- **Continuous Discovery Habits** de Teresa Torres — para el marco de discovery.
- **The Mom Test** de Rob Fitzpatrick — para las reglas de entrevista a usuarios.
- **Jobs to Be Done** — para el marco de necesidades del usuario.
- **INVEST** de Bill Wake — para validación de historias.
- **Gherkin / BDD** — para criterios de aceptación testeables.
No pretende reemplazar ninguno de estos marcos. Solo los operacionaliza en un flujo diario de PM.
 
---
