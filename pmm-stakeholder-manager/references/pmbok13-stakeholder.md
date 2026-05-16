# PMBOK KA 13 — Stakeholder Management: Guía de Referencia

Referencia operativa para el skill `pmm-stakeholder-manager`. Cubre los cuatro
procesos de gestión de stakeholders adaptados a lo que PMM expone via MCP.

---

## Los cuatro procesos

| # | Proceso | Grupo | Qué produce |
|---|---------|-------|-------------|
| 13.1 | Identify Stakeholders | Initiating + Executing | Registro de stakeholders |
| 13.2 | Plan Stakeholder Engagement | Planning | Plan de engagement |
| 13.3 | Manage Stakeholder Engagement | Executing | Acciones de engagement |
| 13.4 | Monitor Stakeholder Engagement | Monitoring & Controlling | Actualizaciones al plan |

Este skill cubre principalmente **13.1** (identificación) y **13.2** (plan),
ya que son los que generan proposals accionables para PMM.

---

## 13.1 — Identificar Stakeholders

### Fuentes de identificación disponibles en PMM

| Fuente | Tool MCP | Qué buscar |
|---|---|---|
| Registro actual | `get_project_context` (stakeholders_count) | Cuántos hay registrados |
| Documentos del proyecto | `query_project_knowledge` | Menciones de personas/orgs no registradas |
| Emails indexados | `query_project_knowledge` | Participantes en CC/To no registrados |
| Contexto del proyecto | `query_project_knowledge` | Clientes, sponsors, proveedores mencionados |

### Queries recomendadas para identificación

Llama a `query_project_knowledge` con estas preguntas en secuencia:

1. `"Who are the project stakeholders, sponsors, clients, or external parties mentioned?"`
2. `"What organizations, companies, or departments are involved in this project?"`
3. `"Who sent or received emails related to this project? List all participants."`
4. `"Who needs to approve deliverables or sign off on the project?"`
5. `"Who are the end users or beneficiaries of the project outcome?"`

### Criterios de identificación

Un candidato a stakeholder debe aparecer en **al menos dos** de estas señales:

- Mencionado por nombre en documentos del proyecto
- Aparece en To/CC de emails del proyecto
- Citado como aprobador, revisor, o receptor de entregables
- Pertenece a una organización con relación contractual al proyecto

Si solo aparece en una señal con baja confianza, incluirlo con `confidence: low`
y marcar claramente que necesita verificación humana.

---

## 13.2 — Plan Stakeholder Engagement

### Objetivo

Definir cómo comunicarse con cada stakeholder según su cuadrante Poder/Interés.
Ver `power-interest-matrix.md` para estrategias detalladas por cuadrante.

### Elementos mínimos del plan de engagement

Para cada stakeholder en Q1 (gestión activa) y Q2 (gestión de cerca), el plan
debe incluir:

| Elemento | Descripción |
|---|---|
| **Frecuencia** | Con qué regularidad se comunica (diario / semanal / quincenal / mensual) |
| **Canal** | Email, reunión, dashboard, informe, call |
| **Contenido** | Qué información necesita recibir (estado general, riesgos, decisiones, etc.) |
| **Responsable** | Quién del equipo gestiona esta relación |

Para Q3 y Q4, basta con indicar el canal y frecuencia mínima.

---

## Anti-hallucination: reglas de evidencia

**Estas reglas no son negociables:**

1. **Nunca proponer un stakeholder sin citar de dónde viene.** Si lo encontraste
   en el knowledge base, cita la query que lo devolvió. Si aparece en un email,
   menciona que el email está indexado.

2. **No inventar roles.** Si el rol no está explícito, usar `"mencionado en
   documentos del proyecto"` — no asignar "Sponsor" o "Cliente" sin evidencia.

3. **No proponer duplicados.** Antes de proponer cualquier stakeholder, comprueba
   si ya existe en el registro de PMM usando `get_project_context`. Si hay match
   por nombre o email, omitir la proposal y anotarlo en el informe.

4. **Confidencia honesta.** Si la clasificación Poder/Interés es inferida (no
   declarada explícitamente), el campo `confidence` debe ser `low` o `medium`.
   Nunca `high` para una inferencia.

---

## Qué hace PMM hoy (contexto del skill)

`ProjectEmailIntelligenceAnalyzer` ya extrae participantes de emails como
`stakeholder_signal` con confidence 0.45 (heurística). Este skill complementa
ese sistema: cruza los participantes detectados con el registro de stakeholders
de PMM, aplica la matriz Poder/Interés, y propone con contexto PMBOK en lugar
de un finding genérico.

El skill NO reemplaza el analyzer — trabaja sobre la misma base de datos de
emails indexados que el analyzer ya procesó.

---

## Señales de éxito de este skill

El output del skill es útil cuando:

- Propone stakeholders concretos con evidencia trazable (no listas genéricas)
- La clasificación Poder/Interés tiene justificación explícita
- El plan de engagement es específico, no boilerplate
- No propone duplicados del registro existente
- El PM puede aprobar o rechazar cada proposal individualmente en el inbox
