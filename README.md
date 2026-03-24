<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Workflow — Mobile</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Syne:wght@600;700;800&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0c0c13;--surface:#111120;--surface2:#16162a;
  --border:#1e1e35;--border2:#282848;
  --text:#e6e6f2;--muted:#545470;
  --obsidian:#8b7ff5;--tableau:#f0a830;--claude:#d4826a;--n8n:#52b88a;
  --wire:#28284a;
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
html,body{height:100%;overflow:hidden}
body{background:var(--bg);color:var(--text);font-family:'Syne',sans-serif;display:flex;flex-direction:column}
body::before{content:'';position:fixed;inset:0;background-image:radial-gradient(rgba(255,255,255,.03) 1px,transparent 1px);background-size:24px 24px;pointer-events:none;z-index:0}

/* ── TOP BAR ── */
.topbar{flex-shrink:0;padding:16px 18px 12px;position:relative;z-index:2;background:var(–bg)}
.topbar-inner{display:flex;align-items:center;justify-content:space-between;gap:12px}
.eyebrow{font-family:‘DM Mono’,monospace;font-size:9px;letter-spacing:.2em;text-transform:uppercase;color:var(–muted)}
h1{font-size:20px;font-weight:800;letter-spacing:-.03em;margin-top:4px}
h1 em{font-style:normal;background:linear-gradient(110deg,var(–obsidian),var(–claude),var(–tableau));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.uc-badge{flex-shrink:0;background:var(–surface);border:1px solid var(–border2);border-radius:8px;padding:6px 10px;text-align:center}
.uc-badge .uc-num{font-family:‘DM Mono’,monospace;font-size:18px;font-weight:500;line-height:1}
.uc-badge .uc-of{font-family:‘DM Mono’,monospace;font-size:9px;color:var(–muted);letter-spacing:.1em}

/* ── PILL TABS ── */
.pill-row{flex-shrink:0;display:flex;gap:6px;padding:0 18px 12px;overflow-x:auto;scrollbar-width:none;position:relative;z-index:2}
.pill-row::-webkit-scrollbar{display:none}
.pill{flex-shrink:0;padding:7px 14px;border-radius:100px;border:1px solid var(–border);background:none;color:var(–muted);font-family:‘DM Mono’,monospace;font-size:10.5px;cursor:pointer;letter-spacing:.05em;transition:all .2s}
.pill:active{opacity:.7}
.pill.active{background:var(–surface);border-color:var(–border2);color:var(–text)}
.pill.p0.active{border-color:var(–obsidian);color:var(–obsidian)}
.pill.p1.active{border-color:var(–tableau);color:var(–tableau)}
.pill.p2.active{border-color:var(–claude);color:var(–claude)}
.pill.p3.active{border-color:var(–n8n);color:var(–n8n)}

/* ── SCROLL AREA ── */
.scroll-area{flex:1;overflow-y:auto;overflow-x:hidden;-webkit-overflow-scrolling:touch;position:relative;z-index:1}
.scroll-area::-webkit-scrollbar{display:none}

/* ── PANELS ── */
.panel{display:none;padding:0 18px 80px;animation:fadeUp .28s ease}
.panel.active{display:block}
@keyframes fadeUp{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}

/* ── PANEL HEADER ── */
.phead{margin-bottom:18px}
.phead h2{font-size:16px;font-weight:700;letter-spacing:-.02em;line-height:1.25;margin-bottom:4px}
.phead p{font-family:‘DM Mono’,monospace;font-size:10px;color:var(–muted);letter-spacing:.04em;line-height:1.5}

/* ── VERTICAL FLOW CARD ── */
.vflow{display:flex;flex-direction:column;align-items:center;gap:0}

/* Node card */
.nc{width:100%;background:var(–surface);border:1.5px solid var(–border);border-radius:14px;padding:14px 16px;display:flex;align-items:center;gap:14px;transition:border-color .2s}
.nc.obsidian{border-color:rgba(139,127,245,.5)} .nc.obsidian:active{border-color:var(–obsidian)}
.nc.tableau {border-color:rgba(240,168,48,.45)} .nc.tableau:active{border-color:var(–tableau)}
.nc.claude  {border-color:rgba(212,130,106,.5)} .nc.claude:active{border-color:var(–claude)}
.nc.n8n     {border-color:rgba(82,184,138,.45)} .nc.n8n:active{border-color:var(–n8n)}
.nc.src     {border-color:var(–border2);border-style:dashed}

.nc-left{flex-shrink:0;width:44px;height:44px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:20px;background:var(–bg)}
.nc.obsidian .nc-left{background:rgba(139,127,245,.1)}
.nc.tableau  .nc-left{background:rgba(240,168,48,.1)}
.nc.claude   .nc-left{background:rgba(212,130,106,.1)}
.nc.n8n      .nc-left{background:rgba(82,184,138,.1)}

.nc-right{flex:1;min-width:0}
.nc-tool{font-family:‘DM Mono’,monospace;font-size:9px;letter-spacing:.15em;text-transform:uppercase;margin-bottom:2px}
.obsidian .nc-tool{color:var(–obsidian)} .tableau .nc-tool{color:var(–tableau)} .claude .nc-tool{color:var(–claude)} .n8n .nc-tool{color:var(–n8n)} .src .nc-tool{color:var(–muted)}
.nc-label{font-size:13.5px;font-weight:700;letter-spacing:-.01em;line-height:1.25}
.nc-desc{font-size:11px;color:var(–muted);margin-top:3px;line-height:1.45;font-family:‘DM Mono’,monospace}

.nc-badge{flex-shrink:0;background:var(–surface2);border:1px solid var(–border2);border-radius:6px;font-family:‘DM Mono’,monospace;font-size:9px;padding:2px 7px;color:var(–muted);white-space:nowrap;align-self:flex-start;margin-top:2px}

/* Arrow connector */
.arrow-down{display:flex;align-items:center;flex-direction:column;gap:2px;margin:4px 0;padding-left:22px;align-self:flex-start}
.arrow-shaft{width:1.5px;height:24px;background:var(–wire);position:relative}
.arrow-shaft::after{content:’’;position:absolute;bottom:-1px;left:50%;transform:translateX(-50%);border:5px solid transparent;border-top:7px solid var(–wire)}
.arrow-lbl{font-family:‘DM Mono’,monospace;font-size:9px;color:var(–muted);letter-spacing:.06em}

/* Split label */
.split-label{width:100%;text-align:center;font-family:‘DM Mono’,monospace;font-size:9px;color:var(–muted);letter-spacing:.1em;text-transform:uppercase;margin:10px 0 6px;padding:0;position:relative}
.split-label::before,.split-label::after{content:’’;position:absolute;top:50%;width:30%;height:1px;background:var(–wire)}
.split-label::before{left:0} .split-label::after{right:0}

/* Branch wrapper — side by side on wider mobile */
.branch-pair{display:grid;grid-template-columns:1fr 1fr;gap:10px;width:100%;margin-top:4px}
.branch-col{display:flex;flex-direction:column;gap:0;align-items:center}
.branch-tag{font-family:‘DM Mono’,monospace;font-size:9px;letter-spacing:.1em;text-transform:uppercase;color:var(–muted);margin-bottom:8px;text-align:center}

/* Narrower node for branch */
.nc-sm{border-radius:12px;padding:11px 12px}
.nc-sm .nc-left{width:36px;height:36px;font-size:16px;border-radius:9px}
.nc-sm .nc-label{font-size:12px}
.nc-sm .nc-desc{font-size:10px}

/* ── LOOP FLOW (UC3) ── */
.loop-row{display:grid;grid-template-columns:1fr 1fr;gap:10px;width:100%}
.loop-label{width:100%;text-align:center;font-family:‘DM Mono’,monospace;font-size:9px;color:var(–muted);letter-spacing:.08em;text-transform:uppercase;margin:10px 0 6px;display:flex;align-items:center;gap:8px;justify-content:center}
.loop-line{flex:1;height:1px;background:var(–wire);max-width:60px}

/* ── HUB (UC4) ── */
.hub-center{width:100%;background:var(–surface2);border:2px solid var(–n8n);border-radius:16px;padding:16px;display:flex;align-items:center;gap:14px;box-shadow:0 0 32px rgba(82,184,138,.12)}
.hub-center .nc-left{background:rgba(82,184,138,.12);width:48px;height:48px;font-size:24px;border-radius:12px}
.hub-center .nc-label{font-size:15px}
.hub-outputs{display:grid;grid-template-columns:1fr 1fr;gap:8px;width:100%;margin-top:8px}

/* ── NOTE ── */
.note{margin-top:16px;background:rgba(255,255,255,.025);border:1px dashed var(–border2);border-radius:10px;padding:12px 14px;font-family:‘DM Mono’,monospace;font-size:11px;color:var(–muted);line-height:1.6}
.note strong{color:var(–text)}

/* ── LEGEND STRIP ── */
.legend-strip{flex-shrink:0;display:flex;gap:14px;padding:10px 18px;border-top:1px solid var(–border);background:var(–bg);overflow-x:auto;scrollbar-width:none;position:relative;z-index:2}
.legend-strip::-webkit-scrollbar{display:none}
.leg{display:flex;align-items:center;gap:5px;font-family:‘DM Mono’,monospace;font-size:9.5px;color:var(–muted);letter-spacing:.04em;white-space:nowrap;flex-shrink:0}
.leg-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0}
</style>

