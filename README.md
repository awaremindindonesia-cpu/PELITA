<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>PELITA — Platform Evaluasi Lintas-Kondisi Mental Mahasiswa</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Lora:ital,wght@0,400;0,500;1,400&display=swap" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
/* ─── RESET & BASE ─────────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --amethyst:    #5C3D8F;
  --amethyst-dk: #2D1B5E;
  --amethyst-lt: #7B5CAD;
  --lavender:    #C4B0E8;
  --lavender-lt: #EDE8F9;
  --sky:         #A8C8E8;
  --sky-lt:      #E8F4FF;
  --mint:        #B2DED6;
  --mint-dk:     #4DA89A;
  --cream:       #F5F0FF;
  --white:       #FFFFFF;
  --text-dk:     #1A1035;
  --text-md:     #4A3F6B;
  --text-lt:     #8B7DAB;
  --border:      #DDD5F3;
  --danger:      #E05C6E;
  --warning:     #F2A65A;
  --success:     #5AB08A;
  --radius-sm:   8px;
  --radius-md:   14px;
  --radius-lg:   22px;
  --shadow-sm:   0 2px 8px rgba(92,61,143,.10);
  --shadow-md:   0 6px 24px rgba(92,61,143,.15);
  --shadow-lg:   0 16px 48px rgba(92,61,143,.22);
  --sidebar-w:   240px;
  --transition:  0.25s cubic-bezier(.4,0,.2,1);
}
html { scroll-behavior: smooth; }
body {
  font-family: 'Plus Jakarta Sans', sans-serif;
  background: var(--cream);
  color: var(--text-dk);
  min-height: 100vh;
  display: flex;
  overflow-x: hidden;
}

/* ─── SCROLLBAR ────────────────────────────────────── */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--lavender); border-radius: 99px; }

/* ─── SIDEBAR ──────────────────────────────────────── */
#sidebar {
  width: var(--sidebar-w);
  min-height: 100vh;
  background: linear-gradient(175deg, var(--amethyst-dk) 0%, var(--amethyst) 60%, #6E45A8 100%);
  display: flex;
  flex-direction: column;
  position: fixed;
  left: 0; top: 0;
  z-index: 100;
  box-shadow: 4px 0 32px rgba(45,27,94,.25);
  transition: transform var(--transition);
}
.sidebar-brand {
  padding: 28px 22px 20px;
  border-bottom: 1px solid rgba(255,255,255,.12);
}
.brand-icon {
  width: 44px; height: 44px;
  background: rgba(255,255,255,.15);
  border-radius: 12px;
  display: flex; align-items: center; justify-content: center;
  margin-bottom: 10px;
  font-size: 22px;
}
.brand-name {
  font-size: 22px; font-weight: 800;
  color: #fff;
  letter-spacing: .5px;
  line-height: 1.1;
}
.brand-tagline {
  font-size: 10px; color: var(--lavender);
  font-weight: 500; line-height: 1.4;
  margin-top: 4px; text-transform: uppercase;
  letter-spacing: .6px;
}
.sidebar-nav {
  padding: 16px 12px;
  flex: 1;
}
.nav-section-label {
  font-size: 9.5px; color: rgba(196,176,232,.55);
  text-transform: uppercase; letter-spacing: 1.2px;
  font-weight: 700; padding: 10px 10px 6px;
}
.nav-item {
  display: flex; align-items: center; gap: 10px;
  padding: 10px 14px;
  border-radius: var(--radius-sm);
  color: rgba(255,255,255,.75);
  font-size: 13.5px; font-weight: 500;
  cursor: pointer;
  transition: background var(--transition), color var(--transition);
  margin-bottom: 2px;
  user-select: none;
}
.nav-item:hover { background: rgba(255,255,255,.10); color: #fff; }
.nav-item.active {
  background: rgba(255,255,255,.18);
  color: #fff;
  font-weight: 700;
}
.nav-item .nav-icon { font-size: 16px; width: 20px; text-align: center; }
.sidebar-footer {
  padding: 14px 22px 20px;
  border-top: 1px solid rgba(255,255,255,.10);
}
.badge-gratis {
  display: inline-flex; align-items: center; gap: 6px;
  background: rgba(255,255,255,.12);
  border: 1px solid rgba(255,255,255,.18);
  color: var(--mint);
  font-size: 10.5px; font-weight: 700;
  padding: 5px 10px; border-radius: 99px;
  text-transform: uppercase; letter-spacing: .8px;
}

/* ─── MAIN CONTENT ─────────────────────────────────── */
#main {
  margin-left: var(--sidebar-w);
  flex: 1;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* ─── TOP BAR ──────────────────────────────────────── */
#topbar {
  background: rgba(255,255,255,.85);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  padding: 14px 32px;
  display: flex; align-items: center; justify-content: space-between;
  position: sticky; top: 0; z-index: 50;
}
.topbar-date {
  font-size: 13px; color: var(--text-lt);
  font-weight: 500;
}
.topbar-date span { color: var(--amethyst); font-weight: 700; }
.avatar-btn {
  width: 36px; height: 36px;
  background: linear-gradient(135deg, var(--amethyst), var(--amethyst-lt));
  border-radius: 50%; border: none; cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  color: #fff; font-size: 14px; font-weight: 700;
  box-shadow: var(--shadow-sm);
}
.hamburger-btn {
  display: none; background: none; border: none;
  font-size: 22px; cursor: pointer; color: var(--amethyst);
  margin-right: 12px;
}

/* ─── PAGES ────────────────────────────────────────── */
.page { display: none; padding: 28px 32px 48px; animation: fadeIn .3s ease; }
.page.active { display: block; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

/* ─── PAGE HEADER ──────────────────────────────────── */
.page-header { margin-bottom: 28px; }
.page-title { font-size: 26px; font-weight: 800; color: var(--amethyst-dk); line-height: 1.2; }
.page-subtitle { font-size: 14px; color: var(--text-lt); margin-top: 5px; }

/* ─── WELLBEING HERO ───────────────────────────────── */
.wellbeing-hero {
  background: linear-gradient(130deg, var(--amethyst-dk) 0%, var(--amethyst) 50%, #7B5CAD 100%);
  border-radius: var(--radius-lg);
  padding: 28px 32px;
  color: #fff;
  margin-bottom: 24px;
  position: relative;
  overflow: hidden;
}
.wellbeing-hero::before {
  content: '';
  position: absolute; top: -40px; right: -40px;
  width: 200px; height: 200px;
  background: rgba(255,255,255,.05);
  border-radius: 50%;
}
.wellbeing-hero::after {
  content: '';
  position: absolute; bottom: -60px; right: 60px;
  width: 160px; height: 160px;
  background: rgba(196,176,232,.1);
  border-radius: 50%;
}
.hero-top { display: flex; align-items: flex-start; justify-content: space-between; gap: 16px; flex-wrap: wrap; }
.hero-greeting { font-size: 13px; color: var(--lavender); font-weight: 500; margin-bottom: 4px; }
.hero-name { font-size: 22px; font-weight: 800; line-height: 1.2; }
.hero-status-badge {
  background: rgba(255,255,255,.15);
  border: 1px solid rgba(255,255,255,.25);
  border-radius: 99px;
  padding: 6px 14px;
  font-size: 12px; font-weight: 700;
  display: flex; align-items: center; gap: 6px;
  white-space: nowrap;
}
.status-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--mint);
  box-shadow: 0 0 0 2px rgba(178,222,214,.3);
  animation: pulse 2s infinite;
}
@keyframes pulse {
  0%, 100% { box-shadow: 0 0 0 0 rgba(178,222,214,.5); }
  50%       { box-shadow: 0 0 0 5px rgba(178,222,214,0); }
}
.hero-metrics { display: flex; gap: 20px; margin-top: 22px; flex-wrap: wrap; }
.hero-metric { text-align: center; }
.hero-metric-val { font-size: 24px; font-weight: 800; line-height: 1; }
.hero-metric-lbl { font-size: 10px; color: var(--lavender); font-weight: 500; text-transform: uppercase; letter-spacing: .6px; margin-top: 3px; }
.hero-divider { width: 1px; background: rgba(255,255,255,.15); margin: 0 4px; }

/* ─── CARDS GRID ───────────────────────────────────── */
.cards-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px,1fr)); gap: 16px; margin-bottom: 24px; }
.stat-card {
  background: var(--white);
  border-radius: var(--radius-md);
  padding: 18px 20px;
  border: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  transition: box-shadow var(--transition), transform var(--transition);
}
.stat-card:hover { box-shadow: var(--shadow-md); transform: translateY(-2px); }
.stat-card-icon { font-size: 22px; margin-bottom: 10px; }
.stat-card-val { font-size: 28px; font-weight: 800; color: var(--amethyst-dk); line-height: 1; }
.stat-card-lbl { font-size: 12px; color: var(--text-lt); font-weight: 500; margin-top: 4px; }
.stat-card-sub { font-size: 11px; color: var(--text-lt); margin-top: 6px; }
.trend-up { color: var(--success); font-weight: 700; }
.trend-dn { color: var(--danger); font-weight: 700; }

/* ─── TWO-COL LAYOUT ───────────────────────────────── */
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 24px; }
@media (max-width: 900px) { .two-col { grid-template-columns: 1fr; } }

