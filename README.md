<!DOCTYPE html>
<html lang="hu">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sakk Megnyitások – Tanuló & Gyakorló</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0f1923;
  --bg2: #162030;
  --bg3: #1e2d3e;
  --gold: #d4a843;
  --gold2: #e8c56a;
  --text: #e8e4dc;
  --text2: #9aa8b8;
  --light-sq: #f0d9b5;
  --dark-sq: #b58863;
  --highlight: #5d8a5e;
  --highlight2: #7ab87c;
  --error: #c0392b;
  --success: #27ae60;
  --border: #253545;
  --card-bg: #162030;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: 'Inter', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  overflow-x: hidden;
}

/* LAYOUT */
.app { display: flex; min-height: 100vh; }
.sidebar {
  width: 260px;
  background: var(--bg2);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0; left: 0; bottom: 0;
  z-index: 100;
  transition: transform 0.3s ease;
  overflow-y: auto;
}
.sidebar.closed { transform: translateX(-260px); }
.main { margin-left: 260px; flex: 1; min-height: 100vh; transition: margin-left 0.3s; }
.main.full { margin-left: 0; }

/* SIDEBAR */
.sidebar-logo {
  padding: 24px 20px 16px;
  border-bottom: 1px solid var(--border);
}
.sidebar-logo .logo-icon { font-size: 28px; margin-bottom: 6px; }
.sidebar-logo h1 {
  font-family: 'Playfair Display', serif;
  font-size: 16px;
  font-weight: 700;
  color: var(--gold);
  line-height: 1.2;
}
.sidebar-logo p { font-size: 11px; color: var(--text2); margin-top: 3px; }

.progress-bar-wrap { padding: 16px 20px; border-bottom: 1px solid var(--border); }
.progress-label { font-size: 11px; color: var(--text2); margin-bottom: 6px; display: flex; justify-content: space-between; }
.progress-bar { background: var(--bg3); border-radius: 4px; height: 6px; overflow: hidden; }
.progress-fill { height: 100%; background: linear-gradient(90deg, var(--gold), var(--gold2)); border-radius: 4px; transition: width 0.5s ease; }

.sidebar-search { padding: 12px 20px; border-bottom: 1px solid var(--border); }
.sidebar-search input {
  width: 100%; padding: 8px 12px;
  background: var(--bg3); border: 1px solid var(--border);
  border-radius: 6px; color: var(--text);
  font-size: 13px; font-family: 'Inter', sans-serif;
  outline: none; transition: border-color 0.2s;
}
.sidebar-search input:focus { border-color: var(--gold); }

.sidebar-cats { flex: 1; overflow-y: auto; padding: 8px 0; }
.cat-group { margin-bottom: 4px; }
.cat-header {
  padding: 8px 20px; font-size: 10px; font-weight: 600;
  text-transform: uppercase; letter-spacing: 1px;
  color: var(--text2); cursor: pointer;
  display: flex; justify-content: space-between; align-items: center;
}
.cat-header:hover { color: var(--gold); }
.cat-items { overflow: hidden; }
.sidebar-item {
  padding: 8px 20px 8px 28px;
  font-size: 13px; cursor: pointer;
  color: var(--text2); transition: all 0.15s;
  display: flex; align-items: center; gap: 8px;
  border-left: 2px solid transparent;
}
.sidebar-item:hover { color: var(--text); background: var(--bg3); }
.sidebar-item.active { color: var(--gold); border-left-color: var(--gold); background: rgba(212,168,67,0.07); }
.sidebar-item.done { color: var(--success); }
.sidebar-item .check { font-size: 11px; margin-left: auto; }

