<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Aperture — AI UX Copilot</title>
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'%3E%3Ccircle cx='16' cy='16' r='15' fill='%230A0A0F'/%3E%3Cg fill='none' stroke='%238C7AFE' stroke-width='2'%3E%3Cpath d='M16 4 L22 13 L16 16 Z'/%3E%3Cpath d='M28 16 L19 22 L16 16 Z'/%3E%3Cpath d='M16 28 L10 19 L16 16 Z'/%3E%3Cpath d='M4 16 L13 10 L16 16 Z'/%3E%3C/g%3E%3C/svg%3E">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,400;0,9..144,500;0,9..144,600;0,9..144,700;1,9..144,500&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0A0A0F;
  --surface:#15131E;
  --surface-2:#1C1A28;
  --surface-3:#242131;
  --border:#2B2838;
  --border-soft:#201E2B;
  --text:#F3F1FA;
  --text-dim:#A7A2C2;
  --text-faint:#716C8C;
  --violet:#8C7AFE;
  --violet-dim:#5B4FC4;
  --violet-glow:rgba(140,122,254,0.35);
  --cyan:#5EE6D0;
  --coral:#FF8166;
  --success:#4ADE91;
  --warning:#F5B954;
  --error:#FF6B81;
  --info:#7CB8FF;
  --radius-sm:8px;
  --radius-md:14px;
  --radius-lg:20px;
  --font-display:'Fraunces',serif;
  --font-body:'Inter',sans-serif;
  --font-mono:'JetBrains Mono',monospace;
  --ease:cubic-bezier(.22,1,.36,1);
}
@media (prefers-reduced-motion: reduce){
  *,*::before,*::after{animation:none!important;transition:none!important;}
}
*{box-sizing:border-box;}
html,body{margin:0;padding:0;}
body{
  background:
    radial-gradient(900px 500px at 85% -10%, rgba(140,122,254,0.14), transparent 60%),
    radial-gradient(700px 400px at 0% 100%, rgba(94,230,208,0.08), transparent 60%),
    var(--bg);
  color:var(--text);
  font-family:var(--font-body);
  font-size:15px;
  line-height:1.5;
  min-height:100vh;
  -webkit-font-smoothing:antialiased;
}
::selection{background:var(--violet-glow);color:#fff;}
a{color:inherit;}
button{font-family:inherit;}
:focus-visible{outline:2px solid var(--violet);outline-offset:2px;border-radius:4px;}
.hidden{display:none!important;}
::-webkit-scrollbar{width:10px;height:10px;}
::-webkit-scrollbar-thumb{background:var(--surface-3);border-radius:8px;border:2px solid transparent;background-clip:padding-box;}
::-webkit-scrollbar-track{background:transparent;}

/* ---------- Shell ---------- */
.shell{
  display:grid;
  grid-template-columns:232px 1fr 300px;
  max-width:1680px;
  margin:0 auto;
  min-height:100vh;
  gap:0;
}
.sidebar{
  border-right:1px solid var(--border-soft);
  padding:22px 16px;
  display:flex;
  flex-direction:column;
  position:sticky;
  top:0;
  height:100vh;
}
.brand{
  display:flex;align-items:center;gap:10px;
  padding:4px 6px 22px;
}
.brand svg{flex-shrink:0;filter:drop-shadow(0 0 10px var(--violet-glow));}
.brand-name{font-family:var(--font-display);font-weight:600;font-size:19px;letter-spacing:0.2px;}
.brand-sub{font-size:11px;color:var(--text-faint);margin-top:1px;}

.nav{display:flex;flex-direction:column;gap:2px;margin-top:4px;}
.nav-item{
  display:flex;align-items:center;gap:11px;
  padding:9px 11px;border-radius:var(--radius-sm);
  color:var(--text-dim);text-decoration:none;cursor:pointer;
  font-size:13.5px;font-weight:500;
  border:1px solid transparent;
  background:none;width:100%;text-align:left;
  transition:background .15s var(--ease),color .15s var(--ease),border-color .15s var(--ease);
}
.nav-item svg{flex-shrink:0;opacity:.85;}
.nav-item:hover{background:var(--surface-2);color:var(--text);}
.nav-item.active{background:linear-gradient(135deg,rgba(140,122,254,0.16),rgba(94,230,208,0.06));color:var(--text);border-color:rgba(140,122,254,0.3);}
.nav-item.active svg{opacity:1;}
.nav-group-label{font-size:11px;color:var(--text-faint);text-transform:none;margin:18px 11px 6px;font-weight:600;}

.sidebar-spacer{flex:1;}
.export-card{
  background:var(--surface-2);border:1px solid var(--border);
  border-radius:var(--radius-md);padding:14px;
}
.export-card h4{margin:0 0 4px;font-size:13px;font-weight:600;}
.export-card p{margin:0 0 10px;font-size:12px;color:var(--text-dim);line-height:1.45;}
.btn-ghost-sm{
  font-size:12px;font-weight:600;color:var(--violet);background:none;border:1px solid var(--violet-dim);
  padding:6px 10px;border-radius:7px;cursor:pointer;transition:background .15s;
}
.btn-ghost-sm:hover{background:rgba(140,122,254,0.12);}

/* ---------- Main ---------- */
.main{padding:26px 28px 60px;min-width:0;}
.topbar{display:flex;justify-content:space-between;align-items:flex-start;gap:16px;flex-wrap:wrap;}
.greeting h1{font-family:var(--font-display);font-size:28px;font-weight:600;margin:0 0 4px;letter-spacing:.2px;}
.greeting p{margin:0;color:var(--text-dim);font-size:14px;}
.greeting-name{cursor:pointer;border-bottom:1px dashed var(--text-faint);}
.topbar-actions{display:flex;gap:8px;align-items:center;}
.icon-btn{
  width:36px;height:36px;border-radius:50%;display:flex;align-items:center;justify-content:center;
  background:var(--surface-2);border:1px solid var(--border);color:var(--text-dim);cursor:pointer;
  transition:transform .15s var(--ease),border-color .15s,color .15s;position:relative;
}
.icon-btn:hover{color:var(--text);border-color:var(--violet-dim);transform:translateY(-1px);}
.dot-badge{position:absolute;top:7px;right:7px;width:6px;height:6px;border-radius:50%;background:var(--coral);}

.pills{display:flex;gap:8px;flex-wrap:wrap;margin:20px 0 22px;}
.pill{
  padding:8px 14px;border-radius:999px;background:var(--surface-2);border:1px solid var(--border);
  color:var(--text-dim);font-size:13px;font-weight:500;cursor:pointer;
  transition:all .15s var(--ease);
}
.pill:hover{color:var(--text);border-color:var(--violet-dim);}
.pill.active{background:linear-gradient(135deg,var(--violet),#6a5ae0);color:#fff;border-color:transparent;}

.tabs{display:flex;gap:4px;border-bottom:1px solid var(--border-soft);margin-bottom:22px;}
.tab{
  padding:10px 4px;margin-right:18px;background:none;border:none;color:var(--text-faint);
  font-size:13.5px;font-weight:600;cursor:pointer;border-bottom:2px solid transparent;
  transition:color .15s;
}
.tab:hover{color:var(--text-dim);}
.tab.active{color:var(--text);border-bottom-color:var(--violet);}

.panel{display:none;}
.panel.active{display:block;}

/* reveal */
.reveal{opacity:0;transform:translateY(10px);}
.app-ready .reveal{opacity:1;transform:none;transition:opacity .5s var(--ease),transform .5s var(--ease);}
.reveal.d1{transition-delay:.03s;}
.reveal.d2{transition-delay:.09s;}
.reveal.d3{transition-delay:.15s;}
.reveal.d4{transition-delay:.21s;}

/* Hero ask card */
.hero-card{
  background:linear-gradient(160deg,var(--surface-2),var(--surface));
  border:1px solid var(--border);border-radius:var(--radius-lg);
  padding:22px;position:relative;overflow:hidden;
}
.hero-card::before{
  content:"";position:absolute;top:-60px;right:-60px;width:220px;height:220px;border-radius:50%;
  background:radial-gradient(circle,var(--violet-glow),transparent 70%);pointer-events:none;
}
.hero-top{display:flex;align-items:center;gap:10px;margin-bottom:12px;}
.hero-top .aperture-mini{width:26px;height:26px;}
.hero-top span{font-weight:600;font-size:14px;}
.hero-input-wrap{
  display:flex;align-items:flex-end;gap:8px;background:var(--surface);border:1px solid var(--border);
  border-radius:var(--radius-md);padding:12px 12px 12px 16px;
}
.hero-input-wrap textarea{
  flex:1;background:none;border:none;color:var(--text);font-family:inherit;font-size:14.5px;
  resize:none;min-height:24px;max-height:120px;line-height:1.5;
}
.hero-input-wrap textarea:focus{outline:none;}
.hero-input-wrap textarea::placeholder{color:var(--text-faint);}
.icon-btn-flat{background:none;border:none;color:var(--text-faint);cursor:pointer;padding:6px;border-radius:7px;display:flex;}
.icon-btn-flat:hover{color:var(--text-dim);background:var(--surface-2);}
.send-btn{
  background:linear-gradient(135deg,var(--violet),#6a5ae0);border:none;color:#fff;width:34px;height:34px;
  border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0;
  transition:transform .15s var(--ease),box-shadow .15s;
}
.send-btn:hover{transform:scale(1.06);box-shadow:0 4px 18px var(--violet-glow);}
.send-btn:disabled{opacity:.4;cursor:default;transform:none;box-shadow:none;}
.chip-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:14px;}
.chip{
  font-size:12.5px;padding:7px 12px;border-radius:999px;background:var(--surface-2);
  border:1px solid var(--border);color:var(--text-dim);cursor:pointer;transition:all .15s;
}
.chip:hover{color:var(--text);border-color:var(--cyan);}

/* stat / memory cards */
.grid-3{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-top:18px;}
.stat-card{
  background:var(--surface-2);border:1px solid var(--border);border-radius:var(--radius-md);
  padding:16px;transition:transform .15s var(--ease),border-color .15s;
}
.stat-card:hover{transform:translateY(-2px);border-color:var(--border);}
.stat-card .num{font-family:var(--font-display);font-size:26px;font-weight:600;}
.stat-card .label{font-size:12.5px;color:var(--text-dim);margin-top:2px;}

.section-head{display:flex;justify-content:space-between;align-items:center;margin:28px 0 12px;}
.section-head h3{font-size:15px;font-weight:600;margin:0;}
.section-head .meta{font-size:12px;color:var(--text-faint);}

.memory-list{display:flex;flex-direction:column;gap:8px;}
.memory-row{
  display:flex;align-items:center;gap:12px;background:var(--surface-2);border:1px solid var(--border);
  border-radius:var(--radius-sm);padding:11px 14px;font-size:13px;color:var(--text-dim);
}
.memory-row .swatch-stack{display:flex;}
.memory-row .swatch-stack span{width:16px;height:16px;border-radius:50%;border:2px solid var(--surface-2);margin-left:-6px;}
.memory-row .swatch-stack span:first-child{margin-left:0;}
.empty-state{
  text-align:center;padding:34px 20px;color:var(--text-faint);border:1px dashed var(--border);
  border-radius:var(--radius-md);font-size:13px;
}
.empty-state strong{display:block;color:var(--text-dim);font-size:13.5px;margin-bottom:4px;font-weight:600;}

/* -------- Color lab -------- */
.direction-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:16px;}
.direction-card{
  background:var(--surface-2);border:1px solid var(--border);border-radius:var(--radius-lg);
  padding:18px;display:flex;flex-direction:column;gap:12px;
}
.direction-card h4{margin:0;font-size:15px;font-weight:600;}
.direction-card .dir-desc{font-size:12.5px;color:var(--text-dim);margin:-6px 0 0;line-height:1.45;}
.swatch-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:6px;}
.swatch{
  aspect-ratio:1;border-radius:8px;cursor:pointer;border:1px solid rgba(255,255,255,0.08);
  position:relative;transition:transform .15s var(--ease);
}
.swatch:hover{transform:scale(1.08);z-index:2;}
.swatch-label{font-size:9.5px;color:var(--text-faint);text-align:center;margin-top:3px;font-family:var(--font-mono);}
.dir-actions{display:flex;gap:8px;margin-top:4px;}
.btn-primary{
  background:linear-gradient(135deg,var(--violet),#6a5ae0);color:#fff;border:none;padding:9px 14px;
  border-radius:8px;font-size:13px;font-weight:600;cursor:pointer;transition:transform .15s var(--ease),box-shadow .15s;
}
.btn-primary:hover{transform:translateY(-1px);box-shadow:0 6px 20px var(--violet-glow);}
.btn-secondary{
  background:var(--surface-3);color:var(--text-dim);border:1px solid var(--border);padding:9px 14px;
  border-radius:8px;font-size:13px;font-weight:600;cursor:pointer;transition:all .15s;
}
.btn-secondary:hover{color:var(--text);border-color:var(--violet-dim);}
.badge-active{font-size:10.5px;font-weight:700;color:var(--success);background:rgba(74,222,145,0.12);padding:3px 8px;border-radius:999px;letter-spacing:.3px;}

.swatch-popover{
  position:fixed;z-index:200;background:var(--surface-3);border:1px solid var(--border);border-radius:10px;
  padding:12px 14px;font-size:12px;box-shadow:0 10px 40px rgba(0,0,0,0.5);min-width:200px;
}
.swatch-popover .sp-row{display:flex;justify-content:space-between;gap:10px;padding:3px 0;font-family:var(--font-mono);color:var(--text-dim);}
.swatch-popover .sp-row b{color:var(--text);font-weight:500;}
.swatch-popover .sp-copy{margin-top:8px;width:100%;}

.preview-strip{
  margin-top:24px;border-radius:var(--radius-lg);padding:20px;border:1px solid var(--border);
}
.preview-strip .ps-row{display:flex;gap:10px;flex-wrap:wrap;align-items:center;margin-bottom:12px;}
.ps-btn{padding:8px 14px;border-radius:8px;font-size:13px;font-weight:600;border:none;cursor:default;}
.ps-badge{padding:4px 10px;border-radius:999px;font-size:11.5px;font-weight:600;}
.ps-alert{padding:10px 14px;border-radius:8px;font-size:12.5px;border:1px solid;flex:1;min-width:200px;}
.ps-toggle{display:flex;gap:6px;}
.ps-toggle button{font-size:11.5px;padding:5px 10px;border-radius:6px;border:1px solid var(--border);background:none;color:var(--text-dim);cursor:pointer;}
.ps-toggle button.on{background:var(--surface-3);color:var(--text);}

.contrast-box{
  margin-top:20px;background:var(--surface-2);border:1px solid var(--border);border-radius:var(--radius-md);
  padding:18px;
}
.contrast-box h4{margin:0 0 12px;font-size:14px;font-weight:600;}
.contrast-controls{display:flex;gap:10px;align-items:center;flex-wrap:wrap;}
.contrast-controls select{
  background:var(--surface-3);border:1px solid var(--border);color:var(--text);padding:8px 10px;
  border-radius:7px;font-size:12.5px;font-family:inherit;
}
.contrast-result{display:flex;align-items:center;gap:16px;margin-top:16px;flex-wrap:wrap;}
.contrast-sample{
  width:90px;height:60px;border-radius:8px;display:flex;align-items:center;justify-content:center;
  font-size:13px;font-weight:600;border:1px solid var(--border);
}
.contrast-ratio-num{font-family:var(--font-display);font-size:28px;font-weight:600;}
.pass-pill{font-size:11px;font-weight:700;padding:3px 9px;border-radius:999px;margin-left:6px;}
.pass{background:rgba(74,222,145,0.15);color:var(--success);}
.fail{background:rgba(255,107,129,0.15);color:var(--error);}

.export-row{display:flex;gap:8px;margin-top:16px;flex-wrap:wrap;}

/* -------- Type lab -------- */
.type-card{
  background:var(--surface-2);border:1px solid var(--border);border-radius:var(--radius-lg);
  padding:20px;margin-bottom:18px;
}
.type-card-head{display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:10px;margin-bottom:14px;}
.type-card-head h4{margin:0 0 4px;font-size:15px;font-weight:600;}
.type-card-head p{margin:0;font-size:12.5px;color:var(--text-dim);}
.type-specimen{background:var(--surface);border:1px solid var(--border-soft);border-radius:var(--radius-md);padding:20px;}
.spec-row{padding:7px 0;border-bottom:1px dashed var(--border-soft);}
.spec-row:last-child{border-bottom:none;}
.spec-tag{font-family:var(--font-mono);font-size:10.5px;color:var(--text-faint);display:block;margin-bottom:2px;}
.scale-controls{display:flex;gap:18px;margin:16px 0;flex-wrap:wrap;align-items:center;}
.scale-controls label{font-size:12px;color:var(--text-dim);display:flex;flex-direction:column;gap:4px;}
.scale-controls input[type=range]{width:160px;accent-color:var(--violet);}
.scale-controls select{background:var(--surface-3);border:1px solid var(--border);color:var(--text);padding:6px 8px;border-radius:6px;font-size:12px;}

/* -------- Insights -------- */
.tag-row{display:flex;gap:8px;margin-bottom:18px;flex-wrap:wrap;}
.article-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:14px;}
.article-card{
  background:var(--surface-2);border:1px solid var(--border);border-radius:var(--radius-md);padding:16px;
  display:flex;flex-direction:column;gap:8px;text-decoration:none;color:inherit;
  transition:transform .15s var(--ease),border-color .15s;
}
.article-card:hover{transform:translateY(-3px);border-color:var(--violet-dim);}
.article-card .src{font-size:11px;color:var(--cyan);font-weight:600;}
.article-card h5{margin:0;font-size:14px;font-weight:600;line-height:1.4;}
.article-card p{margin:0;font-size:12.5px;color:var(--text-dim);line-height:1.5;flex:1;}
.article-card .meta{font-size:11px;color:var(--text-faint);}
.skeleton{background:linear-gradient(90deg,var(--surface-2) 25%,var(--surface-3) 37%,var(--surface-2) 63%);background-size:400% 100%;animation:shimmer 1.4s ease infinite;border-radius:var(--radius-md);height:150px;}
@keyframes shimmer{0%{background-position:100% 0}100%{background-position:-100% 0}}

/* -------- History -------- */
.history-item{
  display:flex;justify-content:space-between;align-items:center;gap:10px;background:var(--surface-2);
  border:1px solid var(--border);border-radius:var(--radius-sm);padding:12px 14px;margin-bottom:8px;cursor:pointer;
  transition:border-color .15s;
}
.history-item:hover{border-color:var(--violet-dim);}
.history-item .ht{font-size:13px;font-weight:500;}
.history-item .hd{font-size:11px;color:var(--text-faint);margin-top:2px;}
.history-item button{background:none;border:none;color:var(--text-faint);cursor:pointer;padding:4px;}
.history-item button:hover{color:var(--error);}

/* ---------- Right panel ---------- */
.rightpanel{border-left:1px solid var(--border-soft);padding:26px 20px;}
.rp-head{font-size:12.5px;font-weight:700;color:var(--text-faint);margin-bottom:12px;display:flex;align-items:center;gap:6px;}
.rp-conv{
  background:var(--surface-2);border:1px solid var(--border);border-radius:var(--radius-sm);padding:11px 12px;
  margin-bottom:8px;cursor:pointer;transition:border-color .15s;
}
.rp-conv:hover{border-color:var(--violet-dim);}
.rp-conv .t{font-size:12.5px;font-weight:500;}
.rp-conv .d{font-size:10.5px;color:var(--text-faint);margin-top:3px;}
.rp-insight{display:flex;gap:10px;padding:10px 0;border-bottom:1px solid var(--border-soft);text-decoration:none;color:inherit;}
.rp-insight:last-child{border-bottom:none;}
.rp-insight .dotmark{width:6px;height:6px;border-radius:50%;background:var(--cyan);margin-top:6px;flex-shrink:0;}
.rp-insight .ti{font-size:12.5px;font-weight:500;line-height:1.4;}
.rp-insight .tm{font-size:10.5px;color:var(--text-faint);margin-top:2px;}

/* ---------- FAB + Chat drawer ---------- */
.ai-fab{
  position:fixed;bottom:26px;right:26px;width:58px;height:58px;border-radius:50%;
  background:linear-gradient(135deg,var(--violet),#6a5ae0);border:none;cursor:pointer;z-index:500;
  display:flex;align-items:center;justify-content:center;box-shadow:0 8px 30px var(--violet-glow);
  transition:transform .2s var(--ease),box-shadow .2s;
}
.ai-fab:hover{transform:scale(1.08);box-shadow:0 10px 36px var(--violet-glow);}
.ai-fab .pulse{
  position:absolute;inset:0;border-radius:50%;border:2px solid var(--violet);opacity:0;
  animation:fabpulse 2.6s ease-out infinite;
}
@keyframes fabpulse{0%{transform:scale(1);opacity:.6;}100%{transform:scale(1.6);opacity:0;}}

.chat-drawer{
  position:fixed;top:0;right:0;height:100vh;width:400px;max-width:100vw;background:var(--surface);
  border-left:1px solid var(--border);z-index:600;display:flex;flex-direction:column;
  transform:translateX(100%);transition:transform .32s var(--ease);box-shadow:-20px 0 60px rgba(0,0,0,0.4);
}
.chat-drawer.open{transform:translateX(0);}
.cd-head{display:flex;align-items:center;justify-content:space-between;padding:16px 18px;border-bottom:1px solid var(--border-soft);}
.cd-head-left{display:flex;align-items:center;gap:10px;}
.cd-title{font-size:14.5px;font-weight:600;}
.cd-status{font-size:11px;color:var(--text-faint);display:flex;align-items:center;gap:5px;margin-top:1px;}
.cd-status .sd{width:6px;height:6px;border-radius:50%;background:var(--success);}
.cd-head-right{display:flex;gap:4px;}
.cd-messages{flex:1;overflow-y:auto;padding:16px 18px;display:flex;flex-direction:column;gap:12px;}
.msg{max-width:88%;padding:11px 14px;border-radius:14px;font-size:13.5px;line-height:1.55;white-space:pre-wrap;}
.msg.user{align-self:flex-end;background:linear-gradient(135deg,var(--violet),#6a5ae0);color:#fff;border-bottom-right-radius:4px;}
.msg.bot{align-self:flex-start;background:var(--surface-2);border:1px solid var(--border);border-bottom-left-radius:4px;}
.msg.bot .tag{display:block;font-size:10px;font-weight:700;color:var(--cyan);margin-bottom:5px;letter-spacing:.2px;}
.typing{align-self:flex-start;display:flex;gap:4px;padding:12px 14px;background:var(--surface-2);border:1px solid var(--border);border-radius:14px;border-bottom-left-radius:4px;}
.typing span{width:6px;height:6px;border-radius:50%;background:var(--text-faint);animation:bounce 1.2s infinite;}
.typing span:nth-child(2){animation-delay:.15s;}
.typing span:nth-child(3){animation-delay:.3s;}
@keyframes bounce{0%,60%,100%{transform:translateY(0);opacity:.5;}30%{transform:translateY(-4px);opacity:1;}}
.cd-chips{display:flex;gap:6px;flex-wrap:wrap;padding:0 18px 12px;}
.cd-chips .chip{font-size:11.5px;padding:5px 10px;}
.cd-input-row{display:flex;gap:8px;align-items:flex-end;padding:14px 18px;border-top:1px solid var(--border-soft);}
.cd-input-row textarea{
  flex:1;background:var(--surface-2);border:1px solid var(--border);color:var(--text);font-family:inherit;
  font-size:13.5px;border-radius:12px;padding:10px 12px;resize:none;max-height:100px;min-height:20px;
}
.cd-input-row textarea:focus{outline:none;border-color:var(--violet-dim);}

.cd-settings{padding:16px 18px;border-bottom:1px solid var(--border-soft);display:none;}
.cd-settings.open{display:block;}
.cd-settings label{font-size:11.5px;color:var(--text-dim);display:block;margin-bottom:6px;font-weight:600;}
.cd-settings input[type=password],.cd-settings input[type=text]{
  width:100%;background:var(--surface-2);border:1px solid var(--border);color:var(--text);padding:8px 10px;
  border-radius:7px;font-size:12.5px;font-family:var(--font-mono);margin-bottom:8px;
}
.cd-settings p{font-size:11px;color:var(--text-faint);line-height:1.5;margin:6px 0 10px;}
.cd-settings .row{display:flex;gap:8px;}

.toast{
  position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(20px);background:var(--surface-3);
  border:1px solid var(--border);color:var(--text);padding:10px 18px;border-radius:999px;font-size:12.5px;
  z-index:999;opacity:0;transition:all .25s var(--ease);box-shadow:0 10px 30px rgba(0,0,0,0.4);pointer-events:none;
}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0);}

.overlay-mask{position:fixed;inset:0;background:rgba(0,0,0,0.4);z-index:590;opacity:0;pointer-events:none;transition:opacity .25s;}
.overlay-mask.show{opacity:1;pointer-events:auto;}

/* ---------- Responsive ---------- */
@media (max-width:1180px){
  .shell{grid-template-columns:220px 1fr;}
  .rightpanel{display:none;}
}
@media (max-width:860px){
  .shell{grid-template-columns:1fr;}
  .sidebar{position:static;height:auto;flex-direction:row;align-items:center;padding:14px 16px;border-right:none;border-bottom:1px solid var(--border-soft);overflow-x:auto;}
  .brand{padding:0 14px 0 0;}
  .nav{flex-direction:row;}
  .nav-group-label,.sidebar-spacer,.export-card{display:none;}
  .nav-item{white-space:nowrap;}
  .main{padding:20px 16px 60px;}
  .grid-3{grid-template-columns:1fr;}
  .chat-drawer{width:100%;}
}
</style>
</head>
<body>

<div class="shell" id="shell">

  <!-- SIDEBAR -->
  <aside class="sidebar">
    <div class="brand">
      <svg class="aperture-mini" width="30" height="30" viewBox="0 0 32 32" fill="none">
        <circle cx="16" cy="16" r="15" fill="#15131E" stroke="#2B2838"/>
        <path d="M16 5 L23 14 L16 16.5 Z" fill="#8C7AFE"/>
        <path d="M27 16 L18 22 L16 16.5 Z" fill="#6a5ae0"/>
        <path d="M16 27 L9 18 L16 16.5 Z" fill="#5EE6D0"/>
        <path d="M5 16 L14 10 L16 16.5 Z" fill="#7a6ce0"/>
      </svg>
      <div>
        <div class="brand-name">Aperture</div>
        <div class="brand-sub">AI UX Copilot</div>
      </div>
    </div>
    <nav class="nav">
      <button class="nav-item active" data-tab="overview">
        <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg>
        Overview
      </button>
      <button class="nav-item" data-tab="colorlab">
        <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="13.5" cy="6.5" r=".5" fill="currentColor"/><circle cx="17.5" cy="10.5" r=".5" fill="currentColor"/><circle cx="8.5" cy="7.5" r=".5" fill="currentColor"/><circle cx="6.5" cy="12.5" r=".5" fill="currentColor"/><path d="M12 22a10 10 0 1 1 7-17c1 1 1.5 2 1 3.5-.5 1.3-1.7 1.5-3 1.5h-1.5c-1.4 0-2.5 1.1-2.5 2.5 0 .7.3 1.2.6 1.7.4.6.9 1.3.4 2.1-.4.7-1.2 1-2 1.2-.6.2-.9.7-.9 1.3 0 2.3 2 2.7 2 3.2"/></svg>
        Color Lab
      </button>
      <button class="nav-item" data-tab="typelab">
        <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 7V4h16v3"/><path d="M9 20h6"/><path d="M12 4v16"/></svg>
        Type Lab
      </button>
      <button class="nav-item" data-tab="insights">
        <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 11l18-5v12L3 14v-3z"/><path d="M11.6 16.3 13 21"/></svg>
        Insights
      </button>
      <button class="nav-item" data-tab="history">
        <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 12a9 9 0 1 0 3-6.7"/><path d="M3 4v5h5"/><path d="M12 7v5l3 3"/></svg>
        History
      </button>
    </nav>
    <div class="sidebar-spacer"></div>
    <div class="export-card">
      <h4>Export workspace</h4>
      <p>Download your active palette, type pairing and session summary as one token bundle.</p>
      <button class="btn-ghost-sm" id="exportWorkspaceBtn">Download .json</button>
    </div>
  </aside>

  <!-- MAIN -->
  <main class="main">
    <div class="topbar reveal">
      <div class="greeting">
        <h1 id="greetingText">Good day, <span class="greeting-name" id="nameSpan" title="Click to set your name">there</span></h1>
        <p>What are we designing today?</p>
      </div>
      <div class="topbar-actions">
        <button class="icon-btn" id="notifBtn" aria-label="Notifications">
          <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 8a6 6 0 1 0-12 0c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.7 21a2 2 0 0 1-3.4 0"/></svg>
          <span class="dot-badge"></span>
        </button>
        <button class="icon-btn" id="helpBtn" aria-label="Keyboard laws reference">
          <svg width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M9.5 9a2.5 2.5 0 1 1 3.5 2.3c-.8.4-1 .9-1 1.7"/><path d="M12 17h.01"/></svg>
        </button>
      </div>
    </div>

    <div class="pills reveal d1" id="pillRow">
      <button class="pill active" data-prompt="audit">Audit a screen</button>
      <button class="pill" data-prompt="palette">Generate palette</button>
      <button class="pill" data-prompt="typepair">Pair typography</button>
      <button class="pill" data-prompt="law">Explain a UX law</button>
      <button class="pill" data-prompt="refs">Find references</button>
    </div>

    <div class="tabs reveal d1">
      <button class="tab active" data-paneltab="overview">Overview</button>
      <button class="tab" data-paneltab="colorlab">Color Lab</button>
      <button class="tab" data-paneltab="typelab">Type Lab</button>
      <button class="tab" data-paneltab="insights">Insights</button>
      <button class="tab" data-paneltab="history">History</button>
    </div>

    <!-- OVERVIEW -->
    <section class="panel active" id="panel-overview">
      <div class="hero-card reveal d2">
        <div class="hero-top">
          <svg class="aperture-mini" width="24" height="24" viewBox="0 0 32 32" fill="none">
            <circle cx="16" cy="16" r="15" fill="#1C1A28" stroke="#2B2838"/>
            <path d="M16 5 L23 14 L16 16.5 Z" fill="#8C7AFE"/><path d="M27 16 L18 22 L16 16.5 Z" fill="#6a5ae0"/>
            <path d="M16 27 L9 18 L16 16.5 Z" fill="#5EE6D0"/><path d="M5 16 L14 10 L16 16.5 Z" fill="#7a6ce0"/>
          </svg>
          <span>Ask Aperture</span>
        </div>
        <div class="hero-input-wrap">
          <textarea id="heroInput" rows="1" placeholder="Describe a screen, a decision, or paste a problem you're stuck on..."></textarea>
          <button class="icon-btn-flat" id="micBtn" aria-label="Voice input" title="Voice input">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 15a3 3 0 0 0 3-3V6a3 3 0 0 0-6 0v6a3 3 0 0 0 3 3z"/><path d="M19 11a7 7 0 0 1-14 0"/><path d="M12 18v4"/></svg>
          </button>
          <button class="send-btn" id="heroSendBtn" aria-label="Send">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2"><path d="M22 2 11 13"/><path d="M22 2 15 22l-4-9-9-4 20-7z"/></svg>
          </button>
        </div>
        <div class="chip-row">
          <button class="chip" data-ask="Audit my onboarding flow for first-time users">Audit onboarding flow</button>
          <button class="chip" data-ask="Build a dark-mode palette for a fintech dashboard">Dark fintech palette</button>
          <button class="chip" data-ask="Explain Fitts's Law with a UI example">Explain Fitts's Law</button>
          <button class="chip" data-ask="What's wrong with using a modal for a destructive delete confirmation?">Delete confirmation UX</button>
        </div>
      </div>

      <div class="grid-3 reveal d3">
        <div class="stat-card"><div class="num" id="statConvos">0</div><div class="label">Conversations saved</div></div>
        <div class="stat-card"><div class="num" id="statPalettes">0</div><div class="label">Palettes generated</div></div>
        <div class="stat-card"><div class="num" id="statFonts">0</div><div class="label">Pairings compared</div></div>
      </div>

      <div class="section-head reveal d4"><h3>Project memory</h3><span class="meta">Decisions stay local to this browser</span></div>
      <div class="memory-list reveal d4" id="memoryList"></div>
    </section>

    <!-- COLOR LAB -->
    <section class="panel" id="panel-colorlab">
      <div class="section-head" style="margin-top:0"><h3>Three directions, one decision</h3><span class="meta">Click a swatch for exact values</span></div>
      <div class="direction-grid" id="directionGrid"></div>

      <div class="preview-strip" id="previewStrip">
        <div class="section-head" style="margin:0 0 14px"><h3>Live preview — <span id="activePaletteName">no palette selected</span></h3>
          <div class="ps-toggle" id="themeToggle"><button data-mode="dark" class="on">Dark</button><button data-mode="light">Light</button></div>
        </div>
        <div class="ps-row" id="psButtons"></div>
        <div class="ps-row" id="psAlerts"></div>
      </div>

      <div class="contrast-box">
        <h4>Contrast checker (WCAG 2.2)</h4>
        <div class="contrast-controls">
          Text <select id="contrastFg"></select>
          on <select id="contrastBg"></select>
        </div>
        <div class="contrast-result" id="contrastResult"></div>
      </div>

      <div class="export-row">
        <button class="btn-secondary" data-export="css">Export CSS variables</button>
        <button class="btn-secondary" data-export="tailwind">Export Tailwind config</button>
        <button class="btn-secondary" data-export="json">Export JSON tokens</button>
      </div>
    </section>

    <!-- TYPE LAB -->
    <section class="panel" id="panel-typelab">
      <div class="section-head" style="margin-top:0"><h3>Typography pairings</h3><span class="meta">Real Google Fonts, loaded on demand</span></div>
      <div id="typeCardsWrap"></div>
      <div class="export-row">
        <button class="btn-secondary" data-texport="css">Export CSS variables</button>
        <button class="btn-secondary" data-texport="json">Export JSON tokens</button>
      </div>
    </section>

    <!-- INSIGHTS -->
    <section class="panel" id="panel-insights">
      <div class="section-head" style="margin-top:0"><h3>Trending in UX &amp; design</h3><span class="meta">Live from Medium</span></div>
      <div class="tag-row" id="tagRow">
        <button class="pill active" data-tag="ux-design">UX Design</button>
        <button class="pill" data-tag="design-systems">Design Systems</button>
        <button class="pill" data-tag="accessibility">Accessibility</button>
        <button class="pill" data-tag="artificial-intelligence">AI Products</button>
      </div>
      <div id="articleWrap" class="article-grid"></div>
    </section>

    <!-- HISTORY -->
    <section class="panel" id="panel-history">
      <div class="section-head" style="margin-top:0"><h3>Conversation history</h3><button class="btn-ghost-sm" id="clearHistoryBtn">Clear all</button></div>
      <div id="historyWrap"></div>
    </section>
  </main>

  <!-- RIGHT PANEL -->
  <aside class="rightpanel">
    <div class="rp-head">RECENT CONVERSATIONS</div>
    <div id="rpConvos"></div>
    <div class="rp-head" style="margin-top:26px">TRENDING NOW</div>
    <div id="rpInsights"></div>
  </aside>
</div>

<!-- FAB -->
<button class="ai-fab" id="fabBtn" aria-label="Open Aperture AI chat">
  <span class="pulse"></span>
  <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2"><path d="M12 3v2"/><path d="M12 19v2"/><path d="m4.2 4.2 1.4 1.4"/><path d="m18.4 18.4 1.4 1.4"/><path d="M3 12h2"/><path d="M19 12h2"/><path d="m4.2 19.8 1.4-1.4"/><path d="m18.4 5.6 1.4-1.4"/><circle cx="12" cy="12" r="4"/></svg>
</button>

<div class="overlay-mask" id="overlayMask"></div>

<!-- CHAT DRAWER -->
<div class="chat-drawer" id="chatDrawer">
  <div class="cd-head">
    <div class="cd-head-left">
      <svg width="22" height="22" viewBox="0 0 32 32" fill="none"><circle cx="16" cy="16" r="15" fill="#1C1A28" stroke="#2B2838"/><path d="M16 5 L23 14 L16 16.5 Z" fill="#8C7AFE"/><path d="M27 16 L18 22 L16 16.5 Z" fill="#6a5ae0"/><path d="M16 27 L9 18 L16 16.5 Z" fill="#5EE6D0"/><path d="M5 16 L14 10 L16 16.5 Z" fill="#7a6ce0"/></svg>
      <div>
        <div class="cd-title">Ask Aperture</div>
        <div class="cd-status"><span class="sd" id="statusDot"></span><span id="statusText">Local reasoning engine</span></div>
      </div>
    </div>
    <div class="cd-head-right">
      <button class="icon-btn-flat" id="settingsToggle" aria-label="Settings" title="Connect your own API key">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.68 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 1 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
      </button>
      <button class="icon-btn-flat" id="closeDrawerBtn" aria-label="Close">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 6 6 18"/><path d="M6 6l12 12"/></svg>
      </button>
    </div>
  </div>

  <div class="cd-settings" id="cdSettings">
    <label>Your name</label>
    <input type="text" id="nameInput" placeholder="e.g. Priya">
    <label>Anthropic API key (optional)</label>
    <input type="password" id="apiKeyInput" placeholder="sk-ant-...">
    <p>Stored only in this browser's local storage and sent directly from your browser to Anthropic when set. Leave blank to use the built-in local UX-reasoning engine — no key needed, fully offline.</p>
    <div class="row">
      <button class="btn-primary" id="saveSettingsBtn">Save</button>
      <button class="btn-secondary" id="clearKeyBtn">Clear key</button>
    </div>
  </div>

  <div class="cd-messages" id="cdMessages" aria-live="polite"></div>
  <div class="cd-chips">
    <button class="chip" data-ask="List all 10 of Nielsen's usability heuristics briefly">Nielsen heuristics</button>
    <button class="chip" data-ask="Explain Hick's Law">Hick's Law</button>
    <button class="chip" data-ask="What dark patterns should I avoid?">Dark patterns</button>
    <button class="chip" data-ask="What's the minimum touch target size for accessibility?">Touch targets</button>
  </div>
  <div class="cd-input-row">
    <textarea id="cdInput" rows="1" placeholder="Ask about your design, theme or requirement..."></textarea>
    <button class="send-btn" id="cdSendBtn" aria-label="Send">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2"><path d="M22 2 11 13"/><path d="M22 2 15 22l-4-9-9-4 20-7z"/></svg>
    </button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
(function(){
"use strict";

/* ============ STATE ============ */
const STORE_KEY='aperture_state_v2';
function defaultState(){
  return {
    name:'', apiKey:'',
    activePaletteId:null, paletteTheme:'dark', paletteUses:0,
    activeTypeId:null, typeUses:0,
    conversations:[]
  };
}
let state;
try{ state = Object.assign(defaultState(), JSON.parse(localStorage.getItem(STORE_KEY))||{}); }
catch(e){ state = defaultState(); }
function saveState(){ try{ localStorage.setItem(STORE_KEY, JSON.stringify(state)); }catch(e){} }

function $(sel,ctx){ return (ctx||document).querySelector(sel); }
function $all(sel,ctx){ return Array.prototype.slice.call((ctx||document).querySelectorAll(sel)); }

function toast(msg){
  const t=$('#toast'); t.textContent=msg; t.classList.add('show');
  clearTimeout(toast._h); toast._h=setTimeout(()=>t.classList.remove('show'),2200);
}
function download(filename, content, mime){
  const blob=new Blob([content],{type:mime});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a'); a.href=url; a.download=filename;
  document.body.appendChild(a); a.click(); a.remove();
  setTimeout(()=>URL.revokeObjectURL(url),1000);
}
function escapeHtml(s){
  return (s||'').replace(/[&<>"']/g, c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

/* ============ COLOR MATH ============ */
function hexToRgb(hex){
  hex=hex.replace('#','');
  if(hex.length===3) hex=hex.split('').map(c=>c+c).join('');
  const n=parseInt(hex,16);
  return [(n>>16)&255,(n>>8)&255,n&255];
}
function rgbToHsl(r,g,b){
  r/=255;g/=255;b/=255;
  const max=Math.max(r,g,b),min=Math.min(r,g,b);
  let h,s,l=(max+min)/2;
  if(max===min){h=s=0;}
  else{
    const d=max-min;
    s=l>0.5?d/(2-max-min):d/(max+min);
    switch(max){
      case r:h=(g-b)/d+(g<b?6:0);break;
      case g:h=(b-r)/d+2;break;
      default:h=(r-g)/d+4;
    }
    h/=6;
  }
  return [Math.round(h*360),Math.round(s*100),Math.round(l*100)];
}
function relLuminance(r,g,b){
  const a=[r,g,b].map(v=>{v/=255;return v<=0.03928?v/12.92:Math.pow((v+0.055)/1.055,2.4);});
  return a[0]*0.2126+a[1]*0.7152+a[2]*0.0722;
}
function contrastRatio(hex1,hex2){
  const [r1,g1,b1]=hexToRgb(hex1), [r2,g2,b2]=hexToRgb(hex2);
  const l1=relLuminance(r1,g1,b1)+0.05, l2=relLuminance(r2,g2,b2)+0.05;
  return l1>l2? l1/l2 : l2/l1;
}

/* ============ PALETTE DIRECTIONS ============ */
const PALETTES=[
  {id:'professional', name:'Professional & Trustworthy', desc:'Deep institutional blue with a muted gold accent — built for fintech, healthcare and anything handling other people\'s money or records.',
    tokens:{
      primary:'#2F5FDB', secondary:'#0F3D91', accent:'#D8A94E',
      background:'#0B1220', surface:'#121A2B', text:'#EAF0FB', textSecondary:'#8FA3C4',
      border:'#223253', success:'#3DDC84', warning:'#F5B942', error:'#FF6B6B', info:'#4EA8FF'
    }},
  {id:'minimal', name:'Modern & Minimal', desc:'Near-monochrome ink and paper with a single violet accent doing all the talking. Works equally well light or dark.',
    tokens:{
      primary:'#6C5CE7', secondary:'#2D2D34', accent:'#B4A7F5',
      background:'#101014', surface:'#1A1A20', text:'#F2F2F5', textSecondary:'#8C8C96',
      border:'#28282F', success:'#34C77B', warning:'#E8A33D', error:'#E85D5D', info:'#4F8FEA'
    },
    light:{background:'#FAFAFA', surface:'#FFFFFF', text:'#16161A', textSecondary:'#5B5B66', border:'#E3E3E8'}},
  {id:'creative', name:'Creative & Distinctive', desc:'Violet iris against a coral flare — an aperture catching light. Best for AI products and anything that wants to feel alive.',
    tokens:{
      primary:'#8C7AFE', secondary:'#FF6F59', accent:'#5EE6D0',
      background:'#0A0A0F', surface:'#16141F', text:'#F5F2FF', textSecondary:'#A79FC9',
      border:'#2A2438', success:'#4ADE91', warning:'#F7B955', error:'#FF6B81', info:'#7CB8FF'
    }}
];
const TOKEN_LABELS=['primary','secondary','accent','background','surface','text','textSecondary','border','success','warning','error','info'];

function renderDirectionGrid(){
  const wrap=$('#directionGrid'); wrap.innerHTML='';
  PALETTES.forEach(p=>{
    const t=p.tokens;
    const card=document.createElement('div'); card.className='direction-card';
    const isActive=state.activePaletteId===p.id;
    card.innerHTML=
      '<h4>'+p.name+(isActive?' <span class="badge-active">ACTIVE</span>':'')+'</h4>'+
      '<p class="dir-desc">'+p.desc+'</p>'+
      '<div class="swatch-grid">'+TOKEN_LABELS.map(k=>
        '<div><div class="swatch" style="background:'+t[k]+'" data-hex="'+t[k]+'" data-label="'+k+'" data-pid="'+p.id+'"></div><div class="swatch-label">'+k+'</div></div>'
      ).join('')+'</div>'+
      '<div class="dir-actions"><button class="btn-primary" data-use="'+p.id+'">Use this palette</button><button class="btn-secondary" data-shuffle="'+p.id+'">Shuffle hue</button></div>';
    wrap.appendChild(card);
  });
}

let popoverEl=null;
function showSwatchPopover(e, hex, label){
  if(popoverEl) popoverEl.remove();
  const [r,g,b]=hexToRgb(hex), [h,s,l]=rgbToHsl(r,g,b);
  const pop=document.createElement('div'); pop.className='swatch-popover';
  pop.innerHTML=
    '<div class="sp-row"><span>Token</span><b>'+label+'</b></div>'+
    '<div class="sp-row"><span>HEX</span><b>'+hex.toUpperCase()+'</b></div>'+
    '<div class="sp-row"><span>RGB</span><b>'+r+', '+g+', '+b+'</b></div>'+
    '<div class="sp-row"><span>HSL</span><b>'+h+'°, '+s+'%, '+l+'%</b></div>'+
    '<button class="btn-secondary sp-copy">Copy HEX</button>';
  document.body.appendChild(pop);
  const rect=e.target.getBoundingClientRect();
  let left=rect.left, top=rect.bottom+8;
  if(left+210>window.innerWidth) left=window.innerWidth-220;
  if(top+160>window.innerHeight) top=rect.top-170;
  pop.style.left=left+'px'; pop.style.top=top+'px';
  pop.querySelector('.sp-copy').addEventListener('click',()=>{
    navigator.clipboard.writeText(hex.toUpperCase()).then(()=>toast('Copied '+hex.toUpperCase()));
  });
  popoverEl=pop;
  setTimeout(()=>document.addEventListener('click', closePopoverOnce),0);
}
function closePopoverOnce(e){
  if(popoverEl && !popoverEl.contains(e.target)){ popoverEl.remove(); popoverEl=null; }
  document.removeEventListener('click', closePopoverOnce);
}

function hueShift(hex, deg){
  const [r,g,b]=hexToRgb(hex);
  let [h,s,l]=rgbToHsl(r,g,b);
  h=(h+deg+360)%360;
  return hslToHex(h,s,l);
}
function hslToHex(h,s,l){
  s/=100;l/=100;
  const k=n=>(n+h/30)%12;
  const a=s*Math.min(l,1-l);
  const f=n=>l-a*Math.max(-1,Math.min(k(n)-3,Math.min(9-k(n),1)));
  const toHex=v=>Math.round(v*255).toString(16).padStart(2,'0');
  return '#'+toHex(f(0))+toHex(f(8))+toHex(f(4));
}

function getActivePalette(){ return PALETTES.find(p=>p.id===state.activePaletteId); }

function applyActivePalette(){
  const p=getActivePalette();
  $('#activePaletteName').textContent=p?p.name:'no palette selected';
  if(!p) return;
  let t=Object.assign({},p.tokens);
  if(state.paletteTheme==='light' && p.light) t=Object.assign(t,p.light);
  const strip=$('#previewStrip');
  strip.style.background=t.background;
  strip.style.borderColor=t.border;
  strip.style.color=t.text;

  $('#psButtons').innerHTML=
    '<button class="ps-btn" style="background:'+t.primary+';color:#fff">Primary action</button>'+
    '<button class="ps-btn" style="background:'+t.surface+';color:'+t.text+';border:1px solid '+t.border+'">Secondary</button>'+
    '<span class="ps-badge" style="background:'+t.success+'22;color:'+t.success+'">Success</span>'+
    '<span class="ps-badge" style="background:'+t.warning+'22;color:'+t.warning+'">Warning</span>'+
    '<span class="ps-badge" style="background:'+t.error+'22;color:'+t.error+'">Error</span>'+
    '<span class="ps-badge" style="background:'+t.info+'22;color:'+t.info+'">Info</span>';
  $('#psAlerts').innerHTML=
    '<div class="ps-alert" style="background:'+t.surface+';border-color:'+t.border+';color:'+t.text+'">This palette is now active across your workspace preview.</div>';

  const sel1=$('#contrastFg'), sel2=$('#contrastBg');
  [sel1,sel2].forEach(sel=>{
    sel.innerHTML=TOKEN_LABELS.map(k=>'<option value="'+t[k]+'">'+k+' ('+t[k]+')</option>').join('');
  });
  sel1.value=t.text; sel2.value=t.background;
  renderContrast();
}

function renderContrast(){
  const fg=$('#contrastFg').value, bg=$('#contrastBg').value;
  if(!fg||!bg) return;
  const ratio=contrastRatio(fg,bg);
  const aaNormal=ratio>=4.5, aaaNormal=ratio>=7, aaLarge=ratio>=3;
  $('#contrastResult').innerHTML=
    '<div class="contrast-sample" style="background:'+bg+';color:'+fg+'">Aa Text</div>'+
    '<div><div class="contrast-ratio-num">'+ratio.toFixed(2)+':1</div>'+
    '<div style="margin-top:6px">'+
    'Normal text AA <span class="pass-pill '+(aaNormal?'pass':'fail')+'">'+(aaNormal?'PASS':'FAIL')+'</span> '+
    'AAA <span class="pass-pill '+(aaaNormal?'pass':'fail')+'">'+(aaaNormal?'PASS':'FAIL')+'</span><br>'+
    'Large text AA <span class="pass-pill '+(aaLarge?'pass':'fail')+'">'+(aaLarge?'PASS':'FAIL')+'</span>'+
    '</div></div>';
}

function exportPalette(kind){
  const p=getActivePalette();
  if(!p){ toast('Pick a palette first'); return; }
  const t=p.tokens;
  if(kind==='css'){
    const css=':root {\n'+TOKEN_LABELS.map(k=>'  --color-'+k.replace(/([A-Z])/g,'-$1').toLowerCase()+': '+t[k]+';').join('\n')+'\n}\n';
    download('aperture-'+p.id+'-tokens.css', css, 'text/css');
  } else if(kind==='tailwind'){
    const js='module.exports = {\n  theme: {\n    extend: {\n      colors: {\n'+TOKEN_LABELS.map(k=>'        '+k+": '"+t[k]+"',").join('\n')+'\n      }\n    }\n  }\n}\n';
    download('aperture-'+p.id+'-tailwind.config.js', js, 'text/javascript');
  } else {
    download('aperture-'+p.id+'-tokens.json', JSON.stringify({name:p.name, tokens:t}, null, 2), 'application/json');
  }
  toast('Downloaded');
}

/* ============ TYPOGRAPHY ============ */
const TYPE_PAIRINGS=[
  {id:'editorial', name:'Editorial Trust', desc:'A warm serif headline against a clean sans body — reads credible without feeling corporate. Good for healthcare and finance.',
   heading:'Fraunces', body:'Source Sans 3', googleParam:'Fraunces:ital,opsz,wght@0,9..144,600&family=Source+Sans+3:wght@400;600;700'},
  {id:'precision', name:'Precision Minimal', desc:'Geometric display paired with a neutral workhorse body. Built for dense enterprise and SaaS screens.',
   heading:'Space Grotesk', body:'Inter', googleParam:'Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600'},
  {id:'bold', name:'Bold Creative', desc:'A chunky geometric display face with an easygoing body — made for AI products that want to feel a little alive.',
   heading:'Unbounded', body:'Work Sans', googleParam:'Unbounded:wght@500;700&family=Work+Sans:wght@400;500;600'}
];
let scaleState={base:16, ratio:1.25};
const loadedFontSets=new Set();

function loadGoogleFonts(param){
  if(loadedFontSets.has(param)) return;
  const link=document.createElement('link');
  link.rel='stylesheet';
  link.href='https://fonts.googleapis.com/css2?family='+param+'&display=swap';
  document.head.appendChild(link);
  loadedFontSets.add(param);
}

function renderTypeLab(){
  const wrap=$('#typeCardsWrap'); wrap.innerHTML='';
  TYPE_PAIRINGS.forEach(tp=>{
    loadGoogleFonts(tp.googleParam);
    const card=document.createElement('div'); card.className='type-card';
    const isActive=state.activeTypeId===tp.id;
    card.innerHTML=
      '<div class="type-card-head"><div><h4>'+tp.name+(isActive?' <span class="badge-active">ACTIVE</span>':'')+'</h4><p>'+tp.desc+' — '+tp.heading+' / '+tp.body+'</p></div>'+
      '<div class="dir-actions"><button class="btn-primary" data-usetype="'+tp.id+'">Use this pairing</button></div></div>'+
      (isActive? scaleControlsHtml() : '')+
      '<div class="type-specimen" data-specimen="'+tp.id+'"></div>';
    wrap.appendChild(card);
    renderSpecimen(card.querySelector('[data-specimen]'), tp, isActive?scaleState:{base:16,ratio:1.25});
  });
  if(state.activeTypeId){
    const baseEl=$('#scaleBase'), ratioEl=$('#scaleRatio');
    if(baseEl){
      baseEl.addEventListener('input', e=>{ scaleState.base=+e.target.value; $('#scaleBaseVal').textContent=scaleState.base+'px'; refreshActiveSpecimen(); });
      ratioEl.addEventListener('change', e=>{ scaleState.ratio=+e.target.value; refreshActiveSpecimen(); });
    }
  }
}
function scaleControlsHtml(){
  return '<div class="scale-controls">'+
    '<label>Base size <input type="range" id="scaleBase" min="14" max="19" value="'+scaleState.base+'"><span id="scaleBaseVal">'+scaleState.base+'px</span></label>'+
    '<label>Scale ratio <select id="scaleRatio">'+
      ['1.125','1.2','1.25','1.333'].map(r=>'<option value="'+r+'" '+(r==String(scaleState.ratio)?'selected':'')+'>'+r+'</option>').join('')+
    '</select></label></div>';
}
function refreshActiveSpecimen(){
  const tp=TYPE_PAIRINGS.find(t=>t.id===state.activeTypeId);
  const el=document.querySelector('[data-specimen="'+tp.id+'"]');
  renderSpecimen(el, tp, scaleState);
}
function renderSpecimen(el, tp, scale){
  const base=scale.base, r=scale.ratio;
  const sizes={display:Math.round(base*Math.pow(r,4)), h1:Math.round(base*Math.pow(r,3)), h2:Math.round(base*Math.pow(r,2)), h3:Math.round(base*r), body:base, small:Math.round(base/r)};
  el.innerHTML=
    specRow('Display', tp.heading, 600, sizes.display)+
    specRow('H1', tp.heading, 600, sizes.h1)+
    specRow('H2', tp.heading, 500, sizes.h2)+
    specRow('H3', tp.heading, 500, sizes.h3, tp.body)+
    specRowBody('Body text', tp.body, sizes.body, 'The quiet part of good design is rarely the part anyone notices — it is the part that gets out of the way.')+
    specRowBody('Small / caption', tp.body, sizes.small, 'Used for metadata, timestamps and helper text.');
}
function specRow(tag, font, weight, size, h3sub){
  return '<div class="spec-row"><span class="spec-tag">'+tag+' · '+size+'px</span>'+
    '<div style="font-family:\''+font+'\',sans-serif;font-weight:'+weight+';font-size:'+size+'px;line-height:1.15">Designing with intent</div></div>';
}
function specRowBody(tag, font, size, text){
  return '<div class="spec-row"><span class="spec-tag">'+tag+' · '+size+'px</span>'+
    '<div style="font-family:\''+font+'\',sans-serif;font-size:'+size+'px;line-height:1.6;max-width:62ch;color:var(--text-dim)">'+text+'</div></div>';
}
function exportType(kind){
  const tp=TYPE_PAIRINGS.find(t=>t.id===state.activeTypeId);
  if(!tp){ toast('Pick a pairing first'); return; }
  if(kind==='css'){
    const css="@import url('https://fonts.googleapis.com/css2?family="+tp.googleParam+"&display=swap');\n\n:root {\n  --font-heading: '"+tp.heading+"', sans-serif;\n  --font-body: '"+tp.body+"', sans-serif;\n  --font-size-base: "+scaleState.base+"px;\n  --type-scale-ratio: "+scaleState.ratio+";\n}\n";
    download('aperture-'+tp.id+'-type.css', css, 'text/css');
  } else {
    download('aperture-'+tp.id+'-type.json', JSON.stringify({name:tp.name, heading:tp.heading, body:tp.body, base:scaleState.base, ratio:scaleState.ratio}, null, 2), 'application/json');
  }
  toast('Downloaded');
}

/* ============ INSIGHTS (live Medium RSS) ============ */
async function fetchInsights(tag){
  const wrap=$('#articleWrap');
  wrap.innerHTML=Array(6).fill('<div class="skeleton"></div>').join('');
  try{
    const feedUrl='https://medium.com/feed/tag/'+tag;
    const api='https://api.rss2json.com/v1/api.json?rss_url='+encodeURIComponent(feedUrl);
    const res=await fetch(api);
    if(!res.ok) throw new Error('bad response');
    const data=await res.json();
    if(data.status!=='ok' || !data.items || !data.items.length) throw new Error('empty feed');
    wrap.innerHTML='';
    data.items.slice(0,9).forEach(item=>{
      const tmp=document.createElement('div'); tmp.innerHTML=item.description||'';
      let desc=(tmp.textContent||'').trim().slice(0,140);
      if(desc.length===140) desc+='…';
      const date=item.pubDate? new Date(item.pubDate).toLocaleDateString(undefined,{month:'short',day:'numeric'}) : '';
      const card=document.createElement('a');
      card.className='article-card'; card.href=item.link; card.target='_blank'; card.rel='noopener noreferrer';
      const srcEl=document.createElement('span'); srcEl.className='src'; srcEl.textContent='MEDIUM · '+tag.toUpperCase();
      const h=document.createElement('h5'); h.textContent=item.title||'Untitled';
      const p=document.createElement('p'); p.textContent=desc;
      const m=document.createElement('span'); m.className='meta'; m.textContent=(item.author||'')+(date?' · '+date:'');
      card.appendChild(srcEl); card.appendChild(h); card.appendChild(p); card.appendChild(m);
      wrap.appendChild(card);
    });
    renderRpInsights(data.items.slice(0,4));
  }catch(err){
    wrap.innerHTML='<div class="empty-state" style="grid-column:1/-1"><strong>Couldn\'t reach the live feed</strong>This browser may be offline, or the feed source is temporarily unavailable. Nothing fabricated here — try again.<br><br><button class="btn-primary" id="retryInsights">Retry</button></div>';
    const retry=$('#retryInsights'); if(retry) retry.addEventListener('click',()=>fetchInsights(tag));
  }
}
function renderRpInsights(items){
  const wrap=$('#rpInsights');
  if(!items || !items.length){ wrap.innerHTML='<div class="empty-state">No trending items yet</div>'; return; }
  wrap.innerHTML=items.map(it=>
    '<a class="rp-insight" href="'+it.link+'" target="_blank" rel="noopener noreferrer"><span class="dotmark"></span><div><div class="ti">'+escapeHtml((it.title||'').slice(0,70))+'</div><div class="tm">Medium</div></div></a>'
  ).join('');
}

/* ============ CHAT / UX ADVISOR ENGINE ============ */
const HEURISTICS=[
  "Visibility of system status — always show what's happening.",
  "Match between system and the real world — speak the user's language.",
  "User control and freedom — give an exit and an undo.",
  "Consistency and standards — don't make people relearn.",
  "Error prevention — stop the mistake before it happens.",
  "Recognition rather than recall — surface options, don't hide them in memory.",
  "Flexibility and efficiency of use — accelerators for returning users.",
  "Aesthetic and minimalist design — every extra element competes for attention.",
  "Help users recognize, diagnose and recover from errors — plain language, no codes.",
  "Help and documentation — searchable, task-focused, easy to find."
];

function getLocalAdvice(raw){
  const msg=raw.toLowerCase();
  const name=state.name? state.name : null;

  function has(words){ return words.some(w=>msg.includes(w)); }

  if(has(['hi','hello','hey']) && msg.length<20){
    return {tag:'Greeting', text:(name?'Hey '+name+'.':'Hey there.')+' Tell me about the screen or decision you\'re working on — the more specific, the sharper the advice. You can also tap one of the chips below.'};
  }

  if(has(['nielsen','heuristic'])){
    return {tag:"Nielsen's 10 heuristics", text:HEURISTICS.map((h,i)=>(i+1)+'. '+h).join('\n')};
  }

  if(has(["fitts"])){
    return {tag:"Fitts's Law", text:"The time to reach a target depends on its distance and inversely on its size — bigger and closer targets get hit faster and with fewer errors.\n\nApply it here:\n• Primary actions should be large and close to where attention already is (e.g. end of a form, not tucked in a far corner).\n• Destructive and primary actions should never sit adjacent at the same size — a mis-click becomes expensive.\n• On touch, keep targets at least 24×24 CSS px (WCAG 2.2 SC 2.5.8), 44×44 is safer for primary actions.\n\nWant me to check a specific button or menu against this?"};
  }
  if(has(['hick'])){
    return {tag:"Hick's Law", text:"Decision time increases with the number and complexity of choices. A menu with 20 flat options is slower to scan than 5 grouped categories, even though the total option count is similar.\n\nApply it here:\n• Progressive disclosure: show 3-5 primary paths, hide the rest behind \"More\".\n• Group by user goal, not by internal system structure.\n• Defaults matter — a sensible pre-selected option removes a decision entirely.\n\nIs this for a nav menu, a settings page, or something else?"};
  }
  if(has(['miller'])){
    return {tag:"Miller's Law", text:"Working memory comfortably holds about 4±1 chunks at a time, not the old \"7±2\" myth. Chunk related fields, steps or items together rather than listing them flat.\n\nApply it here: group a 12-field form into 3 sections of 4, break a long onboarding into labeled stages, and use visual grouping (cards, dividers) so the chunk is seen, not just implied."};
  }
  if(has(['jakob'])){
    return {tag:"Jakob's Law", text:"Users spend most of their time on other products, so they bring those mental models with them. Fighting an established convention (cart icon, hamburger menu, swipe-to-delete) costs more than it earns unless you have a very strong reason to diverge.\n\nWhere are you considering breaking convention? That's usually the more interesting question than whether to follow it."};
  }

  if(has(['dark pattern'])){
    return {tag:'Dark patterns to avoid', text:"The ones that come up most in review:\n• Confirmshaming — guilt-tripping copy on a decline button (\"No, I don't want to save money\").\n• Roach motel — easy to subscribe, deliberately hard to cancel.\n• Forced continuity — free trial silently converts to paid with no reminder.\n• Sneak into basket — a pre-checked add-on at checkout.\n• Fake urgency — a countdown timer that resets on refresh.\n\nEach of these trades a short-term conversion bump for long-term trust loss and, increasingly, regulatory risk (FTC, CCPA, DSA all now name specific patterns). If you flag one in your own flow, I can help you rewrite it honestly."};
  }

  if(has(['touch target','tap target','thumb'])){
    return {tag:'Touch targets', text:"WCAG 2.2 SC 2.5.8 sets a 24×24 CSS px minimum for any target that isn't inline text, with exceptions for spacing or equivalent controls nearby. In practice, aim for 44×44 px for anything primary — it accounts for thumb imprecision and is forgiving on smaller phones. Add at least 8px of breathing room between adjacent targets so a mis-tap doesn't trigger the wrong action."};
  }

  if(has(['contrast','color blind','accessib','wcag'])){
    return {tag:'Accessibility & contrast', text:"Quick reference: WCAG AA needs 4.5:1 for normal text, 3:1 for large text (24px+ or 19px+ bold) and UI components. AAA pushes normal text to 7:1.\n\nBeyond contrast: never use color alone to carry meaning (pair an error state with an icon or label, not just red), make sure focus states are visible for keyboard users, and test with a color-blindness simulator — roughly 1 in 12 men have some form of it.\n\nOpen the Color Lab tab — the contrast checker there computes real ratios against your active palette."};
  }

  if(has(['typography','font','typeface'])){
    return {tag:'Typography', text:"Three things decide whether type reads as considered or default:\n1. Line length — keep body copy under 75-80 characters per line; longer and the eye loses its place on the return sweep.\n2. A real scale — pick a ratio (1.2 for dense UI, 1.333 for editorial) and stick to it instead of eyeballing sizes.\n3. Pairing contrast — two typefaces should either clearly differ in voice (serif vs. geometric sans) or not be paired at all; two similar grotesques just look like an inconsistency.\n\nHead to the Type Lab tab — three real pairings are loaded with live Google Fonts and an adjustable scale."};
  }
  if(has(['palette','color scheme','brand color'])){
    return {tag:'Color direction', text:"Before picking hex values, decide what the color needs to signal: trust (blues, desaturated), energy (saturated warm hues), calm (low-saturation, higher value). Then build outward from one primary and one neutral — most of a UI should be neutral, with color reserved for action and status.\n\nOpen the Color Lab tab — three directions are ready with full semantic tokens, a contrast checker, and CSS/Tailwind/JSON export."};
  }

  if(has(['onboarding','first-time','empty state','first time'])){
    return {tag:'Onboarding & empty states', text:"Problem framing: a blank state with no content and no next step reads as broken, not empty.\n\nThree approaches, ascending effort:\n• Conservative: a single sentence + one clear primary action (\"No projects yet — create your first one\").\n• Improved: add a short illustration or icon to make the emptiness feel intentional rather than an error.\n• Ambitious: pre-populate with one realistic sample item the user can edit or delete, so they learn the interface by doing instead of reading.\n\nRecommendation depends on how first-session drop-off is trending — if you have that number, I can help you pick."};
  }

  if(has(['error message','error state','validation'])){
    return {tag:'Error states', text:"Good error copy does three things: says what happened, in plain language (no codes like ERR_402 facing the user); says why if it's useful (\"card expired\" beats \"payment failed\"); and says exactly what to do next.\n\nInline validation should fire on blur, not on every keystroke — validating a half-typed email as invalid punishes the user for still typing. Reserve red for the field actually in error, not the whole form."};
  }

  if(has(['cta','button text','conversion'])){
    return {tag:'CTA & button copy', text:"A CTA should name the result of the click, in active voice — \"Save changes\" rather than \"Submit\", \"Start free trial\" rather than \"Get started\" if a trial is genuinely what happens next. Keep the vocabulary consistent through the whole flow: if the button says \"Publish\", the confirmation toast should say \"Published\", not \"Your post has been saved and is now live\"."};
  }

  if(has(['mobile','responsive','small screen'])){
    return {tag:'Mobile & responsive', text:"Design the constrained case first, not last — a layout that works at 375px and is enhanced for wider viewports tends to stay disciplined; the reverse (shrinking a desktop layout) tends to produce cramped, illegible mobile screens.\n\nWatch for: 16px minimum input font size (prevents iOS auto-zoom on focus), thumb-reachable primary actions in the bottom third of the screen, and horizontal scroll that happens by accident rather than by design."};
  }

  if(has(['dashboard','table','enterprise','admin'])){
    return {tag:'Enterprise & dashboard UX', text:"Dense data UIs fail most often on information hierarchy, not information volume. Before adding filters, ask what the three most common questions a user asks this screen are, and make those three answers visible without interaction.\n\nTables: sticky headers past ~8 rows, right-align numbers, and never rely on row color alone to flag status — pair it with an icon or label for colorblind users and for printed/exported views."};
  }

  if(has(['fintech','payment','bank','transaction'])){
    return {tag:'Fintech UX', text:"Trust is the product here as much as the feature set. Patterns that consistently help: showing running balance after every transaction preview (not just before/after), confirming irreversible transfers with the actual recipient name and amount spelled out (not just a generic \"Confirm?\"), and never auto-advancing through a payment flow without an explicit tap — surprise charges destroy trust permanently.\n\nWhat's the specific flow — transfer, onboarding, or a statement/dashboard view?"};
  }
  if(has(['healthcare','patient','clinical','hipaa'])){
    return {tag:'Healthcare UX', text:"Two constraints shape most clinical UI decisions: the user is often interrupted mid-task (design for resumability, not just completion), and errors have higher stakes than almost any other domain, so confirmation for anything that writes to a patient record is not optional.\n\nI can give you UX direction here, but I can't make a compliance determination — HIPAA and similar regulations need review from someone qualified to assess your actual data handling, not a visual design pass."};
  }
  if(has(['ecommerce','checkout','cart','e-commerce'])){
    return {tag:'E-commerce UX', text:"Checkout abandonment is usually a trust or effort problem, not a price problem. The reliable fixes: show total cost (incl. shipping/tax) before the final step, not as a reveal; offer guest checkout as the default, account creation as the upsell after; and keep the step count visible so users know how much is left."};
  }

  if(has(['problem','struggling','redesign','stuck','broken'])){
    return {tag:'Problem framing', text:"Let's structure this properly rather than jump to a fix.\n\n1. What's the problem, specifically — not \"users are confused\" but where, doing what, with what outcome?\n2. What's the likely root cause — is it a missing affordance, a mismatched mental model, or a technical constraint leaking into the UI?\n3. Who's affected and how often?\n\nGive me those three and I'll come back with three solutions (conservative / improved / innovative), their relative complexity, and a success metric for each."};
  }

  return {tag:'Aperture', text:"I can go deeper if you give me more to work with — the screen, the user goal, or the specific decision you're stuck on. In the meantime: what's the one thing you want this interface to make effortless?"};
}

/* real Claude call if user supplied their own key */
async function callClaude(messages){
  const res=await fetch('https://api.anthropic.com/v1/messages',{
    method:'POST',
    headers:{
      'content-type':'application/json',
      'x-api-key': state.apiKey,
      'anthropic-version':'2023-06-01',
      'anthropic-dangerous-direct-browser-access':'true'
    },
    body:JSON.stringify({
      model:'claude-sonnet-5',
      max_tokens:600,
      system:'You are Aperture, an expert UX design copilot. Answer with concrete, specific UX guidance grounded in usability heuristics, HCI research and accessibility guidelines (WCAG). Be concise, opinionated, and practical. Avoid generic filler.',
      messages: messages
    })
  });
  if(!res.ok) throw new Error('api error '+res.status);
  const data=await res.json();
  return (data.content && data.content[0] && data.content[0].text) || '...';
}

/* ============ CHAT UI ============ */
let activeConvo=null;
function newConvo(){
  activeConvo={ id:'c'+Date.now(), title:null, created:Date.now(), messages:[] };
}
newConvo();

function renderMessages(){
  const wrap=$('#cdMessages'); wrap.innerHTML='';
  activeConvo.messages.forEach(m=>{
    const d=document.createElement('div');
    d.className='msg '+m.role;
    if(m.role==='bot' && m.tag) d.innerHTML='<span class="tag">'+escapeHtml(m.tag)+'</span>'+escapeHtml(m.text);
    else d.textContent=m.text;
    wrap.appendChild(d);
  });
  wrap.scrollTop=wrap.scrollHeight;
}
function addTyping(){
  const wrap=$('#cdMessages');
  const d=document.createElement('div'); d.className='typing'; d.id='typingIndicator';
  d.innerHTML='<span></span><span></span><span></span>';
  wrap.appendChild(d); wrap.scrollTop=wrap.scrollHeight;
}
function removeTyping(){ const t=$('#typingIndicator'); if(t) t.remove(); }

async function sendChat(text){
  text=text.trim();
  if(!text) return;
  if(!activeConvo.title) activeConvo.title=text.slice(0,48);
  activeConvo.messages.push({role:'user', text});
  renderMessages();
  openDrawer();
  addTyping();

  const useRemote = !!state.apiKey;
  const delay = useRemote? 0 : 500+Math.random()*500;

  setTimeout(async ()=>{
    let tag=null, replyText;
    if(useRemote){
      try{
        const history=activeConvo.messages.slice(-8).map(m=>({role:m.role==='user'?'user':'assistant', content:m.text}));
        replyText = await callClaude(history);
        tag='Claude';
        setStatus(true);
      }catch(err){
        const local=getLocalAdvice(text);
        replyText = local.text+'\n\n(Live API call failed, answered with the local engine instead.)';
        tag=local.tag;
        setStatus(false);
      }
    } else {
      const local=getLocalAdvice(text);
      replyText=local.text; tag=local.tag;
    }
    removeTyping();
    activeConvo.messages.push({role:'bot', text:replyText, tag});
    renderMessages();
    persistConvo();
  }, delay);
}

function persistConvo(){
  const idx=state.conversations.findIndex(c=>c.id===activeConvo.id);
  const summary={id:activeConvo.id, title:activeConvo.title, created:activeConvo.created, messages:activeConvo.messages};
  if(idx>=0) state.conversations[idx]=summary; else state.conversations.unshift(summary);
  state.conversations=state.conversations.slice(0,40);
  saveState();
  renderOverviewStats();
  renderMemoryList();
  renderHistory();
  renderRpConvos();
}

function renderOverviewStats(){
  $('#statConvos').textContent=state.conversations.length;
  $('#statPalettes').textContent=state.paletteUses;
  $('#statFonts').textContent=state.typeUses;
}
function renderMemoryList(){
  const wrap=$('#memoryList');
  const rows=[];
  const p=getActivePalette();
  if(p) rows.push('<div class="memory-row"><div class="swatch-stack">'+['primary','secondary','accent'].map(k=>'<span style="background:'+p.tokens[k]+'"></span>').join('')+'</div>Palette set to <b style="color:var(--text);margin-left:4px">'+p.name+'</b></div>');
  const tp=TYPE_PAIRINGS.find(t=>t.id===state.activeTypeId);
  if(tp) rows.push('<div class="memory-row">🔤 Typography set to <b style="color:var(--text)">'+tp.name+'</b> ('+tp.heading+' + '+tp.body+')</div>');
  if(state.conversations[0]) rows.push('<div class="memory-row">💬 Last conversation: <b style="color:var(--text)">'+escapeHtml(state.conversations[0].title||'Untitled')+'</b></div>');
  if(!rows.length){
    wrap.innerHTML='<div class="empty-state"><strong>Nothing saved yet</strong>Generate a palette, pick a type pairing, or ask Aperture something — decisions you make will show up here.</div>';
  } else {
    wrap.innerHTML=rows.join('');
  }
}
function renderHistory(){
  const wrap=$('#historyWrap');
  if(!state.conversations.length){
    wrap.innerHTML='<div class="empty-state"><strong>No conversations yet</strong>Anything you ask Aperture is saved here, locally in this browser.</div>';
    return;
  }
  wrap.innerHTML=state.conversations.map(c=>
    '<div class="history-item" data-open="'+c.id+'"><div><div class="ht">'+escapeHtml(c.title||'Untitled')+'</div><div class="hd">'+new Date(c.created).toLocaleString()+' · '+c.messages.length+' messages</div></div>'+
    '<button data-del="'+c.id+'" aria-label="Delete"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/></svg></button></div>'
  ).join('');
}
function renderRpConvos(){
  const wrap=$('#rpConvos');
  if(!state.conversations.length){ wrap.innerHTML='<div class="empty-state">Nothing yet — ask Aperture something to start.</div>'; return; }
  wrap.innerHTML=state.conversations.slice(0,5).map(c=>
    '<div class="rp-conv" data-open="'+c.id+'"><div class="t">'+escapeHtml(c.title||'Untitled')+'</div><div class="d">'+timeAgo(c.created)+'</div></div>'
  ).join('');
}
function timeAgo(ts){
  const s=Math.floor((Date.now()-ts)/1000);
  if(s<60) return 'just now';
  if(s<3600) return Math.floor(s/60)+'m ago';
  if(s<86400) return Math.floor(s/3600)+'h ago';
  return Math.floor(s/86400)+'d ago';
}

function openConvo(id){
  const c=state.conversations.find(c=>c.id===id);
  if(!c) return;
  activeConvo={id:c.id, title:c.title, created:c.created, messages:c.messages.slice()};
  renderMessages();
  openDrawer();
}

/* ============ DRAWER ============ */
function openDrawer(){ $('#chatDrawer').classList.add('open'); $('#overlayMask').classList.add('show'); }
function closeDrawer(){ $('#chatDrawer').classList.remove('open'); $('#overlayMask').classList.remove('show'); }
function setStatus(remoteOk){
  const dot=$('#statusDot'), text=$('#statusText');
  if(state.apiKey){
    dot.style.background= remoteOk!==false ? 'var(--cyan)' : 'var(--error)';
    text.textContent= remoteOk!==false ? 'Connected to Claude' : 'Claude unreachable — using local engine';
  } else {
    dot.style.background='var(--success)';
    text.textContent='Local reasoning engine';
  }
}

/* ============ EVENTS ============ */
function wireNav(){
  $all('.nav-item').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      $all('.nav-item').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      activatePanel(btn.dataset.tab);
    });
  });
  $all('.tab').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      $all('.tab').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      activatePanel(btn.dataset.paneltab);
    });
  });
}
function activatePanel(id){
  $all('.panel').forEach(p=>p.classList.remove('active'));
  $('#panel-'+id).classList.add('active');
  $all('.nav-item').forEach(b=>b.classList.toggle('active', b.dataset.tab===id));
  $all('.tab').forEach(b=>b.classList.toggle('active', b.dataset.paneltab===id));
  if(id==='insights' && !$('#articleWrap').children.length){
    fetchInsights($('.tag-row .pill.active').dataset.tag);
  }
}