/* ─── CARD ─────────────────────────────────────────── */
.card {
  background: var(--white);
  border-radius: var(--radius-md);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  overflow: hidden;
}
.card-header {
  padding: 16px 20px 12px;
  border-bottom: 1px solid var(--lavender-lt);
  display: flex; align-items: center; gap: 10px;
}
.card-header-icon { font-size: 18px; }
.card-title { font-size: 14px; font-weight: 700; color: var(--amethyst-dk); }
.card-subtitle { font-size: 11.5px; color: var(--text-lt); margin-top: 1px; }
.card-body { padding: 20px; }

/* ─── HORMONE SLIDERS ──────────────────────────────── */
.hormone-grid { display: grid; gap: 20px; }
.hormone-item {}
.hormone-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
.hormone-label { display: flex; align-items: center; gap: 8px; font-size: 13px; font-weight: 700; color: var(--text-dk); }
.hormone-emoji { font-size: 16px; }
.hormone-desc { font-size: 10.5px; color: var(--text-lt); font-weight: 400; display: block; margin-top: 1px; }
.hormone-value-badge {
  font-size: 13px; font-weight: 800;
  min-width: 46px; text-align: center;
  padding: 3px 10px;
  border-radius: 99px;
  transition: background var(--transition), color var(--transition);
}
.slider-track { position: relative; }
input[type=range].hormone-slider {
  -webkit-appearance: none;
  appearance: none;
  width: 100%; height: 8px;
  border-radius: 99px;
  outline: none;
  cursor: pointer;
  transition: height var(--transition);
}
input[type=range].hormone-slider:focus { height: 10px; }
input[type=range].hormone-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 20px; height: 20px;
  border-radius: 50%;
  background: var(--white);
  border: 3px solid currentColor;
  box-shadow: 0 2px 8px rgba(0,0,0,.15);
  cursor: pointer;
  transition: transform var(--transition), box-shadow var(--transition);
}
input[type=range].hormone-slider::-webkit-slider-thumb:hover {
  transform: scale(1.2);
  box-shadow: 0 4px 12px rgba(0,0,0,.2);
}
.slider-ticks { display: flex; justify-content: space-between; margin-top: 3px; }
.slider-tick { font-size: 9px; color: var(--text-lt); font-weight: 500; }

/* ─── DIARY AREA ───────────────────────────────────── */
.diary-label { font-size: 13px; font-weight: 700; color: var(--text-dk); margin-bottom: 8px; display: flex; align-items: center; gap: 6px; }
textarea.diary-input {
  width: 100%;
  min-height: 120px;
  border: 1.5px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 14px;
  font-family: 'Lora', serif;
  font-size: 14px;
  line-height: 1.7;
  color: var(--text-dk);
  background: var(--lavender-lt);
  resize: vertical;
  transition: border-color var(--transition), box-shadow var(--transition);
  outline: none;
}
textarea.diary-input:focus {
  border-color: var(--amethyst);
  box-shadow: 0 0 0 3px rgba(92,61,143,.12);
  background: var(--white);
}
textarea.diary-input::placeholder { color: var(--text-lt); font-style: italic; }

/* ─── TAGS ─────────────────────────────────────────── */
.tags-label { font-size: 13px; font-weight: 700; color: var(--text-dk); margin: 16px 0 8px; display: flex; align-items: center; gap: 6px; }
.tags-grid { display: flex; flex-wrap: wrap; gap: 8px; }
.tag-chip {
  padding: 6px 13px;
  border-radius: 99px;
  font-size: 12px; font-weight: 600;
  border: 1.5px solid var(--border);
  background: var(--white);
  color: var(--text-md);
  cursor: pointer;
  transition: all var(--transition);
  user-select: none;
}
.tag-chip:hover { border-color: var(--amethyst); color: var(--amethyst); background: var(--lavender-lt); }
.tag-chip.selected {
  background: var(--amethyst);
  border-color: var(--amethyst);
  color: #fff;
  box-shadow: 0 2px 8px rgba(92,61,143,.25);
}

/* ─── MOOD EMOJI SELECTOR ──────────────────────────── */
.mood-row { display: flex; gap: 10px; margin-bottom: 16px; flex-wrap: wrap; }
.mood-option {
  flex: 1; min-width: 60px;
  text-align: center;
  padding: 10px 6px;
  border-radius: var(--radius-sm);
  border: 2px solid var(--border);
  cursor: pointer;
  transition: all var(--transition);
  user-select: none;
}
.mood-option:hover { border-color: var(--lavender); background: var(--lavender-lt); }
.mood-option.selected { border-color: var(--amethyst); background: var(--lavender-lt); }
.mood-emoji { font-size: 26px; }
.mood-lbl { font-size: 10px; color: var(--text-lt); font-weight: 600; margin-top: 3px; }

/* ─── SAVE BUTTON ──────────────────────────────────── */
.btn-save {
  width: 100%;
  padding: 15px;
  background: linear-gradient(135deg, var(--amethyst) 0%, var(--amethyst-lt) 100%);
  color: #fff;
  border: none;
  border-radius: var(--radius-md);
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-size: 15px; font-weight: 700;
  cursor: pointer;
  box-shadow: 0 4px 16px rgba(92,61,143,.35);
  transition: transform var(--transition), box-shadow var(--transition), opacity var(--transition);
  display: flex; align-items: center; justify-content: center; gap: 8px;
  margin-top: 8px;
}
.btn-save:hover { transform: translateY(-2px); box-shadow: 0 8px 24px rgba(92,61,143,.45); }
.btn-save:active { transform: translateY(0); }

/* ─── HISTORY LIST ─────────────────────────────────── */
.entry-list { display: flex; flex-direction: column; gap: 12px; }
.entry-item {
  padding: 14px 16px;
  background: var(--lavender-lt);
  border-radius: var(--radius-sm);
  border-left: 4px solid var(--amethyst);
  transition: box-shadow var(--transition);
  cursor: pointer;
}
.entry-item:hover { box-shadow: var(--shadow-sm); }
.entry-date { font-size: 11px; color: var(--text-lt); font-weight: 600; margin-bottom: 4px; }
.entry-preview { font-family: 'Lora', serif; font-size: 13px; color: var(--text-md); line-height: 1.5; }
.entry-tags { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 8px; }
.entry-tag { font-size: 10px; background: var(--white); color: var(--amethyst); padding: 2px 8px; border-radius: 99px; border: 1px solid var(--lavender); font-weight: 600; }
.entry-hormones { display: flex; gap: 12px; margin-top: 10px; flex-wrap: wrap; }
.entry-hormone { font-size: 10.5px; color: var(--text-lt); font-weight: 600; }
.entry-hormone span { font-weight: 800; color: var(--text-dk); }

/* ─── ANALYTICS ────────────────────────────────────── */
.chart-container { position: relative; height: 260px; }
.analytics-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 20px; }
@media (max-width: 700px) { .analytics-grid { grid-template-columns: 1fr; } }
.analytics-stat {
  padding: 16px;
  background: var(--lavender-lt);
  border-radius: var(--radius-sm);
  border: 1px solid var(--border);
}
.analytics-stat-val { font-size: 22px; font-weight: 800; color: var(--amethyst-dk); }
.analytics-stat-lbl { font-size: 11px; color: var(--text-lt); font-weight: 500; margin-top: 2px; }

/* ─── KONSELING PAGE ───────────────────────────────── */
.konseling-card {
  background: var(--white);
  border-radius: var(--radius-lg);
  border: 1px solid var(--border);
  padding: 24px;
  box-shadow: var(--shadow-sm);
  margin-bottom: 16px;
}
.konselor-header { display: flex; align-items: center; gap: 14px; margin-bottom: 12px; }
.konselor-avatar {
  width: 52px; height: 52px;
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 22px;
  flex-shrink: 0;
}
.konselor-name { font-size: 16px; font-weight: 700; color: var(--text-dk); }
.konselor-role { font-size: 12px; color: var(--text-lt); font-weight: 500; }
.btn-jadwal {
  width: 100%;
  padding: 11px;
  background: var(--lavender-lt);
  color: var(--amethyst);
  border: 1.5px solid var(--lavender);
  border-radius: var(--radius-sm);
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-size: 13px; font-weight: 700;
  cursor: pointer;
  transition: all var(--transition);
}
.btn-jadwal:hover { background: var(--amethyst); color: #fff; border-color: var(--amethyst); }
.schedule-form { margin-top: 12px; display: none; }
.schedule-form.open { display: block; }
.form-group { margin-bottom: 12px; }
.form-label { font-size: 12px; font-weight: 700; color: var(--text-md); margin-bottom: 5px; display: block; }
.form-input {
  width: 100%;
  padding: 9px 12px;
  border: 1.5px solid var(--border);
  border-radius: var(--radius-sm);
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-size: 13px;
  color: var(--text-dk);
  outline: none;
  transition: border-color var(--transition), box-shadow var(--transition);
}
.form-input:focus { border-color: var(--amethyst); box-shadow: 0 0 0 3px rgba(92,61,143,.1); }
.btn-confirm-jadwal {
  width: 100%; padding: 11px;
  background: linear-gradient(135deg, var(--amethyst), var(--amethyst-lt));
  color: #fff; border: none;
  border-radius: var(--radius-sm);
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-size: 13px; font-weight: 700;
  cursor: pointer;
  transition: all var(--transition);
}
.btn-confirm-jadwal:hover { opacity: .9; transform: translateY(-1px); }

/* ─── RESOURCES ────────────────────────────────────── */
.resource-item {
  display: flex; align-items: flex-start; gap: 14px;
  padding: 16px;
  background: var(--white);
  border-radius: var(--radius-md);
  border: 1px solid var(--border);
  margin-bottom: 12px;
  transition: box-shadow var(--transition), transform var(--transition);
}
.resource-item:hover { box-shadow: var(--shadow-md); transform: translateY(-2px); }
.resource-icon { font-size: 28px; flex-shrink: 0; }
.resource-title { font-size: 14px; font-weight: 700; color: var(--amethyst-dk); }
.resource-desc { font-size: 12px; color: var(--text-lt); margin-top: 3px; line-height: 1.5; }
.resource-link {
  display: inline-block; margin-top: 8px;
  font-size: 12px; font-weight: 700; color: var(--amethyst);
  text-decoration: none; cursor: pointer;
}

/* ─── EMPTY STATE ──────────────────────────────────── */
.empty-state { text-align: center; padding: 40px 20px; }
.empty-state-icon { font-size: 48px; margin-bottom: 12px; }
.empty-state-text { font-size: 14px; color: var(--text-lt); font-weight: 500; }

/* ─── MODAL OVERLAY ────────────────────────────────── */
#modal-overlay {
  position: fixed; inset: 0;
  background: rgba(45,27,94,.55);
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
  z-index: 1000;
  display: flex; align-items: center; justify-content: center;
  padding: 20px;
  opacity: 0; pointer-events: none;
  transition: opacity .35s ease;
}
#modal-overlay.visible { opacity: 1; pointer-events: all; }

