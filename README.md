<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>Empire Ledger</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root {
  --bg: #0a0a0f;
  --surface: #111118;
  --surface2: #18181f;
  --border: #2a2a38;
  --accent: #c8a96e;
  --accent2: #6e8ec8;
  --accent3: #6ec8a9;
  --danger: #c86e6e;
  --text: #e8e4dc;
  --muted: #6e6e82;
  --gold: #c8a96e;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: 'DM Sans', sans-serif;
  font-size: 14px;
  min-height: 100vh;
  background-image: radial-gradient(ellipse at 20% 0%, rgba(200,169,110,0.06) 0%, transparent 60%), radial-gradient(ellipse at 80% 100%, rgba(110,142,200,0.05) 0%, transparent 60%);
}
.header {
  padding: 20px 20px 0;
  display: flex;
  align-items: flex-end;
  gap: 16px;
  border-bottom: 1px solid var(--border);
  flex-wrap: wrap;
}
.logo { font-family: 'Bebas Neue', sans-serif; font-size: 30px; letter-spacing: 3px; color: var(--accent); line-height: 1; padding-bottom: 14px; }
.logo span { color: var(--muted); font-size: 11px; letter-spacing: 1px; display: block; font-family: 'DM Mono', monospace; }
nav { display: flex; gap: 0; margin-left: auto; overflow-x: auto; }
nav button { background: none; border: none; color: var(--muted); font-family: 'DM Mono', monospace; font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase; padding: 10px 14px; cursor: pointer; border-bottom: 2px solid transparent; transition: all 0.2s; white-space: nowrap; }
nav button:hover { color: var(--text); }
nav button.active { color: var(--accent); border-bottom-color: var(--accent); }
.container { padding: 20px; max-width: 1400px; }
.nw-bar { background: var(--surface); border: 1px solid var(--border); border-radius: 12px; padding: 16px 20px; display: grid; grid-template-columns: repeat(5, 1fr); gap: 0; margin-bottom: 20px; }
.nw-item { padding: 0 14px; border-right: 1px solid var(--border); }
.nw-item:first-child { padding-left: 0; }
.nw-item:last-child { border-right: none; padding-right: 0; }
.nw-label { font-family: 'DM Mono', monospace; font-size: 9px; letter-spacing: 1.5px; text-transform: uppercase; color: var(--muted); margin-bottom: 4px; }
.nw-value { font-family: 'Bebas Neue', sans-serif; font-size: 22px; letter-spacing: 1px; color: var(--accent); }
.nw-value.total { color: var(--gold); font-size: 26px; }
.nw-value.cayman { color: var(--accent3); }
.nw-value.bank { color: var(--accent2); }
.nw-sub { font-size: 10px; color: var(--muted); margin-top: 2px; }
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; }
.card { background: var(--surface); border: 1px solid var(--border); border-radius: 12px; padding: 18px 20px; }
.card-title { font-family: 'DM Mono', monospace; font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--accent); margin-bottom: 14px; display: flex; align-items: center; gap: 8px; }
.card-title .dot { width: 5px; height: 5px; border-radius: 50%; background: var(--accent); flex-shrink: 0; }
.field { margin-bottom: 12px; }
.field label { display: block; font-family: 'DM Mono', monospace; font-size: 9px; letter-spacing: 1px; text-transform: uppercase; color: var(--muted); margin-bottom: 5px; }
input, select, textarea { width: 100%; background: var(--bg); border: 1px solid var(--border); border-radius: 8px; color: var(--text); font-family: 'DM Mono', monospace; font-size: 13px; padding: 8px 11px; outline: none; transition: border-color 0.2s; }
input:focus, select:focus, textarea:focus { border-color: var(--accent); }
select option { background: var(--surface); }
textarea { resize: vertical; min-height: 55px; }
.input-row { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.btn { background: var(--accent); color: #0a0a0f; border: none; border-radius: 8px; font-family: 'DM Mono', monospace; font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase; font-weight: 500; padding: 9px 18px; cursor: pointer; transition: all 0.15s; width: 100%; margin-top: 4px; }
.btn:hover { background: #d4ba82; transform: translateY(-1px); }
.btn:active { transform: translateY(0); }
.btn.secondary { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
.btn.secondary:hover { border-color: var(--accent); color: var(--accent); background: var(--surface2); }
.btn.danger { background: var(--danger); color: white; }
.btn.green { background: var(--accent3); color: #0a0a0f; }
.btn.sm { padding: 5px 12px; font-size: 9px; margin-top: 0; width: auto; }
.page { display: none; }
.page.active { display: block; }
.table-wrap { overflow-x: auto; margin-top: 4px; }
table { width: 100%; border-collapse: collapse; font-family: 'DM Mono', monospace; font-size: 11px; }
th { text-align: left; color: var(--muted); font-size: 9px; letter-spacing: 1px; text-transform: uppercase; padding: 7px 9px; border-bottom: 1px solid var(--border); }
td { padding: 8px 9px; border-bottom: 1px solid rgba(42,42,56,0.5); vertical-align: middle; }
tr:last-child td { border-bottom: none; }
tr:hover td { background: rgba(200,169,110,0.03); }
.tag { display: inline-block; font-family: 'DM Mono', monospace; font-size: 8px; letter-spacing: 1px; text-transform: uppercase; padding: 2px 7px; border-radius: 4px; font-weight: 500; }
.tag.okay { background: rgba(110,142,200,0.15); color: var(--accent2); }
.tag.good { background: rgba(110,200,169,0.15); color: var(--accent3); }
.tag.moderate { background: rgba(200,169,110,0.15); color: var(--accent); }
.tag.amazing { background: rgba(200,110,110,0.15); color: #e09090; }
.tag.sold { background: rgba(110,200,169,0.15); color: var(--accent3); }
.tag.bought { background: rgba(200,110,110,0.15); color: var(--danger); }
.stat-row { display: flex; justify-content: space-between; align-items: center; padding: 7px 0; border-bottom: 1px solid rgba(42,42,56,0.5); font-family: 'DM Mono', monospace; font-size: 12px; }
.stat-row:last-child { border-bottom: none; }
.stat-row .key { color: var(--muted); font-size: 10px; }
.stat-row .val { color: var(--text); }
.stat-row .val.pos { color: var(--accent3); }
.stat-row .val.neg { color: var(--danger); }
.stat-row .val.gold { color: var(--accent); }
.badge { font-family: 'DM Mono', monospace; font-size: 9px; background: var(--surface2); border: 1px solid var(--border); border-radius: 4px; padding: 2px 6px; color: var(--muted); margin-left: 6px; }
.timer-display { font-family: 'Bebas Neue', sans-serif; font-size: 44px; letter-spacing: 3px; color: var(--accent); text-align: center; padding: 8px 0; }
.timer-sub { text-align: center; font-family: 'DM Mono', monospace; font-size: 10px; color: var(--muted); margin-bottom: 10px; }
.profit-pill { display: inline-block; font-family: 'DM Mono', monospace; font-size: 10px; padding: 2px 9px; border-radius: 20px; }
.profit-pill.pos { background: rgba(110,200,169,0.12); color: var(--accent3); }
.profit-pill.neg { background: rgba(200,110,110,0.12); color: var(--danger); }
.divider { height: 1px; background: var(--border); margin: 14px 0; }
.empty { text-align: center; padding: 28px 20px; color: var(--muted); font-family: 'DM Mono', monospace; font-size: 11px; letter-spacing: 1px; }
.empty .icon { font-size: 24px; margin-bottom: 6px; }
.editable { cursor: pointer; border-bottom: 1px dashed var(--border); }
.editable:hover { border-color: var(--accent); color: var(--accent); }
::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
/* AI Import */
.drop-zone { border: 2px dashed var(--border); border-radius: 12px; padding: 28px 20px; text-align: center; cursor: pointer; transition: all 0.2s; }
.drop-zone:hover, .drop-zone.drag-over { border-color: var(--accent); background: rgba(200,169,110,0.04); }
.drop-zone .dz-icon { font-size: 32px; margin-bottom: 8px; }
.drop-zone p { font-family: 'DM Mono', monospace; font-size: 11px; color: var(--muted); letter-spacing: 1px; }
.drop-zone p.hint { font-size: 10px; margin-top: 4px; }
.ai-result { background: var(--bg); border: 1px solid var(--border); border-radius: 8px; padding: 14px; margin-top: 14px; font-family: 'DM Mono', monospace; font-size: 12px; line-height: 1.6; max-height: 260px; overflow-y: auto; display: none; }
.ai-result.visible { display: block; }
.ai-loading { text-align: center; padding: 20px; font-family: 'DM Mono', monospace; font-size: 11px; color: var(--accent); letter-spacing: 1px; display: none; }
.ai-loading.visible { display: block; }
/* Setup modal */
.modal-bg { position: fixed; inset: 0; background: rgba(0,0,0,0.8); z-index: 1000; display: flex; align-items: center; justify-content: center; padding: 20px; display: none; }
.modal-bg.open { display: flex; }
.modal { background: var(--surface); border: 1px solid var(--border); border-radius: 16px; padding: 28px; max-width: 560px; width: 100%; max-height: 80vh; overflow-y: auto; }
.modal h2 { font-family: 'Bebas Neue', sans-serif; font-size: 24px; letter-spacing: 2px; color: var(--accent); margin-bottom: 16px; }
.modal p { color: var(--muted); font-size: 13px; line-height: 1.7; margin-bottom: 12px; }
.modal code { background: var(--bg); border: 1px solid var(--border); border-radius: 6px; padding: 10px 14px; display: block; font-family: 'DM Mono', monospace; font-size: 12px; color: var(--accent3); margin: 8px 0 14px; line-height: 1.8; }
.step { display: flex; gap: 12px; margin-bottom: 14px; }
.step-num { font-family: 'Bebas Neue', sans-serif; font-size: 20px; color: var(--accent); flex-shrink: 0; line-height: 1.2; }
.step-body { font-size: 13px; color: var(--text); line-height: 1.6; }
.step-body strong { color: var(--accent); }
/* Chart */
.chart-wrap { position: relative; height: 200px; }
@media (max-width: 700px) {
  .nw-bar { grid-template-columns: 1fr 1fr; gap: 10px; }
  .nw-item { border-right: none; border-bottom: 1px solid var(--border); padding: 0 0 10px; }
  .nw-item:last-child { border-bottom: none; padding-bottom: 0; }
  .grid-2, .grid-3 { grid-template-columns: 1fr; }
}
</style>
</head>
<body>

<div class="header">
  <div class="logo">Empire Ledger <span>Jaymarley8 — Torn Operations</span></div>
  <nav>
    <button class="active" onclick="showPage('dashboard',this)">Dashboard</button>
    <button onclick="showPage('travel',this)">Travel</button>
    <button onclick="showPage('trades',this)">Trades</button>
    <button onclick="showPage('calculator',this)">Daily Calc</button>
    <button onclick="showPage('ai',this)">AI Import</button>
    <button onclick="showPage('charts',this)">Charts</button>
    <button onclick="showPage('settings',this)">Settings</button>
  </nav>
</div>

<div class="container">

  <!-- NET WORTH BAR -->
  <div class="nw-bar">
    <div class="nw-item">
      <div class="nw-label">Total Net Worth</div>
      <div class="nw-value total" id="nw-total">$0</div>
      <div class="nw-sub" id="nw-change">—</div>
    </div>
    <div class="nw-item">
      <div class="nw-label">Cash on Hand</div>
      <div class="nw-value" id="nw-cash">$0</div>
      <div class="nw-sub editable" onclick="promptEdit('cash','Cash on Hand')">tap to update</div>
    </div>
    <div class="nw-item">
      <div class="nw-label">Bank</div>
      <div class="nw-value bank" id="nw-bank">$0</div>
      <div class="nw-sub" id="nw-bank-info">—</div>
    </div>
    <div class="nw-item">
      <div class="nw-label">Cayman Islands</div>
      <div class="nw-value cayman" id="nw-cayman">$0</div>
      <div class="nw-sub editable" onclick="promptEdit('cayman','Cayman Islands')">tap to update</div>
    </div>
    <div class="nw-item">
      <div class="nw-label">Inventory Value</div>
      <div class="nw-value" id="nw-inv">$0</div>
      <div class="nw-sub editable" onclick="promptEdit('inv','Inventory Value')">tap to update</div>
    </div>
  </div>

  <!-- DASHBOARD -->
  <div class="page active" id="page-dashboard">
    <div class="grid-2" style="margin-bottom:16px">
      <div class="card">
        <div class="card-title"><div class="dot"></div>Update Balances</div>
        <div class="input-row">
          <div class="field"><label>Cash on Hand</label><input type="number" id="inp-cash" placeholder="594263"></div>
          <div class="field"><label>Cayman Balance</label><input type="number" id="inp-cayman" placeholder="627992497"></div>
        </div>
        <div class="input-row">
          <div class="field"><label>Bank Balance</label><input type="number" id="inp-bank" placeholder="3050423"></div>
          <div class="field"><label>Bank Term</label>
            <select id="inp-bank-term">
              <option value="">No term</option>
              <option value="1w">1 Week</option>
              <option value="2w">2 Weeks</option>
              <option value="1m">1 Month</option>
              <option value="3m">3 Months</option>
            </select>
          </div>
        </div>
        <div class="input-row">
          <div class="field"><label>Bank Interest Rate (%)</label><input type="number" id="inp-bank-rate" placeholder="e.g. 3.5" step="0.01"></div>
          <div class="field"><label>Cayman Interest Rate (%)</label><input type="number" id="inp-cayman-rate" placeholder="e.g. 1.5" step="0.01"></div>
        </div>
        <div class="field"><label>Inventory Value (unsold items est.)</label><input type="number" id="inp-inv" placeholder="0"></div>
        <button class="btn" onclick="saveBalances()">Save Snapshot</button>
      </div>

      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent3)"></div>Today at a Glance</div>
        <div class="stat-row"><span class="key">Job income today</span><span class="val pos" id="sum-job">$0</span></div>
        <div class="stat-row"><span class="key">Travel runs</span><span class="val" id="sum-runs">0</span></div>
        <div class="stat-row"><span class="key">Travel revenue</span><span class="val pos" id="sum-travel-rev">$0</span></div>
        <div class="stat-row"><span class="key">Cost of goods</span><span class="val neg" id="sum-travel-cost">$0</span></div>
        <div class="stat-row"><span class="key">Trade revenue</span><span class="val pos" id="sum-trade-rev">$0</span></div>
        <div class="stat-row"><span class="key">Bank interest (daily est.)</span><span class="val pos" id="sum-bank-int">$0</span></div>
        <div class="stat-row"><span class="key">Cayman interest (daily est.)</span><span class="val pos" id="sum-cayman-int">$0</span></div>
        <div class="divider"></div>
        <div class="stat-row"><span class="key" style="color:var(--text);font-size:12px">NET TODAY</span><span class="val gold" id="sum-net" style="font-size:15px">$0</span></div>
        <div style="margin-top:12px">
          <label style="font-family:'DM Mono',monospace;font-size:9px;letter-spacing:1px;text-transform:uppercase;color:var(--muted);display:block;margin-bottom:5px">Log Job Pay</label>
          <div style="display:flex;gap:8px"><input type="number" id="inp-job" placeholder="500000" style="flex:1"><button class="btn sm" onclick="logJobPay()">Log</button></div>
        </div>
      </div>
    </div>

    <div class="card">
      <div class="card-title"><div class="dot" style="background:var(--accent2)"></div>Recent Activity <span class="badge" id="activity-count">0 entries</span></div>
      <div class="table-wrap">
        <table>
          <thead><tr><th>Time</th><th>Type</th><th>Description</th><th>Amount</th><th>Net</th></tr></thead>
          <tbody id="activity-tbody"><tr><td colspan="5"><div class="empty"><div class="icon">📋</div>No activity yet</div></td></tr></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- TRAVEL -->
  <div class="page" id="page-travel">
    <div class="grid-2" style="margin-bottom:16px">
      <div class="card">
        <div class="card-title"><div class="dot"></div>Log a Travel Run</div>
        <div class="field">
          <label>Location</label>
          <select id="t-location" onchange="fillTravelTime()">
            <option value="">Select...</option>
            <option value="United Kingdom" data-time="90">United Kingdom (90 min)</option>
            <option value="France" data-time="70">France (70 min)</option>
            <option value="Germany" data-time="80">Germany (80 min)</option>
            <option value="Japan" data-time="120">Japan (120 min)</option>
            <option value="Canada" data-time="50">Canada (50 min)</option>
            <option value="Mexico" data-time="45">Mexico (45 min)</option>
            <option value="Cayman Islands" data-time="25">Cayman Islands (25 min)</option>
            <option value="South Africa" data-time="100">South Africa (100 min)</option>
            <option value="China" data-time="115">China (115 min)</option>
            <option value="UAE" data-time="110">UAE (110 min)</option>
            <option value="Custom" data-time="0">Custom...</option>
          </select>
        </div>
        <div class="input-row">
          <div class="field"><label>Custom Location</label><input type="text" id="t-custom-loc" placeholder="if custom"></div>
          <div class="field"><label>Travel Time (min, one way)</label><input type="number" id="t-travel-time" placeholder="90"></div>
        </div>
        <div class="field"><label>Items Purchased</label><input type="text" id="t-items" placeholder="e.g. Flowers, Plushies"></div>
        <div class="input-row">
          <div class="field"><label>Cost of Goods ($)</label><input type="number" id="t-cost" placeholder="600000"></div>
          <div class="field"><label>Quantity</label><input type="number" id="t-qty" placeholder="0"></div>
        </div>
        <div class="input-row">
          <div class="field"><label>Est. Sale Value ($)</label><input type="number" id="t-revenue" placeholder="0"></div>
          <div class="field"><label>Resale Quality</label>
            <select id="t-quality">
              <option value="okay">Okay</option>
              <option value="good" selected>Good</option>
              <option value="moderate">Moderate</option>
              <option value="amazing">Amazing</option>
            </select>
          </div>
        </div>
        <div class="field"><label>Via Trader?</label>
          <select id="t-trader">
            <option value="yes" selected>Yes — trader (premium for safety)</option>
            <option value="no">No — direct market</option>
          </select>
        </div>
        <div class="field"><label>Notes</label><textarea id="t-notes" placeholder="Stock timing, restocks, etc."></textarea></div>
        <button class="btn" onclick="logTravel()">Log Run</button>
      </div>

      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent3)"></div>Travel Timer</div>
        <div class="timer-display" id="timer-display">00:00:00</div>
        <div class="timer-sub" id="timer-label">ready</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:14px">
          <button class="btn green" onclick="startTimer()">Start</button>
          <button class="btn secondary" onclick="stopTimer()">Stop / Reset</button>
        </div>
        <div class="divider"></div>
        <div class="card-title" style="margin-top:12px"><div class="dot"></div>Today's Travel Stats</div>
        <div class="stat-row"><span class="key">Runs today</span><span class="val" id="ts-runs">0</span></div>
        <div class="stat-row"><span class="key">Total cost of goods</span><span class="val neg" id="ts-cost">$0</span></div>
        <div class="stat-row"><span class="key">Est. revenue</span><span class="val pos" id="ts-rev">$0</span></div>
        <div class="stat-row"><span class="key">Est. profit</span><span class="val gold" id="ts-profit">$0</span></div>
        <div class="stat-row"><span class="key">Avg profit / run</span><span class="val" id="ts-avg">$0</span></div>
      </div>
    </div>

    <div class="card">
      <div class="card-title"><div class="dot" style="background:var(--accent2)"></div>Travel Log <span class="badge" id="travel-count">0 runs</span></div>
      <div class="table-wrap">
        <table>
          <thead><tr><th>Date/Time</th><th>Location</th><th>Items</th><th>Qty</th><th>Cost</th><th>Revenue</th><th>Profit</th><th>Time</th><th>Quality</th><th>Trader</th><th></th></tr></thead>
          <tbody id="travel-tbody"><tr><td colspan="11"><div class="empty"><div class="icon">✈️</div>No travel runs logged yet</div></td></tr></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- TRADES -->
  <div class="page" id="page-trades">
    <div class="grid-2" style="margin-bottom:16px">
      <div class="card">
        <div class="card-title"><div class="dot"></div>Log a Trade</div>
        <div class="input-row">
          <div class="field"><label>Trader Name</label><input type="text" id="tr-trader" placeholder="e.g. Dracula, MKBBGL"></div>
          <div class="field"><label>Trade Type</label>
            <select id="tr-type">
              <option value="sold">I Sold</option>
              <option value="bought">I Bought</option>
            </select>
          </div>
        </div>
        <div class="field"><label>Items</label><input type="text" id="tr-items" placeholder="e.g. 19x Monkey Plushie, 61x Sheep Plushie"></div>
        <div class="input-row">
          <div class="field"><label>Price Received / Paid ($)</label><input type="number" id="tr-price" placeholder="0"></div>
          <div class="field"><label>Market Value Est. ($)</label><input type="number" id="tr-market" placeholder="optional"></div>
        </div>
        <div class="field"><label>Safety Premium Paid?</label>
          <select id="tr-premium">
            <option value="yes">Yes — paid trader markup</option>
            <option value="no">No</option>
          </select>
        </div>
        <div class="field"><label>Notes</label><textarea id="tr-notes" placeholder="e.g. Regular buyer, paid fast, receipt link"></textarea></div>
        <button class="btn" onclick="logTrade()">Log Trade</button>
      </div>

      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent3)"></div>Trade Summary</div>
        <div class="stat-row"><span class="key">Total sold (all time)</span><span class="val pos" id="tr-total-sold">$0</span></div>
        <div class="stat-row"><span class="key">Total bought (all time)</span><span class="val neg" id="tr-total-bought">$0</span></div>
        <div class="stat-row"><span class="key">Unique traders</span><span class="val" id="tr-unique">0</span></div>
        <div class="stat-row"><span class="key">Total trades logged</span><span class="val" id="tr-count">0</span></div>
        <div class="divider"></div>
        <div class="card-title"><div class="dot"></div>Top Traders by Volume</div>
        <div id="top-traders"><div class="empty" style="padding:12px 0"><div class="icon" style="font-size:18px">🤝</div>No trades yet</div></div>
      </div>
    </div>

    <div class="card">
      <div class="card-title"><div class="dot" style="background:var(--accent2)"></div>Trade Log <span class="badge" id="trades-count">0 trades</span></div>
      <div class="table-wrap">
        <table>
          <thead><tr><th>Date/Time</th><th>Trader</th><th>Type</th><th>Items</th><th>Price</th><th>Market Val</th><th>Premium</th><th>Notes</th><th></th></tr></thead>
          <tbody id="trades-tbody"><tr><td colspan="9"><div class="empty"><div class="icon">📒</div>No trades logged yet</div></td></tr></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- DAILY CALCULATOR -->
  <div class="page" id="page-calculator">
    <div class="grid-2" style="margin-bottom:16px">
      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent3)"></div>Income</div>
        <div class="stat-row"><span class="key">Job Pay (today)</span><span class="val pos" id="calc-job">$0</span></div>
        <div class="stat-row"><span class="key">Travel Revenue (today's runs)</span><span class="val pos" id="calc-travel">$0</span></div>
        <div class="stat-row"><span class="key">Trade Revenue (today)</span><span class="val pos" id="calc-trades">$0</span></div>
        <div class="stat-row"><span class="key">Bank Interest (daily est.)</span><span class="val pos" id="calc-bank-int">$0</span></div>
        <div class="stat-row"><span class="key">Cayman Interest (daily est.)</span><span class="val pos" id="calc-cayman-int">$0</span></div>
        <div class="divider"></div>
        <div class="field"><label>Add Custom Income</label>
          <div class="input-row"><input type="text" id="ci-label" placeholder="Source"><input type="number" id="ci-amount" placeholder="Amount"></div>
          <button class="btn sm green" style="margin-top:7px" onclick="addCustomIncome()">+ Add Income</button>
        </div>
        <div id="custom-incomes"></div>
      </div>
      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--danger)"></div>Expenses</div>
        <div class="stat-row"><span class="key">Cost of Goods (travel today)</span><span class="val neg" id="calc-cogs">$0</span></div>
        <div class="stat-row"><span class="key">Purchases (bought trades today)</span><span class="val neg" id="calc-bought">$0</span></div>
        <div class="divider"></div>
        <div class="field"><label>Add Custom Expense</label>
          <div class="input-row"><input type="text" id="ce-label" placeholder="Expense"><input type="number" id="ce-amount" placeholder="Amount"></div>
          <button class="btn sm danger" style="margin-top:7px" onclick="addCustomExpense()">+ Add Expense</button>
        </div>
        <div id="custom-expenses"></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title"><div class="dot" style="background:var(--gold)"></div>Daily Total</div>
      <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:0">
        <div style="padding:14px 18px;border-right:1px solid var(--border)">
          <div class="nw-label">Total Income</div>
          <div class="nw-value" style="color:var(--accent3);font-size:24px" id="calc-total-inc">$0</div>
        </div>
        <div style="padding:14px 18px;border-right:1px solid var(--border)">
          <div class="nw-label">Total Expenses</div>
          <div class="nw-value" style="color:var(--danger);font-size:24px" id="calc-total-exp">$0</div>
        </div>
        <div style="padding:14px 18px">
          <div class="nw-label">Net Profit Today</div>
          <div class="nw-value gold" style="font-size:24px" id="calc-net">$0</div>
        </div>
      </div>
    </div>
  </div>

  <!-- AI IMPORT -->
  <div class="page" id="page-ai">
    <div class="grid-2" style="margin-bottom:16px">
      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent3)"></div>AI Screenshot Import</div>
        <p style="color:var(--muted);font-size:12px;font-family:'DM Mono',monospace;line-height:1.6;margin-bottom:14px">Drop any Torn screenshot — trade screen, trade receipt, bank screen, travel screen. AI will read it and extract the data.</p>

        <div class="drop-zone" id="drop-zone" onclick="document.getElementById('img-upload').click()" ondragover="handleDragOver(event)" ondragleave="handleDragLeave(event)" ondrop="handleDrop(event)">
          <div class="dz-icon">📸</div>
          <p>Tap to upload screenshot</p>
          <p class="hint">or drag & drop • supports PNG, JPG</p>
        </div>
        <input type="file" id="img-upload" accept="image/*" style="display:none" onchange="handleImageUpload(event)">

        <div class="ai-loading" id="ai-loading">
          <div style="font-size:20px;margin-bottom:8px">⚡</div>
          Reading screenshot...
        </div>

        <div class="ai-result" id="ai-result"></div>

        <div id="ai-action-btns" style="display:none;margin-top:12px">
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px">
            <button class="btn green" id="ai-log-trade-btn" style="display:none" onclick="logAITrade()">Log as Trade</button>
            <button class="btn" id="ai-log-travel-btn" style="display:none" onclick="logAITravel()">Log as Travel Run</button>
            <button class="btn secondary" id="ai-update-balance-btn" style="display:none" onclick="applyAIBalance()">Update Balances</button>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-title"><div class="dot"></div>How to Use AI Import</div>
        <div style="font-family:'DM Mono',monospace;font-size:11px;color:var(--muted);line-height:1.8">
          <div style="margin-bottom:10px"><span style="color:var(--accent)">Trade Screen</span><br>Screenshots showing items you added + the buyer/seller and dollar amount — it extracts the trader, items list, and price.</div>
          <div style="margin-bottom:10px"><span style="color:var(--accent)">Trade Receipt</span><br>The tornexchange.com receipt — extracts each item, price, quantity, and subtotals.</div>
          <div style="margin-bottom:10px"><span style="color:var(--accent)">Balance Screen</span><br>Your cash balance shown in the game header — updates your cash on hand.</div>
          <div><span style="color:var(--accent)">Travel Screen</span><br>Any screen showing items you bought abroad — logs a travel run with items and location.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- CHARTS -->
  <div class="page" id="page-charts">
    <div class="grid-2" style="margin-bottom:16px">
      <div class="card">
        <div class="card-title"><div class="dot"></div>Net Worth Over Time</div>
        <div class="chart-wrap"><canvas id="chart-nw"></canvas></div>
      </div>
      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent3)"></div>Income Breakdown (Today)</div>
        <div class="chart-wrap"><canvas id="chart-income"></canvas></div>
      </div>
    </div>
    <div class="grid-2">
      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent2)"></div>Travel Profit by Location</div>
        <div class="chart-wrap"><canvas id="chart-travel"></canvas></div>
      </div>
      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--danger)"></div>Top Traders by Volume</div>
        <div class="chart-wrap"><canvas id="chart-traders"></canvas></div>
      </div>
    </div>
  </div>

  <!-- SETTINGS -->
  <div class="page" id="page-settings">
    <div class="grid-2" style="margin-bottom:16px">
      <div class="card">
        <div class="card-title"><div class="dot"></div>Backup & Restore</div>
        <p style="color:var(--muted);font-size:12px;font-family:'DM Mono',monospace;line-height:1.6;margin-bottom:14px">Export your data as a JSON file to back it up. Import it on any device to restore everything.</p>
        <button class="btn green" onclick="exportData()" style="margin-bottom:8px">Export Data (Download JSON)</button>
        <button class="btn secondary" onclick="document.getElementById('import-file').click()">Import Data (Load JSON)</button>
        <input type="file" id="import-file" accept=".json" style="display:none" onchange="importData(event)">
        <div class="divider"></div>
        <div class="card-title"><div class="dot" style="background:var(--danger)"></div>Danger Zone</div>
        <button class="btn danger" onclick="clearAllData()">Clear All Data</button>
      </div>

      <div class="card">
        <div class="card-title"><div class="dot" style="background:var(--accent3)"></div>GitHub Pages Setup (iPhone)</div>
        <div class="step">
          <div class="step-num">1</div>
          <div class="step-body">Go to <strong>github.com</strong> and create a free account if you don't have one.</div>
        </div>
        <div class="step">
          <div class="step-num">2</div>
          <div class="step-body">Create a new repository called <strong>empire-ledger</strong> (public).</div>
        </div>
        <div class="step">
          <div class="step-num">3</div>
          <div class="step-body">Upload this HTML file — rename it to <strong>index.html</strong> before uploading.</div>
        </div>
        <div class="step">
          <div class="step-num">4</div>
          <div class="step-body">Go to Settings → Pages → set Source to <strong>main branch</strong> → Save.</div>
        </div>
        <div class="step">
          <div class="step-num">5</div>
          <div class="step-body">Your URL will be <strong>yourusername.github.io/empire-ledger</strong>. Open it in Safari on iPhone → Share → <strong>Add to Home Screen</strong>.</div>
        </div>
        <div style="background:rgba(200,169,110,0.08);border:1px solid rgba(200,169,110,0.2);border-radius:8px;padding:10px 12px;margin-top:4px">
          <p style="font-family:'DM Mono',monospace;font-size:10px;color:var(--accent);line-height:1.6">Data saves in Safari's localStorage on your phone. Use Export/Import on this page to back it up periodically.</p>
        </div>
      </div>
    </div>
  </div>

