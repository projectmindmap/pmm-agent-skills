# PRINCE2 7th Edition — Los 7 Principios

Los principios son las bases no negociables de PRINCE2. Si un proyecto no los
cumple, no se puede considerar gestionado con PRINCE2, independientemente de
que use las prácticas o los procesos. Un principio no aplicado es siempre
**Critical** o **High**.

Cada criterio indica qué tool MCP lo puede verificar.

---

## P1 — Justificación Comercial Continua (Continued Business Justification)

El proyecto debe tener una razón de negocio documentada y válida en todo momento.
Si la justificación deja de ser válida, el proyecto debe detenerse.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| P1.1 | Business case documentado | `query_project_knowledge("Is there a business case, project justification, or ROI analysis?")` | El knowledge base contiene algún documento de justificación |
| P1.2 | Business case referenciado en el baseline | `baseline_id` en `get_project_context` + `query_project_knowledge("Does the project have an approved baseline?")` | El proyecto tiene baseline aprobado |
| P1.3 | Beneficios esperados identificados | `query_project_knowledge("What are the expected benefits, outcomes, or ROI of this project?")` | Al menos un beneficio cuantificable o cualificable está documentado |

**Falla si:**
- P1.1 y P1.2 ambos fallan → **Critical** (sin justificación, el proyecto no tiene base PRINCE2)
- Solo P1.1 falla, baseline existe → **High**
- Solo P1.3 falla → **Medium**

---

## P2 — Aprender de la Experiencia (Learn from Experience)

Los equipos PRINCE2 aprenden de proyectos anteriores. Las lecciones aprendidas
se buscan al inicio, se registran durante, y se comparten al cierre.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| P2.1 | Lecciones aprendidas previas consultadas | `query_project_knowledge("Are there lessons learned from previous projects referenced here?")` | Se mencionan lecciones de proyectos anteriores |
| P2.2 | Registro de lecciones del proyecto actual | `query_project_knowledge("Are there lessons learned or retrospectives documented for this project?")` | Al menos una lección registrada durante el proyecto |

**Falla si:**
- Ambos fallan en proyecto con duración > 4 semanas → **Medium** (procesos de aprendizaje inexistentes)
- Solo P2.2 falla en proyecto en ejecución → **Medium**
- Proyecto recién iniciado (< 2 semanas) → **Not Assessable**

---

## P3 — Roles y Responsabilidades Definidos (Defined Roles and Responsibilities)

PRINCE2 requiere un Project Board con tres roles obligatorios:
- **Executive (Patrocinador):** responsable del business case, tiene autoridad final
- **Senior User:** representa a quienes usarán el producto
- **Senior Supplier:** representa a quienes construyen el producto

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| P3.1 | Executive/Sponsor identificado | `query_project_knowledge("Who is the project sponsor, executive, or project owner?")` | Hay un sponsor/executive con nombre identificado |
| P3.2 | PM (Project Manager) identificado | `get_project_context` (stakeholders) + `query_project_knowledge("Who is the project manager?")` | PM identificado |
| P3.3 | Usuarios o clientes del producto identificados | `query_project_knowledge("Who are the end users or customers of the project deliverables?")` | Al menos un usuario o cliente identificado |
| P3.4 | Proveedores/equipo de entrega identificados | `query_project_knowledge("Who is responsible for delivering or building the project outputs?")` | Al menos un supplier/equipo de entrega identificado |

**Falla si:**
- P3.1 falla → **Critical** (sin Executive no hay Project Board PRINCE2)
- P3.2 falla → **High**
- P3.3 y P3.4 ambos fallan → **High** (sin usuarios ni proveedores, el proyecto no tiene estructura de delivery)
- Solo uno de P3.3/P3.4 falla → **Medium**

---

## P4 — Gestión por Fases (Manage by Stages)

El proyecto se planifica y controla en fases secuenciales. Al final de cada fase,
el Project Board revisa el estado y autoriza (o no) continuar.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| P4.1 | Fases o hitos del proyecto documentados | `query_project_knowledge("What are the project phases, stages, or milestones?")` | Al menos dos fases o hitos están definidos |
| P4.2 | Proceso de revisión al final de fase documentado | `query_project_knowledge("Is there a stage gate, phase review, or go/no-go process?")` | Se menciona algún mecanismo de revisión entre fases |

**Falla si:**
- P4.1 falla → **High** (sin fases no hay gestión por etapas)
- P4.2 falla, P4.1 pasa → **Medium**
- Proyecto muy pequeño (< 4 semanas, equipo < 3) → aplica solo P4.1, marcar P4.2 como **Not Assessable**

---

## P5 — Gestión por Excepción (Manage by Exception)

Cada nivel de la jerarquía del proyecto tiene tolerancias definidas. Solo se
escala al nivel superior cuando las tolerancias se superan. Esto libera al
Project Board de micro-gestionar el día a día.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| P5.1 | Tolerancias o umbrales de escalada definidos | `query_project_knowledge("Are there defined tolerances, thresholds, or escalation criteria for the project?")` | Se mencionan tolerancias (coste, tiempo, alcance, riesgo) |
| P5.2 | Proceso de escalada documentado | `query_project_knowledge("What is the escalation process when issues exceed thresholds?")` | Hay un proceso de escalada definido |

**Falla si:**
- P5.1 falla → **High** (sin tolerancias el PM no puede operar autónomamente)
- P5.2 falla → **Medium**
- Ambos fallan → **High**

---

## P6 — Enfoque en los Productos (Focus on Products)

PRINCE2 orienta el proyecto hacia entregables (products) con criterios de
calidad definidos, no hacia actividades. Cada entregable tiene una Product
Description con criterios de aceptación.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| P6.1 | Entregables del proyecto documentados | `query_project_knowledge("What are the project deliverables or products?")` | Al menos un entregable está descrito |
| P6.2 | Criterios de calidad/aceptación por entregable | `query_project_knowledge("Are there quality criteria or acceptance criteria for deliverables?")` | Al menos un entregable tiene criterios de aceptación explícitos |

**Falla si:**
- P6.1 falla → **High** (sin productos definidos, el proyecto no tiene orientación)
- P6.2 falla → **Medium** (entregables definidos pero sin criterios de calidad)

---

## P7 — Adaptación al Entorno (Tailor to Suit the Project)

PRINCE2 no se aplica de forma rígida. Se adapta a las características del
proyecto (tamaño, complejidad, sector, entorno regulatorio). Un proyecto pequeño
puede tener documentación combinada; uno grande necesita más formalidad.

| # | Criterio | Fuente MCP | Pass |
|---|----------|-----------|------|
| P7.1 | El nivel de formalidad es coherente con el tamaño/riesgo del proyecto | Análisis cualitativo cruzando `get_project_context` (health, risks, stakeholders) con los documentos del knowledge base | La documentación del proyecto es proporcional a su complejidad |

**Nota:** Este principio raramente es **Fail** por sí solo. Se usa para matizar
otros findings: si el proyecto es pequeño y sencillo, reduce la severidad de
findings de P2 o P5.

**Falla si:**
- Proyecto complejo (> 3 meses, > 5 stakeholders, baseline existente) con documentación mínima → **Medium**
- Proyecto simple con documentación excesiva para su tamaño → no es un finding (no penalizar el exceso)