function autoGrow(el){
  el.style.height='auto';
  el.style.height=Math.min(el.scrollHeight,120)+'px';
}

function wireHero(){
  const input=$('#heroInput');
  input.addEventListener('input', ()=>autoGrow(input));
  input.addEventListener('keydown', e=>{
    if(e.key==='Enter' && !e.shiftKey){ e.preventDefault(); $('#heroSendBtn').click(); }
  });
  $('#heroSendBtn').addEventListener('click', ()=>{
    if(!input.value.trim()) return;
    sendChat(input.value);
    input.value=''; autoGrow(input);
  });
  $all('.chip-row .chip, .cd-chips .chip').forEach(c=>{
    c.addEventListener('click', ()=> sendChat(c.dataset.ask));
  });
  $all('#pillRow .pill').forEach(p=>{
    p.addEventListener('click', ()=>{
      $all('#pillRow .pill').forEach(x=>x.classList.remove('active'));
      p.classList.add('active');
      const map={audit:'Audit a screen I describe for usability issues', palette:'Help me generate a color palette',
        typepair:'Help me pick a typography pairing', law:'Explain a UX law or heuristic', refs:'Suggest reference products for inspiration'};
      input.value=map[p.dataset.prompt]||''; input.focus(); autoGrow(input);
    });
  });
  if(window.SpeechRecognition||window.webkitSpeechRecognition){
    const SR=window.SpeechRecognition||window.webkitSpeechRecognition;
    const rec=new SR(); rec.lang='en-US'; rec.interimResults=false;
    let listening=false;
    $('#micBtn').addEventListener('click', ()=>{
      if(listening) return;
      listening=true; $('#micBtn').style.color='var(--violet)';
      rec.start();
    });
    rec.onresult=e=>{ input.value+=(input.value?' ':'')+e.results[0][0].transcript; autoGrow(input); };
    rec.onend=()=>{ listening=false; $('#micBtn').style.color=''; };
    rec.onerror=()=>{ listening=false; $('#micBtn').style.color=''; toast('Voice input unavailable'); };
  } else {
    $('#micBtn').addEventListener('click', ()=>toast('Voice input not supported in this browser'));
  }
}