#modal-box {
  background: var(--white);
  border-radius: var(--radius-lg);
  padding: 36px;
  max-width: 460px; width: 100%;
  box-shadow: var(--shadow-lg);
  transform: translateY(20px) scale(.97);
  transition: transform .35s cubic-bezier(.34,1.56,.64,1);
  text-align: center;
  position: relative;
}
#modal-overlay.visible #modal-box { transform: translateY(0) scale(1); }

.modal-wave {
  width: 80px; height: 80px;
  background: linear-gradient(135deg, #FFE8EC, #FFD0D8);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 36px;
  margin: 0 auto 20px;
  animation: float 3s ease-in-out infinite;
}
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(-6px); }
}
.modal-title {
  font-size: 17px; font-weight: 800;
  color: var(--amethyst-dk); margin-bottom: 10px;
}
.modal-subtitle {
  font-size: 11px; font-weight: 700;
  color: var(--danger); text-transform: uppercase;
  letter-spacing: .8px; margin-bottom: 8px;
  display: flex; align-items: center; justify-content: center; gap: 5px;
}
.modal-body {
  font-size: 14px; color: var(--text-md);
  line-height: 1.7; margin-bottom: 24px;
  font-family: 'Lora', serif;
}
.modal-body strong { color: var(--amethyst-dk); font-family: 'Plus Jakarta Sans', sans-serif; }
.modal-actions { display: flex; flex-direction: column; gap: 10px; }
.btn-modal-primary {
  padding: 14px;
  background: linear-gradient(135deg, var(--amethyst-dk), var(--amethyst));
  color: #fff; border: none;
  border-radius: var(--radius-md);
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-size: 14px; font-weight: 700;
  cursor: pointer;
  box-shadow: 0 4px 16px rgba(92,61,143,.4);
  transition: transform var(--transition), box-shadow var(--transition);
  display: flex; align-items: center; justify-content: center; gap: 8px;
}
.btn-modal-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 24px rgba(92,61,143,.5); }
.btn-modal-secondary {
  padding: 13px;
  background: var(--lavender-lt);
  color: var(--amethyst);
  border: 1.5px solid var(--lavender);
  border-radius: var(--radius-md);
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-size: 13px; font-weight: 600;
  cursor: pointer;
  transition: all var(--transition);
}
.btn-modal-secondary:hover { background: var(--lavender); }
.modal-close-hint { font-size: 11px; color: var(--text-lt); margin-top: 12px; }

/* ─── TOAST ────────────────────────────────────────── */
#toast {
  position: fixed; bottom: 28px; left: 50%;
  transform: translateX(-50%) translateY(80px);
  background: var(--amethyst-dk);
  color: #fff;
  padding: 12px 24px;
  border-radius: 99px;
  font-size: 13px; font-weight: 600;
  z-index: 2000;
  transition: transform .4s cubic-bezier(.34,1.56,.64,1);
  display: flex; align-items: center; gap: 8px;
  box-shadow: var(--shadow-lg);
  white-space: nowrap;
}
#toast.show { transform: translateX(-50%) translateY(0); }

/* ─── RESPONSIVE ───────────────────────────────────── */
@media (max-width: 768px) {
  :root { --sidebar-w: 0px; }
  #sidebar { transform: translateX(-240px); width: 240px; }
  #sidebar.open { transform: translateX(0); }
  #main { margin-left: 0; }
  .hamburger-btn { display: flex; }
  .page { padding: 20px 16px 48px; }
  #topbar { padding: 12px 16px; }
  .wellbeing-hero { padding: 20px; }
  #modal-box { padding: 24px 20px; }
}
@media (max-width: 500px) {
  .hero-metrics { gap: 14px; }
  .hero-metric-val { font-size: 20px; }
}
</style>
</head>
<body>

<!-- ═══ SIDEBAR ═══════════════════════════════════════ -->
<nav id="sidebar">
  <div class="sidebar-brand">
    <div class="brand-icon">🕯️</div>
    <div class="brand-name">PELITA</div>
    <div class="brand-tagline">Platform Evaluasi Lintas-Kondisi Mental Mahasiswa</div>
  </div>
  <div class="sidebar-nav">
    <div class="nav-section-label">Menu Utama</div>
    <div class="nav-item active" onclick="showPage('dashboard')" id="nav-dashboard">
      <span class="nav-icon">🏠</span> Beranda
    </div>
    <div class="nav-item" onclick="showPage('checkin')" id="nav-checkin">
      <span class="nav-icon">📝</span> Cek-in Jurnal
    </div>
    <div class="nav-item" onclick="showPage('history')" id="nav-history">
      <span class="nav-icon">📚</span> Riwayat Jurnal
    </div>
    <div class="nav-item" onclick="showPage('analytics')" id="nav-analytics">
      <span class="nav-icon">📊</span> Analitik Tren
    </div>
    <div class="nav-section-label" style="margin-top:8px;">Dukungan</div>
    <div class="nav-item" onclick="showPage('konseling')" id="nav-konseling">
      <span class="nav-icon">🤝</span> Jadwal Konseling
    </div>
    <div class="nav-item" onclick="showPage('resources')" id="nav-resources">
      <span class="nav-icon">📖</span> Sumber Daya
    </div>
  </div>
  <div class="sidebar-footer">
    <div class="badge-gratis">✅ Fasilitas Kampus — Gratis</div>
    <div style="font-size:11px; color:rgba(196,176,232,.45); margin-top:8px;">Fakultas Psikologi © 2025</div>
  </div>
</nav>

