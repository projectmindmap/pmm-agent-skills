# PRINCE2 7th Edition — Los 7 Procesos

Los procesos describen QUÉ se hace y CUÁNDO a lo largo del ciclo de vida del
proyecto. A diferencia de las prácticas (continuas), los procesos son eventos
discretos: se ejecutan una vez (o una vez por fase) en momentos específicos.

En PMM MCP v1, la evaluación de procesos es **parcialmente assessable**: se
puede inferir si se han ejecutado desde documentos y contexto, pero no hay un
log de actividad de proyecto. Esta sección guía al auditor sobre qué buscar.

---

## Cuándo evaluar los procesos

La mayoría de los procesos son **Not Assessable** si el proyecto acaba de
empezar. Usa el campo `baseline_id` y las fechas del proyecto (si están en el
knowledge base) para determinar qué procesos deberían haberse ejecutado ya.

| Estado del proyecto | Procesos evaluables |
|---|---|
| Inicio (< 2 semanas, sin baseline) | SU, IP parcialmente |
| Planificación (sin baseline) | SU, IP, DP parcialmente |
| Baseline aprobado | SU, IP, DP, CS (primera fase), MP |
| En ejecución (> 1 fase completada) | Todos excepto CP |
| Cierre | Todos |

---

## PR-SU — Starting Up a Project (Puesta en Marcha)

**Cuándo ocurre:** Antes de iniciar el proyecto formalmente. Es el "¿merece la pena hacer este proyecto?".

**Qué produce:** Project Brief, Project Approach, nombramiento del Project Board y PM.

| Evidencia a buscar | Query MCP |
|---|---|
| Project Brief o documento de inicio | `query_project_knowledge("Is there a project brief, project charter, or initiation document?")` |
| PM y Executive nombrados | `query_project_knowledge("When was the project manager appointed? Who appointed them?")` |

**Severidad si no hay evidencia:** Medium (el proceso puede haberse ejecutado sin dejar rastro en el knowledge base).

---

## PR-IP — Initiating a Project (Iniciación)

**Cuándo ocurre:** Al inicio formal del proyecto, autorizado por el Project Board.

**Qué produce:** Project Initiation Documentation (PID) — el documento maestro que combina business case, plan, estrategias de gestión (calidad, riesgo, comunicación, cambio) y estructura de organización.

| Evidencia a buscar | Query MCP |
|---|---|
| PID o equivalente | `query_project_knowledge("Is there a Project Initiation Document, PID, or equivalent master project document?")` |
| Estrategias de gestión (calidad, riesgo, comunicación) | `query_project_knowledge("Are there management strategies or approaches for quality, risk, and communication?")` |

**Severidad si no hay evidencia:** High (el PID es el documento central de PRINCE2; su ausencia implica que la iniciación no fue formal).

---

## PR-DP — Directing a Project (Dirección)

**Cuándo ocurre:** Continuo — el Project Board toma decisiones en puntos de control (inicio, fin de fase, cierre, excepciones).

**Qué produce:** Decisiones de autorización. No es un proceso con un documento único.

| Evidencia a buscar | Query MCP |
|---|---|
| Actas de decisión del Project Board | `query_project_knowledge("Are there board decisions, steering committee minutes, or project authorization records?")` |
| Aprobaciones de fase | `query_project_knowledge("Are there stage gate approvals or end of stage decisions?")` |

**Nota:** Si el proyecto es pequeño y el PM también es el Executive, este proceso puede estar informalmente integrado. Marcar como **Partial** si hay evidencia de decisiones pero no de un Project Board formal.

**Severidad si no hay evidencia:** Medium para proyectos pequeños; High para proyectos con > 3 meses y múltiples stakeholders.

---

## PR-CS — Controlling a Stage (Control de Fase)

**Cuándo ocurre:** Durante cada fase, el PM autoriza trabajo, monitoriza avance, gestiona issues y reporta al Project Board.

**Qué produce:** Work Packages autorizados, Checkpoint Reports, Issue Reports.

| Evidencia a buscar | Query MCP |
|---|---|
| Checkpoint/status reports | `query_project_knowledge("Are there checkpoint reports, status updates, or work package records?")` |
| Gestión de issues durante la fase | `query_project_knowledge("Are there issue reports or problem logs from the current stage?")` |

**Severidad si no hay evidencia:** Medium (el control puede ejecutarse sin documentación formal en proyectos ágiles adaptados).

---

## PR-MP — Managing Product Delivery (Gestión de Entrega)

**Cuándo ocurre:** El Team Manager (o el equipo) acepta, ejecuta y entrega Work Packages al PM.

**Qué produce:** Productos completados y verificados contra la Product Description.

| Evidencia a buscar | Query MCP |
|---|---|
| Evidencia de entrega de productos | `query_project_knowledge("Are there product deliverables, completed work packages, or delivery confirmations?")` |
| Aceptación de calidad | `query_project_knowledge("Are there quality review records or product acceptance confirmations?")` |

**Severidad si no hay evidencia:** Low/Not Assessable si el proyecto está en fases tempranas. Medium si debería haber entregables ya completados.

---

## PR-SB — Managing a Stage Boundary (Gestión de Límite de Fase)

**Cuándo ocurre:** Al final de cada fase (excepto la última), el PM prepara el End Stage Report y el Stage Plan de la siguiente fase para que el Project Board apruebe continuar.

**Qué produce:** End Stage Report, Stage Plan aprobado, actualización del Project Plan.

| Evidencia a buscar | Query MCP |
|---|---|
| End Stage Report o informe de cierre de fase | `query_project_knowledge("Are there end stage reports, phase completion reports, or stage boundary records?")` |
| Stage Plan de la siguiente fase | `query_project_knowledge("Is there a stage plan or next phase plan approved by the project board?")` |

**Severidad si no hay evidencia:** Not Assessable si el proyecto no ha completado su primera fase. Medium si debería haber habido al menos una revisión de límite de fase.

---

## PR-CP — Closing a Project (Cierre)

**Cuándo ocurre:** Al final del proyecto (última fase). El PM entrega los productos, confirma la aceptación, y prepara el End Project Report y el Benefits Review Plan.

**Qué produce:** End Project Report, Lessons Learned Report, Benefits Review Plan, notificación de cierre al Project Board.

| Evidencia a buscar | Query MCP |
|---|---|
| End Project Report o informe de cierre | `query_project_knowledge("Is there an end project report, project closure document, or final report?")` |
| Lecciones aprendidas del proyecto | `query_project_knowledge("Are there lessons learned or a lessons log from this project?")` |
| Benefits Review Plan | `query_project_knowledge("Is there a benefits review plan or post-project review?")` |

**Severidad si no hay evidencia:** Not Assessable si el proyecto no ha cerrado. High si el proyecto debería haber cerrado ya (inferir por fechas si están en el knowledge base).

---

## Guía de evaluación práctica

Dado que MCP v1 no tiene un log de actividad de proceso, el auditor infiere la
ejecución de procesos desde los artefactos que producen:

- Si el artefacto existe en el knowledge base → el proceso probablemente ocurrió
- Si el artefacto no existe pero el proyecto está en la etapa correcta → Partial
- Si el proyecto es tan temprano que el proceso aún no debería haber ocurrido → Not Assessable

**No penalices la ausencia de documentos que el proyecto aún no ha necesitado.**
PRINCE2 adapta el nivel de formalidad a la etapa del proyecto (principio P7).