</div><!-- /container -->

<script>
// =====================
// STATE
// =====================
const KEY = 'empireledger_v3';

function defaultState() {
  return {
    balances: { cash: 594263, bank: 3050423, cayman: 627992497, inv: 0, bankRate: 0, caymanRate: 0, bankTerm: '' },
    travelLog: [],
    tradeLog: [],
    activityLog: [],
    nwHistory: [{ ts: Date.now(), total: 631636183 }],
    jobPayToday: 0,
    customIncomes: [],
    customExpenses: [],
    lastSaveDate: null
  };
}

let S = (() => {
  try { return JSON.parse(localStorage.getItem(KEY)) || defaultState(); }
  catch { return defaultState(); }
})();

// Seed with screenshot trade data if tradeLog is empty
function seedTradeData() {
  if (S.tradeLog.length > 0) return;
  const now = Date.now();
  const h = 3600000;
  const trades = [
    { ts: now - 8*h, trader: 'Dracula', type: 'sold', items: '19x Banana Orchid, 9x Crocus, 45x Dahlia, 4x Tribulus Omanense, 10x Personal Computer, 10x Elephant Statue, 9x Nodding Turtle, 9x Jaguar Plushie, 1x Nessie Plushie, 61x Sheep Plushie, 18x Stingray Plushie, 75x Teddy Bear Plushie, 9x Wolverine Plushie', price: 1110000, market: 0, premium: 'yes', notes: '279 items total' },
    { ts: now - 7*h, trader: 'FrugalWhorz', type: 'sold', items: '19x Crocus, 19x Dahlia, 19x Monkey Plushie, 19x Panda Plushie, 19x Red Fox Plushie', price: 2156150, market: 2461883, premium: 'yes', notes: 'Trade receipt: Crocus $117,515 + Dahlia $44,973 + Monkey $633,422 + Panda $1,051,973 + Red Fox $614,384' },
    { ts: now - 6*h, trader: 'MKBBGL', type: 'sold', items: '19x Tribulus Omanense, 19x Monkey Plushie, 19x Nessie Plushie', price: 2461298, market: 0, premium: 'yes', notes: 'Trade log 07/05/26' },
    { ts: now - 5*h, trader: 'MKBBGL', type: 'sold', items: '38x Tribulus Omanense, 19x Red Fox Plushie, 38x Stingray Plushie', price: 3239576, market: 0, premium: 'yes', notes: 'Trade log 10/05/26' },
    { ts: now - 4*h, trader: 'Deadringers', type: 'sold', items: '19x Dahlia, 19x Nessie Plushie, 19x Red Fox Plushie, 38x Stingray Plushie, 19x Tribulus Omanense, 57x Wolverine Plushie', price: 3142277, market: 0, premium: 'yes', notes: '' },
    { ts: now - 3*h, trader: 'Clouds', type: 'sold', items: '38x Banana Orchid, 38x Crocus, 57x Lion Plushie, 19x Panda Plushie, 19x Red Fox Plushie, 19x Stingray Plushie, 19x Tribulus Omanense, 38x Wolverine Plushie', price: 7236701, market: 0, premium: 'yes', notes: '' },
    { ts: now - 2*h, trader: 'Clouds', type: 'sold', items: '1x Can of Crocozade, 1x Can of Damp Valley, 1x Can of Goose Juice, 87x Can of Munster, 66x Can of Red Cow, 1x Can of Rockstar Rudolph, 92x Can of Taurine Elite, 1x Can of X-MASS, 28x Cannabis, 12x Xanax, 19x Stingray Plushie, 19x Tribulus Omanense', price: 583219171, market: 0, premium: 'yes', notes: '329 items across 5 categories — large bulk trade' },
  ];
  S.tradeLog = trades;
  trades.forEach(t => {
    S.activityLog.push({ ts: t.ts, type: 'Trade', desc: `Sold to ${t.trader}`, amount: t.price, net: t.price });
  });
  S.activityLog.sort((a, b) => b.ts - a.ts);
  save();
}

