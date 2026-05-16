# Severity Criteria for PMI Audit Findings

A finding's severity reflects its impact on project success, not its visibility
or ease of correction. Apply the highest matching criterion.

---

## Critical

The gap directly endangers project delivery, compliance with contractual PMI
commitments, or creates unmanaged exposure above severity 15 (P×I).

Assign **Critical** when any of the following is true:

- **No baseline exists.** The project has no approved scope/schedule/cost
  baseline. Nothing can be meaningfully controlled without it.
- **Risk register is empty or has fewer than 3 risks** for a non-trivial project
  (duration > 4 weeks, team > 2 people). An empty register is not "low risk" —
  it is an unexamined risk profile.
- **High-severity unmitigated risks (P×I ≥ 15) with no response strategy.**
  Threats at this level with `response_strategy: null` or `status: identified`
  and no mitigation steps represent active exposure.
- **Zero stakeholders registered** in a project with external parties (client,
  sponsor, vendors). Stakeholder management cannot exist without identification.
- **The project has a PMI/PMBOK contractual commitment** (found in knowledge
  base) and a Critical-level gap exists in the relevant area.

---

## High

The gap creates significant process weakness that will likely surface as a
problem during execution or at closure.

Assign **High** when any of the following is true:

- **Risk register has no coverage in one or more RBS categories** (T, E, O, PM)
  and the project type makes that category likely (e.g., no External risks in a
  project with suppliers; no Organizational risks in a multi-team project).
- **More than 40% of risks with severity ≥ 8 have no mitigation plan.** High
  risks without response plans are deferred problems.
- **Stakeholders registered but with no role, power, or interest classification.**
  An unclassified stakeholder register provides no engagement guidance.
- **Scope coverage below 60%** (PMM health score dimension). Significant scope
  areas are undefined in the ontology.
- **No evidence of a communications plan** in a project with 3+ stakeholders.
  Information flow cannot be managed informally at scale.
- **No project charter or equivalent** found in knowledge base for a project
  older than 2 weeks with a defined scope.

---

## Medium

The gap represents a process maturity shortfall that creates friction or
inefficiency but is unlikely to cause outright failure on its own.

Assign **Medium** when any of the following is true:

- **Risk triggers not defined** for more than 50% of high-severity risks. Risks
  without triggers cannot be monitored proactively.
- **Opportunity risks (risk_type: opportunity) are zero** in the register. PMBOK
  requires both threats and opportunities to be managed.
- **Stakeholder count is low relative to project complexity** (e.g., 1–2
  stakeholders for a multi-phase project) — likely incomplete identification.
- **Scope coverage 60–80%** — meaningful gaps but core deliverables are defined.
- **No quality criteria or acceptance criteria** found in the knowledge base for
  key deliverables.
- **No lessons learned or retrospective** referenced in the knowledge base for
  a project past its midpoint.

---

## Not Assessable

Use this designation — not a severity level — when PMM MCP v1 does not expose
the data needed to evaluate the area. State what information would be needed.

Known Not Assessable areas in PMM MCP v1:
- **Cost Management (KA 7)** — no budget or actual cost data exposed by MCP
- **Schedule Management detail (KA 6)** — milestones and tasks not yet in MCP
  (pending PRO-148)
- **Procurement Management (KA 12)** — no vendor/contract data in MCP
- **Quality Management implementation (KA 8)** — only assessable if the PM has
  uploaded quality plans or test records to the knowledge base