function wireChatDrawer(){
  $('#fabBtn').addEventListener('click', ()=> $('#chatDrawer').classList.contains('open') ? closeDrawer() : openDrawer());
  $('#closeDrawerBtn').addEventListener('click', closeDrawer);
  $('#overlayMask').addEventListener('click', closeDrawer);
  $('#settingsToggle').addEventListener('click', ()=> $('#cdSettings').classList.toggle('open'));
  $('#saveSettingsBtn').addEventListener('click', ()=>{
    state.name=$('#nameInput').value.trim();
    state.apiKey=$('#apiKeyInput').value.trim();
    saveState();
    updateGreeting();
    setStatus();
    toast('Saved');
    $('#cdSettings').classList.remove('open');
  });
  $('#clearKeyBtn').addEventListener('click', ()=>{
    state.apiKey=''; $('#apiKeyInput').value=''; saveState(); setStatus(); toast('API key cleared');
  });
  const cdInput=$('#cdInput');
  cdInput.addEventListener('input', ()=>autoGrow(cdInput));
  cdInput.addEventListener('keydown', e=>{
    if(e.key==='Enter' && !e.shiftKey){ e.preventDefault(); $('#cdSendBtn').click(); }
  });
  $('#cdSendBtn').addEventListener('click', ()=>{
    if(!cdInput.value.trim()) return;
    sendChat(cdInput.value);
    cdInput.value=''; autoGrow(cdInput);
  });
}