</head>
<body>

<!-- TOP BAR -->

<div class="topbar">
  <div class="topbar-inner">
    <div>
      <div class="eyebrow">// Mobile View · Workflow</div>
      <h1>Your <em>4-Tool Stack</em></h1>
    </div>
    <div class="uc-badge">
      <div class="uc-num" id="uc-num">01</div>
      <div class="uc-of">of 04</div>
    </div>
  </div>
</div>

<!-- PILL TABS -->

<div class="pill-row">
  <button class="pill p0 active" onclick="show(0,this)">01 · Tracking</button>
  <button class="pill p1"        onclick="show(1,this)">02 · Data Eval</button>
  <button class="pill p2"        onclick="show(2,this)">03 · Realtime Ops</button>
  <button class="pill p3"        onclick="show(3,this)">04 · Automation</button>
</div>

<!-- SCROLL AREA -->

<div class="scroll-area">

  <!-- ═══ UC1 — Initiative Tracking ═══ -->

  <div class="panel active" id="p0">
    <div class="phead">
      <h2>Initiative Tracking & Document Sharing</h2>
      <p>Obsidian → n8n → Data layer → Tableau → Stakeholders</p>
    </div>
    <div class="vflow">

```
  <div class="nc obsidian">
    <div class="nc-left">📓</div>
    <div class="nc-right">
      <div class="nc-tool">Obsidian</div>
      <div class="nc-label">Write & Track</div>
      <div class="nc-desc">Briefs, OKRs, decisions, meeting notes</div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">file change</div></div>

  <div class="nc n8n">
    <div class="nc-left">🔄</div>
    <div class="nc-right">
      <div class="nc-tool">n8n</div>
      <div class="nc-label">Sync Status</div>
      <div class="nc-desc">Watch vault → push to Google Sheet / Airtable</div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">structured rows</div></div>

  <div class="nc src">
    <div class="nc-left">🗄️</div>
    <div class="nc-right">
      <div class="nc-tool">Data Layer</div>
      <div class="nc-label">Sheet / Airtable</div>
      <div class="nc-desc">Normalised initiative records</div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">live connection</div></div>

  <div class="nc tableau">
    <div class="nc-left">📊</div>
    <div class="nc-right">
      <div class="nc-tool">Tableau</div>
      <div class="nc-label">Dashboard</div>
      <div class="nc-desc">Health, completion %, risk flags for leadership</div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">share link</div></div>

  <div class="nc src">
    <div class="nc-left">👥</div>
    <div class="nc-right">
      <div class="nc-tool">Output</div>
      <div class="nc-label">Stakeholders</div>
      <div class="nc-desc">Tableau Public / Server view</div>
    </div>
  </div>
</div>
<div class="note">💡 <strong>Obsidian</strong> = writing layer. <strong>n8n</strong> = boring sync. <strong>Tableau</strong> = polished face for leadership.</div>
```

  </div>

  <!-- ═══ UC2 — Data Evaluation ═══ -->

  <div class="panel" id="p1">
    <div class="phead">
      <h2>Deep Data Evaluation</h2>
      <p>Tableau for structured views · Claude for fast ad-hoc</p>
    </div>
    <div class="vflow">

