# Back-order-tracking
<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Order Tracker — BVGlazing Systems</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=IBM+Plex+Mono:wght@400;500&family=Lato:wght@300;400;700&display=swap');

:root {
–bg: #f5f2ec; –surface: #ffffff; –surface2: #f0ede6; –border: #ddd9d0;
–ink: #1a1814; –muted: #8a8478; –accent: #c8410a; –accent2: #2b6cb0;
–green: #276749; –yellow: #b7791f;
–urgent: #c8410a; –normal: #b7791f; –onhold: #c8720a;
–row-urgent: rgba(200,65,10,0.06);
–row-overdue: rgba(200,65,10,0.12);
–row-onhold: rgba(200,114,10,0.06);
–row-done: rgba(39,103,73,0.05);
}

- { margin:0; padding:0; box-sizing:border-box; }
  body {
  background: var(–bg); color: var(–ink); font-family: ‘Lato’, sans-serif; min-height: 100vh;
  }

/* HEADER */
.header { background: var(–surface); border-bottom: 2px solid var(–border); padding: 14px 28px; display: flex; align-items: center; justify-content: space-between; box-shadow: 0 1px 6px rgba(0,0,0,0.06); }
.header-left { display: flex; align-items: center; gap: 14px; }
.header-logo img { display:block; border-radius:8px; }
.brand-name { font-family: ‘Syne’, sans-serif; font-size: 1.1rem; font-weight: 800; color: #1a2e4a; letter-spacing: 0.5px; line-height: 1.2; }
.brand-title { font-family: ‘Syne’, sans-serif; font-size: 0.72rem; font-weight: 700; color: var(–accent); letter-spacing: 2px; text-transform: uppercase; }
.header-sub { font-size: 0.68rem; color: var(–muted); margin-top: 2px; font-family: ‘IBM Plex Mono’, monospace; }
.header-right { display: flex; flex-direction: column; align-items: flex-end; gap: 2px; }
#hclock { font-family: ‘IBM Plex Mono’, monospace; font-size: 1.1rem; color: var(–ink); letter-spacing: 1px; font-weight: 500; }
#hdate { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.65rem; color: var(–muted); letter-spacing: 0.5px; }

/* TABS */
.tabs { display: flex; background: var(–surface); padding: 0 28px; border-bottom: 2px solid var(–border); gap: 4px; }
.tab { padding: 11px 22px; font-family: ‘Syne’, sans-serif; font-size: 0.78rem; font-weight: 700; letter-spacing: 1.5px; text-transform: uppercase; cursor: pointer; color: var(–muted); border-bottom: 3px solid transparent; margin-bottom: -2px; transition: all 0.2s; border-radius: 4px 4px 0 0; }
.tab.active { color: var(–accent); border-bottom-color: var(–accent); background: rgba(200,65,10,0.04); }
.tab:hover:not(.active) { color: var(–ink); background: var(–surface2); }

.content { padding: 24px 28px; }

/* DAILY SUMMARY BANNER */
.daily-summary { background: linear-gradient(135deg, #1a2e4a 0%, #2c4567 100%); color: #fff; border-radius: 12px; padding: 18px 24px; margin-bottom: 22px; display: flex; align-items: center; justify-content: space-between; gap: 20px; box-shadow: 0 4px 16px rgba(26,46,74,0.15); }
.summary-greeting h2 { font-family: ‘Syne’, sans-serif; font-size: 1.3rem; font-weight: 700; letter-spacing: 0.5px; margin-bottom: 4px; }
.summary-greeting p { font-size: 0.85rem; opacity: 0.85; line-height: 1.5; }
.summary-stats { display: flex; gap: 18px; }
.summary-stat { text-align: center; padding: 0 14px; border-left: 1px solid rgba(255,255,255,0.2); }
.summary-stat:first-child { border-left: none; }
.summary-num { font-family: ‘Syne’, sans-serif; font-size: 1.6rem; font-weight: 800; line-height: 1; }
.summary-label { font-size: 0.65rem; letter-spacing: 1.5px; text-transform: uppercase; opacity: 0.75; margin-top: 4px; font-family: ‘IBM Plex Mono’, monospace; }

/* KPI ROW */
.kpi-row { display: grid; grid-template-columns: repeat(5, 1fr); gap: 14px; margin-bottom: 22px; }
.kpi-card { background: var(–surface); border: 1px solid var(–border); border-radius: 8px; padding: 16px 18px; border-top: 3px solid var(–border); transition: box-shadow 0.2s; }
.kpi-card:hover { box-shadow: 0 4px 20px rgba(0,0,0,0.08); }
.kpi-card.red-top { border-top-color: var(–urgent); }
.kpi-card.blue-top { border-top-color: var(–normal); }
.kpi-card.gray-top { border-top-color: var(–onhold); }
.kpi-card.green-top { border-top-color: var(–green); }
.kpi-card.yellow-top { border-top-color: var(–yellow); }
.kpi-label { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.6rem; letter-spacing: 1.5px; text-transform: uppercase; color: var(–muted); margin-bottom: 4px; }
.kpi-val { font-family: ‘Syne’, sans-serif; font-size: 2.2rem; font-weight: 800; line-height: 1; color: var(–ink); }
.kpi-sub { font-size: 0.72rem; color: var(–muted); margin-top: 2px; }

/* TWO COLUMN LAYOUT FOR CHART + RECURRING */
.insights-row { display: grid; grid-template-columns: 1.5fr 1fr; gap: 16px; margin-bottom: 22px; }
.insight-card { background: var(–surface); border: 1px solid var(–border); border-radius: 10px; padding: 18px 20px; }
.insight-title { font-family: ‘Syne’, sans-serif; font-size: 0.85rem; font-weight: 700; letter-spacing: 1.5px; text-transform: uppercase; color: var(–ink); margin-bottom: 14px; display: flex; align-items: center; gap: 8px; }
.insight-title .icon { font-size: 1rem; }

/* CHART */
.chart-container { height: 180px; display: flex; align-items: flex-end; gap: 8px; padding: 10px 0 22px 0; position: relative; }
.bar-group { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 6px; height: 100%; justify-content: flex-end; }
.bar { width: 100%; max-width: 38px; background: linear-gradient(to top, var(–accent), #e57334); border-radius: 4px 4px 0 0; transition: all 0.3s; cursor: pointer; position: relative; min-height: 4px; }
.bar:hover { transform: translateY(-2px); }
.bar-tooltip { position: absolute; top: -28px; left: 50%; transform: translateX(-50%); background: var(–ink); color: #fff; padding: 3px 8px; border-radius: 4px; font-size: 0.7rem; font-family: ‘IBM Plex Mono’, monospace; white-space: nowrap; opacity: 0; pointer-events: none; transition: opacity 0.2s; }
.bar:hover .bar-tooltip { opacity: 1; }
.bar-label { font-size: 0.65rem; color: var(–muted); font-family: ‘IBM Plex Mono’, monospace; letter-spacing: 0.5px; }

/* RECURRING PROBLEMS */
.recurring-list { display: flex; flex-direction: column; gap: 8px; }
.recurring-item { display: flex; align-items: center; justify-content: space-between; padding: 8px 12px; background: var(–surface2); border-radius: 6px; border-left: 3px solid var(–accent); }
.recurring-name { font-size: 0.82rem; font-weight: 700; color: var(–ink); }
.recurring-meta { font-size: 0.7rem; color: var(–muted); margin-top: 1px; }
.recurring-count { font-family: ‘Syne’, sans-serif; font-size: 1.4rem; font-weight: 800; color: var(–accent); }

/* PRIORITY LEGEND */
.priority-legend { display: flex; gap: 18px; margin-bottom: 16px; padding: 10px 14px; background: var(–surface); border: 1px solid var(–border); border-radius: 8px; align-items: center; flex-wrap: wrap; }
.legend-title { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.6rem; letter-spacing: 1px; color: var(–muted); text-transform: uppercase; margin-right: 4px; }
.leg-item { display: flex; align-items: center; gap: 6px; font-size: 0.75rem; font-weight: 700; }
.leg-dot { width: 10px; height: 10px; border-radius: 2px; }

/* NOTIFICATIONS */
#notification-area { position: fixed; top: 20px; right: 20px; z-index: 200; display: flex; flex-direction: column; gap: 10px; max-width: 360px; }
.notification { background: var(–surface); border: 1px solid var(–border); border-left: 4px solid var(–urgent); border-radius: 8px; padding: 12px 16px; box-shadow: 0 8px 24px rgba(0,0,0,0.15); animation: slideInRight 0.3s ease; cursor: pointer; }
.notification.success { border-left-color: var(–green); }
.notification.warning { border-left-color: var(–yellow); }
@keyframes slideInRight { from { transform: translateX(120%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }
.notif-title { font-weight: 700; font-size: 0.85rem; margin-bottom: 4px; }
.notif-msg { font-size: 0.78rem; color: var(–muted); line-height: 1.4; }

/* TOOLBAR */
.toolbar { display: flex; align-items: center; gap: 10px; margin-bottom: 16px; flex-wrap: wrap; }
.search-box { flex: 1; min-width: 200px; padding: 8px 14px; border: 1px solid var(–border); border-radius: 6px; background: var(–surface); font-family: ‘Lato’, sans-serif; font-size: 0.82rem; color: var(–ink); outline: none; transition: border-color 0.2s; }
.search-box:focus { border-color: var(–accent); }
.search-box::placeholder { color: var(–muted); }
select.filter-sel { padding: 8px 12px; border: 1px solid var(–border); border-radius: 6px; background: var(–surface); font-family: ‘IBM Plex Mono’, monospace; font-size: 0.72rem; color: var(–ink); outline: none; cursor: pointer; }
.btn { padding: 8px 18px; border-radius: 6px; font-family: ‘Syne’, sans-serif; font-weight: 700; font-size: 0.72rem; letter-spacing: 1.5px; text-transform: uppercase; cursor: pointer; border: none; transition: all 0.2s; }
.btn-primary { background: var(–accent); color: #fff; }
.btn-primary:hover { background: #a33408; }
.btn-outline { background: transparent; border: 1.5px solid var(–border); color: var(–ink); }
.btn-outline:hover { border-color: var(–ink); }
.btn-sm { padding: 4px 10px; font-size: 0.65rem; border-radius: 4px; }

/* OVERDUE BANNER */
.overdue-banner { background: rgba(200,65,10,0.08); border: 1px solid rgba(200,65,10,0.3); border-left: 4px solid var(–accent); border-radius: 6px; padding: 10px 16px; margin-bottom: 16px; font-size: 0.8rem; color: var(–accent); font-weight: 700; display: flex; align-items: center; gap: 8px; }
.overdue-banner span { font-weight: 400; color: var(–ink); }

/* TABLE */
.table-wrap { background: var(–surface); border: 1px solid var(–border); border-radius: 10px; overflow: hidden; box-shadow: 0 2px 12px rgba(0,0,0,0.05); overflow-x: auto; }
table { width: 100%; border-collapse: collapse; min-width: 900px; }
thead { background: var(–ink); }
th { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.6rem; letter-spacing: 1.5px; text-transform: uppercase; color: #9a9690; padding: 12px 14px; text-align: left; white-space: nowrap; }
td { padding: 11px 14px; font-size: 0.82rem; border-bottom: 1px solid var(–surface2); vertical-align: middle; }
tr:last-child td { border-bottom: none; }

/* COLOR-CODED ROWS */
tr.row-overdue td { background: var(–row-overdue) !important; }
tr.row-urgent td { background: var(–row-urgent); }
tr.row-onhold td { background: var(–row-onhold); }
tr.row-done td { background: var(–row-done); opacity: 0.55; }
tr.row-done .order-id { text-decoration: line-through; }
tr:hover td { background: var(–surface2) !important; }
tr.row-overdue td:first-child { border-left: 3px solid var(–accent); }
tr.row-urgent td:first-child { border-left: 3px solid var(–urgent); }
tr.row-onhold td:first-child { border-left: 3px solid var(–onhold); }
tr.row-done td:first-child { border-left: 3px solid var(–green); }

.order-id { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.75rem; font-weight: 500; color: var(–accent2); }

.priority-badge { display: inline-block; padding: 3px 10px; border-radius: 3px; font-family: ‘IBM Plex Mono’, monospace; font-size: 0.6rem; font-weight: 500; letter-spacing: 0.5px; text-transform: uppercase; }
.pri-Urgent   { background: rgba(200,65,10,0.12); color: var(–urgent); border: 1px solid rgba(200,65,10,0.25); }
.pri-Normal   { background: rgba(183,121,31,0.12); color: var(–normal); border: 1px solid rgba(183,121,31,0.25); }
.pri-On-Hold  { background: rgba(200,114,10,0.12); color: var(–onhold); border: 1px solid rgba(200,114,10,0.25); }

.status-badge { display: inline-block; padding: 3px 10px; border-radius: 3px; font-family: ‘IBM Plex Mono’, monospace; font-size: 0.6rem; letter-spacing: 0.5px; text-transform: uppercase; }
.st-Pending     { background: rgba(183,121,31,0.12); color: var(–yellow); }
.st-In-Progress { background: rgba(43,108,176,0.12); color: var(–accent2); }
.st-Completed   { background: rgba(39,103,73,0.12); color: var(–green); }
.st-On-Hold     { background: rgba(200,114,10,0.12); color: var(–onhold); }

.date-cell { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.72rem; white-space: nowrap; line-height: 1.3; }
.date-cell.overdue { color: var(–accent); font-weight: 700; }
.days-overdue { display: block; font-size: 0.65rem; font-weight: 700; color: var(–urgent); margin-top: 1px; }
.days-pending { display: block; font-size: 0.65rem; color: var(–muted); margin-top: 1px; }

.assignee-tag { display: inline-flex; align-items: center; gap: 5px; font-size: 0.78rem; }
.assignee-avatar { width: 22px; height: 22px; border-radius: 50%; background: var(–accent2); color: #fff; font-size: 0.65rem; font-weight: 700; display: flex; align-items: center; justify-content: center; font-family: ‘Syne’, sans-serif; }

.action-btns { display: flex; gap: 6px; flex-wrap: nowrap; }

/* MODAL */
.modal-overlay { position: fixed; inset: 0; background: rgba(26,24,20,0.6); z-index: 100; display: none; align-items: center; justify-content: center; backdrop-filter: blur(2px); padding: 20px; }
.modal-overlay.open { display: flex; }
.modal { background: var(–surface); border-radius: 12px; padding: 28px 32px; width: 640px; max-width: 95vw; max-height: 90vh; overflow-y: auto; box-shadow: 0 20px 60px rgba(0,0,0,0.25); animation: slideUp 0.2s ease; }
@keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
.modal-title { font-family: ‘Syne’, sans-serif; font-size: 1.1rem; font-weight: 800; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 20px; padding-bottom: 12px; border-bottom: 2px solid var(–border); }
.form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
.form-group { display: flex; flex-direction: column; gap: 5px; }
.form-group.full { grid-column: 1 / -1; }
.form-label { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.62rem; letter-spacing: 1px; text-transform: uppercase; color: var(–muted); }
.form-input, .form-select, .form-textarea { padding: 8px 12px; border: 1px solid var(–border); border-radius: 6px; background: var(–bg); font-family: ‘Lato’, sans-serif; font-size: 0.82rem; color: var(–ink); outline: none; transition: border-color 0.2s; }
.form-input:focus, .form-select:focus, .form-textarea:focus { border-color: var(–accent); }
.form-textarea { resize: vertical; min-height: 64px; }
.modal-actions { display: flex; gap: 10px; justify-content: flex-end; margin-top: 20px; padding-top: 16px; border-top: 1px solid var(–border); }

/* PHOTO ATTACHMENTS */
.photo-upload-area { border: 2px dashed var(–border); border-radius: 8px; padding: 18px; text-align: center; cursor: pointer; transition: all 0.2s; background: var(–bg); }
.photo-upload-area:hover { border-color: var(–accent); background: rgba(200,65,10,0.03); }
.photo-upload-area input[type=file] { display: none; }
.photo-upload-icon { font-size: 1.6rem; margin-bottom: 4px; }
.photo-upload-text { font-size: 0.8rem; color: var(–muted); }
.photo-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(80px, 1fr)); gap: 8px; margin-top: 10px; }
.photo-thumb { position: relative; width: 100%; aspect-ratio: 1; border-radius: 6px; overflow: hidden; border: 1px solid var(–border); cursor: pointer; }
.photo-thumb img { width: 100%; height: 100%; object-fit: cover; }
.photo-remove { position: absolute; top: 2px; right: 2px; background: rgba(0,0,0,0.7); color: #fff; border: none; width: 20px; height: 20px; border-radius: 50%; cursor: pointer; font-size: 0.7rem; line-height: 1; }

/* COMMENT THREAD */
.comments-section { grid-column: 1/-1; padding-top: 12px; border-top: 1px solid var(–border); margin-top: 6px; }
.comments-title { font-family: ‘IBM Plex Mono’, monospace; font-size: 0.62rem; letter-spacing: 1px; text-transform: uppercase; color: var(–muted); margin-bottom: 8px; }
.comments-list { display: flex; flex-direction: column; gap: 8px; max-height: 200px; overflow-y: auto; padding: 4px 0; margin-bottom: 10px; }
.comment { background: var(–surface2); border-radius: 6px; padding: 8px 12px; border-left: 3px solid var(–accent2); }
.comment-meta { font-size: 0.68rem; color: var(–muted); font-family: ‘IBM Plex Mono’, monospace; margin-bottom: 4px; }
.comment-text { font-size: 0.82rem; line-height: 1.4; }
.comment-input-row { display: flex; gap: 8px; }
.comment-input { flex: 1; padding: 8px 12px; border: 1px solid var(–border); border-radius: 6px; font-family: ‘Lato’, sans-serif; font-size: 0.82rem; outline: none; }
.comment-input:focus { border-color: var(–accent); }

/* AUTOSAVE INDICATOR */
.autosave-indicator { position: fixed; bottom: 16px; right: 16px; background: var(–green); color: #fff; padding: 6px 14px; border-radius: 20px; font-size: 0.7rem; font-family: ‘IBM Plex Mono’, monospace; letter-spacing: 0.5px; opacity: 0; transition: opacity 0.3s; z-index: 50; box-shadow: 0 4px 12px rgba(39,103,73,0.3); }
.autosave-indicator.show { opacity: 1; }

/* PHOTO LIGHTBOX */
.lightbox { position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 300; display: none; align-items: center; justify-content: center; cursor: pointer; }
.lightbox.open { display: flex; }
.lightbox img { max-width: 90vw; max-height: 90vh; border-radius: 8px; }

@media print { .tabs, .toolbar, .action-btns, .modal-overlay, .header, #notification-area, .autosave-indicator, .insights-row, .daily-summary, .priority-legend { display: none !important; } body { background: #fff; } .table-wrap { box-shadow: none; border: 1px solid #ccc; } }

@keyframes fadeIn { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: translateY(0); } }
.fade-in { animation: fadeIn 0.3s ease forwards; }
</style>

</head>
<body>

<!-- HEADER -->

<div class="header">
  <div class="header-left">
    <div class="header-logo">
      <img src="data:image/jpeg;base64,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" alt="BVGlazing Systems" width="52" height="52">
    </div>
    <div class="header-brand">
      <div class="brand-name">BVGlazing Systems</div>
      <div class="brand-title">Order Tracker</div>
      <div class="header-sub">131 Caldari Road, Concord, ON &nbsp;·&nbsp; Back Orders & Service Orders</div>
    </div>
  </div>
  <div class="header-right">
    <div id="hclock">--:--</div>
    <div id="hdate">---</div>
  </div>
</div>

<!-- TABS -->

<div class="tabs">
  <div class="tab active" onclick="switchTab('backorders')">Back Orders</div>
  <div class="tab" onclick="switchTab('serviceorders')">Service Orders</div>
</div>

<div class="content">

  <!-- DAILY MORNING SUMMARY -->

  <div class="daily-summary" id="daily-summary"></div>

  <!-- KPI ROW -->

  <div class="kpi-row" id="kpi-row"></div>

  <!-- INSIGHTS: WEEKLY CHART + RECURRING PROBLEMS -->

  <div class="insights-row">
    <div class="insight-card">
      <div class="insight-title"><span class="icon">📊</span> Weekly Trend — Completed Orders (Last 7 Days)</div>
      <div class="chart-container" id="weekly-chart"></div>
    </div>
    <div class="insight-card">
      <div class="insight-title"><span class="icon">🔁</span> Recurring Problems</div>
      <div class="recurring-list" id="recurring-list"></div>
    </div>
  </div>

  <!-- PRIORITY LEGEND -->

  <div class="priority-legend">
    <div class="legend-title">Priority:</div>
    <div class="leg-item"><div class="leg-dot" style="background:var(--urgent)"></div><span style="color:var(--urgent)">Urgent</span></div>
    <div class="leg-item"><div class="leg-dot" style="background:var(--normal)"></div><span style="color:var(--normal)">Normal</span></div>
    <div class="leg-item"><div class="leg-dot" style="background:var(--onhold)"></div><span style="color:var(--onhold)">On Hold</span></div>
    <span style="color:var(--muted);font-size:0.7rem;margin-left:auto;font-family:'IBM Plex Mono',monospace">Rows are color-coded: red = overdue, orange = on hold, green = done</span>
  </div>

  <!-- OVERDUE BANNER -->

  <div class="overdue-banner" id="overdue-banner" style="display:none">⚠ <span id="overdue-text"></span></div>

  <!-- TOOLBAR -->

  <div class="toolbar">
    <input class="search-box" type="text" id="search-input" placeholder="🔍  Search by job code, project, work order, assignee…" oninput="renderTable()">
    <select class="filter-sel" id="filter-priority" onchange="renderTable()">
      <option value="">All Priorities</option><option>Urgent</option><option>Normal</option><option>On Hold</option>
    </select>
    <select class="filter-sel" id="filter-status" onchange="renderTable()">
      <option value="">All Statuses</option><option>Pending</option><option>In-Progress</option><option>Completed</option><option>On-Hold</option>
    </select>
    <button class="btn btn-primary" onclick="openModal()">+ New Order</button>
    <button class="btn btn-outline" onclick="exportCSV()">⬇ Export</button>
    <button class="btn btn-outline" onclick="window.print()">🖨 Print</button>
    <button class="btn btn-outline" onclick="confirmReset()" style="color:var(--urgent);border-color:var(--urgent)">⟲ Reset</button>
  </div>

  <!-- TABLE -->

  <div class="table-wrap"><table><thead id="table-head"></thead><tbody id="table-body"></tbody></table></div>
</div>

<!-- MODAL -->

<div class="modal-overlay" id="modal-overlay" onclick="closeModalOutside(event)">
  <div class="modal" onclick="event.stopPropagation()">
    <div class="modal-title" id="modal-title">New Back Order</div>
    <div class="form-grid" id="modal-form"></div>
    <div class="modal-actions">
      <button class="btn btn-outline" onclick="closeModal()">Cancel</button>
      <button class="btn btn-primary" onclick="saveOrder()">Save Order</button>
    </div>
  </div>
</div>

<!-- NOTIFICATIONS -->

<div id="notification-area"></div>
<div class="autosave-indicator" id="autosave-ind">✓ Saved</div>
<div class="lightbox" id="lightbox" onclick="closeLightbox()"><img id="lightbox-img" src="" alt=""></div>

<script>
// ─── CONFIG ─────────────────────────────────────────────
const PRODUCTS    = ['Window Frames','Sliding Doors','Lift-&-Slide Doors','Door Sashes','Screens'];
const PRIORITIES  = ['Urgent','Normal','On Hold'];
const STATUSES    = ['Pending','In-Progress','Completed','On-Hold'];
const ISSUE_TYPES = ['Installation Issue','Damaged on Delivery','Missing Parts','Warranty Repair','Measurement Error','Hardware Defect'];
const TEAM        = ['Abhay','Firas','Derek M.','James T.','Carlos R.','Sara K.','Mike P.'];
const PRIORITY_WEIGHT = { Urgent:0, Normal:1, 'On Hold':2 };
const STORAGE_KEY = 'bvg_order_tracker_v3';

let activeTab = 'backorders';
let editingId = null;
let currentPhotos = [];
let currentComments = [];
let notifiedOrders = new Set();

// ─── DEFAULT SEED DATA ──────────────────────────────────
const DEFAULT_BO = [
  { id:'BO-1041', jobCode:'1074', workOrder:'WO-4421', projectName:'Maple Ridge Phase 2',  floor:'3rd Floor', windowDoorNum:'W-14, W-15',   product:'Sliding Doors',      dueDate:'2026-04-28', priority:'Urgent',  status:'Pending',     assignee:'Abhay',    reason:'Glass delayed from supplier', photos:[], comments:[], createdAt:'2026-04-15' },
  { id:'BO-1042', jobCode:'1085', workOrder:'WO-4435', projectName:'Northview Tower',       floor:'7th Floor', windowDoorNum:'W-02 to W-10', product:'Window Frames',      dueDate:'2026-05-03', priority:'Normal',  status:'In-Progress', assignee:'Derek M.', reason:'Combi machine backlog',       photos:[], comments:[], createdAt:'2026-04-20' },
  { id:'BO-1043', jobCode:'1091', workOrder:'WO-4448', projectName:'Sunrise Condo Block A', floor:'1st Floor', windowDoorNum:'D-01, D-02',   product:'Lift-&-Slide Doors', dueDate:'2026-05-08', priority:'Normal',  status:'Pending',     assignee:'James T.', reason:'Awaiting hardware kit',       photos:[], comments:[], createdAt:'2026-04-22' },
  { id:'BO-1044', jobCode:'1063', workOrder:'WO-4452', projectName:'GreenBuild HQ',         floor:'Lobby',     windowDoorNum:'S-01 to S-06', product:'Screens',            dueDate:'2026-04-25', priority:'Urgent',  status:'On-Hold',     assignee:'Sara K.',  reason:'Client requested design change', photos:[], comments:[], createdAt:'2026-04-10' },
  { id:'BO-1045', jobCode:'1058', workOrder:'WO-4460', projectName:'TerraHomes Unit 5',     floor:'2nd Floor', windowDoorNum:'D-05, D-06',   product:'Door Sashes',        dueDate:'2026-05-12', priority:'Normal',  status:'Completed',   assignee:'Carlos R.',reason:'Resolved',                    photos:[], comments:[], createdAt:'2026-04-18', completedAt:'2026-04-30' },
  { id:'BO-1046', jobCode:'1102', workOrder:'WO-4471', projectName:'Lakeside Residences',   floor:'5th Floor', windowDoorNum:'W-22 to W-28', product:'Window Frames',      dueDate:'2026-05-15', priority:'On Hold', status:'In-Progress', assignee:'Abhay',    reason:'Waiting on client approval',  photos:[], comments:[], createdAt:'2026-04-25' },
  { id:'BO-1047', jobCode:'1117', workOrder:'WO-4485', projectName:'PineCrest Mall',        floor:'Ground',    windowDoorNum:'S-10 to S-18', product:'Screens',            dueDate:'2026-05-20', priority:'Normal',  status:'Pending',     assignee:'Mike P.',  reason:'Material on order',           photos:[], comments:[], createdAt:'2026-04-26' },
];
const DEFAULT_SO = [
  { id:'SO-881', jobCode:'1074', client:'Maple Ridge Homes',     issueType:'Installation Issue',  technician:'Derek M.',  priority:'Urgent',  status:'Pending',     dueDate:'2026-04-30', assignee:'Derek M.', notes:'Wrong frame size installed, needs rework', photos:[], comments:[], createdAt:'2026-04-25' },
  { id:'SO-882', jobCode:'1085', client:'Northview Construction',issueType:'Missing Parts',       technician:'James T.',  priority:'Normal',  status:'In-Progress', dueDate:'2026-05-05', assignee:'James T.', notes:'Track rollers missing from delivery',     photos:[], comments:[], createdAt:'2026-04-26' },
  { id:'SO-883', jobCode:'1091', client:'Sunrise Builds',        issueType:'Damaged on Delivery', technician:'Carlos R.', priority:'Urgent',  status:'Pending',     dueDate:'2026-04-29', assignee:'Carlos R.',notes:'Corner bead cracked on 3 frames',         photos:[], comments:[], createdAt:'2026-04-24' },
  { id:'SO-884', jobCode:'1063', client:'GreenBuild Inc.',       issueType:'Warranty Repair',     technician:'Derek M.',  priority:'Normal',  status:'Completed',   dueDate:'2026-04-22', assignee:'Derek M.', notes:'Seal replacement done',                   photos:[], comments:[], createdAt:'2026-04-15', completedAt:'2026-04-22' },
  { id:'SO-885', jobCode:'1058', client:'TerraHomes',            issueType:'Hardware Defect',     technician:'James T.',  priority:'On Hold', status:'On-Hold',     dueDate:'2026-05-10', assignee:'James T.', notes:'Waiting for replacement locking mechanism', photos:[], comments:[], createdAt:'2026-04-28' },
  { id:'SO-886', jobCode:'1117', client:'Lakeside Dev.',         issueType:'Measurement Error',   technician:'Carlos R.', priority:'Normal',  status:'Pending',     dueDate:'2026-05-07', assignee:'Carlos R.',notes:'Frame width off by 8mm, remake required', photos:[], comments:[], createdAt:'2026-04-29' },
];

let backOrders = [];
let serviceOrders = [];

// ─── PERSISTENCE (autosave) ─────────────────────────────
function loadData() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) {
      const parsed = JSON.parse(saved);
      backOrders = parsed.backOrders || [];
      serviceOrders = parsed.serviceOrders || [];
      return true;
    }
  } catch(e) { console.error('Load error:', e); }
  return false;
}
function saveData() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify({ backOrders, serviceOrders, savedAt: new Date().toISOString() }));
    showAutosave();
  } catch(e) { console.error('Save error:', e); alert('Could not save (storage may be full).'); }
}
function showAutosave() {
  const ind = document.getElementById('autosave-ind');
  ind.classList.add('show');
  clearTimeout(showAutosave._t);
  showAutosave._t = setTimeout(() => ind.classList.remove('show'), 1500);
}
function confirmReset() {
  if (confirm('Reset all data to defaults? This will erase all your custom orders.')) {
    backOrders = JSON.parse(JSON.stringify(DEFAULT_BO));
    serviceOrders = JSON.parse(JSON.stringify(DEFAULT_SO));
    saveData(); renderAll();
  }
}

// ─── CLOCK (12-hour) + DAILY SUMMARY ────────────────────
function updateClock() {
  const now = new Date();
  document.getElementById('hclock').textContent = now.toLocaleTimeString('en-US', { hour12:true, hour:'numeric', minute:'2-digit' });
  document.getElementById('hdate').textContent = now.toLocaleDateString('en-CA', { weekday:'long', year:'numeric', month:'long', day:'numeric' });
}
function getGreeting() {
  const h = new Date().getHours();
  if (h < 12) return 'Good morning';
  if (h < 17) return 'Good afternoon';
  return 'Good evening';
}
function renderDailySummary() {
  const today = new Date(); today.setHours(0,0,0,0);
  const all = [...backOrders, ...serviceOrders];
  const overdue = all.filter(o => o.status !== 'Completed' && new Date(o.dueDate) < today).length;
  const dueToday = all.filter(o => o.status !== 'Completed' && datesEqual(new Date(o.dueDate), today)).length;
  const urgent = all.filter(o => o.status !== 'Completed' && o.priority === 'Urgent').length;
  const yesterday = new Date(today); yesterday.setDate(yesterday.getDate() - 1);
  const completedYesterday = all.filter(o => o.completedAt && datesEqual(new Date(o.completedAt), yesterday)).length;

  let msg;
  if (overdue > 0) msg = `You have <strong>${overdue} overdue</strong> order${overdue>1?'s':''} that need immediate follow-up.`;
  else if (urgent > 0) msg = `<strong>${urgent} urgent</strong> order${urgent>1?'s':''} ${urgent>1?'are':'is'} open today.`;
  else if (dueToday > 0) msg = `<strong>${dueToday}</strong> order${dueToday>1?'s are':' is'} due today.`;
  else msg = 'You\'re all caught up — great work staying on top of things!';

  document.getElementById('daily-summary').innerHTML = `
    <div class="summary-greeting">
      <h2>${getGreeting()}, Abhay 👋</h2>
      <p>${msg}</p>
    </div>
    <div class="summary-stats">
      <div class="summary-stat"><div class="summary-num">${overdue}</div><div class="summary-label">Overdue</div></div>
      <div class="summary-stat"><div class="summary-num">${dueToday}</div><div class="summary-label">Due Today</div></div>
      <div class="summary-stat"><div class="summary-num">${urgent}</div><div class="summary-label">Urgent Open</div></div>
      <div class="summary-stat"><div class="summary-num">${completedYesterday}</div><div class="summary-label">Done Yesterday</div></div>
    </div>
  `;
}

// ─── WEEKLY CHART ───────────────────────────────────────
function renderWeeklyChart() {
  const today = new Date(); today.setHours(0,0,0,0);
  const days = [];
  for (let i = 6; i >= 0; i--) {
    const d = new Date(today); d.setDate(d.getDate() - i);
    const all = [...backOrders, ...serviceOrders];
    const count = all.filter(o => o.completedAt && datesEqual(new Date(o.completedAt), d)).length;
    days.push({ date: d, count, label: d.toLocaleDateString('en-US', { weekday:'short' }) });
  }
  const max = Math.max(...days.map(d => d.count), 1);
  const html = days.map(d => `
    <div class="bar-group">
      <div class="bar" style="height:${(d.count/max)*100}%">
        <div class="bar-tooltip">${d.count} done · ${d.date.toLocaleDateString('en-US',{month:'short',day:'numeric'})}</div>
      </div>
      <div class="bar-label">${d.label}</div>
    </div>
  `).join('');
  document.getElementById('weekly-chart').innerHTML = html;
}

// ─── RECURRING PROBLEMS ─────────────────────────────────
function renderRecurringProblems() {
  // Combine projects across both lists, count back orders + service issues
  const counts = {};
  [...backOrders, ...serviceOrders].forEach(o => {
    const key = o.projectName || o.client;
    if (!key) return;
    if (!counts[key]) counts[key] = { count:0, urgent:0 };
    counts[key].count++;
    if (o.priority === 'Urgent') counts[key].urgent++;
  });
  const sorted = Object.entries(counts).sort((a,b) => b[1].count - a[1].count).slice(0, 5);
  if (!sorted.length) {
    document.getElementById('recurring-list').innerHTML = '<div style="color:var(--muted);font-size:0.82rem;font-style:italic;padding:10px">No recurring patterns yet.</div>';
    return;
  }
  document.getElementById('recurring-list').innerHTML = sorted.map(([name, d]) => `
    <div class="recurring-item">
      <div>
        <div class="recurring-name">${name}</div>
        <div class="recurring-meta">${d.urgent} urgent · ${d.count} total issue${d.count>1?'s':''}</div>
      </div>
      <div class="recurring-count">${d.count}</div>
    </div>
  `).join('');
}

// ─── NOTIFICATIONS ──────────────────────────────────────
function checkOverdueNotifications() {
  const today = new Date(); today.setHours(0,0,0,0);
  const all = [...backOrders, ...serviceOrders];
  all.forEach(o => {
    if (o.status === 'Completed') return;
    const due = new Date(o.dueDate);
    if (due < today && !notifiedOrders.has(o.id)) {
      const days = Math.floor((today - due) / (1000*60*60*24));
      pushNotification('⚠️ Overdue Order', `${o.id} — ${o.projectName || o.client} is ${days} day${days>1?'s':''} overdue`, 'warning');
      notifiedOrders.add(o.id);
    }
  });
}
function pushNotification(title, msg, type='') {
  const n = document.createElement('div');
  n.className = `notification ${type}`;
  n.innerHTML = `<div class="notif-title">${title}</div><div class="notif-msg">${msg}</div>`;
  n.onclick = () => n.remove();
  document.getElementById('notification-area').appendChild(n);
  setTimeout(() => n.remove(), 8000);
}

// ─── TAB SWITCH ─────────────────────────────────────────
function switchTab(tab) {
  activeTab = tab;
  document.querySelectorAll('.tab').forEach((t, i) => t.classList.toggle('active', (i===0 && tab==='backorders') || (i===1 && tab==='serviceorders')));
  document.getElementById('search-input').value = '';
  document.getElementById('filter-priority').value = '';
  document.getElementById('filter-status').value = '';
  renderAll();
}

// ─── KPIs ───────────────────────────────────────────────
function renderKPIs() {
  const orders = activeTab === 'backorders' ? backOrders : serviceOrders;
  const today = new Date(); today.setHours(0,0,0,0);
  const overdue = orders.filter(o => o.status !== 'Completed' && new Date(o.dueDate) < today).length;
  const urgent  = orders.filter(o => o.priority === 'Urgent' && o.status !== 'Completed').length;
  const onhold  = orders.filter(o => o.priority === 'On Hold').length;
  const active  = orders.filter(o => o.status === 'Pending' || o.status === 'In-Progress').length;
  const done    = orders.filter(o => o.status === 'Completed').length;

  document.getElementById('kpi-row').innerHTML = `
    <div class="kpi-card red-top"><div class="kpi-label">Overdue</div><div class="kpi-val" style="color:var(--urgent)">${overdue}</div><div class="kpi-sub">past due date</div></div>
    <div class="kpi-card red-top"><div class="kpi-label">Urgent</div><div class="kpi-val">${urgent}</div><div class="kpi-sub">open urgent</div></div>
    <div class="kpi-card blue-top"><div class="kpi-label">Active</div><div class="kpi-val">${active}</div><div class="kpi-sub">in progress</div></div>
    <div class="kpi-card gray-top"><div class="kpi-label">On Hold</div><div class="kpi-val" style="color:var(--onhold)">${onhold}</div><div class="kpi-sub">paused</div></div>
    <div class="kpi-card green-top"><div class="kpi-label">Completed</div><div class="kpi-val" style="color:var(--green)">${done}</div><div class="kpi-sub">total done</div></div>
  `;

  const banner = document.getElementById('overdue-banner');
  if (overdue > 0) {
    const ids = orders.filter(o => o.status !== 'Completed' && new Date(o.dueDate) < today).map(o => o.id).join(', ');
    banner.style.display = 'flex';
    document.getElementById('overdue-text').textContent = `${overdue} order(s) OVERDUE — ${ids}. Immediate follow-up required.`;
  } else banner.style.display = 'none';
}

// ─── TABLE ──────────────────────────────────────────────
function renderTable() {
  const search  = document.getElementById('search-input').value.toLowerCase();
  const filterP = document.getElementById('filter-priority').value;
  const filterS = document.getElementById('filter-status').value;
  const today = new Date(); today.setHours(0,0,0,0);

  let orders = activeTab === 'backorders' ? [...backOrders] : [...serviceOrders];
  if (search)  orders = orders.filter(o => JSON.stringify(o).toLowerCase().includes(search));
  if (filterP) orders = orders.filter(o => o.priority === filterP);
  if (filterS) orders = orders.filter(o => o.status === filterS);

  orders.sort((a, b) => {
    const aOver = a.status !== 'Completed' && new Date(a.dueDate) < today;
    const bOver = b.status !== 'Completed' && new Date(b.dueDate) < today;
    if (aOver !== bOver) return aOver ? -1 : 1;
    const aDone = a.status === 'Completed', bDone = b.status === 'Completed';
    if (aDone !== bDone) return aDone ? 1 : -1;
    if ((PRIORITY_WEIGHT[a.priority]||1) !== (PRIORITY_WEIGHT[b.priority]||1)) return (PRIORITY_WEIGHT[a.priority]||1) - (PRIORITY_WEIGHT[b.priority]||1);
    return new Date(a.dueDate) - new Date(b.dueDate);
  });

  activeTab === 'backorders' ? renderBOTable(orders, today) : renderSOTable(orders, today);
}

function getRowClass(o, today) {
  if (o.status === 'Completed') return 'row-done';
  if (new Date(o.dueDate) < today) return 'row-overdue';
  if (o.priority === 'Urgent') return 'row-urgent';
  if (o.priority === 'On Hold') return 'row-onhold';
  return '';
}
function getDueDateCell(o, today) {
  const due = new Date(o.dueDate);
  const isOverdue = o.status !== 'Completed' && due < today;
  const days = Math.floor((today - due) / (1000*60*60*24));
  let extra = '';
  if (isOverdue) extra = `<span class="days-overdue">${days} day${days!==1?'s':''} overdue</span>`;
  else if (o.status !== 'Completed') {
    const daysLeft = Math.floor((due - today) / (1000*60*60*24));
    if (daysLeft <= 7) extra = `<span class="days-pending">in ${daysLeft} day${daysLeft!==1?'s':''}</span>`;
  }
  return `<span class="date-cell ${isOverdue?'overdue':''}">${fmt(o.dueDate)}${isOverdue?' ⚠':''}${extra}</span>`;
}
function getAssigneeCell(name) {
  if (!name) return '<span style="color:var(--muted);font-size:0.75rem">—</span>';
  const initials = name.split(' ').map(p => p[0]).join('').slice(0,2).toUpperCase();
  return `<span class="assignee-tag"><span class="assignee-avatar">${initials}</span>${name}</span>`;
}
function commentBadge(o) {
  const c = (o.comments||[]).length, p = (o.photos||[]).length;
  let parts = [];
  if (c) parts.push(`💬 ${c}`);
  if (p) parts.push(`📎 ${p}`);
  if (!parts.length) return '';
  return `<span style="font-size:0.68rem;color:var(--muted);font-family:'IBM Plex Mono',monospace;margin-left:6px">${parts.join(' ')}</span>`;
}

function renderBOTable(orders, today) {
  document.getElementById('table-head').innerHTML = `<tr>
    <th>Job Code</th><th>Work Order</th><th>Project Name</th><th>Floor</th>
    <th>Win/Door #</th><th>Due Date</th><th>Priority</th><th>Status</th><th>Assigned</th><th>Reason</th><th>Actions</th>
  </tr>`;
  const tbody = document.getElementById('table-body');
  tbody.innerHTML = '';
  if (!orders.length) { tbody.innerHTML = emptyRow(11); return; }
  orders.forEach(o => {
    const tr = document.createElement('tr');
    tr.className = `${getRowClass(o, today)} fade-in`;
    tr.innerHTML = `
      <td><span class="order-id">${o.jobCode}</span>${commentBadge(o)}</td>
      <td><span class="order-id">${o.workOrder}</span></td>
      <td><strong>${o.projectName}</strong></td>
      <td>${o.floor}</td>
      <td style="font-family:'IBM Plex Mono',monospace;font-size:0.72rem">${o.windowDoorNum}</td>
      <td>${getDueDateCell(o, today)}</td>
      <td><span class="priority-badge pri-${o.priority.replace(' ','-')}">${o.priority}</span></td>
      <td><span class="status-badge st-${o.status.replace(' ','-')}">${o.status}</span></td>
      <td>${getAssigneeCell(o.assignee)}</td>
      <td style="max-width:160px;font-size:0.78rem;color:var(--muted)">${o.reason}</td>
      <td><div class="action-btns">
        ${o.status !== 'Completed' ? `<button class="btn btn-outline btn-sm" onclick="markComplete('${o.id}')" title="Mark complete">✓</button>` : ''}
        <button class="btn btn-outline btn-sm" onclick="editOrder('${o.id}')">Edit</button>
        <button class="btn btn-outline btn-sm" style="color:var(--urgent);border-color:var(--urgent)" onclick="deleteOrder('${o.id}')">✕</button>
      </div></td>`;
    tbody.appendChild(tr);
  });
}

function renderSOTable(orders, today) {
  document.getElementById('table-head').innerHTML = `<tr>
    <th>SO #</th><th>Job Code</th><th>Client</th><th>Issue</th><th>Technician</th>
    <th>Due Date</th><th>Priority</th><th>Status</th><th>Assigned</th><th>Notes</th><th>Actions</th>
  </tr>`;
  const tbody = document.getElementById('table-body');
  tbody.innerHTML = '';
  if (!orders.length) { tbody.innerHTML = emptyRow(11); return; }
  orders.forEach(o => {
    const tr = document.createElement('tr');
    tr.className = `${getRowClass(o, today)} fade-in`;
    tr.innerHTML = `
      <td><span class="order-id">${o.id}</span>${commentBadge(o)}</td>
      <td><span class="order-id">${o.jobCode||''}</span></td>
      <td><strong>${o.client}</strong></td>
      <td>${o.issueType}</td>
      <td>${o.technician}</td>
      <td>${getDueDateCell(o, today)}</td>
      <td><span class="priority-badge pri-${o.priority.replace(' ','-')}">${o.priority}</span></td>
      <td><span class="status-badge st-${o.status.replace(' ','-')}">${o.status}</span></td>
      <td>${getAssigneeCell(o.assignee)}</td>
      <td style="max-width:160px;font-size:0.78rem;color:var(--muted)">${o.notes}</td>
      <td><div class="action-btns">
        ${o.status !== 'Completed' ? `<button class="btn btn-outline btn-sm" onclick="markComplete('${o.id}')">✓</button>` : ''}
        <button class="btn btn-outline btn-sm" onclick="editOrder('${o.id}')">Edit</button>
        <button class="btn btn-outline btn-sm" style="color:var(--urgent);border-color:var(--urgent)" onclick="deleteOrder('${o.id}')">✕</button>
      </div></td>`;
    tbody.appendChild(tr);
  });
}

// ─── MODAL ──────────────────────────────────────────────
function openModal(prefill = null) {
  editingId = prefill ? prefill.id : null;
  currentPhotos = prefill ? [...(prefill.photos || [])] : [];
  currentComments = prefill ? [...(prefill.comments || [])] : [];

  document.getElementById('modal-title').textContent = editingId
    ? (activeTab === 'backorders' ? 'Edit Back Order' : 'Edit Service Order')
    : (activeTab === 'backorders' ? 'New Back Order' : 'New Service Order');

  const f = document.getElementById('modal-form');
  const priOpts = PRIORITIES.map(p => `<option ${prefill?.priority===p?'selected':''}>${p}</option>`).join('');
  const staOpts = STATUSES.map(s => `<option ${prefill?.status===s?'selected':''}>${s}</option>`).join('');
  const teamOpts = ['<option value="">— Unassigned —</option>', ...TEAM.map(t => `<option ${prefill?.assignee===t?'selected':''}>${t}</option>`)].join('');

  if (activeTab === 'backorders') {
    f.innerHTML = `
      <input type="hidden" id="f-id" value="${prefill?.id || genId('BO')}">
      <div class="form-group"><label class="form-label">Job Code</label>
        <input class="form-input" id="f-jobCode" placeholder="e.g. 1074" value="${prefill?.jobCode||''}"></div>
      <div class="form-group"><label class="form-label">Work Order #</label>
        <input class="form-input" id="f-workOrder" placeholder="e.g. WO-4421" value="${prefill?.workOrder||''}"></div>
      <div class="form-group full"><label class="form-label">Project Name</label>
        <input class="form-input" id="f-projectName" placeholder="e.g. Maple Ridge Phase 2" value="${prefill?.projectName||''}"></div>
      <div class="form-group"><label class="form-label">Floor</label>
        <input class="form-input" id="f-floor" placeholder="e.g. 3rd Floor" value="${prefill?.floor||''}"></div>
      <div class="form-group"><label class="form-label">Window / Door #</label>
        <input class="form-input" id="f-windowDoorNum" placeholder="e.g. W-14, D-01 to D-03" value="${prefill?.windowDoorNum||''}"></div>
      <div class="form-group"><label class="form-label">Product Type</label>
        <select class="form-select" id="f-product">${PRODUCTS.map(p=>`<option ${prefill?.product===p?'selected':''}>${p}</option>`).join('')}</select></div>
      <div class="form-group"><label class="form-label">Due Date</label>
        <input class="form-input" id="f-dueDate" type="date" value="${prefill?.dueDate||''}"></div>
      <div class="form-group"><label class="form-label">Priority</label>
        <select class="form-select" id="f-priority">${priOpts}</select></div>
      <div class="form-group"><label class="form-label">Status</label>
        <select class="form-select" id="f-status">${staOpts}</select></div>
      <div class="form-group full"><label class="form-label">Assigned To</label>
        <select class="form-select" id="f-assignee">${teamOpts}</select></div>
      <div class="form-group full"><label class="form-label">Reason for Back Order</label>
        <textarea class="form-textarea" id="f-reason" placeholder="Describe…">${prefill?.reason||''}</textarea></div>

```
  ${photoSection()}
  ${commentSection()}
`;
```

} else {
f.innerHTML = ` <input type="hidden" id="f-id" value="${prefill?.id || genId('SO')}"> <div class="form-group"><label class="form-label">Job Code</label> <input class="form-input" id="f-jobCode" placeholder="e.g. 1074" value="${prefill?.jobCode||''}"></div> <div class="form-group"><label class="form-label">Client Name</label> <input class="form-input" id="f-client" placeholder="e.g. Sunrise Builds" value="${prefill?.client||''}"></div> <div class="form-group"><label class="form-label">Issue Type</label> <select class="form-select" id="f-issueType">${ISSUE_TYPES.map(t=>`<option ${prefill?.issueType===t?‘selected’:’’}>${t}</option>`).join('')}</select></div> <div class="form-group"><label class="form-label">Technician</label> <input class="form-input" id="f-technician" placeholder="e.g. Derek M." value="${prefill?.technician||''}"></div> <div class="form-group"><label class="form-label">Due Date</label> <input class="form-input" id="f-dueDate" type="date" value="${prefill?.dueDate||''}"></div> <div class="form-group"><label class="form-label">Priority</label> <select class="form-select" id="f-priority">${priOpts}</select></div> <div class="form-group"><label class="form-label">Status</label> <select class="form-select" id="f-status">${staOpts}</select></div> <div class="form-group"><label class="form-label">Assigned To</label> <select class="form-select" id="f-assignee">${teamOpts}</select></div> <div class="form-group full"><label class="form-label">Notes</label> <textarea class="form-textarea" id="f-notes" placeholder="Describe…">${prefill?.notes||''}</textarea></div> ${photoSection()} ${commentSection()} `;
}
renderPhotos();
renderComments();
document.getElementById(‘modal-overlay’).classList.add(‘open’);
}

function photoSection() {
return `<div class="form-group full"> <label class="form-label">Photos</label> <label class="photo-upload-area"> <div class="photo-upload-icon">📷</div> <div class="photo-upload-text">Click to add photos (or drag & drop)</div> <input type="file" accept="image/*" multiple onchange="handlePhotoUpload(event)"> </label> <div class="photo-grid" id="photo-grid"></div> </div>`;
}
function commentSection() {
return `<div class="comments-section"> <div class="comments-title">💬 Comment Thread</div> <div class="comments-list" id="comments-list"></div> <div class="comment-input-row"> <input class="comment-input" id="comment-input" placeholder="Add a comment…" onkeydown="if(event.key==='Enter')addComment()"> <button class="btn btn-primary btn-sm" onclick="addComment()">Add</button> </div> </div>`;
}

function handlePhotoUpload(e) {
const files = Array.from(e.target.files);
files.forEach(file => {
if (currentPhotos.length >= 6) { alert(‘Max 6 photos per order.’); return; }
const reader = new FileReader();
reader.onload = (evt) => {
currentPhotos.push({ data: evt.target.result, name: file.name });
renderPhotos();
};
reader.readAsDataURL(file);
});
}
function renderPhotos() {
const g = document.getElementById(‘photo-grid’);
if (!g) return;
g.innerHTML = currentPhotos.map((p, i) => `<div class="photo-thumb" onclick="openLightbox('${p.data}')"> <img src="${p.data}" alt="${p.name}"> <button class="photo-remove" onclick="event.stopPropagation();removePhoto(${i})">✕</button> </div>`).join(’’);
}
function removePhoto(i) { currentPhotos.splice(i, 1); renderPhotos(); }
function openLightbox(src) { document.getElementById(‘lightbox-img’).src = src; document.getElementById(‘lightbox’).classList.add(‘open’); }
function closeLightbox() { document.getElementById(‘lightbox’).classList.remove(‘open’); }

function addComment() {
const input = document.getElementById(‘comment-input’);
const text = input.value.trim();
if (!text) return;
currentComments.push({ text, author: ‘Abhay’, at: new Date().toISOString() });
input.value = ‘’;
renderComments();
}
function renderComments() {
const list = document.getElementById(‘comments-list’);
if (!list) return;
if (!currentComments.length) { list.innerHTML = ‘<div style="color:var(--muted);font-size:0.78rem;font-style:italic;padding:6px 0">No comments yet.</div>’; return; }
list.innerHTML = currentComments.map(c => `<div class="comment"> <div class="comment-meta">${c.author} · ${new Date(c.at).toLocaleString('en-US', {month:'short',day:'numeric',hour:'numeric',minute:'2-digit',hour12:true})}</div> <div class="comment-text">${escapeHtml(c.text)}</div> </div>`).join(’’);
list.scrollTop = list.scrollHeight;
}
function escapeHtml(s) { return s.replace(/[&<>”’]/g, c => ({’&’:’&’,’<’:’<’,’>’:’>’,’”’:’"’,”’”:’'’}[c])); }

function closeModal() { document.getElementById(‘modal-overlay’).classList.remove(‘open’); editingId = null; currentPhotos = []; currentComments = []; }
function closeModalOutside(e) { if (e.target === document.getElementById(‘modal-overlay’)) closeModal(); }

function saveOrder() {
const g = id => document.getElementById(id)?.value?.trim() || ‘’;
const wasCompleted = editingId ? (activeTab===‘backorders’?backOrders:serviceOrders).find(o=>o.id===editingId)?.status === ‘Completed’ : false;

if (activeTab === ‘backorders’) {
const obj = { id:g(‘f-id’), jobCode:g(‘f-jobCode’), workOrder:g(‘f-workOrder’), projectName:g(‘f-projectName’), floor:g(‘f-floor’), windowDoorNum:g(‘f-windowDoorNum’), product:g(‘f-product’), dueDate:g(‘f-dueDate’), priority:g(‘f-priority’), status:g(‘f-status’), assignee:g(‘f-assignee’), reason:g(‘f-reason’), photos: currentPhotos, comments: currentComments };
if (!obj.projectName || !obj.dueDate) { alert(‘Please fill in Project Name and Due Date.’); return; }
if (obj.status === ‘Completed’ && !wasCompleted) obj.completedAt = new Date().toISOString();
if (editingId) { const i = backOrders.findIndex(o=>o.id===editingId); if(i>=0) { obj.createdAt = backOrders[i].createdAt; if(wasCompleted) obj.completedAt = backOrders[i].completedAt; backOrders[i]=obj; } }
else { obj.createdAt = new Date().toISOString().slice(0,10); backOrders.unshift(obj); }
} else {
const obj = { id:g(‘f-id’), jobCode:g(‘f-jobCode’), client:g(‘f-client’), issueType:g(‘f-issueType’), technician:g(‘f-technician’), dueDate:g(‘f-dueDate’), priority:g(‘f-priority’), status:g(‘f-status’), assignee:g(‘f-assignee’), notes:g(‘f-notes’), photos: currentPhotos, comments: currentComments };
if (!obj.client || !obj.dueDate) { alert(‘Please fill in Client Name and Due Date.’); return; }
if (obj.status === ‘Completed’ && !wasCompleted) obj.completedAt = new Date().toISOString();
if (editingId) { const i = serviceOrders.findIndex(o=>o.id===editingId); if(i>=0) { obj.createdAt = serviceOrders[i].createdAt; if(wasCompleted) obj.completedAt = serviceOrders[i].completedAt; serviceOrders[i]=obj; } }
else { obj.createdAt = new Date().toISOString().slice(0,10); serviceOrders.unshift(obj); }
}
closeModal(); saveData(); renderAll();
pushNotification(‘✓ Order Saved’, `Order saved successfully.`, ‘success’);
}

function editOrder(id) {
const o = activeTab===‘backorders’ ? backOrders.find(x=>x.id===id) : serviceOrders.find(x=>x.id===id);
if (o) openModal(o);
}
function markComplete(id) {
const list = activeTab===‘backorders’ ? backOrders : serviceOrders;
const o = list.find(x=>x.id===id);
if (o) { o.status = ‘Completed’; o.completedAt = new Date().toISOString(); saveData(); renderAll(); pushNotification(‘✓ Marked Complete’, `${id} done!`, ‘success’); }
}
function deleteOrder(id) {
if (!confirm(`Delete order ${id}?`)) return;
if (activeTab===‘backorders’) backOrders = backOrders.filter(o=>o.id!==id);
else serviceOrders = serviceOrders.filter(o=>o.id!==id);
saveData(); renderAll();
}

function exportCSV() {
const orders = activeTab===‘backorders’ ? backOrders : serviceOrders;
if (!orders.length) return;
const exportable = orders.map(({photos, comments, …rest}) => ({…rest, photoCount: (photos||[]).length, commentCount: (comments||[]).length }));
const keys = Object.keys(exportable[0]);
const csv = [keys.join(’,’), …exportable.map(o => keys.map(k=>`"${(o[k]||'').toString().replace(/"/g,'""')}"`).join(’,’))].join(’\n’);
const a = document.createElement(‘a’);
a.href = ‘data:text/csv;charset=utf-8,’ + encodeURIComponent(csv);
a.download = `${activeTab}-${new Date().toISOString().slice(0,10)}.csv`;
a.click();
}

function fmt(d) { if (!d) return ‘—’; const [y,m,day] = d.split(’-’); return `${day}/${m}/${y}`; }
function emptyRow(cols) { return `<tr><td colspan="${cols}" style="text-align:center;color:var(--muted);padding:32px;font-style:italic">No orders match your filters.</td></tr>`; }
function genId(prefix) { const list = prefix===‘BO’ ? backOrders : serviceOrders; const nums = list.map(o => parseInt(o.id.split(’-’)[1])).filter(n => !isNaN(n)); return prefix + ‘-’ + (Math.max(…nums, 1000) + 1); }
function datesEqual(a, b) { return a.getFullYear()===b.getFullYear() && a.getMonth()===b.getMonth() && a.getDate()===b.getDate(); }

function renderAll() {
renderDailySummary();
renderKPIs();
renderTable();
renderWeeklyChart();
renderRecurringProblems();
}

// ─── INIT ───────────────────────────────────────────────
if (!loadData()) {
backOrders = JSON.parse(JSON.stringify(DEFAULT_BO));
serviceOrders = JSON.parse(JSON.stringify(DEFAULT_SO));
saveData();
}
setInterval(updateClock, 1000); updateClock();
setInterval(checkOverdueNotifications, 60000);
renderAll();
setTimeout(() => { checkOverdueNotifications(); pushNotification(‘👋 Welcome back, Abhay’, ‘Your data is auto-saved to your browser.’, ‘success’); }, 800);
</script>

</body>
</html>