<!-- ═══ MAIN ═══════════════════════════════════════════ -->
<div id="main">
  <!-- TOP BAR -->
  <div id="topbar">
    <div style="display:flex; align-items:center; gap:0;">
      <button class="hamburger-btn" onclick="toggleSidebar()">☰</button>
      <div class="topbar-date" id="topbar-date"></div>
    </div>
    <div style="display:flex; align-items:center; gap:12px;">
      <div style="font-size:12px; color:var(--text-lt);">Mahasiswa</div>
      <div class="avatar-btn">M</div>
    </div>
  </div>

  <!-- ═══ PAGE: DASHBOARD ═══ -->
  <div class="page active" id="page-dashboard">
    <!-- WELLBEING HERO -->
    <div class="wellbeing-hero">
      <div class="hero-top">
        <div>
          <div class="hero-greeting">Selamat datang kembali 👋</div>
          <div class="hero-name">Mahasiswa PELITA</div>
          <div style="font-size:12px; color:var(--lavender); margin-top:6px;">Fasilitas Dukungan Psikologis — Fakultas Psikologi</div>
        </div>
        <div class="hero-status-badge" id="hero-status-badge">
          <span class="status-dot" id="hero-dot"></span>
          <span id="hero-status-text">Memuat status…</span>
        </div>
      </div>
      <div class="hero-metrics" id="hero-metrics">
        <div class="hero-metric">
          <div class="hero-metric-val" id="m-total">0</div>
          <div class="hero-metric-lbl">Jurnal Tersimpan</div>
        </div>
        <div class="hero-divider"></div>
        <div class="hero-metric">
          <div class="hero-metric-val" id="m-streak">0</div>
          <div class="hero-metric-lbl">Hari Berturut</div>
        </div>
        <div class="hero-divider"></div>
        <div class="hero-metric">
          <div class="hero-metric-val" id="m-avg-stress">—</div>
          <div class="hero-metric-lbl">Rata-rata Kortisol</div>
        </div>
        <div class="hero-divider"></div>
        <div class="hero-metric">
          <div class="hero-metric-val" id="m-wellbeing">—</div>
          <div class="hero-metric-lbl">Wellbeing Score</div>
        </div>
      </div>
    </div>

    <!-- STAT CARDS -->
    <div class="cards-grid">
      <div class="stat-card">
        <div class="stat-card-icon">🧠</div>
        <div class="stat-card-val" id="s-dopamine">—</div>
        <div class="stat-card-lbl">Dopamin (rata-rata 7 hari)</div>
        <div class="stat-card-sub" id="s-dopamine-sub">Belum ada data</div>
      </div>
      <div class="stat-card">
        <div class="stat-card-icon">☀️</div>
        <div class="stat-card-val" id="s-serotonin">—</div>
        <div class="stat-card-lbl">Serotonin (rata-rata 7 hari)</div>
        <div class="stat-card-sub" id="s-serotonin-sub">Belum ada data</div>
      </div>
      <div class="stat-card">
        <div class="stat-card-icon">🫂</div>
        <div class="stat-card-val" id="s-oksitosin">—</div>
        <div class="stat-card-lbl">Oksitosin (rata-rata 7 hari)</div>
        <div class="stat-card-sub" id="s-oksitosin-sub">Belum ada data</div>
      </div>
      <div class="stat-card">
        <div class="stat-card-icon">⚡</div>
        <div class="stat-card-val" id="s-kortisol">—</div>
        <div class="stat-card-lbl">Kortisol (rata-rata 7 hari)</div>
        <div class="stat-card-sub" id="s-kortisol-sub">Belum ada data</div>
      </div>
    </div>

    <!-- MINI CHART + RECENT ENTRY -->
    <div class="two-col">
      <div class="card">
        <div class="card-header">
          <span class="card-header-icon">📈</span>
          <div>
            <div class="card-title">Tren Kortisol 7 Hari</div>
            <div class="card-subtitle">Indikator stres utama</div>
          </div>
        </div>
        <div class="card-body">
          <div class="chart-container">
            <canvas id="chart-mini"></canvas>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header">
          <span class="card-header-icon">🗒️</span>
          <div>
            <div class="card-title">Entri Terbaru</div>
            <div class="card-subtitle">Jurnal terakhir yang dicatat</div>
          </div>
        </div>
        <div class="card-body" id="recent-entry-body">
          <div class="empty-state">
            <div class="empty-state-icon">✍️</div>
            <div class="empty-state-text">Belum ada jurnal.<br>Mulai check-in pertamamu hari ini!</div>
          </div>
        </div>
      </div>
    </div>

    <div style="text-align:center; margin-top:8px;">
      <button class="btn-save" style="max-width:360px; margin:0 auto;" onclick="showPage('checkin')">
        ✏️ Mulai Cek-in Hari Ini
      </button>
    </div>
  </div>

  <!-- ═══ PAGE: CHECK-IN ═══ -->
  <div class="page" id="page-checkin">
    <div class="page-header">
      <div class="page-title">📝 Cek-in Jurnal Harian</div>
      <div class="page-subtitle">Rekam kondisi mentalmu hari ini — jujur & bebas dihakimi</div>
    </div>

    <div class="two-col">
      <!-- LEFT: MOOD & SLIDERS -->
      <div style="display:flex; flex-direction:column; gap:20px;">
        <!-- MOOD PICKER -->
        <div class="card">
          <div class="card-header">
            <span class="card-header-icon">😊</span>
            <div>
              <div class="card-title">Suasana Hati Saat Ini</div>
              <div class="card-subtitle">Pilih yang paling menggambarkan perasaanmu</div>
            </div>
          </div>
          <div class="card-body">
            <div class="mood-row" id="mood-row">
              <div class="mood-option" onclick="selectMood(this, 'Sangat Baik')" data-mood="Sangat Baik">
                <div class="mood-emoji">😄</div>
                <div class="mood-lbl">Sangat Baik</div>
              </div>
              <div class="mood-option" onclick="selectMood(this, 'Baik')" data-mood="Baik">
                <div class="mood-emoji">🙂</div>
                <div class="mood-lbl">Baik</div>
              </div>
              <div class="mood-option" onclick="selectMood(this, 'Netral')" data-mood="Netral">
                <div class="mood-emoji">😐</div>
                <div class="mood-lbl">Netral</div>
              </div>
              <div class="mood-option" onclick="selectMood(this, 'Kurang')" data-mood="Kurang">
                <div class="mood-emoji">😔</div>
                <div class="mood-lbl">Kurang</div>
              </div>
              <div class="mood-option" onclick="selectMood(this, 'Buruk')" data-mood="Buruk">
                <div class="mood-emoji">😞</div>
                <div class="mood-lbl">Buruk</div>
              </div>
            </div>
          </div>
        </div>

        <!-- HORMONE SLIDERS -->
        <div class="card">
          <div class="card-header">
            <span class="card-header-icon">🧬</span>
            <div>
              <div class="card-title">Kondisi Hormon (Estimasi)</div>
              <div class="card-subtitle">Geser sesuai perasaan relatifmu hari ini</div>
            </div>
          </div>
          <div class="card-body">
            <div class="hormone-grid">

              <!-- Dopamin -->
              <div class="hormone-item">
                <div class="hormone-header">
                  <div class="hormone-label">
                    <span class="hormone-emoji">🧠</span>
                    <div>Dopamin
                      <span class="hormone-desc">Motivasi & rasa pencapaian</span>
                    </div>
                  </div>
                  <div class="hormone-value-badge" id="badge-dopamine" style="background:#EDE8F9; color:#5C3D8F;">50</div>
                </div>
                <input type="range" class="hormone-slider" id="slider-dopamine" min="0" max="100" value="50"
                       oninput="updateSlider('dopamine', this.value)" />
                <div class="slider-ticks"><span class="slider-tick">Rendah</span><span class="slider-tick">Sedang</span><span class="slider-tick">Tinggi</span></div>
              </div>

              <!-- Serotonin -->
              <div class="hormone-item">
                <div class="hormone-header">
                  <div class="hormone-label">
                    <span class="hormone-emoji">☀️</span>
                    <div>Serotonin
                      <span class="hormone-desc">Kebahagiaan & ketenangan</span>
                    </div>
                  </div>
                  <div class="hormone-value-badge" id="badge-serotonin" style="background:#EDE8F9; color:#5C3D8F;">50</div>
                </div>
                <input type="range" class="hormone-slider" id="slider-serotonin" min="0" max="100" value="50"
                       oninput="updateSlider('serotonin', this.value)" />
                <div class="slider-ticks"><span class="slider-tick">Rendah</span><span class="slider-tick">Sedang</span><span class="slider-tick">Tinggi</span></div>
              </div>

              <!-- Oksitosin -->
              <div class="hormone-item">
                <div class="hormone-header">
                  <div class="hormone-label">
                    <span class="hormone-emoji">🫂</span>
                    <div>Oksitosin
                      <span class="hormone-desc">Koneksi sosial & rasa aman</span>
                    </div>
                  </div>
                  <div class="hormone-value-badge" id="badge-oksitosin" style="background:#EDE8F9; color:#5C3D8F;">50</div>
                </div>
                <input type="range" class="hormone-slider" id="slider-oksitosin" min="0" max="100" value="50"
                       oninput="updateSlider('oksitosin', this.value)" />
                <div class="slider-ticks"><span class="slider-tick">Rendah</span><span class="slider-tick">Sedang</span><span class="slider-tick">Tinggi</span></div>
              </div>

              <!-- Kortisol -->
              <div class="hormone-item">
                <div class="hormone-header">
                  <div class="hormone-label">
                    <span class="hormone-emoji">⚡</span>
                    <div>Kortisol
                      <span class="hormone-desc">Stres & beban pikiran</span>
                    </div>
                  </div>
                  <div class="hormone-value-badge" id="badge-kortisol" style="background:#EDE8F9; color:#5C3D8F;">50</div>
                </div>
                <input type="range" class="hormone-slider" id="slider-kortisol" min="0" max="100" value="50"
                       oninput="updateSlider('kortisol', this.value)" />
                <div class="slider-ticks"><span class="slider-tick">Rendah</span><span class="slider-tick">Sedang</span><span class="slider-tick">Tinggi</span></div>
              </div>

            </div>
          </div>
        </div>
      </div>

      <!-- RIGHT: DIARY + TAGS + SAVE -->
      <div style="display:flex; flex-direction:column; gap:20px;">
        <div class="card">
          <div class="card-header">
            <span class="card-header-icon">✍️</span>
            <div>
              <div class="card-title">Catatan Diary</div>
              <div class="card-subtitle">Tuliskan apa saja yang sedang kamu rasakan atau alami</div>
            </div>
          </div>
          <div class="card-body">
            <div class="diary-label">📖 Apa yang ada di pikiranmu hari ini?</div>
            <textarea class="diary-input" id="diary-text"
              placeholder="Mulai menulis di sini… Tidak ada yang dihakimi. Ini ruangmu sendiri.&#10;&#10;Contoh: Hari ini bimbingan cukup melelahkan. Dosen meminta revisi bab 3 lagi…"></textarea>

            <div class="tags-label">🏷️ Aktivitas Hari Ini <span style="font-size:11px; color:var(--text-lt); font-weight:400;">(pilih semua yang relevan)</span></div>
            <div class="tags-grid" id="tags-grid">
              <div class="tag-chip" onclick="toggleTag(this)">📋 Bimbingan Skripsi</div>
              <div class="tag-chip" onclick="toggleTag(this)">✏️ Revisi Proposal</div>
              <div class="tag-chip" onclick="toggleTag(this)">📊 Analisis Data (SPSS)</div>
              <div class="tag-chip" onclick="toggleTag(this)">📈 SEM / CFA</div>
              <div class="tag-chip" onclick="toggleTag(this)">📝 Ujian Tengah Semester</div>
              <div class="tag-chip" onclick="toggleTag(this)">📅 Ujian Akhir Semester</div>
              <div class="tag-chip" onclick="toggleTag(this)">👥 Tugas Kelompok</div>
              <div class="tag-chip" onclick="toggleTag(this)">📚 Seminar / Kuliah Tamu</div>
              <div class="tag-chip" onclick="toggleTag(this)">💼 Praktik Lapangan</div>
              <div class="tag-chip" onclick="toggleTag(this)">⏰ Deadline Tugas</div>
              <div class="tag-chip" onclick="toggleTag(this)">🎓 Sidang Skripsi</div>
              <div class="tag-chip" onclick="toggleTag(this)">😴 Kurang Istirahat</div>
              <div class="tag-chip" onclick="toggleTag(this)">🏠 Masalah Akomodasi</div>
              <div class="tag-chip" onclick="toggleTag(this)">💰 Masalah Finansial</div>
              <div class="tag-chip" onclick="toggleTag(this)">🤝 Masalah Sosial</div>
              <div class="tag-chip" onclick="toggleTag(this)">✅ Pencapaian Hari Ini</div>
            </div>
          </div>
        </div>

        <!-- LIVE WELLBEING INDICATOR -->
        <div class="card" id="live-indicator-card">
          <div class="card-body" style="padding:16px 20px;">
            <div style="display:flex; align-items:center; justify-content:space-between; margin-bottom:10px;">
              <div style="font-size:13px; font-weight:700; color:var(--text-dk);">Indikator Wellbeing Saat Ini</div>
              <div style="font-size:22px; font-weight:800;" id="live-score-val">—</div>
            </div>
            <div style="height:8px; border-radius:99px; background:var(--lavender-lt); overflow:hidden;">
              <div id="live-score-bar" style="height:100%; width:50%; border-radius:99px; background:var(--amethyst); transition:width .5s ease, background .5s ease;"></div>
            </div>
            <div style="font-size:11px; color:var(--text-lt); margin-top:6px;" id="live-score-label">Geser slider untuk melihat estimasi</div>
          </div>
        </div>

        <button class="btn-save" onclick="saveJournal()">
          💾 Simpan Jurnal Hari Ini
        </button>
      </div>
    </div>
  </div>

  <!-- ═══ PAGE: HISTORY ═══ -->
  <div class="page" id="page-history">
    <div class="page-header">
      <div class="page-title">📚 Riwayat Jurnal</div>
      <div class="page-subtitle">Rekam jejak perjalanan mentalmu</div>
    </div>
    <div class="card">
      <div class="card-header">
        <span class="card-header-icon">🗓️</span>
        <div>
          <div class="card-title">Semua Entri</div>
          <div class="card-subtitle" id="history-count-lbl">Memuat…</div>
        </div>
      </div>
      <div class="card-body">
        <div id="history-list"></div>
      </div>
    </div>
  </div>

  <!-- ═══ PAGE: ANALYTICS ═══ -->
  <div class="page" id="page-analytics">
    <div class="page-header">
      <div class="page-title">📊 Analitik Tren Kondisi Mental</div>
      <div class="page-subtitle">Visualisasi pola hormon 7 hari terakhir untuk tinjauan konselor & dosen wali</div>
    </div>

    <div class="analytics-grid">
      <div class="analytics-stat">
        <div class="analytics-stat-val" id="an-total">0</div>
        <div class="analytics-stat-lbl">Total entri tersimpan</div>
      </div>
      <div class="analytics-stat">
        <div class="analytics-stat-val" id="an-high-stress">0</div>
        <div class="analytics-stat-lbl">Hari kortisol tinggi (>70)</div>
      </div>
      <div class="analytics-stat">
        <div class="analytics-stat-val" id="an-avg-wb">—</div>
        <div class="analytics-stat-lbl">Rata-rata wellbeing 7 hari</div>
      </div>
      <div class="analytics-stat">
        <div class="analytics-stat-val" id="an-trend">—</div>
        <div class="analytics-stat-lbl">Tren kortisol</div>
      </div>
    </div>

    <div class="card" style="margin-bottom:20px;">
      <div class="card-header">
        <span class="card-header-icon">📉</span>
        <div>
          <div class="card-title">Semua Hormon — 7 Hari Terakhir</div>
          <div class="card-subtitle">Dopamin · Serotonin · Oksitosin · Kortisol</div>
        </div>
      </div>
      <div class="card-body">
        <div class="chart-container" style="height:300px;">
          <canvas id="chart-all-hormones"></canvas>
        </div>
      </div>
    </div>

    <div class="two-col">
      <div class="card">
        <div class="card-header">
          <span class="card-header-icon">⚡</span>
          <div>
            <div class="card-title">Kortisol — Tren Stres</div>
            <div class="card-subtitle">Semakin tinggi = semakin tinggi beban stres</div>
          </div>
        </div>
        <div class="card-body">
          <div class="chart-container">
            <canvas id="chart-kortisol"></canvas>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-header">
          <span class="card-header-icon">🎯</span>
          <div>
            <div class="card-title">Wellbeing Score</div>
            <div class="card-subtitle">Skor gabungan kondisi mental harian</div>
          </div>
        </div>
        <div class="card-body">
          <div class="chart-container">
            <canvas id="chart-wellbeing"></canvas>
          </div>
        </div>
      </div>
    </div>

    <div style="margin-top:20px; padding:16px 20px; background:var(--lavender-lt); border-radius:var(--radius-md); border:1px dashed var(--lavender);">
      <div style="font-size:13px; font-weight:700; color:var(--amethyst-dk); margin-bottom:4px;">ℹ️ Untuk Dosen Wali & Konselor</div>
      <div style="font-size:12px; color:var(--text-md); line-height:1.6;">Data analitik ini dapat digunakan untuk mengidentifikasi pola stres kronik dan menentukan waktu intervensi yang tepat. Kortisol tinggi berkelanjutan (>70 selama >3 hari) menjadi sinyal untuk sesi konseling proaktif.</div>
    </div>
  </div>

  <!-- ═══ PAGE: KONSELING ═══ -->
  <div class="page" id="page-konseling">
    <div class="page-header">
      <div class="page-title">🤝 Jadwal Konseling Fakultas</div>
      <div class="page-subtitle">Konselor kami siap mendampingi — tanpa syarat, tanpa biaya</div>
    </div>

    <div style="padding:16px 20px; background:linear-gradient(135deg, #EDE8F9, #E8F4FF); border-radius:var(--radius-md); border:1px solid var(--lavender); margin-bottom:20px; display:flex; align-items:flex-start; gap:12px;">
      <div style="font-size:24px;">💜</div>
      <div>
        <div style="font-size:14px; font-weight:700; color:var(--amethyst-dk);">Semua layanan konseling gratis untuk mahasiswa aktif</div>
        <div style="font-size:12px; color:var(--text-md); margin-top:3px; line-height:1.6;">Tidak perlu khawatir soal biaya atau stigma. Ini adalah hakmu sebagai mahasiswa fakultas.</div>
      </div>
    </div>

    <div id="konselor-list"></div>
  </div>

  <!-- ═══ PAGE: RESOURCES ═══ -->
  <div class="page" id="page-resources">
    <div class="page-header">
      <div class="page-title">📖 Sumber Daya Kesehatan Mental</div>
      <div class="page-subtitle">Bacaan, teknik, dan materi pendukung untuk kesejahteraan akademikmu</div>
    </div>
    <div id="resources-list"></div>
  </div>