function save() { localStorage.setItem(KEY, JSON.stringify(S)); }

// =====================
// NAVIGATION
// =====================
function showPage(name, btn) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
  document.getElementById('page-' + name).classList.add('active');
  if (btn) btn.classList.add('active');
  if (name === 'charts') renderCharts();
}

// =====================
// FORMAT
// =====================
function fmt(n) {
  if (n === null || n === undefined || isNaN(n)) return '$0';
  const abs = Math.abs(n);
  const sign = n < 0 ? '-' : '';
  if (abs >= 1e9) return sign + '$' + (abs/1e9).toFixed(2) + 'B';
  if (abs >= 1e6) return sign + '$' + (abs/1e6).toFixed(2) + 'M';
  if (abs >= 1e3) return sign + '$' + (abs/1e3).toFixed(1) + 'K';
  return (n < 0 ? '-$' : '$') + Math.abs(n).toLocaleString();
}

function fmtDate(ts) {
  const d = new Date(ts);
  return d.toLocaleDateString('en-US', { month:'short', day:'numeric' }) + ' ' +
    d.toLocaleTimeString('en-US', { hour:'2-digit', minute:'2-digit' });
}

function todayStr() { return new Date().toDateString(); }

function totalNW() {
  return (S.balances.cash||0) + (S.balances.bank||0) + (S.balances.cayman||0) + (S.balances.inv||0);
}

