# PMBOK 7 Audit Checklist — PMM MCP Adaptation

Assessment criteria for each of the 10 PMBOK knowledge areas, adapted to data
available via PMM MCP v1 tools. For each area, apply every criterion that has
available data, then assign Pass / Partial / Fail / Not Assessable.

---

## 1. Integration Management

**Data sources:** `get_project_context` (baseline_id), `query_project_knowledge`

### Criteria

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 1.1 | Project charter exists | `query_project_knowledge("Is there a project charter or project initiation document?")` | Knowledge base contains a charter or equivalent initiation document |
| 1.2 | Approved baseline exists | `baseline_id` in `get_project_context` | Field is present and non-null |
| 1.3 | Change management process referenced | `query_project_knowledge("Is there a change control or change management process?")` | Some reference to change control exists |

### Scoring logic

- 1.2 fails → **Fail, Critical** (no baseline = nothing to control)
- 1.1 fails, project > 2 weeks old → **Partial, High**
- 1.3 not found → **Partial, Medium**
- All pass → **Pass**

---

## 2. Scope Management

**Data sources:** `get_project_context` (health_score dimensions, ontology_gaps), `query_project_knowledge`

### Criteria

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 2.1 | Scope coverage adequate | `health_score.scope_coverage` dimension | ≥ 0.80 (80%) |
| 2.2 | Ontology gaps are minimal | `ontology_gaps` array | 0–2 gaps; each gap is minor or non-blocking |
| 2.3 | Main deliverables are defined | `query_project_knowledge("What are the main deliverables and expected outcomes?")` | At least the primary deliverable is described |
| 2.4 | Acceptance criteria defined | `query_project_knowledge("Are acceptance criteria defined for key deliverables?")` | At least one deliverable has explicit acceptance criteria |

### Scoring logic

- scope_coverage < 0.60 → **Fail, High**
- scope_coverage 0.60–0.79 → **Partial, Medium**
- ≥ 3 blocking ontology gaps → bump severity one level up
- 2.4 not satisfied → add a Medium finding under Quality Management (see KA 5)
- All pass → **Pass**

---

## 3. Schedule Management

**Data sources:** `get_project_context`, `query_project_knowledge`

> PMM MCP v1 does not expose a task list, milestone dates, or dependency graph
> (pending PRO-148). Full schedule assessment is **Not Assessable** at this time.

### Partial assessment available

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 3.1 | Milestones mentioned in knowledge base | `query_project_knowledge("What are the project milestones and target dates?")` | At least one milestone with a target date is documented |
| 3.2 | Schedule baseline referenced | `query_project_knowledge("Is there a project schedule or timeline?")` | A schedule artifact is referenced in the knowledge base |

### Scoring logic

- If neither 3.1 nor 3.2 yields evidence → **Not Assessable** (MCP v1 limitation; note what data would be needed)
- If partial evidence found but no structured task list → **Partial, Medium** with a note that full assessment requires PRO-148
- If both pass and baseline_id exists → **Pass** (schedule is referenced and baselined)

---

## 4. Cost Management

**Data sources:** none available in MCP v1

> PMM MCP v1 does not expose budget baseline, actual cost to date, or earned
> value metrics. This area is **Not Assessable** in all audits until the MCP is
> expanded. Do not invent a finding here.

Mark as: **Not Assessable — budget and actual cost data not exposed by MCP v1.**

---

## 5. Quality Management

**Data sources:** `query_project_knowledge`

### Criteria

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 5.1 | Quality plan or quality criteria exist | `query_project_knowledge("Is there a quality plan, quality criteria, or quality standards for this project?")` | A quality plan or at least measurable quality criteria are referenced |
| 5.2 | Acceptance criteria defined for key deliverables | `query_project_knowledge("Are there acceptance criteria or definition-of-done for deliverables?")` | At least one deliverable has explicit acceptance criteria |
| 5.3 | Test records or quality review results | `query_project_knowledge("Are there test records, review results, or quality assurance artifacts?")` | Some quality verification activity is documented (optional for early-stage projects) |

### Scoring logic

- 5.1 and 5.2 both missing → **Fail, High** (no quality definition means no quality control)
- Only 5.2 missing → **Partial, Medium**
- Only 5.1 missing but acceptance criteria exist → **Partial, Medium**
- 5.3 missing for a project past midpoint → add Medium finding
- All pass → **Pass**

---

## 6. Resource Management

**Data sources:** `get_project_context`, `query_project_knowledge`

### Criteria

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 6.1 | Team roles are defined | `query_project_knowledge("What team roles and responsibilities are defined for this project?")` | At least core roles (PM, team members) are identified with responsibilities |
| 6.2 | Resource assignments linked to deliverables | `query_project_knowledge("How are team members assigned to project tasks or workstreams?")` | At least high-level ownership is documented |
| 6.3 | Project manager identified | Any reference in knowledge base or stakeholder register | A named PM is associated with the project |

### Scoring logic

- 6.1 and 6.3 both missing → **Partial, High** (no accountable team structure)
- 6.1 missing, 6.3 present → **Partial, Medium**
- 6.2 missing only → **Partial, Medium** (assignments likely informal)
- All pass → **Pass**