function wireColorLab(){
  $('#directionGrid').addEventListener('click', e=>{
    const sw=e.target.closest('.swatch');
    if(sw){ showSwatchPopover(e, sw.dataset.hex, sw.dataset.label); return; }
    const useBtn=e.target.closest('[data-use]');
    if(useBtn){
      state.activePaletteId=useBtn.dataset.use;
      state.paletteUses++;
      saveState();
      renderDirectionGrid();
      applyActivePalette();
      renderOverviewStats();
      renderMemoryList();
      toast('Palette applied');
      return;
    }
    const shuffleBtn=e.target.closest('[data-shuffle]');
    if(shuffleBtn){
      const p=PALETTES.find(p=>p.id===shuffleBtn.dataset.shuffle);
      const deg=(Math.random()>0.5?1:-1)*(15+Math.random()*25);
      ['primary','secondary','accent'].forEach(k=>{ p.tokens[k]=hueShift(p.tokens[k], deg); });
      renderDirectionGrid();
      if(state.activePaletteId===p.id) applyActivePalette();
      toast('Hue shifted');
    }
  });
  $('#contrastFg').addEventListener('change', renderContrast);
  $('#contrastBg').addEventListener('change', renderContrast);
  $('#themeToggle').addEventListener('click', e=>{
    const btn=e.target.closest('button'); if(!btn) return;
    $all('#themeToggle button').forEach(b=>b.classList.remove('on'));
    btn.classList.add('on');
    state.paletteTheme=btn.dataset.mode;
    saveState();
    applyActivePalette();
  });
  $all('[data-export]').forEach(b=> b.addEventListener('click', ()=>exportPalette(b.dataset.export)));
}