```
  <div class="nc src">
    <div class="nc-left">🗄️</div>
    <div class="nc-right">
      <div class="nc-tool">Source</div>
      <div class="nc-label">Data Warehouse / DB</div>
      <div class="nc-desc">Raw operational data</div>
    </div>
  </div>

  <div class="split-label">two paths</div>

  <div class="branch-pair">
    <!-- Path A -->
    <div class="branch-col">
      <div class="branch-tag">Structured</div>
      <div class="nc tableau nc-sm" style="width:100%">
        <div class="nc-left">📊</div>
        <div class="nc-right">
          <div class="nc-tool">Tableau</div>
          <div class="nc-label">KPI Dashboard</div>
          <div class="nc-desc">Trends, cohorts, stable metrics</div>
        </div>
      </div>
      <div class="arrow-down" style="padding-left:18px"><div class="arrow-shaft"></div></div>
      <div class="nc src nc-sm" style="width:100%">
        <div class="nc-left">👥</div>
        <div class="nc-right">
          <div class="nc-tool">Output</div>
          <div class="nc-label">Stakeholder View</div>
          <div class="nc-desc">Shared link</div>
        </div>
      </div>
    </div>
    <!-- Path B -->
    <div class="branch-col">
      <div class="branch-tag">⚡ Ad-hoc</div>
      <div class="nc n8n nc-sm" style="width:100%">
        <div class="nc-left">🔄</div>
        <div class="nc-right">
          <div class="nc-tool">n8n</div>
          <div class="nc-label">Extract & Deliver</div>
          <div class="nc-desc">Format CSV/JSON for analyst</div>
        </div>
      </div>
      <div class="arrow-down" style="padding-left:18px"><div class="arrow-shaft"></div><div class="arrow-lbl">paste to Claude</div></div>
      <div class="nc claude nc-sm" style="width:100%">
        <div class="nc-left">🧠</div>
        <div class="nc-right">
          <div class="nc-tool">Claude+HTML</div>
          <div class="nc-label">Instant Chart</div>
          <div class="nc-desc">"Why did X drop?" in seconds</div>
        </div>
      </div>
    </div>
  </div>
</div>
<div class="note">⚡ <strong>Claude fills the gap</strong> — no dashboard rebuild needed. Paste raw data, ask a question, get an interactive breakdown instantly.</div>
```

  </div>

  <!-- ═══ UC3 — Realtime Ops ═══ -->

  <div class="panel" id="p2">
    <div class="phead">
      <h2>Realtime Ops — Driver Tracking</h2>
      <p>n8n ingests · Claude reasons · HTML displays · loops back</p>
    </div>
    <div class="vflow">

