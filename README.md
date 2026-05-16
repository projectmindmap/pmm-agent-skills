# PMM Agent Skills

Agent Skills for [ProjectMindMap](https://app.projectmindmap.com) (PMM) — connect external AI agents (Claude, Copilot, Cursor, Gemini, etc.) to PMM via its MCP server.

Built following the [AgentSkills.io](https://agentskills.io) open standard.

---

## Available Skills

| Skill | Description |
|---|---|
| [`risk-manager`](./risk-manager/) | Full PMI/PMBOK risk management audit — identify, qualify, quantify, plan responses, and monitor project risks |
| [`executive-reporter`](./executive-reporter/) | Generate a visual HTML executive report with health dashboard, risk matrix, and milestone status |
| [`pmm-pmi-auditor`](./pmm-pmi-auditor/) | PMBOK 7 compliance audit — evaluates all 10 knowledge areas against PMM data and submits a structured non-conformity report to the PMM inbox for human review. Runs automatically on each baseline update. |
| [`pmm-stakeholder-manager`](./pmm-stakeholder-manager/) | Identifies unregistered stakeholders from indexed emails and documents, classifies them using the Power/Interest matrix (PMBOK KA 13), and proposes each one individually to the PMM inbox. Generates an engagement plan for high-influence stakeholders. |

---

## Setup

### Claude Code (Desktop) — 1 step

Open **Settings → Skills → Add Marketplace** and paste:
```
https://github.com/projectmindmap/pmm-agent-skills
```

That's it. Claude Code will:
1. Install the `pmm-risk-manager` and `pmm-executive-reporter` skills
2. Register the PMM MCP server automatically
3. Trigger an OAuth login to ProjectMindMap the first time a skill runs

> **Requires**: ProjectMindMap Pro or Enterprise plan.

---

### Claude.ai (web) — 2 steps

OAuth has a known bug in the web interface. Use a Bearer token instead:

**Step 1** — Generate a token in PMM:  
Profile → MCP Tokens → Generate Token → choose `Read + Propose` → copy it.

**Step 2** — Add the MCP server manually in Claude.ai:  
Settings → Connectors → Add → HTTP:
```
URL:    https://app.projectmindmap.com/mcp/pmm
Header: Authorization: Bearer YOUR_TOKEN
```

Then add the skills: download this repo as a ZIP → drag into Claude.ai Project Knowledge.

---

### Other agents (Cursor, Copilot, Gemini CLI, VS Code)

**MCP config**:
```json
{
  "pmm": {
    "type": "http",
    "url": "https://app.projectmindmap.com/mcp/pmm",
    "headers": { "Authorization": "Bearer YOUR_TOKEN" }
  }
}
```

Get your token: PMM → Profile → MCP Tokens → Generate Token.

**Skills**: Clone this repo and follow your agent's plugin/skill installation docs. All skills follow the [AgentSkills.io](https://agentskills.io) open format.

---

## MCP Tools Reference

These tools are available once you connect to PMM via MCP:

| Tool | Mode | Description |
|---|---|---|
| Tool | Mode | Description |
|---|---|---|
| `list_projects` | Read | List PMM projects accessible to you |
| `get_project_context` | Read | Compact project summary — health, baseline, counts, gaps |
| `query_project_knowledge` | Read | Semantic search over indexed project documents |
| `list_risks` | Read | Full risk register — category, severity, response strategy, mitigation status, triggers |
| `validate_entity` | Read | Validate a stakeholder, risk, or scope item before submitting |
| `submit_proposal` | Propose | Create a reviewable proposal (human must approve it) |
| `get_proposal_status` | Read | Check the status of a submitted proposal |

> `list_projects` (PRO-142) and `list_risks` (PRO-143) are in progress. Until live, find your `project_id` in the PMM project URL.  
> `audit_report` proposal kind required by `pmm-pmi-auditor` is tracked in PRO-182.

---

## Philosophy

- **PMM is the source of truth.** Agents read context from PMM and submit proposals back — they never write directly.
- **Humans approve.** All agent proposals land in the PMM Inbox. A team member reviews and applies them.
- **Skills are audited.** Every MCP call is logged in PMM's AI Action Log.

---

## Roadmap

**In progress**
- [ ] `pmm-prince2-auditor` — PRINCE2 2023 compliance audit, structured analogously to the PMI auditor

**Planned**
- [ ] `pmm-planner-manager` — schedule and WBS review (requires PRO-148: MCP task/milestone exposure)
- [ ] `pmm-finance-controller` — budget and earned value analysis (requires MCP cost data expansion)
- [ ] `risk-manager` v2 — real-time timeline risk detection (milestones + tasks via MCP)
- [ ] `executive-reporter` v2 — generate report stored in PMM (PDF/HTML, shareable URL)

---

## Contributing

Issues and PRs welcome. Follow the [AgentSkills.io](https://agentskills.io) format for new skills.

## License

MIT