function wireTypeLab(){
  $('#typeCardsWrap').addEventListener('click', e=>{
    const btn=e.target.closest('[data-usetype]');
    if(!btn) return;
    state.activeTypeId=btn.dataset.usetype;
    state.typeUses++;
    saveState();
    renderTypeLab();
    renderOverviewStats();
    renderMemoryList();
    toast('Pairing applied');
  });
  $all('[data-texport]').forEach(b=> b.addEventListener('click', ()=>exportType(b.dataset.texport)));
}

function wireInsights(){
  $('#tagRow').addEventListener('click', e=>{
    const btn=e.target.closest('.pill'); if(!btn) return;
    $all('#tagRow .pill').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    fetchInsights(btn.dataset.tag);
  });
}

function wireHistoryAndRp(){
  document.addEventListener('click', e=>{
    const open=e.target.closest('[data-open]');
    if(open){ openConvo(open.dataset.open); return; }
    const del=e.target.closest('[data-del]');
    if(del){
      state.conversations=state.conversations.filter(c=>c.id!==del.dataset.del);
      saveState(); renderHistory(); renderRpConvos(); renderOverviewStats();
    }
  });
  $('#clearHistoryBtn').addEventListener('click', ()=>{
    if(!state.conversations.length) return;
    if(confirm('Clear all saved conversations? This cannot be undone.')){
      state.conversations=[]; saveState(); renderHistory(); renderRpConvos(); renderOverviewStats(); renderMemoryList();
      toast('History cleared');
    }
  });
}