</div><!-- /#main -->

<!-- ═══ MODAL: EARLY WARNING ═══════════════════════ -->
<div id="modal-overlay" role="dialog" aria-modal="true" aria-labelledby="modal-title">
  <div id="modal-box">
    <div class="modal-wave">🫂</div>
    <div class="modal-subtitle">⚠️ Deteksi Kelelahan Akademik</div>
    <div class="modal-title" id="modal-title">Hei, kami melihat ada yang berat hari ini.</div>
    <div class="modal-body">
      Dari catatanmu hari ini sepertinya beban tugas atau skripsi sedang terasa <strong>sangat membebani</strong>. Ingat, kamu tidak harus memikulnya sendirian. <strong>Fakultas hadir untuk mendukungmu</strong> — selalu dan tanpa syarat.
    </div>
    <div class="modal-actions">
      <button class="btn-modal-primary" onclick="goToKonseling()">
        🗓️ Jadwalkan Konseling Fakultas
      </button>
      <button class="btn-modal-secondary" onclick="dismissModalRest()">
        😴 Saya coba istirahat dulu
      </button>
    </div>
    <div class="modal-close-hint">Entri jurnal tetap tersimpan setelah menutup pop-up ini.</div>
  </div>
</div>

<!-- ═══ TOAST ═══════════════════════════════════════ -->
<div id="toast">✅ Jurnal berhasil disimpan!</div>