function dailyInterest(bal, rate) { return (bal * (rate/100)) / 365; }

// =====================
// BALANCES
// =====================
function saveBalances() {
  const fields = ['cash','bank','cayman','inv','bankRate','caymanRate'];
  const inputs = ['inp-cash','inp-bank','inp-cayman','inp-inv','inp-bank-rate','inp-cayman-rate'];
  fields.forEach((f, i) => {
    const v = parseFloat(document.getElementById(inputs[i]).value);
    if (!isNaN(v)) S.balances[f] = v;
  });
  const term = document.getElementById('inp-bank-term').value;
  if (term !== undefined) S.balances.bankTerm = term;

  S.nwHistory.push({ ts: Date.now(), total: totalNW() });
  if (S.nwHistory.length > 120) S.nwHistory.shift();
  save();
  updateNWBar();
  updateDashboard();
}

function promptEdit(field, label) {
  const val = prompt('Enter new ' + label + ':');
  if (val !== null && !isNaN(parseFloat(val))) {
    S.balances[field] = parseFloat(val);
    S.nwHistory.push({ ts: Date.now(), total: totalNW() });
    save();
    updateNWBar();
    updateDashboard();
  }
}

function updateNWBar() {
  const total = totalNW();
  const prev = S.nwHistory.length > 1 ? S.nwHistory[S.nwHistory.length-2].total : total;
  const diff = total - prev;
  document.getElementById('nw-total').textContent = fmt(total);
  document.getElementById('nw-cash').textContent = fmt(S.balances.cash);
  document.getElementById('nw-bank').textContent = fmt(S.balances.bank);
  document.getElementById('nw-cayman').textContent = fmt(S.balances.cayman);
  document.getElementById('nw-inv').textContent = fmt(S.balances.inv);
  document.getElementById('nw-bank-info').textContent = S.balances.bankTerm ?
    `${S.balances.bankTerm} @ ${S.balances.bankRate}%` : 'no term set';
  document.getElementById('nw-change').textContent = diff === 0 ? 'no change since last save' :
    (diff > 0 ? '▲ ' : '▼ ') + fmt(Math.abs(diff)) + ' since last save';
}