```
  <div class="loop-label"><div class="loop-line"></div>Ingestion<div class="loop-line"></div></div>
  <div class="nc src">
    <div class="nc-left">📡</div>
    <div class="nc-right">
      <div class="nc-tool">Live Sources</div>
      <div class="nc-label">GPS + Orders + Availability</div>
      <div class="nc-desc">Webhooks, APIs, streams</div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">webhook trigger</div></div>

  <div class="nc n8n">
    <div class="nc-left">⚙️</div>
    <div class="nc-right">
      <div class="nc-tool">n8n</div>
      <div class="nc-label">Ingest & Normalise</div>
      <div class="nc-desc">Transform payload, detect threshold events</div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">structured context</div></div>

  <div class="loop-label"><div class="loop-line"></div>Decision<div class="loop-line"></div></div>
  <div class="nc claude">
    <div class="nc-left">🧠</div>
    <div class="nc-right">
      <div class="nc-tool">Claude API</div>
      <div class="nc-label">Reason & Assign</div>
      <div class="nc-desc">Multi-variable optimisation, anomaly flags</div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">decision + display data</div></div>

  <div class="loop-label"><div class="loop-line"></div>Display & Act<div class="loop-line"></div></div>
  <div class="loop-row">
    <div class="nc claude nc-sm" style="width:100%">
      <div class="nc-left">🖥️</div>
      <div class="nc-right">
        <div class="nc-tool">HTML</div>
        <div class="nc-label">Live Dashboard</div>
        <div class="nc-desc">Map, driver table, queue</div>
      </div>
    </div>
    <div class="nc n8n nc-sm" style="width:100%">
      <div class="nc-left">📲</div>
      <div class="nc-right">
        <div class="nc-tool">n8n</div>
        <div class="nc-label">Push & Log</div>
        <div class="nc-desc">Notify driver, log to warehouse</div>
      </div>
    </div>
  </div>
  <div class="arrow-down"><div class="arrow-shaft"></div><div class="arrow-lbl">historical data</div></div>

  <div class="loop-label"><div class="loop-line"></div>Review & Document<div class="loop-line"></div></div>
  <div class="loop-row">
    <div class="nc tableau nc-sm" style="width:100%">
      <div class="nc-left">📊</div>
      <div class="nc-right">
        <div class="nc-tool">Tableau</div>
        <div class="nc-label">Ops Review</div>
        <div class="nc-desc">Trends & SLA analysis</div>
      </div>
    </div>
    <div class="nc obsidian nc-sm" style="width:100%">
      <div class="nc-left">📓</div>
      <div class="nc-right">
        <div class="nc-tool">Obsidian</div>
        <div class="nc-label">Incident Log</div>
        <div class="nc-desc">Post-mortems & learnings</div>
      </div>
    </div>
  </div>
</div>
<div class="note">🔁 <strong>Closed loop:</strong> Live data → AI decision → action → logged → visualised → documented.</div>
```

  </div>

  <!-- ═══ UC4 — Automation Glue ═══ -->

  <div class="panel" id="p3">
    <div class="phead">
      <h2>n8n as Automation Glue</h2>
      <p>Central hub — all triggers flow through n8n</p>
    </div>
    <div class="vflow">