function wireMisc(){
  $('#nameSpan').addEventListener('click', ()=>{
    const n=prompt('What should Aperture call you?', state.name||'');
    if(n!==null){ state.name=n.trim(); saveState(); updateGreeting(); }
  });
  $('#exportWorkspaceBtn').addEventListener('click', ()=>{
    const p=getActivePalette(), tp=TYPE_PAIRINGS.find(t=>t.id===state.activeTypeId);
    const bundle={
      exportedAt:new Date().toISOString(),
      palette: p? {name:p.name, tokens:p.tokens} : null,
      typography: tp? {name:tp.name, heading:tp.heading, body:tp.body, scale:scaleState} : null,
      conversationCount: state.conversations.length
    };
    download('aperture-workspace.json', JSON.stringify(bundle,null,2), 'application/json');
    toast('Workspace exported');
  });
  $('#helpBtn').addEventListener('click', ()=> sendChat('List all 10 of Nielsen\'s usability heuristics briefly'));
  $('#notifBtn').addEventListener('click', ()=> toast('No new notifications'));
}

function updateGreeting(){
  const hour=new Date().getHours();
  const period= hour<12 ? 'Good morning' : hour<18 ? 'Good afternoon' : 'Good evening';
  $('#greetingText').childNodes[0].textContent=period+', ';
  $('#nameSpan').textContent= state.name || 'there';
  if($('#nameInput')) $('#nameInput').value=state.name||'';
  if($('#apiKeyInput')) $('#apiKeyInput').value=state.apiKey||'';
}

/* ============ INIT ============ */
function init(){
  wireNav(); wireHero(); wireChatDrawer(); wireColorLab(); wireTypeLab(); wireInsights(); wireHistoryAndRp(); wireMisc();
  updateGreeting();
  renderDirectionGrid();
  if(state.activePaletteId) applyActivePalette(); else $('#activePaletteName').textContent='no palette selected — pick one below';
  renderTypeLab();
  renderOverviewStats();
  renderMemoryList();
  renderHistory();
  renderRpConvos();
  setStatus();
  fetchInsights('ux-design');

  requestAnimationFrame(()=> document.body.classList.add('app-ready'));
}
document.addEventListener('DOMContentLoaded', init);
})();
</script>
</body>
</html>