// =====================
// DASHBOARD
// =====================
function getTodayTravel() { return S.travelLog.filter(r => new Date(r.ts).toDateString() === todayStr()); }
function getTodayTrades() { return S.tradeLog.filter(r => new Date(r.ts).toDateString() === todayStr()); }

function updateDashboard() {
  const todayRuns = getTodayTravel();
  const todayTrades = getTodayTrades();
  const travelCost = todayRuns.reduce((a,r) => a+r.cost, 0);
  const travelRev = todayRuns.reduce((a,r) => a+r.revenue, 0);
  const tradeRev = todayTrades.filter(t => t.type==='sold').reduce((a,t) => a+t.price, 0);
  const bankInt = dailyInterest(S.balances.bank, S.balances.bankRate);
  const caymanInt = dailyInterest(S.balances.cayman, S.balances.caymanRate);
  const netToday = (S.jobPayToday||0) + travelRev - travelCost + tradeRev + bankInt + caymanInt;

  document.getElementById('sum-job').textContent = fmt(S.jobPayToday||0);
  document.getElementById('sum-runs').textContent = todayRuns.length;
  document.getElementById('sum-travel-rev').textContent = fmt(travelRev);
  document.getElementById('sum-travel-cost').textContent = fmt(travelCost);
  document.getElementById('sum-trade-rev').textContent = fmt(tradeRev);
  document.getElementById('sum-bank-int').textContent = fmt(Math.round(bankInt));
  document.getElementById('sum-cayman-int').textContent = fmt(Math.round(caymanInt));
  document.getElementById('sum-net').textContent = fmt(Math.round(netToday));

  updateActivityTable();
  updateTravelStats();
  updateTradeStats();
  updateCalculator();
}