/* TOPBAR */
.topbar {
  display: flex; align-items: center; gap: 12px;
  padding: 14px 24px;
  border-bottom: 1px solid var(--border);
  background: var(--bg2);
  position: sticky; top: 0; z-index: 50;
}
.menu-btn {
  background: none; border: none; color: var(--text);
  font-size: 20px; cursor: pointer; padding: 4px;
  display: none;
}
.topbar-title { font-family: 'Playfair Display', serif; font-size: 18px; }
.topbar-badge {
  padding: 4px 10px; border-radius: 20px;
  font-size: 11px; font-weight: 600;
  background: var(--bg3); color: var(--text2);
}
.topbar-badge.beginner { background: rgba(39,174,96,0.2); color: #5dbd7a; }
.topbar-badge.intermediate { background: rgba(212,168,67,0.2); color: var(--gold); }
.topbar-badge.advanced { background: rgba(192,57,43,0.2); color: #e74c3c; }
.topbar-actions { margin-left: auto; display: flex; gap: 8px; }
.btn {
  padding: 8px 16px; border-radius: 6px;
  font-size: 13px; font-weight: 500; cursor: pointer;
  border: none; transition: all 0.2s; font-family: 'Inter', sans-serif;
}
.btn-gold { background: var(--gold); color: #0f1923; }
.btn-gold:hover { background: var(--gold2); }
.btn-outline { background: transparent; border: 1px solid var(--border); color: var(--text2); }
.btn-outline:hover { border-color: var(--gold); color: var(--gold); }
.btn-outline.active-mode { border-color: var(--gold); color: var(--gold); background: rgba(212,168,67,0.1); }

/* HOME SCREEN */
#home-screen { padding: 32px 28px; }
.home-hero {
  text-align: center; padding: 40px 20px 32px;
  border-bottom: 1px solid var(--border);
  margin-bottom: 32px;
}
.home-hero .big-piece { font-size: 64px; line-height: 1; margin-bottom: 16px; filter: drop-shadow(0 4px 12px rgba(212,168,67,0.3)); }
.home-hero h2 {
  font-family: 'Playfair Display', serif;
  font-size: 32px; color: var(--text);
  margin-bottom: 10px;
}
.home-hero p { font-size: 15px; color: var(--text2); max-width: 480px; margin: 0 auto 20px; line-height: 1.6; }
.stats-row { display: flex; justify-content: center; gap: 32px; margin-top: 20px; }
.stat-item { text-align: center; }
.stat-num { font-family: 'Playfair Display', serif; font-size: 28px; color: var(--gold); }
.stat-label { font-size: 11px; color: var(--text2); margin-top: 2px; }

.filter-row { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 20px; align-items: center; }
.filter-btn {
  padding: 6px 14px; border-radius: 20px; font-size: 12px;
  cursor: pointer; border: 1px solid var(--border);
  background: transparent; color: var(--text2);
  transition: all 0.2s; font-family: 'Inter', sans-serif;
}
.filter-btn:hover, .filter-btn.active { background: var(--gold); color: #0f1923; border-color: var(--gold); font-weight: 600; }

.cat-section { margin-bottom: 36px; }
.cat-title {
  font-family: 'Playfair Display', serif;
  font-size: 20px; color: var(--text);
  margin-bottom: 16px;
  display: flex; align-items: center; gap: 10px;
}
.cat-title::after { content: ''; flex: 1; height: 1px; background: var(--border); }

.cards-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 16px; }
.card {
  background: var(--card-bg);
  border: 1px solid var(--border);
  border-radius: 10px; padding: 18px;
  cursor: pointer; transition: all 0.2s;
  position: relative; overflow: hidden;
}
.card:hover { border-color: var(--gold); transform: translateY(-2px); box-shadow: 0 8px 24px rgba(0,0,0,0.3); }
.card.completed { border-color: rgba(93,138,94,0.5); }
.card-done-badge {
  position: absolute; top: 12px; right: 12px;
  background: var(--success); color: white;
  border-radius: 50%; width: 22px; height: 22px;
  display: flex; align-items: center; justify-content: center;
  font-size: 12px;
}
.card-name {
  font-family: 'Playfair Display', serif;
  font-size: 16px; margin-bottom: 6px; color: var(--text);
  padding-right: 28px;
}
.card-desc { font-size: 12px; color: var(--text2); line-height: 1.5; margin-bottom: 12px; }
.card-meta { display: flex; justify-content: space-between; align-items: center; }
.diff-badge {
  font-size: 10px; font-weight: 600; padding: 3px 8px;
  border-radius: 4px; text-transform: uppercase; letter-spacing: 0.5px;
}
.diff-badge.kezdő { background: rgba(39,174,96,0.2); color: #5dbd7a; }
.diff-badge.haladó { background: rgba(212,168,67,0.2); color: var(--gold); }
.diff-badge.mester { background: rgba(192,57,43,0.2); color: #e74c3c; }
.card-steps { font-size: 11px; color: var(--text2); }
.card-learn-btn {
  width: 100%; margin-top: 12px; padding: 8px;
  background: var(--bg3); border: 1px solid var(--border);
  border-radius: 6px; color: var(--text2);
  font-size: 12px; font-weight: 500; cursor: pointer;
  transition: all 0.2s; font-family: 'Inter', sans-serif;
}
.card-learn-btn:hover { background: var(--gold); color: #0f1923; border-color: var(--gold); }

/* LEARNING / PRACTICE SCREEN */
#learn-screen { display: none; }
.learn-layout { display: grid; grid-template-columns: 1fr 340px; gap: 0; min-height: calc(100vh - 57px); }
.board-area { padding: 28px; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; }
.board-wrap { position: relative; }

canvas#chessboard {
  border-radius: 4px;
  cursor: pointer;
  box-shadow: 0 16px 48px rgba(0,0,0,0.5);
  display: block;
}
.board-coords-file {
  display: flex; justify-content: space-around;
  padding: 4px 0; width: 100%;
}
.board-coords-file span, .board-coord-rank span {
  font-size: 11px; color: var(--text2); font-weight: 500;
}
.board-coord-rank {
  position: absolute; left: -18px; top: 0; bottom: 0;
  display: flex; flex-direction: column; justify-content: space-around;
}

.notation-area {
  margin-top: 20px; width: 100%;
  max-width: 480px;
  background: var(--bg2); border: 1px solid var(--border);
  border-radius: 8px; padding: 14px 16px;
}
.notation-title { font-size: 11px; color: var(--text2); margin-bottom: 8px; text-transform: uppercase; letter-spacing: 1px; }
.notation-moves { display: flex; flex-wrap: wrap; gap: 4px; }
.notation-move {
  font-size: 13px; padding: 3px 7px; border-radius: 4px;
  cursor: pointer; transition: background 0.15s;
  font-family: 'Inter', sans-serif;
}
.notation-move:hover { background: var(--bg3); }
.notation-move.current { background: var(--gold); color: #0f1923; font-weight: 600; }
.notation-move.passed { color: var(--text2); }
.notation-move.move-num { color: var(--text2); font-size: 12px; }

.side-panel {
  background: var(--bg2);
  border-left: 1px solid var(--border);
  display: flex; flex-direction: column;
  overflow: hidden;
}
.panel-tabs { display: flex; border-bottom: 1px solid var(--border); }
.panel-tab {
  flex: 1; padding: 14px 10px; text-align: center;
  font-size: 13px; font-weight: 500; cursor: pointer;
  color: var(--text2); border-bottom: 2px solid transparent;
  transition: all 0.2s;
}
.panel-tab.active { color: var(--gold); border-bottom-color: var(--gold); }
.panel-content { flex: 1; overflow-y: auto; padding: 20px; }

/* EXPLANATION PANEL */
.explanation-box {
  background: var(--bg3); border-radius: 8px; padding: 16px;
  margin-bottom: 16px; border-left: 3px solid var(--gold);
  animation: slideIn 0.3s ease;
}
@keyframes slideIn { from { opacity: 0; transform: translateX(10px); } to { opacity: 1; transform: translateX(0); } }
.expl-move { font-family: 'Playfair Display', serif; font-size: 18px; color: var(--gold); margin-bottom: 8px; }
.expl-text { font-size: 13px; color: var(--text); line-height: 1.65; }
.expl-tip { margin-top: 10px; font-size: 12px; color: var(--text2); font-style: italic; }

.move-controls { display: flex; gap: 8px; margin-bottom: 16px; }
.move-controls .btn { flex: 1; font-size: 13px; }
.btn-prev { background: var(--bg3); border: 1px solid var(--border); color: var(--text); }
.btn-prev:hover { border-color: var(--gold); color: var(--gold); }
.btn-next { background: var(--gold); color: #0f1923; }
.btn-next:hover { background: var(--gold2); }

/* MOVES LIST */
.moves-list { display: flex; flex-direction: column; gap: 4px; }
.move-item {
  padding: 8px 12px; border-radius: 6px;
  font-size: 13px; cursor: pointer;
  display: flex; align-items: center; gap: 8px;
  color: var(--text2); transition: all 0.15s;
}
.move-item:hover { background: var(--bg3); color: var(--text); }
.move-item.current { background: rgba(212,168,67,0.15); color: var(--gold); font-weight: 500; }
.move-item.passed { color: var(--text); }
.move-item .move-idx { font-size: 11px; color: var(--text2); width: 20px; }
.move-item .move-notation { font-weight: 500; }
.move-item .move-piece { font-size: 16px; }

/* PRACTICE PANEL */
.practice-status {
  padding: 12px 16px; border-radius: 8px; margin-bottom: 16px;
  font-size: 13px; font-weight: 500; text-align: center;
  transition: all 0.3s;
}
.practice-status.waiting { background: var(--bg3); color: var(--text2); }
.practice-status.correct { background: rgba(39,174,96,0.2); color: #5dbd7a; }
.practice-status.wrong { background: rgba(192,57,43,0.2); color: #e74c3c; }
.practice-status.hint { background: rgba(212,168,67,0.15); color: var(--gold); }

.hint-btn {
  width: 100%; padding: 10px; border-radius: 6px;
  background: transparent; border: 1px dashed var(--border);
  color: var(--text2); font-size: 13px; cursor: pointer;
  transition: all 0.2s; font-family: 'Inter', sans-serif;
  margin-bottom: 12px;
}
.hint-btn:hover { border-color: var(--gold); color: var(--gold); }

.completion-box {
  background: linear-gradient(135deg, rgba(212,168,67,0.15), rgba(93,138,94,0.15));
  border: 1px solid var(--gold);
  border-radius: 10px; padding: 24px; text-align: center;
  display: none;
}
.completion-box.show { display: block; animation: slideIn 0.4s ease; }
.completion-icon { font-size: 40px; margin-bottom: 10px; }
.completion-title { font-family: 'Playfair Display', serif; font-size: 20px; color: var(--gold); margin-bottom: 8px; }
.completion-text { font-size: 13px; color: var(--text2); margin-bottom: 16px; }

/* OVERLAY / MOBILE */
.sidebar-overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(0,0,0,0.6); z-index: 99;
}
.sidebar-overlay.show { display: block; }

/* RESPONSIVE */
@media (max-width: 900px) {
  .learn-layout { grid-template-columns: 1fr; }
  .side-panel { border-left: none; border-top: 1px solid var(--border); }
}
@media (max-width: 700px) {
  .sidebar { transform: translateX(-260px); }
  .sidebar.open { transform: translateX(0); }
  .main { margin-left: 0 !important; }
  .menu-btn { display: block; }
  .cards-grid { grid-template-columns: 1fr; }
  #home-screen { padding: 20px 16px; }
  .board-area { padding: 16px 8px; }
  .stats-row { gap: 20px; }
}

/* SCROLLBAR */
::-webkit-scrollbar { width: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

.opening-info {
  background: var(--bg3); border-radius: 8px; padding: 14px;
  margin-bottom: 16px; font-size: 12px; color: var(--text2); line-height: 1.6;
}
.opening-info strong { color: var(--text); display: block; margin-bottom: 4px; }

.practice-prompt {
  font-size: 13px; color: var(--text2); margin-bottom: 12px;
  padding: 10px 14px; background: var(--bg3); border-radius: 6px; line-height: 1.5;
}

.color-choice { display: flex; gap: 8px; margin-bottom: 16px; }
.color-btn {
  flex: 1; padding: 10px; border-radius: 6px; cursor: pointer;
  border: 1px solid var(--border); background: var(--bg3);
  color: var(--text); font-size: 13px; font-family: 'Inter', sans-serif;
  transition: all 0.2s; text-align: center;
}
.color-btn:hover { border-color: var(--gold); }
.color-btn.active { border-color: var(--gold); background: rgba(212,168,67,0.15); color: var(--gold); }
</style>
</head>
<body>
<div class="app">

<!-- SIDEBAR -->
<nav class="sidebar" id="sidebar">
  <div class="sidebar-logo">
    <div class="logo-icon">♚</div>
    <h1>Sakk Megnyitások</h1>
    <p>Tanulj és gyakorolj lépésről lépésre</p>
  </div>
  <div class="progress-bar-wrap">
    <div class="progress-label">
      <span>Haladás</span>
      <span id="progress-text">0 / 50</span>
    </div>
    <div class="progress-bar"><div class="progress-fill" id="progress-fill" style="width:0%"></div></div>
  </div>
  <div class="sidebar-search">
    <input type="text" id="sidebar-search" placeholder="Keresés..." oninput="filterSidebar(this.value)">
  </div>
  <div class="sidebar-cats" id="sidebar-cats"></div>
</nav>
<div class="sidebar-overlay" id="sidebar-overlay" onclick="toggleSidebar()"></div>

<!-- MAIN -->
<main class="main" id="main-area">

  <!-- TOPBAR -->
  <div class="topbar">
    <button class="menu-btn" onclick="toggleSidebar()">☰</button>
    <div class="topbar-title" id="topbar-title">Sakk Megnyitások</div>
    <span class="topbar-badge" id="topbar-badge" style="display:none"></span>
    <div class="topbar-actions" id="topbar-actions">
      <button class="btn btn-outline" onclick="goHome()">← Vissza</button>
    </div>
  </div>

  <!-- HOME -->
  <div id="home-screen">
    <div class="home-hero">
      <div class="big-piece">♛</div>
      <h2>Ismerd meg a megnyitásokat</h2>
      <p>50 klasszikus sakk megnyitás és gambit – lépésről lépésre, magyarázatokkal, gyakorlással.</p>
      <div class="stats-row">
        <div class="stat-item"><div class="stat-num">50</div><div class="stat-label">Megnyitás</div></div>
        <div class="stat-item"><div class="stat-num" id="stat-done">0</div><div class="stat-label">Teljesítve</div></div>
        <div class="stat-item"><div class="stat-num">4</div><div class="stat-label">Kategória</div></div>
      </div>
    </div>
    <div class="filter-row">
      <button class="filter-btn active" onclick="setFilter('all', this)">Összes</button>
      <button class="filter-btn" onclick="setFilter('Nyílt játékok', this)">Nyílt játékok</button>
      <button class="filter-btn" onclick="setFilter('Zárt játékok', this)">Zárt játékok</button>
      <button class="filter-btn" onclick="setFilter('Flank megnyitások', this)">Flank</button>
      <button class="filter-btn" onclick="setFilter('Gambitek', this)">Gambitek</button>
      <button class="filter-btn" onclick="setFilter('done', this)">✓ Teljesített</button>
    </div>
    <div id="openings-container"></div>
  </div>

  <!-- LEARN / PRACTICE -->
  <div id="learn-screen">
    <div class="learn-layout">
      <div class="board-area">
        <div class="board-wrap" id="board-wrap">
          <div class="board-coord-rank" id="rank-labels"></div>
          <canvas id="chessboard" width="480" height="480"></canvas>
          <div class="board-coords-file" id="file-labels"></div>
        </div>
        <div class="notation-area">
          <div class="notation-title">Lépések jelölése</div>
          <div class="notation-moves" id="notation-moves"></div>
        </div>
      </div>
      <div class="side-panel">
        <div class="panel-tabs">
          <div class="panel-tab active" id="tab-learn" onclick="switchTab('learn')">📖 Tanulás</div>
          <div class="panel-tab" id="tab-practice" onclick="switchTab('practice')">🎯 Gyakorlás</div>
        </div>
        <div class="panel-content" id="panel-content"></div>
      </div>
    </div>
  </div>

</main>
</div>

<script>
// ============================================================
// DATA: 50 OPENINGS
// ============================================================
const openings = [
// ---- NYÍLT JÁTÉKOK ----
{
  id:"ruy-lopez", name:"Spanyol játék (Ruy López)", category:"Nyílt játékok", difficulty:"haladó",
  shortDesc:"Az egyik legmélyebben elemzett megnyitás. Fehér tartós nyomást gyakorol e5-re.",
  info:"A Ruy López 1490 körül lett leírva Ruy López de Segura spanyol pap által. Azóta a leggyakrabban játszott e4–e5 megnyitás.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrumkontroll és átlók megnyitása a vezérnek és a futónak. Az egyik legerősebb első lépés."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Fekete szimmetrikusan veszi át a centrum kontrollját. Ez a legfőbb válasz e4-re."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"A huszár e5 gyalogot támadja, aktív pozícióba kerül, és megkészíti a rövid sáncolást."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"A huszár védi e5-öt és kontrollál d4-et. Természetes, fejlesztő lépés."},
    {from:"f1",to:"b5",notation:"3.Bb5",color:"w",explanation:"A spanyol futó! Közvetetten nyomást gyakorol e5-re: ha c6 huszár elmegy, e5 gyalog védetlenné válik."},
    {from:"a7",to:"a6",notation:"3...a6",color:"b",explanation:"Morphy-védelemben fekete elküldi a futót. Teret nyer, de időt veszít."},
    {from:"b5",to:"a4",notation:"4.Ba4",color:"w",explanation:"A futó visszalép a6 gyalog elől, de megtartja a nyomást c6 huszárra."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Aktív fejlesztés – a huszár e4-et fenyegeti, és ellennyomást tesz a centrum ellen."},
    {from:"e1",to:"g1",notation:"5.O-O",color:"w",explanation:"Rövid sáncolás: fehér királya biztonságba kerül, a bástya aktív pozícióba jut."},
    {from:"f6",to:"e4",notation:"5...Nxe4",color:"b",explanation:"A berlini vagy nyílt változatban fekete bátran megszerzi e4-et. Ez az 'Open Ruy López'."},
  ]
},
{
  id:"italian-game", name:"Olasz játék", category:"Nyílt játékok", difficulty:"kezdő",
  shortDesc:"Klasszikus centrumépítő megnyitás. Ideális a kezdők számára.",
  info:"Az Olasz játék a legrégebbi megnyitások egyike. Gyors fejlesztés és erős centrumellenőrzés a cél.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrumkontroll, átlók megnyitása."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetrikus centrum – fekete egyenlő pályán áll."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Huszár fejlesztés, e5 fenyegetés."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"e5 védelem, természetes fejlesztés."},
    {from:"f1",to:"c4",notation:"3.Bc4",color:"w",explanation:"Az olasz futó! f7-et szemmel tartja – a fekete király melletti leggyengébb pont."},
    {from:"f8",to:"c5",notation:"3...Bc5",color:"b",explanation:"Fekete is aktívan fejleszt, f2-re néz, szimmetrikus pozíció."},
    {from:"c2",to:"c3",notation:"4.c3",color:"w",explanation:"Készíti d4-et – erős centrumépítés a következő lépésben."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés, egyben e4-et fenyegeti."},
    {from:"d2",to:"d4",notation:"5.d4",color:"w",explanation:"Hatalmas centrumlecsapás! Fehér dominál a centrum felett."},
    {from:"e5",to:"d4",notation:"5...exd4",color:"b",explanation:"Fekete megszerzi a gyalogot, de fehér nyílt pozíciót kap."},
  ]
},
{
  id:"giuoco-piano", name:"Giuoco Piano", category:"Nyílt játékok", difficulty:"kezdő",
  shortDesc:"Az 'Csendes játék' – lassú, pozicionális felépítés az olasz rendszerben.",
  info:"A Giuoco Piano az Olasz játék egyik fő változata. A 'csendes játék' nevet onnan kapta, hogy nem törekszenek korai gambitre.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum és átlók megnyitása."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Egyenlő centrum."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés, e5 támadás."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Védi e5-öt."},
    {from:"f1",to:"c4",notation:"3.Bc4",color:"w",explanation:"Olasz futó, f7 nyomás."},
    {from:"f8",to:"c5",notation:"3...Bc5",color:"b",explanation:"Szimmetrikus futó fejlesztés."},
    {from:"c2",to:"c3",notation:"4.c3",color:"w",explanation:"d4 előkészítése."},
    {from:"d7",to:"d6",notation:"4...d6",color:"b",explanation:"Védelem, a futó megtámasztása. Szilárd felépítés."},
    {from:"d2",to:"d4",notation:"5.d4",color:"w",explanation:"Centrumtámadás – fehér vállalja a harcot."},
    {from:"c5",to:"b6",notation:"5...Bb6",color:"b",explanation:"A futó biztonságba húzódik, de megtartja az átlót."},
  ]
},
{
  id:"four-knights", name:"Négy huszáros játék", category:"Nyílt játékok", difficulty:"kezdő",
  shortDesc:"Mindkét oldal 4 huszárt fejleszt az első négy lépésben. Szimmetrikus és kiegyensúlyozott.",
  info:"A Négy huszáros játék szimmetrikus és egyenletes. Jó tanulási alap a fejlesztési elvek miatt.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum megnyitás."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetrikus válasz."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Huszár fejlesztés."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Huszár fejlesztés."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Második huszár fejlesztés – a négy huszár szimmetria felé."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Fekete is fejleszti a negyedik huszárt – szimmetria kialakul."},
    {from:"f1",to:"b5",notation:"4.Bb5",color:"w",explanation:"Spanyol-jellegű futó – a huszárt köti, nyomást ad."},
    {from:"f8",to:"b4",notation:"4...Bb4",color:"b",explanation:"Szimmetrikus futó fejlesztés fekétől."},
    {from:"e1",to:"g1",notation:"5.O-O",color:"w",explanation:"Sáncolás – biztonság és aktivizálás."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Fekete is sáncol szimmetrikusan."},
  ]
},
{
  id:"scottish-game", name:"Skót játék", category:"Nyílt játékok", difficulty:"haladó",
  shortDesc:"Fehér gyorsan d4-el nyit, éles centrumharcot generálva.",
  info:"A Skót játékot Kasparov kedvenc fegyverként használta a fehér oldalon a 90-es években.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum megnyitás."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetrikus válasz."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés, e5 támadás."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Védi e5-öt."},
    {from:"d2",to:"d4",notation:"3.d4",color:"w",explanation:"A skót nyitás! Azonnal centrumharcot provokál."},
    {from:"e5",to:"d4",notation:"3...exd4",color:"b",explanation:"Fekete elfogadja a cserét – megnyílik a pozíció."},
    {from:"f3",to:"d4",notation:"4.Nxd4",color:"w",explanation:"A huszár visszahódítja a gyalogot, aktív pozícióba kerül."},
    {from:"f8",to:"c5",notation:"4...Bc5",color:"b",explanation:"Klasszikus – a futó d4 huszárt fenyegeti."},
    {from:"d4",to:"c6",notation:"5.Nxc6",color:"w",explanation:"Cserevariáns – nyílt b-vonal, de fekete pár jó futót kap."},
    {from:"d8",to:"f6",notation:"5...Qf6",color:"b",explanation:"Vezér fejlesztés – egyszerre védi a leesett gyalogot és fenyeget f2-re."},
  ]
},
{
  id:"petrov", name:"Petrov-védelem", category:"Nyílt játékok", difficulty:"haladó",
  shortDesc:"Fekete 2...Nf6-tal ellentámad e4-re – szimmetriát és soliditást keres.",
  info:"A Petrov-védelem rendkívül szilárd – sok nagymester választja remis-biztonság végett.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"e5 támadás."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Petrov! Fekete nem védi e5-öt, hanem ellentámad e4-re."},
    {from:"f3",to:"e5",notation:"3.Nxe5",color:"w",explanation:"Fehér leüti e5-öt."},
    {from:"d7",to:"d6",notation:"3...d6",color:"b",explanation:"A huszár elűzése e5-ről."},
    {from:"e5",to:"f3",notation:"4.Nf3",color:"w",explanation:"Visszalép a huszár, megtartva az anyagi egyensúlyt."},
    {from:"f6",to:"e4",notation:"4...Nxe4",color:"b",explanation:"Fekete most leüti e4-et – kiegyenlített anyagi helyzet."},
    {from:"d2",to:"d4",notation:"5.d4",color:"w",explanation:"Fehér centrumnyomást épít, fejlesztésre készül."},
    {from:"d6",to:"d5",notation:"5...d5",color:"b",explanation:"Szimmetrikus centrumtartás – jellegzetesen tömör Petrov-struktúra."},
  ]
},
{
  id:"kings-gambit", name:"Királygambit", category:"Nyílt játékok", difficulty:"haladó",
  shortDesc:"Fehér f2-f4-gyel gambitet kínál, robbanékony támadást nyitva.",
  info:"A Királygambit a romantikus sakk szimbóluma. Fischer is sokat foglalkozott ezzel és cikket írt róla.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"f2",to:"f4",notation:"2.f4",color:"w",explanation:"A Királygambit! f4 gyalogot ajánlja fel a centrumért és f-vonalon gyors támadásért."},
    {from:"e5",to:"f4",notation:"2...exf4",color:"b",explanation:"Fekete elfogadja! Gyalogot nyer, de fehér nyílt f-vonalat és kontrollt kap."},
    {from:"g1",to:"f3",notation:"3.Nf3",color:"w",explanation:"Fejlesztés és g4 megelőzése – fekete nem tolhatja tovább a gyalogot."},
    {from:"g7",to:"g5",notation:"3...g5",color:"b",explanation:"Fekete tartja a gambitugyalogot és tolni kezdi a g-vonalat."},
    {from:"h2",to:"h4",notation:"4.h4",color:"w",explanation:"Fehér ostromolja g5-öt és megpróbálja szétnyitni fekete királyoldalát."},
    {from:"g5",to:"g4",notation:"4...g4",color:"b",explanation:"Fekete eltolja a gyalogot – f3 huszárt fenyegeti."},
    {from:"f3",to:"e5",notation:"5.Ne5",color:"w",explanation:"A huszár erős centrumba ugrik – g4-et fenyegeti, de d7-re is rátekint."},
    {from:"d8",to:"h4",notation:"5...Qh4+",color:"b",explanation:"Sakk a vezérrel! Fekete élesen játszik és fehér királyát megtámadja."},
  ]
},
{
  id:"vienna-game", name:"Bécsi játék", category:"Nyílt játékok", difficulty:"haladó",
  shortDesc:"2.Nc3 – az e4 gyalog megtámasztása, rugalmas és erős felépítés.",
  info:"A Bécsi játék lassabb tempóban épít fel centrumtámadást, f4 lehetőségét tartva fenn.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"b1",to:"c3",notation:"2.Nc3",color:"w",explanation:"Bécsi játék – az e4 gyalog megtámasztása, f4 lehetőségének megőrzése."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Aktív fejlesztés, e4-et fenyegeti."},
    {from:"f2",to:"f4",notation:"3.f4",color:"w",explanation:"Bécsi gambit! Fehér f-vonalat nyit, ám gyalogot nem ajánl."},
    {from:"d7",to:"d5",notation:"3...d5",color:"b",explanation:"Ellentámadás a centrum ellen – erős és szimmetrikus."},
    {from:"f4",to:"e5",notation:"4.fxe5",color:"w",explanation:"Fehér elfoglalja e5-öt, de elzárja a futójának az útját."},
    {from:"f6",to:"e4",notation:"4...Nxe4",color:"b",explanation:"A huszár befészkeli magát e4-be – kiváló pozíció."},
    {from:"d1",to:"f3",notation:"5.Qf3",color:"w",explanation:"Vezér fenyegeti e4 huszárt és f7-et is – aktív centrumjáték."},
    {from:"e4",to:"c3",notation:"5...Nxc3",color:"b",explanation:"Huszár cserével fekete duplaggyalogot ad fehérnek, de megduplázza a gyalogokat."},
  ]
},
{
  id:"caro-kann", name:"Caro-Kann védelem", category:"Nyílt játékok", difficulty:"haladó",
  shortDesc:"1...c6 szilárd védelemmel fekete tartós pozicionális egyenlőséget szerez.",
  info:"A Caro-Kann előnye, hogy a c8 futó nem zárul be, mint a Francia védelemben. Petrosian és Karpov kedvence volt.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c6",notation:"1...c6",color:"b",explanation:"Caro-Kann! Felkészíti d5-öt a centrumtámadásra anélkül, hogy befogná a futóját."},
    {from:"d2",to:"d4",notation:"2.d4",color:"w",explanation:"Fehér is centrumot épít – kialakul az e4-d4 ék."},
    {from:"d7",to:"d5",notation:"2...d5",color:"b",explanation:"Centrumtámadás! Fekete megkérdőjelezi fehér centrumát."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Megtámasztja e4-et és huszárt fejleszt."},
    {from:"d5",to:"e4",notation:"3...dxe4",color:"b",explanation:"Fekete cserél – az egyik szilárd változat."},
    {from:"c3",to:"e4",notation:"4.Nxe4",color:"w",explanation:"Fehér nyílt vonalakkal folytatja – huszár az e4 erős ponton."},
    {from:"b8",to:"d7",notation:"4...Nd7",color:"b",explanation:"Fejlesztés – c6-ra készül a huszár, tartja a feszes szerkezetet."},
    {from:"g1",to:"f3",notation:"5.Nf3",color:"w",explanation:"Második huszár fejlesztés, sáncolásra készül."},
    {from:"g8",to:"f6",notation:"5...Ngf6",color:"b",explanation:"Fejtő fejlesztés – a huszárok kontrollálják a középet."},
  ]
},
{
  id:"french-defense", name:"Francia védelem", category:"Nyílt játékok", difficulty:"haladó",
  shortDesc:"1...e6 zárja az utat a c8 futónak, de szilárd centrumstruktúrát épít.",
  info:"A Francia védelmet Nimzowitsch és Botvinnik nevéhez kötik. Erős és stratégiai jellegű.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e6",notation:"1...e6",color:"b",explanation:"Francia védelem – a d5 centrumtámadásra készül."},
    {from:"d2",to:"d4",notation:"2.d4",color:"w",explanation:"Centrumépítés."},
    {from:"d7",to:"d5",notation:"2...d5",color:"b",explanation:"Centrumtámadás – fekete egyenlő centrumot akar."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Tarrasch-variáns – e4 megtámasztása."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Klasszikus fejlesztés, e4-et fenyegeti."},
    {from:"c1",to:"g5",notation:"4.Bg5",color:"w",explanation:"Nimzo-variáns – az f6 huszárt köti, nyomást ad a pozícióra."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Felkészíti a sáncolást, kilép a kötés alól."},
    {from:"e4",to:"e5",notation:"5.e5",color:"w",explanation:"Blanc előrenyomul – huszárt távolít el, és területi előnyt szerez."},
    {from:"f6",to:"d7",notation:"5...Nfd7",color:"b",explanation:"A huszár visszalép és a g5 futóra vár – Francia zártvédelmi struktúra."},
  ]
},
// ---- ZÁRT JÁTÉKOK ----
{
  id:"queens-gambit", name:"Dáma-gambit", category:"Zárt játékok", difficulty:"haladó",
  shortDesc:"Fehér c4-gyel gambitot kínál, centrumkontrollért. A zárt játékok egyik leghíresebb megnyitása.",
  info:"A Dáma-gambit a legismertebb zárt megnyitás. 1.d4 d5 2.c4 – fehér nem valódi gambit, hiszen a gyalogot könnyen visszanyeri.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt játék – fehér a d-vonal mentén épít centrumot."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Szimmetrikus centrum – fekete is igényli a centrumot."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"A Dáma-gambit! Fehér felajánlja a c4 gyalogot – ám ha fekete elfogadja, fehér könnyen visszanyeri."},
    {from:"e7",to:"e6",notation:"2...e6",color:"b",explanation:"Dáma-gambit visszautasítva – fekete szilárd centrumot tart."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Huszár fejlesztés, d5 megtámasztása elleni nyomás."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Fejlesztés, d4 megtámasztása elleni nyomás."},
    {from:"c1",to:"g5",notation:"4.Bg5",color:"w",explanation:"Az f6 huszárt köti, nyomást ad d5-re – klasszikus ortodox Dáma-gambit."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Készíti a sáncolást, kilép a kötés alól."},
    {from:"e2",to:"e3",notation:"5.e3",color:"w",explanation:"Szilárd, zárt struktúra – fehér befejezi a fejlesztést."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Fekete sáncol – a király biztonságba kerül."},
  ]
},
{
  id:"queens-gambit-accepted", name:"Dáma-gambit elfogadva", category:"Zárt játékok", difficulty:"haladó",
  shortDesc:"Fekete elfogadja a c4 gyalogot, de fehér könnyen visszanyeri és jobb centrumot kap.",
  info:"Ha fekete elfogadja a QGA-ban, fehér egy erős centrum fejlődési előny árán visszanyeri a gyalogot.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Szimmetria."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Dáma-gambit."},
    {from:"d5",to:"c4",notation:"2...dxc4",color:"b",explanation:"Dáma-gambit elfogadva! Fekete leüti c4-et, ám elveszíti a centrumát."},
    {from:"g1",to:"f3",notation:"3.Nf3",color:"w",explanation:"Fejlesztés – fehér majd visszanyeri c4-et."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"e2",to:"e3",notation:"4.e3",color:"w",explanation:"Fehér felkészíti a c4 gyalog visszanyerését futóval."},
    {from:"e7",to:"e6",notation:"4...e6",color:"b",explanation:"Szilárd tartás."},
    {from:"f1",to:"c4",notation:"5.Bxc4",color:"w",explanation:"Fehér visszanyeri a gambitugyalogot, és erős átlóval rendelkezik f7 felé."},
    {from:"c7",to:"c5",notation:"5...c5",color:"b",explanation:"Fekete visszatámad a centrum ellen – d4 gyalogot fenyegeti."},
  ]
},
{
  id:"nimzo-indian", name:"Nimzowitsch-indiai védelem", category:"Zárt játékok", difficulty:"mester",
  shortDesc:"3...Bb4 – fekete a c3 huszárt köti, megakadályozva az erős centrum kiépítést.",
  info:"Nimzowitsch nevéhez fűződik ez a szupermodern megnyitás. Fekete az e4 gyalog megakadályozásával játszik – hipermodern elv.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt játék."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Indiai rendszer előkészítése."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum kiszélesítése."},
    {from:"e7",to:"e6",notation:"2...e6",color:"b",explanation:"Rugalmasság – többféle indiai rendszer felé nyílik az út."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Huszár fejlesztés."},
    {from:"f8",to:"b4",notation:"3...Bb4",color:"b",explanation:"Nimzovitsch-indiai! A futó köti c3 huszárt – megakadályozza e4-et."},
    {from:"e2",to:"e3",notation:"4.e3",color:"w",explanation:"Szilárd, megtámasztja a d4 gyalogot."},
    {from:"e8",to:"g8",notation:"4...O-O",color:"b",explanation:"Sáncolás – biztonságban a király."},
    {from:"g1",to:"e2",notation:"5.Nge2",color:"w",explanation:"Huszár 'ne2-re' – nem blokkolja a futót, és meg fogja szüntetni a kötést."},
    {from:"d7",to:"d5",notation:"5...d5",color:"b",explanation:"Centrumtámadás – fekete is részt vesz a centrumharcban."},
  ]
},
{
  id:"kings-indian", name:"Királyindiai védelem", category:"Zárt játékok", difficulty:"mester",
  shortDesc:"Fekete fianchettoban fejleszt és d6-c5 ellentámadásra vár. A legélesebb dinamikus védekezés.",
  info:"Királyindiai a dinamizmus csúcsa: Bronstein, Fischer, Kasparov kedvence. Fekete engedi a fehér centrum kiépülést, majd felrobbantja.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés, d4 kontrollálása."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum kiszélesítés."},
    {from:"g7",to:"g6",notation:"2...g6",color:"b",explanation:"Fianchetto előkészítése – a futó g7-re kerül."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Huszár fejlesztés."},
    {from:"f8",to:"g7",notation:"3...Bg7",color:"b",explanation:"A sárkány-futó! Átlón hosszan nyomja a centrum."},
    {from:"e2",to:"e4",notation:"4.e4",color:"w",explanation:"Nagy centrumék – fehér hatalmas előnnyel bír teret."},
    {from:"d7",to:"d6",notation:"4...d6",color:"b",explanation:"Szilárd, megtámasztja e5-öt és megőrzi a rugalmasságot."},
    {from:"g1",to:"f3",notation:"5.Nf3",color:"w",explanation:"Fejlesztés, sáncolásra készül."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás – aztán jön az e5 vagy c5 ellentámadás!"},
  ]
},
{
  id:"queens-indian", name:"Dáma-indiai védelem", category:"Zárt játékok", difficulty:"mester",
  shortDesc:"3...b6 fianchetto – fekete a d4 ellen véd, rugalmas, pozicionális játék.",
  info:"A Dáma-indiai nagy pozicionális tudást igényel. Karpov és Petrosian egyik kedvenc fegyvere.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e6",notation:"2...e6",color:"b",explanation:"Rugalmas."},
    {from:"g1",to:"f3",notation:"3.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"b7",to:"b6",notation:"3...b6",color:"b",explanation:"Dáma-indiai! b6 előkészíti a c8 futó fianchettoját b7-re."},
    {from:"e2",to:"e3",notation:"4.e3",color:"w",explanation:"Szilárd struktúra."},
    {from:"c8",to:"b7",notation:"4...Bb7",color:"b",explanation:"A futó a nagy átlón – d4-et és e4-et ellenőrzi."},
    {from:"f1",to:"d3",notation:"5.Bd3",color:"w",explanation:"Fejlesztés, e4 előkészítése."},
    {from:"d7",to:"d5",notation:"5...d5",color:"b",explanation:"Centrumtámadás – egyenlő centrum fekete részéről."},
  ]
},
{
  id:"grunfeld", name:"Grünfeld-védelem", category:"Zárt játékok", difficulty:"mester",
  shortDesc:"Fekete engedi a fehér centrum kiépülést d4+c4+e4, majd aláaknázza azt.",
  info:"A Grünfeld hipermodern megnyitás: Kasparov legendásan nyert Karpov ellen ezzel. A d5-ös gyalog áldozatán alapul.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum."},
    {from:"g7",to:"g6",notation:"2...g6",color:"b",explanation:"Fianchetto felkészítése."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Huszár fejlesztés."},
    {from:"d7",to:"d5",notation:"3...d5",color:"b",explanation:"Grünfeld! Fekete azonnal centrumba lép – d5-öt áldozza a támadhatatlan d-vonal ellen."},
    {from:"c4",to:"d5",notation:"4.cxd5",color:"w",explanation:"Fehér elfoglalja a d5 pontot."},
    {from:"f6",to:"d5",notation:"4...Nxd5",color:"b",explanation:"A huszár visszafoglalja – szemben áll c3 huszárral."},
    {from:"e2",to:"e4",notation:"5.e4",color:"w",explanation:"Fehér nagy centruméket épít – e4+d4 – de ez a cél Grünfeldben."},
    {from:"d5",to:"c3",notation:"5...Nxc3",color:"b",explanation:"A huszár csap – dupla gyalog kerül c3-ra, de fehér centruma óriási lesz."},
  ]
},
{
  id:"dutch-defense", name:"Holland védelem", category:"Zárt játékok", difficulty:"haladó",
  shortDesc:"1...f5 – aszimmetrikus pozíció, fekete a királyoldalon tör előre.",
  info:"A Holland védelem harcias és egyedi. Ben-Oni rendszerekhez hasonlóan aszimmetrikus küzdelmet idéz elő.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"f7",to:"f5",notation:"1...f5",color:"b",explanation:"Holland! Fekete azonnal jelzi szándékát: királyoldalon tör előre."},
    {from:"g2",to:"g3",notation:"2.g3",color:"w",explanation:"Fianchetto felkészítése – fehér is fejleszt és e4-re törekszik."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés, centrumnyomás."},
    {from:"f1",to:"g2",notation:"3.Bg2",color:"w",explanation:"Fianchetto – a futó a hosszú átlón erős."},
    {from:"e7",to:"e6",notation:"3...e6",color:"b",explanation:"Rugalmas szilárd fejlesztés."},
    {from:"g1",to:"f3",notation:"4.Nf3",color:"w",explanation:"Huszár fejlesztés."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Fejlesztés, sáncolásra készít."},
    {from:"e1",to:"g1",notation:"5.O-O",color:"b",explanation:"Fehér sáncol."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Fekete is sáncol – aztán következik e5 vagy d6 plan."},
  ]
},
{
  id:"benoni", name:"Benoni-védelem", category:"Zárt játékok", difficulty:"mester",
  shortDesc:"5...c5 – aszimmetrikus struktúra, fekete királyoldalon tör, fehér dámaszárnyán.",
  info:"A Benoni a játékelmélet egyik legbonyolultabb terrénje. Fischer '72-es meccsén Spassky ellen is alkalmazta.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"2...c5",color:"b",explanation:"Benoni! Fekete c5-öt tolva támadja d4-et és aszimmetrikus csatát kér."},
    {from:"d4",to:"d5",notation:"3.d5",color:"w",explanation:"Fehér előrenyomul – d5 gyalog nagy területi előnyt biztosít."},
    {from:"e7",to:"e6",notation:"3...e6",color:"b",explanation:"Fekete megkérdőjelezi d5-öt."},
    {from:"b1",to:"c3",notation:"4.Nc3",color:"w",explanation:"Fejlesztés, d5 megerősítése."},
    {from:"e6",to:"d5",notation:"4...exd5",color:"b",explanation:"Fekete leüti – megnyílik az e-vonal."},
    {from:"c4",to:"d5",notation:"5.cxd5",color:"w",explanation:"A klasszikus Benoni struktúra kialakul: fehér d5 erős passé-gyalog."},
    {from:"d7",to:"d6",notation:"5...d6",color:"b",explanation:"Megtámasztja, rögzíti a struktúrát. Következik g6 és Bg7 – sárkányszerű felállás."},
  ]
},
// ---- FLANK MEGNYITÁSOK ----
{
  id:"english-opening", name:"Angol megnyitás", category:"Flank megnyitások", difficulty:"haladó",
  shortDesc:"1.c4 – flank megnyitás, indirekt centrum-kontroll, sokféle alakzatba torkollik.",
  info:"Az Angol megnyitás rugalmas. Botvinnik és Karpov is szívesen alkalmazta. A c4 gyalog kontrollja az indirekt centrumstratégia alapja.",
  moves:[
    {from:"c2",to:"c4",notation:"1.c4",color:"w",explanation:"Angol megnyitás! Flank centrum – d5 és e5 kontrollálása oldalról."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Fekete azonnal centrumba lép – szimmetrikus Angol."},
    {from:"b1",to:"c3",notation:"2.Nc3",color:"w",explanation:"Fejlesztés, e5 nyomása."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés, aktív centrum-kontroll."},
    {from:"g1",to:"f3",notation:"3.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"b8",to:"c6",notation:"3...Nc6",color:"b",explanation:"Védi e5-öt."},
    {from:"g2",to:"g3",notation:"4.g3",color:"w",explanation:"Fianchetto felkészítése – a hosszú átló mentén ellenőrzi a centrumot."},
    {from:"f8",to:"b4",notation:"4...Bb4",color:"b",explanation:"Fekete a c3 huszárt köti – Nimzószerű megközelítés."},
    {from:"f1",to:"g2",notation:"5.Bg2",color:"w",explanation:"Fianchetto – erős futó a hosszú átlón."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Fekete sáncol – kiegyenlített, rugalmas pozíció."},
  ]
},
{
  id:"reti-opening", name:"Réti megnyitás", category:"Flank megnyitások", difficulty:"mester",
  shortDesc:"1.Nf3 – hipermodern, flank fejlesztés. Fehér indirekt módon ellenőrzi a centrumot.",
  info:"Richard Réti alkotta meg ezt az avant-garde megnyitást az 1920-as években. Vezér kiadás nélkül kontrollál!",
  moves:[
    {from:"g1",to:"f3",notation:"1.Nf3",color:"w",explanation:"Réti! Huszár fejlesztve – d4 és e5 kontrollálva, de gyalog előre még nem megy."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Fekete erős centrumot vesz – d5 gyalog."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Réti gambit – c4-gyel megtámadja d5-öt."},
    {from:"d5",to:"c4",notation:"2...dxc4",color:"b",explanation:"Elfogadja a gambitugyalogot."},
    {from:"e2",to:"e3",notation:"3.e3",color:"w",explanation:"Felkészíti a futó fejlesztést és c4 visszanyerését."},
    {from:"c7",to:"c5",notation:"3...c5",color:"b",explanation:"Centrum megerősítése – fekete is fejleszti a centrumát."},
    {from:"f1",to:"c4",notation:"4.Bxc4",color:"w",explanation:"Fehér visszanyeri c4-et – erős futóval."},
    {from:"e7",to:"e6",notation:"4...e6",color:"b",explanation:"Rugalmas fejlesztés."},
    {from:"e1",to:"g1",notation:"5.O-O",color:"w",explanation:"Sáncolás – fehér gyorsan aktivizál."},
    {from:"g8",to:"f6",notation:"5...Nf6",color:"b",explanation:"Fejlesztés – aktív centrum-ellenőrzés."},
  ]
},
{
  id:"london-system", name:"Londoni rendszer", category:"Flank megnyitások", difficulty:"kezdő",
  shortDesc:"Megbízható, zárt rendszer fehérnek: Bf4, Nf3, e3, c3. Minimális memorizálás.",
  info:"A London rendszer az egyik legnépszerűbb amatőr megnyitás – szilárd, könnyen tanulható és kevés elméletet igényel.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Szimmetrikus centrum."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c1",to:"f4",notation:"3.Bf4",color:"w",explanation:"London kulcslépése! A futó aktív helyre kerül, mielőtt bekerülne e3 gyalog."},
    {from:"e7",to:"e6",notation:"3...e6",color:"b",explanation:"Szilárd fejlesztés."},
    {from:"e2",to:"e3",notation:"4.e3",color:"w",explanation:"Szilárd felépítés – a futó megtámasztva."},
    {from:"f8",to:"d6",notation:"4...Bd6",color:"b",explanation:"Fekete támadja f4 futót, ellennyomást okoz."},
    {from:"f4",to:"g3",notation:"5.Bg3",color:"w",explanation:"A futó cserét elkerüli."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás."},
  ]
},
{
  id:"catalan", name:"Katalán megnyitás", category:"Flank megnyitások", difficulty:"mester",
  shortDesc:"Dáma-gambit + fianchetto kombinációja. Fehér g2-s futója hosszú átlókon dominál.",
  info:"A Katalán megnyitás az elitre jellemző megnyitás, amelyet Kasparov, Kramnik és Anand is rendszeresen alkalmazott.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e6",notation:"2...e6",color:"b",explanation:"Rugalmas."},
    {from:"g2",to:"g3",notation:"3.g3",color:"w",explanation:"Katalán! Fianchetto felkészítése – a futó g2-re kerül és d5-re néz."},
    {from:"d7",to:"d5",notation:"3...d5",color:"b",explanation:"Centrumtámadás."},
    {from:"f1",to:"g2",notation:"4.Bg2",color:"w",explanation:"A Katalán futó! Hosszú átlón keresztülszúr a queenside-on."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Fejlesztés, sáncolásra készül."},
    {from:"g1",to:"f3",notation:"5.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Fekete sáncol – komplex pozicionális harc kezdődik."},
  ]
},
{
  id:"kings-indian-attack", name:"Királyindiai támadás (KIA)", category:"Flank megnyitások", difficulty:"haladó",
  shortDesc:"Nf3, g3, Bg2, d3, e4 – fehér rugalmas felállásból királyoldali rohamot indít.",
  info:"A KIA különösen Francia vagy Szicíliai ellen hatékony. Fischer is rendszeresen alkalmazta.",
  moves:[
    {from:"g1",to:"f3",notation:"1.Nf3",color:"w",explanation:"Huszár fejlesztés, rugalmas kezdés."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Centrum."},
    {from:"g2",to:"g3",notation:"2.g3",color:"w",explanation:"Fianchetto felkészítés."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"f1",to:"g2",notation:"3.Bg2",color:"w",explanation:"Fianchetto – hosszú átló kontroll."},
    {from:"e7",to:"e6",notation:"3...e6",color:"b",explanation:"Rugalmas, zárt struktúra."},
    {from:"e1",to:"g1",notation:"4.O-O",color:"w",explanation:"Sáncolás."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Fejlesztés, sáncolásra kész."},
    {from:"d2",to:"d3",notation:"5.d3",color:"w",explanation:"Zárt centrum – e4 előkészítése."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás – következik e4, Nbd2, e5 királyoldali nyomás."},
  ]
},
{
  id:"nimzo-larsen", name:"Nimzowitsch-Larsen támadás", category:"Flank megnyitások", difficulty:"haladó",
  shortDesc:"1.b3 – gyors fianchetto és a c8 futó elleni nyomás. Larsen és Fischer kedvence.",
  info:"Larsen grandiózus siker-sorozatot ért el ezzel a 1.b3-mal. Szokatlan és kellemetlenül rugalmas.",
  moves:[
    {from:"b2",to:"b3",notation:"1.b3",color:"w",explanation:"Nimzo-Larsen! Fianchetto felkészítése – a futó b2-re kerül és a hosszú átlót kontrollálni fogja."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Fekete erős centrumot vesz fel."},
    {from:"c1",to:"b2",notation:"2.Bb2",color:"w",explanation:"Fianchetto teljesül – a futó e5-öt és d4-et fenyegeti."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés, e5 védelme."},
    {from:"e2",to:"e3",notation:"3.e3",color:"w",explanation:"Szilárd felépítés."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"f1",to:"b5",notation:"4.Bb5",color:"w",explanation:"Fehér a c6 huszárt köti – spanyol motívum."},
    {from:"f6",to:"e4",notation:"4...Nxe4",color:"b",explanation:"Fekete bátran leüti e4-et."},
    {from:"b1",to:"c3",notation:"5.Nc3",color:"w",explanation:"Huszár fejlesztés, e4 huszárt fenyegeti."},
    {from:"e4",to:"c3",notation:"5...Nxc3",color:"b",explanation:"Csere – fehér duplaggyalogot kap, de jó fejlettséget nyer."},
  ]
},
// ---- GAMBITEK ----
{
  id:"sicilian-najdorf", name:"Szicíliai Najdorf", category:"Gambitek", difficulty:"mester",
  shortDesc:"A legélesebb és leggyakrabban játszott sakkmegnyitás. 5...a6 rugalmasan hagy minden opciót.",
  info:"A Szicíliai Najdorf a modern sakk egyik csúcsa. Fischer ezt egyetlen, objektíven legjobb megnyitásnak tartotta fekétének.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai! c5 gyalog – nem szimmetria, hanem harc a d4-ért."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés, d4 előkészítése."},
    {from:"d7",to:"d6",notation:"2...d6",color:"b",explanation:"Megtámasztja e5-öt és fejleszti a futókat."},
    {from:"d2",to:"d4",notation:"3.d4",color:"w",explanation:"Fehér nyit – a szicíliai harc szíve."},
    {from:"c5",to:"d4",notation:"3...cxd4",color:"b",explanation:"Csere – fekete megkapja a d4 gyalogot, fehér nyílt játékot kap."},
    {from:"f3",to:"d4",notation:"4.Nxd4",color:"w",explanation:"Visszahódítja d4-et, a huszár erős centralon van."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés, e4-et fenyegeti."},
    {from:"b1",to:"c3",notation:"5.Nc3",color:"w",explanation:"Fejlesztés."},
    {from:"a7",to:"a6",notation:"5...a6",color:"b",explanation:"Najdorf kulcslépése! 'Ruhát vesz fel' – b5-öt megakadályozza és rugalmasságot tart fenn."},
  ]
},
{
  id:"sicilian-dragon", name:"Szicíliai Sárkány", category:"Gambitek", difficulty:"mester",
  shortDesc:"6...g6 – fekete g7-es sárkányfutóval királyoldalon dominál és éles ellenrohamot vezet.",
  info:"A Sárkány a legélesebb Szicíliai változat. Fehér tipikusan h4-h5 rohammal próbál mattot adni, míg fekete c-vonalon tör.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"d7",to:"d6",notation:"2...d6",color:"b",explanation:"Alap."},
    {from:"d2",to:"d4",notation:"3.d4",color:"w",explanation:"Nyitás."},
    {from:"c5",to:"d4",notation:"3...cxd4",color:"b",explanation:"Csere."},
    {from:"f3",to:"d4",notation:"4.Nxd4",color:"w",explanation:"Visszafoglalja."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"b1",to:"c3",notation:"5.Nc3",color:"w",explanation:"Fejlesztés."},
    {from:"g7",to:"g6",notation:"5...g6",color:"b",explanation:"Sárkány! A g6-g7-es fianchetto kialakítja a Sárkány-futó állást – az egyik legerősebb futó-pozíció."},
  ]
},
{
  id:"sicilian-scheveningen", name:"Szicíliai Scheveningen", category:"Gambitek", difficulty:"mester",
  shortDesc:"6...e6 – fekete zártan tart, de d5 ugrással előrenyomulhat dinamikusan.",
  info:"Kasparov egyik legkedveltebb védelme fekete oldalon. A d5-ös betörés végső fegyver.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"d7",to:"d6",notation:"2...d6",color:"b",explanation:"Alap."},
    {from:"d2",to:"d4",notation:"3.d4",color:"w",explanation:"Nyitás."},
    {from:"c5",to:"d4",notation:"3...cxd4",color:"b",explanation:"Csere."},
    {from:"f3",to:"d4",notation:"4.Nxd4",color:"w",explanation:"Visszafoglalás."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"b1",to:"c3",notation:"5.Nc3",color:"w",explanation:"Fejlesztés."},
    {from:"e7",to:"e6",notation:"5...e6",color:"b",explanation:"Scheveningen! Zárt és szilárd struktúra – d5 betörés a jövőben a kulcsmotívum."},
  ]
},
{
  id:"queens-gambit-gambited", name:"Királygambit modern változat", category:"Gambitek", difficulty:"haladó",
  shortDesc:"2.f4 – klasszikus romantikus gambit, fehér a centrum és f-vonal kontrolljáért hoz áldozatot.",
  info:"A Királygambit volt a 19. századi sakkjátékok kedvence. Morphy és Anderssen korszakának alappillére.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"f2",to:"f4",notation:"2.f4",color:"w",explanation:"Királygambit! Fehér f-vonalat akar nyitni és centrumért áldoz gyalogot."},
    {from:"e5",to:"f4",notation:"2...exf4",color:"b",explanation:"Elfogadja – anyagi előny, de fehér dinamizmust kap."},
    {from:"g1",to:"f3",notation:"3.Nf3",color:"w",explanation:"g4 megakadályozása és fejlesztés."},
    {from:"d7",to:"d5",notation:"3...d5",color:"b",explanation:"Centrum ellentámadás – fischeri módszer."},
    {from:"e4",to:"d5",notation:"4.exd5",color:"w",explanation:"Fehér elfogadja a centrum-cserét."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés és d5 visszahódítás készül."},
    {from:"f1",to:"c4",notation:"5.Bc4",color:"w",explanation:"Futó fejlesztés – f7-t fenyegeti."},
    {from:"f6",to:"d5",notation:"5...Nxd5",color:"b",explanation:"Fekete visszanyeri d5-öt – izgalmas, kiegyensúlyozatlan pozíció."},
  ]
},
{
  id:"smith-morra", name:"Smith-Morra gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"1.e4 c5 2.d4 cxd4 3.c3 – fehér gyalogért fejlettséget és nyílt állást cserél.",
  info:"Az amatőrök kedvence Szicíliai ellen. Marc Esserman nagymester a modern napokban revitalizálta.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai."},
    {from:"d2",to:"d4",notation:"2.d4",color:"w",explanation:"Smith-Morra kezdete."},
    {from:"c5",to:"d4",notation:"2...cxd4",color:"b",explanation:"Fekete elfogadja."},
    {from:"c2",to:"c3",notation:"3.c3",color:"w",explanation:"Smith-Morra gambit! Fehér felajánlja c3-at is a gyors fejlettségért."},
    {from:"d4",to:"c3",notation:"3...dxc3",color:"b",explanation:"Elfogadja – kettős gyalog előnnyel."},
    {from:"b1",to:"c3",notation:"4.Nxc3",color:"w",explanation:"Fehér visszafoglalja, hatalmas fejlettségi előnnyel."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés, e4 fenyegetés."},
    {from:"e2",to:"e3",notation:"5.e3",color:"w",explanation:"Szilárd fejlesztés."},
    {from:"e7",to:"e6",notation:"5...e6",color:"b",explanation:"Védekezés, fejlesztés."},
  ]
},
{
  id:"budapest-gambit", name:"Budapest-gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"2...Nf6 3...Ne4 – fekete aktív, szokatlan gambitet játszik d4 ellen.",
  info:"A Budapest-gambitet 1896-ban Breyer és Abonyi budapesti játékosok fejlesztették ki. Szokatlanul aktív fekete részéről.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"2...e5",color:"b",explanation:"Budapest-gambit! Fekete azonnal centrumban ajánl gyalogot."},
    {from:"d4",to:"e5",notation:"3.dxe5",color:"w",explanation:"Fehér elfogadja a gambitugyalogot."},
    {from:"f6",to:"e4",notation:"3...Ne4",color:"b",explanation:"A huszár befészkeli magát e4-be – nagyon aktív pozíció."},
    {from:"c1",to:"f4",notation:"4.Bf4",color:"w",explanation:"Futó fejlesztés, megtámasztja e5-öt."},
    {from:"b8",to:"c6",notation:"4...Nc6",color:"b",explanation:"Fejlesztés, e5 elleni nyomás."},
    {from:"g1",to:"f3",notation:"5.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"f8",to:"b4",notation:"5...Bb4+",color:"b",explanation:"Sakk! Fekete c3-at kényszeríti, és erős presszúrát tart fenn."},
  ]
},
{
  id:"benko-gambit", name:"Benko-gambit", category:"Gambitek", difficulty:"mester",
  shortDesc:"5...b5 – fekete a- és b-vonalat áldoz aktív, tartós nyomásért a hosszú oldalon.",
  info:"A Benko-gambit (Volga-gambit) Pal Benkő magyar-amerikai nagymester nevéhez fűződik. Hosszú nyomást ad.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c2",to:"c4",notation:"2.c4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"2...c5",color:"b",explanation:"Centrum támadás."},
    {from:"d4",to:"d5",notation:"3.d5",color:"w",explanation:"Fehér előrenyomul."},
    {from:"b7",to:"b5",notation:"3...b5",color:"b",explanation:"Benko-gambit! Fekete b5-öt ajánlja fel a- és b-vonal megnyitásáért."},
    {from:"c4",to:"b5",notation:"4.cxb5",color:"w",explanation:"Fehér elfogadja."},
    {from:"a7",to:"a6",notation:"4...a6",color:"b",explanation:"Folytatja a gambitusorozatot – a-vonal megnyitás is."},
    {from:"b5",to:"a6",notation:"5.bxa6",color:"w",explanation:"Elfogadja – két gyaloggal előnyös fehér, de erős nyomást kap."},
    {from:"f8",to:"a6",notation:"5...Bxa6",color:"b",explanation:"Fekete visszahódítja az anyagot futóval – a- és b-vonal mentén dominál."},
  ]
},
{
  id:"evans-gambit", name:"Evans-gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"4.b4 – fehér a c4 futót elvezeti és erős centrumot épít az Olasz játékban.",
  info:"Kapitány Evans alkotta 1824-ben. Morphy, Kasparov is játszotta. Romantikus, élénk és veszélyes gambit.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés."},
    {from:"f1",to:"c4",notation:"3.Bc4",color:"w",explanation:"Olasz futó."},
    {from:"f8",to:"c5",notation:"3...Bc5",color:"b",explanation:"Szimmetrikus futó."},
    {from:"b2",to:"b4",notation:"4.b4",color:"w",explanation:"Evans-gambit! Fehér b4-gyel kinyomja a c5 futót és erős d4 centrumot épít."},
    {from:"c5",to:"b4",notation:"4...Bxb4",color:"b",explanation:"Fekete elfogadja a gambitugyalogot – futót nyer, centrum veszt."},
    {from:"c2",to:"c3",notation:"5.c3",color:"w",explanation:"Felkészíti d4-et – a centrum feltörés következik."},
    {from:"b4",to:"a5",notation:"5...Ba5",color:"b",explanation:"A futó visszahúzódik – sok változat keletkezik innen."},
  ]
},
{
  id:"blackmar-diemer", name:"Blackmar-Diemer gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"3.f3 – fehér gyalogot ajánl nyílt, aktív d4-es centrum játékért.",
  info:"A BDG a legharciasabb 1.d4 gambit. Riemann, Blackmar és Diemer fejlesztette ki. Szinte mindig éles.​",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Centrum."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Szimmetria."},
    {from:"e2",to:"e4",notation:"2.e4",color:"w",explanation:"Gambit felajánlás – d5 megtámadva."},
    {from:"d5",to:"e4",notation:"2...dxe4",color:"b",explanation:"Elfogadja."},
    {from:"f2",to:"f3",notation:"3.f3",color:"w",explanation:"BDG kulcslépése! Fehér a gambitugyalogot is feláldozza – nyílt vonalak kellenek."},
    {from:"e4",to:"f3",notation:"3...exf3",color:"b",explanation:"Elfogadja mindkét gyalogot."},
    {from:"d1",to:"f3",notation:"4.Qxf3",color:"w",explanation:"Vezér aktívan a táblára kerül – f7-t fenyegeti már most."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés, a vezér megtámadása."},
    {from:"f1",to:"d3",notation:"5.Bd3",color:"w",explanation:"Futó fejlesztés."},
    {from:"e7",to:"e6",notation:"5...e6",color:"b",explanation:"Szilárd védekezés."},
  ]
},
{
  id:"alekhine-defense", name:"Alekhine-védelem", category:"Gambitek", difficulty:"mester",
  shortDesc:"1...Nf6 – fekete e4 megtámadásával provokálja a gyalogközpontot, majd aláaknázza.",
  info:"Alekhine grandiózus, hipermodern ötlete: hagyd felépíteni a centrumot, majd támadj!",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Alekhine-védelem! A huszár azonnal megtámadja e4-et – provokál."},
    {from:"e4",to:"e5",notation:"2.e5",color:"w",explanation:"Fehér elfogadja a provokációt és elűzi a huszárt."},
    {from:"f6",to:"d5",notation:"2...Nd5",color:"b",explanation:"A huszár visszalép, de d5-en erős pontot foglal el."},
    {from:"d2",to:"d4",notation:"3.d4",color:"w",explanation:"Centrum kiszélesítés – fehér nagy centrumot épít."},
    {from:"d7",to:"d6",notation:"3...d6",color:"b",explanation:"Fekete azonnal aláaknázza a centrumot."},
    {from:"c2",to:"c4",notation:"4.c4",color:"w",explanation:"Elűzi d5-ről a huszárt – nagy centrum, de gyengíti magát."},
    {from:"d5",to:"b6",notation:"4...Nb6",color:"b",explanation:"Visszavonul, de nyomást tart fenn."},
    {from:"e5",to:"d6",notation:"5.exd6",color:"w",explanation:"Csere – megnyílik a d-vonal."},
    {from:"e7",to:"d6",notation:"5...exd6",color:"b",explanation:"Visszafoglalja – következik aktív fejlesztés."},
  ]
},
{
  id:"pirc-defense", name:"Pirc-védelem", category:"Gambitek", difficulty:"mester",
  shortDesc:"Fekete fianchettos g6+Bg7 felállásával tolerálja a fehér centrumot, majd belülről robbantja fel.",
  info:"A Pirc-védelem a Királyindiai egyik testvérvédelme. Szilárd, de aktív ellenállást ígér.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"d7",to:"d6",notation:"1...d6",color:"b",explanation:"Pirc – megtámasztja e5-öt, rugalmas."},
    {from:"d2",to:"d4",notation:"2.d4",color:"w",explanation:"Centrum kiszélesítés."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés, e4 fenyegetés."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Fejlesztés."},
    {from:"g7",to:"g6",notation:"3...g6",color:"b",explanation:"Fianchetto előkészítése."},
    {from:"c1",to:"e3",notation:"4.Be3",color:"w",explanation:"Osztályos rendszer – fejlesztés és centrum."},
    {from:"f8",to:"g7",notation:"4...Bg7",color:"b",explanation:"Sárkány-futó! Erős, hosszú átlókon keresztülszúr."},
    {from:"d1",to:"d2",notation:"5.Qd2",color:"w",explanation:"Hosszú sáncolásra készül – fehér királyoldalon támad."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás – következik c5 vagy e5 ellentámadás."},
  ]
},
{
  id:"letti-gambit", name:"Letti-gambit (Latvian)", category:"Gambitek", difficulty:"haladó",
  shortDesc:"2...f5 – fekete extrém agresszív ellentámadással komplikált állást hoz létre.",
  info:"A Letti-gambit rendkívül éles és veszélyes – mindkét oldal számára. Ajánlott ismerőknek.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés, e5 fenyegetés."},
    {from:"f7",to:"f5",notation:"2...f5",color:"b",explanation:"Letti-gambit! Fekete azonnal ellentámad, gambitugyalogot ajánl."},
    {from:"f3",to:"e5",notation:"3.Nxe5",color:"w",explanation:"Fehér leüti e5-öt – elfogadja."},
    {from:"d8",to:"f6",notation:"3...Qf6",color:"b",explanation:"Vezér f6-ra – e5 huszárt fenyegeti és védi f5-öt."},
    {from:"d2",to:"d4",notation:"4.d4",color:"w",explanation:"Centrum, megtámasztja a huszárt."},
    {from:"d7",to:"d6",notation:"4...d6",color:"b",explanation:"Elűzi e5-ről a huszárt."},
    {from:"e5",to:"c4",notation:"5.Nc4",color:"w",explanation:"Visszavonul – fehér anyagi előnnyel, de fekete aktívan áll."},
    {from:"f5",to:"e4",notation:"5...fxe4",color:"b",explanation:"Fekete megszerez e4-et – komplikált, kiszámíthatatlan harc."},
  ]
},
{
  id:"modern-defense", name:"Modern védelem", category:"Gambitek", difficulty:"mester",
  shortDesc:"1...g6 – fekete először fianchettoban fejleszt, majd flexibilis tervet választ.",
  info:"A Modern védelem rugalmas és trükkös: fekete nem kötelezi el magát korán, hanem megvárja fehér szándékát.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"g7",to:"g6",notation:"1...g6",color:"b",explanation:"Modern védelem! Fekete g6 fianchettoban fejleszt – rugalmas és trükkös."},
    {from:"d2",to:"d4",notation:"2.d4",color:"w",explanation:"Fehér centrum."},
    {from:"f8",to:"g7",notation:"2...Bg7",color:"b",explanation:"Fianchetto – a sárkány-futó a hosszú átlón."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Fejlesztés."},
    {from:"d7",to:"d6",notation:"3...d6",color:"b",explanation:"Megtámasztja e5-öt – Pirc-jellegű felépítés."},
    {from:"g1",to:"f3",notation:"4.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c1",to:"e3",notation:"5.Be3",color:"w",explanation:"Aktív futó fejlesztés."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás – kompakt, szilárd védelmi felépítés."},
  ]
},
{
  id:"elephant-gambit", name:"Elephant gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"2...d5 – fekete radikálisan ellentámad e4-re d5-tel.",
  info:"Az Elephant-gambit ritka és sokkoló. Ha fehér nem vigyáz, gyorsan veszélyes helyzetbe kerülhet.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés, e5 fenyegetés."},
    {from:"d7",to:"d5",notation:"2...d5",color:"b",explanation:"Elephant-gambit! Fekete radikálisan ajánlja d5-öt – ellentámad e4 ellen."},
    {from:"e4",to:"d5",notation:"3.exd5",color:"w",explanation:"Fehér elfogadja."},
    {from:"e5",to:"e4",notation:"3...e4",color:"b",explanation:"Fekete e4-et tolja – az f3 huszárt megakadályozza!"},
    {from:"d1",to:"e2",notation:"4.Qe2",color:"w",explanation:"Fenyegeti e4 huszárt, és f3-at felszabadítja."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Fejlesztés, védi e4-et."},
    {from:"e2",to:"e4",notation:"5.Qxe4",color:"w",explanation:"Visszafoglalja e4-et – fehér anyagi előnnyel, de fekete aktív."},
    {from:"g8",to:"f6",notation:"5...Nf6",color:"b",explanation:"Fejlesztés, e4 vezért fenyegeti."},
  ]
},
{
  id:"sicilian-kan", name:"Szicíliai Kan-variáns", category:"Gambitek", difficulty:"haladó",
  shortDesc:"4...a6 – rugalmas, minden opciót nyitva hagyó Szicíliai variáns.",
  info:"A Kan-variáns (Paulsen) maximalista rugalmassággal rendelkezik. Fekete figyeli fehér szándékát.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"e7",to:"e6",notation:"2...e6",color:"b",explanation:"Rugalmas."},
    {from:"d2",to:"d4",notation:"3.d4",color:"w",explanation:"Centrum."},
    {from:"c5",to:"d4",notation:"3...cxd4",color:"b",explanation:"Csere."},
    {from:"f3",to:"d4",notation:"4.Nxd4",color:"w",explanation:"Visszafoglalás."},
    {from:"a7",to:"a6",notation:"4...a6",color:"b",explanation:"Kan-variáns – a6 megakadályozza b5-öt, teret tesz b5-nek fekétének."},
    {from:"b1",to:"c3",notation:"5.Nc3",color:"w",explanation:"Fejlesztés."},
    {from:"d8",to:"c7",notation:"5...Qc7",color:"b",explanation:"Vezér c7-re – védi c5-öt, ellenőrzi e5-öt, rugalmas."},
  ]
},
{
  id:"scandinavian", name:"Skandináv védelem", category:"Gambitek", difficulty:"haladó",
  shortDesc:"1...d5 – fekete azonnal megtámadja e4-et, szokatlan és megbízható védekezés.",
  info:"A Skandináv (vagy Center Counter) populáris amatőr szinten. Anand 1995-ös PCA meccsen is játszotta.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Skandináv! Fekete azonnal megtámadja e4-et d5-tel – élénk ellenhatás."},
    {from:"e4",to:"d5",notation:"2.exd5",color:"w",explanation:"Fehér elfogadja."},
    {from:"d8",to:"d5",notation:"2...Qxd5",color:"b",explanation:"Klasszikus – vezér visszahódítja d5-öt."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Huszár fejlesztés és a vezér megtámadása."},
    {from:"d5",to:"a5",notation:"3...Qa5",color:"b",explanation:"Vezér a5-re húzódik – aktív, de kissé korai vezér-fejlesztés."},
    {from:"d2",to:"d4",notation:"4.d4",color:"w",explanation:"Centrum."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"g1",to:"f3",notation:"5.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"c8",to:"f5",notation:"5...Bf5",color:"b",explanation:"Futó fejlesztés – aktív, rugalmas állás."},
  ]
},
{
  id:"ponziani", name:"Ponziani megnyitás", category:"Nyílt játékok", difficulty:"kezdő",
  shortDesc:"3.c3 – meglepő, kevéssé ismert, mégis szilárd fehér megnyitás.",
  info:"A Ponziani megnyitás régimódi, de meglepő lehet. d4 felkészítése c3-mal a kulcsötlet.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés."},
    {from:"c2",to:"c3",notation:"3.c3",color:"w",explanation:"Ponziani! d4 előkészítése – fehér erős centrumot tervez."},
    {from:"d7",to:"d5",notation:"3...d5",color:"b",explanation:"Legjobb ellen-centrum – fekete megelőzi d4-et."},
    {from:"d1",to:"a4",notation:"4.Qa4",color:"w",explanation:"Vezér aktivizálás – c6 huszárt és d5 gyalogot köti."},
    {from:"f7",to:"f6",notation:"4...f6",color:"b",explanation:"Megtámasztja e5-öt."},
    {from:"f1",to:"b5",notation:"5.Bb5",color:"w",explanation:"Huszár kötés spanyol stílusban."},
    {from:"g8",to:"e7",notation:"5...Ne7",color:"b",explanation:"A huszár védi a kötött c6-os huszárt."},
  ]
},
{
  id:"halloween-gambit", name:"Halloween gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"4.Nxe5 – fehér huszárral megtámadja e5-öt, extrém gambit a Négy huszárosban.",
  info:"A Halloween-gambit meglepő és veszélyes amatőr szinten – huszárt ajánl fel tempóért és pozícióért.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés."},
    {from:"b1",to:"c3",notation:"3.Nc3",color:"w",explanation:"Négy huszáros."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Négy huszáros."},
    {from:"f3",to:"e5",notation:"4.Nxe5",color:"w",explanation:"Halloween-gambit! Fehér leüti e5-öt huszárral – őrület, de meglepő!"},
    {from:"c6",to:"e5",notation:"4...Nxe5",color:"b",explanation:"Fekete elfogadja a huszárt."},
    {from:"d2",to:"d4",notation:"5.d4",color:"w",explanation:"Fehér centrumtámadással kompenzálja a veszteségét."},
    {from:"e5",to:"c6",notation:"5...Nc6",color:"b",explanation:"A huszár visszahúzódik – fekete anyagi előnnyel, de fehér jó centrummal."},
  ]
},
{
  id:"bird-opening", name:"Bird megnyitás", category:"Flank megnyitások", difficulty:"haladó",
  shortDesc:"1.f4 – fehér királyoldali f-vonallal indít, From-gambit lehetséges rá.",
  info:"A Bird megnyitást Henry Bird angol nagymester tette híressé a 19. században. Szokatlan és rugalmas.",
  moves:[
    {from:"f2",to:"f4",notation:"1.f4",color:"w",explanation:"Bird megnyitás! f4 azonnal jelzi fehér királyoldali szándékát."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Erős centrum – a legjobb válasz."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"e2",to:"e3",notation:"3.e3",color:"w",explanation:"Szilárd struktúra."},
    {from:"e7",to:"e6",notation:"3...e6",color:"b",explanation:"Rugalmas fejlesztés."},
    {from:"f1",to:"e2",notation:"4.Be2",color:"w",explanation:"Fejlesztés, sáncolásra kész."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Fejlesztés."},
    {from:"e1",to:"g1",notation:"5.O-O",color:"w",explanation:"Sáncolás."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Fekete is sáncol – kiegyenlített pozíció."},
  ]
},
{
  id:"from-gambit", name:"From-gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"1.f4 e5 – fekete gambittel reagál a Bird megnyitásra, éles és kiszámíthatatlan.",
  info:"A From-gambit a Bird megnyitás elleni legélesebb válasz. Sokkoló és veszélyes.",
  moves:[
    {from:"f2",to:"f4",notation:"1.f4",color:"w",explanation:"Bird megnyitás."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"From-gambit! Fekete azonnal centrumban provokál."},
    {from:"f4",to:"e5",notation:"2.fxe5",color:"w",explanation:"Fehér elfogadja."},
    {from:"d7",to:"d6",notation:"2...d6",color:"b",explanation:"Fekete azonnal visszatámad e5-re."},
    {from:"e5",to:"d6",notation:"3.exd6",color:"w",explanation:"Fehér elfogadja a cserét."},
    {from:"f8",to:"d6",notation:"3...Bxd6",color:"b",explanation:"Fekete visszahódítja d6-ot – aktív futóval, fejlettségi előnnyel."},
    {from:"g1",to:"f3",notation:"4.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"g7",to:"g5",notation:"4...g5",color:"b",explanation:"Fekete kényelmes tempóban tologatja a gyalogot – veszélyes királyoldali nyomás."},
    {from:"d2",to:"d4",notation:"5.d4",color:"w",explanation:"Centrum."},
    {from:"g5",to:"g4",notation:"5...g4",color:"b",explanation:"g4-et tolva üzi el f3 huszárt – éles, kiszámíthatatlan harc."},
  ]
},
{
  id:"colle-system", name:"Colle-rendszer", category:"Zárt játékok", difficulty:"kezdő",
  shortDesc:"Nf3, d4, e3, Bd3 – zárt, megbízható fehér rendszer minimal elmélettel.",
  info:"A Colle-rendszer az amatőrök egyik kedvence: könnyen megtanulható és mégis veszélyes.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"e2",to:"e3",notation:"3.e3",color:"w",explanation:"Colle felépítés – szilárd centrum."},
    {from:"e7",to:"e6",notation:"3...e6",color:"b",explanation:"Zárt fejlesztés."},
    {from:"f1",to:"d3",notation:"4.Bd3",color:"w",explanation:"Futó fejlesztés – e4 lépést készíti elő."},
    {from:"b8",to:"d7",notation:"4...Nbd7",color:"b",explanation:"Fejlesztés."},
    {from:"e1",to:"g1",notation:"5.O-O",color:"w",explanation:"Sáncolás."},
    {from:"f8",to:"d6",notation:"5...Bd6",color:"b",explanation:"Fejlesztés, sáncolásra kész."},
  ]
},
{
  id:"torre-attack", name:"Torre-támadás", category:"Zárt játékok", difficulty:"kezdő",
  shortDesc:"Bg5 korai fejlesztéssel – egyszerű, rugalmas rendszer d4 után.",
  info:"Carlos Torre mexikói nagymester nevéhez fűzik ezt a megnyitást az 1920-as évekből.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"e7",to:"e6",notation:"2...e6",color:"b",explanation:"Rugalmas."},
    {from:"c1",to:"g5",notation:"3.Bg5",color:"w",explanation:"Torre kulcslépése! Korai futó fejlesztés – f6 huszárt köti."},
    {from:"d7",to:"d5",notation:"3...d5",color:"b",explanation:"Centrum."},
    {from:"e2",to:"e3",notation:"4.e3",color:"w",explanation:"Szilárd felépítés."},
    {from:"f8",to:"e7",notation:"4...Be7",color:"b",explanation:"Fejlesztés, sáncolásra kész."},
    {from:"b1",to:"d2",notation:"5.Nbd2",color:"w",explanation:"Huszár fejlesztés – e4 opciót tartja nyitva."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás."},
  ]
},
{
  id:"polish-opening", name:"Lengyel megnyitás", category:"Flank megnyitások", difficulty:"kezdő",
  shortDesc:"1.b4 – az orangutan! Excentrikus, de meglepő és trükkös megnyitás.",
  info:"A Lengyel megnyitást (1.b4) Savielly Tartakower 'Orangutan'-nak nevezte el egy 1924-es New York-i partija után.",
  moves:[
    {from:"b2",to:"b4",notation:"1.b4",color:"w",explanation:"Lengyel megnyitás! b4-el flank-ból indul – c5 és a5 megakadályozása a cél."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Fekete centrum."},
    {from:"c1",to:"b2",notation:"2.Bb2",color:"w",explanation:"Fianchetto – a futó e5-re és a centrum felé néz."},
    {from:"f8",to:"b4",notation:"2...Bxb4",color:"b",explanation:"Fekete elfogadja a b4 gyalogot."},
    {from:"b2",to:"e5",notation:"3.Bxe5",color:"w",explanation:"A futó leüti e5-öt – kompenzáció a gyalogért."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Fejlesztés, futót fenyegeti."},
    {from:"e5",to:"f4",notation:"4.Bf4",color:"w",explanation:"Futó biztonságba húzódik."},
    {from:"d7",to:"d5",notation:"4...d5",color:"b",explanation:"Centrum."},
    {from:"e2",to:"e3",notation:"5.e3",color:"w",explanation:"Szilárd."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás."},
  ]
},
{
  id:"stonewall-attack", name:"Kőfal támadás", category:"Zárt játékok", difficulty:"kezdő",
  shortDesc:"d4-e3-f4-c3 kőfal struktúra – szilárd, nehezen feltörhető felállás fehérnek.",
  info:"A Kőfal klasszikus amatőr megnyitás – egyszerű, de nehéz ellene játszani ha nem ismered.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Centrum."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Szimmetria."},
    {from:"e2",to:"e3",notation:"2.e3",color:"w",explanation:"Kőfal kezdete – szilárd, zárt struktúra."},
    {from:"g8",to:"f6",notation:"2...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"f2",to:"f4",notation:"3.f4",color:"w",explanation:"Kőfal kulcslépése! e4-d4-f4 erőd struktúra épül."},
    {from:"e7",to:"e6",notation:"3...e6",color:"b",explanation:"Zárt fejlesztés."},
    {from:"g1",to:"f3",notation:"4.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"f8",to:"d6",notation:"4...Bd6",color:"b",explanation:"Fejlesztés, f4 fenyegetés."},
    {from:"f1",to:"d3",notation:"5.Bd3",color:"w",explanation:"Fejlesztés – a futó az e4 előretörésre vár."},
    {from:"e8",to:"g8",notation:"5...O-O",color:"b",explanation:"Sáncolás."},
  ]
},
{
  id:"trompowsky", name:"Trompowsky-támadás", category:"Flank megnyitások", difficulty:"haladó",
  shortDesc:"2.Bg5 – fehér korai futófejlesztéssel meglepő, agresszív játékot kezd.",
  info:"A Trompowsky-támadást Julian Hodgson és Nigel Short terjesztette el a 90-es évek elejétől.",
  moves:[
    {from:"d2",to:"d4",notation:"1.d4",color:"w",explanation:"Zárt centrum."},
    {from:"g8",to:"f6",notation:"1...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"c1",to:"g5",notation:"2.Bg5",color:"w",explanation:"Trompowsky! Futó azonnal f6 huszárt köti – rendhagyó és meglepő."},
    {from:"f6",to:"e4",notation:"2...Ne4",color:"b",explanation:"Fekete megtámadja a futót."},
    {from:"g5",to:"f4",notation:"3.Bf4",color:"w",explanation:"Futó visszalép, megtartva az aktivitást."},
    {from:"d7",to:"d5",notation:"3...d5",color:"b",explanation:"Centrum."},
    {from:"e2",to:"e3",notation:"4.e3",color:"w",explanation:"Szilárd felépítés."},
    {from:"e7",to:"e6",notation:"4...e6",color:"b",explanation:"Rugalmas fejlesztés."},
    {from:"f1",to:"d3",notation:"5.Bd3",color:"w",explanation:"Futó fejlesztés."},
    {from:"f7",to:"f5",notation:"5...f5",color:"b",explanation:"Holland-jellegű fekete – aktívan tartja a centrum előnyt."},
  ]
},
{
  id:"kings-fianchetto", name:"Királyoldali fianchetto", category:"Flank megnyitások", difficulty:"kezdő",
  shortDesc:"1.g3 Bg2 – fehér gyors fianchettoban fejleszti futóját, rugalmas rendszer.",
  info:"A Királyoldali fianchetto rendszer rendkívül rugalmas. Bármilyen ellentámadásra felkészülten tud reagálni.",
  moves:[
    {from:"g2",to:"g3",notation:"1.g3",color:"w",explanation:"Fianchetto felkészítése – rugalmas, nem kötelezi el fehért."},
    {from:"d7",to:"d5",notation:"1...d5",color:"b",explanation:"Centrum."},
    {from:"f1",to:"g2",notation:"2.Bg2",color:"w",explanation:"Fianchetto kész – erős futó a hosszú átlón."},
    {from:"c7",to:"c5",notation:"2...c5",color:"b",explanation:"Centrum fejlesztés."},
    {from:"g1",to:"f3",notation:"3.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Fejlesztés."},
    {from:"e1",to:"g1",notation:"4.O-O",color:"w",explanation:"Sáncolás – fehér gyorsan aktivizál."},
    {from:"g7",to:"g6",notation:"4...g6",color:"b",explanation:"Fekete is fianchettoban fejleszt – sárkányszerű struktúra."},
    {from:"d2",to:"d3",notation:"5.d3",color:"w",explanation:"Zárt centrum – rugalmas, minimális elmélet."},
    {from:"f8",to:"g7",notation:"5...Bg7",color:"b",explanation:"Sárkány-futó – szimmetrikus fianchetto pozíció."},
  ]
},
{
  id:"sicilian-alapin", name:"Szicíliai Alapin", category:"Gambitek", difficulty:"haladó",
  shortDesc:"2.c3 – fehér elkerüli a fő szicíliait és d4-et tervez c3-ból.",
  info:"Az Alapin-variáns anti-Szicíliai rendszer. c3-mal épít d4-et, de elveszíti c3 huszárhelyét.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai."},
    {from:"c2",to:"c3",notation:"2.c3",color:"w",explanation:"Alapin! Fehér d4-et tervez c3 alátámasztással – elkerüli a fő szicíliait."},
    {from:"d7",to:"d5",notation:"2...d5",color:"b",explanation:"Legaktívabb válasz – fekete megelőzi d4-et."},
    {from:"e4",to:"d5",notation:"3.exd5",color:"w",explanation:"Fehér leüti d5-öt."},
    {from:"d8",to:"d5",notation:"3...Qxd5",color:"b",explanation:"Fekete visszafoglalja – aktív vezérrel."},
    {from:"d2",to:"d4",notation:"4.d4",color:"w",explanation:"Centrumtámadás a vezér ellen."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés, e4 fenyegetés."},
    {from:"g1",to:"f3",notation:"5.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"c8",to:"g4",notation:"5...Bg4",color:"b",explanation:"Futó fejlesztés – f3 huszárt köti."},
  ]
},
{
  id:"moscow-variation", name:"Szicíliai Moszkvai variáns", category:"Gambitek", difficulty:"haladó",
  shortDesc:"3.Bb5+ anti-Szicíliai – egyszerű és hatékony fehér rendszer Szicíliai ellen.",
  info:"A Moszkvai variáns népszerű anti-szicíliai választás. Fehér gyorsan fejleszt és nyomást ad.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"d7",to:"d6",notation:"2...d6",color:"b",explanation:"Alap."},
    {from:"f1",to:"b5",notation:"3.Bb5+",color:"w",explanation:"Moszkvai variáns! Sakk – azonnal nyomást ad, megelőzi d4-et."},
    {from:"c8",to:"d7",notation:"3...Bd7",color:"b",explanation:"A futó blokkolja a sakkot – legtermészetesebb válasz."},
    {from:"b5",to:"d7",notation:"4.Bxd7+",color:"w",explanation:"Csere – fekete dupla gyalogjával komoly hátrányban."},
    {from:"d8",to:"d7",notation:"4...Qxd7",color:"b",explanation:"Vezér visszafoglalja."},
    {from:"e1",to:"g1",notation:"5.O-O",color:"w",explanation:"Sáncolás."},
    {from:"b8",to:"c6",notation:"5...Nc6",color:"b",explanation:"Fejlesztés."},
  ]
},
{
  id:"sicilian-grand-prix", name:"Szicíliai Nagy Díj-támadás", category:"Gambitek", difficulty:"haladó",
  shortDesc:"2.Nc3 3.f4 – fehér gyors királyoldali rohamot épít a Szicíliai ellen.",
  info:"A Grand Prix-támadás amatőr szinten rendkívül hatékony. Nc3, f4, Nf3, d3 rendszer.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"c7",to:"c5",notation:"1...c5",color:"b",explanation:"Szicíliai."},
    {from:"b1",to:"c3",notation:"2.Nc3",color:"w",explanation:"Anti-Szicíliai – f4 lehetőségének megőrzése."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés."},
    {from:"f2",to:"f4",notation:"3.f4",color:"w",explanation:"Grand Prix kulcslépése! Királyoldali roham előkészítve."},
    {from:"g7",to:"g6",notation:"3...g6",color:"b",explanation:"Fianchetto fekétének – Sárkányszerű felépítés."},
    {from:"g1",to:"f3",notation:"4.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"f8",to:"g7",notation:"4...Bg7",color:"b",explanation:"Fianchetto teljes."},
    {from:"f1",to:"c4",notation:"5.Bc4",color:"w",explanation:"Fejlesztés – f7 nyomás, sáncolásra kész."},
    {from:"e7",to:"e6",notation:"5...e6",color:"b",explanation:"Védi d5-öt, sáncolásra készül."},
  ]
},
{
  id:"fried-liver", name:"Sült Máj (Fried Liver Attack)", category:"Gambitek", difficulty:"mester",
  shortDesc:"7.Nxf7 – fehér huszárt áldoz f7-re a két huszáros megnyitásban, mattot fenyegetve.",
  info:"A legvakmerőbb és leglátványosabb sakkgambit. Szinte csak online és amatőr szinten játszott.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés."},
    {from:"f1",to:"c4",notation:"3.Bc4",color:"w",explanation:"Olasz futó."},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Két huszár védelem."},
    {from:"f3",to:"g5",notation:"4.Ng5",color:"w",explanation:"A huszár f7-re tör – fenyeget Nxf7 cserét."},
    {from:"d7",to:"d5",notation:"4...d5",color:"b",explanation:"Legjobb ellenjáték – Ng5 ellen."},
    {from:"e4",to:"d5",notation:"5.exd5",color:"w",explanation:"Fehér leüti d5-öt."},
    {from:"f6",to:"d5",notation:"5...Nxd5",color:"b",explanation:"Visszafoglalás – és most jön a Sült Máj!"},
  ]
},
{
  id:"jerome-gambit", name:"Jerome-gambit", category:"Gambitek", difficulty:"haladó",
  shortDesc:"4.Bxf7+ – fehér futót és huszárt áldoz az Olasz játékban mattért, romantikus de nem szound.",
  info:"Az Amatőr Amasa B. Jerome alkotta 1874-ben. Objektíve elveszett, de meglepő és tanulságos.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"g1",to:"f3",notation:"2.Nf3",color:"w",explanation:"Fejlesztés."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés."},
    {from:"f1",to:"c4",notation:"3.Bc4",color:"w",explanation:"Olasz futó."},
    {from:"f8",to:"c5",notation:"3...Bc5",color:"b",explanation:"Szimmetrikus futó."},
    {from:"c4",to:"f7",notation:"4.Bxf7+",color:"w",explanation:"Jerome-gambit! Futót áldoz f7-re – királyra sakk, kavarodást okoz!"},
    {from:"e8",to:"f7",notation:"4...Kxf7",color:"b",explanation:"Fekete muszáj leütni."},
    {from:"f3",to:"e5",notation:"5.Nxe5+",color:"w",explanation:"Huszár is áldoz e5-re – sakk a királyra!"},
    {from:"f7",to:"e6",notation:"5...Ke6",color:"b",explanation:"Fekete királyának menekülnie kell – de anyagi fölénybe kerül!"},
  ]
},
{
  id:"center-game", name:"Centrumsátor-játék", category:"Nyílt játékok", difficulty:"kezdő",
  shortDesc:"2.d4 – fehér azonnal centrumot akar nyitni, de a vezér korán kijön.",
  info:"A centrumsátor-játék egyszerű de az idő előtt kijövő vezér problémákat okoz fehérnek.",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"d2",to:"d4",notation:"2.d4",color:"w",explanation:"Centrumsátor! Fehér azonnal nyit – de idő előtti lehet."},
    {from:"e5",to:"d4",notation:"2...exd4",color:"b",explanation:"Csere – fekete elfogadja."},
    {from:"d1",to:"d4",notation:"3.Qxd4",color:"w",explanation:"Vezér visszahódítja – de a vezér korán kijön és fenyeget támadhatatni."},
    {from:"b8",to:"c6",notation:"3...Nc6",color:"b",explanation:"Fejlesztés ÉS vezér megtámadása – a4 huszár nyer tempót."},
    {from:"d4",to:"e3",notation:"4.Qe3",color:"w",explanation:"Vezér visszahúzódik."},
    {from:"g8",to:"f6",notation:"4...Nf6",color:"b",explanation:"Fejlesztés, e4 fenyegetés."},
    {from:"b1",to:"c3",notation:"5.Nc3",color:"w",explanation:"Fejlesztés."},
    {from:"f8",to:"b4",notation:"5...Bb4",color:"b",explanation:"Nimzó-jellegű kötés – fekete fejlettségi előnnyel bír."},
  ]
},
{
  id:"wayward-queen", name:"Rebellis Vezér (Scholar's Mate előkészítő)", category:"Nyílt játékok", difficulty:"kezdő",
  shortDesc:"2.Qh5 – fehér f7-re tör a vezérrel. Kezdők ellen gyorsan mattot ad.",
  info:"A tudós matt (Scholar's Mate) a leggyakoribb kezdők közti befejezés. 4 lépéses matt lehetséges!",
  moves:[
    {from:"e2",to:"e4",notation:"1.e4",color:"w",explanation:"Centrum."},
    {from:"e7",to:"e5",notation:"1...e5",color:"b",explanation:"Szimmetria."},
    {from:"d1",to:"h5",notation:"2.Qh5",color:"w",explanation:"Rebellis vezér! Azonnal f7-re néz – trükk, de könnyen kivédhető."},
    {from:"b8",to:"c6",notation:"2...Nc6",color:"b",explanation:"Fejlesztés ÉS f7 védelme – legjobb válasz."},
    {from:"f1",to:"c4",notation:"3.Bc4",color:"w",explanation:"Futó f7-re néz – Scholar's Matt veszély!"},
    {from:"g8",to:"f6",notation:"3...Nf6",color:"b",explanation:"Legjobb! A vezért megtámadja és f7-t védi."},
    {from:"h5",to:"f3",notation:"4.Qf3",color:"w",explanation:"Vezér visszahúzódik – f7 fenyegetés marad."},
    {from:"d7",to:"d5",notation:"4...d5",color:"b",explanation:"Centrumtámadás – visszahódítja a vezér által nyomott területet."},
    {from:"e4",to:"d5",notation:"5.exd5",color:"w",explanation:"Csere."},
    {from:"c6",to:"d4",notation:"5...Nd4",color:"b",explanation:"Lovasugrás – f3 vezért fenyegeti és d5 gyalogot köti."},
  ]
},
];

// ============================================================
// STATE
// ============================================================
let currentOpening = null;
let currentMoveIdx = 0;
let boardState = {};
let selectedCell = null;
let activeTab = 'learn';
let practicePhase = 0; // how many moves shown before player's turn
let playerColor = 'w';
let practiceWrongCount = 0;
let filterCat = 'all';
let completed = JSON.parse(localStorage.getItem('chess_completed') || '[]');

// ============================================================
// BOARD
// ============================================================
// Both colors use the same (white/outline) unicode shapes — color is applied via canvas fill+stroke
const PIECE_UNICODE = {
  'wK':'♔','wQ':'♕','wR':'♖','wB':'♗','wN':'♘','wP':'♙',
  'bK':'♔','bQ':'♕','bR':'♖','bB':'♗','bN':'♘','bP':'♙'
};

const INITIAL_BOARD = {
  'a1':'wR','b1':'wN','c1':'wB','d1':'wQ','e1':'wK','f1':'wB','g1':'wN','h1':'wR',
  'a2':'wP','b2':'wP','c2':'wP','d2':'wP','e2':'wP','f2':'wP','g2':'wP','h2':'wP',
  'a7':'bP','b7':'bP','c7':'bP','d7':'bP','e7':'bP','f7':'bP','g7':'bP','h7':'bP',
  'a8':'bR','b8':'bN','c8':'bB','d8':'bQ','e8':'bK','f8':'bB','g8':'bN','h8':'bR'
};

function colIdx(c) { return 'abcdefgh'.indexOf(c); }
function rowIdx(r) { return 8 - parseInt(r); }
function cellFromXY(file, rank) { return 'abcdefgh'[file] + rank; }

function initBoard() {
  boardState = JSON.parse(JSON.stringify(INITIAL_BOARD));
}

let CELL_SIZE = 60;
function updateCellSize() {
  const wrap = document.getElementById('board-wrap');
  const canvas = document.getElementById('chessboard');
  const maxW = Math.min(480, (wrap ? wrap.offsetWidth : 480) - 24);
  CELL_SIZE = Math.floor(maxW / 8);
  canvas.width = CELL_SIZE * 8;
  canvas.height = CELL_SIZE * 8;
  canvas.style.width = (CELL_SIZE*8)+'px';
  canvas.style.height = (CELL_SIZE*8)+'px';
}

let highlightedFrom = null;
let highlightedTo = null;

function drawBoard() {
  const canvas = document.getElementById('chessboard');
  const ctx = canvas.getContext('2d');
  updateCellSize();

  for (let r = 0; r < 8; r++) {
    for (let f = 0; f < 8; f++) {
      const isLight = (r + f) % 2 === 0;
      const cell = cellFromXY(f, 8-r);
      let bg = isLight ? '#f0d9b5' : '#b58863';

      // Highlight
      if (selectedCell && cell === selectedCell) bg = '#829769';
      else if (highlightedFrom && cell === highlightedFrom) bg = '#cdd46a';
      else if (highlightedTo && cell === highlightedTo) bg = '#aaca44';

      ctx.fillStyle = bg;
      ctx.fillRect(f*CELL_SIZE, r*CELL_SIZE, CELL_SIZE, CELL_SIZE);

      // Piece — both sides use the same outline glyph; color comes from fill+stroke
      const piece = boardState[cell];
      if (piece) {
        const px = f*CELL_SIZE + CELL_SIZE/2;
        const py = r*CELL_SIZE + CELL_SIZE/2;
        const isWhite = piece[0] === 'w';
        const fontSize = Math.floor(CELL_SIZE * 0.72);
        ctx.font = `${fontSize}px serif`;
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.shadowBlur = 0;
        if (isWhite) {
          ctx.strokeStyle = '#3a2a0a';
          ctx.lineWidth = Math.max(1.5, CELL_SIZE * 0.04);
          ctx.fillStyle = '#ffffff';
          ctx.strokeText(PIECE_UNICODE[piece], px, py);
          ctx.fillText(PIECE_UNICODE[piece], px, py);
        } else {
          ctx.strokeStyle = '#f0d9b5';
          ctx.lineWidth = Math.max(1.5, CELL_SIZE * 0.04);
          ctx.fillStyle = '#1a1008';
          ctx.strokeText(PIECE_UNICODE[piece], px, py);
          ctx.fillText(PIECE_UNICODE[piece], px, py);
        }
      }
    }
  }

  // Rank/file labels
  const rl = document.getElementById('rank-labels');
  const fl = document.getElementById('file-labels');
  if (rl) {
    rl.innerHTML = '';
    rl.style.top = 0;
    rl.style.left = '-18px';
    rl.style.height = CELL_SIZE*8+'px';
    for (let r=0;r<8;r++){
      const s = document.createElement('span');
      s.textContent = 8-r;
      s.style.height = CELL_SIZE+'px';
      s.style.display = 'flex';
      s.style.alignItems = 'center';
      rl.appendChild(s);
    }
  }
  if (fl) {
    fl.innerHTML = '';
    fl.style.width = CELL_SIZE*8+'px';
    for (let f=0;f<8;f++){
      const s = document.createElement('span');
      s.textContent = 'abcdefgh'[f];
      s.style.width = CELL_SIZE+'px';
      s.style.display = 'inline-block';
      s.style.textAlign = 'center';
      fl.appendChild(s);
    }
  }
}

// ============================================================
// MOVE APPLICATION
// ============================================================
function applyMove(from, to) {
  if (boardState[from]) {
    const piece = boardState[from];
    delete boardState[from];
    boardState[to] = piece;
    // Castling
    if (piece === 'wK' && from === 'e1' && to === 'g1') { boardState['f1'] = 'wR'; delete boardState['h1']; }
    if (piece === 'wK' && from === 'e1' && to === 'c1') { boardState['d1'] = 'wR'; delete boardState['a1']; }
    if (piece === 'bK' && from === 'e8' && to === 'g8') { boardState['f8'] = 'bR'; delete boardState['h8']; }
    if (piece === 'bK' && from === 'e8' && to === 'c8') { boardState['d8'] = 'bR'; delete boardState['a8']; }
  }
}

function replayToMove(idx) {
  initBoard();
  for (let i = 0; i <= idx; i++) {
    const m = currentOpening.moves[i];
    applyMove(m.from, m.to);
  }
}

// ============================================================
// NAVIGATION
// ============================================================
function goHome() {
  document.getElementById('home-screen').style.display = 'block';
  document.getElementById('learn-screen').style.display = 'none';
  document.getElementById('topbar-badge').style.display = 'none';
  document.getElementById('topbar-title').textContent = 'Sakk Megnyitások';
  document.getElementById('topbar-actions').innerHTML = '';
  currentOpening = null;
  renderCards();
  updateProgress();
}

function openOpening(id) {
  currentOpening = openings.find(o => o.id === id);
  if (!currentOpening) return;
  currentMoveIdx = -1;
  activeTab = 'learn';
  initBoard();
  selectedCell = null;
  highlightedFrom = null;
  highlightedTo = null;

  document.getElementById('home-screen').style.display = 'none';
  document.getElementById('learn-screen').style.display = 'block';

  const badge = document.getElementById('topbar-badge');
  badge.style.display = '';
  badge.textContent = currentOpening.difficulty;
  badge.className = 'topbar-badge ' + ({kezdő:'beginner',haladó:'intermediate',mester:'advanced'}[currentOpening.difficulty]||'');
  document.getElementById('topbar-title').textContent = currentOpening.name;
  document.getElementById('topbar-actions').innerHTML = `
    <button class="btn btn-outline" onclick="goHome()">← Vissza</button>
    <button class="btn btn-outline active-mode" id="tab-btn-learn" onclick="switchTab('learn')">Tanulás</button>
    <button class="btn btn-outline" id="tab-btn-practice" onclick="switchTab('practice')">Gyakorlás</button>
  `;

  updateCellSize();
  drawBoard();
  renderNotation();
  renderLearnPanel();
  renderSidebar();

  // Highlight active in sidebar
  document.querySelectorAll('.sidebar-item').forEach(el => {
    el.classList.toggle('active', el.dataset.id === id);
  });
}

function switchTab(tab) {
  activeTab = tab;
  document.getElementById('tab-learn').classList.toggle('active', tab==='learn');
  document.getElementById('tab-practice').classList.toggle('active', tab==='practice');
  const lb = document.getElementById('tab-btn-learn');
  const pb = document.getElementById('tab-btn-practice');
  if(lb) lb.classList.toggle('active-mode', tab==='learn');
  if(pb) pb.classList.toggle('active-mode', tab==='practice');
  if (tab==='learn') {
    renderLearnPanel();
  } else {
    initPractice();
  }
}

// ============================================================
// LEARN TAB
// ============================================================
function renderLearnPanel() {
  const idx = currentMoveIdx;
  const move = idx >= 0 ? currentOpening.moves[idx] : null;
  const total = currentOpening.moves.length;
  const isLast = idx >= total - 1;

  let html = `<div class="opening-info"><strong>${currentOpening.name}</strong>${currentOpening.info}</div>`;

  html += `<div class="move-controls">
    <button class="btn btn-prev" onclick="prevMove()">← Előző</button>
    <button class="btn btn-next" ${isLast?'disabled style="opacity:.5"':''} onclick="nextMove()">${isLast?'Vége ✓':'Következő →'}</button>
  </div>`;

  if (move) {
    html += `<div class="explanation-box">
      <div class="expl-move">${move.notation} <span style="font-size:22px">${PIECE_UNICODE[boardState[move.to]] || ''}</span></div>
      <div class="expl-text">${move.explanation}</div>
      <div class="expl-tip">${move.color==='w'?'⬜ Fehér':'⬛ Fekete'} lépés • ${idx+1}/${total}</div>
    </div>`;
  } else {
    html += `<div class="explanation-box"><div class="expl-move">Kiindulóhelyzet</div><div class="expl-text">Nyomj a "Következő" gombra az első lépéshez. Figyeld a tábla kiemeléseit!</div></div>`;
  }

  // Moves list
  html += `<div class="moves-list">`;
  currentOpening.moves.forEach((m,i) => {
    html += `<div class="move-item ${i===idx?'current':i<idx?'passed':''}" onclick="jumpTo(${i})">
      <span class="move-piece">${m.color==='w'?'⬜':'⬛'}</span>
      <span class="move-notation">${m.notation}</span>
    </div>`;
  });
  html += `</div>`;

  if (isLast && idx >= 0) {
    html += `<div class="completion-box show" style="margin-top:16px">
      <div class="completion-icon">🏆</div>
      <div class="completion-title">Megnyitás teljesítve!</div>
      <div class="completion-text">Elsajátítottad a ${currentOpening.name} megnyitást. Próbáld ki a Gyakorlás módot is!</div>
      <button class="btn btn-gold" onclick="markDone(); switchTab('practice')">Tovább a Gyakorláshoz →</button>
    </div>`;
  }

  document.getElementById('panel-content').innerHTML = html;
  renderNotation();
}

function nextMove() {
  if (!currentOpening) return;
  if (currentMoveIdx < currentOpening.moves.length - 1) {
    currentMoveIdx++;
    const m = currentOpening.moves[currentMoveIdx];
    applyMove(m.from, m.to);
    highlightedFrom = m.from;
    highlightedTo = m.to;
    drawBoard();
    renderLearnPanel();
    if (currentMoveIdx === currentOpening.moves.length - 1) markDone();
  }
}

function prevMove() {
  if (!currentOpening) return;
  if (currentMoveIdx > -1) {
    currentMoveIdx--;
    if (currentMoveIdx >= 0) {
      replayToMove(currentMoveIdx);
      const m = currentOpening.moves[currentMoveIdx];
      highlightedFrom = m.from;
      highlightedTo = m.to;
    } else {
      initBoard();
      highlightedFrom = null;
      highlightedTo = null;
    }
    drawBoard();
    renderLearnPanel();
  }
}

function jumpTo(i) {
  currentMoveIdx = i;
  replayToMove(i);
  const m = currentOpening.moves[i];
  highlightedFrom = m.from;
  highlightedTo = m.to;
  drawBoard();
  renderLearnPanel();
}

function markDone() {
  if (!completed.includes(currentOpening.id)) {
    completed.push(currentOpening.id);
    localStorage.setItem('chess_completed', JSON.stringify(completed));
    updateProgress();
    renderSidebar();
  }
}

// ============================================================
// PRACTICE TAB
// ============================================================
function initPractice() {
  practicePhase = Math.min(3, Math.floor(currentOpening.moves.length / 2));
  practiceWrongCount = 0;
  playerColor = currentOpening.moves[practicePhase]?.color === 'w' ? 'w' : 'b';
  initBoard();
  // Replay up to practicePhase
  for (let i = 0; i < practicePhase; i++) {
    applyMove(currentOpening.moves[i].from, currentOpening.moves[i].to);
  }
  currentMoveIdx = practicePhase - 1;
  highlightedFrom = null; highlightedTo = null;
  selectedCell = null;
  drawBoard();
  renderPracticePanel();
  renderNotation();
}

function renderPracticePanel() {
  const nextMoveNeeded = currentOpening.moves[currentMoveIdx + 1];
  const isFinished = currentMoveIdx >= currentOpening.moves.length - 1;

  let statusClass = 'waiting', statusText = '';
  if (isFinished) {
    statusClass = 'correct';
    statusText = '🏆 Megnyitást teljesítetted! Kattints az "Újraindítás" gombra.';
  } else if (nextMoveNeeded) {
    statusText = `${nextMoveNeeded.color==='w'?'⬜ Fehér':'⬛ Fekete'} következik – kattints a figurára, majd a célmezőre.`;
  }

  let html = `
    <div class="opening-info"><strong>Gyakorlás: ${currentOpening.name}</strong>Az első ${practicePhase} lépés automatikusan megtörtént. Folytasd a megnyitást!</div>
    <div class="color-choice">
      <div class="color-btn ${playerColor==='w'?'active':''}" onclick="setPlayerColor('w')">⬜ Fehér</div>
      <div class="color-btn ${playerColor==='b'?'active':''}" onclick="setPlayerColor('b')">⬛ Fekete</div>
    </div>
    <div class="practice-status ${statusClass}" id="practice-status">${statusText || 'Tedd meg a következő lépést!'}</div>
    <button class="hint-btn" onclick="showHint()">💡 Tipp mutatása</button>
    <button class="btn btn-outline" style="width:100%;margin-bottom:12px" onclick="initPractice()">↺ Újraindítás</button>
  `;

  html += `<div class="moves-list">`;
  currentOpening.moves.forEach((m,i) => {
    html += `<div class="move-item ${i<=currentMoveIdx?'passed':''} ${i===currentMoveIdx?'current':''}">
      <span class="move-piece">${m.color==='w'?'⬜':'⬛'}</span>
      <span class="move-notation">${i<=currentMoveIdx?m.notation:'???'}</span>
    </div>`;
  });
  html += `</div>`;

  if (isFinished) {
    html += `<div class="completion-box show" style="margin-top:16px">
      <div class="completion-icon">🎯</div>
      <div class="completion-title">Kiváló!</div>
      <div class="completion-text">Tökéletesen lejátszottad a megnyitást. Próbáld meg emlékezetből is!</div>
    </div>`;
  }

  document.getElementById('panel-content').innerHTML = html;
}

function setPlayerColor(c) {
  playerColor = c;
  initPractice();
}

function showHint() {
  const nextMove = currentOpening.moves[currentMoveIdx + 1];
  if (!nextMove) return;
  practiceWrongCount++;
  const el = document.getElementById('practice-status');
  if (el) {
    el.className = 'practice-status hint';
    el.textContent = `💡 Tipp: ${nextMove.notation} – ${nextMove.explanation.substring(0,80)}...`;
  }
  highlightedFrom = nextMove.from;
  drawBoard();
}

// ============================================================
// CANVAS CLICK (PRACTICE)
// ============================================================
document.getElementById('chessboard').addEventListener('click', function(e) {
  if (activeTab !== 'practice' || !currentOpening) return;
  const canvas = e.target;
  const rect = canvas.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  const file = Math.floor(x / CELL_SIZE);
  const rank = 8 - Math.floor(y / CELL_SIZE);
  const cell = cellFromXY(file, rank);

  const nextMove = currentOpening.moves[currentMoveIdx + 1];
  if (!nextMove) return;

  if (!selectedCell) {
    // Check if there is a piece of the correct color here
    if (boardState[cell] && boardState[cell][0] === nextMove.color) {
      selectedCell = cell;
      highlightedFrom = cell;
      drawBoard();
    }
  } else {
    if (cell === selectedCell) {
      selectedCell = null;
      highlightedFrom = null;
      drawBoard();
      return;
    }
    // Attempt move
    if (selectedCell === nextMove.from && cell === nextMove.to) {
      // Correct!
      applyMove(nextMove.from, nextMove.to);
      currentMoveIdx++;
      highlightedFrom = nextMove.from;
      highlightedTo = nextMove.to;
      selectedCell = null;
      drawBoard();
      const el = document.getElementById('practice-status');
      if (el) {
        el.className = 'practice-status correct';
        el.textContent = `✓ Helyes! ${nextMove.explanation}`;
      }

      // Auto-play opponent's response if player's next opponent move exists
      setTimeout(() => {
        const opp = currentOpening.moves[currentMoveIdx + 1];
        if (opp && opp.color !== nextMove.color) {
          applyMove(opp.from, opp.to);
          currentMoveIdx++;
          highlightedFrom = opp.from;
          highlightedTo = opp.to;
          drawBoard();
          renderPracticePanel();
          renderNotation();
        } else {
          renderPracticePanel();
          renderNotation();
        }
      }, 700);
    } else {
      // Wrong
      const el = document.getElementById('practice-status');
      if (el) {
        el.className = 'practice-status wrong';
        el.textContent = `✗ Ez nem a helyes lépés. Próbáld újra! (${selectedCell}→${cell} helyett ${nextMove.from}→${nextMove.to} kellett)`;
      }
      selectedCell = null;
      highlightedFrom = null;
      drawBoard();
    }
  }
});

// ============================================================
// NOTATION
// ============================================================
function renderNotation() {
  const container = document.getElementById('notation-moves');
  if (!currentOpening) { container.innerHTML = ''; return; }
  let html = '';
  let moveNum = 1;
  currentOpening.moves.forEach((m, i) => {
    if (m.color === 'w') {
      html += `<span class="notation-move move-num">${moveNum}.</span>`;
      moveNum++;
    }
    const cls = i === currentMoveIdx ? 'current' : i < currentMoveIdx ? 'passed' : '';
    html += `<span class="notation-move ${cls}" onclick="activeTab==='learn'&&jumpTo(${i})">${m.notation}</span>`;
  });
  container.innerHTML = html;
}

// ============================================================
// HOME / CARDS
// ============================================================
function renderCards() {
  const container = document.getElementById('openings-container');
  const cats = [...new Set(openings.map(o=>o.category))];
  let html = '';
  cats.forEach(cat => {
    const filtered = openings.filter(o => {
      if (filterCat === 'all') return o.category === cat;
      if (filterCat === 'done') return o.category === cat && completed.includes(o.id);
      return o.category === cat && o.category === filterCat;
    });
    if (!filtered.length) return;
    html += `<div class="cat-section"><div class="cat-title">${cat}</div><div class="cards-grid">`;
    filtered.forEach(o => {
      const done = completed.includes(o.id);
      html += `<div class="card ${done?'completed':''}" onclick="openOpening('${o.id}')">
        ${done?'<div class="card-done-badge">✓</div>':''}
        <div class="card-name">${o.name}</div>
        <div class="card-desc">${o.shortDesc}</div>
        <div class="card-meta">
          <span class="diff-badge ${o.difficulty}">${o.difficulty}</span>
          <span class="card-steps">${o.moves.length} lépés</span>
        </div>
        <button class="card-learn-btn">Tanulás →</button>
      </div>`;
    });
    html += `</div></div>`;
  });
  container.innerHTML = html;
  document.getElementById('stat-done').textContent = completed.length;
}

function setFilter(cat, btn) {
  filterCat = cat;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  renderCards();
}

// ============================================================
// SIDEBAR
// ============================================================
function renderSidebar() {
  const cats = [...new Set(openings.map(o=>o.category))];
  const container = document.getElementById('sidebar-cats');
  let html = `<div class="cat-group">
    <div class="cat-header" onclick="goHome()">🏠 Főoldal</div>
  </div>`;
  cats.forEach(cat => {
    html += `<div class="cat-group"><div class="cat-header">${cat}</div><div class="cat-items">`;
    openings.filter(o=>o.category===cat).forEach(o => {
      const done = completed.includes(o.id);
      const active = currentOpening && currentOpening.id === o.id;
      html += `<div class="sidebar-item ${active?'active':''} ${done?'done':''}" data-id="${o.id}" onclick="openOpening('${o.id}')">
        ${done?'✓ ':''}${o.name}
      </div>`;
    });
    html += `</div></div>`;
  });
  container.innerHTML = html;
}

function filterSidebar(q) {
  q = q.toLowerCase();
  document.querySelectorAll('.sidebar-item').forEach(el => {
    el.style.display = el.textContent.toLowerCase().includes(q) ? '' : 'none';
  });
}

function updateProgress() {
  const n = completed.length;
  const total = openings.length;
  document.getElementById('progress-text').textContent = `${n} / ${total}`;
  document.getElementById('progress-fill').style.width = (n/total*100)+'%';
}

// ============================================================
// SIDEBAR TOGGLE (MOBILE)
// ============================================================
function toggleSidebar() {
  const sb = document.getElementById('sidebar');
  const ov = document.getElementById('sidebar-overlay');
  const open = sb.classList.toggle('open');
  ov.classList.toggle('show', open);
}

// ============================================================
// INIT
// ============================================================
window.addEventListener('resize', () => {
  if (currentOpening) drawBoard();
});

renderCards();
renderSidebar();
updateProgress();
</script>
</body>
</html>