<!-- ═══ SIDEBAR OVERLAY (mobile) ══════════════════ -->
<div id="sidebar-overlay" onclick="toggleSidebar()"
     style="display:none; position:fixed; inset:0; background:rgba(0,0,0,.4); z-index:99;"></div>

<script>
/* ══════════════════════════════════════════════════
   DATA LAYER
═══════════════════════════════════════════════════*/
const STORAGE_KEY = 'pelita_journals';

function loadJournals() {
  try { return JSON.parse(localStorage.getItem(STORAGE_KEY)) || []; }
  catch { return []; }
}
function saveJournals(data) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
}

/* ══════════════════════════════════════════════════
   NAV
═══════════════════════════════════════════════════*/
let currentPage = 'dashboard';

function showPage(id) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('page-' + id).classList.add('active');
  document.getElementById('nav-' + id).classList.add('active');
  currentPage = id;
  if (id === 'dashboard')  renderDashboard();
  if (id === 'history')    renderHistory();
  if (id === 'analytics')  renderAnalytics();
  if (id === 'konseling')  renderKonseling();
  if (id === 'resources')  renderResources();
  // close sidebar on mobile
  const sb = document.getElementById('sidebar');
  if (sb.classList.contains('open')) toggleSidebar();
}

function toggleSidebar() {
  const sb = document.getElementById('sidebar');
  const ov = document.getElementById('sidebar-overlay');
  const open = sb.classList.toggle('open');
  ov.style.display = open ? 'block' : 'none';
}

/* ══════════════════════════════════════════════════
   TOPBAR DATE
═══════════════════════════════════════════════════*/
(function() {
  const now = new Date();
  const days = ['Minggu','Senin','Selasa','Rabu','Kamis','Jumat','Sabtu'];
  const months = ['Jan','Feb','Mar','Apr','Mei','Jun','Jul','Agu','Sep','Okt','Nov','Des'];
  const str = `${days[now.getDay()]}, ${now.getDate()} ${months[now.getMonth()]} ${now.getFullYear()}`;
  document.getElementById('topbar-date').innerHTML = `<span>${str}</span>`;
})();

/* ══════════════════════════════════════════════════
   HORMONE SLIDERS
═══════════════════════════════════════════════════*/
const sliderState = { dopamine: 50, serotonin: 50, oksitosin: 50, kortisol: 50 };

function getSliderColor(hormone, value) {
  if (hormone === 'kortisol') {
    // Low=green, High=red
    if (value < 30) return { bg: '#E8F8F4', color: '#2B8A6A' };
    if (value < 60) return { bg: '#FFF5E6', color: '#D4890A' };
    return { bg: '#FFE8EC', color: '#C0293E' };
  }
  // Others: Low=red, High=green
  if (value < 30) return { bg: '#FFE8EC', color: '#C0293E' };
  if (value < 60) return { bg: '#FFF5E6', color: '#D4890A' };
  return { bg: '#E8F8F4', color: '#2B8A6A' };
}

function getSliderTrackGradient(hormone, value) {
  if (hormone === 'kortisol') {
    return `linear-gradient(to right, #4DA89A 0%, #F2A65A ${value * 0.5}%, #E05C6E ${value}%, #ddd5f3 ${value}%, #ddd5f3 100%)`;
  }
  return `linear-gradient(to right, #5C3D8F 0%, #7B5CAD ${value}%, #ddd5f3 ${value}%, #ddd5f3 100%)`;
}

function updateSlider(hormone, value) {
  const v = parseInt(value);
  sliderState[hormone] = v;
  const badge = document.getElementById('badge-' + hormone);
  const slider = document.getElementById('slider-' + hormone);
  const { bg, color } = getSliderColor(hormone, v);
  badge.textContent = v;
  badge.style.background = bg;
  badge.style.color = color;
  slider.style.background = getSliderTrackGradient(hormone, v);
  slider.style.color = color;
  updateLiveScore();
}

function initSliders() {
  ['dopamine','serotonin','oksitosin','kortisol'].forEach(h => {
    updateSlider(h, sliderState[h]);
  });
}

/* ══════════════════════════════════════════════════
   LIVE WELLBEING SCORE
═══════════════════════════════════════════════════*/
function calcWellbeing(d, s, o, k) {
  return Math.round(((d + s + o + (100 - k)) / 4));
}

function updateLiveScore() {
  const { dopamine: d, serotonin: s, oksitosin: o, kortisol: k } = sliderState;
  const score = calcWellbeing(d, s, o, k);
  const el = document.getElementById('live-score-val');
  const bar = document.getElementById('live-score-bar');
  const lbl = document.getElementById('live-score-label');
  if (!el) return;
  el.textContent = score;
  bar.style.width = score + '%';
  if (score >= 70) {
    bar.style.background = 'linear-gradient(90deg, #4DA89A, #5AB08A)';
    lbl.textContent = '✅ Kondisimu terlihat baik hari ini';
    el.style.color = '#2B8A6A';
  } else if (score >= 45) {
    bar.style.background = 'linear-gradient(90deg, #F2A65A, #E8B84B)';
    lbl.textContent = '⚠️ Perlu perhatian — coba istirahat sejenak';
    el.style.color = '#D4890A';
  } else {
    bar.style.background = 'linear-gradient(90deg, #E05C6E, #C0293E)';
    lbl.textContent = '🚨 Beban cukup tinggi — pertimbangkan minta bantuan';
    el.style.color = '#C0293E';
  }
}

/* ══════════════════════════════════════════════════
   MOOD SELECTOR
═══════════════════════════════════════════════════*/
let selectedMood = '';
function selectMood(el, mood) {
  document.querySelectorAll('.mood-option').forEach(m => m.classList.remove('selected'));
  el.classList.add('selected');
  selectedMood = mood;
}

/* ══════════════════════════════════════════════════
   TAGS
═══════════════════════════════════════════════════*/
function toggleTag(el) {
  el.classList.toggle('selected');
}

function getSelectedTags() {
  return Array.from(document.querySelectorAll('#tags-grid .tag-chip.selected'))
    .map(t => t.textContent.trim());
}

/* ══════════════════════════════════════════════════
   SAVE JOURNAL + EARLY WARNING SYSTEM
═══════════════════════════════════════════════════*/
let pendingEntry = null;

function saveJournal() {
  const { dopamine: d, serotonin: s, oksitosin: o, kortisol: k } = sliderState;
  const text = document.getElementById('diary-text').value.trim();
  const tags = getSelectedTags();
  const now = new Date();

  const entry = {
    id: Date.now(),
    date: now.toISOString(),
    mood: selectedMood || 'Netral',
    dopamine: d, serotonin: s, oksitosin: o, kortisol: k,
    wellbeing: calcWellbeing(d, s, o, k),
    diary: text,
    tags: tags
  };

  // ── EARLY WARNING SYSTEM ────────────────────────
  const highStress   = k > 80;
  const burnoutCombo = k > 70 && d < 30;

  if (highStress || burnoutCombo) {
    pendingEntry = entry;
    showModal();
    return; // Don't save yet
  }

  commitSave(entry);
}

function commitSave(entry) {
  const journals = loadJournals();
  journals.unshift(entry);
  saveJournals(journals);
  resetCheckinForm();
  showToast('✅ Jurnal berhasil disimpan!');
  renderDashboard();
}

function resetCheckinForm() {
  sliderState.dopamine = 50;
  sliderState.serotonin = 50;
  sliderState.oksitosin = 50;
  sliderState.kortisol = 50;
  initSliders();
  ['dopamine','serotonin','oksitosin','kortisol'].forEach(h => {
    document.getElementById('slider-' + h).value = 50;
  });
  document.getElementById('diary-text').value = '';
  document.querySelectorAll('.tag-chip').forEach(t => t.classList.remove('selected'));
  document.querySelectorAll('.mood-option').forEach(m => m.classList.remove('selected'));
  selectedMood = '';
  updateLiveScore();
}

/* ══════════════════════════════════════════════════
   MODAL
═══════════════════════════════════════════════════*/
function showModal() {
  const overlay = document.getElementById('modal-overlay');
  overlay.classList.add('visible');
  // trap focus
  document.getElementById('modal-box').querySelector('.btn-modal-primary').focus();
}

function hideModal() {
  document.getElementById('modal-overlay').classList.remove('visible');
}

function goToKonseling() {
  hideModal();
  if (pendingEntry) {
    commitSave(pendingEntry);
    pendingEntry = null;
  }
  showPage('konseling');
}

function dismissModalRest() {
  hideModal();
  if (pendingEntry) {
    commitSave(pendingEntry);
    pendingEntry = null;
  }
  showToast('💜 Istirahat yang baik ya. Jurnal tersimpan.');
}

// Close on overlay click
document.getElementById('modal-overlay').addEventListener('click', function(e) {
  if (e.target === this) {
    hideModal();
    if (pendingEntry) { commitSave(pendingEntry); pendingEntry = null; }
  }
});