function logJobPay() {
  const amt = parseFloat(document.getElementById('inp-job').value)||0;
  if (!amt) return;
  S.jobPayToday = (S.jobPayToday||0) + amt;
  S.activityLog.unshift({ ts: Date.now(), type: 'Job', desc: 'Job pay received', amount: amt, net: amt });
  if (S.activityLog.length > 300) S.activityLog.pop();
  save(); updateDashboard();
  document.getElementById('inp-job').value = '';
}

function updateActivityTable() {
  const tbody = document.getElementById('activity-tbody');
  const items = S.activityLog.slice(0, 40);
  document.getElementById('activity-count').textContent = items.length + ' entries';
  if (!items.length) {
    tbody.innerHTML = '<tr><td colspan="5"><div class="empty"><div class="icon">📋</div>No activity yet</div></td></tr>';
    return;
  }
  tbody.innerHTML = items.map(a => `
    <tr>
      <td>${fmtDate(a.ts)}</td>
      <td><span class="tag ${a.type==='Trade'?'sold':'okay'}">${a.type}</span></td>
      <td style="max-width:220px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${a.desc}</td>
      <td>${fmt(a.amount)}</td>
      <td><span class="profit-pill ${(a.net||0)>=0?'pos':'neg'}">${fmt(a.net||0)}</span></td>
    </tr>`).join('');
}

// =====================
// TRAVEL
// =====================
function fillTravelTime() {
  const sel = document.getElementById('t-location');
  const opt = sel.options[sel.selectedIndex];
  const time = opt.dataset.time;
  if (time && time !== '0') document.getElementById('t-travel-time').value = time;
}

let timerInterval = null, timerStart = null;
function startTimer() {
  if (timerInterval) return;
  timerStart = Date.now();
  document.getElementById('timer-label').textContent = 'traveling...';
  timerInterval = setInterval(() => {
    const e = Math.floor((Date.now()-timerStart)/1000);
    const h = String(Math.floor(e/3600)).padStart(2,'0');
    const m = String(Math.floor((e%3600)/60)).padStart(2,'0');
    const s = String(e%60).padStart(2,'0');
    document.getElementById('timer-display').textContent = `${h}:${m}:${s}`;
  }, 1000);
}
function stopTimer() {
  clearInterval(timerInterval); timerInterval = null; timerStart = null;
  document.getElementById('timer-display').textContent = '00:00:00';
  document.getElementById('timer-label').textContent = 'ready';
}

function logTravel() {
  const locSel = document.getElementById('t-location').value;
  const loc = locSel === 'Custom' ? document.getElementById('t-custom-loc').value : locSel;
  const travelTime = parseInt(document.getElementById('t-travel-time').value)||0;
  const items = document.getElementById('t-items').value;
  const cost = parseFloat(document.getElementById('t-cost').value)||0;
  const qty = parseInt(document.getElementById('t-qty').value)||0;
  const revenue = parseFloat(document.getElementById('t-revenue').value)||0;
  const quality = document.getElementById('t-quality').value;
  const trader = document.getElementById('t-trader').value;
  const notes = document.getElementById('t-notes').value;
  if (!loc || !items) { alert('Please enter location and items.'); return; }
  const profit = revenue - cost;
  S.travelLog.unshift({ ts: Date.now(), loc, travelTime, items, cost, qty, revenue, quality, trader, notes, profit });
  S.activityLog.unshift({ ts: Date.now(), type: 'Travel', desc: `${loc} — ${items}`, amount: revenue, net: profit });
  if (S.activityLog.length > 300) S.activityLog.pop();
  save(); updateDashboard(); renderTravelTable();
  ['t-items','t-cost','t-qty','t-revenue','t-notes'].forEach(id => document.getElementById(id).value = '');
}

function renderTravelTable() {
  const tbody = document.getElementById('travel-tbody');
  document.getElementById('travel-count').textContent = S.travelLog.length + ' runs';
  if (!S.travelLog.length) { tbody.innerHTML = '<tr><td colspan="11"><div class="empty"><div class="icon">✈️</div>No travel runs logged yet</div></td></tr>'; return; }
  tbody.innerHTML = S.travelLog.slice(0,60).map((r,i) => `
    <tr>
      <td>${fmtDate(r.ts)}</td><td>${r.loc}</td>
      <td style="max-width:160px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${r.items}</td>
      <td>${r.qty||'-'}</td>
      <td style="color:var(--danger)">${fmt(r.cost)}</td>
      <td style="color:var(--accent3)">${fmt(r.revenue)}</td>
      <td><span class="profit-pill ${r.profit>=0?'pos':'neg'}">${fmt(r.profit)}</span></td>
      <td>${r.travelTime?r.travelTime+'m':'-'}</td>
      <td><span class="tag ${r.quality}">${r.quality}</span></td>
      <td>${r.trader==='yes'?'✓':'—'}</td>
      <td><button class="btn sm secondary" onclick="deleteTravel(${i})">✕</button></td>
    </tr>`).join('');
}

function deleteTravel(i) {
  if (!confirm('Delete this travel run?')) return;
  S.travelLog.splice(i,1); save(); updateDashboard(); renderTravelTable();
}

function updateTravelStats() {
  const today = getTodayTravel();
  const cost = today.reduce((a,r)=>a+r.cost,0);
  const rev = today.reduce((a,r)=>a+r.revenue,0);
  const profit = rev - cost;
  document.getElementById('ts-runs').textContent = today.length;
  document.getElementById('ts-cost').textContent = fmt(cost);
  document.getElementById('ts-rev').textContent = fmt(rev);
  document.getElementById('ts-profit').textContent = fmt(profit);
  document.getElementById('ts-avg').textContent = today.length ? fmt(Math.round(profit/today.length)) : '$0';
}

// =====================
// TRADES
// =====================
function logTrade(data) {
  const d = data || {
    trader: document.getElementById('tr-trader').value,
    type: document.getElementById('tr-type').value,
    items: document.getElementById('tr-items').value,
    price: parseFloat(document.getElementById('tr-price').value)||0,
    market: parseFloat(document.getElementById('tr-market').value)||0,
    premium: document.getElementById('tr-premium').value,
    notes: document.getElementById('tr-notes').value
  };
  if (!d.trader || !d.items) { alert('Enter trader name and items.'); return; }
  S.tradeLog.unshift({ ts: Date.now(), ...d });
  S.activityLog.unshift({ ts: Date.now(), type: 'Trade', desc: `${d.type==='sold'?'Sold to':'Bought from'} ${d.trader}: ${d.items}`, amount: d.price, net: d.type==='sold'?d.price:-d.price });
  if (S.activityLog.length > 300) S.activityLog.pop();
  save(); updateTradeStats(); renderTradeTable(); updateDashboard();
  if (!data) ['tr-trader','tr-items','tr-price','tr-market','tr-notes'].forEach(id => document.getElementById(id).value = '');
}

function renderTradeTable() {
  const tbody = document.getElementById('trades-tbody');
  document.getElementById('trades-count').textContent = S.tradeLog.length + ' trades';
  if (!S.tradeLog.length) { tbody.innerHTML = '<tr><td colspan="9"><div class="empty"><div class="icon">📒</div>No trades logged yet</div></td></tr>'; return; }
  tbody.innerHTML = S.tradeLog.slice(0,60).map((r,i) => `
    <tr>
      <td>${fmtDate(r.ts)}</td><td>${r.trader}</td>
      <td><span class="tag ${r.type}">${r.type}</span></td>
      <td style="max-width:180px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${r.items}</td>
      <td style="color:${r.type==='sold'?'var(--accent3)':'var(--danger)'}">${fmt(r.price)}</td>
      <td>${r.market?fmt(r.market):'—'}</td>
      <td>${r.premium==='yes'?'✓':'-'}</td>
      <td style="color:var(--muted);font-size:10px;max-width:120px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${r.notes||'—'}</td>
      <td><button class="btn sm secondary" onclick="deleteTrade(${i})">✕</button></td>
    </tr>`).join('');
}

