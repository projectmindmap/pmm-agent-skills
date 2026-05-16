# Stakeholder Register — {{PROJECT_NAME}}

**Fecha de análisis:** {{DATE}}  
**Generado por:** pmm-stakeholder-manager · pmm-agent-skills  
**Stakeholders ya registrados en PMM:** {{EXISTING_COUNT}}  
**Stakeholders nuevos identificados:** {{NEW_COUNT}}  
**Estado:** Pendiente de revisión en el inbox de PMM

---

## Resumen ejecutivo

- {{BULLET_1: visión general — p. ej. "Se identificaron 4 stakeholders no registrados a partir de emails y documentos indexados"}}
- {{BULLET_2: finding más relevante — p. ej. "El sponsor del proyecto ({{NOMBRE}}) aparece en 3 emails pero no está en el registro"}}
- {{BULLET_3: patrón observado o riesgo — p. ej. "2 de los 4 propuestos son de alta influencia (Q1) — su ausencia del registro implica que no hay plan de engagement para ellos"}}
- **Acción recomendada:** {{CONCRETE_NEXT_STEP — p. ej. "Revisar y aprobar las proposals en el inbox antes de la próxima reunión de steering"}}

---

## Stakeholders actuales (referencia)

> Extraídos de `get_project_context`. No se proponen cambios sobre estos.

| Nombre | Rol registrado | Cuadrante estimado |
|---|---|---|
| {{EXISTING_STAKEHOLDER_1}} | {{ROLE}} | {{Q1/Q2/Q3/Q4}} |
| {{EXISTING_STAKEHOLDER_2}} | {{ROLE}} | {{Q1/Q2/Q3/Q4}} |

*(Si el registro está vacío, esta tabla estará vacía y todos serán nuevos.)*

---

## Stakeholders propuestos

> Cada fila es una proposal separada en el inbox. El PM puede aprobar o rechazar individualmente.

| # | Nombre | Rol inferido | Org | Poder | Interés | Cuadrante | Confianza | Evidencia |
|---|---|---|---|---|---|---|---|---|
| 1 | {{NAME}} | {{ROLE}} | {{ORG}} | {{high/medium/low}} | {{high/medium/low}} | {{Q1-Q4}} | {{high/medium/low}} | {{EVIDENCE}} |
| 2 | {{NAME}} | {{ROLE}} | {{ORG}} | {{high/medium/low}} | {{high/medium/low}} | {{Q1-Q4}} | {{high/medium/low}} | {{EVIDENCE}} |

---

## Plan de engagement propuesto

> Solo para stakeholders en Q1 y Q2 (alta influencia). Q3 y Q4 se gestionan con comunicación estándar.

### {{NAME}} — Q{{N}}

**Estrategia:** {{QUADRANT_STRATEGY — p. ej. "Gestionar activamente: reuniones quincenales, alertas proactivas de riesgos"}}

**Frecuencia de comunicación:** {{FREQUENCY}}  
**Canal principal:** {{CHANNEL}}  
**Contenido clave:** {{WHAT_TO_SHARE}}  
**Responsable:** {{OWNER — "PM" si no se puede inferir}}

---

## Propuestas no enviadas (descartadas)

> Candidatos identificados pero descartados por baja evidencia o porque ya están registrados.

| Nombre | Motivo de descarte |
|---|---|
| {{NAME}} | {{REASON — p. ej. "Solo aparece en un email, evidencia insuficiente" / "Ya está registrado como {{ROLE}}"}} |

---

## Metodología

Este análisis fue realizado por `pmm-stakeholder-manager` usando PMBOK 7 KA 13
como referencia. Fuentes: registro de stakeholders de PMM, emails indexados, y
knowledge base del proyecto. Ningún stakeholder fue propuesto sin evidencia
trazable. Las clasificaciones de Poder/Interés son estimaciones del agente —
la validación humana es necesaria antes de aplicar.

Este informe es una propuesta pendiente de revisión en el inbox de PMM. Los
datos del proyecto no fueron modificados.
