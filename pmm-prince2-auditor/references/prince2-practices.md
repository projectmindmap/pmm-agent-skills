# PRINCE2 7th Edition — Las 7 Prácticas

Las prácticas (anteriormente llamadas "Temas" en PRINCE2 2017) son las áreas de
gestión que el proyecto debe abordar de forma continua. A diferencia de los
principios (no negociables), las prácticas se adaptan a la escala del proyecto.

Para cada práctica, el checklist indica qué datos MCP permiten evaluarla y con
qué severidad se marca un fallo.

---

## PR1 — Business Case

Documenta la justificación para el proyecto: coste, beneficio, riesgo y tiempo.
Se actualiza a lo largo del proyecto para confirmar que sigue siendo viable.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| PR1.1 | Business case inicial documentado | `query_project_knowledge("Is there a business case, cost-benefit analysis, or investment justification?")` | Documento de business case existe en el knowledge base |
| PR1.2 | Beneficios medibles definidos | `query_project_knowledge("What are the measurable benefits or success metrics?")` | Al menos un beneficio con métrica definida |
| PR1.3 | Relación coste-beneficio referenciada | `query_project_knowledge("What is the estimated cost and expected return of this project?")` | Hay alguna referencia a coste o ROI |

**Severidad:**
- PR1.1 falla → **High** (sin business case no hay práctica de Business Case)
- PR1.2 y PR1.3 fallan, PR1.1 pasa → **Medium**

> *Nota: Coste y datos financieros no están expuestos en MCP v1. Si el knowledge
> base tampoco los contiene, marcar PR1.3 como Not Assessable.*

---

## PR2 — Organización

Define la estructura de gobierno del proyecto: Project Board, PM, y Team
Managers. Cada rol tiene responsabilidades y niveles de autoridad claros.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| PR2.1 | Project Board constituido (Executive + Senior User + Senior Supplier) | `query_project_knowledge("Who is on the project board, steering committee, or governance group?")` | Los tres roles del Project Board están identificados |
| PR2.2 | PM con autoridad delegada | `query_project_knowledge("What authority does the project manager have?")` | La autoridad del PM está documentada |
| PR2.3 | Roles y responsabilidades documentados | `query_project_knowledge("Are roles and responsibilities documented for the project team?")` | Existe un documento de roles o RACI |

**Severidad:**
- PR2.1 falla → **High** (sin Project Board no hay gobierno PRINCE2)
- PR2.2 falla → **Medium**
- PR2.3 falla → **Medium**

---

## PR3 — Calidad (Quality)

Garantiza que los entregables del proyecto cumplen los requisitos acordados.
Incluye el Quality Management Approach, las Product Descriptions con criterios
de calidad, y el Quality Register.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| PR3.1 | Quality Management Approach documentado | `query_project_knowledge("Is there a quality management approach, quality plan, or QA strategy?")` | Existe documento de gestión de calidad |
| PR3.2 | Criterios de aceptación por entregable | `query_project_knowledge("Are there acceptance criteria or quality criteria for project deliverables?")` | Al menos un entregable tiene criterios de aceptación |
| PR3.3 | Revisiones de calidad planificadas | `query_project_knowledge("Are quality reviews, inspections, or testing activities planned?")` | Se mencionan actividades de revisión de calidad |

**Severidad:**
- PR3.1 y PR3.2 fallan → **High**
- Solo PR3.2 falla → **Medium**
- Solo PR3.3 falla → **Medium**

---

## PR4 — Planes (Plans)

PRINCE2 requiere tres niveles de plan: Project Plan (visión global), Stage Plans
(detalle por fase), y Team Plans (opcionales). Los planes gestionan las 6
tolerancias: tiempo, coste, alcance, calidad, riesgo y beneficio.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| PR4.1 | Project Plan (plan general del proyecto) | `query_project_knowledge("Is there a project plan, project timeline, or overall schedule?")` | Existe un plan de proyecto |
| PR4.2 | Stage Plans (planes por fase) | `query_project_knowledge("Are there stage plans or phase plans for each project stage?")` | Al menos un plan de fase existe |
| PR4.3 | Baseline del plan aprobado | `baseline_id` en `get_project_context` | Hay baseline aprobado |

