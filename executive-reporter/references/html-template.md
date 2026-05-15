# PMM Executive Report — HTML Template

This file is referenced by `executive-reporter/SKILL.md`. Use it as the base template for all generated reports. Adapt the data sections with real values from MCP tools. Keep all CSS inline.

---

## Brand Colors

```
--pmm-purple:       #7c3aed   (primary brand, headers, badges)
--pmm-purple-light: #ede9fe   (backgrounds, hover states)
--pmm-dark:         #1e1b4b   (text, dark headers)
--pmm-green:        #16a34a   (on track, healthy, success)
--pmm-yellow:       #d97706   (at risk, warning)
--pmm-red:          #dc2626   (critical, delayed, danger)
--pmm-gray:         #6b7280   (muted text, borders)
--pmm-gray-light:   #f3f4f6   (card backgrounds)
--white:            #ffffff
```

---

## Full HTML Template

Adapt this template with real data. Replace all `{{ placeholders }}` with actual values. Remove placeholder rows and add real data rows.

```html
<!DOCTYPE html>
<html lang="{{ project.language | default: 'en' }}">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{ project.name }} — Executive Report</title>
<style>
  /* Reset & base */
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    font-size: 14px;
    line-height: 1.6;
    color: #1e1b4b;
    background: #f8f7ff;
    padding: 24px;
  }

  /* Layout */
  .report { max-width: 900px; margin: 0 auto; background: #fff; border-radius: 12px;
            box-shadow: 0 4px 24px rgba(124,58,237,0.08); overflow: hidden; }

  /* Header */
  .report-header {
    background: linear-gradient(135deg, #7c3aed 0%, #4f46e5 100%);
    color: #fff;
    padding: 32px 40px 28px;
  }
  .report-header h1 { font-size: 26px; font-weight: 700; letter-spacing: -0.5px; }
  .report-header .subtitle { opacity: 0.85; font-size: 13px; margin-top: 4px; }
  .header-meta { display: flex; gap: 16px; margin-top: 16px; flex-wrap: wrap; }
  .meta-chip {
    background: rgba(255,255,255,0.15);
    border: 1px solid rgba(255,255,255,0.25);
    border-radius: 20px;
    padding: 4px 12px;
    font-size: 12px;
    font-weight: 500;
  }
  .health-badge {
    display: inline-flex; align-items: center; gap: 6px;
    padding: 6px 14px; border-radius: 20px; font-weight: 700; font-size: 13px;
  }
  .health-badge.healthy  { background: #16a34a; color: #fff; }
  .health-badge.warning  { background: #d97706; color: #fff; }
  .health-badge.critical { background: #dc2626; color: #fff; }

  /* Sections */
  .section { padding: 28px 40px; border-bottom: 1px solid #ede9fe; }
  .section:last-child { border-bottom: none; }
  .section-title {
    font-size: 11px; font-weight: 700; letter-spacing: 1.2px;
    text-transform: uppercase; color: #7c3aed; margin-bottom: 16px;
  }

  /* Executive summary */
  .summary-list { list-style: none; display: flex; flex-direction: column; gap: 8px; }
  .summary-list li {
    padding: 10px 16px;
    border-left: 3px solid #7c3aed;
    background: #f8f7ff;
    border-radius: 0 8px 8px 0;
    font-size: 13px;
  }
  .summary-list li.good   { border-color: #16a34a; background: #f0fdf4; }
  .summary-list li.warn   { border-color: #d97706; background: #fffbeb; }
  .summary-list li.danger { border-color: #dc2626; background: #fef2f2; }

  /* Health dashboard */
  .health-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
  .health-card {
    background: #f8f7ff; border-radius: 10px; padding: 16px;
    text-align: center; border: 1px solid #ede9fe;
  }
  .health-card .score {
    font-size: 36px; font-weight: 800; line-height: 1;
    margin-bottom: 4px;
  }
  .health-card .label { font-size: 11px; color: #6b7280; font-weight: 500; text-transform: uppercase; letter-spacing: 0.5px; }
  .health-card .bar-track {
    height: 6px; background: #e5e7eb; border-radius: 3px; margin-top: 10px; overflow: hidden;
  }
  .health-card .bar-fill {
    height: 100%; border-radius: 3px;
    transition: width 0.3s;
  }
  .score.green { color: #16a34a; }
  .score.yellow { color: #d97706; }
  .score.red { color: #dc2626; }
  .bar-fill.green { background: #16a34a; }
  .bar-fill.yellow { background: #d97706; }
  .bar-fill.red { background: #dc2626; }

  /* Risk matrix */
  .risk-section { display: grid; grid-template-columns: 1fr 220px; gap: 24px; align-items: start; }
  .risk-matrix-wrap { overflow-x: auto; }
  .risk-matrix {
    border-collapse: collapse; font-size: 11px;
    table-layout: fixed; width: 100%;
  }
  .risk-matrix th, .risk-matrix td {
    width: 40px; height: 40px; text-align: center;
    border: 1px solid #e5e7eb; font-weight: 600;
  }
  .risk-matrix th { background: #f3f4f6; color: #6b7280; }
  .risk-matrix .axis-label { font-size: 10px; writing-mode: vertical-rl; transform: rotate(180deg); background: #f3f4f6; }
  /* Matrix color cells by severity zone */
  .zone-low    { background: #dcfce7; color: #166534; }
  .zone-medium { background: #fef9c3; color: #713f12; }
  .zone-high   { background: #fed7aa; color: #7c2d12; }
  .zone-critical { background: #fecaca; color: #7f1d1d; }
  .risk-matrix td.has-risk { cursor: default; font-size: 16px; }

  /* Risk counts */
  .risk-counts { display: flex; flex-direction: column; gap: 10px; }
  .risk-count-row {
    display: flex; align-items: center; justify-content: space-between;
    padding: 8px 12px; border-radius: 8px; font-size: 12px; font-weight: 600;
  }
  .risk-count-row.critical { background: #fef2f2; color: #991b1b; }
  .risk-count-row.high     { background: #fff7ed; color: #9a3412; }
  .risk-count-row.medium   { background: #fefce8; color: #78350f; }
  .risk-count-row.low      { background: #f0fdf4; color: #166534; }
  .risk-count-row .badge {
    background: currentColor; color: #fff;
    border-radius: 10px; padding: 1px 8px; font-size: 11px;
  }
  /* Fix badge color */
  .risk-count-row.critical .badge { background: #dc2626; }
  .risk-count-row.high .badge     { background: #ea580c; }
  .risk-count-row.medium .badge   { background: #ca8a04; }
  .risk-count-row.low .badge      { background: #16a34a; }

  /* Top risks table */
  .risks-table { width: 100%; border-collapse: collapse; font-size: 12px; }
  .risks-table th {
    text-align: left; padding: 8px 10px;
    background: #f8f7ff; border-bottom: 2px solid #ede9fe;
    font-weight: 600; color: #6b7280; font-size: 11px; text-transform: uppercase;
  }
  .risks-table td { padding: 10px; border-bottom: 1px solid #f3f4f6; vertical-align: top; }
  .risks-table tr:last-child td { border-bottom: none; }
  .severity-pill {
    display: inline-block; border-radius: 12px; padding: 2px 8px;
    font-size: 10px; font-weight: 700; letter-spacing: 0.3px;
  }
  .severity-pill.critical { background: #fecaca; color: #7f1d1d; }
  .severity-pill.high     { background: #fed7aa; color: #7c2d12; }
  .severity-pill.medium   { background: #fef9c3; color: #713f12; }

  /* Milestones */
  .milestone-list { display: flex; flex-direction: column; gap: 12px; }
  .milestone-item { display: flex; align-items: center; gap: 12px; }
  .milestone-name { flex: 1; font-size: 13px; font-weight: 500; }
  .milestone-date { font-size: 11px; color: #6b7280; min-width: 80px; text-align: right; }
  .milestone-bar-track {
    flex: 1; height: 8px; background: #e5e7eb; border-radius: 4px; overflow: hidden;
    min-width: 120px;
  }
  .milestone-bar-fill { height: 100%; border-radius: 4px; }
  .milestone-bar-fill.on-track { background: #16a34a; }
  .milestone-bar-fill.at-risk  { background: #d97706; }
  .milestone-bar-fill.delayed  { background: #dc2626; }
  .milestone-status {
    font-size: 10px; font-weight: 700; text-transform: uppercase;
    min-width: 60px; text-align: right;
  }
  .milestone-status.on-track { color: #16a34a; }
  .milestone-status.at-risk  { color: #d97706; }
  .milestone-status.delayed  { color: #dc2626; }

  /* Open actions */
  .actions-list { display: flex; flex-direction: column; gap: 8px; }
  .action-item {
    display: flex; align-items: flex-start; gap: 12px;
    padding: 10px 14px; background: #fffbeb;
    border: 1px solid #fde68a; border-radius: 8px;
  }
  .action-icon { font-size: 16px; flex-shrink: 0; }
  .action-text { font-size: 13px; }
  .action-text strong { display: block; margin-bottom: 2px; }

  /* Footer */
  .report-footer {
    background: #f8f7ff; padding: 16px 40px;
    border-top: 1px solid #ede9fe;
    display: flex; align-items: center; justify-content: space-between;
    flex-wrap: wrap; gap: 8px;
  }
  .report-footer p { font-size: 11px; color: #9ca3af; }
  .pmm-logo { font-size: 12px; font-weight: 700; color: #7c3aed; }

  /* Print */
  @media print {
    body { background: #fff; padding: 0; }
    .report { box-shadow: none; border-radius: 0; }
    .section { page-break-inside: avoid; }
  }

  /* Responsive */
  @media (max-width: 640px) {
    .section { padding: 20px; }
    .report-header { padding: 20px; }
    .health-grid { grid-template-columns: 1fr; }
    .risk-section { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>
<div class="report">

  <!-- HEADER -->
  <div class="report-header">
    <h1>{{ project.name }}</h1>
    <p class="subtitle">Executive Report · {{ report_date }}</p>
    <div class="header-meta">
      <span class="meta-chip">📅 Baseline: {{ baseline.status }}</span>
      <span class="meta-chip">👥 {{ stakeholders_count }} stakeholders</span>
      <span class="meta-chip">⚠️ {{ risks_total }} risks</span>
      <!-- Health badge: pick class based on score -->
      <span class="health-badge healthy">● Health {{ health_score }}/100</span>
    </div>
  </div>

  <!-- EXECUTIVE SUMMARY -->
  <div class="section">
    <p class="section-title">Executive Summary</p>
    <ul class="summary-list">
      <li class="good">✅ <strong>On track:</strong> {{ summary_positive_point_1 }}</li>
      <li class="good">✅ <strong>Achieved:</strong> {{ summary_positive_point_2 }}</li>
      <li class="warn">⚠️ <strong>Watch:</strong> {{ summary_warning_point }}</li>
      <li class="danger">🔴 <strong>Action needed:</strong> {{ summary_action_needed }}</li>
      <!-- Add or remove bullets as needed -->
    </ul>
  </div>

  <!-- HEALTH DASHBOARD -->
  <div class="section">
    <p class="section-title">Health Dashboard</p>
    <div class="health-grid">
      <div class="health-card">
        <!-- Pick score class: green ≥70, yellow 40-69, red <40 -->
        <div class="score green">{{ scope_health }}%</div>
        <div class="label">Scope Coverage</div>
        <div class="bar-track"><div class="bar-fill green" style="width:{{ scope_health }}%"></div></div>
      </div>
      <div class="health-card">
        <div class="score yellow">{{ risk_health }}%</div>
        <div class="label">Risk Coverage</div>
        <div class="bar-track"><div class="bar-fill yellow" style="width:{{ risk_health }}%"></div></div>
      </div>
      <div class="health-card">
        <div class="score green">{{ stakeholder_health }}%</div>
        <div class="label">Stakeholder Coverage</div>
        <div class="bar-track"><div class="bar-fill green" style="width:{{ stakeholder_health }}%"></div></div>
      </div>
    </div>
  </div>

  <!-- RISK SNAPSHOT -->
  <div class="section">
    <p class="section-title">Risk Snapshot</p>
    <div class="risk-section">
      <!-- 5×5 Risk Matrix (Impact columns, Probability rows) -->
      <div class="risk-matrix-wrap">
        <table class="risk-matrix">
          <thead>
            <tr>
              <th></th>
              <th>I=1</th><th>I=2</th><th>I=3</th><th>I=4</th><th>I=5</th>
            </tr>
          </thead>
          <tbody>
            <!-- P=5 row (highest probability) -->
            <tr>
              <th>P=5</th>
              <td class="zone-medium"></td>
              <td class="zone-high"></td>
              <td class="zone-critical"></td>
              <td class="zone-critical has-risk" title="Risk example">●</td>
              <td class="zone-critical"></td>
            </tr>
            <tr>
              <th>P=4</th>
              <td class="zone-medium"></td>
              <td class="zone-medium"></td>
              <td class="zone-high has-risk" title="Risk example">●</td>
              <td class="zone-critical"></td>
              <td class="zone-critical"></td>
            </tr>
            <tr>
              <th>P=3</th>
              <td class="zone-low"></td>
              <td class="zone-medium"></td>
              <td class="zone-medium"></td>
              <td class="zone-high"></td>
              <td class="zone-critical"></td>
            </tr>
            <tr>
              <th>P=2</th>
              <td class="zone-low"></td>
              <td class="zone-low"></td>
              <td class="zone-medium"></td>
              <td class="zone-medium"></td>
              <td class="zone-high"></td>
            </tr>
            <tr>
              <th>P=1</th>
              <td class="zone-low"></td>
              <td class="zone-low"></td>
              <td class="zone-low"></td>
              <td class="zone-medium"></td>
              <td class="zone-medium"></td>
            </tr>
          </tbody>
        </table>
        <p style="font-size:10px;color:#9ca3af;margin-top:6px;">● = risk registered · P=Probability · I=Impact</p>
      </div>

      <!-- Risk counts sidebar -->
      <div>
        <div class="risk-counts">
          <div class="risk-count-row critical">
            <span>Critical (≥15)</span>
            <span class="badge">{{ risks_critical }}</span>
          </div>
          <div class="risk-count-row high">
            <span>High (8–14)</span>
            <span class="badge">{{ risks_high }}</span>
          </div>
          <div class="risk-count-row medium">
            <span>Medium (4–7)</span>
            <span class="badge">{{ risks_medium }}</span>
          </div>
          <div class="risk-count-row low">
            <span>Low (1–3)</span>
            <span class="badge">{{ risks_low }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Top risks table -->
    <table class="risks-table" style="margin-top:20px;">
      <thead>
        <tr>
          <th>Risk</th>
          <th>Category</th>
          <th>Severity</th>
          <th>Response Strategy</th>
        </tr>
      </thead>
      <tbody>
        <!-- Repeat for each top risk -->
        <tr>
          <td>{{ risk_1_description }}</td>
          <td>{{ risk_1_category }}</td>
          <td><span class="severity-pill critical">{{ risk_1_score }}</span></td>
          <td>{{ risk_1_strategy }}</td>
        </tr>
        <tr>
          <td>{{ risk_2_description }}</td>
          <td>{{ risk_2_category }}</td>
          <td><span class="severity-pill high">{{ risk_2_score }}</span></td>
          <td>{{ risk_2_strategy }}</td>
        </tr>
        <tr>
          <td>{{ risk_3_description }}</td>
          <td>{{ risk_3_category }}</td>
          <td><span class="severity-pill medium">{{ risk_3_score }}</span></td>
          <td>{{ risk_3_strategy }}</td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- MILESTONE STATUS -->
  <div class="section">
    <p class="section-title">Milestone Status</p>
    <div class="milestone-list">
      <!-- Repeat for each milestone -->
      <div class="milestone-item">
        <span class="milestone-name">{{ milestone_1_name }}</span>
        <div class="milestone-bar-track">
          <div class="milestone-bar-fill on-track" style="width:{{ milestone_1_progress }}%"></div>
        </div>
        <span class="milestone-date">{{ milestone_1_date }}</span>
        <span class="milestone-status on-track">On Track</span>
      </div>
      <div class="milestone-item">
        <span class="milestone-name">{{ milestone_2_name }}</span>
        <div class="milestone-bar-track">
          <div class="milestone-bar-fill at-risk" style="width:{{ milestone_2_progress }}%"></div>
        </div>
        <span class="milestone-date">{{ milestone_2_date }}</span>
        <span class="milestone-status at-risk">At Risk</span>
      </div>
      <div class="milestone-item">
        <span class="milestone-name">{{ milestone_3_name }}</span>
        <div class="milestone-bar-track">
          <div class="milestone-bar-fill delayed" style="width:{{ milestone_3_progress }}%"></div>
        </div>
        <span class="milestone-date">{{ milestone_3_date }}</span>
        <span class="milestone-status delayed">Delayed</span>
      </div>
    </div>
  </div>

  <!-- OPEN ACTIONS -->
  <div class="section">
    <p class="section-title">Open Actions</p>
    <div class="actions-list">
      <!-- Add one item per pending decision or approval needed -->
      <div class="action-item">
        <span class="action-icon">🔔</span>
        <div class="action-text">
          <strong>{{ action_1_title }}</strong>
          {{ action_1_description }}
        </div>
      </div>
      <div class="action-item">
        <span class="action-icon">📋</span>
        <div class="action-text">
          <strong>{{ action_2_title }}</strong>
          {{ action_2_description }}
        </div>
      </div>
    </div>
  </div>

  <!-- FOOTER -->
  <div class="report-footer">
    <p>Generated by <strong>PMM Executive Reporter</strong> · Data from ProjectMindMap · {{ report_date }}</p>
    <p class="pmm-logo">ProjectMindMap</p>
  </div>

</div>
</body>
</html>
```

---

## Usage notes

- Place `●` dots in the risk matrix cells where actual risks fall (use the cell's `title` attribute for the risk name).
- For the health score badge class: `healthy` if score ≥ 70, `warning` if 40–69, `critical` if < 40.
- For the score color class: `green` ≥ 70, `yellow` 40–69, `red` < 40.
- Fill the executive summary with the 3 most important positive points and 1–2 warnings/actions. Keep each bullet under 20 words.
- If milestone data is not available from MCP (pending PRO-148), note it as "Milestone detail pending — upgrade to list_milestones tool" in that section.