```
  <div class="loop-label"><div class="loop-line"></div>Triggers<div class="loop-line"></div></div>
  <div class="loop-row">
    <div class="nc src nc-sm" style="width:100%"><div class="nc-left">⏰</div><div class="nc-right"><div class="nc-tool">Trigger</div><div class="nc-label">Schedule / Cron</div></div></div>
    <div class="nc src nc-sm" style="width:100%"><div class="nc-left">📥</div><div class="nc-right"><div class="nc-tool">Trigger</div><div class="nc-label">Webhook Event</div></div></div>
  </div>
  <div style="height:6px"></div>
  <div class="loop-row">
    <div class="nc src nc-sm" style="width:100%"><div class="nc-left">🔔</div><div class="nc-right"><div class="nc-tool">Trigger</div><div class="nc-label">Threshold Alert</div></div></div>
    <div class="nc src nc-sm" style="width:100%"><div class="nc-left">📝</div><div class="nc-right"><div class="nc-tool">Trigger</div><div class="nc-label">Form / Manual</div></div></div>
  </div>

  <div class="arrow-down"><div class="arrow-shaft" style="height:30px"></div><div class="arrow-lbl">all routes through</div></div>

  <div class="hub-center">
    <div class="nc-left">🔄</div>
    <div class="nc-right">
      <div class="nc-tool" style="color:var(--n8n);font-family:'DM Mono',monospace;font-size:9px;letter-spacing:.15em;text-transform:uppercase;margin-bottom:2px">n8n Orchestrator</div>
      <div class="nc-label" style="font-size:15px">Central Hub</div>
      <div class="nc-desc" style="font-family:'DM Mono',monospace;font-size:11px;color:var(--muted);margin-top:3px;line-height:1.4">Routes, transforms, triggers, loops</div>
    </div>
  </div>

  <div class="arrow-down"><div class="arrow-shaft" style="height:30px"></div><div class="arrow-lbl">dispatches to</div></div>

  <div class="loop-label"><div class="loop-line"></div>Dispatched To<div class="loop-line"></div></div>
  <div class="hub-outputs">
    <div class="nc claude nc-sm" style="width:100%"><div class="nc-left">🧠</div><div class="nc-right"><div class="nc-tool">Claude</div><div class="nc-label">AI Processing</div><div class="nc-desc">Summarise, decide, draft</div></div></div>
    <div class="nc tableau nc-sm" style="width:100%"><div class="nc-left">📊</div><div class="nc-right"><div class="nc-tool">Tableau</div><div class="nc-label">Data Refresh</div><div class="nc-desc">Push updated sources</div></div></div>
    <div class="nc obsidian nc-sm" style="width:100%"><div class="nc-left">📓</div><div class="nc-right"><div class="nc-tool">Obsidian</div><div class="nc-label">Auto-File Notes</div><div class="nc-desc">Write logs to vault</div></div></div>
    <div class="nc src nc-sm" style="width:100%"><div class="nc-left">📨</div><div class="nc-right"><div class="nc-tool">External</div><div class="nc-label">Notify & Deliver</div><div class="nc-desc">Slack, email, SMS</div></div></div>
  </div>
</div>
<div class="note">🔗 <strong>Rule:</strong> If you move data between tools manually more than twice — automate it in n8n.</div>
```

  </div>

</div><!-- /scroll-area -->

<!-- LEGEND STRIP -->

<div class="legend-strip">
  <div class="leg"><div class="leg-dot" style="background:var(--obsidian)"></div>Obsidian</div>
  <div class="leg"><div class="leg-dot" style="background:var(--tableau)"></div>Tableau</div>
  <div class="leg"><div class="leg-dot" style="background:var(--claude)"></div>Claude+HTML</div>
  <div class="leg"><div class="leg-dot" style="background:var(--n8n)"></div>n8n</div>
  <div class="leg"><div class="leg-dot" style="background:var(--border2);border-radius:50%"></div>Data/Ext</div>
</div>

<script>
const nums=['01','02','03','04'];
function show(i,btn){
  document.querySelectorAll('.panel').forEach((p,j)=>p.classList.toggle('active',i===j));
  document.querySelectorAll('.pill').forEach(t=>t.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('uc-num').textContent=nums[i];
  document.querySelector('.scroll-area').scrollTop=0;
}
</script>

</body>
</html>