/* ══════════════════════════════════════════════════
   TOAST
═══════════════════════════════════════════════════*/
let toastTimer = null;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  if (toastTimer) clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 3500);
}

/* ══════════════════════════════════════════════════
   DASHBOARD RENDER
═══════════════════════════════════════════════════*/
let miniChartInst = null;

function renderDashboard() {
  const journals = loadJournals();
  const recent7 = journals.slice(0, 7);

  // HERO metrics
  document.getElementById('m-total').textContent = journals.length;

  // Streak
  let streak = 0;
  const today = new Date(); today.setHours(0,0,0,0);
  for (let i = 0; i < journals.length; i++) {
    const d = new Date(journals[i].date); d.setHours(0,0,0,0);
    const diff = Math.round((today - d) / 86400000);
    if (diff === i) streak++;
    else break;
  }
  document.getElementById('m-streak').textContent = streak;

  // Avg kortisol
  const avgK = recent7.length ? Math.round(recent7.reduce((a,b) => a + b.kortisol, 0) / recent7.length) : null;
  document.getElementById('m-avg-stress').textContent = avgK !== null ? avgK : '—';

  // Avg wellbeing
  const avgWb = recent7.length ? Math.round(recent7.reduce((a,b) => a + b.wellbeing, 0) / recent7.length) : null;
  document.getElementById('m-wellbeing').textContent = avgWb !== null ? avgWb : '—';

  // Status badge
  const dot = document.getElementById('hero-dot');
  const txt = document.getElementById('hero-status-text');
  if (avgWb === null) {
    txt.textContent = 'Belum ada data';
    dot.style.background = '#aaa';
  } else if (avgWb >= 70) {
    txt.textContent = 'Kondisi Baik';
    dot.style.background = '#5AB08A';
  } else if (avgWb >= 45) {
    txt.textContent = 'Perlu Perhatian';
    dot.style.background = '#F2A65A';
    dot.style.boxShadow = '0 0 0 2px rgba(242,166,90,.3)';
  } else {
    txt.textContent = 'Butuh Dukungan';
    dot.style.background = '#E05C6E';
    dot.style.boxShadow = '0 0 0 2px rgba(224,92,110,.3)';
  }

  // Stat cards
  ['dopamine','serotonin','oksitosin','kortisol'].forEach(h => {
    const avg = recent7.length ? Math.round(recent7.reduce((a,b) => a + b[h], 0) / recent7.length) : null;
    const el = document.getElementById('s-' + h);
    const sub = document.getElementById('s-' + h + '-sub');
    if (avg !== null) {
      el.textContent = avg;
      sub.textContent = `Berdasarkan ${recent7.length} entri terakhir`;
    } else {
      el.textContent = '—';
      sub.textContent = 'Belum ada data';
    }
  });

  // Recent entry
  const body = document.getElementById('recent-entry-body');
  if (journals.length === 0) {
    body.innerHTML = `<div class="empty-state"><div class="empty-state-icon">✍️</div><div class="empty-state-text">Belum ada jurnal.<br>Mulai check-in pertamamu!</div></div>`;
  } else {
    const e = journals[0];
    const d = new Date(e.date);
    body.innerHTML = `
      <div style="font-size:11px;color:var(--text-lt);font-weight:600;margin-bottom:6px;">${formatDate(d)}</div>
      <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px;">
        <span style="font-size:20px;">${moodEmoji(e.mood)}</span>
        <span style="font-size:14px;font-weight:700;color:var(--amethyst-dk);">${e.mood}</span>
        <span style="font-size:12px;color:var(--text-lt);">· Wellbeing: <b>${e.wellbeing}</b></span>
      </div>
      ${e.diary ? `<p style="font-family:'Lora',serif;font-size:13px;color:var(--text-md);line-height:1.6;">${truncate(e.diary, 140)}</p>` : '<p style="font-size:12px;color:var(--text-lt);font-style:italic;">Tidak ada catatan diary.</p>'}
      ${e.tags.length ? `<div class="entry-tags">${e.tags.map(t => `<span class="entry-tag">${t}</span>`).join('')}</div>` : ''}
      <div class="entry-hormones">
        <span class="entry-hormone">🧠 <span>${e.dopamine}</span></span>
        <span class="entry-hormone">☀️ <span>${e.serotonin}</span></span>
        <span class="entry-hormone">🫂 <span>${e.oksitosin}</span></span>
        <span class="entry-hormone">⚡ <span>${e.kortisol}</span></span>
      </div>`;
  }

  // Mini chart
  const labels7 = getLast7Labels(recent7);
  const kortData = getLast7Data(recent7, 'kortisol');
  if (miniChartInst) miniChartInst.destroy();
  const ctx = document.getElementById('chart-mini');
  miniChartInst = new Chart(ctx, {
    type: 'line',
    data: {
      labels: labels7,
      datasets: [{
        label: 'Kortisol',
        data: kortData,
        fill: true,
        backgroundColor: 'rgba(224,92,110,.12)',
        borderColor: '#E05C6E',
        borderWidth: 2.5,
        pointBackgroundColor: '#E05C6E',
        pointRadius: 4,
        tension: 0.4
      }]
    },
    options: chartOptions('Kortisol', [0, 100])
  });
}

/* ══════════════════════════════════════════════════
   HISTORY RENDER
═══════════════════════════════════════════════════*/
function renderHistory() {
  const journals = loadJournals();
  const lbl = document.getElementById('history-count-lbl');
  const list = document.getElementById('history-list');
  lbl.textContent = `${journals.length} entri tersimpan`;
  if (!journals.length) {
    list.innerHTML = `<div class="empty-state"><div class="empty-state-icon">📭</div><div class="empty-state-text">Belum ada entri jurnal. Mulai check-in pertamamu!</div></div>`;
    return;
  }
  list.innerHTML = '<div class="entry-list">' + journals.map(e => {
    const d = new Date(e.date);
    return `
      <div class="entry-item">
        <div class="entry-date">${formatDate(d)} &nbsp;·&nbsp; ${moodEmoji(e.mood)} ${e.mood} &nbsp;·&nbsp; Wellbeing: <b>${e.wellbeing}</b></div>
        ${e.diary ? `<div class="entry-preview">${truncate(e.diary, 180)}</div>` : '<div class="entry-preview" style="font-style:italic;color:var(--text-lt);">Tanpa catatan diary.</div>'}
        ${e.tags.length ? `<div class="entry-tags">${e.tags.map(t => `<span class="entry-tag">${t}</span>`).join('')}</div>` : ''}
        <div class="entry-hormones">
          <span class="entry-hormone">🧠 Dopamin <span>${e.dopamine}</span></span>
          <span class="entry-hormone">☀️ Serotonin <span>${e.serotonin}</span></span>
          <span class="entry-hormone">🫂 Oksitosin <span>${e.oksitosin}</span></span>
          <span class="entry-hormone">⚡ Kortisol <span>${e.kortisol}</span></span>
        </div>
      </div>`;
  }).join('') + '</div>';
}

/* ══════════════════════════════════════════════════
   ANALYTICS RENDER
═══════════════════════════════════════════════════*/
let chartAllInst = null, chartKortInst = null, chartWbInst = null;

function renderAnalytics() {
  const journals = loadJournals();
  const recent7 = journals.slice(0, 7);

  document.getElementById('an-total').textContent = journals.length;
  const highStressDays = journals.filter(e => e.kortisol > 70).length;
  document.getElementById('an-high-stress').textContent = highStressDays;

  const avgWb = recent7.length ? (recent7.reduce((a,b) => a + b.wellbeing, 0) / recent7.length).toFixed(1) : '—';
  document.getElementById('an-avg-wb').textContent = avgWb;

  if (recent7.length >= 2) {
    const first = recent7[recent7.length - 1].kortisol;
    const last  = recent7[0].kortisol;
    const diff  = last - first;
    document.getElementById('an-trend').textContent = diff > 0 ? `↑ ${diff}` : diff < 0 ? `↓ ${Math.abs(diff)}` : '→ Stabil';
    document.getElementById('an-trend').style.color = diff > 5 ? 'var(--danger)' : diff < -5 ? 'var(--success)' : 'var(--text-dk)';
  } else {
    document.getElementById('an-trend').textContent = '—';
  }

  const labels = getLast7Labels(recent7);
  const dopData = getLast7Data(recent7, 'dopamine');
  const serData = getLast7Data(recent7, 'serotonin');
  const oksiData = getLast7Data(recent7, 'oksitosin');
  const kortData = getLast7Data(recent7, 'kortisol');
  const wbData   = getLast7Data(recent7, 'wellbeing');

  if (chartAllInst) chartAllInst.destroy();
  chartAllInst = new Chart(document.getElementById('chart-all-hormones'), {
    type: 'line',
    data: {
      labels,
      datasets: [
        { label: 'Dopamin',   data: dopData,  borderColor: '#5C3D8F', backgroundColor: 'rgba(92,61,143,.08)',  fill: false, tension: 0.4, borderWidth: 2.5, pointRadius: 4 },
        { label: 'Serotonin', data: serData,  borderColor: '#A8C8E8', backgroundColor: 'rgba(168,200,232,.08)', fill: false, tension: 0.4, borderWidth: 2.5, pointRadius: 4 },
        { label: 'Oksitosin', data: oksiData, borderColor: '#B2DED6', backgroundColor: 'rgba(178,222,214,.08)', fill: false, tension: 0.4, borderWidth: 2.5, pointRadius: 4 },
        { label: 'Kortisol',  data: kortData, borderColor: '#E05C6E', backgroundColor: 'rgba(224,92,110,.08)',  fill: false, tension: 0.4, borderWidth: 2.5, pointRadius: 4 },
      ]
    },
    options: chartOptions(null, [0, 100], true)
  });

  if (chartKortInst) chartKortInst.destroy();
  chartKortInst = new Chart(document.getElementById('chart-kortisol'), {
    type: 'bar',
    data: {
      labels,
      datasets: [{
        label: 'Kortisol',
        data: kortData,
        backgroundColor: kortData.map(v => v > 70 ? 'rgba(224,92,110,.75)' : v > 50 ? 'rgba(242,166,90,.75)' : 'rgba(90,176,138,.6)'),
        borderRadius: 6,
        borderSkipped: false
      }]
    },
    options: chartOptions('Kortisol', [0, 100])
  });

  if (chartWbInst) chartWbInst.destroy();
  chartWbInst = new Chart(document.getElementById('chart-wellbeing'), {
    type: 'line',
    data: {
      labels,
      datasets: [{
        label: 'Wellbeing Score',
        data: wbData,
        fill: true,
        backgroundColor: 'rgba(92,61,143,.12)',
        borderColor: '#5C3D8F',
        borderWidth: 2.5,
        pointBackgroundColor: '#5C3D8F',
        pointRadius: 5,
        tension: 0.4
      }]
    },
    options: chartOptions('Wellbeing', [0, 100])
  });
}

