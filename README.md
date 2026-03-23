
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sales Performance Dashboard 2024</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0e1a;
    --surface: #111827;
    --surface2: #1a2235;
    --border: rgba(255,255,255,0.07);
    --accent: #00e5ff;
    --accent2: #ff6b35;
    --accent3: #7c3aed;
    --accent4: #10b981;
    --gold: #f59e0b;
    --text: #f1f5f9;
    --muted: #64748b;
    --glow: 0 0 40px rgba(0,229,255,0.15);
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Background mesh */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background:
      radial-gradient(ellipse 80% 50% at 20% 10%, rgba(0,229,255,0.06) 0%, transparent 60%),
      radial-gradient(ellipse 60% 40% at 80% 80%, rgba(124,58,237,0.07) 0%, transparent 60%),
      radial-gradient(ellipse 50% 60% at 60% 30%, rgba(255,107,53,0.04) 0%, transparent 60%);
    pointer-events: none;
    z-index: 0;
  }

  .wrapper { position: relative; z-index: 1; padding: 24px; max-width: 1600px; margin: 0 auto; }

  /* ─── HEADER ─── */
  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 28px;
    padding-bottom: 20px;
    border-bottom: 1px solid var(--border);
  }

  .header-left h1 {
    font-family: 'Syne', sans-serif;
    font-size: 28px;
    font-weight: 800;
    letter-spacing: -0.5px;
    background: linear-gradient(135deg, #fff 0%, var(--accent) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .header-left p {
    font-size: 13px;
    color: var(--muted);
    margin-top: 4px;
    font-weight: 300;
    letter-spacing: 0.5px;
  }

  .header-right {
    display: flex;
    gap: 10px;
    align-items: center;
  }

  .badge {
    background: rgba(0,229,255,0.1);
    border: 1px solid rgba(0,229,255,0.25);
    color: var(--accent);
    padding: 6px 14px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 500;
    letter-spacing: 0.5px;
  }

  .badge.orange {
    background: rgba(255,107,53,0.1);
    border-color: rgba(255,107,53,0.25);
    color: var(--accent2);
  }

  /* ─── FILTER TABS ─── */
  .filter-bar {
    display: flex;
    gap: 8px;
    margin-bottom: 24px;
    flex-wrap: wrap;
    align-items: center;
  }

  .filter-label {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-right: 4px;
  }

  .filter-btn {
    padding: 7px 16px;
    border-radius: 8px;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--muted);
    font-family: 'DM Sans', sans-serif;
    font-size: 12px;
    cursor: pointer;
    transition: all 0.2s;
    font-weight: 500;
  }

  .filter-btn:hover, .filter-btn.active {
    background: rgba(0,229,255,0.12);
    border-color: rgba(0,229,255,0.4);
    color: var(--accent);
  }

  .filter-btn.active {
    box-shadow: 0 0 16px rgba(0,229,255,0.2);
  }

  /* ─── KPI CARDS ─── */
  .kpi-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 14px;
    margin-bottom: 20px;
  }

  .kpi-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 20px 18px;
    position: relative;
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.25s, box-shadow 0.25s;
  }

  .kpi-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--card-accent, var(--accent));
    opacity: 0.8;
  }

  .kpi-card::after {
    content: '';
    position: absolute;
    top: -30px; right: -20px;
    width: 90px; height: 90px;
    border-radius: 50%;
    background: var(--card-accent, var(--accent));
    opacity: 0.05;
    transition: opacity 0.3s;
  }

  .kpi-card:hover { transform: translateY(-3px); box-shadow: 0 12px 40px rgba(0,0,0,0.4); }
  .kpi-card:hover::after { opacity: 0.1; }

  .kpi-icon {
    font-size: 22px;
    margin-bottom: 10px;
    display: block;
  }

  .kpi-label {
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.8px;
    margin-bottom: 6px;
  }

  .kpi-value {
    font-family: 'Syne', sans-serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--text);
    line-height: 1;
  }

  .kpi-sub {
    font-size: 11px;
    color: var(--muted);
    margin-top: 6px;
  }

  .kpi-trend {
    font-size: 11px;
    font-weight: 600;
    margin-top: 6px;
    display: inline-flex;
    align-items: center;
    gap: 3px;
  }
  .up { color: var(--accent4); }
  .warn { color: var(--gold); }

  /* ─── CHART GRID ─── */
  .chart-grid {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr;
    grid-template-rows: auto auto;
    gap: 16px;
    margin-bottom: 16px;
  }

  .chart-row-2 {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }

  .chart-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 20px;
    position: relative;
    overflow: hidden;
    transition: box-shadow 0.3s;
  }

  .chart-card:hover {
    box-shadow: 0 8px 32px rgba(0,0,0,0.35);
    border-color: rgba(255,255,255,0.1);
  }

  .chart-card.span-row { grid-row: span 2; }

  .chart-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 16px;
  }

  .chart-title {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.3px;
    color: var(--text);
  }

  .chart-sub {
    font-size: 11px;
    color: var(--muted);
    margin-top: 3px;
  }

  .chart-badge {
    font-size: 10px;
    padding: 3px 9px;
    border-radius: 12px;
    font-weight: 600;
    letter-spacing: 0.3px;
  }

  .cb-cyan { background: rgba(0,229,255,0.12); color: var(--accent); }
  .cb-orange { background: rgba(255,107,53,0.12); color: var(--accent2); }
  .cb-green { background: rgba(16,185,129,0.12); color: var(--accent4); }
  .cb-purple { background: rgba(124,58,237,0.12); color: #a78bfa; }
  .cb-gold { background: rgba(245,158,11,0.12); color: var(--gold); }

  canvas { width: 100% !important; }

  /* ─── DATA TABLE ─── */
  .table-section {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 20px;
    margin-bottom: 16px;
    overflow: hidden;
  }

  .table-controls {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
    gap: 12px;
    flex-wrap: wrap;
  }

  .search-box {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 8px 14px;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    width: 240px;
    transition: border-color 0.2s;
  }

  .search-box:focus {
    outline: none;
    border-color: rgba(0,229,255,0.4);
  }

  .search-box::placeholder { color: var(--muted); }

  .table-scroll { overflow-x: auto; }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 12.5px;
  }

  th {
    font-family: 'Syne', sans-serif;
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.8px;
    color: var(--muted);
    padding: 10px 14px;
    text-align: left;
    border-bottom: 1px solid var(--border);
    white-space: nowrap;
    cursor: pointer;
    user-select: none;
    transition: color 0.2s;
  }

  th:hover { color: var(--accent); }
  th .sort-icon { margin-left: 4px; opacity: 0.5; }

  td {
    padding: 10px 14px;
    border-bottom: 1px solid rgba(255,255,255,0.03);
    color: var(--text);
    white-space: nowrap;
    transition: background 0.15s;
  }

  tr:hover td { background: rgba(255,255,255,0.025); }

  .city-tag {
    display: inline-flex;
    align-items: center;
    gap: 5px;
  }

  .dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    display: inline-block;
  }

  .profit-bar-cell { display: flex; align-items: center; gap: 8px; }

  .profit-bar-bg {
    flex: 1;
    height: 5px;
    background: var(--surface2);
    border-radius: 3px;
    overflow: hidden;
    min-width: 60px;
  }

  .profit-bar-fill {
    height: 100%;
    border-radius: 3px;
    background: linear-gradient(90deg, var(--accent4), var(--accent));
    transition: width 0.6s ease;
  }

  .status-pill {
    padding: 2px 9px;
    border-radius: 10px;
    font-size: 10px;
    font-weight: 600;
  }

  .sp-high { background: rgba(16,185,129,0.12); color: var(--accent4); }
  .sp-mid { background: rgba(245,158,11,0.12); color: var(--gold); }
  .sp-low { background: rgba(255,107,53,0.12); color: var(--accent2); }

  /* ─── BOTTOM ROW ─── */
  .bottom-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 16px;
  }

  /* ─── LEADERBOARD ─── */
  .leaderboard-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
  }

  .leaderboard-item:last-child { border-bottom: none; }

  .rank {
    font-family: 'Syne', sans-serif;
    font-size: 18px;
    font-weight: 800;
    color: var(--surface2);
    width: 28px;
    text-align: center;
  }

  .rank.gold { color: #f59e0b; }
  .rank.silver { color: #94a3b8; }
  .rank.bronze { color: #cd7f32; }

  .lb-name { flex: 1; font-size: 13px; font-weight: 500; }
  .lb-sub { font-size: 11px; color: var(--muted); }

  .lb-bar-wrap { width: 80px; }
  .lb-bar-bg { height: 4px; background: var(--surface2); border-radius: 2px; }
  .lb-bar-fill { height: 4px; border-radius: 2px; transition: width 0.6s; }

  .lb-val {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    text-align: right;
    min-width: 70px;
  }

  /* ─── PAYMENT SEGMENT SPLIT ─── */
  .split-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 0;
    border-bottom: 1px solid var(--border);
  }

  .split-item:last-child { border-bottom: none; }

  .split-label { font-size: 13px; font-weight: 500; }
  .split-val { font-size: 12px; color: var(--muted); }
  .split-pct {
    font-family: 'Syne', sans-serif;
    font-size: 15px;
    font-weight: 700;
  }

  .split-bar {
    width: 100%;
    height: 4px;
    background: var(--surface2);
    border-radius: 2px;
    margin-top: 6px;
    overflow: hidden;
  }

  .split-bar-fill {
    height: 100%;
    border-radius: 2px;
    transition: width 0.8s ease;
  }

  /* ─── TOOLTIP ─── */
  .tooltip {
    position: fixed;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 10px 14px;
    font-size: 12px;
    pointer-events: none;
    z-index: 9999;
    display: none;
    box-shadow: 0 8px 30px rgba(0,0,0,0.5);
  }

  /* ─── ANIMATIONS ─── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .kpi-card { animation: fadeUp 0.4s ease both; }
  .kpi-card:nth-child(1) { animation-delay: 0.05s; }
  .kpi-card:nth-child(2) { animation-delay: 0.1s; }
  .kpi-card:nth-child(3) { animation-delay: 0.15s; }
  .kpi-card:nth-child(4) { animation-delay: 0.2s; }
  .kpi-card:nth-child(5) { animation-delay: 0.25s; }

  .chart-card { animation: fadeUp 0.5s ease both; }
  .chart-card:nth-child(1) { animation-delay: 0.3s; }
  .chart-card:nth-child(2) { animation-delay: 0.35s; }
  .chart-card:nth-child(3) { animation-delay: 0.4s; }

  /* ─── SCROLLBAR ─── */
  ::-webkit-scrollbar { width: 6px; height: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--surface2); border-radius: 3px; }

  /* ─── RESPONSIVE ─── */
  @media (max-width: 1200px) {
    .kpi-grid { grid-template-columns: repeat(3, 1fr); }
    .chart-grid { grid-template-columns: 1fr 1fr; }
    .chart-row-2 { grid-template-columns: 1fr 1fr; }
    .bottom-grid { grid-template-columns: 1fr 1fr; }
  }

  @media (max-width: 768px) {
    .kpi-grid { grid-template-columns: repeat(2, 1fr); }
    .chart-grid, .chart-row-2, .bottom-grid { grid-template-columns: 1fr; }
    .header { flex-direction: column; gap: 12px; align-items: flex-start; }
  }

  /* pulse dot */
  .pulse-dot {
    width: 8px; height: 8px;
    background: var(--accent4);
    border-radius: 50%;
    display: inline-block;
    position: relative;
    margin-right: 6px;
  }
  .pulse-dot::after {
    content: '';
    position: absolute;
    inset: -3px;
    border-radius: 50%;
    border: 1px solid var(--accent4);
    animation: pulse 2s infinite;
    opacity: 0;
  }
  @keyframes pulse {
    0% { transform: scale(0.8); opacity: 0.8; }
    100% { transform: scale(2); opacity: 0; }
  }
</style>
</head>
<body>
<div class="wrapper">

  <!-- HEADER -->
  <div class="header">
    <div class="header-left">
      <h1>Sales Performance Dashboard</h1>
      <p>Fiscal Year 2024 &nbsp;·&nbsp; 500 Transactions &nbsp;·&nbsp; 6 Cities &nbsp;·&nbsp; 4 Regions</p>
    </div>
    <div class="header-right">
      <span class="badge"><span class="pulse-dot"></span>Live Data</span>
      <span class="badge orange">FY 2024</span>
      <span class="badge" style="background:rgba(124,58,237,0.1);border-color:rgba(124,58,237,0.25);color:#a78bfa">India</span>
    </div>
  </div>

  <!-- FILTER TABS -->
  <div class="filter-bar">
    <span class="filter-label">Filter by</span>
    <button class="filter-btn active" onclick="filterData('all', this)">All Regions</button>
    <button class="filter-btn" onclick="filterData('North', this)">North</button>
    <button class="filter-btn" onclick="filterData('South', this)">South</button>
    <button class="filter-btn" onclick="filterData('East', this)">East</button>
    <button class="filter-btn" onclick="filterData('West', this)">West</button>
    <div style="width:1px;height:20px;background:var(--border);margin:0 6px"></div>
    <button class="filter-btn" onclick="filterQuarter('all', this)">All Quarters</button>
    <button class="filter-btn" onclick="filterQuarter('Q1', this)">Q1</button>
    <button class="filter-btn" onclick="filterQuarter('Q2', this)">Q2</button>
    <button class="filter-btn" onclick="filterQuarter('Q3', this)">Q3</button>
    <button class="filter-btn" onclick="filterQuarter('Q4', this)">Q4</button>
  </div>

  <!-- KPI CARDS -->
  <div class="kpi-grid">
    <div class="kpi-card" style="--card-accent: #00e5ff">
      <span class="kpi-icon">💰</span>
      <div class="kpi-label">Total Revenue</div>
      <div class="kpi-value" id="kpi-revenue">₹1.26Cr</div>
      <div class="kpi-sub">Across all channels</div>
      <div class="kpi-trend up">↑ 500 orders processed</div>
    </div>
    <div class="kpi-card" style="--card-accent: #10b981">
      <span class="kpi-icon">📈</span>
      <div class="kpi-label">Net Profit</div>
      <div class="kpi-value" id="kpi-profit">₹12.64L</div>
      <div class="kpi-sub">Profit margin 10.0%</div>
      <div class="kpi-trend up">↑ Healthy margins</div>
    </div>
    <div class="kpi-card" style="--card-accent: #ff6b35">
      <span class="kpi-icon">🛒</span>
      <div class="kpi-label">Total Orders</div>
      <div class="kpi-value" id="kpi-orders">500</div>
      <div class="kpi-sub">Avg 41.7 / month</div>
      <div class="kpi-trend up">↑ Consistent volume</div>
    </div>
    <div class="kpi-card" style="--card-accent: #7c3aed">
      <span class="kpi-icon">📦</span>
      <div class="kpi-label">Units Sold</div>
      <div class="kpi-value" id="kpi-units">1,485</div>
      <div class="kpi-sub">Avg 2.97 units/order</div>
      <div class="kpi-trend up">↑ Strong demand</div>
    </div>
    <div class="kpi-card" style="--card-accent: #f59e0b">
      <span class="kpi-icon">🏷️</span>
      <div class="kpi-label">Avg Discount</div>
      <div class="kpi-value" id="kpi-discount">15.2%</div>
      <div class="kpi-sub">Across all products</div>
      <div class="kpi-trend warn">⚠ Monitor closely</div>
    </div>
  </div>

  <!-- MAIN CHART GRID -->
  <div class="chart-grid">
    <!-- Monthly Trend - spans 2 rows -->
    <div class="chart-card span-row">
      <div class="chart-header">
        <div>
          <div class="chart-title">Monthly Revenue & Profit Trend</div>
          <div class="chart-sub">Jan – Dec 2024 · All Regions</div>
        </div>
        <span class="chart-badge cb-cyan">12 Months</span>
      </div>
      <canvas id="monthlyChart" height="220"></canvas>
    </div>

    <!-- Region Sales -->
    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">Sales by Region</div>
          <div class="chart-sub">Revenue distribution</div>
        </div>
        <span class="chart-badge cb-orange">4 Regions</span>
      </div>
      <canvas id="regionChart" height="160"></canvas>
    </div>

    <!-- Category Split -->
    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">Category Revenue</div>
          <div class="chart-sub">Electronics vs Furniture</div>
        </div>
        <span class="chart-badge cb-purple">2 Categories</span>
      </div>
      <canvas id="categoryChart" height="160"></canvas>
    </div>

    <!-- Segment Doughnut -->
    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">Customer Segments</div>
          <div class="chart-sub">Revenue share</div>
        </div>
        <span class="chart-badge cb-green">3 Segments</span>
      </div>
      <canvas id="segmentChart" height="160"></canvas>
    </div>

    <!-- Quarterly -->
    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">Quarterly Performance</div>
          <div class="chart-sub">Q1–Q4 comparison</div>
        </div>
        <span class="chart-badge cb-gold">Grouped</span>
      </div>
      <canvas id="quarterChart" height="160"></canvas>
    </div>
  </div>

  <!-- SECOND CHART ROW -->
  <div class="chart-row-2">
    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">Sub-Category Revenue</div>
          <div class="chart-sub">All 6 sub-categories</div>
        </div>
        <span class="chart-badge cb-cyan">Horizontal</span>
      </div>
      <canvas id="subcatChart" height="180"></canvas>
    </div>

    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">City-wise Sales</div>
          <div class="chart-sub">Top 6 cities</div>
        </div>
        <span class="chart-badge cb-orange">Radar</span>
      </div>
      <canvas id="cityChart" height="180"></canvas>
    </div>

    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">Payment Mode Split</div>
          <div class="chart-sub">Card · Cash · UPI</div>
        </div>
        <span class="chart-badge cb-purple">Polar</span>
      </div>
      <canvas id="paymentChart" height="180"></canvas>
    </div>

    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">Profit vs Sales Scatter</div>
          <div class="chart-sub">Monthly correlation</div>
        </div>
        <span class="chart-badge cb-green">Scatter</span>
      </div>
      <canvas id="scatterChart" height="180"></canvas>
    </div>
  </div>

  <!-- BOTTOM SECTION -->
  <div class="bottom-grid">

    <!-- City Leaderboard -->
    <div class="chart-card">
      <div class="chart-header">
        <div>
          <div class="chart-title">🏆 City Revenue Leaderboard</div>
          <div class="chart-sub">Ranked by total sales</div>
        </div>
        <span class="chart-badge cb-gold">Top 6</span>
      </div>
      <div id="leaderboard"></div>
    </div>

    <!-- Data Table -->
    <div class="chart-card" style="grid-column: span 2">
      <div class="table-controls">
        <div>
          <div class="chart-title">📋 City Performance Summary</div>
          <div class="chart-sub">Click column headers to sort</div>
        </div>
        <input class="search-box" type="text" placeholder="🔍  Search city, region..." id="searchBox" oninput="filterTable()">
      </div>
      <div class="table-scroll">
        <table id="summaryTable">
          <thead>
            <tr>
              <th onclick="sortTable(0)">City <span class="sort-icon">⇅</span></th>
              <th onclick="sortTable(1)">Region <span class="sort-icon">⇅</span></th>
              <th onclick="sortTable(2)">Sales (₹) <span class="sort-icon">⇅</span></th>
              <th onclick="sortTable(3)">Profit (₹) <span class="sort-icon">⇅</span></th>
              <th onclick="sortTable(4)">Margin <span class="sort-icon">⇅</span></th>
              <th onclick="sortTable(5)">Orders <span class="sort-icon">⇅</span></th>
              <th>Profit Bar</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody id="tableBody"></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- Payment splits inline -->
  <div class="chart-row-2" style="margin-top:16px">
    <div class="chart-card" style="grid-column:span 2">
      <div class="chart-header">
        <div class="chart-title">💳 Payment Mode Deep Dive</div>
        <div class="chart-sub">Revenue and order share by payment method</div>
      </div>
      <div id="paymentSplits"></div>
    </div>
    <div class="chart-card" style="grid-column:span 2">
      <div class="chart-header">
        <div class="chart-title">👥 Customer Segment Analysis</div>
        <div class="chart-sub">Revenue contribution by segment</div>
      </div>
      <div id="segmentSplits"></div>
    </div>
  </div>

</div><!-- /wrapper -->

<div class="tooltip" id="tooltip"></div>

<script>
// ═══════════════════════════════════════════════
// DATA
// ═══════════════════════════════════════════════
const DATA = {
  monthly: [
    {m:'Jan',s:863456,p:86921,o:34},{m:'Feb',s:1179207,p:106186,o:40},
    {m:'Mar',s:1189143,p:98989,o:42},{m:'Apr',s:917034,p:92946,o:38},
    {m:'May',s:1076146,p:114004,o:40},{m:'Jun',s:978697,p:101692,o:41},
    {m:'Jul',s:1084673,p:103040,o:43},{m:'Aug',s:1344700,p:128245,o:50},
    {m:'Sep',s:845243,p:94804,o:37},{m:'Oct',s:835961,p:104416,o:37},
    {m:'Nov',s:933112,p:114402,o:47},{m:'Dec',s:1350931,p:118503,o:51}
  ],
  region: [
    {r:'East',s:2224854,p:227251,o:88},{r:'North',s:4412631,p:445025,o:175},
    {r:'South',s:3928412,p:398177,o:158},{r:'West',s:2032406,p:193695,o:79}
  ],
  category: [
    {c:'Electronics',s:8137230,p:796457},{c:'Furniture',s:4461073,p:467691}
  ],
  subcat: [
    {s:'Mobiles',v:2236219,p:240440},{s:'Tables',v:2224854,p:227251},
    {s:'Computers',v:2176412,p:204585},{s:'Seating',v:2032406,p:193695},
    {s:'Accessories',v:1976213,p:187862},{s:'Devices',v:1952199,p:210315}
  ],
  segment: [
    {s:'Consumer',v:4396179},{s:'Corporate',v:3815033},{s:'Home Office',v:4387091}
  ],
  payment: [
    {m:'Card',s:4081260,o:160},{m:'Cash',s:4353574,o:171},{m:'UPI',s:4163469,o:169}
  ],
  city: [
    {c:'Delhi',r:'North',s:2236219,p:240440,o:92},
    {c:'Kolkata',r:'East',s:2224854,p:227251,o:88},
    {c:'Lucknow',r:'North',s:2176412,p:204585,o:83},
    {c:'Mumbai',r:'West',s:2032406,p:193695,o:79},
    {c:'Bangalore',r:'South',s:1976213,p:187862,o:76},
    {c:'Chennai',r:'South',s:1952199,p:210315,o:82}
  ],
  quarter: [
    {q:'Q1',s:3231806,p:292096,o:116},{q:'Q2',s:2971877,p:308642,o:119},
    {q:'Q3',s:3274616,p:326089,o:130},{q:'Q4',s:3120004,p:337321,o:135}
  ]
};

// ═══════════════════════════════════════════════
// CHART.JS GLOBALS
// ═══════════════════════════════════════════════
Chart.defaults.color = '#64748b';
Chart.defaults.font.family = 'DM Sans';
Chart.defaults.font.size = 11;

const CYAN = '#00e5ff', ORANGE = '#ff6b35', GREEN = '#10b981',
      PURPLE = '#7c3aed', GOLD = '#f59e0b', PINK = '#ec4899';

const COLORS = [CYAN, ORANGE, GREEN, PURPLE, GOLD, PINK];

function rgba(hex, a) {
  const r = parseInt(hex.slice(1,3),16), g = parseInt(hex.slice(3,5),16), b = parseInt(hex.slice(5,7),16);
  return `rgba(${r},${g},${b},${a})`;
}

const gridOpts = {
  color: 'rgba(255,255,255,0.04)',
  drawBorder: false
};

const noLegend = { display: false };
const tooltipStyle = {
  backgroundColor: '#1a2235',
  borderColor: 'rgba(255,255,255,0.08)',
  borderWidth: 1,
  titleColor: '#f1f5f9',
  bodyColor: '#94a3b8',
  padding: 10,
  cornerRadius: 8,
  displayColors: true,
  boxWidth: 8, boxHeight: 8
};

// ── Monthly Line Chart ──
const monthlyCtx = document.getElementById('monthlyChart').getContext('2d');
const salesGrad = monthlyCtx.createLinearGradient(0, 0, 0, 300);
salesGrad.addColorStop(0, rgba(CYAN, 0.3));
salesGrad.addColorStop(1, rgba(CYAN, 0));
const profitGrad = monthlyCtx.createLinearGradient(0, 0, 0, 300);
profitGrad.addColorStop(0, rgba(GREEN, 0.3));
profitGrad.addColorStop(1, rgba(GREEN, 0));

new Chart(monthlyCtx, {
  type: 'line',
  data: {
    labels: DATA.monthly.map(d=>d.m),
    datasets: [
      {
        label: 'Sales (₹)',
        data: DATA.monthly.map(d=>d.s),
        borderColor: CYAN,
        backgroundColor: salesGrad,
        borderWidth: 2.5,
        pointBackgroundColor: CYAN,
        pointRadius: 4,
        pointHoverRadius: 6,
        fill: true,
        tension: 0.4,
        yAxisID: 'y'
      },
      {
        label: 'Profit (₹)',
        data: DATA.monthly.map(d=>d.p),
        borderColor: GREEN,
        backgroundColor: profitGrad,
        borderWidth: 2,
        pointBackgroundColor: GREEN,
        pointRadius: 3,
        pointHoverRadius: 5,
        fill: true,
        tension: 0.4,
        yAxisID: 'y1'
      },
      {
        label: 'Orders',
        data: DATA.monthly.map(d=>d.o),
        borderColor: ORANGE,
        backgroundColor: 'transparent',
        borderWidth: 1.5,
        borderDash: [5,3],
        pointBackgroundColor: ORANGE,
        pointRadius: 3,
        tension: 0.4,
        yAxisID: 'y2'
      }
    ]
  },
  options: {
    responsive: true,
    interaction: { mode: 'index', intersect: false },
    plugins: {
      legend: {
        display: true,
        position: 'top',
        align: 'end',
        labels: { color: '#94a3b8', boxWidth: 10, boxHeight: 10, padding: 16 }
      },
      tooltip: { ...tooltipStyle,
        callbacks: {
          label: ctx => {
            if(ctx.dataset.label === 'Orders') return ` Orders: ${ctx.parsed.y}`;
            return ` ${ctx.dataset.label}: ₹${ctx.parsed.y.toLocaleString('en-IN')}`;
          }
        }
      }
    },
    scales: {
      x: { grid: gridOpts, ticks: { color: '#64748b' } },
      y: { grid: gridOpts, ticks: { color: '#64748b', callback: v => '₹'+v/100000+'L' }, position: 'left' },
      y1: { grid: { display: false }, ticks: { color: GREEN, callback: v => '₹'+v/1000+'K' }, position: 'right' },
      y2: { display: false }
    }
  }
});

// ── Region Bar Chart ──
new Chart(document.getElementById('regionChart'), {
  type: 'bar',
  data: {
    labels: DATA.region.map(d=>d.r),
    datasets: [{
      label: 'Sales',
      data: DATA.region.map(d=>d.s),
      backgroundColor: COLORS.map(c=>rgba(c,0.8)),
      borderColor: COLORS,
      borderWidth: 1,
      borderRadius: 6
    }]
  },
  options: {
    responsive: true,
    plugins: { legend: noLegend, tooltip: { ...tooltipStyle, callbacks: { label: ctx=>' ₹'+ctx.parsed.y.toLocaleString('en-IN') } } },
    scales: {
      x: { grid: { display: false }, ticks: { color: '#64748b' } },
      y: { grid: gridOpts, ticks: { color: '#64748b', callback: v=>'₹'+v/100000+'L' } }
    }
  }
});

// ── Category Doughnut ──
new Chart(document.getElementById('categoryChart'), {
  type: 'doughnut',
  data: {
    labels: DATA.category.map(d=>d.c),
    datasets: [{
      data: DATA.category.map(d=>d.s),
      backgroundColor: [rgba(CYAN,0.8), rgba(ORANGE,0.8)],
      borderColor: [CYAN, ORANGE],
      borderWidth: 2,
      hoverOffset: 8
    }]
  },
  options: {
    responsive: true,
    cutout: '68%',
    plugins: {
      legend: { display: true, position: 'bottom', labels: { color: '#94a3b8', boxWidth: 10 } },
      tooltip: { ...tooltipStyle, callbacks: { label: ctx=>' ₹'+ctx.parsed.toLocaleString('en-IN') } }
    }
  }
});

// ── Segment Doughnut ──
new Chart(document.getElementById('segmentChart'), {
  type: 'doughnut',
  data: {
    labels: DATA.segment.map(d=>d.s),
    datasets: [{
      data: DATA.segment.map(d=>d.v),
      backgroundColor: [rgba(GREEN,0.8), rgba(PURPLE,0.8), rgba(GOLD,0.8)],
      borderColor: [GREEN, PURPLE, GOLD],
      borderWidth: 2,
      hoverOffset: 8
    }]
  },
  options: {
    responsive: true,
    cutout: '68%',
    plugins: {
      legend: { display: true, position: 'bottom', labels: { color: '#94a3b8', boxWidth: 10 } },
      tooltip: { ...tooltipStyle, callbacks: { label: ctx=>' ₹'+ctx.parsed.toLocaleString('en-IN') } }
    }
  }
});

// ── Quarterly Grouped Bar ──
new Chart(document.getElementById('quarterChart'), {
  type: 'bar',
  data: {
    labels: DATA.quarter.map(d=>d.q),
    datasets: [
      {
        label: 'Sales',
        data: DATA.quarter.map(d=>d.s),
        backgroundColor: rgba(CYAN, 0.75),
        borderColor: CYAN,
        borderWidth: 1,
        borderRadius: 5
      },
      {
        label: 'Profit',
        data: DATA.quarter.map(d=>d.p),
        backgroundColor: rgba(GREEN, 0.75),
        borderColor: GREEN,
        borderWidth: 1,
        borderRadius: 5
      }
    ]
  },
  options: {
    responsive: true,
    plugins: {
      legend: { display: true, position: 'top', align: 'end', labels: { color: '#94a3b8', boxWidth: 10 } },
      tooltip: { ...tooltipStyle, callbacks: { label: ctx=>' ₹'+ctx.parsed.y.toLocaleString('en-IN') } }
    },
    scales: {
      x: { grid: { display: false }, ticks: { color: '#64748b' } },
      y: { grid: gridOpts, ticks: { color: '#64748b', callback: v=>'₹'+v/100000+'L' } }
    }
  }
});

// ── SubCat Horizontal Bar ──
new Chart(document.getElementById('subcatChart'), {
  type: 'bar',
  data: {
    labels: DATA.subcat.map(d=>d.s),
    datasets: [{
      label: 'Sales',
      data: DATA.subcat.map(d=>d.v),
      backgroundColor: COLORS.map(c=>rgba(c,0.75)),
      borderColor: COLORS,
      borderWidth: 1,
      borderRadius: 4
    }]
  },
  options: {
    indexAxis: 'y',
    responsive: true,
    plugins: { legend: noLegend, tooltip: { ...tooltipStyle, callbacks: { label: ctx=>' ₹'+ctx.parsed.x.toLocaleString('en-IN') } } },
    scales: {
      x: { grid: gridOpts, ticks: { color: '#64748b', callback: v=>'₹'+v/100000+'L' } },
      y: { grid: { display: false }, ticks: { color: '#94a3b8' } }
    }
  }
});

// ── City Radar ──
new Chart(document.getElementById('cityChart'), {
  type: 'radar',
  data: {
    labels: DATA.city.map(d=>d.c),
    datasets: [
      {
        label: 'Sales',
        data: DATA.city.map(d=>d.s),
        backgroundColor: rgba(CYAN, 0.15),
        borderColor: CYAN,
        borderWidth: 2,
        pointBackgroundColor: CYAN,
        pointRadius: 4
      },
      {
        label: 'Profit',
        data: DATA.city.map(d=>d.p),
        backgroundColor: rgba(GREEN, 0.15),
        borderColor: GREEN,
        borderWidth: 2,
        pointBackgroundColor: GREEN,
        pointRadius: 3
      }
    ]
  },
  options: {
    responsive: true,
    plugins: {
      legend: { display: true, position: 'top', align: 'end', labels: { color: '#94a3b8', boxWidth: 10 } },
      tooltip: { ...tooltipStyle }
    },
    scales: {
      r: {
        grid: { color: 'rgba(255,255,255,0.05)' },
        angleLines: { color: 'rgba(255,255,255,0.05)' },
        ticks: { display: false },
        pointLabels: { color: '#94a3b8', font: { size: 11 } }
      }
    }
  }
});

// ── Payment Polar ──
new Chart(document.getElementById('paymentChart'), {
  type: 'polarArea',
  data: {
    labels: DATA.payment.map(d=>d.m),
    datasets: [{
      data: DATA.payment.map(d=>d.s),
      backgroundColor: [rgba(PURPLE,0.7), rgba(GOLD,0.7), rgba(CYAN,0.7)],
      borderColor: [PURPLE, GOLD, CYAN],
      borderWidth: 2
    }]
  },
  options: {
    responsive: true,
    plugins: {
      legend: { display: true, position: 'bottom', labels: { color: '#94a3b8', boxWidth: 10 } },
      tooltip: { ...tooltipStyle, callbacks: { label: ctx=>' ₹'+ctx.parsed.r.toLocaleString('en-IN') } }
    },
    scales: {
      r: {
        grid: { color: 'rgba(255,255,255,0.05)' },
        ticks: { display: false }
      }
    }
  }
});

// ── Scatter ──
new Chart(document.getElementById('scatterChart'), {
  type: 'scatter',
  data: {
    datasets: [{
      label: 'Monthly (Sales vs Profit)',
      data: DATA.monthly.map(d=>({ x: d.s, y: d.p, label: d.m })),
      backgroundColor: COLORS.map(c=>rgba(c,0.8)),
      borderColor: '#1a2235',
      borderWidth: 1,
      pointRadius: 8,
      pointHoverRadius: 10
    }]
  },
  options: {
    responsive: true,
    plugins: {
      legend: noLegend,
      tooltip: {
        ...tooltipStyle,
        callbacks: {
          label: ctx => ` ${ctx.raw.label}: Sales ₹${ctx.raw.x.toLocaleString('en-IN')} | Profit ₹${ctx.raw.y.toLocaleString('en-IN')}`
        }
      }
    },
    scales: {
      x: { grid: gridOpts, ticks: { color: '#64748b', callback: v=>'₹'+v/100000+'L' } },
      y: { grid: gridOpts, ticks: { color: '#64748b', callback: v=>'₹'+v/1000+'K' } }
    }
  }
});

// ═══════════════════════════════════════════════
// LEADERBOARD
// ═══════════════════════════════════════════════
const maxSale = Math.max(...DATA.city.map(d=>d.s));
const lbColors = [GOLD, '#94a3b8', '#cd7f32', CYAN, GREEN, ORANGE];
const rankClass = ['gold','silver','bronze','','',''];
const lbEl = document.getElementById('leaderboard');
lbEl.innerHTML = DATA.city.map((d,i)=>{
  const pct = (d.s/maxSale*100).toFixed(0);
  return `<div class="leaderboard-item">
    <div class="rank ${rankClass[i]}">${i+1}</div>
    <div style="flex:1">
      <div class="lb-name">${d.c}</div>
      <div class="lb-sub">${d.r} · ${d.o} orders</div>
      <div class="lb-bar-wrap" style="width:100%;margin-top:5px">
        <div class="lb-bar-bg"><div class="lb-bar-fill" style="width:${pct}%;background:${lbColors[i]}"></div></div>
      </div>
    </div>
    <div class="lb-val" style="color:${lbColors[i]}">₹${(d.s/100000).toFixed(1)}L</div>
  </div>`;
}).join('');

// ═══════════════════════════════════════════════
// DATA TABLE
// ═══════════════════════════════════════════════
const cityColors = { Delhi: CYAN, Kolkata: ORANGE, Lucknow: GREEN, Mumbai: PURPLE, Bangalore: GOLD, Chennai: PINK };
const regionColors = { North: CYAN, East: ORANGE, South: GREEN, West: PURPLE };

function getStatus(margin) {
  if(margin >= 0.12) return '<span class="status-pill sp-high">High</span>';
  if(margin >= 0.09) return '<span class="status-pill sp-mid">Mid</span>';
  return '<span class="status-pill sp-low">Low</span>';
}

let tableData = DATA.city.map(d=>({ ...d, margin: d.p/d.s }));
let sortCol = 2, sortAsc = false;

function renderTable(data) {
  const maxProfit = Math.max(...data.map(d=>d.p));
  document.getElementById('tableBody').innerHTML = data.map(d=>{
    const pct = (d.p/maxProfit*100).toFixed(0);
    const col = cityColors[d.c] || CYAN;
    return `<tr>
      <td><span class="city-tag"><span class="dot" style="background:${col}"></span>${d.c}</span></td>
      <td><span style="color:${regionColors[d.r]||CYAN};font-weight:600">${d.r}</span></td>
      <td style="font-weight:600;color:var(--text)">₹${d.s.toLocaleString('en-IN')}</td>
      <td style="color:${GREEN}">₹${d.p.toLocaleString('en-IN')}</td>
      <td style="color:${GOLD}">${(d.margin*100).toFixed(1)}%</td>
      <td>${d.o}</td>
      <td style="min-width:100px">
        <div class="profit-bar-cell">
          <div class="profit-bar-bg"><div class="profit-bar-fill" style="width:${pct}%"></div></div>
          <span style="font-size:10px;color:var(--muted)">${pct}%</span>
        </div>
      </td>
      <td>${getStatus(d.margin)}</td>
    </tr>`;
  }).join('');
}

function sortTable(col) {
  if(sortCol === col) sortAsc = !sortAsc;
  else { sortCol = col; sortAsc = false; }
  const keys = ['c','r','s','p','margin','o'];
  tableData.sort((a,b)=>{
    const av = typeof a[keys[col]] === 'string' ? a[keys[col]].localeCompare(b[keys[col]]) : a[keys[col]] - b[keys[col]];
    return sortAsc ? av : -av;
  });
  renderTable(tableData);
}

function filterTable() {
  const q = document.getElementById('searchBox').value.toLowerCase();
  renderTable(tableData.filter(d=>d.c.toLowerCase().includes(q)||d.r.toLowerCase().includes(q)));
}

renderTable(tableData);

// ═══════════════════════════════════════════════
// PAYMENT & SEGMENT SPLITS
// ═══════════════════════════════════════════════
const totalPaySales = DATA.payment.reduce((a,d)=>a+d.s,0);
const payColors = [PURPLE, GOLD, CYAN];
document.getElementById('paymentSplits').innerHTML = DATA.payment.map((d,i)=>{
  const pct = (d.s/totalPaySales*100).toFixed(1);
  return `<div class="split-item">
    <div>
      <div class="split-label">${d.m}</div>
      <div class="split-val">${d.o} orders</div>
      <div class="split-bar"><div class="split-bar-fill" style="width:${pct}%;background:${payColors[i]}"></div></div>
    </div>
    <div style="text-align:right;min-width:100px">
      <div class="split-pct" style="color:${payColors[i]}">${pct}%</div>
      <div class="split-val">₹${(d.s/100000).toFixed(1)}L</div>
    </div>
  </div>`;
}).join('');

const totalSegSales = DATA.segment.reduce((a,d)=>a+d.v,0);
const segColors = [GREEN, PURPLE, GOLD];
document.getElementById('segmentSplits').innerHTML = DATA.segment.map((d,i)=>{
  const pct = (d.v/totalSegSales*100).toFixed(1);
  return `<div class="split-item">
    <div>
      <div class="split-label">${d.s}</div>
      <div class="split-val">₹${(d.v/100000).toFixed(1)}L revenue</div>
      <div class="split-bar"><div class="split-bar-fill" style="width:${pct}%;background:${segColors[i]}"></div></div>
    </div>
    <div style="text-align:right;min-width:100px">
      <div class="split-pct" style="color:${segColors[i]}">${pct}%</div>
    </div>
  </div>`;
}).join('');

// ═══════════════════════════════════════════════
// FILTER FUNCTIONS (visual toggle only)
// ═══════════════════════════════════════════════
function filterData(region, btn) {
  document.querySelectorAll('.filter-btn').forEach(b=>{
    if(['All Regions','North','South','East','West'].includes(b.textContent.trim())) b.classList.remove('active');
  });
  btn.classList.add('active');

  // Update KPIs visually
  const filtered = region === 'all' ? DATA.city : DATA.city.filter(d=>d.r===region);
  const fs = filtered.reduce((a,d)=>a+d.s,0);
  const fp = filtered.reduce((a,d)=>a+d.p,0);
  const fo = filtered.reduce((a,d)=>a+d.o,0);
  document.getElementById('kpi-revenue').textContent = '₹'+fs.toLocaleString('en-IN');
  document.getElementById('kpi-profit').textContent = '₹'+fp.toLocaleString('en-IN');
  document.getElementById('kpi-orders').textContent = fo;
  document.getElementById('kpi-discount').textContent = '15.2%';

  // Highlight table
  tableData = region === 'all' ? DATA.city.map(d=>({...d,margin:d.p/d.s})) : DATA.city.filter(d=>d.r===region).map(d=>({...d,margin:d.p/d.s}));
  renderTable(tableData);
}

function filterQuarter(q, btn) {
  document.querySelectorAll('.filter-btn').forEach(b=>{
    if(['All Quarters','Q1','Q2','Q3','Q4'].includes(b.textContent.trim())) b.classList.remove('active');
  });
  btn.classList.add('active');

  if(q === 'all') {
    document.getElementById('kpi-orders').textContent = '500';
    return;
  }
  const qMap = {Q1:[0,1,2],Q2:[3,4,5],Q3:[6,7,8],Q4:[9,10,11]};
  const idxs = qMap[q];
  const qMonths = idxs.map(i=>DATA.monthly[i]);
  const qs = qMonths.reduce((a,d)=>a+d.s,0);
  const qp = qMonths.reduce((a,d)=>a+d.p,0);
  const qo = qMonths.reduce((a,d)=>a+d.o,0);
  document.getElementById('kpi-revenue').textContent = '₹'+qs.toLocaleString('en-IN');
  document.getElementById('kpi-profit').textContent = '₹'+qp.toLocaleString('en-IN');
  document.getElementById('kpi-orders').textContent = qo;
}
</script>
</body>
</html>
