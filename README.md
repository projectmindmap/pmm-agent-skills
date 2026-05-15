# PMM Agent Skills

Agent Skills for [ProjectMindMap](https://app.projectmindmap.com) (PMM) — connect external AI agents (Claude, Copilot, Cursor, Gemini, etc.) to PMM via its MCP server.

Built following the [AgentSkills.io](https://agentskills.io) open standard.

---

## Available Skills

| Skill | Description |
|---|---|
| [`risk-manager`](./risk-manager/) | Full PMI/PMBOK risk management audit — identify, qualify, quantify, plan responses, and monitor project risks |
| [`executive-reporter`](./executive-reporter/) | Generate a visual HTML executive report with health dashboard, risk matrix, and milestone status |

---

## Setup

### 1. Get a PMM MCP Token

1. Log in to [ProjectMindMap](https://app.projectmindmap.com) (Pro or Enterprise plan required)
2. Go to **Profile → MCP Tokens**
3. Click **Generate Token**
4. Choose ability: `Read only` or `Read + Propose`
5. Copy the token — it expires in 90 days

> Tokens with `Read only` let agents read project context and search knowledge.  
> Tokens with `Read + Propose` also let agents submit proposals to your inbox for review.

### 2. Configure MCP in Your Agent

**Claude Code** (`~/.claude/claude.json` or project `.claude/claude.json`):
```json
{
  "mcpServers": {
    "pmm": {
      "type": "http",
      "url": "https://app.projectmindmap.com/mcp/pmm",
      "headers": {
        "Authorization": "Bearer YOUR_PMM_TOKEN"
      }
    }
  }
}
```

**Claude.ai** (Desktop):  
Settings → Claude Code → MCP Servers → Add Server → paste the URL and token.

**Cursor / VS Code**:  
Add to your MCP config file:
```json
{
  "pmm": {
    "url": "https://app.projectmindmap.com/mcp/pmm",
    "headers": { "Authorization": "Bearer YOUR_PMM_TOKEN" }
  }
}
```

**OAuth (alternative)**:  
PMM also supports OAuth for clients that use redirect-based auth (`claude://`, `cursor://`, `vscode://` schemes). Use the discovery URL `https://app.projectmindmap.com/mcp/pmm` — the client will walk you through OAuth automatically.

### 3. Install the Skills

**Claude Code** (recommended):  
Add this repo as a skill plugin in your `.claude/settings.json`:
```json
{
  "plugins": [
    "https://github.com/projectmindmap/pmm-agent-skills"
  ]
}
```

Or clone locally and reference the path:
```json
{
  "plugins": [
    "/path/to/pmm-agent-skills"
  ]
}
```

**Claude.ai**:  
Download this repo as a ZIP → drag into Claude.ai → Project Knowledge.

**Other agents** (Cursor, Copilot, Gemini CLI):  
Clone this repo and follow the agent's skill/plugin installation docs. Skills follow the AgentSkills.io open format.

---

## MCP Tools Reference

These tools are available once you connect to PMM via MCP:

| Tool | Mode | Description |
|---|---|---|
| `list_projects` | Read | List PMM projects accessible to you |
| `get_project_context` | Read | Compact project summary — health, baseline, counts, gaps |
| `query_project_knowledge` | Read | Semantic search over indexed project documents |
| `validate_entity` | Read | Validate a stakeholder, risk, or scope item before submitting |
| `submit_proposal` | Propose | Create a reviewable proposal (human must approve it) |
| `get_proposal_status` | Read | Check the status of a submitted proposal |

> `list_projects` and `list_risks` are being added in [PRO-142](https://linear.app/projectmindmap/issue/PRO-142) and [PRO-143](https://linear.app/projectmindmap/issue/PRO-143). Until then, find your `project_id` in the PMM project URL.

---

## Philosophy

- **PMM is the source of truth.** Agents read context from PMM and submit proposals back — they never write directly.
- **Humans approve.** All agent proposals land in the PMM Inbox. A team member reviews and applies them.
- **Skills are audited.** Every MCP call is logged in PMM's AI Action Log.

---

## Roadmap

- [ ] `risk-manager` v2 — real-time timeline risk detection (milestones + tasks dates via MCP)
- [ ] `executive-reporter` v2 — generate report stored in PMM (PDF/HTML, shareable URL)
- [ ] `scope-reviewer` — ontology gap analysis and scope completeness audit

---

## Contributing

Issues and PRs welcome. Follow the [AgentSkills.io](https://agentskills.io) format for new skills.

## License

MIT