function deleteTrade(i) {
  if (!confirm('Delete this trade?')) return;
  S.tradeLog.splice(i,1); save(); updateTradeStats(); renderTradeTable(); updateDashboard();
}

function updateTradeStats() {
  const sold = S.tradeLog.filter(t=>t.type==='sold').reduce((a,t)=>a+t.price,0);
  const bought = S.tradeLog.filter(t=>t.type==='bought').reduce((a,t)=>a+t.price,0);
  const traders = [...new Set(S.tradeLog.map(t=>t.trader))];
  document.getElementById('tr-total-sold').textContent = fmt(sold);
  document.getElementById('tr-total-bought').textContent = fmt(bought);
  document.getElementById('tr-unique').textContent = traders.length;
  document.getElementById('tr-count').textContent = S.tradeLog.length;
  const traderMap = {};
  S.tradeLog.forEach(t => { traderMap[t.trader] = (traderMap[t.trader]||0) + t.price; });
  const sorted = Object.entries(traderMap).sort((a,b)=>b[1]-a[1]).slice(0,5);
  const el = document.getElementById('top-traders');
  el.innerHTML = sorted.length ? sorted.map(([name,vol]) =>
    `<div class="stat-row"><span class="key">${name}</span><span class="val gold">${fmt(vol)}</span></div>`
  ).join('') : '<div class="empty" style="padding:10px 0"><div class="icon" style="font-size:16px">🤝</div>No trades</div>';
}

// =====================
// CALCULATOR
// =====================
let customIncomes = S.customIncomes || [];
let customExpenses = S.customExpenses || [];

function addCustomIncome() {
  const label = document.getElementById('ci-label').value;
  const amount = parseFloat(document.getElementById('ci-amount').value)||0;
  if (!label) return;
  customIncomes.push({ label, amount });
  S.customIncomes = customIncomes;
  document.getElementById('ci-label').value = '';
  document.getElementById('ci-amount').value = '';
  save(); updateCalculator();
}

function addCustomExpense() {
  const label = document.getElementById('ce-label').value;
  const amount = parseFloat(document.getElementById('ce-amount').value)||0;
  if (!label) return;
  customExpenses.push({ label, amount });
  S.customExpenses = customExpenses;
  document.getElementById('ce-label').value = '';
  document.getElementById('ce-amount').value = '';
  save(); updateCalculator();
}

function updateCalculator() {
  const todayRuns = getTodayTravel();
  const todayTrades = getTodayTrades();
  const travelCost = todayRuns.reduce((a,r)=>a+r.cost,0);
  const travelRev = todayRuns.reduce((a,r)=>a+r.revenue,0);
  const tradeRev = todayTrades.filter(t=>t.type==='sold').reduce((a,t)=>a+t.price,0);
  const tradeBought = todayTrades.filter(t=>t.type==='bought').reduce((a,t)=>a+t.price,0);
  const bankInt = dailyInterest(S.balances.bank, S.balances.bankRate);
  const caymanInt = dailyInterest(S.balances.cayman, S.balances.caymanRate);
  const ciTotal = customIncomes.reduce((a,c)=>a+c.amount,0);
  const ceTotal = customExpenses.reduce((a,c)=>a+c.amount,0);
  const grossInc = (S.jobPayToday||0) + travelRev + tradeRev + bankInt + caymanInt + ciTotal;
  const grossExp = travelCost + tradeBought + ceTotal;
  const net = grossInc - grossExp;

  document.getElementById('calc-job').textContent = fmt(S.jobPayToday||0);
  document.getElementById('calc-travel').textContent = fmt(travelRev);
  document.getElementById('calc-trades').textContent = fmt(tradeRev);
  document.getElementById('calc-bank-int').textContent = fmt(Math.round(bankInt));
  document.getElementById('calc-cayman-int').textContent = fmt(Math.round(caymanInt));
  document.getElementById('calc-cogs').textContent = fmt(travelCost);
  document.getElementById('calc-bought').textContent = fmt(tradeBought);
  document.getElementById('calc-total-inc').textContent = fmt(Math.round(grossInc));
  document.getElementById('calc-total-exp').textContent = fmt(Math.round(grossExp));
  document.getElementById('calc-net').textContent = fmt(Math.round(net));

  document.getElementById('custom-incomes').innerHTML = customIncomes.map((c,i) =>
    `<div class="stat-row"><span class="key">${c.label}</span><span style="display:flex;align-items:center;gap:6px"><span class="val pos">${fmt(c.amount)}</span><button class="btn sm secondary" onclick="customIncomes.splice(${i},1);S.customIncomes=customIncomes;save();updateCalculator()">✕</button></span></div>`
  ).join('');

  document.getElementById('custom-expenses').innerHTML = customExpenses.map((c,i) =>
    `<div class="stat-row"><span class="key">${c.label}</span><span style="display:flex;align-items:center;gap:6px"><span class="val neg">${fmt(c.amount)}</span><button class="btn sm secondary" onclick="customExpenses.splice(${i},1);S.customExpenses=customExpenses;save();updateCalculator()">✕</button></span></div>`
  ).join('');
}

// =====================
// AI IMPORT
// =====================
let aiExtracted = null;

function handleDragOver(e) { e.preventDefault(); document.getElementById('drop-zone').classList.add('drag-over'); }
function handleDragLeave(e) { document.getElementById('drop-zone').classList.remove('drag-over'); }
function handleDrop(e) {
  e.preventDefault();
  document.getElementById('drop-zone').classList.remove('drag-over');
  const file = e.dataTransfer.files[0];
  if (file) processImageFile(file);
}
function handleImageUpload(e) { if (e.target.files[0]) processImageFile(e.target.files[0]); }

function processImageFile(file) {
  const reader = new FileReader();
  reader.onload = async (e) => {
    const base64 = e.target.result.split(',')[1];
    const mediaType = file.type || 'image/png';
    await sendToAI(base64, mediaType);
  };
  reader.readAsDataURL(file);
}

async function sendToAI(base64, mediaType) {
  const loadEl = document.getElementById('ai-loading');
  const resultEl = document.getElementById('ai-result');
  const actionEl = document.getElementById('ai-action-btns');
  loadEl.classList.add('visible');
  resultEl.classList.remove('visible');
  actionEl.style.display = 'none';
  aiExtracted = null;

  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        messages: [{
          role: 'user',
          content: [
            { type: 'image', source: { type: 'base64', media_type: mediaType, data: base64 } },
            { type: 'text', text: `You are reading a screenshot from the browser game "Torn". Extract ALL relevant financial data and return ONLY valid JSON with no markdown fences or extra text.

Return this exact structure (include only fields that have data):
{
  "type": "trade" | "travel" | "balance" | "receipt" | "unknown",
  "trader": "name of the other person in trade",
  "my_role": "seller" | "buyer",
  "items": "comma-separated list of items with quantities e.g. 19x Monkey Plushie, 38x Stingray Plushie",
  "price": 1234567,
  "location": "country name if travel screen",
  "cash_balance": 12345,
  "bank_balance": 12345,
  "cayman_balance": 12345,
  "notes": "any other useful info",
  "summary": "one sentence human-readable summary of what you found"
}` }
          ]
        }]
      })
    });

    const data = await response.json();
    const text = data.content.map(c => c.text || '').join('');

    try {
      aiExtracted = JSON.parse(text.replace(/```json|```/g, '').trim());
    } catch {
      aiExtracted = { type: 'unknown', summary: text };
    }

    loadEl.classList.remove('visible');
    resultEl.classList.add('visible');
    resultEl.innerHTML = `
      <div style="color:var(--accent);margin-bottom:8px;font-size:10px;letter-spacing:1px">AI EXTRACTED</div>
      <div style="color:var(--accent3);margin-bottom:6px">${aiExtracted.summary || 'Data extracted'}</div>
      <pre style="color:var(--muted);font-size:10px;white-space:pre-wrap">${JSON.stringify(aiExtracted, null, 2)}</pre>
    `;

    actionEl.style.display = 'block';
    document.getElementById('ai-log-trade-btn').style.display = (aiExtracted.type === 'trade' || aiExtracted.type === 'receipt') ? 'block' : 'none';
    document.getElementById('ai-log-travel-btn').style.display = aiExtracted.type === 'travel' ? 'block' : 'none';
    document.getElementById('ai-update-balance-btn').style.display = (aiExtracted.cash_balance || aiExtracted.bank_balance || aiExtracted.cayman_balance) ? 'block' : 'none';

  } catch (err) {
    loadEl.classList.remove('visible');
    resultEl.classList.add('visible');
    resultEl.innerHTML = `<div style="color:var(--danger)">Error: ${err.message}</div>`;
  }
}