**Nota MCP:** Tareas y milestones detallados no están expuestos en MCP v1
(pendiente PRO-148). Si el knowledge base tampoco los contiene, PR4.2 es
**Not Assessable**.

**Severidad:**
- PR4.1 falla → **High**
- PR4.3 falla → **High** (sin baseline no hay control de plan)
- PR4.2 no assessable → anotar limitación, no penalizar

---

## PR5 — Riesgo (Risk)

Gestiona las amenazas y oportunidades que pueden afectar al proyecto. El Risk
Register documenta todos los riesgos con propietario, probabilidad, impacto y
respuesta.

Esta práctica puede evaluarse en profundidad gracias a `list_risks`.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| PR5.1 | Risk Register no vacío | `list_risks` count | ≥ 3 riesgos para proyectos no triviales |
| PR5.2 | Propietario de riesgo asignado | `owner` field en cada riesgo | > 70% de riesgos tienen propietario |
| PR5.3 | Respuesta definida para riesgos altos | `response_strategy` en riesgos con P×I ≥ 15 | Ningún riesgo alto sin respuesta |
| PR5.4 | Tanto amenazas como oportunidades registradas | `risk_type` en el registro | Al menos una oportunidad registrada |
| PR5.5 | Risk Management Approach documentado | `query_project_knowledge("Is there a risk management approach or risk strategy?")` | Se menciona un enfoque de gestión de riesgos |

**Severidad:**
- PR5.1 falla (registro vacío) → **Critical**
- PR5.3 falla (riesgos altos sin respuesta) → **Critical**
- PR5.2 < 50% con propietario → **High**
- PR5.4 falla (sin oportunidades) → **Medium**
- PR5.5 falla → **Medium**

---

## PR6 — Cambio (Change)

Controla los cambios al alcance, los planes, y los baselines. Incluye el Issue
Register (para problemas, cambios solicitados y off-specifications) y el Change
Management Approach.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| PR6.1 | Proceso de control de cambios documentado | `query_project_knowledge("Is there a change control process or change request procedure?")` | Se describe cómo se gestionan los cambios |
| PR6.2 | Baseline aprobado (base del control de cambios) | `baseline_id` en `get_project_context` | Baseline existe |
| PR6.3 | Tolerancias de cambio definidas | `query_project_knowledge("What are the tolerances or thresholds for approving changes without escalation?")` | Se mencionan umbrales de autorización de cambio |

**Severidad:**
- PR6.1 falla y baseline existe → **High** (hay algo que proteger pero sin proceso)
- PR6.2 falla → ya cubierto por P1 (justificación) — solo anotar aquí como nota
- PR6.3 falla → **Medium**

---

## PR7 — Progreso (Progress)

Monitoriza el avance real versus el plan, gestiona las excepciones, y provee
reportes de estado al Project Board. Incluye los Checkpoint Reports (del PM
hacia el Project Board) y los Highlight Reports (hacia stakeholders).

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| PR7.1 | Mecanismo de reporting de progreso definido | `query_project_knowledge("How is project progress reported? Is there a status report or progress report?")` | Se describe algún mecanismo de reporting |
| PR7.2 | Frecuencia de reporting acordada | `query_project_knowledge("What is the reporting cadence or status update frequency?")` | Se menciona periodicidad |
| PR7.3 | Evidencia de reporting ejecutado | `query_project_knowledge("Are there status reports, highlight reports, or checkpoint reports in this project?")` | Existe al menos un informe de progreso |

**Severidad:**
- PR7.1 falla → **Medium** (sin reporting el Project Board no puede gobernar)
- PR7.2 falla → **Medium**
- PR7.3 falla en proyecto en ejecución → **Medium**

---

## Resumen de severidades por práctica

| Práctica | Fallo crítico posible | Condición |
|---|---|---|
| PR1 Business Case | High | Sin documento de business case |
| PR2 Organización | High | Sin Project Board |
| PR3 Calidad | High | Sin criterios de aceptación ni plan de calidad |
| PR4 Planes | High | Sin project plan o sin baseline |
| PR5 Riesgo | **Critical** | Registro vacío o riesgos altos sin respuesta |
| PR6 Cambio | High | Sin proceso de cambio con baseline existente |
| PR7 Progreso | Medium | Sin mecanismo de reporting |