/* ══════════════════════════════════════════════════
   CHART HELPERS
═══════════════════════════════════════════════════*/
function chartOptions(title, yRange, legend = false) {
  return {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: { display: legend, position: 'bottom', labels: { font: { family: 'Plus Jakarta Sans', size: 11 }, usePointStyle: true, padding: 14 } },
      tooltip: { bodyFont: { family: 'Plus Jakarta Sans' }, titleFont: { family: 'Plus Jakarta Sans', weight: '700' } }
    },
    scales: {
      x: { grid: { display: false }, ticks: { font: { family: 'Plus Jakarta Sans', size: 11 }, color: '#8B7DAB' } },
      y: {
        min: yRange[0], max: yRange[1],
        grid: { color: 'rgba(196,176,232,.18)' },
        ticks: { font: { family: 'Plus Jakarta Sans', size: 11 }, color: '#8B7DAB', stepSize: 20 }
      }
    }
  };
}

function getLast7Labels(entries) {
  // entries are newest-first; we want oldest-first for chart
  return [...entries].reverse().map(e => {
    const d = new Date(e.date);
    return `${d.getDate()}/${d.getMonth()+1}`;
  });
}
function getLast7Data(entries, key) {
  return [...entries].reverse().map(e => e[key]);
}

/* ══════════════════════════════════════════════════
   KONSELING RENDER
═══════════════════════════════════════════════════*/
const konselorData = [
  { name: 'Dr. Sari Bulan, M.Psi.', role: 'Psikolog Klinis — Burnout & Stres Akademik', emoji: '👩‍💼', color: '#EDE8F9', days: 'Senin, Rabu, Jumat', hours: '09:00 – 15:00' },
  { name: 'Rizky Pratama, M.Psi.', role: 'Konselor Psikologi — Kecemasan & Identitas', emoji: '👨‍⚕️', color: '#E8F8F4', days: 'Selasa, Kamis', hours: '10:00 – 16:00' },
  { name: 'Dr. Ayu Wulandari', role: 'Dosen & Konselor — Relasi & Adaptasi Akademik', emoji: '👩‍🏫', color: '#E8F4FF', days: 'Senin – Jumat', hours: '13:00 – 17:00' },
];

function renderKonseling() {
  const el = document.getElementById('konselor-list');
  el.innerHTML = konselorData.map((k, i) => `
    <div class="konseling-card">
      <div class="konselor-header">
        <div class="konselor-avatar" style="background:${k.color}; font-size:26px;">${k.emoji}</div>
        <div>
          <div class="konselor-name">${k.name}</div>
          <div class="konselor-role">${k.role}</div>
          <div style="font-size:11px;color:var(--amethyst);margin-top:4px;font-weight:600;">📅 ${k.days} &nbsp;·&nbsp; 🕐 ${k.hours}</div>
        </div>
      </div>
      <button class="btn-jadwal" onclick="toggleScheduleForm(${i})">🗓️ Ajukan Jadwal Konseling</button>
      <div class="schedule-form" id="schedule-form-${i}">
        <div class="form-group">
          <label class="form-label">Nama Mahasiswa</label>
          <input class="form-input" type="text" placeholder="Nama lengkap" />
        </div>
        <div class="form-group">
          <label class="form-label">NIM</label>
          <input class="form-input" type="text" placeholder="Nomor Induk Mahasiswa" />
        </div>
        <div class="form-group">
          <label class="form-label">Pilih Tanggal</label>
          <input class="form-input" type="date" />
        </div>
        <div class="form-group">
          <label class="form-label">Topik yang Ingin Dibahas</label>
          <input class="form-input" type="text" placeholder="misal: Stres skripsi, masalah konsentrasi…" />
        </div>
        <button class="btn-confirm-jadwal" onclick="confirmJadwal(${i})">✅ Konfirmasi Jadwal</button>
      </div>
    </div>
  `).join('');
}

function toggleScheduleForm(i) {
  const f = document.getElementById('schedule-form-' + i);
  f.classList.toggle('open');
}

function confirmJadwal(i) {
  const f = document.getElementById('schedule-form-' + i);
  const inputs = f.querySelectorAll('.form-input');
  const filled = [...inputs].every(inp => inp.value.trim() !== '');
  if (!filled) { showToast('⚠️ Lengkapi semua kolom jadwal terlebih dahulu.'); return; }
  showToast('✅ Permintaan jadwal konseling berhasil dikirim!');
  f.classList.remove('open');
  inputs.forEach(inp => inp.value = '');
}

/* ══════════════════════════════════════════════════
   RESOURCES RENDER
═══════════════════════════════════════════════════*/
const resourcesData = [
  { icon: '🧘', title: 'Teknik Pernapasan 4-7-8 untuk Meredakan Stres', desc: 'Teknik pernapasan terbukti secara klinis yang bisa dilakukan kapan saja, di mana saja, dalam 2 menit.' },
  { icon: '📓', title: 'Panduan Journaling Terstruktur untuk Mahasiswa', desc: 'Metode menulis reflektif yang membantu mengurai pikiran ruwet dan menemukan solusi secara mandiri.' },
  { icon: '⏱️', title: 'Manajemen Waktu dengan Teknik Pomodoro Akademik', desc: 'Adaptasi metode Pomodoro khusus untuk menulis skripsi, revisi proposal, dan analisis data.' },
  { icon: '🌿', title: 'Mengenal Burnout Akademik: Tanda & Cara Mengatasinya', desc: 'Artikel edukasi psikologi tentang perbedaan kelelahan biasa dan burnout yang butuh intervensi.' },
  { icon: '💬', title: 'Cara Berkomunikasi Asertif dengan Dosen Pembimbing', desc: 'Panduan komunikasi sehat untuk menyampaikan kesulitan revisi atau menjadwal bimbingan.' },
  { icon: '📞', title: 'Hotline Kesehatan Mental Mahasiswa', desc: 'Kontak darurat dan saluran bantuan psikologis 24 jam dari Kemenkes & universitas mitra. ☎ 119 ext. 8' },
];

function renderResources() {
  document.getElementById('resources-list').innerHTML = resourcesData.map(r => `
    <div class="resource-item">
      <div class="resource-icon">${r.icon}</div>
      <div>
        <div class="resource-title">${r.title}</div>
        <div class="resource-desc">${r.desc}</div>
        <span class="resource-link" onclick="showToast('📖 Membuka materi — segera hadir di PELITA.')">Baca Selengkapnya →</span>
      </div>
    </div>
  `).join('');
}

/* ══════════════════════════════════════════════════
   UTILS
═══════════════════════════════════════════════════*/
function formatDate(d) {
  const days = ['Minggu','Senin','Selasa','Rabu','Kamis','Jumat','Sabtu'];
  const months = ['Januari','Februari','Maret','April','Mei','Juni','Juli','Agustus','September','Oktober','November','Desember'];
  return `${days[d.getDay()]}, ${d.getDate()} ${months[d.getMonth()]} ${d.getFullYear()} — ${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}`;
}

function moodEmoji(mood) {
  const map = { 'Sangat Baik': '😄', 'Baik': '🙂', 'Netral': '😐', 'Kurang': '😔', 'Buruk': '😞' };
  return map[mood] || '🙂';
}

function truncate(str, n) {
  return str.length > n ? str.slice(0, n) + '…' : str;
}

/* ══════════════════════════════════════════════════
   INIT
═══════════════════════════════════════════════════*/
(function init() {
  initSliders();
  updateLiveScore();
  renderDashboard();
  renderResources();
})();
</script>
</body>
</html>