function logAITrade() {
  if (!aiExtracted) return;
  const isSeller = aiExtracted.my_role === 'seller' || !aiExtracted.my_role;
  logTrade({
    trader: aiExtracted.trader || 'Unknown',
    type: isSeller ? 'sold' : 'bought',
    items: aiExtracted.items || '',
    price: aiExtracted.price || 0,
    market: 0,
    premium: 'yes',
    notes: aiExtracted.notes || 'Imported from screenshot'
  });
  document.getElementById('ai-result').innerHTML += `<div style="color:var(--accent3);margin-top:8px">✓ Logged as trade</div>`;
}

function logAITravel() {
  if (!aiExtracted) return;
  S.travelLog.unshift({
    ts: Date.now(), loc: aiExtracted.location || 'Unknown', travelTime: 0,
    items: aiExtracted.items || '', cost: 0, qty: 0, revenue: 0,
    quality: 'good', trader: 'yes', notes: aiExtracted.notes || 'Imported from screenshot', profit: 0
  });
  save(); renderTravelTable(); updateDashboard();
  document.getElementById('ai-result').innerHTML += `<div style="color:var(--accent3);margin-top:8px">✓ Logged as travel run</div>`;
}

function applyAIBalance() {
  if (!aiExtracted) return;
  if (aiExtracted.cash_balance) S.balances.cash = aiExtracted.cash_balance;
  if (aiExtracted.bank_balance) S.balances.bank = aiExtracted.bank_balance;
  if (aiExtracted.cayman_balance) S.balances.cayman = aiExtracted.cayman_balance;
  S.nwHistory.push({ ts: Date.now(), total: totalNW() });
  save(); updateNWBar(); updateDashboard();
  document.getElementById('ai-result').innerHTML += `<div style="color:var(--accent3);margin-top:8px">✓ Balances updated</div>`;
}

// =====================
// CHARTS
// =====================
let charts = {};

function renderCharts() {
  Chart.defaults.color = '#6e6e82';
  Chart.defaults.borderColor = '#2a2a38';

  const destroy = (key) => { if (charts[key]) { charts[key].destroy(); delete charts[key]; } };

  // NW history
  destroy('nw');
  if (S.nwHistory.length > 0) {
    charts.nw = new Chart(document.getElementById('chart-nw').getContext('2d'), {
      type: 'line',
      data: {
        labels: S.nwHistory.map(h => fmtDate(h.ts)),
        datasets: [{ label: 'Net Worth', data: S.nwHistory.map(h=>h.total), borderColor: '#c8a96e', backgroundColor: 'rgba(200,169,110,0.1)', fill: true, tension: 0.4, pointRadius: 3 }]
      },
      options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
    });
  }

  // Income breakdown today
  destroy('income');
  const todayRuns = getTodayTravel();
  const todayTrades = getTodayTrades();
  const incData = [S.jobPayToday||0, todayRuns.reduce((a,r)=>a+r.revenue,0), todayTrades.filter(t=>t.type==='sold').reduce((a,t)=>a+t.price,0), Math.round(dailyInterest(S.balances.bank,S.balances.bankRate)), Math.round(dailyInterest(S.balances.cayman,S.balances.caymanRate))].map(v=>Math.max(0,v));
  charts.income = new Chart(document.getElementById('chart-income').getContext('2d'), {
    type: 'doughnut',
    data: {
      labels: ['Job','Travel Rev','Trade Rev','Bank Int','Cayman Int'],
      datasets: [{ data: incData, backgroundColor: ['#c8a96e','#6ec8a9','#6e8ec8','#c86e6e','#a96ec8'], borderWidth: 0 }]
    },
    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'bottom', labels: { boxWidth: 10, font: { family: 'DM Mono', size: 9 } } } } }
  });

  // Travel by location
  destroy('travel');
  const locMap = {};
  S.travelLog.forEach(r => { locMap[r.loc] = (locMap[r.loc]||0) + r.profit; });
  const locKeys = Object.keys(locMap);
  charts.travel = new Chart(document.getElementById('chart-travel').getContext('2d'), {
    type: 'bar',
    data: {
      labels: locKeys.length ? locKeys : ['No data'],
      datasets: [{ data: locKeys.length ? Object.values(locMap) : [0], backgroundColor: Object.values(locMap).map(v=>v>=0?'rgba(110,200,169,0.7)':'rgba(200,110,110,0.7)'), borderRadius: 6 }]
    },
    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { x: { grid: { display: false } } } }
  });

  // Top traders
  destroy('traders');
  const traderMap = {};
  S.tradeLog.forEach(t => { traderMap[t.trader] = (traderMap[t.trader]||0) + t.price; });
  const sorted = Object.entries(traderMap).sort((a,b)=>b[1]-a[1]).slice(0,7);
  charts.traders = new Chart(document.getElementById('chart-traders').getContext('2d'), {
    type: 'bar',
    data: {
      labels: sorted.length ? sorted.map(([n])=>n) : ['No data'],
      datasets: [{ data: sorted.length ? sorted.map(([,v])=>v) : [0], backgroundColor: 'rgba(200,169,110,0.7)', borderRadius: 6 }]
    },
    options: { responsive: true, maintainAspectRatio: false, indexAxis: 'y', plugins: { legend: { display: false } }, scales: { y: { grid: { display: false } } } }
  });
}

// =====================
// BACKUP / RESTORE
// =====================
function exportData() {
  const blob = new Blob([JSON.stringify(S, null, 2)], { type: 'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'empire-ledger-backup-' + new Date().toISOString().slice(0,10) + '.json';
  a.click();
}

function importData(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = (ev) => {
    try {
      const data = JSON.parse(ev.target.result);
      S = data;
      customIncomes = S.customIncomes || [];
      customExpenses = S.customExpenses || [];
      save(); init();
      alert('Data imported successfully!');
    } catch { alert('Invalid backup file.'); }
  };
  reader.readAsText(file);
}

function clearAllData() {
  if (!confirm('This will delete ALL your data permanently. Are you sure?')) return;
  if (!confirm('Last chance — really delete everything?')) return;
  S = defaultState(); customIncomes = []; customExpenses = [];
  save(); init(); alert('All data cleared.');
}

// =====================
// INIT
// =====================
function init() {
  seedTradeData();
  updateNWBar();
  updateDashboard();
  renderTravelTable();
  renderTradeTable();
  updateTradeStats();
  updateCalculator();

  // Prefill balance inputs
  if (S.balances.cash) document.getElementById('inp-cash').value = S.balances.cash;
  if (S.balances.bank) document.getElementById('inp-bank').value = S.balances.bank;
  if (S.balances.cayman) document.getElementById('inp-cayman').value = S.balances.cayman;
  if (S.balances.inv) document.getElementById('inp-inv').value = S.balances.inv;
  if (S.balances.bankRate) document.getElementById('inp-bank-rate').value = S.balances.bankRate;
  if (S.balances.caymanRate) document.getElementById('inp-cayman-rate').value = S.balances.caymanRate;
  if (S.balances.bankTerm) document.getElementById('inp-bank-term').value = S.balances.bankTerm;
}

init();
</script>
</body>
</html>

