# Matriz Poder / Interés — Estrategias de Engagement

Herramienta central del PMBOK KA 13 (Stakeholder Management). Clasifica a cada
stakeholder en uno de cuatro cuadrantes según su **poder** (capacidad de influir
en el proyecto) y su **interés** (grado en que le afectan los resultados).

---

## Los cuatro cuadrantes

```
         Alto poder
              │
  Gestionar   │  Gestionar
  de cerca    │  activamente
  (H/L)       │  (H/H)
──────────────┼──────────────── Alto interés
  Monitorizar │  Mantener
  (L/L)       │  informado
              │  (L/H)
         Bajo poder
```

---

## Q1 — Alta poder, Alto interés (Gestionar activamente)

**Quiénes son:** sponsors, clientes directos, directores de departamento con
presupuesto o decisión sobre el proyecto.

**Estrategia:**
- Reuniones regulares (mínimo quincenal en fase de ejecución)
- Involucrar en decisiones clave: cambios de alcance, cambios de baseline
- Comunicación proactiva de riesgos antes de que se materialicen
- Validación de entregables antes de la revisión formal

**Señales de que están en este cuadrante:**
- Firman el acta de constitución o el contrato
- Tienen autoridad para aprobar o rechazar cambios
- Su insatisfacción podría cancelar el proyecto

---

## Q2 — Alta poder, Bajo interés (Gestionar de cerca)

**Quiénes son:** ejecutivos que financian el proyecto pero delegan el día a día,
equipos de compliance o legal con veto sobre entregables.

**Estrategia:**
- Actualizaciones periódicas concisas (resumen ejecutivo mensual)
- Alertarles únicamente cuando su intervención es necesaria
- No saturar con detalles operativos — perderán el interés y dejarán de leer
- Mantener canal de escalada abierto y documentado

**Señales de que están en este cuadrante:**
- Están en el BCC de los emails de estado
- Preguntan "¿cómo va?" en pasillos pero no participan en reuniones
- Su firma se necesita al cierre, no durante la ejecución

---

## Q3 — Bajo poder, Alto interés (Mantener informado)

**Quiénes son:** usuarios finales del entregable, equipos que dependen del
resultado pero no deciden sobre el proyecto.

**Estrategia:**
- Updates regulares (newsletter, demos, puertas abiertas)
- Recoger feedback temprano para evitar rechazo en entrega
- Incluir en UAT (pruebas de aceptación de usuario)
- No pedirles que tomen decisiones — su rol es feedback, no aprobación

**Señales de que están en este cuadrante:**
- Usan el producto o proceso final
- Participan en reuniones de requisitos pero no en steering committee
- Pueden crear ruido si no se sienten escuchados

---

## Q4 — Bajo poder, Bajo interés (Monitorizar)

**Quiénes son:** proveedores secundarios, departamentos con impacto marginal,
auditores pasivos.

**Estrategia:**
- Comunicación mínima bajo demanda
- Incluir en distribuciones de correo generales pero no en reuniones
- Revisar trimestralmente si su poder/interés ha cambiado

**Señales de que están en este cuadrante:**
- Reciben copias de documentos pero raramente responden
- Su nombre aparece en CC por cortesía o protocolo

---

## Cómo estimar poder e interés sin datos explícitos

Cuando el skill analiza emails y documentos de un proyecto y encuentra menciones
de personas, aplica estas heurísticas para clasificarlas:

| Señal en el proyecto | Inferencia |
|---|---|
| Firma documentos, aparece en "Aprobado por" | Alto poder |
| Destinatario principal (To:) en emails de decisión | Alto poder |
| Solo aparece en CC, nunca en To | Bajo poder |
| Pregunta activamente sobre el estado | Alto interés |
| Responde rápido, hace preguntas detalladas | Alto interés |
| Solo recibe resúmenes, no responde | Bajo interés |
| Citado como "necesita saber" por otros stakeholders | Medio interés |

Cuando la señal es ambigua, clasificar en cuadrante más conservador (mayor poder,
mayor interés) y anotarlo como "estimado — pendiente de confirmación".

---

## Campos del registro de stakeholders (PMBOK 13.1.3)

Cada stakeholder identificado debe tener:

| Campo | Descripción | Obligatorio |
|---|---|---|
| `name` | Nombre completo | Sí |
| `role` | Rol en el proyecto (sponsor, cliente, usuario, etc.) | Sí |
| `organization` | Empresa u organización | No |
| `power` | `high` / `medium` / `low` | Sí |
| `interest` | `high` / `medium` / `low` | Sí |
| `quadrant` | Q1 / Q2 / Q3 / Q4 (derivado de poder × interés) | Sí |
| `engagement_strategy` | Texto libre — qué, cuándo, cómo | Sí |
| `evidence` | De dónde viene esta identificación (email, documento, etc.) | Sí |
| `confidence` | `high` / `medium` / `low` — certeza de la clasificación | Sí |

No propongas un stakeholder si no puedes rellenar al menos: `name`, `role`,
`power`, `interest`, `evidence`, y `confidence`.