---

## 7. Communications Management

**Data sources:** `get_project_context` (stakeholders_count), `query_project_knowledge`

### Criteria

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 7.1 | Communications plan exists | `query_project_knowledge("Is there a communications plan or stakeholder communication strategy?")` | A communications plan or equivalent artifact is referenced |
| 7.2 | Reporting cadence defined | `query_project_knowledge("What is the reporting cadence or status update process?")` | At least one recurring communication activity is documented |
| 7.3 | Stakeholder count warrants formal plan | `stakeholders_count` from `get_project_context` | If stakeholders_count ≥ 3, a formal plan is expected |

### Scoring logic

- stakeholders_count ≥ 3 AND 7.1 missing → **Partial, High** (scale requires formal plan)
- stakeholders_count < 3 AND 7.1 missing → **Partial, Medium** (informal may be acceptable)
- 7.2 missing regardless of stakeholder count → add Medium finding
- All pass → **Pass**

---

## 8. Risk Management

**Data sources:** `list_risks`, `get_project_context` (risks_by_severity, open_risks, unmitigated)

This is the most data-rich area. Apply all criteria.

### Criteria

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 8.1 | Risk register is non-empty | `list_risks` result count | ≥ 3 risks for any non-trivial project (duration > 4 weeks OR team > 2) |
| 8.2 | All four RBS categories represented | Category field on each risk (T / E / O / PM) | At least one risk in each category plausible for this project type |
| 8.3 | High-severity risks have response strategies | `response_strategy` and `status` on risks with P×I ≥ 15 | None with `response_strategy: null` and `status: identified` only |
| 8.4 | More than 40% of risks with severity ≥ 8 mitigated | Mitigation status on risks ≥ 8 | ≤ 40% without a mitigation plan |
| 8.5 | Opportunity risks included | `risk_type: opportunity` exists in register | At least one opportunity risk |
| 8.6 | Triggers defined for high-severity risks | `trigger` field on risks with severity ≥ 8 | > 50% of high-severity risks have a defined trigger |

### Scoring logic

- 8.1 fails (empty register) → **Fail, Critical**
- 8.3 fails (unmitigated P×I ≥ 15) → **Fail, Critical**
- 8.2 fails (missing RBS category plausible for project type) → **Partial, High**
- 8.4 fails (>40% high-severity without mitigation) → **Partial, High**
- 8.5 fails → **Partial, Medium**
- 8.6 fails → **Partial, Medium**
- All pass → **Pass**

### RBS category plausibility guide

| Category | Likely present when... |
|----------|----------------------|
| T (Technical) | Project involves software, infrastructure, or integrations |
| E (External) | Project has suppliers, regulators, clients, or market dependencies |
| O (Organizational) | Project spans 2+ teams, has budget/priority competition, or requires sponsorship |
| PM (Project Management) | Any project — PM risks (scope creep, estimation, communication) are universal |

---

## 9. Procurement Management

**Data sources:** none available in MCP v1

> PMM MCP v1 does not expose vendor contracts, SLAs, or procurement plans.
> This area is **Not Assessable** in all audits until the MCP is expanded or
> the PM uploads relevant documents to the knowledge base.

Exception: if `query_project_knowledge("Are there vendor contracts, procurement agreements, or outsourcing arrangements?")` returns substantive evidence, attempt a qualitative assessment and note what is present vs. missing.

---

## 10. Stakeholder Management

**Data sources:** `get_project_context` (stakeholders_count, health_score stakeholder dimension), `query_project_knowledge`

### Criteria

| # | Criterion | Data point | Pass condition |
|---|-----------|-----------|----------------|
| 10.1 | Stakeholders identified | `stakeholders_count` from `get_project_context` | ≥ 1 stakeholder registered for any project with external parties |
| 10.2 | Stakeholder register is classified | `query_project_knowledge("Are stakeholders classified by role, power, or interest?")` | At least one classification axis (power/interest matrix, RACI, role) is applied |
| 10.3 | Stakeholder coverage adequate | `health_score.stakeholder_coverage` dimension | ≥ 0.70 (70%) |
| 10.4 | Engagement strategy referenced | `query_project_knowledge("Is there a stakeholder engagement plan or engagement strategy?")` | Some reference to stakeholder engagement approach exists |

### Scoring logic

- 10.1 fails AND project has known external parties (from knowledge base query) → **Fail, Critical**
- 10.1 fails AND no evidence of external parties → **Partial, High**
- 10.2 fails (stakeholders present but not classified) → **Partial, High** (no engagement guidance)
- 10.3 < 0.70 → **Partial, Medium**
- 10.4 fails → **Partial, Medium** (optional for very small projects)
- All pass → **Pass**

---

## Assessment Summary Template

After applying all criteria above, build the findings table in this order:

1. Fail — Critical (highest priority)
2. Fail — High
3. Partial — Critical
4. Partial — High
5. Partial — Medium
6. Pass
7. Not Assessable (last — these are scope limitations, not findings)

If an area has multiple issues of different severities, report the highest severity
and note the others in the recommendation.
