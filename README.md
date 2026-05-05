[mapa_combinado_v7.html](https://github.com/user-attachments/files/27414485/mapa_combinado_v7.html)<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mapa Combinado v7 · Central de Ajuda + Treinamentos</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@300;400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;overflow:hidden;font-family:'IBM Plex Sans',system-ui,sans-serif;background:#f4f2ee;color:#1e1c18}

/* ── TOPBAR ── */
#topbar{
  position:fixed;top:0;left:0;right:0;height:44px;z-index:200;
  background:#fff;border-bottom:1px solid #e5e2db;
  display:flex;align-items:center;gap:0;padding:0 16px;
}
.brand{font-size:13px;font-weight:600;color:#1e1c18;letter-spacing:-.01em;white-space:nowrap}
.brand span{font-weight:300;color:#999}
.sep{width:1px;height:18px;background:#e5e2db;margin:0 12px;flex-shrink:0}
.meta{font-size:11px;color:#aaa;white-space:nowrap}
.meta strong{color:#555;font-weight:500}
.right{margin-left:auto;display:flex;align-items:center;gap:7px}

.tb-btn{
  display:flex;align-items:center;gap:5px;padding:5px 11px;border-radius:6px;
  border:1px solid #e0ddd6;background:#fff;font-size:11px;
  font-family:'IBM Plex Sans',sans-serif;color:#555;cursor:pointer;transition:all .12s;white-space:nowrap;
}
.tb-btn:hover{background:#f4f2ee;border-color:#ccc;color:#1e1c18}
.tb-btn.active{background:#1e1c18;color:#fff;border-color:#1e1c18}

#search-wrap{position:relative;display:flex;align-items:center}
#search-input{
  width:190px;padding:5px 10px 5px 28px;border:1px solid #e0ddd6;border-radius:6px;
  font-size:11px;font-family:'IBM Plex Sans',sans-serif;background:#f9f8f6;color:#1e1c18;
  outline:none;transition:all .15s;
}
#search-input:focus{width:240px;background:#fff;border-color:#aaa}
#search-input::placeholder{color:#bbb}
.search-icon{position:absolute;left:8px;color:#bbb;pointer-events:none;font-size:11px}

/* ── LAYOUT ── */
#app{position:fixed;inset:0;top:44px;display:flex}

/* ── LEGEND PANEL ── */
#legend{
  width:200px;flex-shrink:0;background:#fff;border-right:1px solid #e5e2db;
  overflow-y:auto;display:flex;flex-direction:column;
}
#legend::-webkit-scrollbar{width:3px}
#legend::-webkit-scrollbar-thumb{background:#ddd;border-radius:3px}
.leg-section{padding:10px 0 6px}
.leg-title{font-size:9px;font-weight:600;letter-spacing:.1em;text-transform:uppercase;color:#bbb;padding:0 12px 6px}
.leg-item{
  display:flex;align-items:center;gap:7px;padding:4px 12px;cursor:pointer;
  transition:background .1s;font-size:11px;color:#444;line-height:1.3;
}
.leg-item:hover{background:#f4f2ee}
.leg-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0}
.leg-divider{height:1px;background:#f0ede7;margin:4px 0}

/* ── CANVAS ── */
#canvas-wrap{
  flex:1;overflow:hidden;position:relative;cursor:grab;background:#f4f2ee;
}
#canvas-wrap.dragging{cursor:grabbing}
#canvas-wrap::before{
  content:'';position:absolute;inset:0;
  background-image:radial-gradient(circle,#d8d4cc 1px,transparent 1px);
  background-size:28px 28px;pointer-events:none;opacity:.45;
}

/* The single transform container */
#stage{position:absolute;top:0;left:0;transform-origin:0 0}

/* The main SVG for edges+connections */
#main-svg{position:absolute;top:0;left:0;overflow:visible;pointer-events:none}

/* Map labels */
.map-label{
  position:absolute;font-size:11px;font-weight:700;letter-spacing:.12em;text-transform:uppercase;
  color:#555;white-space:nowrap;pointer-events:none;
}

/* ── ZOOM ── */
#zoom-controls{
  position:absolute;bottom:18px;right:18px;display:flex;flex-direction:column;gap:4px;z-index:10;
}
.zoom-btn{
  width:28px;height:28px;border-radius:6px;border:1px solid #e0ddd6;background:#fff;
  color:#555;font-size:15px;cursor:pointer;display:flex;align-items:center;justify-content:center;
  transition:all .12s;box-shadow:0 1px 4px rgba(0,0,0,.06);
}
.zoom-btn:hover{background:#f4f2ee;color:#1e1c18}
#zoom-pct{text-align:center;font-size:9px;font-family:'IBM Plex Mono',monospace;color:#aaa;padding:1px 0}

/* ── LEGEND TOGGLE ── */
#conn-toggle{display:flex;align-items:center;gap:5px;padding:4px 11px}
#conn-toggle input{cursor:pointer}
#conn-toggle label{font-size:11px;color:#555;cursor:pointer}

/* ── SIDEBAR ── */
#sidebar{
  position:absolute;right:0;top:0;bottom:0;width:290px;background:#fff;
  border-left:1px solid #e5e2db;display:flex;flex-direction:column;z-index:50;
  transform:translateX(100%);transition:transform .22s cubic-bezier(.4,0,.2,1);
}
#sidebar.open{transform:translateX(0)}
#sb-head{padding:14px;border-bottom:1px solid #eee;flex-shrink:0;position:relative}
#sb-tag{font-size:9px;font-weight:600;letter-spacing:.1em;text-transform:uppercase;color:#bbb;margin-bottom:4px}
#sb-name{font-size:13px;font-weight:600;line-height:1.3;color:#1e1c18;padding-right:24px}
#sb-count{font-size:10px;color:#aaa;margin-top:3px;font-family:'IBM Plex Mono',monospace}
#sb-renamed-box{
  margin-top:8px;padding:7px 9px;background:#fffbe6;border:1px solid #f0d060;
  border-radius:6px;font-size:10.5px;color:#7a6000;display:none;
}
#sb-renamed-box .old-name{display:block;text-decoration:line-through;color:#b09030;margin-bottom:2px;font-size:10px}
#sb-links{
  margin-top:8px;padding:7px 9px;background:#eef6ff;border:1px solid #b8d8f8;
  border-radius:6px;font-size:10.5px;color:#1a4a7a;display:none;
}
#sb-links .link-title{font-weight:600;margin-bottom:4px;font-size:10px;text-transform:uppercase;letter-spacing:.06em}
.link-mod{display:flex;align-items:center;gap:6px;padding:3px 0;font-size:11px}
.link-mod-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0}
#sb-close{
  position:absolute;top:10px;right:10px;width:22px;height:22px;border-radius:5px;
  border:1px solid #e5e2db;background:#f9f8f6;cursor:pointer;font-size:13px;color:#888;
  display:flex;align-items:center;justify-content:center;transition:all .12s;
}
#sb-close:hover{background:#eee;color:#333}
#sb-body{flex:1;overflow-y:auto;padding:10px 14px 14px}
#sb-body::-webkit-scrollbar{width:3px}
#sb-body::-webkit-scrollbar-thumb{background:#ddd;border-radius:3px}
.art-row{
  display:flex;gap:7px;padding:5px 0;border-bottom:1px solid #f0ede7;
  font-size:11px;color:#444;line-height:1.4;align-items:flex-start;
}
.art-row:last-child{border-bottom:none}
.art-n{font-family:'IBM Plex Mono',monospace;font-size:9px;color:#ccc;flex-shrink:0;padding-top:2px;min-width:18px}

/* ── SEARCH RESULTS ── */
#search-results{
  position:absolute;top:44px;left:200px;right:0;background:#fff;
  border:1px solid #e5e2db;border-top:none;max-height:260px;overflow-y:auto;
  z-index:300;display:none;box-shadow:0 4px 16px rgba(0,0,0,.08);
}
.sr-item{padding:7px 12px;cursor:pointer;font-size:11px;color:#444;border-bottom:1px solid #f4f2ee}
.sr-item:hover{background:#f4f2ee}
.sr-name{font-weight:500;color:#1e1c18}
.sr-cat{font-size:10px;color:#aaa}
.sr-art{font-size:10px;color:#888;font-style:italic;margin-top:1px}
/* ── COMMENTS PANEL ── */
#comments-panel{
  position:fixed;top:44px;left:200px;width:340px;bottom:0;background:#fff;
  border-right:1px solid #e5e2db;z-index:140;overflow:hidden;display:none;
  flex-direction:column;box-shadow:2px 0 16px rgba(0,0,0,.08);
}
#comments-panel.open{display:flex}
#cp-head{padding:12px 14px;border-bottom:1px solid #eee;flex-shrink:0;display:flex;align-items:center;gap:8px}
#cp-title{font-size:13px;font-weight:700;flex:1;color:#1e1c18}
#cp-context{font-size:10px;color:#888;padding:6px 14px 4px;background:#f9f8f6;border-bottom:1px solid #eee;flex-shrink:0;min-height:28px}
#cp-list{flex:1;overflow-y:auto;padding:10px 12px;display:flex;flex-direction:column;gap:10px}
#cp-list::-webkit-scrollbar{width:3px}
#cp-list::-webkit-scrollbar-thumb{background:#ddd;border-radius:3px}
#cp-form{flex-shrink:0;border-top:1px solid #eee;padding:10px 12px;background:#fafaf8}
.comment-card{background:#f9f8f6;border:1px solid #ede9e2;border-radius:8px;padding:9px 11px}
.comment-card.pinned{border-left:3px solid #f5c842;background:#fffbe6}
.cc-header{display:flex;align-items:center;gap:6px;margin-bottom:5px}
.cc-avatar{width:22px;height:22px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;color:#fff;flex-shrink:0}
.cc-author{font-size:11px;font-weight:600;color:#1e1c18;flex:1}
.cc-date{font-size:9px;color:#bbb;font-family:'IBM Plex Mono',monospace}
.cc-node{font-size:9px;color:#888;background:#f0ede7;border-radius:3px;padding:1px 5px;margin-bottom:4px;display:inline-block}
.cc-text{font-size:11.5px;color:#333;line-height:1.6;white-space:pre-wrap}
.cc-actions{display:flex;gap:6px;margin-top:6px}
.cc-pin-btn{font-size:9px;color:#bbb;border:none;background:none;cursor:pointer;padding:0}
.cc-pin-btn:hover{color:#f5c842}
.cc-pin-btn.active{color:#f5c842;font-weight:600}
.cc-del-btn{font-size:9px;color:#ddd;border:none;background:none;cursor:pointer;padding:0;margin-left:auto}
.cc-del-btn:hover{color:#e55}
#cp-form textarea{
  width:100%;border:1px solid #e0ddd6;border-radius:6px;padding:7px 9px;
  font-size:11px;font-family:'IBM Plex Sans',sans-serif;color:#1e1c18;
  resize:none;outline:none;background:#fff;line-height:1.5;
  transition:border-color .15s;
}
#cp-form textarea:focus{border-color:#aaa}
#cp-form-row{display:flex;align-items:center;gap:6px;margin-top:6px}
#cp-author-input{
  flex:1;border:1px solid #e0ddd6;border-radius:5px;padding:5px 8px;
  font-size:11px;font-family:'IBM Plex Sans',sans-serif;outline:none;background:#f9f8f6;
}
#cp-author-input:focus{background:#fff;border-color:#aaa}
#cp-send-btn{
  padding:5px 12px;border-radius:5px;border:none;background:#1e1c18;color:#fff;
  font-size:11px;font-family:'IBM Plex Sans',sans-serif;cursor:pointer;white-space:nowrap;
  transition:background .12s;
}
#cp-send-btn:hover{background:#333}
.node-comment-dot{
  position:absolute;top:-4px;right:-4px;width:14px;height:14px;border-radius:50%;
  background:#f5c842;border:2px solid #fff;display:flex;align-items:center;justify-content:center;
  font-size:7px;font-weight:700;color:#1e1c18;pointer-events:none;z-index:10;
}
</style>
</head>
<body>

<div id="topbar">
  <div class="brand">Mapa Combinado <span>v7</span></div>
  <div class="sep"></div>
  <div class="meta">
    <strong>Central de Ajuda</strong> + <strong>Treinamentos</strong> · Conta Azul
    &nbsp;·&nbsp; <span id="total-label">— seções · — módulos</span>
  </div>
  <div class="right">
    <div id="conn-toggle">
      <input type="checkbox" id="show-connections" checked>
      <label for="show-connections">Conexões</label>
    </div>
    <div class="sep"></div>
    <div style="display:flex;align-items:center;gap:2px;background:#f4f2ee;border-radius:6px;padding:2px">
      <button class="tb-btn active" id="btn-after" style="font-size:10px;padding:4px 10px" onclick="switchTab('after',this)">✦ Agora</button>
      <button class="tb-btn" id="btn-antes" style="font-size:10px;padding:4px 10px" onclick="switchTab('antes',this)">◌ Antes</button>
    </div>
    <div class="sep"></div>
    <button class="tb-btn" id="btn-validacao" onclick="toggleValidacao()" style="font-size:10px">✅ Validação</button>
    <button class="tb-btn" id="btn-comments" onclick="toggleComments()" style="font-size:10px">💬 Comentários</button>
    <div class="sep"></div>
    <button class="tb-btn" id="btn-expand">⊞ Expandir</button>
    <button class="tb-btn" id="btn-collapse">⊟ Recolher</button>
    <button class="tb-btn" id="btn-reset">↺ Reset</button>
    <button class="tb-btn" id="btn-fit">⊡ Fit</button>
    <div class="sep"></div>
    <button class="tb-btn" id="btn-sync" onclick="syncData()" style="font-size:10px" title="Recarregar dados compartilhados">🔄 Sync</button>
    <div id="search-wrap">
      <span class="search-icon">🔍</span>
      <input type="text" id="search-input" placeholder="Buscar artigo ou seção…">
    </div>
  </div>
</div>

<!-- BEFORE MODE BANNER -->
<div id="before-banner" style="display:none;position:fixed;top:44px;left:0;right:0;z-index:90;background:#6b5a3a;color:#fff;font-size:11px;font-weight:500;padding:5px 20px;text-align:center;letter-spacing:.04em">
  ◌ ESTRUTURA ANTERIOR — M5 Despesas e M14 Projetos não existiam &nbsp;·&nbsp; M2 sem segmentação por tipo de negócio &nbsp;·&nbsp; Despesas misturadas com Compras
</div>

<!-- VALIDATION PANEL -->
<div id="validacao-panel" style="display:none;position:fixed;top:44px;right:0;width:360px;bottom:0;background:#fff;border-left:1px solid #e5e2db;z-index:150;overflow-y:auto;box-shadow:-4px 0 16px rgba(0,0,0,.1)">
  <div style="padding:14px 16px;border-bottom:1px solid #eee;display:flex;align-items:center;gap:10px">
    <span style="font-size:14px;font-weight:700;flex:1">✅ Checklist de Validação</span>
    <button onclick="toggleValidacao()" style="border:none;background:#f0ede7;border-radius:5px;padding:3px 9px;cursor:pointer;font-size:12px">✕ Fechar</button>
  </div>
  <div style="padding:6px 12px 4px;background:#f9f8f6;border-bottom:1px solid #eee;font-size:10px;color:#888">Itens alinhados na agenda de quinta-feira · Bruna Schmitz · 5 mai 2026</div>
  <div id="val-list" style="padding:10px 12px;display:flex;flex-direction:column;gap:8px"></div>
</div>

<div id="app">
  <div id="legend">
    <div class="leg-section">
      <div class="leg-title">Central de Ajuda</div>
      <div id="leg-ca"></div>
    </div>
    <div class="leg-divider"></div>
    <div class="leg-section">
      <div class="leg-title">Treinamentos</div>
      <div id="leg-tr"></div>
    </div>
    <div class="leg-divider"></div>
    <div class="leg-section" style="padding:10px 12px 8px">
      <div class="leg-title" style="padding:0 0 8px">Legenda</div>
      <div style="display:flex;flex-direction:column;gap:6px;font-size:10.5px;color:#555">
        <div style="display:flex;align-items:center;gap:6px"><span style="font-size:12px">⭐</span><span><strong>NOVO</strong> — aula ou módulo novo</span></div>
        <div style="display:flex;align-items:center;gap:6px"><span style="font-size:12px">🟣</span><span><strong>CONCEITO</strong> — aula intro</span></div>
        <div style="display:flex;align-items:center;gap:6px"><span style="font-size:12px">🔥</span><span><strong>DESTAQUE</strong> — roteiro</span></div>
        <div style="display:flex;align-items:center;gap:6px"><span style="font-size:11px;background:#f5c842;border-radius:50%;width:16px;height:16px;display:inline-flex;align-items:center;justify-content:center;flex-shrink:0">★</span><span>Seção renomeada</span></div>
        <div style="display:flex;align-items:center;gap:6px"><span style="display:inline-block;width:6px;height:6px;border-radius:50%;background:#3b82f6;flex-shrink:0"></span><span>Tem conexão CA → Treino</span></div>
        <div style="display:flex;align-items:center;gap:6px"><div style="width:20px;height:0;border-top:2px dashed #3b82f6;opacity:.7"></div><span>Conexão CA → Treinamento</span></div>
        <div style="display:flex;align-items:center;gap:6px"><div style="width:20px;height:0;border-top:2px dashed #7a756e;opacity:.9"></div><span>Aula reaproveitada (Antes→Agora)</span></div>
        <div style="margin-top:4px;padding-top:6px;border-top:1px solid #f0ede7;font-size:10px;color:#888">Antes/Depois: use o toggle no topo</div>
        <div style="margin-top:4px;font-size:10px;color:#888">✥ Arraste nós para reposicionar</div>
        <div style="margin-top:4px;font-size:10px;color:#888">⊟⊞ Comparar: visão lado a lado</div>
        <div style="margin-top:4px;padding-top:6px;border-top:1px solid #f0ede7">
          <div style="display:flex;align-items:center;gap:6px;font-size:10px;color:#888">
            <span style="background:#f5c842;border-radius:50%;width:14px;height:14px;display:inline-flex;align-items:center;justify-content:center;font-size:8px;font-weight:700;color:#1e1c18;flex-shrink:0">1</span>
            <span>Nó tem comentários</span>
          </div>
          <div style="font-size:10px;color:#aaa;margin-top:3px">💬 Clique num nó e abra "Comentários" para comentar. Visível para todos.</div>
        </div>
      </div>
    </div>
  </div>

  <div id="canvas-wrap">
    <!-- LABELS BAR — fixed screen coords, not affected by zoom/pan -->
    <div id="map-labels-bar" style="position:absolute;top:0;left:0;right:0;height:36px;z-index:15;pointer-events:none;overflow:hidden;"></div>
    <div id="stage">
      <svg id="main-svg"></svg>
      <div id="before-container"></div>
      <div id="ca-container"></div>
      <div id="tr-container"></div>
      <div id="ca-before-container"></div>
    </div>
    <!-- SPLIT VIEW PANEL -->
    <div id="split-panel" style="display:none;position:absolute;inset:0;overflow:hidden;background:#f4f2ee">
      <div style="position:absolute;top:0;left:0;right:0;height:28px;display:flex;align-items:stretch;z-index:20;border-bottom:1px solid #e0ddd6">
        <div style="flex:1;display:flex;align-items:center;justify-content:center;gap:6px;background:#6b5a3a;color:#fff;font-size:10px;font-weight:600;letter-spacing:.06em">◌ ANTES</div>
        <div style="width:1px;background:#ccc"></div>
        <div style="flex:1;display:flex;align-items:center;justify-content:center;gap:6px;background:#1a5a7a;color:#fff;font-size:10px;font-weight:600;letter-spacing:.06em">✦ AGORA</div>
      </div>
      <div style="position:absolute;top:28px;bottom:0;left:0;right:0;display:flex">
        <div id="split-left" style="flex:1;overflow:auto;border-right:2px dashed #ccc;position:relative">
          <iframe id="split-before-frame" style="width:100%;height:100%;border:none;background:#f4f2ee" src="about:blank"></iframe>
        </div>
        <div id="split-right" style="flex:1;overflow:auto;position:relative">
          <iframe id="split-after-frame" style="width:100%;height:100%;border:none;background:#f4f2ee" src="about:blank"></iframe>
        </div>
      </div>
      <div style="position:absolute;top:38px;left:50%;transform:translateX(-50%);z-index:30;background:#fff;border:1px solid #e0ddd6;border-radius:20px;padding:3px 12px;font-size:10px;color:#888;white-space:nowrap;box-shadow:0 2px 8px rgba(0,0,0,.08)">← Arraste para navegar em cada painel →</div>
    </div>
    <!-- DRAG HINT -->
    <div id="drag-hint" style="position:absolute;bottom:56px;left:50%;transform:translateX(-50%);background:rgba(30,28,24,0.75);color:#fff;font-size:10px;border-radius:16px;padding:4px 12px;pointer-events:none;opacity:0;transition:opacity .3s;white-space:nowrap">✥ Arraste nós para reposicionar</div>
    <div id="search-results"></div>
    <div id="zoom-controls">
      <button class="zoom-btn" id="btn-zin">+</button>
      <div id="zoom-pct">100%</div>
      <button class="zoom-btn" id="btn-zout">−</button>
    </div>
  </div>

  <!-- COMMENTS PANEL -->
  <div id="comments-panel">
    <div id="cp-head">
      <span style="font-size:16px">💬</span>
      <span id="cp-title">Comentários</span>
      <span id="cp-count" style="font-size:10px;background:#f0ede7;border-radius:10px;padding:1px 7px;color:#666;font-family:'IBM Plex Mono',monospace">0</span>
      <button onclick="toggleComments()" style="border:none;background:#f0ede7;border-radius:5px;padding:3px 9px;cursor:pointer;font-size:12px;margin-left:4px">✕</button>
    </div>
    <div id="cp-context">Selecione um nó no mapa para comentar nele, ou adicione um comentário geral abaixo.<br><span style="font-size:9.5px;color:#bbb">💡 Botão direito em seções do Antes para comentar</span></div>
    <div id="cp-list"></div>
    <div id="cp-form">
      <textarea id="cp-text" rows="3" placeholder="Escreva um comentário… (visível para todos)"></textarea>
      <div id="cp-form-row">
        <input id="cp-author-input" type="text" placeholder="Seu nome" maxlength="30">
        <button id="cp-send-btn" onclick="addComment()">Enviar 💬</button>
      </div>
    </div>
  </div>

  <div id="sidebar">
    <div id="sb-head">
      <button id="sb-close">×</button>
      <div id="sb-tag"></div>
      <div id="sb-name"></div>
      <div id="sb-count"></div>
      <div id="sb-renamed-box">
        <span class="old-name" id="sb-old-name"></span>
        <span>★ Renomeada na nova estrutura</span>
      </div>
      <div id="sb-links">
        <div class="link-title">🔗 Módulos relacionados</div>
        <div id="sb-links-list"></div>
      </div>
    </div>
    <div id="sb-body"></div>
  </div>
</div>

<script>
// ═══════════════════════════════════════════════════════
// RENAMED MAP
// ═══════════════════════════════════════════════════════
const RENAMED = {
  "Aprender a usar a plataforma":"Aprender a utilizar a Conta Azul Pro",
  "Vincular o certificado digital":"Vincular meu certificado digital",
  "Agilizar correções e segurança das cobranças":"Agilizar correções e garantir a segurança das Cobranças da Conta Azul",
  "Acompanhar relatórios de compras e estoque":"Acompanhar relatórios de compras",
  "Analisar dashboards de carteira de clientes":"Analisar a saúde financeira da carteira de clientes com os dashboards",
  "Buscar e transmitir notas fiscais automaticamente":"Automatizar a busca e transmissão de notas fiscais",
  "Bater saldo com sugestões da IA":"Bater saldo (com sugestões da IA)",
  "Configurar e emitir cupom fiscal (NFC-e)":"Configurar emissão de cupom fiscal (NFC-e)",
  "Configurar cobranças via Pix, Boleto ou Link de cartão":"Configurar o envio de cobranças via Pix, Boleto ou Link de cartão da Conta PJ",
  "Conectar e automatizar envio ao contador":"Conectar e automatizar envio de documentos ao contador",
  "Consultar taxas e prazos das Cobranças":"Consultar taxas e prazos das Cobranças da Conta Azul",
  "Criar relatórios personalizáveis com IA":"Criar relatórios personalizáveis Inteligência Artificial",
  "Encontrar um contador parceiro Conta Azul":"Encontrar um contador ou BPO financeiro parceiro Conta Azul",
  "Explorar o painel e treinamentos para parceiros":"Explorar o painel e treinamentos iniciais",
  "Integrar a Conta Azul Mais com sistema contábil":"Integrar a Conta Azul Mais com seu sistema contábil",
  "Monitorar entrada e saída de estoque":"Monitorar entrada e saída de produtos",
  "Organizar ordens de serviço e orçamentos":"Organizar ordem de serviços e orçamentos",
  "Realizar backups e download de anexos":"Realizar backups e download de anexos dos clientes",
  "Conhecer a estrutura e níveis da parceria":"Conhecer a estrutura e os níveis da parceria",
  "Organizar meus cadastros (Clientes, Fornecedores, Serviços)":"Organizar meus cadastros",
  "Lançar despesas fixas, recorrentes e parceladas":"Lançar despesas fixas",
};

// ═══════════════════════════════════════════════════════
// TREINAMENTOS DATA
// ═══════════════════════════════════════════════════════
const MODS = [
  {id:"intro",num:"Intro",name:"Introdução",col:"#27500A",bg:"#EAF3DE",lessons:[
    {n:"Quem é o cliente Conta Azul"},{n:"Qualidade"},{n:"Canais e ferramentas"},{n:"Zendesk"}
  ]},
  {id:"m1",num:"M1",name:"Configurações",col:"#0C447C",bg:"#E6F1FB",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Introdução e cadastro de dados"},{n:"Controle de acesso e segurança"},{n:"Plano e backup"},{n:"Cadastros"}
  ]},
  {id:"m2",num:"M2",name:"Controle de vendas e recebimento",col:"#3C3489",bg:"#EEEDFE",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Serviço recorrente",nw:true},{n:"Serviço avulso",nw:true},{n:"Empresa para Empresa",nw:true},{n:"Empresa para Consumidor",nw:true},{n:"E-commerce",nw:true},{n:"Controle de vendas e recebimento"},{n:"Consulta Serasa"},{n:"Frente de caixa"},{n:"Config. recebimento (Terceiros e CPJ)"}
  ]},
  {id:"m3",num:"M3",name:"Emissão e gerenciamento de NF",col:"#085041",bg:"#E1F5EE",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Certificado digital e config. geral de NF"},{n:"Nota fiscal de produto"},{n:"Conferência de regras fiscais"},{n:"Nota fiscal de serviço"},{n:"Nota fiscal de consumidor"},{n:"Busca automática"}
  ]},
  {id:"m4",num:"M4",name:"Controle de compras e importação",col:"#633806",bg:"#FAEEDA",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Registro de compras"},{n:"Importação de NF de compra + impacto financeiro/estoque",nw:true},{n:"Nota fiscal de importação"}
  ]},
  {id:"m5",num:"M5",name:"Despesas",col:"#501313",bg:"#FCEBEB",nw:true,lessons:[
    {n:"Introdução e conceito",c:true},{n:"Fluxo de despesas",d:true,nw:true},{n:"Registro e categorização de despesas",nw:true},{n:"Despesas fixas e recorrentes",nw:true},{n:"Controle e análise de despesas",nw:true}
  ]},
  {id:"m6",num:"M6",name:"Conta AI Captura (IA)",col:"#4B1528",bg:"#FBEAF0",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Config. e usabilidade da IA Captura"}
  ]},
  {id:"m7",num:"M7",name:"Pagamentos e cobranças (Conta PJ)",col:"#4B1528",bg:"#FBEAF0",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Ativação, configuração e uso no dia a dia"}
  ]},
  {id:"m8",num:"M8",name:"Conciliação bancária",col:"#4A1B0C",bg:"#FAECE7",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Cadastro de contas financeiras e integração"},{n:"Conciliação"},{n:"Conciliação cartão de crédito"}
  ]},
  {id:"m9",num:"M9",name:"Organização financeira",col:"#4A1B0C",bg:"#FAECE7",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Cadastros financeiros"},{n:"Contas a pagar/receber"},{n:"Análise do financeiro"},{n:"Inadimplência"}
  ]},
  {id:"m10",num:"M10",name:"Antecipação de parcelas",col:"#4A1B0C",bg:"#FAECE7",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Gestão e antecipação"}
  ]},
  {id:"m11",num:"M11",name:"Gestão de estoque",col:"#042C53",bg:"#E6F1FB",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Produtos, kit e variações"},{n:"Controle de saldos"}
  ]},
  {id:"m12",num:"M12",name:"Relatórios e dashboards",col:"#2C2C2A",bg:"#F1EFE8",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Diferença entre telas financeiras"},{n:"Relatórios"}
  ]},
  {id:"m13",num:"M13",name:"Integração com outros sistemas",col:"#173404",bg:"#EAF3DE",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Integrações com outros sistemas"}
  ]},
  {id:"m14",num:"M14",name:"Projetos",col:"#4B1528",bg:"#FBEAF0",nw:true,lessons:[
    {n:"Introdução e conceito",c:true},{n:"O que são projetos na Conta Azul",nw:true},{n:"Criação e configuração de projetos",nw:true},{n:"Lançamentos e tarefas por projeto",nw:true},{n:"Relatório e acompanhamento de projetos",nw:true}
  ]},
  {id:"m15",num:"M15",name:"Conexão com o contador",col:"#412402",bg:"#FAEEDA",lessons:[
    {n:"Introdução e conceito",c:true},{n:"Programa de parceria"},{n:"Conexão e envio de dados"},{n:"Conta Azul Mais — painel do contador"},{n:"Exportação contábil"},{n:"Integração com a Domínio"}
  ]},
];

// ═══════════════════════════════════════════════════════
// CONNECTIONS: CA section name → Training module IDs
// ═══════════════════════════════════════════════════════
const CONNECTIONS = {
  // p1 - Começar
  "Configurar os dados iniciais da empresa": ["m1"],
  "Controlar usuários e acessos": ["m1"],
  "Vincular o certificado digital": ["m1","m3"],
  "Proteger meus dados e manter a conexão": ["m1"],
  "Aprender a usar a plataforma": ["intro","m1"],
  "Simplificar a gestão com o app Conta Azul de Bolso": ["m1"],
  "Administrar minha assinatura": ["m1"],
  // p2 - Implantar
  "🔄 Serviço recorrente": ["m2"],
  "🧾 Serviço avulso": ["m2"],
  "🏭 Empresa para Empresa": ["m2","m3","m4"],
  "🏪 Empresa para Consumidor": ["m2","m3"],
  "🛒 E-commerce": ["m2","m13"],
  // p3 - Vendas
  "Registrar e controlar vendas": ["m2"],
  "Consultar guias rápidos de vendas e recebimentos": ["m2"],
  "Gerenciar a venda de serviços": ["m2"],
  "Gerenciar a venda de produtos": ["m2","m4"],
  "Organizar ordens de serviço e orçamentos": ["m2"],
  "Acompanhar contratos recorrentes": ["m2"],
  "Configurar cobranças via Pix, Boleto ou Link de cartão": ["m2","m7"],
  "Emitir boletos por outros bancos": ["m2"],
  "Consultar taxas e prazos das Cobranças": ["m2","m7"],
  "Agilizar correções e segurança das cobranças": ["m2","m7"],
  "Controlar parcelas a receber de clientes": ["m2","m9"],
  "Confirmar recebimentos e pagamentos": ["m2","m8"],
  "Automatizar a gestão de inadimplência": ["m2","m9"],
  "Realizar vendas no PDV": ["m2"],
  "Utilizar o frente de caixa (PDV)": ["m2"],
  // p4 - NF
  "Ajustar os pré-requisitos de emissão": ["m3"],
  "Entender termos e siglas fiscais": ["m3"],
  "Conhecer notas técnicas fiscais": ["m3"],
  "Preencher nota fiscal de serviço (NFS-e)": ["m3"],
  "Gerenciar notas de serviço emitidas": ["m3"],
  "Consultar regras de emissão por cidade (NFS-e)": ["m3"],
  "Preencher nota fiscal de produto (NF-e)": ["m3","m4"],
  "Gerar emissão da nota de produto": ["m3"],
  "Emitir nota fiscal de devolução": ["m3"],
  "Importar XML manualmente": ["m3","m4"],
  "Configurar e emitir cupom fiscal (NFC-e)": ["m3"],
  "Corrigir, cancelar ou devolver nota fiscal": ["m3"],
  "Solucionar rejeições de NFS-e": ["m3"],
  "Solucionar rejeições de NF-e": ["m3"],
  "Solucionar rejeições de NFC-e": ["m3"],
  "Buscar e transmitir notas fiscais automaticamente": ["m3","m6"],
  // p5 - Despesas
  "Lançar despesas fixas, recorrentes e parceladas": ["m5","m9"],
  "Lançar contas a pagar e a receber": ["m5","m9"],
  "Controlar contas a pagar e a receber": ["m5","m9"],
  "Controlar gastos e pagamentos de cartão de crédito": ["m5","m8"],
  "Filtrar buscas de lançamentos financeiros": ["m5","m9"],
  "Esclarecer dúvidas gerais do financeiro": ["m9"],
  "Configurar categorias e centros de custo": ["m5","m9"],
  "Auditar lançamentos realizados por usuários": ["m9"],
  // p6 - AI Captura
  "Agilizar o recebimento de documentos com IA": ["m6"],
  "Otimizar o uso da Conta AI Captura": ["m6"],
  "Habilitar ferramentas à Conta AI Captura": ["m6"],
  "Controlar o fluxo de documentos enviados": ["m6","m15"],
  // p7 - Conta PJ
  "Ativar minha Conta PJ Conta Azul": ["m7"],
  "Conhecer a Conta PJ da Conta Azul": ["m7"],
  "Analisar extrato da Conta PJ Conta Azul": ["m7","m8"],
  "Pagar boletos, guias e transferências pelo ERP": ["m7"],
  "Automatizar pagamentos com a Conta PJ": ["m7"],
  "Importar boletos via DDA": ["m7","m8"],
  "Controlar aprovações e multiusuários de pagamento": ["m7"],
  // p8 - Conciliação
  "Entender a conciliação bancária": ["m8"],
  "Iniciar a conciliação bancária": ["m8"],
  "Fazer conciliação bancária completa": ["m8"],
  "Identificar tipos de lançamentos na conciliação": ["m8"],
  "Habilitar integração bancária automática": ["m8"],
  "Integrar contas através do Open Finance": ["m8"],
  "Importar extrato bancário manualmente (OFX, CSV e XLS)": ["m8"],
  "Entender a integração e gestão de contas financeiras": ["m8","m9"],
  "Corrigir o saldo na conciliação": ["m8"],
  "Corrigir a integração automática": ["m8"],
  "Bater saldo com sugestões da IA": ["m8","m6"],
  "Entender o Extrato de movimentações": ["m8","m9"],
  // p9 - Financeiro
  "Organizar contas financeiras da minha empresa": ["m9","m8"],
  "Entender a Visão de competência vs caixa": ["m9","m12"],
  "Esclarecer dúvidas gerais do financeiro": ["m9"],
  "Organizar meus cadastros (Clientes, Fornecedores, Serviços)": ["m1","m9"],
  // p10 - Antecipação
  "Descobrir como funciona a antecipação": ["m10"],
  "Solicitar a antecipação de recebíveis": ["m10"],
  "Monitorar limites e pagamentos da antecipação": ["m10"],
  // p11 - Estoque
  "Lançar e controlar compras de produto": ["m4","m11"],
  "Cadastrar e gerenciar produtos": ["m11","m4"],
  "Monitorar entrada e saída de estoque": ["m11"],
  "Gerenciar movimentação de estoque": ["m11"],
  "Gerenciar kits e variações de produtos": ["m11"],
  "Manter o inventário atualizado": ["m11"],
  "Acompanhar relatórios de compras e estoque": ["m11","m12"],
  "Realizar backups e download de anexos": ["m1"],
  // p12 - Relatórios
  "Acompanhar fluxo de caixa": ["m12","m9"],
  "Analisar DRE: receitas, custos e despesas": ["m12"],
  "Monitorar a saúde financeira": ["m12"],
  "Esclarecer dúvidas frequentes de relatórios": ["m12"],
  "Explorar relatórios gerenciais": ["m12"],
  "Gerenciar o desempenho comercial": ["m12","m2"],
  "Criar relatórios personalizáveis com IA": ["m12","m6"],
  "Analisar dashboards de carteira de clientes": ["m12","m2"],
  "Visualizar e organizar a base de clientes": ["m12","m2"],
  // p13 - API
  "Conhecer aplicativos integrados": ["m13"],
  "Consultar a documentação da API": ["m13"],
  // p14 - Contador
  "Conectar e automatizar envio ao contador": ["m15"],
  "Iniciar a jornada de conexão com o Domínio": ["m15"],
  "Realizar a exportação e fechamento contábil": ["m15"],
  "Integrar a Conta Azul Mais com sistema contábil": ["m15"],
  "Transmitir lançamentos e documentos fiscais": ["m15"],
  "Monitorar histórico e status das transmissões": ["m15"],
  "Conectar e vincular empresas à Conta Azul Mais": ["m15"],
  "Explorar o painel e treinamentos para parceiros": ["m15"],
  "Conhecer a estrutura e níveis da parceria": ["m15"],
  "Resgatar benefícios e gerenciar recompensas": ["m15"],
  "Conectar clientes à plataforma e ganhar pontos": ["m15"],
  "Controlar assinaturas e lote do Plano Exclusivo": ["m15"],
  "Encontrar um contador parceiro Conta Azul": ["m15"],
  // p15 - Conta Azul Mais (all connect to m15)
  "Começar a jornada na Conta Azul Mais": ["m15"],
  "Potencializar as vantagens do Programa de Parceria": ["m15"],
  "Gerenciar a carteira de clientes": ["m15"],
  "Conectar Conta Azul Mais e Domínio": ["m15"],
  "Organizar as rotinas contábeis e fiscais": ["m15"],
  "Otimizar a produtividade com dashboards e IA": ["m15"],
  "Explorar as melhorias e suporte especializado": ["m15"],
};

// ═══════════════════════════════════════════════════════
// CENTRAL DE AJUDA DATA (inline – full from source)
// ═══════════════════════════════════════════════════════
const CATS = [
  { id:"p1", name:"Come\u00e7ar a usar a Conta Azul Pro", color:"#5b6bb0", sections:[
    {name:"Configurar os dados iniciais da empresa", arts:20, articles:["Checklist de configurações: por onde começar", "Meus cadastros: como preencher o dados da empresa", "Meus cadastros: como inserir a marca da empresa", "Meus cadastros: como atualizar meu regime tributário", "Meus cadastros: o que acontece quando eu configuro o Sublimite do Simples Nacional na Conta Azul?", "Meus cadastros: o que é o Sublimite do Simples Nacional?", "Meus cadastros: quais são os regimes tributários brasileiros?", "Meus cadastros: qual é o impacto de trocar meu Regime Tributário nos dados da Conta Azul?", "Busca: como pesquisar as funcionalidades", "Favoritos: o que é e como funciona", "Novo registro: o que é e como funciona", "Configurações e plano: o que é apresentado neste menu", "Mais configurações no ERP: o que é e como funciona", "Primeiros passos: como usar a Conta Azul Pro em uma empresa de serviço", "Primeiros passos: como usar a Conta Azul Pro em uma empresa de comércio", "Primeiros passos: como usar a Conta Azul Pro em uma empresa de atacado ou indústria", "Meus cadastros: como alterar o CNPJ informado na Conta Azul Pro?", "Meus cadastros: posso voltar para o layout ou versão anterior da Conta Azul?", "Meus cadastros: como garantir o recebimento do e-mail de boas-vindas?", "Dados de acesso: qual a previsão para retorno da funcionalidade de edição de e-mail?"]},
    {name:"Controlar usu\u00e1rios e acessos", arts:27, articles:["Dados de acesso: como cadastrar um novo usuário", "Dados de acesso: como editar dados do usuário", "Dados de acesso: como transferir acesso de e-mail do meu usuário", "Dados de acesso: mensagens exibidas pelo sistema ao cadastrar um usuário já criado", "Dados de acesso: como recuperar a senha da Conta Azul", "Dados de acesso: como alterar", "Conta Azul de Bolso: como cadastrar o acesso de multiempresas", "Conta Azul de Bolso: como excluir os dados de uma empresa já cadastrada no Conta Azul de Bolso?", "Conta Azul de Bolso: o multiempresas é diferente de ter vários usuários?", "Conta Azul de Bolso: meus usuários terão acesso ao multiempresas no dispositivo deles?", "Meus cadastros: como parar de receber notificações em um e-mail antigo?", "Dados de acesso: como recuperar o acesso a uma empresa gerenciada?", "Dados de acesso: como funcionam as permissões de edição por perfil de acesso?", "Dados de acesso: como alterar o e-mail de acesso à Conta PJ da ContaAzul IP?", "Dados de acesso: como resolver e-mail bloqueado em conta teste anterior?", "Dados de acesso: como recuperar acesso a conta com e-mail inacessível?", "Dados de acesso: como alterar o e-mail de acesso à conta do sistema?", "Dados de acesso: como resolver problemas de acesso e autenticação de usuários?", "Dados de acesso: como resolver erro de e-mail ou senha incorretos?", "Dados de acesso: como recuperar acesso a conta empresarial sem dados do antigo usuário?", "Dados de acesso: como recuperar a senha de acesso?", "Dados de acesso: como liberar acesso para o vendedor usar o Conta Azul de Bolso?", "Dados de acesso: como gerenciar várias empresas com um único login na Conta Azul?", "Dados de acesso: como cadastrar meu funcionário na Conta Azul?", "Vendas: como cadastrar um vendedor no sistema?", "Dados de acesso: como dar acesso ao Conta Azul para outros usuários?", "Dados de acesso: quantos usuários posso cadastrar no meu plano?"]},
    {name:"Vincular o certificado digital", arts:11, articles:["Certificado digital: o que é e quando usá-lo", "Certificado digital: como adquirir", "Certificado digital A1: adquira com desconto", "Certificado digital A1: como cadastrar", "Certificado digital A1: como exportar o certificado salvo em seu computador", "Certificado digital A1: como instalar em seu computador", "Certificado digital A1: como trocar", "Certificado digital A3: como configurar", "Certificado digital A3: quais são as certificadoras de A3 homologadas pela Conta Azul?", "Certificado digital: como consultar a validade do certificado digital A1", "Certificado digital: perguntas frequentes sobre configuração do A3"]},
    {name:"Proteger meus dados e manter a conex\u00e3o", arts:33, articles:["Segurança: como utilizar a Conta Azul com segurança", "Conta Azul: acesso com autenticação de dois fatores", "Conta Azul Pro: configuração inicial para autenticação de dois fatores", "Conta Azul Pro: redefinição (reset) da autenticação de dois fatores", "Conta Azul: perguntas frequentes de acesso com autenticação de dois fatores", "Conta Azul de Bolso: configuração inicial para autenticação de dois fatores", "Conta Azul de Bolso: redefinição (reset) da autenticação de dois fatores", "Conta Azul de Bolso: soluções para primeiros acessos", "Conta Azul Mais: configuração inicial para autenticação de dois fatores", "Conta Azul Mais: redefinição (reset) da autenticação de dois fatores", "LGPD: como a Lei Geral de Proteção de Dados se aplica à Conta Azul", "Termos de Uso: o que é e como funciona", "Dados de acesso: como acessar o site da Conta Azul Pro?", "Dados de acesso: como acessar a conta após trocar de computador?", "Dados de acesso: como reativar acesso bloqueado por IP suspeito?", "Dados de acesso: como resolver problema de carregamento da tela de vendas?", "Dados de acesso: qual o motivo de erros de acesso frequentes?", "Dados de acesso: como acessar minha conta travada e ver informações de vencimento?", "Dados de acesso: como proceder para resolver bloqueios de acesso ao sistema, especialmente aqueles relacionados à segurança?", "Dados de acesso: como acessar o Conta Azul no iPad e resolver problemas de exibição?", "Dados de acesso: como recuperar senha quando a página não avança?", "Dados de acesso: como resolver o problema de queda ao acessar o sistema em múltiplos aparelhos?", "Dados de acesso: como resolver problemas de instabilidade no sistema?", "Dados de acesso: como resolver lentidão na transição de telas do sistema?", "Dados de acesso: como resolver erro de dados não obtidos nas páginas?", "Dados de acesso: o que fazer para corrigir a mensagem 'Os dados não podem ser carregados'?", "Dados de acesso: como resolver problemas com autenticação facial na Conta Azul?", "Dados de acesso: por que algumas funcionalidades não abrem no meu navegador?", "Vendas: como resolver problemas ao acessar as vendas?", "Dados de acesso: como limpar o cache do meu navegador?", "Dados de acesso: o sistema está lento, o que fazer?", "Suporte: como resolver a lentidão da Conta Azul", "Suporte: como resolver problemas de login no ERP"]},
    {name:"Aprender a usar a plataforma", arts:11, articles:["O que é o ERP Conta Azul Pro?", "Cadastros iniciais: Primeiros passos na Conta Azul Pro", "Capacitação: como aprender a usar a Conta Azul Pro", "Capacitação: como participar do Plantão de dúvidas", "Capacitação: como funcionam os treinamentos gravados", "Capacitação: como funciona a Trilha de treinamento", "Capacitação: como contratar a Consultoria de implantação Conta Azul", "Capacitação: como funciona a Importação de dados da Consultoria de implantação Conta Azul", "Capacitação: como funciona o Pacote Básico da Consultoria de Implantação Conta Azul", "Capacitação: como funciona o Pacote Avançado da Consultoria de Implantação Conta Azul", "Capacitação: como funciona o Pacote Personalizado da Consultoria de Implantação Conta Azul"]},
    {name:"Simplificar a gest\u00e3o com o app Conta Azul de Bolso", arts:4, articles:["Conta Azul de Bolso: O que é possível fazer no aplicativo Conta Azul de Bolso", "Conta Azul de Bolso: como fazer o download do aplicativo", "Conta Azul de Bolso: como funciona a Tela inicial no app", "Conta Azul de Bolso: ativar notificações"]},
    {name:"Administrar minha assinatura", arts:34, articles:["Como contratar um plano na Conta Azul", "Meu Plano: como contratar um novo plano para outra empresa", "Meu Plano: como aproveitar melhor o período de testes da Conta Azul", "Meu Plano: como funciona a Política Comercial da Conta Azul", "Meu Plano: perguntas frequentes sobre a Política Comercial da Conta Azul", "Meu Plano: como visualizar dados de cobrança", "Meu Plano: o que é apresentado em Resumo do plano", "Meu Plano: o que é apresentado em Histórico de pagamento", "Meu Plano: o que é apresentado em Dados de faturamento", "Meu Plano: como editar dados de faturamento da assinatura", "Meu Plano: como trocar a forma de pagamento", "Meu Plano: nota fiscal do plano contratado", "Meu Plano: como funciona o pagamento no boleto", "Meu Plano: quais as vigências liberadas para pagamento por boleto de renovação", "Meu Plano: como acessar o boleto de renovação", "Meu Plano: como acessar seu boleto de renovação após o vencimento", "Meu Plano: como acessar seu boleto com conta bloqueada", "Meu Plano: como funciona o pagamento no cartão de crédito", "Meu Plano: o que significa Este plano é pago por um administrador?", "Meu Plano: como solicitar a devolução do ISS para empresas de Joinville (SC)", "Meu Plano: como contratar os serviços adicionais da Conta Azul Pro", "Meu Plano: como alterar a vigência e recorrência do plano", "Meu Plano: o que é e por que acontece o reajuste e atualização de planos", "Meu Plano: por que seu plano pode ser reajustado", "Meu Plano: como funciona a renovação automática", "Meu Plano: como funciona o cancelamento", "Meu Plano: posso cancelar durante o período de testes?", "Meu Plano: como funciona a reativação e o armazenamento de dados após o cancelamento?", "Meu Plano: como funciona o estorno ao cancelar o plano", "Renovação do plano: como recuperar dados de comandas em aberto após expirar o plano?", "Renovação do plano: por que a renovação da Conta Azul aparece duas vezes na fatura do cartão?", "Renovação do plano: onde encontro o valor da minha assinatura na Conta Azul?", "Dados de acesso: como solucionar erro ao inserir dados do cartão no pagamento da assinatura?", "Dados de acesso: como desbloquear o acesso à conta após pagamento?"]},
    {name:"Falar com a Conta Azul", arts:23, articles:["Suporte: Conheça os canais de atendimento oficiais da Conta Azul", "Suporte: como funciona o suporte por WhatsApp?", "Suporte: como funciona o suporte por chat?", "Suporte: como falar com o suporte via telefone?", "Suporte: como ter suporte sobre a Conta PJ da ContaAzul IP?", "Suporte: como solicitar de reuniões de engajamento?", "Suporte: onde localizar o ID de suporte", "Suporte: conheça a Cami, nossa assistente virtual", "Suporte: como ter as melhores respostas com a IA", "Suporte: como ativar o serviço adicional de Suporte telefônico", "Suporte: como acompanhar meus chamados?", "Suporte: quais informações encontro em Meus chamados?", "Suporte: qual o tempo de retorno para os meus chamados abertos?", "Status da Conta Azul: o que é e como funciona", "Suporte: boas práticas de uso da Central de Ajuda", "Novidades da Conta Azul Pro em 2026", "Novidades da Conta Azul Pro em 2025", "Novidades da Conta Azul Pro em 2024", "Suporte: posso receber atendimento presencial na Conta Azul?", "Suporte: como funciona a Central de chamados?", "E-mails da Conta Azul: o que é que como funciona o envio do Resumo Financeiro Diário e Semanal", "E-mails da Conta Azul: como evitar que os e-mails caiam no SPAM", "E-mails da Conta Azul: como funciona e como evitar o bloqueio no envio de e-mails"]},
    {name:"Ficar por dentro das mudan\u00e7as e evolu\u00e7\u00f5es", arts:5, articles:["Programa de Parceria: Conheça as novidades para parceiros da Conta Azul", "Novidades da Conta Azul Mais em 2026", "Novidades da Conta Azul Mais em 2025", "Conta Azul Mais: Central de chamados", "Conta Azul Mais: Descontinuação do módulo Obrigações"]},
  ]},
  { id:"p2", name:"Implantar na minha empresa", color:"#b8a04a", sections:[
    {name:"\ud83d\udd04 Servi\u00e7o recorrente", arts:5, articles:["1. Configurar a empresa (26 artigos)", "2. Cadastrar contratos e mensalidades (18 artigos)", "3. Enviar cobranças automáticas (7 artigos)", "4. Conectar o banco e conciliar (15 artigos)", "5. Analisar Fluxo de Caixa e DRE (7 artigos)"]},
    {name:"\ud83e\uddfe Servi\u00e7o avulso", arts:5, articles:["1. Configurar a empresa (27 artigos)", "2. Registrar vendas e serviços (7 artigos)", "3. Emitir NFS-e e enviar cobrança (12 artigos)", "4. Conectar o banco e conciliar (15 artigos)", "5. Analisar lucro por projeto (7 artigos)"]},
    {name:"\ud83c\udfed Empresa para Empresa", arts:5, articles:["1. Configurar empresa e produtos (27 artigos)", "2. Registrar venda e emitir NF-e (9 artigos)", "3. Cadastrar conta bancária e integrar (15 artigos)", "4. Conciliar e dar baixa nas vendas (12 artigos)", "5. Registrar compras e controlar estoque (11 artigos)"]},
    {name:"\ud83c\udfea Empresa para Consumidor", arts:5, articles:["1. Configurar empresa, produtos e PDV (21 artigos)", "2. Vender no caixa e emitir NFC-e (8 artigos)", "3. Fechar o caixa e conferir pagamentos (6 artigos)", "4. Conectar o banco e conciliar (13 artigos)", "5. Analisar vendas e posição de estoque (7 artigos)"]},
    {name:"\ud83d\uded2 E-commerce", arts:5, articles:["1. Configurar a empresa e integrar marketplace (21 artigos)", "2. Importar vendas automaticamente (7 artigos)", "3. Cadastrar conta bancária e integrar (15 artigos)", "4. Conciliar repasses do marketplace (11 artigos)", "5. Analisar lucro por canal no DRE (5 artigos)"]},
  ]},
  { id:"p3", name:"Controlar vendas e recebimento de clientes", color:"#2e7d5e", sections:[
    {name:"Registrar e controlar vendas", arts:25, articles:["Vendas: como lançar", "Vendas: como preencher", "Vendas: como clonar", "Vendas: como editar", "Conta Azul de Bolso: como funcionam as Vendas e Orçamentos no app", "Vendas: quais as condição de pagamento e parcelas disponíveis", "Vendas: como usar mais de uma forma de pagamento", "Vendas: quais as formas de pagamento disponíveis por tipo de conta financeira", "Vendas: como excluir", "Vendas: como excluir venda sem nota fiscal emitida", "Vendas: como excluir venda recorrente criada via contrato", "Vendas: como excluir parcelas a receber de uma venda", "Vendas: quais são os filtros para busca de vendas", "Vendas: quais as situações de negociação e como funcionam", "Vendas: como alterar a situação de negociação", "Vendas: como funciona o envio de vendas com o modelo padrão de e-mail", "Vendas: como enviar e o que é enviado por e-mail para o cliente", "Cobrança de vendas: como consultar as parcelas a receber", "Cobrança de vendas: como posso ver detalhes de envio do e-mail", "Cobrança de vendas: como funciona o pagamento com cartão de crédito", "Cobrança de vendas: como meu cliente receberá a fatura com a opção de pagar via cartão de crédito?", "Cobrança de vendas: eu esperava receber em outra conta, como saber que meu cliente optou por pagar via cartão de crédito?", "Cobrança de vendas: eu recebi o pagamento na Conta PJ da ContaAzul IP, como transferir o pagamento para outra conta?", "Cobrança de vendas: como consultar o pagamento via cartão de crédito?", "Vendas: como controlar o repasse de valores para clientes?"]},
    {name:"Consultar guias r\u00e1pidos de vendas e recebimentos", arts:85, articles:["O que significa uma venda com situação “Processando”?", "Vendas: posso criar vendas automaticamente através da Conta AI Captura?", "Vendas: como coletar assinatura eletrônica no orçamento?", "Vendas: como configurar comissão de vendas automática?", "Vendas: como editar vendedores em várias vendas ao mesmo tempo?", "Como salvar filtros personalizados na tela de vendas?", "Vendas: como imprimir o orçamento sem mostrar os valores?", "Vendas: como mudar a ordem dos itens no orçamento?", "Vendas: como ver o lucro da venda antes de concluir?", "Vendas: como juntar várias vendas ou orçamentos em uma única nota fiscal?", "Vendas: como emitir uma nota fiscal para cada parcela da venda?", "Vendas: como preencher automaticamente a descrição do produto no orçamento?", "Vendas: como criar campos personalizados no orçamento?", "Vendas: como simular parcelas automaticamente no orçamento?", "Vendas: como informar a dedução ou desconto de impostos nas cobranças da Conta Azul?", "Vendas: como editar uma venda após baixa em parcela?", "Financeiro: como funciona o agendamento de boletos na Conta Azul?", "Vendas: como alterar o país no cadastro de cliente?", "Financeiro: por que meu recebimento não aparece em contas a receber?", "Financeiro: como pagamentos em dia são exibidos no contas a receber?", "Vendas: como resolver valor incorreto no fechamento do caixa após cancelamento de venda?", "Contratos: é possível importar contratos por planilha na Conta Azul?", "Vendas: como entender e prevenir pagamentos duplicados ou antecipados?", "Vendas: como configurar as maquininhas de cartão integradas?", "Vendas: como configurar as Pré-definições da venda e permissões do frente de caixa?", "Vendas: como lançar o décimo terceiro nos contratos?", "Vendas: como atualizar o preço de venda dos produtos?", "Vendas: como rastrear quem excluiu uma venda?", "Vendas: como ocultar serviços inativos na tela de cadastro?", "Vendas: como alterar a categoria de receita no frente de caixa?", "Vendas: como editar uma venda já gerada com nota fiscal?", "Vendas: ao excluir uma venda, posso recuperá-la?", "Vendas: como editar uma venda que já foi quitada?", "Vendas: o valor da minha venda está incorreto. O que fazer?", "Vendas: ao excluir um vendedor, perco os orçamentos dele?", "Vendas: como alterar uma venda já emitida?", "Vendas: posso editar parcelas de vendas recorrentes?", "Vendas: meus pedidos de venda não aparecem na Conta Azul. O que fazer?", "Vendas: é possível editar uma venda no Frente de caixa?", "Vendas: é possível adicionar observações internas em contratos?", "Vendas: onde encontro a opção para excluir uma venda?", "Vendas: como anexar um arquivo a um contrato?", "Vendas: por que o sistema bloqueia vendas e orçamentos?", "Vendas: é possível alterar contratos por centro de custo?", "Vendas: quais dados ficam disponíveis ao excluir uma venda?", "Vendas: como atualizar o valor de venda automaticamente?", "Vendas: como editar uma venda para adicionar parcelas?", "Vendas: como alterar a data de geração de vendas no contrato?", "Vendas: posso excluir um serviço com vendas já lançadas?", "Vendas: posso mudar o status de uma venda aprovada?", "Vendas: é possível atualizar os preços de venda em lote?", "Vendas: por que não consigo anexar arquivos em vendas recorrentes?", "Vendas: como ratear o centro de custo após salvar a venda?", "Vendas: como ajustar a emissão automática após reajuste no contrato?", "Vendas: vendas de contrato não aparecem, o que fazer?", "Vendas: o prazo de entrega some ao transformar orçamento em venda. Como resolver?", "Vendas: o contrato encerra automaticamente?", "Vendas: qual a diferença entre contrato avulso e recorrente?", "Vendas: é possível reativar um contrato cancelado?", "Vendas: como adicionar o e-mail do cliente em um contrato?", "Vendas: como gerar um comprovante de pagamento sem ser nota fiscal?", "Vendas: como parcelar um contrato em várias datas no mês?", "Vendas: onde encontro a lixeira de orçamentos excluídos?", "Vendas: o sistema puxa automaticamente a forma de pagamento?", "Vendas: e-mails de venda não chegam, o que fazer?", "Vendas: onde adicionar descrições para pedidos no orçamento?", "Vendas: por que não encontro meu produto na venda?", "Vendas: é possível marcar um cliente como \"mal pagador\" na Conta Azul?", "Vendas: como aplicar um desconto retroativo em uma recorrência?", "Vendas: como criar um orçamento a partir de uma ordem de serviço?", "Vendas: como adicionar marca d'água ou anexar fotos em orçamentos?", "Vendas: a Conta Azul integra com balanças para vendas a granel?", "Vendas: como integrar vendas da minha loja com a Conta Azul?", "Vendas: como dar baixa em vendas da Hotmart na Conta Azul?", "Vendas: como integrar vendas antecipadas da Hotmart no Conta Azul?", "Vendas: como reativar um contrato já encerrado?", "Vendas: como alterar o prazo de validade de um contrato?", "Vendas: por que meu contrato não aparece na lista de contratos?", "Vendas: como alterar a data de faturamento de um contrato?", "Vendas: posso personalizar o layout de impressão do orçamento?", "Vendas: é possível inserir meu próprio modelo de proposta?", "Vendas: como dar baixa em um contrato?", "Vendas: alterar a data da última venda encerra o contrato?", "Vendas: onde vejo a última parcela de um contrato na Conta Azul?", "Frente de caixa (PDV): perguntas frequentes"]},
    {name:"Gerenciar a venda de servi\u00e7os", arts:5, articles:["Venda de serviço: como lançar", "Venda de serviço: como imprimir venda de serviço", "Venda de serviço: como configurar o cálculo de impostos na venda de serviço", "Venda de serviço: como fazer a gestão de consumo de pacotes", "Vendas de serviço: como acompanhar cronograma de faturamento por projeto"]},
    {name:"Gerenciar a venda de produtos", arts:5, articles:["Venda de produto: como lançar", "Venda de produto: como imprimir a venda de produto", "Venda de produto: como atualizar uma venda devolvida", "Venda de produto: como atualizar a venda devolvida com nota fiscal emitida", "Venda de produto: como atualizar a venda devolvida sem nota fiscal"]},
    {name:"Organizar ordens de servi\u00e7o e or\u00e7amentos", arts:10, articles:["Orçamentos: como enviar um orçamento", "Orçamentos: como transformar o orçamento em venda", "Orçamentos: como filtrar meus orçamentos", "Orçamentos: como alterar a situação do orçamento", "Ordem de serviço: como criar", "Ordem de serviço: como editar ou excluir", "Orçamentos: como criar", "Orçamentos: como gerenciar negociações", "Orçamentos: como alterar", "Orçamentos: como excluir"]},
    {name:"Acompanhar contratos recorrentes", arts:58, articles:["Contratos: como funciona a cobrança recorrente por cartão de crédito", "Contratos: como configurar cobranças automáticas por cartão de crédito", "Contratos: como lançar", "Contratos: como clonar", "Contratos: como preencher", "Contratos: como preencher as Informações da venda?", "Contratos: como preencher as Configurações de recorrência?", "Contratos: como preencher a Classificação da venda e Centro de custo?", "Contratos: como preencher os Itens da venda?", "Contratos: como preencher as Informações de pagamento?", "Contratos: como preencher as Configurações de envio automático de e-mails?", "Contratos: configurações de envio automático", "Contratos: como configurar o envio automático da venda, cobrança e NFS-e no contrato?", "Contratos: como incluir ou editar e-mails que receberão automaticamente as vendas do contrato?", "Contratos: como consultar a situação do envio automático do contrato?", "Contratos: como cancelar o envio automático do contrato?", "Contratos: o que acontece se eu editar uma venda recorrente ou editar o contrato com envio automático habilitado?", "Contratos: como editar", "Contratos: como editar a data de automação para emissão e envio automático de cobranças e NFS-e?", "Contratos: qual a diferença entre 'Editar apenas a venda prevista' e 'Editar todas as próximas vendas do contrato'?", "Contratos: o que fazer quando acontece um erro ao editar o contrato?", "Contratos: como editar a recorrência", "Contratos: como editar a vigência", "Contratos: como funciona o contrato com vigência menor que 24 meses", "Contratos: como funciona o contrato com vigência maior que 24 meses", "Contratos: como atualizar contratos em lote", "Contratos: como renovar em lote por mais meses", "Contratos: como aplicar reajuste de valor em lote", "Contratos: quais as regras para aplicar reajuste de valor em lote", "Contratos: o que acontece ao renovar e reajustar em lote ao mesmo tempo", "Contratos: como antecipar a venda", "Contratos: cobrança recorrente com Cartão de crédito via link", "Contratos: como cobrar com Pix cobrança", "Contratos: filtros para busca de contratos", "Contratos: como finalizar e encerrar", "Contratos: o que significam as datas do contrato", "Contratos: o que preencher em Data de início e qual o impacto no contrato?", "Contratos: o que preencher em Dia da geração das vendas e qual o impacto no contrato?", "Contratos: o que preencher em 'Repetir venda a cada', e qual o impacto no contrato?", "Contratos: o que preencher em Término da recorrência e qual o impacto no contrato?", "Contratos: o que preencher em 'Vencer sempre no' e 'Vencimento', e qual o impacto no contrato?", "Contratos: como o preenchimento de datas no contrato funciona na prática?", "Contratos: gestão por número do contrato", "Contratos: como encontrar o número de um contrato já criado?", "Contratos: como editar o número do contrato na criação do contrato?", "Contratos: como editar o número do contrato já criado?", "Contratos: como filtrar pelo número do contrato na tela de vendas?", "Contratos: como diferenciar o Número do contrato do Número da venda de um contrato?", "Contratos: o que significa a mensagem O número do contrato informado já foi utilizado em outro contrato.?", "Contratos: como funciona a exclusão de um contrato", "Contratos: como excluir um contrato", "Contratos: como excluir a venda recorrente criada pelo contrato", "Contratos: como incluir serviços adicionais em contratos recorrentes", "Contratos: como gerenciar implantação e mensalidades de serviços recorrentes", "Contratos: como realizar cadastros para controle de locação", "Contratos: como lançar vendas recorrentes de produto de comércio", "Contratos: como lançar vendas recorrentes de produto da indústria", "Contratos: qual é a diferença entre contratos e vendas na Conta Azul?"]},
    {name:"Configurar cobran\u00e7as via Pix, Boleto ou Link de cart\u00e3o", arts:55, articles:["Boleto da Conta Azul: como emitir e enviar", "Boleto da Conta Azul: como emitir e enviar pela venda avulsa", "Boleto da Conta Azul: como emitir e enviar pelo contrato", "Boleto da Conta Azul: como emitir e enviar pelo financeiro", "Boleto da Conta Azul: como consultar o boleto emitido e enviado?", "Boleto da Conta Azul: como realizar o download em lote dos Boletos da Conta Azul?", "Boleto da Conta Azul: como meu cliente receberá o e-mail com o Boleto da Conta Azul?", "Pix cobrança da Conta Azul: como emitir e enviar", "Pix cobrança da Conta Azul: como emitir e enviar o Pix cobrança da Conta Azul pela venda?", "Pix cobrança da Conta Azul: como emitir e enviar o Pix cobrança da Conta Azul pelo financeiro?", "Pix cobrança da Conta Azul: como meu cliente receberá o e-mail com o Pix cobrança da Conta Azul?", "Cartão de crédito via link da Conta Azul: como emitir e enviar", "Cartão de crédito via link da Conta Azul: como enviar o link a partir de uma venda recorrente (contrato)?", "Cartão de crédito via link da Conta Azul: como enviar o link a partir de uma venda avulsa?", "Cartão de crédito via link da Conta Azul: como enviar o link pelo lançamento financeiro (contas a receber)?", "Cartão de crédito via link da Conta Azul: como meu cliente recebe o Link de pagamento único?", "Cartão de crédito via link da Conta Azul: como meu cliente recebe o Link de pagamento recorrente?", "Cartão de crédito via link da Conta Azul: o que significa se o status da fatura for 'Estornada' ou 'Protestada'?", "Cartão de crédito via link da Conta Azul: se meu cliente parcelar, como vou receber os valores?", "Boleto da Conta Azul: modelo de fatura", "Boleto da Conta Azul: o que aparece no Cabeçalho da fatura do Boleto da Conta Azul?", "Boleto da Conta Azul: o que aparece nos 'Detalhes da venda' do Boleto da Conta Azul?", "Boleto da Conta Azul: o que aparece nos 'Detalhes da fatura' do Boleto da Conta Azul?", "Boleto da Conta Azul: o que aparece em 'Selecione a forma de pagamento' no Boleto da Conta Azul?", "Boleto da Conta Azul: como funciona o botão 'Atualize esta cobrança' em uma fatura expirada no Boleto da Conta Azul?", "Boleto da Conta Azul: o que significa a mensagem \"Esta cobrança está vencida\" quando seleciono a forma de pagamento Pix?", "Pix cobrança da Conta Azul: modelo de fatura", "Pix cobrança da Conta Azul: o que aparece no Cabeçalho da fatura no Pix cobrança da Conta Azul?", "Pix cobrança da Conta Azul: o que aparece nos Detalhes da venda no Pix cobrança da Conta Azul?", "Pix cobrança da Conta Azul: o que aparece nos Detalhes da fatura no Pix cobrança da Conta Azul?", "Pix cobrança da Conta Azul: o que aparece em Selecione a forma de pagamento no Pix cobrança da Conta Azul?", "Pix cobrança da Conta Azul: como funciona o botão Atualize esta cobrança em uma Fatura expirada no Pix cobrança da Conta Azul?", "Cartão de crédito via link da Conta Azul: modelo de fatura", "Cartão de crédito via link da Conta Azul: como é o modelo de fatura do link de pagamento recorrente?", "Cartão de crédito via link da Conta Azul: como é o modelo de fatura do link de pagamento único?", "Boleto da Conta Azul: como editar boleto", "Boleto da Conta Azul: como cancelar", "Cobranças da Conta Azul: Renegociar ou combinar cobranças", "Cobranças da Conta Azul: como selecionar recebimentos para renegociar cobranças?", "Cobranças da Conta Azul: como emitir e enviar nova cobrança renegociada (combinada)?", "Cobranças da Conta Azul: como consultar histórico de renegociações?", "Cobranças da Conta Azul: como utilizar o recurso de Combinar cobranças?", "Cobranças da Conta Azul: como atualizar uma cobrança vencida", "Cobranças da Conta Azul: boleto, Pix ou cartão de crédito na mesma fatura", "Cobranças da Conta Azul: perguntas frequentes sobre o boleto, Pix ou cartão de crédito na mesma fatura", "Cobranças da Conta Azul: modelos de faturas", "Cobranças da Conta Azul: como migrar a cobrança de contratos em lote", "Cobranças: como enviar boletos por WhatsApp", "Cobranças: como criar uma cobrança única de vários contratos?", "Cobranças: como alterar a carteira de um boleto já emitido?", "Cobranças: como verificar se os boletos foram enviados?", "Pix cobrança da Conta Azul: como cadastrar chave Pix", "Pix cobrança da Conta Azul: quais os tipos de chave Pix disponíveis e quem pode cadastrar", "Pix cobrança da Conta Azul: como fazer portabilidade da chave Pix", "Pix cobrança da Conta Azul: como excluir ou alterar chave Pix"]},
    {name:"Emitir boletos por outros bancos", arts:17, articles:["Boletos registrados: bancos homologados pela Conta Azul", "Boletos registrados: como configurar", "Boletos registrados: carteiras bancárias habilitadas pela Conta Azul", "Arquivo de remessa: como gerar", "Arquivo de retorno: como gerar", "Boletos registrados: como consultar os dados bancários da cooperativa Ailos", "Boletos registrados: como consultar os dados bancários do Banco do Brasil", "Boletos registrados: como consultar os dados bancários do Bradesco", "Boletos registrados: como consultar os dados bancários da Caixa Econômica Federal", "Boletos registrados: como consultar os dados bancários do Itaú", "Boletos registrados: como consultar os dados bancários do Santander", "Boletos registrados: como consultar os dados bancários do Sicoob", "Boletos registrados: como consultar os dados bancários do Sicredi", "Cobranças: como baixar arquivo de remessa de boletos em lote?", "Boletos registrados: preciso gerar remessa para todo boleto após a configuração?", "Boletos registrados: meu banco pede homologação para confiurar. O que faço?", "Boletos registrados: posso mudar juros e multa depois de configurado?"]},
    {name:"Consultar taxas e prazos das Cobran\u00e7as", arts:26, articles:["Conta PJ da ContaAzul IP: como configurar juros e multas das cobranças", "Boleto da Conta Azul: taxas e prazo para transferência", "Pix cobrança da Conta Azul: taxas e prazo para transferência", "Cartão de crédito via link da Conta Azul: taxas e prazo para transferência", "Cartão de crédito via link da Conta Azul: consigo simular o total de taxas antes de emitir a cobrança?", "Cartão de crédito via link da Conta Azul: posso transferir as taxas para que o cliente pague?", "Cartão de crédito via link da Conta Azul: meu cliente já pagou o link, mas o lançamento ainda aparece em aberto, por quê?", "Cartão de crédito via link da Conta Azul: como conferir se o cliente pagou antes da transferência?", "Cartão de crédito via link da Conta Azul: se o pagamento for parcelado, recebo o valor parcelado?", "Cobranças da Conta Azul: entenda os descontos na compensação", "Cobranças da Conta Azul: desconto para pagamento antecipado", "Cobranças da Conta Azul: como funciona o desconto para pagamento antecipado em uma Venda avulsa ou Contas a receber?", "Cobranças da Conta Azul: como funciona o desconto para pagamento antecipado em uma Venda recorrente (Contrato)?", "Cobranças da Conta Azul: taxas e transferências", "Cobranças da Conta Azul: quanto tempo leva para o valor cair na Conta PJ após o pagamento?", "Boleto da Conta Azul: como funciona a cobrança por Lembrete de Vencimento?", "Cobranças da Conta Azul: benefícios do boleto", "Cobranças: quais as taxas para emissão de boletos e Pix da Conta Azul?", "Cobranças: como saber o valor final que receberei com as Cobranças da Conta Azul?", "Cobranças: quais são os serviços e taxas aplicáveis das Cobranças da Conta Azul?", "Cobranças: como consultar o total de taxas de Cobranças da Conta Azul na Conta Azul Pro?", "Cobranças: por que minhas taxas de boleto são diferentes?", "Cobranças: qual o custo para emitir boleto na Conta Azul?", "Cobranças: qual o limite máximo para multa de cobrança?", "Cobranças: é possível antecipar recebimentos de Cartão de crédito (via link) da Conta Azul?", "Lembretes de vencimento: quais são as taxas de envio de SMS e WhatsApp?"]},
    {name:"Agilizar corre\u00e7\u00f5es e seguran\u00e7a das cobran\u00e7as", arts:22, articles:["Cobranças: o que é e como funciona o chargeback?", "Cobranças: como funciona a disputa de chargeback?", "Cobranças: o que acontece quando meu cliente inicia a disputa de chargeback?", "Cobranças: como aceitar ou contestar solicitação de estorno do chargeback?", "Cobranças: quais os documentos solicitados para análise da disputa de chargeback?", "Cobranças: como é concluída a disputa de chargeback?", "Pix cobrança da Conta Azul: o que é e como funciona o Mecanismo Especial de Devolução (MED)", "Pix cobrança da Conta Azul: quem pode abrir uma solicitação de MED?", "Pix cobrança da Conta Azul: posso cancelar uma solicitação de MED?", "Pix cobrança da Conta Azul: o MED pode ser usado em desacordo comercial?", "Pix cobrança da Conta Azul: após conculsão do MED, o que acontece se o recebedor não tiver saldo para devolver o Pix?", "Cobranças da Conta Azul | Erro: Não foi possível realizar a transferência.", "Cobranças da Conta Azul | Erro: Não foi possível atualizar a previsão de vencimento", "Cobranças da Conta Azul | Erro: Não foi possível identificar o documento a ser utilizado na cobrança", "Cobranças: boletos apresentam erro ao abrir, o que fazer?", "Cobranças: qual o status de um valor estornado e não recebido?", "Cobranças: como resolver o bloqueio de envio de e-mails?", "Cobranças: qual o motivo da demora na baixa de boletos?", "Cobranças: por que a cobrança no cartão de crédito fica suspensa e como reativar?", "Cobranças: como resolver boleto pago que não consta recebimento?", "Cobranças: qual o prazo para resolução de contestação de pagamento na disputa de chargeback?", "Cobranças: como resolver pagamento não baixado com comprovante enviado?"]},
    {name:"Controlar parcelas a receber de clientes", arts:26, articles:["Conta a receber: como lançar", "Conta a receber: como lançar receita à vista", "Conta a receber: como lançar receita recorrente", "Conta a receber: como editar receita recorrente", "Conta a receber: como lançar receita parcelada", "Conta a receber: como editar receita parcelada", "Conta a receber: como renegociar valores em aberto", "Cobranças: tipos de cobranças emitidas pela Conta Azul", "Cobranças: como funcionam os Boletos da Conta Azul?", "Cobranças: como funciona o Pix cobrança da Conta Azul?", "Cobranças: como funciona o Cartão de crédito via link da Conta Azul?", "Cobranças: como funcionam os Boletos por Outros bancos (homologados)?", "Cobranças: como funciona a compensação de boletos emitidos pela Conta Azul?", "Cobranças: como emitir em lote as cobranças de uma venda", "Cobranças: como visualizar parcelas pagas", "Cobranças: como controlar as parcelas vencidas", "Boletos: quais os tipos de boletos emitidos pela Conta Azul?", "Boletos: como emitir boleto pelo Financeiro da Conta Azul?", "Boletos: como emitir boleto pelo Contrato da Conta Azul?", "Boletos: como emitir boleto pela Venda da Conta Azul?", "Boletos: como filtrar recebimentos por boleto", "Boletos: como fazer download em lote", "Boletos: como editar a descrição de boletos", "Boletos: como ver a baixa", "Boletos: como excluir", "Recibo: como emitir comprovante de baixa"]},
    {name:"Confirmar recebimentos e pagamentos", arts:14, articles:["Lançamentos financeiros: como fazer baixa parcial", "Lançamentos financeiros: como restaurar um lançamento marcado como perda", "Lançamentos financeiros: como visualizar lançamentos salvos como perda", "Lançamentos financeiros: como aplicar juros, multa, desconto e tarifa", "Lançamentos financeiros: como informar pagamento e recebimento adiantado", "Lançamentos financeiros: agendar pagamento", "Lançamentos financeiros: salvar como perda", "Lançamentos financeiros: como os marcados como perda aparecem nos relatórios", "Lançamentos financeiros: como excluir", "Lançamentos financeiros: como excluir em lote", "Lançamentos financeiros: quais os requisitos para excluir em lote?", "Lançamentos financeiros: como excluir em lote pela tela de Visão de competência", "Lançamentos financeiros: como excluir em lote pelas telas de Contas a pagar ou Contas a receber", "Lançamentos financeiros: como excluir lançamentos financeiros?"]},
    {name:"Automatizar a gest\u00e3o de inadimpl\u00eancia", arts:33, articles:["Cobranças da Conta Azul: como agendar o envio de lembrete de vencimentos de um lançamento de contas a receber já criado?", "Cobranças da Conta Azul: como enviar um lembrete de vencimento no contas a receber de uma cobrança parcelada?", "Cobranças da Conta Azul: como enviar um lembrete de vencimento no contas a receber de uma cobrança com parcela única", "Cobranças da Conta Azul: como enviar um lembrete de vencimento na venda recorrente?", "Cobranças da Conta Azul: como enviar um lembrete de vencimento na venda avulsa?", "Cobranças da Conta Azul: perguntas frequentes sobre novos lembretes de vencimento da Conta Azul", "Cobranças da Conta Azul: Novos lembretes de vencimento da Conta Azul", "Lembretes de vencimento: como funcionam", "Lembretes de vencimento: como ativar", "Lembretes de vencimento: como ativar na venda avulsa", "Lembretes de vencimento: como ativar no contrato (venda recorrente)", "Lembretes de vencimento: como ativar no financeiro", "Lembretes de vencimento: como cancelar", "Cobranças: como funciona a gestão de Inadimplentes", "Cobranças: como renegociar uma parcela com a gestão de Inadimplentes?", "Cobranças: como renegociar várias parcelas com a gestão de Inadimplentes?", "Cobranças: como analisar o histórico de renegociações com a gestão de Inadimplentes?", "Cobranças: como encontrar os lançamentos originais renegociados pela gestão de Inadimplentes?", "Cobranças: como gerenciar os novos lançamentos gerados na renegociação com a gestão de Inadimplentes?", "Cobranças: como visualizar o boleto e os lançamentos após realizar a renegociação com a gestão de Inadimplentes?", "Cobranças: quais são os Detalhes dos lançamentos da gestão de Inadimplentes?", "Cobranças: o que significa a Visão por cliente da gestão de Inadimplentes?", "Cobranças: o que significam os Totais em atraso da gestão de Inadimplentes?", "Cobranças: quais são os Atalhos rápidos da gestão de Inadimplentes?", "Consulta Serasa: o que é e como funciona", "Consulta Serasa: como fazer uma consulta Serasa?", "Consulta Serasa: como analisar os detalhes da consulta Serasa?", "Consulta Serasa: como fazer o pagamento da consulta Serasa?", "Consulta Serasa: quais consultas não estão disponíveis?", "Cobranças da Conta Azul: como realizar protesto", "Cobranças: como funcionam as taxas e o pagamento dos lembretes de vencimento?", "Cobranças: quais as taxas e o pagamento dos Novos lembretes de vencimento?", "Cobranças: quais as taxas e o pagamento dos lembretes de vencimento?"]},
    {name:"Realizar vendas no PDV", arts:18, articles:["NFC-e: como ativar o serviço adicional de Frente de caixa (PDV)", "Frente de caixa (PDV): o que é e como funciona", "Frente de caixa (PDV): como abrir e fechar caixa", "Frente de caixa (PDV): como criar uma pré-venda", "Frente de caixa (PDV): como usar os atalhos do teclado", "Frente de caixa (PDV): como informar cliente na NFC-e", "Frente de caixa (PDV): como identificar vendedor na venda", "Frente de caixa (PDV): como escolher mais de uma forma de pagamento na venda", "Frente de caixa (PDV): como finalizar venda sem emitir NFC-e", "Frente de caixa (PDV): como finalizar a venda com a maquininha da Stone", "Frente de caixa (PDV): como realizar a venda integrada à maquininha com mais de uma forma de pagamento?", "Frente de caixa (PDV): como controlar as movimentações do caixa", "Frente de caixa (PDV): o que são suprimento e sangria", "Frente de caixa (PDV): como registrar suprimento manualmente", "Frente de caixa (PDV): como registrar sangria manualmente", "Frente de caixa (PDV): como funcionam os registros automáticos de suprimento e sangria ao abrir e fechar o caixa", "Frente de caixa (PDV): como funciona a conta Cofre e a conta Gaveta", "Frente de caixa (PDV) | Erro: O pagamento foi recusado"]},
    {name:"Utilizar o frente de caixa (PDV)", arts:22, articles:["Frente de caixa (PDV): como emitir a NFC-e", "NFC-e: como agilizar o preenchimento da NFC-e no frente de caixa (PDV)", "NFC-e: como enviar para o contador", "NFC-e: como emitir nota fiscal de devolução da venda do Frente de caixa (PDV)", "NFC-e: como os dados do cliente são preenchidos na nota fiscal de devolução da venda do Frente de caixa (PDV)", "NFC-e: como editar itens, CFOP e CST no rascunho da nota fiscal de devolução da venda do Frente de caixa (PDV)", "Frente de caixa (PDV): o que é e como funciona o Menu fiscal", "Frente de caixa (PDV): o que é a Identificação do PAF-NFC-e no Menu fiscal", "Frente de caixa (PDV): como gerar o relatório de Registros PAF - NFC-e no Menu fiscal", "Frente de caixa (PDV): como gerar o relatório de Vendas identificadas por CPF/CNPJ no Menu fiscal", "Frente de caixa (PDV): o que é apresentado em Requisições externas registradas, Controle dos DAVs e Mesas abertas no Menu fiscal", "Frente de caixa (PDV): em qual impressora posso imprimir a NFC-e?", "Frente de caixa (PDV): como funciona a busca dos produtos informados na NFC-e?", "Frente de caixa (PDV): como consultar as NFC-e emitidas pela Conta Azul?", "Frente de caixa (PDV): posso utilizar leitor de código de barras no Frente de Caixa (PDV)?", "Frente de caixa (PDV): posso emitir NFC-e sem informar o cliente?", "Frente de caixa (PDV): é possível realizar a venda sem ter o produto cadastrado no estoque da Conta Azul?", "Frente de caixa (PDV): é possível calcular desconto no momento da venda no Frente de Caixa (PDV)?", "Frente de caixa (PDV): por que o pagamento em Dinheiro não pode ser editado depois de emitir a NFC-e?", "NFC-e: é possível importar NFC-e para a Conta Azul?", "NFC-e: as notas fiscais de produto aparecem no frente de caixa?", "Notas fiscais: a Conta Azul emite recibo formatado para impressora fiscal?"]},
  ]},
  { id:"p4", name:"Emitir e gerenciar notas fiscais", color:"#a04848", sections:[
    {name:"Ajustar os pr\u00e9-requisitos de emiss\u00e3o", arts:32, articles:["NFS-e: pré-requisitos para emissão", "Emissão de NFS-e: consulte as cidades homologadas", "NFS-e: como configurar", "Notas fiscais de serviço (NFS-e) no Padrão Nacional: o que é e quem pode emitir", "Notas fiscais de serviço (NFS-e) no Padrão Nacional: como configurar na Conta Azul", "Notas fiscais de serviço (NFS-e) no Padrão Nacional: como gerar nota pelo Emissor Nacional", "NFS-e: como calcular a base de cálculo do IBS e da CBS", "NFS-e: perguntas frequentes sobre a Reforma Tributária", "NFS-e: perguntas frequentes sobre nota fiscal de serviço", "NFS-e: quais cidades permitem a substituição de nota fiscal de serviço", "Vendas: como ativar a emissão automática de NFS-e", "Contratos: como ativar a emissão automática de NFS-e", "NFS-e: o que é ISS e como preencher", "NFS-e: como preencher o ISS no cadastro de um novo serviço", "NFS-e: como preencher o ISS no rascunho da nota fiscal de serviços", "NFS-e: o que significa alíquota de ISS fixa e alíquota de ISS variável?", "NFS-e: como manter a alíquota do ISS fixo atualizada?", "NFS-e: como manter a alíquota do ISS variável atualizada?", "NFS-e: como configurar a alíquota de ISS na emissão de NFS-e", "NFS-e: como funciona a edição da alíquota de ISS", "NFS-e: como replicar a alteração da alíquota do ISS?", "NFS-e: como cancelar a replicação de impostos da alíquota de ISS?", "NFS-e: como atualizar os dados da prefeitura para emitir", "NFS-e: como configurar número de RPS", "NFS-e: como preencher o código de serviço", "NFS-e: informações para impostos federais", "NFS-e: como solicitar a homologação novas cidades", "NFS-e: como editar o imposto de um serviço", "NFS-e: como cancelar a replicação de impostos", "NFS-e: como informar impostos federais ao emitir NFS-e no Padrão Nacional", "NFS-e: quais são os novos campos de Tributação Federal no PDF da NFS-e no Padrão Nacional?", "NS-e: como gerenciar retenção de IRRF após alteração de regra?"]},
    {name:"Vincular o certificado digital", arts:11, articles:["Certificado digital: o que é e quando usá-lo", "Certificado digital: como adquirir", "Certificado digital A1: adquira com desconto", "Certificado digital A1: como cadastrar", "Certificado digital A1: como exportar o certificado salvo em seu computador", "Certificado digital A1: como instalar em seu computador", "Certificado digital A1: como trocar", "Certificado digital A3: como configurar", "Certificado digital A3: quais são as certificadoras de A3 homologadas pela Conta Azul?", "Certificado digital: como consultar a validade do certificado digital A1", "Certificado digital: perguntas frequentes sobre configuração do A3"]},
    {name:"Entender termos e siglas fiscais", arts:36, articles:["NF-e: é possível emitir a nota complementar de crédito de ICMS na Conta Azul?", "Notas fiscais: CBS e IBS já geram multa na emissão de NF-e e NFS-e?", "NF-e: qual a diferença entre Pauta Fiscal e MVA e o impacto no cálculo do ICMS-ST?", "NF-e: qual é a diferença entre CST e CSOSN?", "NF-e: onde consultar e como preencher o Código de Classificação Tributária (cClassTrib)", "Notas fiscais: a Conta Azul emite CT-e ou MDF-e?", "NFS-e: onde consultar e como validar os Totais da nota fiscal", "NFS-e: onde consultar e como preencher a Dedução de INSS", "Notas fiscais: como consultar o Valor Aproximado dos Tributos nas notas emitidas pela Conta Azul?", "NFS-e: onde consultar e como funciona o Valor aproximado dos tributos", "NFS-e: onde consultar e como preencher o ART na nota de serviço", "NFS-e: onde consultar e como preencher o CEI ou CNO na nota de serviço", "NFS-e: onde consultar e como preencher a Indicador da operação (Natureza de operação)", "NFS-e: onde consultar e como preencher o IBS e CBS", "NFS-e: onde consultar e como preencher a alíquotas de IBS e CBS", "NFS-e: onde consultar e como preencher o Código de Classificação Tributária (cClassTrib)", "NFS-e: onde consultar e como preencher a Dedução de ISS", "NFS-e: onde consultar e como preencher a alíquota de ISS", "NFS-e: onde consultar e como preencher o Município de incidência do ISS", "NFS-e: onde consultar e como preencher o Responsável pelo recolhimento do ISS", "NFS-e: onde consultar e como preencher o CNAE", "NFS-e: onde consultar e como preencher o Código do serviço", "NFS-e: onde consultar e como preencher a Descrição do serviço", "NFS-e: onde consultar e como preencher a Data do RPS", "NFS-e: onde consultar e como preencher as credenciais de acesso da nota de serviço", "NFS-e: onde consultar e como preencher a Natureza de operação", "NFS-e: onde consultar e como preencher a Série do RPS", "NFS-e: onde consultar e como preencher o Número do RPS", "NFS-e: onde consultar e como preencher a Inscrição municipal", "NFS-e: onde consultar e como preencher o Código NBS (cNBS)", "NFS-e: onde consultar e como preencher o Tipo de serviço (Prestado ou Tomado)", "NFS-e: quais as situações da NFS-e importada", "Notas fiscais: quais notas fiscais podem ser emitidas pela Conta Azul?", "Notas fiscais: como acessar notas fiscais emitidas na Conta Azul", "Notas fiscais: qual o prazo que a nota leva para aparecer como emitida fora da Conta Azul?", "NFS-e: o que é RPS ou DPS?"]},
    {name:"Conhecer notas t\u00e9cnicas fiscais", arts:14, articles:["Nota Técnica 2021.003: como corrigir as rejeições de GTIN", "Nota Técnica 2021.003: quais as regras de validação do GTIN", "Nota Técnica 2020.006: como funciona o preenchimento na Conta Azul", "Nota Técnica 2020.006: como preencher a Forma de pagamento na venda", "Nota Técnica 2020.006: como preencher o Indicador de Presença na nota de produto", "Nota Técnica 2020.006: como preencher o Intermediador da transação na nota de produto", "Nota Técnica 2024.001 - versão 2.00: quais os códigos da Tabela NCM", "Nota Técnica 2020.005: como funciona o preenchimento do CST 51 do ICMS", "Nota Técnica 2020.005: como funciona o preenchimento dos CSTs 10, 70 e 90 do ICMS", "Nota Técnica 2019.001 - versão 1.30: quais são as validações fiscais e limites de base de cálculo do ICMS", "Nota Técnica 2018.005: como preencher a Identificação do Responsável Técnico", "Nota Técnica 2018.005: como preencher o Valor ICMS próprio substituto", "Nota Técnica 2016.003 - versão 2.00: quais os códigos da Tabela NCM", "Nota Técnica 2016.003 - versão 3: quais as mudanças da Tabela NCM"]},
    {name:"Consultar termos e regras fiscais", arts:2, articles:["Notas fiscais: configurações básicas para emissão de notas fiscais", "Notas fiscais: perguntas frequentes sobre os principais impostos no Brasil"]},
    {name:"Preencher nota fiscal de servi\u00e7o (NFS-e)", arts:12, articles:["NFS-e: como preencher a nota fiscal de serviço para emissão", "NFS-e: como emitir", "NFS-e: como gerar rascunho de nota de serviço em lote", "NFS-e: como emitir notas fiscais em lote", "NFS-e: o que fazer quando o status é Aguardando retorno?", "NFS-e: o que fazer quando o status é Em espera?", "NFS-e: como gerar nota de serviço para o exterior ou para tomador estrangeiro", "NFS-e: como emitir em São Paulo sem certificado digital", "NFS-e: por que o PDF da NFS-e está diferente do preenchimento no ERP?", "NFS-e: por que PIS, COFINS e CSLL aparecem agrupados no campo CSLL?", "NFS-e: minha prefeitura comunicou mudança para o  Padrão Nacional, o que fazer?", "NFS-e: como emitir sua primeira nota de serviço"]},
    {name:"Gerenciar notas de servi\u00e7o emitidas", arts:6, articles:["NFS-e: como fazer a substituição de nota fiscal de serviço", "NFS-e: como baixar os arquivos da nota fiscal", "NFS-e: como fazer o download em lote", "NFS-e: encontre a sua nota e entenda a situação", "NFS-e: como filtrar as notas de serviço", "NFS-e: controle e ações em notas de serviço"]},
    {name:"Consultar regras de emiss\u00e3o por cidade (NFS-e)", arts:117, articles:["Notas fiscais de serviço no Padrão Nacional: O que muda até 2026.", "Padrão Siap", "Padrão E-ticons", "Padrão Public Soft", "Padrão SysISS", "Padrão SS INFORMATICA", "Padrão DSF", "Padrão NF-Eletrônica", "Padrão Elmar Tecnologia", "Padrão 3enet (Sinsoft)", "Padrão Gefisco", "Padrão Lençóis Paulista", "Padrão Semfaz (STM)", "Padrão Modernização Pública", "Padrão FISS-LEX", "Padrão EddyData", "Padrão Nota Natalense", "Padrão Nota Manaus", "Padrão Rio Branco", "Padrão Porto Alegre", "Padrão Futurize", "Padrão Giss Online", "Padrão Data Public - Upload", "Padrão Siam", "Padrão Cloud.El", "Padrão Dsfnet | Sorocaba (SP)", "Padrão Dsfnet | São Luís (MA)", "Padrão NFServiços", "Padrão eNotaFiscal", "Padrão IssMap", "Padrão Metrópolis", "Padrão Siat", "Padrão RLZ", "Padrão Primax Online", "Padrão ISS Digital (NFSD)", "Padrão Versa", "Padrão DigiFred", "Padrão Isaneto", "Padrão Cittá", "Padrão Supernova | ISS Digital", "Padrão Gespam", "Padrão FGMaiss", "Padrão FintelISS", "Padrão e-Caucaia", "Padrão Smarapd SIL Tecnologia WS", "Padrão Ativ", "Padrão Prefeitura Rápida", "Padrão São Paulo", "Padrão Tributos Municipais", "Padrão Smarapdsil WS2", "Padrão e-Receita", "Padrão iiBrasil", "Padrão Governo Digital (nfe-cidades)", "Padrão Awatar WS", "Padrão Síntese Tecnologia", "Padrão Memory", "Padrão D2Ti", "Padrão Prefeitura Moderna (Bauhaus)", "Padrão Megasoft", "Padrão ISS Fortaleza", "Padrão Ginfes", "Padrão Sigissweb", "Padrão Carioca", "Padrão Siappa", "Padrão GOVBR ISS Digital", "Padrão SpeedGov", "Padrão SH3", "Padrão Sigcorp", "Padrão Prescon", "Padrão ISS Intel", "Padrão BSIT-BR", "Padrão Governa (Publicenter)", "Padrão GeisWeb", "Padrão DB NFSe", "Padrão Coplan", "Padrão Centi", "Padrão Cecam", "Padrão ISSNFe Online", "Padrão WebISS 2", "Padrão Tinus", "Padrão Thema", "Padrão Tiplan", "Padrão Tecnos", "Padrão System", "Padrão SimplISS", "Padrão Siap.Net", "Padrão São José dos Pinhais", "Padrão Nota Salvador", "Padrão Saatri", "Padrão Pública", "Padrão Prodata", "Padrão Portal Fácil", "Padrão Freire", "Padrão Lexsom", "Padrão PMJP", "Padrão Assessor Público (ISS Online)", "Padrão ISS.net", "Padrão ISISS", "Padrão ISSe", "Padrão IPM", "Padrão Infisc", "Padrão Giap", "Padrão Fiorilli", "Padrão Equiplano", "Padrão Elotech", "Padrão E&L", "Padrão e-Governe", "Padrão Dsfnet", "Padrão DEISS", "Padrão Conam", "Padrão Betha 2.0", "Padrão Barueri Offline", "Padrão Aspec", "Padrão Ágili", "Padrão Abaco", "Padrão ISS4R", "Padrão Asten"]},
    {name:"Preencher nota fiscal de produto (NF-e)", arts:74, articles:["NF-e: perguntas frequentes sobre nota fiscal de produto", "NF-e: configuração da nota fiscal de produto", "NF-e: como calcular a base de cálculo do IBS e da CBS?", "NF-e: o que é o Código de classificação tributária do Padrão Nacional", "NF-e: como realizar o credenciamento da Conta Azul na Sefaz do Paraná", "NF-e: como realizar o credenciamento da Conta Azul na Sefaz de Minas Gerais", "NF-e: qual a situação tributária liberada por regime tributário", "Regras Fiscais: como visualizar a parametrização", "Regras Fiscais: como parametrizar os impostos no Regime Simples Nacional", "Regras Fiscais: como parametrizar os impostos no Regime Normal", "Regras Fiscais: como configurar o preenchimento automático de impostos", "NF-e: como validar Inscrição Estadual (IE)", "NF-e: o que é e como preencher Inscrição Estadual do Substituto Tributário (IE ST)", "NF-e: como a Inscrição Estadual do Substituto Tributário (IE ST) aparece na nota de produto?", "NF-e: o que é e como preencher Indicador de inscrição estadual", "NF-e: como preencher a nota fiscal de venda produto", "NF-e: como visualizar a DANFE antes da emissão", "NF-e: o que é o Código do Benefício Fiscal (cBenef)", "NF-e: o que são Origem da mercadoria e CSOSN", "NF-e: como preencher Origem da mercadoria e CSOSN", "NF-e: quais os campos de preenchimento para o 90 - Outras", "NF-e: quais os campos de preenchimento para o 70 - Com redução de Base e com ST", "NF-e: quais os campos de preenchimento para o 60 - ICMS cobrado anteriormente por ST", "NF-e: quais os campos de preenchimento para o 51 - Diferimento", "NF-e: quais os campos de preenchimento para o 50 - Suspensão", "NF-e: quais os campos de preenchimento para o 41 - Não tributada", "NF-e: quais os campos de preenchimento para o CST 40 - Isenta", "NF-e: quais os campos de preenchimento para o CST 30 - Isenta/não tributada e com ST", "NF-e: quais os campos de preenchimento para o CST 20 - Com redução de Base de Cálculo", "NF-e: quais os campos de preenchimento para o CST 10 - Tributada e com ST", "NF-e: quais os campos de preenchimento para o CST 00 - Tributada Integralmente", "NF-e: quais os campos de preenchimento para o CSOSN 900 - Outros", "NF-e: quais os campos de preenchimento para o CSOSN 500 - ICMS cobrado anteriormente por ST e antecipação", "NF-e: quais os campos de preenchimento para o CSOSN 400 - Não tributada", "NF-e: quais os campos de preenchimento para o CSOSN 300 - Imune", "NF-e: quais os campos de preenchimento para o CSOSN 203 - Isenção do ICMS com Substituição Tributária", "NF-e: quais os campos de preenchimento para o CSOSN 202 - Sem permissão de crédito com Substituição Tributária", "NF-e: quais os campos de preenchimento para o CSOSN 201 - Permissão de crédito com Substituição Tributária", "NF-e: quais os campos de preenchimento para o CSOSN 102 - Tributada sem permissão de crédito", "NF-e: quais os campos de preenchimento para o CSOSN 103 - Isenção no Simples Nacional (faixa de receita bruta)", "NF-e: quais os campos de preenchimento para o CSOSN 101 - Tributada com permissão de crédito", "NF-e: como declarar PIS e Cofins com CST 06 após a Lei Complementar nº 224/2025", "NF-e: perguntas frequentes sobre a Reforma Tributária", "NF-e: perguntas frequentes sobre como destacar impostos na nota de produto", "NF-e: o que é e como é calculado o Valor Aproximado dos Tributos", "NF-e: o que é e como configurar o Diferencial de alíquota (DIFAL)", "NF-e: o que é o cálculo DIFAL por dentro ou base dupla", "NF-e: o que é o cálculo DIFAL por fora ou base única", "NF-e: como verificar e calcular o valor do Diferencial de Alíquotas", "NF-e: checklist completo antes de transmitir nota interestadual com DIFAL", "NF-e: cliente tem inscrição estadual, mas é não contribuinte, como classificar na nota com DIFAL?", "NF-e: quando usar o Indicador de IE \"Contribuinte isento\" na nota com DIFAL?", "NF-e: o DIFAL não apareceu na nota, o que revisar?", "NF-e: impostos retidos para empresas de órgão público", "NF-e: como preencher o rascunho pela chave de acesso", "NF-e: como editar o número durante o preenchimento da nota", "NF-e: como informar o número do pedido de compra", "NF-e: como preencher dados da transportadora", "NF-e: como clonar nota fiscal de produto", "NF-e: o que é que como funciona o Fundo de Combate à Pobreza (FCP)", "NF-e: como informar a alíquota de FCP", "NF-e: como informar o ICMS e gerar a base de cálculo", "NF-e: como emitir com aproveitamento de crédito de ICMS", "NF-e: o que é e quem pode aproveitar o crédito de ICMS", "NF-e: quais CSTs usar e como preencher a alíquota para o aproveitamento de crédito de ICMS", "NF-e: como funcionam as tags de crédito de ICMS (pCredSN e vCredICMSSN) no XML", "NF-e: como excluir o ICMS da base de cálculo do PIS e COFINS", "NF-e: o que é e como funciona a Partilha ICMS", "NF-e: quem precisa aplicar a partilha de ICMS?", "NF-e: quando aparecem os campos de ICMS/FCP interestadual?", "NF-e: como preencher a NF-e para cliente com IE, mas não contribuinte de ICMS", "CT-e e MDF-e: o que é e como funciona", "NF-e: por que minha NF-e de entrega futura mostra CST no rascunho e CSOSN depois de emitir?", "NF-e: quando usar a opção Contribuinte isento?"]},
    {name:"Gerar emiss\u00e3o da nota de produto", arts:28, articles:["NF-e: como emitir nota fiscal de venda de produto", "NF-e: como emitir nota de produto complementar de Frete", "NF-e: como emitir nota de produto complementar de ICMS", "NF-e: como emitir nota de produto complementar de ICMS-ST", "NF-e: como emitir nota de produto complementar de IPI", "NF-e: como emitir nota de produto complementar de Valor", "NF-e: como emitir nota de compra ou entrada", "NF-e: como emitir nota de exportação", "NF-e: como emitir nota de importação", "NF-e: como emitir nota de produtor rural", "NF-e: como emitir nota fiscal de remessa", "NF-e: como emitir nota fiscal de retorno", "NF-e: como emitir nota fiscal de venda à ordem", "NF-e: como emitir nota fiscal de ativo imobilizado", "NF-e: como emitir nota fiscal de venda consignada", "NF-e: como emitir nota fiscal de industrialização", "NF-e: como emitir nota fiscal de venda para entrega futura", "NF-e: como emitir nota fiscal de venda presencial para cliente de outro estado", "NF-e: como emitir nota fiscal de venda Suframa", "NF-e: como emitir nota fiscal de remessa ou retorno para beneficiamento", "NF-e: como emitir nota fiscal em contingência", "NF-e: como emitir nota fiscal interestadual para consumidor final com diferencial de alíquotas", "NF-e: como emitir nota de produto com ICMS-ST para consumidor final contribuinte com diferencial de alíquotas", "NF-e: como acessar e fazer o download", "NF-e: como funciona a emissão de notas fiscais em contingência", "NF-e: como emitir nota de produto pelo emissor gratuito", "NF-e: como criar natureza de operação para nota fiscal de remessa e retorno", "NF-e: quais são as notas fiscais de produto emitidas na Conta Azul Pro"]},
    {name:"Emitir nota fiscal de devolu\u00e7\u00e3o", arts:7, articles:["Nota fiscal de devolução: perguntas frequentes sobre nota fiscal de devolução", "Nota fiscal de devolução: como emitir", "Nota fiscal de devolução: como emitir a nota de devolução parcial", "Nota fiscal de devolução: como emitir devolução parcial de uma nota fiscal de compra", "Nota fiscal de devolução: como emitir devolução parcial de uma nota de venda emitida pela Conta Azul", "Nota fiscal de devolução: como preencher o ICMS ST e IPI devolvidos na nota de devolução de compra", "NF-e: quais as diferenças entre a nota de retorno e a nota de devolução"]},
    {name:"Importar XML manualmente", arts:11, articles:["Importação manual do XML de compra: como fazer", "Importação manual do XML de compra: como importar pela chave de acesso", "Importação manual de NFS-e: consulte as cidades homologadas", "NFS-e: como importar XML", "NFS-e: como importar XML da NFS-e em lote manualmente", "NFS-e: quais os pré-requisitos para importação do XML da NFS-e", "NFS-e: como escriturar nota de serviço emitida fora da Conta Azul", "NFS-e: como editar NFS-e escriturada na Conta Azul", "NFS-e: como excluir NFS-e escriturada na Conta Azul", "NFS-e: como ler o XML da NFS-e e identificar as informações da nota?", "Busca automática de NF-e recebidas: como funciona"]},
    {name:"Configurar e emitir cupom fiscal (NFC-e)", arts:12, articles:["Frente de caixa (PDV): como configurar a emissão de NFC-e", "Frente de caixa (PDV): como gerar CSC", "Frente de caixa (PDV): como integrar com a maquininha da Stone", "Frente de caixa (PDV): como funciona a parceria da Stone com a Conta Azul", "Frente de caixa (PDV): como configurar a primeira integração com a maquininha da Stone", "Frente de caixa (PDV): como adicionar mais maquininhas da Stone", "Frente de caixa (PDV): o que fazer se a integração com a maquininha da Stone falha", "Frente de caixa (PDV): dados de pagamento eletrônico nas emissões de NFC-e", "Frente de caixa (PDV): como emitir NFC-e usando o SAT em São Paulo", "NFC-e: quais os aparelhos SAT aprovados para emitir NFC-e", "Frente de caixa (PDV): datas de registro e autorização da NFC-e", "Frente de caixa (PDV): o que precisa para emitir NFC-e"]},
    {name:"Corrigir, cancelar ou devolver nota fiscal", arts:12, articles:["Carta de Correção: como emitir na Conta Azul", "Carta de Correção: o que pode e não pode ser corrigido na Conta Azul", "Notas fiscais: quais os prazos para emissão e cancelamento de notas fiscais", "NFS-e: como fazer o cancelamento da nota de serviço", "NF-e: como corrigir nota já emitida", "NF-e: como excluir rascunho de nota fiscal de produto sem excluir a Venda", "NF-e: como cancelar nota fiscal de produto", "NF-e e NFC-e: como inutilizar nota fiscal de produto", "Frente de caixa (PDV): prazo de 30 minutos para cancelar NFC-e", "Frente de caixa (PDV): como cancelar NFC-e de venda feita com a maquininha da Stone", "Notas fiscais: ao cancelar a nota fiscal, a venda é excluída?", "NFS-e: por que não consigo emitir uma NFS-e depois de solicitar um cancelamento que ficou 'Aguardando Retorno'?"]},
    {name:"Solucionar rejei\u00e7\u00f5es de NFS-e", arts:288, articles:["Correção do erro Código: 1207", "Correção para o erro: Não Foi Possível Inferir o Anexo do Simples Nacional para Serviço 00.00 e CNAE 000", "Correção para o erro Código: EI88 e EI43", "Correção para o erro Código: E923", "Correção para o erro Código: P9020", "Correção para o erro Código: E606", "Correção do erro Código: S51", "Correção para o erro Código: RNG6110 Falha Schema Xml", "Correção do erro Código: E0390", "Correção para o erro Código: E284", "Correção para o erro Código: GW3001 Descrição: Codigo NBS não informado.", "Correção do erro Código: E0370", "Correção do erro Código: P5005", "Correção do erro Código: GW205", "Correção do erro Código: 218", "Correção para o erro Código E308", "Correção para o erro Código: E0713", "Correção para o erro Código: O tipo de retenção informada não confere com o esperado. Valor Informado: Nao. Valor esperado: Fixo", "Correção para o erro Código: P5008", "Correção para o erro Código: E0178", "Correção para o erro Código: E189 arquivo enviado com erro de certificado.", "Correção para o erro Código: E30", "Correção para o erro Código: RNG6110 O preenchimento do Código Indicador de Operação é obrigatório", "Correção para o erro Código: E0694", "Correção do erro Código: EO312", "Correção para o erro GW3000", "Correção para o erro Código: RNG6110 no SPED Falha Schema XML Mensagem: The element CEP is invalid", "Correção para o erro Código: E000 Descrição: O código do serviço municipal é inválido.", "Correção para o erro: O valor do CBS não pode ser nulo O valor do IBS Municipal não pode ser nulo O valor do IBS Estadual não pode ser nulo", "Correção para o erro Código: GOV105", "Correção para o erro Código: GOV103", "Correção para o erro Código: SCH1", "Correção para o erro Código: E001", "Correção para o erro Código: GWNBS001", "Correção para o erro Código: GW304", "Correção para o erro Código: L115", "Correção para o erro Código: P5004", "Correção para o erro Código: E0931", "Correção para o erro Código: E547", "Correção para o erro Código: 00383", "Correção para o erro Código: X351", "Correção para o erro Código: L1037", "Correção para o erro Código: E365", "Correção para o erro Código: E358", "Correção para o erro Código: E351", "Correção para o erro Código: E293", "Correção para o erro Código: E146", "Correção para o erro Código: E162 Descrição: A partir de 01/01/2026 todas as empresas do seu município estão obrigadas a emitir as NFS-e no Padrão Nacional.", "Correção para o erro Código: 301", "Correção para o erro Código: RNG6110 Descrição: A natureza de operação preenchida não é compatível com as emissões no Padrão Nacional.", "Correção para o erro Código: RNG6110 Descrição: O Regime Especial de Tributação está incorreto no Padrão Nacional", "Correção para o erro Código: E0588", "Correção para o erro Código: GW3001 Descrição: A série do RPS configurada deve conter apenas números e estar no intervalo de 1 a 49.999.", "Correção para o erro Código: E0840", "Correção para o erro Código: E0619", "Correção para o erro Código: L20", "Correção para o erro Código: E0314", "Correção para o erro Código: E0240", "Correção para o erro Código: E0166", "Correção para o erro Código: E0712", "Correção para o erro Código: E0312", "Correção para o erro Código: E0690", "Correção para o erro Código: 221", "Correção para o erro Código: E999 — Erro nao catalogado", "Correção para o erro Código: RNG9997 A natureza de operação preenchida não é compatível com as emissões no Padrão Nacional", "Correção para o erro Código: E0523", "Correção para o erro Código: E0304", "Correção para o erro Código: E0160", "Correção para o erro Código: E0014", "Correção para o erro Código: E1010", "Correção para o erro Código: L84", "Correção para o erro Código: L66", "Correção para o erro Código: E0120", "Correção para o erro Código: E0604", "Correção para o erro Código: RNG9997 O Regime Especial de Tributação está incorreto no Padrão Nacional", "Correção para o erro Código: E0116", "Correção para o erro: A série informada é incorreta. No Padrão Nacional, a série deve conter apenas números", "Correção para o erro Código: E1011", "Correção para o erro Código: RNG9997 O código NBS foi preenchido indevidamente", "Correção para o erro Código: E0600", "Correção para o erro Código: E0634", "Correção para o erro Código: E0688", "Correção para o erro Código: E0032", "Correção para o erro Código: L64", "Correção para o erro Código: E0560", "Correção para o erro Código: RNG9997 A série informada não pertence a faixa definida", "Correção para o erro Código: L069", "Correção para o erro Código: E350", "Correção para o erro Código: E0061", "Correção para o erro Código: 783942073", "Correção para o erro Código: _Cert002", "Correção para o erro Código: E152", "Correção para o erro Código: L156 Aliquota invalida", "Correção para o erro Código: E0625", "Correção para o erro Código: E051", "Correção para o erro Código: E0010 A série informada não pertence a faixa definida para a empresa para emissão da NFS-e no Padrão Nacional", "Correção para o erro Código: E961", "Correção para o erro Código: L999 Configuração da atividade não encontrada ou finalizada pela Prefeitura", "Correção para o erro Código: E138, cancelamento de nota", "Correção para o erro Código: L145", "Correção para o erro Código: E36", "Correção para o erro Código: E000", "Correção para o erro Código: E337", "Correção para o erro Código: E0615", "Correção para o erro Código: 1304", "Correção para o erro Código: L126", "Correção para o erro Código: 311", "Correção para o erro Código: EL0", "Correção para o erro Código: L130", "Correção para o erro Código: 288812695", "Correção para o erro Código: 5500", "Correção para o erro Código: L1", "Correção para o erro código: L000", "Correção para o erro Código: 32", "Correção para o erro Código: 002", "Correção para o erro Código: E174", "Correção para o erro Código: E361", "Correção para o erro Código: E353 NIF do Tomador nao informado", "Correção para o erro Código: E119", "Correção para o erro Código: P68", "Correção para o erro Código: E999 Exigibilidade Aceita apenas 1", "Correção para o erro Código: E548", "Correção para o erro Código: 0", "Correção para o erro: Não foi possível autenticar com as credenciais fornecidas", "Correção para o erro Código: E900", "Correção para o erro: Em Conflito - [#inv0368]", "Correção para o erro Código: E939", "Correção para o erro Código: E0310", "Correção para o erro Códigos: 1206 e 1204", "Correção para o erro Código: 00282", "Correção para o erro Código: E43 Inscricao Municipal/Seq/Dv do prestador", "Correção para o erro Código: E0611", "Correção para o erro Código: E1003", "Correção para o erro Código: 00034", "Correção para o erro Código: E0592", "Correção para o erro Código: E0060", "Correção para o erro Código: 1916692291", "Correção para o erro Código: 00135", "Correção para o erro Código: A1", "Correção para o erro Código: E110", "Correção para o erro Código: L52", "Correção para o erro Códigos: E26 e E241", "Correção para o erro Código: E930", "Correção para o erro Código: E156", "Correção para o erro Código: E0081", "Correção para o erro Código: E0330", "Correção para o erro Código: E138, Mensagem: Usuario nao autorizado a realizar o servico", "Correção para o erro Código: Erro Descrição: Usuário/Contribuinte Não Identificado", "Correção para o erro Código: 00132", "Correção para o erro Código: E179", "Correção para o erro Código: L999 Descrição: DMS FECHADA", "Correção para o erro Código: 1, Mensagem: O RPS __ ja existe para este contribuinte", "Correção para o erro Código: 335 Descrição: Numero do RPS deve ser subsequente ao anterior enviado.", "Correção para o erro Código: 335 Descrição: Código não vigente na data da prestação do serviço", "Correção para o erro: O Usuario Informado no Web Services e Diferente do Usuario Informado no Arquivo XML", "Correção para o erro Código: A01", "Correção para o erro Código: 302", "Correção para o erro Código: E053", "Correção para o erro Código: 1, Mensagem: E#13", "Correção para o erro Código: E221", "Correção para o erro Código: EF001", "Correção para o erro Código: E329", "Correção para o erro Código: E280", "Correção para o erro Código: 1433", "Correção para o erro Código: 00031", "Correção para o erro Código: E199", "Correção para o erro Código: L156", "Correção para o erro Código: 306", "Correção para o erro Código: GW00345", "Correção para o erro Código: 1453", "Correção para o erro Código: 00062", "Correção para o erro Código: GW3346", "Correção para o erro Código: L1014", "Correção para o erro Código: EL244", "Correção para o erro Código: 1441", "Correção para o erro: Serviço prestado é obrigatório", "Correção para o erro Código: 1004133944", "Correção para o erro: Série/RPS vinculada a outra Nota Fiscal", "Correção para o erro: RPS já em uso por outra Venda", "Correção para o erro Código: 473251413", "Correção para o erro Código: GW129", "Correção para o erro Código: 00183", "Correção para o erro Código: L43", "Correção para o erro Código: E188", "Correção para o erro Código: E220", "Correção para o erro Código: 309", "Correção para o erro Código: 1254", "Correção para o erro Código: E039", "Correção para o erro: O Código do Serviço Municipal deve conter 5 dígitos (ex: 01.08, 10.02", "NFS-e | Erro: Ocorreu erro na primeira emissão de NFS-e em São Paulo", "NFS-e | Erro: soluções para erros nas notas de serviço do Distrito Federal", "Correção para o erro: A inscrição municipal do cliente deve conter no máximo 15 caracteres.", "Correção para o erro: Os valores de dedução de ISS e dedução de INSS devem ser iguais", "Correção para o erro Código: E029", "Correção para o erro Código: E043", "Correção para o erro Código: E090", "Correção para o erro Código: E227", "Correção para o erro Código: E228", "Correção para o erro Código: E328", "Correção para o erro Código: E927", "Correção para o erro Código: L003", "Correção para o erro Código: D303", "Correção para o erro Código: 00000 Numero do RPS esta fora da sequencia", "Correção para o erro Código: 301 Senha Invalida", "Correção para o erro Código: E138 Descrição: CNPJ não autorizado a realizar o serviço", "Correção para o erro Código: E52", "Correção para o erro Código: E941", "Correção para o erro Código: 00158", "Correção para o erro Código: 8002", "Correção para o erro Código: Error1310", "Correção para o erro Código: S10", "Correção para o erro: O certificado digital da empresa está vencido", "Correção para o erro Código: E353 Servico nao permitido", "Correção para o erro Código: 301 Login e/ou Senha invalido(s)", "Correção para o erro Código: E222", "Correção para o erro Código: E004", "Correção para o erro Código: 00229", "Correção para o erro Código: 1001", "Correção para o erro: O tomador informado é um órgão público", "Correção para o erro Código: 1465", "Correção para o erro Código: E162 Descrição: Alíquota do simples nacional incorreta. Correção: Informe um percentual de acordo com o enquadramento na tabela de alíquota do simples nacional", "Correção para o erro Código: E97", "Correção para o erro Código: E283", "Correção para o erro Código: E193", "Correção para o erro Código: E166", "Correção para o erro Código: L42", "Correção para o erro Código: E197", "Correção para o erro Código: E194", "Correção para o erro Código: 00210", "Correção para o erro Código: 00060", "Correção para o erro Código: EC93", "Correção para o erro Código: LIB-E010 ERRO! ALÍQUOTA INFORMADA INVÁLIDA!", "Correção para o erro Código: 1 Descrição: A retenção informada N não é válida.", "Correção para o erro Código: 000394", "Correção para o erro Código: E50", "Correção para o erro Código: E821 e E991", "Correção para o erro Códigos: E0010 e E0014", "Correção para o erro: ATENÇÃO! AMBIENTE DE TESTE PARA VALIDAÇÃO DE INTEGRAÇÃO", "Correção para o erro: Campo ISSRetido informado incorretamente, observar legislação municipal.", "Correção para o erro Código: E51", "Correção para o erro Código: 1497", "Correção para o erro Código: 301 Erro de validacao do XSD", "Correção para o erro: Versão do modelo ABRASF indicada não esta vigente.", "Correção para o erro Código: E10", "Correção para o erro Código: E175", "Correção para o erro Código: 0000 Descrição: <Erro>RPS_001_001", "Correção para o erro Código: E33", "Correção para o erro Código: 00221", "Correção para o erro Código: CE103", "Correção para o erro Código: 401 - Acesso Negado!", "Correção para o erro Código: E093", "Correção para o erro Código: L29", "Correção para o erro Código: Error3450", "Correção para o erro Código: L018", "Correção para o erro Código: E327", "Correção para o erro: Inscrição Municipal não encontrada na base de dados da PMB", "Correção para o erro Código: E347", "Correção para o erro Código: L67", "Correção para o erro Código: 1 Descrição: A retenção informada S não é válida", "Correção para o erro Código: L001", "Correção para o erro Código: E145", "Correção para o erro Código: E204", "Correção para o erro Código: L060", "Correção para o erro Código: E158", "Correção para o erro: A empresa já está cadastrada no sistema", "Correção para o erro Código: E233", "Correção para o erro Códigos: EL01 e EL56", "Correção para o erro Código: E198", "Correção para o erro Código: E160", "Correção para o erro Código: E43 Inscricao Municipal do prestador do servico nao encontrada na base de dados do municipio", "Correção para o erro Código: E186", "Correção para o erro Código: E517", "Correção para o erro Código: E45", "Correção para o erro Código: 1202", "Correção para o erro Código: 1107", "Correção para o erro Código: E142", "Correção para o erro Código: E90", "Correção para o erro Código: L016", "Correção para o erro Código: 394", "Correção para o erro Código: E500", "Correção para o erro: O certificado digital informado está expirado.", "Correção para o erro Código: L999 Descrição: E33", "Correção para o erro Código: 0000 Descrição: ERRO VALIDACAO XSD: (400)", "Correção para o erro Código: GW010", "Correção para o erro Código: 00017", "Correção para o erro Código: L044", "Correção para o erro Código: P123", "Correção para o erro Código: E35 Descrição: Código de tributação inexistente"]},
    {name:"Solucionar rejei\u00e7\u00f5es de NF-e", arts:96, articles:["Correção para NF-e: CFOP de Importação e não informado dados da DI [nItem: 1]", "NF-e | Erro: regime tributário diferente do SINTEGRA | o que fazer se o erro continuar", "NF-e | Erro: regime tributário diferente do SINTEGRA | atualize os dados da empresa na Conta Azul", "NF-e | Erro: regime tributário diferente do SINTEGRA | verifique alteração na Receita Federal", "NF-e | Erro: CNPJ emitente não cadastrado", "NF-e | Erro: CFOP inválido para emitente MEI", "NF-e | Erro: A Inscrição Estadual do seu cliente é inválida.", "Correção para NF-e: Não informada Guia de Trânsito Animal", "Correção para NF-e: O código do benefício deve ter 8 ou 10 caracteres", "Correção para NF-e: CST com benefício fiscal e não informado o código de benefício fiscal", "Correção para NF-e: Rejeição: Grupo de Ajuste de Competência não informado", "NFC-e: quais os estados homologados e suas particularidades para emissão de NFC-e", "Correção para NF: cvc-complex-type.2.4.b", "Correção para NF: 231", "Correção para NF-e: CFOP inválido para emitente MEI", "NF-e | Erro: CSOSN inválido para emitente MEI", "Correção para NF-e: Irregularidade fiscal do destinatário", "Correção para NF-e: ICMS partilha FCP - UF remetente não pode ser vazio", "Correção para NF-e: Informado CST para emissor do Simples Nacional (CRT=1 ou 4)", "NF-e | Erro: Resolução de possíveis erros de Inscrição Estadual", "Correção para NF-e: additionalInformationTaxPayer tamanho deve estar entre 0 e 5000", "Correção para NF-e: Falha ao inutilizar!", "Correção para NF-e: Duplicidade de NF-e, com diferença na Chave de Acesso", "Correção para NF-e: CPF do Emitente difere do CPF do Certificado Digital", "Correção para NF-e: foreignTrade.uf não pode ser nulo", "Correção para NF-e: A Inscrição Estadual do seu cliente é inválida.", "Correção para NF-e: Você precisa especificar a \"Forma de Pagamento\".", "Correção para NF-e: Alíquota do ICMS com valor superior a 4 por cento", "Correção para NF-e: As definições de \"ICMS\" estão incompletas", "Correção para NF-e: É preciso informar a \"Chave de acesso da Nota Fiscal\" que está sendo devolvida", "Correção para NF-e: Complemento do endereço deve ter no máximo 60 caracteres", "Correção para NF-e: Falta informar Emitente não pode ser ISENTO", "NF-e | Erro: Falta informar Inscrição Estadual", "Correção para NF-e: Falta informar CFOP válido para dentro do estado", "Correção para NF-e: NF-e de devolucao com valor total superior a NF-e devolvida", "NF-e | Erro: Sua empresa não está habilitada para emissão de NF-e", "Correção para NF-e: NF-e de devolucao com valor do ICMS superior a NF-e devolvida", "Correção para NF-e: Informado indevidamente campo valor de pagamento", "Correção para NF-e: Falta informar CFOP válido para fora do estado", "Correção para NF-e: Alíquota de ICMS superior a definida para a operação interestadual", "Correção para NF-e: Falta informar Unidade", "Correção para NF-e: CNPJ Destinatário não cadastrado", "Correção para NF-e: Evento não atende o Schema XML específico", "Correção para NF-e: O CNPJ do seu cliente não é válido", "Correção para NF-e: '{\"http://www.portalfiscal.inf.br/nfe\":CRT}'", "Correção para NF-e: Valor do Produto difere do produto Valor Unitario de Comercializacao e Quantidade Comercial", "Correção para NF-e: Campo 'Valor de COFINS' do produto obrigatório", "Correção para NF-e: Lote não encontrado", "Correção para NF-e: Falha no Esquema XML", "Correção para NF-e: Não foi possível atualizar a venda com o valor da nota fiscal pois a venda já possui parcelas pagas", "Correção para NF-e: Informado um NCM que não é válido", "Correção para NF-e: Nao informado Nota Fiscal referenciada (CFOP de Exportação Indireta)", "Correção para NF-e: Campo 'Modalidade de Subs. Tributária de ICMS' do produto obrigatório", "Correção para NF-e: '{\"http://www.portalfiscal.inf.br/nfe\":pST}'", "Correção para NF-e: Percentual de FCP igual a zero", "Correção para NF-e: cvc-pattern-valid 'TSerie'", "Correção para NF-e: Certificado Assinatura Data Validade", "Correção para NF-e: Confirmado o recebimento da NF-e pelo destinatário", "Correção para NF-e: '#AnonType_xPedproddetinfNFeTNFe", "Correção para NF-e: Preencha no cadastro de seu cliente o campo Inscrição Estadual", "Correção para NF-e: Duplicidade de NF-e [nRec: ]", "Correção para NF-e: IE do destinatário não vinculada ao CNPJ", "Correção para NF-e: IE do destinatário não cadastrada", "Correção para NF-e: IE do destinatário não está ativa na UF", "Correção para NF-e: IE Destinatário não vinculada ao CPF", "Correção para NF-e: NF-e sem tag IE do destinatário", "Correção para NF-e: Total do FCP difere do somatório dos itens", "Correção para NF-e: CFOP inválido para Nota Fiscal com finalidade de devolução de mercadoria", "Correção para NF-e: Unidade Tributável incompatível com o NCM informado", "NF-e | Erro: Selecione outra opção no campo ‘Situação Tributária (CSOSN)’", "Correção para NF-e: Número da parcela inválido ou não informado", "Correção para NF-e: Percentual de FCP ST igual a zero", "Correção para NF-e: Certificado Assinatura - Cadeia de Certificação", "Correção para NF-e: Informado indevidamente o grupo de ICMS para a UF de destino", "Correção para NF-e: Falha no reconhecimento da autoria ou integridade do arquivo digital", "Correção para NF-e: Soma do valor das parcelas difere do Valor Liquido da Fatura", "Correção para NF-e: NF-e já está inutilizada na Base de dados da SEFAZ", "Correção para NF-e: CNPJ do emitente inválido", "Correção para NF-e: CNPJ emitente não cadastrado", "Correção para NF-e: Data de Emissão muito atrasada. Anterior a 10 dias", "Correção para NF-e: Informado CSOSN para emissor que não é do Simples Nacional", "Correção para NF-e: Erro de comunicação com a SEFAZ", "Correção para NF-e: Esta Nota fiscal pode já ter sido emitida", "Correção para NF-e: CFOP de devolução de mercadoria para NF-e que não tem finalidade de devolução de mercadoria", "Correção para NF-e: Data de Emissão anterior a data de credenciamento", "Correção para NF-e: Certificado Assinatura revogado", "Correção para NF-e: Erro na Chave de Acesso", "Correção para NF-e: Data de Emissão muito atrasada", "NF-e | Erro: Código de ST do IPI incompatível com o Código de Enquadramento", "Correção para NF-e: CFOP de saída para NF-e de entrada", "Correção para NF-e: Operação com ICMS-ST sem informação do CEST", "Correção para NF-e: CFOP Inexistente", "NF-e | Erro: O regime tributário selecionado na tela de Dados da Empresa é diferente do cadastrado no SINTEGRA", "Correção para NF-e: Altere o campo 'Nota para consumidor final' para 'Sim'", "Correção para NF-e: Esta nota está fora do prazo aceito para o cancelamento", "Correção para NF-e: Há um CT-e ou MDF-e vinculado a esta nota"]},
    {name:"Solucionar rejei\u00e7\u00f5es de NFC-e", arts:9, articles:["Correção para NFC-e: 539 - Rejeicao: Duplicidade de NF-e, com diferença na Chave de Acesso", "Correção para NFC-e: CFOP inválido para emitente MEI", "Correção para NFC-e: CSOSN inválido para emitente MEI", "Correção para NFC-e: 778 - Rejeição: Informado NCM inexistente", "Correção para NFC-e: Valor total superior ao permitido para destinatário não identificado", "Correção para NFC-e: _Cert002", "Correção para NFC-e: Codigo de Hash no QR-Code difere do calculado (464)", "Correção para NFC-e: Emissor nao habilitado para emissao", "Correção para NFC-e: Falha de comunicação com o governo"]},
    {name:"Buscar e transmitir notas fiscais automaticamente", arts:24, articles:["Busca automática de NFS-e: como gerar ou vincular à venda", "Busca automática de NF-e recebidas: como configurar", "Busca automática de NF-e recebidas: confirmar ou recusar compra", "Busca automática de NFS-e prestados: o que é e como funciona", "Compra de produto: vincular ou gerar compra na importação da nota fiscal", "Busca automática de NFS-e prestados: como fazer", "Busca automática de NFS-e tomados: como fazer", "Busca automática de NF-e recebidas: não está buscando as minhas notas de compra automaticamente. E agora?", "Busca automática de NFS-e prestados: consulte as cidades homologadas", "Busca automática de NF-e recebidas | Erro: 656 - Consumo Indevido", "Busca automática de NFS-e tomados: consulte as cidades homologadas", "Busca automática de NF-e recebidas | Erro: O código do produto está duplicado", "Importação de XML de NFS-e: como criar venda automática", "Busca automática: até quando a Conta Azul busca minhas notas já emitidas?", "Busca automática de NF-e recebidas: o que verificar quando as notas não são importadas automaticamente?", "Conta Azul Mais: o que é e como fazer a Integração Fiscal", "Conta Azul Mais: como adicionar clientes por planilha na Integração Fiscal", "Conta Azul Mais: como adicionar clientes individualmente na Integração Fiscal", "Conta Azul Mais: como configurar a busca automática de notas fiscais na Integração Fiscal", "Conta Azul Mais: como configurar a transmissão automática de notas fiscais na Integração Fiscal", "Alterdata e Conta Azul: como configurar a transmissão automática de notas fiscais", "SCI e Conta Azul: como configurar a transmissão automática de notas fiscais", "SCI e Conta Azul: como configurar e obter o código na Conta Azul Mais", "SCI e Conta Azul: como importar notas fiscais da Conta Azul"]},
  ]},
  { id:"p5", name:"Acompanhar despesas e pagamentos", color:"#8b5e2a", sections:[
    {name:"Lan\u00e7ar despesas fixas, recorrentes e parceladas", arts:26, articles:["Conta a pagar: como lançar", "Conta a pagar: como lançar despesa recorrente", "Conta a pagar: como editar despesa recorrente", "Conta a pagar: como criar mais recorrências em uma conta a pagar?", "Conta a pagar: como excluir as recorrências de uma conta a pagar?", "Conta a pagar: quais campos posso editar em uma despesa recorrente?", "Conta a pagar: como a Data de vencimento afeta as próximas parcelas na edição de uma despesa recorrente?", "Conta a pagar: como lançar despesa parcelada", "Conta a pagar: como editar despesa parcelada", "Conta a pagar: como lançar despesa à vista", "Conta a pagar: como renegociar valores em aberto", "Financeiro: é possível gerar arquivo CNAB no contas a pagar?", "Financeiro: a Conta Azul Pro faz split de pagamento automático?", "Financeiro: como agendar pagamentos Pix corretamente para datas futuras?", "Financeiro: como registrar despesas com múltiplos meios de pagamento?", "Financeiro: como realizar pagamentos em lote via PIX, incluindo o envio de arquivos?", "Financeiro: como registrar o pagamento de DAE no sistema quando a opção específica não está disponível?", "Financeiro: existe limite de tamanho ou diário para importar pagamentos?", "Financeiro: como verificar comprovantes de pagamento duplicados ou divergentes?", "Contas a pagar: despesa gerada automaticamente pelo parceiro", "Compras: como lançar compra de serviços tomados", "Compras: o que preencher em Informações da compra na Compra de serviços tomados?", "Compras: o que preencher em Informações da nota na Compra de serviços tomados?", "Compras: o que preencher em Retenções na Compra de serviços tomados?", "Compras: o que preencher em Informações de pagamento na Compra de serviços tomados?", "Compras: como lançar compra de aluguel"]},
    {name:"Lan\u00e7ar contas a pagar e a receber", arts:11, articles:["Lançamentos financeiros: como importar planilha com entradas e saídas", "Lançamentos financeiros: como lançar transferência entre contas", "Lançamentos financeiros: como gerenciar empréstimos", "Lançamentos financeiros: como lançar um empréstimo recebido", "Lançamentos financeiros: como informar o pagamento do empréstimo", "Lançamentos financeiros: como criar lançamentos financeiros?", "Lançamentos financeiros: como clonar", "Lançamentos recorrentes: como criar no Contas a Pagar e Contas a Receber", "Lançamentos recorrentes: como é calculada a data de competência de cada ocorrência?", "Lançamentos financeiros: por que Categoria e Centro de custo mudam ao selecionar o contato?", "Conta Azul de Bolso: como funciona o Financeiro no app"]},
    {name:"Controlar contas a pagar e a receber", arts:10, articles:["Lançamentos financeiros: como editar", "Lançamentos financeiros: como editar em lote", "Lançamentos financeiros: como adicionar anexos", "Lançamentos financeiros: como funciona a descrição e a identificação visual dos anexos", "Lançamentos financeiros: como consultar o histórico de edições do anexo", "Lançamentos financeiros: como funciona a data de previsão", "Lançamentos financeiros: o que é e para que serve o Número Sequencial Único (NSU)", "Lançamentos financeiros: como incluir o código NSU", "Lançamentos financeiros: quais são os relatórios de lançamentos financeiros?", "Lançamentos financeiros: como editar lançamentos financeiros?"]},
    {name:"Controlar gastos e pagamentos de cart\u00e3o de cr\u00e9dito", arts:10, articles:["Conta Cartão de Crédito: como cadastrar", "Conta Cartão de Crédito: como lançar e conciliar o pagamento da fatura", "Conta Cartão de Crédito: como informar o pagamento da fatura na conta cartão de crédito?", "Conta Cartão de Crédito: como conciliar o pagamento da fatura com o extrato bancário?", "Conta Cartão de Crédito: como conciliar quando a fatura é paga parcialmente?", "Conta Cartão de Crédito: como posso gerenciar e conciliar meu cartão de crédito pré-pago?", "Conta Cartão de Crédito: como registrar pagamento antecipado da fatura do cartão de crédito?", "Conta Cartão de Crédito: como lançar compras", "Conta Cartão de Crédito: como lançar estorno de compras", "Conta Cartão de Crédito: como ver a fatura"]},
    {name:"Confirmar recebimentos e pagamentos", arts:14, articles:["Lançamentos financeiros: como fazer baixa parcial", "Lançamentos financeiros: como restaurar um lançamento marcado como perda", "Lançamentos financeiros: como visualizar lançamentos salvos como perda", "Lançamentos financeiros: como aplicar juros, multa, desconto e tarifa", "Lançamentos financeiros: como informar pagamento e recebimento adiantado", "Lançamentos financeiros: agendar pagamento", "Lançamentos financeiros: salvar como perda", "Lançamentos financeiros: como os marcados como perda aparecem nos relatórios", "Lançamentos financeiros: como excluir", "Lançamentos financeiros: como excluir em lote", "Lançamentos financeiros: quais os requisitos para excluir em lote?", "Lançamentos financeiros: como excluir em lote pela tela de Visão de competência", "Lançamentos financeiros: como excluir em lote pelas telas de Contas a pagar ou Contas a receber", "Lançamentos financeiros: como excluir lançamentos financeiros?"]},
    {name:"Filtrar buscas de lan\u00e7amentos financeiros", arts:9, articles:["Lançamentos financeiros: como filtrar", "Lançamentos financeiros: como filtrar por período", "Lançamentos financeiros: como filtrar o que está em aberto", "Lançamentos financeiros: como filtrar o que está em aberto na tela de Extrato de movimentações", "Lançamentos financeiros: como filtrar o que está em aberto na tela de Contas a receber", "Lançamentos financeiros: como filtrar o que está em aberto na tela de Contas a pagar", "Lançamentos financeiros: como funciona a visão de lançamentos por página", "Lançamentos financeiros: como personalizar as colunas nas telas do Financeiro", "Lançamentos financeiros: como aplicar filtros nos lançamentos financeiros?"]},
    {name:"Esclarecer d\u00favidas gerais do financeiro", arts:25, articles:["Lançamentos financeiros: perguntas frequentes", "Lançamentos financeiros: diretório de passo a passo financeiro", "Financeiro: por que a opção \"Excluir\" não aparece no financeiro?", "Financeiro: como lançar um imposto retido no financeiro da Conta Azul?", "Financeiro: como parcelar valores e pagamentos em aberto?", "Financeiro: como processar pagamentos com erro de limite?", "Financeiro: como encontrar lançamentos financeiros ocultos no sistema?", "Financeiro: como configurar despesas recorrentes duas vezes por semana?", "Financeiro: como obter comprovante de pagamento?", "Financeiro: como gerar comprovante para entrada de Saldo para IP?", "Financeiro: como identificar usuário e horário de lançamentos financeiros?", "Financeiro: como registrar um lançamento com múltiplos meios de pagamento?", "Financeiro: como registrar despesas com parcelamento além do limite do sistema?", "Financeiro: como evitar que transferências entre contas afetem o saldo da receita?", "Financeiro: como lançar uma despesa como recorrente?", "Financeiro: como provisionar despesas anuais sem lançar individualmente?", "Financeiro: qual a diferença entre provisão e baixa financeira na contabilização?", "Financeiro: por que minhas receitas e despesas não batem?", "Financeiro: como gerenciar empréstimos sem poluir o financeiro?", "Financeiro: como configurar o Controle de movimentações de caixa?", "Financeiro: o sistema calculou o valor das parcelas errado. O que fazer?", "Financeiro: é possível restaurar receitas ou despesas excluídas?", "Financeiro: onde encontro o lançamento recuperado do estorno?", "Financeiro: como configurar condição de pagamento na Conta Azul?", "Financeiro: por que aparecem lançamentos recuperados duplicados?"]},
    {name:"Configurar categorias e centros de custo", arts:22, articles:["Categorias financeiras: o que é e como cadastrar", "Centro de custo: o que é e como cadastrar", "Categoria financeira e centro de custo: como fazer o rateio de centro de custo e categoria", "Categorias financeiras: como entender as categorias e subcategorias", "Categoria financeira e centro de custo: edição no lançamento financeiro", "Categoria financeira: como excluir", "Financeiro: como classificar reembolso de receita como redutor de despesa?", "Financeiro: como fazer o rateio de centro de custo de uma despesa?", "Financeiro: como categorizar múltiplos itens em uma única despesa?", "Financeiro: como excluir categorias financeiras existentes?", "Financeiro: como ratear a mesma categoria entre múltiplos centros de custo?", "Financeiro: como utilizar centros de custo e rateio para orçamento e alocação de despesas?", "Financeiro: como fazer rateio entre categorias de receita e despesa?", "Financeiro: como cadastrar categorias e subcategorias financeiras?", "Financeiro: como criar uma categoria de custos?", "Financeiro: como alterar a categoria de valores recebidos em lote?", "Financeiro: como reclassificar lançamentos entre categorias no sistema, incluindo a opção em lote?", "Financeiro: como migrar lançamentos entre categorias financeiras?", "Financeiro: como classificar transferências bancárias sem contas visíveis?", "Financeiro: como gerenciar centros de custo entre empresas do grupo?", "Financeiro: por que a função de associar categorias financeiras não aparece na minha conta?", "Financeiro: como remover categorias financeiras duplicadas?"]},
    {name:"Auditar lan\u00e7amentos realizados por usu\u00e1rios", arts:2, articles:["Histórico financeiro: o que é e como funciona", "Histórico financeiro: entenda como as ações são registradas"]},
  ]},
  { id:"p6", name:"Automatizar lan\u00e7amentos com Conta AI Captura", color:"#2a7a8a", sections:[
    {name:"Agilizar o recebimento de documentos com IA", arts:9, articles:["Conta Azul Mais Conta AI Captura: como funciona", "Conta Azul Mais | Conta AI Captura: quais são as integrações disponíveis para envio de arquivos", "Conta Azul Mais | Conta AI Captura: como configurar o envio de documentos via WhatsApp", "Conta Azul Mais | Conta AI Captura: como enviar documentos via WhatsApp", "Conta Azul Mais | Conta AI Captura: como criar Grupo de WhatsApp", "Conta Azul Mais | Conta AI Captura: como configurar o envio de documentos via e-mail", "Conta Azul Mais | Conta AI Captura: como enviar documentos via e-mail", "Conta Azul Mais | Conta AI Captura: como integrar com DDA", "Conta Azul Mais | Conta AI Captura: como alterar o número de WhatsApp para envio de documentos?"]},
    {name:"Otimizar o uso da Conta AI Captura", arts:7, articles:["Conta AI Captura: como funciona", "Conta AI Captura: o encaminhamento automático de e-mails funciona?", "Conta AI Captura: documento enviado por e-mail não chegou, o que fazer?", "Conta AI Captura: quais documentos e formatos de arquivo posso importar?", "Conta AI Captura: posso usar o mesmo WhatsApp em mais de uma empresa?", "Conta AI Captura: documento enviado pelo WhatsApp não aparece na Conta Azul, o que fazer?", "Conta AI Captura: a Conta AI Captura identifica lançamentos manuais duplicados automaticamente?"]},
    {name:"Habilitar ferramentas \u00e0 Conta AI Captura", arts:6, articles:["Conta AI Captura: quais são as integrações disponíveis para envio de arquivos", "Conta AI Captura: como configurar o envio de documentos via WhatsApp para a Conta AI Captura?", "Conta AI Captura: como configurar o envio de documentos via e-mail para a Conta AI Captura?", "Conta AI Captura: como integrar com DDA", "Conta AI Captura: como ativar o DDA no Conta AI Captura?", "Conta AI Captura: como desvincular e vincular um número de telefone a outra conta?"]},
    {name:"Controlar o fluxo de documentos enviados", arts:8, articles:["Conta AI Captura: como enviar documentos via e-mail para a Conta AI Captura?", "Conta AI Captura: como enviar documentos via WhatsApp para a Conta AI Captura?", "Conta AI Captura: como criar Grupo de WhatsApp para a Conta AI Captura?", "Conta AI Captura: como revisar e vincular boletos DDA importados via Conta AI Captura?", "Conta AI Captura: como revisar e editar lançamentos importados", "Conta AI Captura: como transformar receita em despesa (ou vice-versa)", "Conta AI Captura: como visualizar lançamentos importados no Financeiro", "Conta AI Captura: como acompanhar documentos importados e pendentes"]},
  ]},
  { id:"p7", name:"Otimizar cobran\u00e7as com a Conta PJ", color:"#2e6b8a", sections:[
    {name:"Ativar minha Conta PJ Conta Azul", arts:18, articles:["Conta PJ da ContaAzul IP: como fazer a validação de identidade?", "Conta PJ da ContaAzul IP: como ativar e cadastrar a senha de transação", "Conta PJ da ContaAzul IP: como ativar com procuração com poderes financeiros", "Conta PJ da ContaAzul IP: como funciona a autenticação facial", "Boleto da Conta Azul: como cadastrar", "Pix cobrança da Conta Azul: como cadastrar", "Cartão de crédito via link da Conta Azul: como cadastrar", "Cartão de crédito via link da Conta Azul: configuração de parcelamento", "Cobranças da Conta Azul: como ativar o serviço adicional de Descontos nas cobranças", "Conta PJ da ContaAzul IP: como alterar a senha de transação", "Conta PJ da ContaAzul IP: como funciona a verificação no Banco Central Protege+", "Conta PJ da ContaAzul IP: como atualizar os dados cadastrais no Aplicativo Conta Azul de Bolso", "Conta PJ da ContaAzul IP: como gerar a declaração de status da Conta PJ (titularidade ou encerramento)", "Cobranças da Conta Azul: Edição desabilitada dos Dados da empresa", "Cobranças da Conta Azul: como editar os Dados da empresa se eu já utilizei as Cobranças da Conta Azul (antigo Receba Fácil)?", "Cobranças da Conta Azul: como editar os dados da empresa após ativar a Conta PJ", "Conta PJ da ContaAzul IP: como resolver financeiro de contas inativas", "Cobranças: como habilitar o parcelamento no link de pagamento?"]},
    {name:"Conhecer a Conta PJ da Conta Azul", arts:4, articles:["Conta PJ da ContaAzul IP: a evolução do Receba Fácil", "Conta PJ da ContaAzul IP: perguntas frequentes", "Cobranças da Conta Azul: perguntas frequentes sobre os meios de recebimento", "Cobranças da Conta Azul: o que é e como funciona"]},
    {name:"Analisar extrato da Conta PJ Conta Azul", arts:24, articles:["Conta PJ da ContaAzul IP: como acessar o Extrato", "Conta PJ da ContaAzul IP: como conciliar os lançamentos", "Conta PJ da ContaAzul IP: como acessar extrato e conciliações pendentes no ERP", "Conta PJ da ContaAzul IP: como criar lançamento para conciliar", "Conta PJ da ContaAzul IP: como conciliar lançamento já criado no ERP", "Conta PJ da ContaAzul IP: como conciliar lançamentos com diferença de valor", "Conta PJ da ContaAzul IP: como conciliar um lançamento com vários lançamentos provisionados", "Conta PJ da ContaAzul IP: como consultar conciliações no aplicativo", "Conta Azul de Bolso: como funcionam as Cobranças da Conta Azul no app", "Conta PJ da ContaAzul IP: como gerenciar estornos, falhas e conciliação do Pix", "Cobranças Conta Azul: o que significa Lançamento recuperado?", "Cobranças da Conta Azul: como consultar os recebimentos previstos na Conta PJ", "Cobranças da Conta Azul: como conferir se o pagamento do cliente foi recebido", "Cobranças da Conta Azul: como conciliar a transferência de valores para conta bancária", "Conta PJ da ContaAzul IP: como conciliar um pagamento recebido que aparece como vencido?", "Financeiro: como conciliar o pagamento de fatura de cartão de crédito feito pela Conta PJ da ContaAzul IP?", "Financeiro: por que o valor previsto de entrada na Conta Azul de Bolso não corresponde ao valor recebido?", "Cobranças: como conciliar um lançamento duplicado ou inesperado?", "Cobranças: como verificar pagamentos de cartão de crédito que não foram creditados ou refletidos no sistema?", "Cobranças: por que pagamentos de cartão de crédito aparecem quitados mas pendentes no contas a receber?", "Cobranças: como conciliar um boleto pago que não aparece no sistema?", "Cobranças: como consultar os e-mails de cobrança enviados na Conta Azul Pro?", "Cobranças da Conta Azul: lançamentos do Receba fácil", "Cobranças da Conta Azul: bancos homologados"]},
    {name:"Pagar boletos, guias e transfer\u00eancias pelo ERP", arts:37, articles:["Conta PJ da ContaAzul IP: como pagar contas", "Conta PJ da ContaAzul IP: pagamentos via boleto", "Conta PJ da ContaAzul IP: pagamentos via Pix", "Conta PJ da ContaAzul IP: como utilizar o Pix", "Conta PJ da ContaAzul IP: limites e regras para envio e aprovação de pagamentos em lote", "Conta PJ da ContaAzul IP: como acessar comprovantes de pagamento", "Conta PJ da ContaAzul IP: como transferir entre minhas contas", "Conta PJ da ContaAzul IP: como configurar suas chaves Pix", "Conta PJ da ContaAzul IP: pagamento de tributos", "Conta PJ da ContaAzul IP: quais tributos posso pagar pela Conta PJ", "Conta PJ da ContaAzul IP:  como lançar o pagamento de tributos pelo ERP Conta Azul Pro", "Conta PJ da ContaAzul IP: como enviar solicitação de tributo já criado", "Conta PJ da ContaAzul IP: como aprovar o pagamento de tributos no aplicativo Conta Azul de Bolso", "Conta PJ da ContaAzul IP: como pagar tributos diretamente pelo aplicativo Conta Azul de Bolso", "Conta PJ da ContaAzul IP: como excluir uma chave Pix?", "Conta PJ da ContaAzul IP: como ajustar limites Pix pelo Conta Azul de Bolso?", "Conta PJ da ContaAzul IP: como solicitar aumento de limite do Pix?", "Conta PJ da ContaAzul IP: como ajustar limites Pix diretamente pela Conta Azul Pro?", "Conta PJ da ContaAzul IP: é possível realizar portabilidade de chave Pix?", "Conta PJ da ContaAzul IP: como acompanhar a situação do agendamento de transferência entre minhas contas no ERP?", "Conta PJ da ContaAzul IP: como realizar a transferência entre minhas contas direto no aplicativo?", "Conta PJ da ContaAzul IP: como remover pelo ERP as contas bancárias cadastradas?", "Conta PJ da ContaAzul IP: pagamento realizado mas fornecedor não recebeu, o que fazer?", "Conta PJ da ContaAzul IP: é possível cancelar ou estornar um pagamento de boleto?", "Conta PJ da ContaAzul IP: é possível estornar ou cancelar um Pix realizado?", "Conta PJ da ContaAzul IP | Erro: boleto consta como já pago", "Conta PJ da ContaAzul IP | Erro: pagamento \"em execução\" que não conclui", "Conta PJ da ContaAzul IP | Erro: boleto vencido ou sem registro", "Conta PJ da ContaAzul IP | Erro: pagamento fora do horário permitido", "Conta PJ da ContaAzul IP | Erro: erro ao pagar tributos (DAS, DARF, FGTS)", "Conta PJ da ContaAzul IP | Erro: código de barras inválido ou divergente", "Conta PJ da ContaAzul IP | Erro: pagamento Pix com status \"não executado\"", "Conta PJ da ContaAzul IP | Erro: Pix QR Code inválido ou fatura expirada", "Conta PJ da ContaAzul IP | Erro: limite de Pix insuficiente", "Conta PJ da ContaAzul IP | Erro: dispositivo não cadastrado", "Conta PJ da ContaAzul IP | Erro: falha na liquidação da transação", "Conta PJ da ContaAzul IP | Erro: dados do cliente não conferem com a conta"]},
    {name:"Automatizar pagamentos com a Conta PJ", arts:12, articles:["Conta PJ da ContaAzul IP: como pagar contas pelo aplicativo Conta Azul de Bolso", "Conta PJ da ContaAzul IP: como agendar pagamentos", "Conta PJ da ContaAzul IP: como solicitar pagamento pelo ERP e aprovar no aplicativo", "Conta PJ da ContaAzul IP: quais os horários, prazos e restrições para agendamento de pagamento na Conta PJ", "Conta PJ da ContaAzul IP: como cancelar o agendamento de pagamento", "Conta PJ da ContaAzul IP: como identificar um pagamento no ERP Conta Azul Pro", "Conta PJ da ContaAzul IP: solicitação de pagamento não aparece no aplicativo", "Conta PJ da ContaAzul IP: como cancelar agendamentos de pagamento no ERP Conta Azul Pro", "Conta PJ da ContaAzul IP: como enviar uma solicitação de pagamento pelo ERP Conta Azul Pro", "Conta PJ da ContaAzul IP: como excluir dados bancários do fornecedor cadastrados", "Conta PJ da ContaAzul IP: por que não consigo agendar um Pix para uma data futura?", "Conta PJ da ContaAzul IP: como aprovar pagamentos Pix em lote na Conta PJ?"]},
    {name:"Importar boletos via DDA", arts:6, articles:["Conta PJ da ContaAzul IP: como funciona o DDA", "Débito Direto Autorizado (DDA): o que é e como funciona", "Débito Direto Autorizado (DDA): boleto cancelado ainda aparece para meu cliente", "Conta Azul de Bolso: como funciona o DDA no app?", "Débito Direto Autorizado (DDA): como resolver um boleto pago que ainda aparece no DDA?", "Débito Direto Autorizado (DDA): como resolver DDA com lançamentos vencidos após pagamento?"]},
    {name:"Controlar aprova\u00e7\u00f5es e multiusu\u00e1rios de pagamento", arts:23, articles:["Conta PJ da ContaAzul IP: como funciona a gestão de usuários", "Conta PJ da ContaAzul IP: como adicionar usuário na Conta PJ da ContaAzul IP?", "Conta PJ da ContaAzul IP: recebi o convite para acessar a Conta PJ da ContaAzul IP, como aceitar?", "Conta PJ da ContaAzul IP: como remover usuário da Conta PJ da ContaAzul IP?", "Conta PJ da ContaAzul IP: quais as permissões por usuário", "Conta PJ da ContaAzul IP: como funciona a gestão de dispositivos", "Conta PJ da ContaAzul IP: como cadastrar o dispositivo e criar a senha de transação ao ativar a Conta PJ da ContaAzul IP?", "Conta PJ da ContaAzul IP: como cadastrar o dispositivo do usuário Administrador principal de Conta PJ da ContaAzul IP já ativa?", "Conta PJ da ContaAzul IP: como cadastrar o dispositivo de outros usuários?", "Conta PJ da ContaAzul IP: como aprovar o cadastro de dispositivo de outros usuários?", "Conta PJ da ContaAzul IP: como ajustar Limite Pix?", "Conta PJ da ContaAzul IP: como bloquear o cadastro de dispositivo de outros usuários?", "Conta PJ da ContaAzul IP: como desbloquear o cadastro de dispositivo de outros usuários?", "Conta PJ da ContaAzul IP: como excluir o cadastro de dispositivo de outros usuários?", "Conta PJ da ContaAzul IP: como aprovar solicitações de pagamento em lote", "Conta PJ da ContaAzul IP: como solucionar aprovações de pagamento em lote com falha", "Conta PJ da ContaAzul IP | Erro: Pix: Validação pendente", "Conta PJ da ContaAzul IP | Erro: Pix: Remova a solicitação de pagamento e altere a chave Pix", "Conta PJ da ContaAzul IP | Erro: Pix: Ajuste o valor da despesa ou cancele a solicitação", "Conta PJ da ContaAzul IP | Erro: Boleto: Este boleto pode ter sido cancelado ou não registrado", "Conta PJ da ContaAzul IP | Erro: Boleto: Pagamento agendado", "Conta PJ da ContaAzul IP | Erro: Boleto: Este boleto vence hoje e estamos fora do horário", "Conta PJ da ContaAzul IP | Erro: Boleto: Ajuste o valor da despesa ou cancele a solicitação"]},
  ]},
  { id:"p8", name:"Realizar concilia\u00e7\u00e3o banc\u00e1ria", color:"#1a5a7a", sections:[
    {name:"Entender a concilia\u00e7\u00e3o banc\u00e1ria", arts:109, articles:["Financeiro: é possível alterar o nome de lançamentos de transferência entre contas?", "Financeiro: como o sistema permite saldo negativo no caixa?", "Financeiro: como lidar com lançamentos de investimentos incorretos no sistema?", "Financeiro: como identificar lançamentos que compõem categorias do DRE e conciliar valores?", "Financeiro: como registrar o uso do cheque especial no sistema?", "Financeiro: por que uma transferência autorizada não está sendo efetivada?", "Financeiro: como registrar limite de cheque especial utilizando a Conta Caixinha?", "Financeiro: como conferir entradas em dinheiro na conta Caixa?", "Financeiro: como conciliar valores entre extrato e relatório de contas pagas?", "Financeiro: como resolver inconsistência entre aplicação e extrato financeiro?", "Financeiro: como zerar o saldo de contas encerradas no sistema?", "Financeiro: como gerenciar lançamentos vencidos após conciliação utilizando a conta caixinha?", "Financeiro: como extrair transferências entre contas do sistema?", "Financeiro: como registrar corretamente transferências bancárias no sistema?", "Financeiro: como ajustar e entender saldo negativo em conta física?", "Financeiro: por que o saldo da Conta Caixinha não está batendo?", "Financeiro: por que o fluxo de caixa não bate com o extrato de conciliação?", "Conciliação: como localizar lançamentos para conciliação?", "Financeiro: como atualizar saldo e excluir conta de investimento?", "Financeiro: como visualizar e conciliar lançamentos após exclusão de contas?", "Conciliação: como corrigir saldo quando o botão \"corrigir manualmente o saldo do banco\" não aparece?", "Conciliação: como conciliar um pagamento dividido entre várias notas de compra?", "Conciliação: como conciliar recebimentos com o mesmo valor quando o banco não identifica o pagador?", "Conciliação: como resolver falha na importação de OFX do banco Caixa?", "Conciliação: como resolver erro na integração da conta Bradesco?", "Conciliação: como corrigir divergência de saldo em uma data específica?", "Conciliação: como criar e integrar conta do Banco Inter com falha de comunicação?", "Conciliação: como reativar a integração bancária automática do Bradesco?", "Conciliação: como exibir lançamentos recentes que não aparecem?", "Conciliação: como zerar lançamentos financeiros ao virar o ano?", "Conciliação: como alterar a descrição do histórico de lançamentos integrados?", "Conciliação: como resolver problemas de integração com o Itaú?", "Conciliação: como resolver venda em aberto mas quitada?", "Conciliação: como resolver e verificar pagamentos duplicados de link de crédito?", "Conciliação: por que o sistema gera lançamentos duplicados?", "Conciliação: por que os saldos ficam diferentes após transferências financeiras?", "Conciliação: rendimentos de investimento são puxados automaticamente para a conta?", "Conciliação: como inserir novos lançamentos manuais sem apagar os existentes?", "Conciliação: como evitar duplicidade ao importar lançamentos atrasados?", "Conciliação: como importar extrato bancário OFX da Stone?", "Conciliação: como resolver falhas na integração automática bancária?", "Conciliação: como atualizar o extrato bancário no sistema?", "Conciliação: como conciliar transferências que foram divididas incorretamente?", "Conciliação: como resolver a não atualização do extrato bancário?", "Conciliação: como resolver falhas na importação automática de extratos bancários?", "Conciliação: como resolver pendências em movimentações e conciliação bancária?", "Conciliação: por que lançamentos bancários são arquivados na conciliação?", "Conciliação: como resolver indisponibilidade na conciliação de contas a pagar?", "Conciliação: como resolver falha na importação de recebimentos da maquininha Stone?", "Conciliação: como importar extrato OFX quando há lançamentos arquivados ou faltantes?", "Conciliação: como realizar a importação de extratos bancários via OFX e quais as alternativas quando o banco não disponibiliza este formato?", "Conciliação: como resolver a indisponibilidade da integração com o Sicoob?", "Conciliação: como importar extrato OFX com período específico?", "Conciliação: por que a conciliação bancária não atualiza os dados?", "Conciliação: como conciliar pagamentos pendentes ou não reconhecidos?", "Conciliação: como conciliar quando o sistema não mostra itens?", "Conciliação: como conciliar recebimentos bancários com liquidação total?", "Conciliação: como registrar uma entrada que está faltando em um investimento?", "Conciliação: por que valores não aparecem no extrato bancário para conciliação?", "Conciliação: como identificar a origem dos recebimentos importados pela descrição do lançamento?", "Conciliação: como remover extrato bancário importado incorretamente?", "Conciliação: como corrigir saldos incorretos após subir arquivo OFX?", "Conciliação: como garantir que pagamentos de boleto sejam reconhecidos e conciliados corretamente?", "Conciliação: por que a minha Conta Caixinha está negativa e pedindo conciliação?", "Conciliação: como identificar a origem de uma conciliação pendente?", "Conciliação: como corrigir divergências de saldo bancário e de caixa em dinheiro?", "Conciliação: como corrigir datas incorretas em extratos OFX importados?", "Conciliação: como conciliar devoluções ou ajustes no cartão de crédito?", "Conciliação: como conciliar e lançar fundos de investimento?", "Conciliação: como garantir que o saldo da conta de recebimento bata com a plataforma?", "Conciliação: como resolver cobranças que permanecem em aberto após conciliação, com uso da Conta Caixinha para pagamentos?", "Conciliação: como dividir um valor de conciliação em duas categorias?", "Conciliação: como conciliar saques da Hotmart com extrato bancário?", "Conciliação: como criar ou atrelar vendas na conciliação bancária?", "Conciliação: como garantir que saldos iniciais e finais batem entre meses?", "Conciliação: como conciliar lançamentos recuperados com vendas em aberto?", "Conciliação: como processar arquivo de retorno de boletos bancários?", "Conciliação: como remover conciliações pendentes de extrato importado?", "Conciliação: como realizar a conciliação de pagamento com nota fiscal corretamente?", "Conciliação: como conciliar um pagamento não registrado no sistema?", "Conciliação: como conciliar uma saída de comissão com uma venda recebida?", "Conciliação: como obter relatório de movimentações bancárias não conciliadas?", "Conciliação: como resolver divergência na conciliação bancária por data incorreta?", "Conciliação: como conciliar boleto com descrição lançamento recuperado?", "Conciliação: como apagar todas as conciliações e começar do zero?", "Conciliação: como conciliar um pagamento duplicado de boleto?", "Conciliação: como resolver pagamento não reconhecido?", "Conciliação: por que a conciliação automática não sugere lançamentos em aberto?", "Conciliação: como resolver a duplicação de receitas com Pix e vendas?", "Conciliação: como conciliar um reembolso de colaborador?", "Conciliação: por que o saldo da conta aparece mesmo após o encerramento?", "Conciliação: como resolver problemas de conciliação e valores pendentes?", "Conciliação: por que transferências bancárias aparecem como receita de vendas?", "Conciliação: como registrar e conciliar uma compra no extrato bancário?", "Conciliação: como conciliar parcelas de empréstimo que não aparecem?", "Conciliação: como registrar e conciliar recebimentos divididos?", "Conciliação: como automatizar conciliação de pagamentos Pix de faturas geradas como boleto?", "Conciliação: como corrigir saldo negativo na conciliação Hotmart?", "Conciliação: como zerar saldo e importar extrato bancário?", "Conciliação: como ajustar corretamente o saldo de uma conta de aplicação?", "Conciliação: como importar retorno bancário detalhado para conciliação de pagamentos?", "Conciliação: como baixar boletos com pagamentos bancários agrupados?", "Conciliação: como registrar transferência de fundos entre contas sem gerar receita?", "Conciliação: como ajustar saldo de conta após longo período sem uso?", "Conciliação: como conciliar um pagamento com valor maior e devolução?", "Conciliação: como excluir lançamentos financeiros de conciliações pendentes?", "Conciliação: como ajustar o saldo diário manualmente?", "Conciliação: como integrar vendas via extrato OFX?", "Conciliação: como conciliar uma compra com saída do banco?"]},
    {name:"Iniciar a concilia\u00e7\u00e3o banc\u00e1ria", arts:17, articles:["Conciliação: diferenças na conciliação por tipo de integração bancária", "Conciliação: como entender os campos, telas e processos?", "Conciliação: o que significam os termos e processos da conciliação?", "Conciliação: quais são as principais telas da conciliação?", "Conciliação: qual o significado dos elementos visuais das telas da conciliação?", "Conciliação: qual o significado da Situação dos lançamentos da conciliação?", "Conciliação: qual o significado dos botões de ação na conciliação?", "Conciliação: manual completo para bater saldo", "Conciliação: como acessar as conciliações pendentes?", "Conciliação: como funciona a sugestão da coluna Lançamentos da Conta Azul?", "Conciliação: como conciliar valor exato?", "Conciliação: como conciliar criando um novo lançamento?", "Conciliação: como desfazer uma conciliação?", "Conciliação: como mudar de conta financeira para continuar a conciliação?", "Conciliação: trilha de vídeos sobre a conciliação bancária", "Conciliação: como acompanhar as movimentações já conciliadas", "Conciliação: o que aparece em cada coluna"]},
    {name:"Fazer concilia\u00e7\u00e3o banc\u00e1ria completa", arts:12, articles:["Conciliação bancária: saldo total das contas cadastradas", "Conciliação: buscar lançamento já criado na Conta Azul", "Conciliação: como encontrar lançamento já criado usando os filtros de Buscar lançamento", "Conciliação: como arquivar lançamentos do banco", "Conciliação: várias movimentações do banco com um lançamento", "Conciliação: uma movimentação do banco com vários lançamentos", "Conciliação: uma movimentação do banco com vários lançamentos exatos", "Conciliação: uma movimentação do banco com vários lançamentos parciais", "Conciliação: revisar e ajustar valores", "Conciliação: quais as opções de ajuste em Revisar valores de uma movimentação do banco com vários lançamentos?", "Conciliação: como editar e conciliar lançamentos em lote", "Conta Azul de Bolso: como funcionam as Conciliações pendentes no app?"]},
    {name:"Identificar tipos de lan\u00e7amentos na concilia\u00e7\u00e3o", arts:11, articles:["Conciliação: como conciliar estorno", "Conciliação: como conciliar empréstimo", "Conciliação: quais os tipos de lançamentos que posso conciliar", "Conciliação: como conciliar transferência entre contas", "Conciliação: como conciliar transferência criando o lançamento", "Conciliação: como conciliar uma transferência entre contas na Conta PJ da ContaAzul IP", "Conciliação: como conciliar investimento e aplicação automática", "Conciliação: como conciliar investimento e aplicação automática do Banco do Brasil", "Conciliação: como conciliar transações da conta poupança", "Conciliação: como conciliar movimentações já criadas para a conta Poupança na Conta Azul?", "Conciliação: como conciliar transações em cheque"]},
    {name:"Habilitar integra\u00e7\u00e3o banc\u00e1ria autom\u00e1tica", arts:26, articles:["Integração automática: como fazer", "Integração automática: como habilitar em novas contas", "Integração automática: como habilitar em contas já cadastradas", "Integração automática: quando acontece a busca dos lançamentos", "Integração automática: horários de atualização por banco", "Integração bancária: como verificar a data da última atualização", "Integração automática: quantos dias são importados na ativação?", "Integração automática: o que é importado ao reativar a integração?", "Integração automática: motivos que impedem a importação", "Integração bancária automática: quais os bancos homologados na Conta Azul", "Integração bancária automática: como habilitar a integração com o Banco do Brasil", "Integração bancária automática: como habilitar a integração com o Bradesco", "Integração bancária automática: como criar usuário sem token no Bradesco", "Integração bancária automática: como habilitar a integração com o BS2", "Integração bancária automática: como habilitar a integração com o BTG Pactual", "Integração bancária automática: como habilitar a integração com o banco Cora", "Integração bancária automática: como habilitar a integração com o banco Inter", "Integração bancária automática: como cadastrar uma nova API no banco Inter", "Integração bancária automática: como habilitar a integração com o Itaú", "Integração bancária automática: como criar usuário sem token no Itaú", "Integração bancária automática: como habilitar a integração com o PagSeguro", "Integração bancária automática: como habilitar a integração com o Santander", "Integração bancária automática: como criar usuário somente leitura no Santander", "Integração bancária automática: como habilitar a integração com o Sicredi", "Integração bancária automática: como habilitar a integração com a Stone", "Integração automática: quantas contas posso integrar?"]},
    {name:"Integrar contas atrav\u00e9s do Open Finance", arts:12, articles:["Open Finance: como funciona a integração na Conta Azul", "Open Finance: como autorizar", "Open Finance: como autorizar a integração com Banco do Brasil", "Open Finance: como autorizar a integração com Bradesco", "Open Finance: como autorizar a integração com C6 Bank", "Open Finance: como autorizar a integração com Caixa Econômica Federal", "Open Finance: como autorizar a integração com Itaú", "Open Finance: como autorizar a integração com Nubank", "Open Finance: como autorizar a integração com Santander", "Open Finance: como autorizar a integração com Sicoob", "Open Finance: como autorizar a integração com Sicredi", "Open Finance: como desabilitar com a Conta Azul"]},
    {name:"Importar extrato banc\u00e1rio manualmente (OFX, CSV e XLS)", arts:24, articles:["Integração bancária via OFX: como fazer integração manual", "Integração bancária via OFX: como importar ao criar uma conta bancária", "Integração bancária via OFX: como importar em conta bancária já cadastrada", "Integração bancária via OFX: o que é o extrato OFX", "Integração bancária via OFX: quais bancos oferecem o arquivo OFX", "Integração bancária via OFX: como corrigir o OFX importado", "Integração bancária via OFX: importei o OFX na conta errada, como corrigir?", "Integração bancária via OFX: como exportar extrato em OFX do Banco BS2", "Integração bancária via OFX: como exportar extrato em CSV do Banco do Brasil", "Integração bancária via OFX: como exportar extrato em OFX do Banco do Brasil", "Integração bancária via OFX: como exportar extrato em OFX do banco Inter", "Integração bancária via OFX: como exportar extrato em OFX do banco Safra", "Integração bancária via OFX: como exportar extrato em CSV do Bradesco", "Integração bancária via OFX: como exportar extrato em OFX do Bradesco", "Integração bancária via OFX: como exportar extrato em OFX da Caixa Econômica Federal", "Integração bancária via OFX: como exportar extrato em OFX do Itaú", "Integração bancária via OFX: como exportar extrato em OFX do Nubank", "Integração bancária via OFX: como exportar extrato em OFX do Santander", "Integração bancária via OFX: como exportar extrato em OFX do Sicoob", "Integração bancária via OFX: como exportar extrato em XLS do Sicoob", "Integração bancária via OFX: como exportar extrato em CSV do Sicredi", "Integração bancária via OFX: como exportar extrato em OFX do Sicredi", "Integração bancária via OFX: como exportar extrato da Stone", "Integração bancária via OFX: como importar o extrato da Conta Stone digital"]},
    {name:"Entender a integra\u00e7\u00e3o e gest\u00e3o de contas financeiras", arts:33, articles:["Integração bancária via OFX: perguntas frequentes sobre integração bancária manual", "Conta Cartão de Crédito: perguntas frequentes sobre gestão da conta cartão de crédito", "Financeiro: como visualizar os dados das contas bancárias cadastradas?", "Financeiro: como ocultar ou desabilitar contas financeiras não utilizadas?", "Financeiro: como transferir movimentações financeiras de uma conta antiga para uma nova?", "Financeiro: por que a opção caixinha não aparece em recebimentos?", "Financeiro: como registrar limite de cheque especial em conta bancária, utilizando a Conta Caixinha?", "Financeiro: como gerenciar e conciliar faturas de cartão de crédito no sistema?", "Financeiro: como acessar dados históricos de conta bancária excluída?", "Financeiro: como conciliar fatura de cartão com estornos sem afetar centros de custo?", "Financeiro: como conciliar pagamentos com cartão pré-pago não cadastrado no ERP?", "Conciliação: como conciliar a fatura do cartão de crédito com o sistema e débitos bancários?", "Financeiro: como resolver e excluir conta bancária inativa com pendências?", "Financeiro: como corrigir valor incorreto na fatura do cartão de crédito?", "Financeiro: como localizar lançamentos de cartão de crédito não encontrados no sistema?", "Financeiro: como atualizar o extrato manualmente?", "Integração bancária: quais dados bancários são visíveis após integração e por quem?", "Conciliação: como resolver pendências e ausências de lançamentos na conciliação, especialmente na conta de cartão de crédito?", "Conciliação: como conciliar pagamentos e despesas do Cartão de Crédito?", "Conciliação: como conciliar fatura de cartão de crédito usando IA?", "Conciliação: como resolver a fatura de cartão de crédito marcada como paga indevidamente?", "Conciliação: como conciliar pagamentos parciais de faturas de cartão de crédito?", "Conciliação: como realizar a conciliação de compras de cartão de crédito?", "Conciliação: como conciliar pagamentos de cartão de crédito, incluindo pagamentos parciais e realizados pelo banco?", "Conciliação: como realizar a conciliação do cartão de crédito na Conta Azul?", "Conciliação: como conciliar saldo da Conta Caixinha com transferências para cartão?", "Conciliação: como lançar, informar o pagamento e conciliar a fatura de cartão de crédito?", "Conciliação: como conciliar uma venda de cartão dividida?", "Conciliação: como conciliar recebimentos de cartão de crédito emitidos externamente?", "Conciliação: como registrar crédito de pagamento em excesso no cartão?", "Conciliação: como vincular pagamentos de cartão de crédito para evitar duplicidade?", "Conciliação: qual passo devo seguir primeiro na conciliação do Cartão de Crédito?", "Conciliação: como importar e conciliar despesas de cartão de crédito via planilha?"]},
    {name:"Corrigir o saldo na concilia\u00e7\u00e3o", arts:63, articles:["Conciliação: como conferir e corrigir o saldo inicial da conta bancária", "Conciliação: como validar e interpretar o extrato bancário", "Conciliação: como verificar se existem lançamentos ignorados/arquivados na data", "Conciliação: corrigir a baixa de um lançamento que não consta no extrato bancário", "Conciliação: corrigir conciliação feita em duplicidade", "Conciliação: como corrigir conciliação se o lançamento foi importado mais de uma vez?", "Conciliação: como corrigir conciliação de lançamento criado em duplicidade?", "Conciliação: como corrigir coluna banco por extrato em OFX", "Conciliação: corrigir lançamento conciliado incorretamente", "Conciliação: como corrigir saldo de conta que não atualiza após edição", "Conciliação: como realizar a conciliação exata de todos os lançamentos retroativos?", "Conciliação: como conciliar lançamentos retroativos a partir de uma data de corte?", "Conciliação: como corrigir saldo retroativo da Conta aplicação?", "Conciliação: lançamentos arquivados", "Conciliação | Erro: Conciliações pendentes", "Conciliação | Erro: Existem lançamentos bancários que não foram conciliados", "Conciliação | Erro: Lançamento da Conta Azul não foi conciliado com nenhum lançamento bancário", "Conciliação | Erro: Saldos diferentes", "Conciliação | Erro: Saldo com diferença de R$", "Conciliação | Erro: Verifique lançamentos não conciliados", "Conciliação bancária: como bater saldo com Banco do Brasil", "Conciliação bancária: como interpretar o extrato do Banco do Brasil?", "Conciliação bancária: como corrigir diferenças de saldo do Banco do Brasil?", "Conciliação bancária: como bater saldo com Bradesco", "Conciliação bancária: como interpretar o extrato do Bradesco?", "Conciliação bancária: como corrigir diferenças de saldo do Bradesco?", "Conciliação bancária: como bater saldo com Banco BS2", "Conciliação bancária: como interpretar o extrato do Banco BS2?", "Conciliação bancária: como corrigir diferenças de saldo do Banco BS2?", "Conciliação bancária: como bater saldo com BTG Pactual", "Conciliação bancária: como interpretar o extrato do BTG Pactual?", "Conciliação bancária: como corrigir diferenças de saldo do BTG Pactual?", "Conciliação bancária: como bater saldo com C6 Bank", "Conciliação bancária: como interpretar o extrato do C6 Bank?", "Conciliação bancária: como corrigir diferenças de saldo do C6 Bank?", "Conciliação bancária: como bater saldo com Caixa Econômica Federal", "Conciliação bancária: como interpretar o extrato da Caixa Econômica Federal?", "Conciliação bancária: como corrigir diferenças de saldo da Caixa Econômica Federal?", "Conciliação bancária: como bater saldo com o banco Cora", "Conciliação bancária: como interpretar o extrato do banco Cora?", "Conciliação bancária: como corrigir diferenças de saldo do banco Cora?", "Conciliação bancária: como bater saldo com Inter", "Conciliação bancária: como interpretar o extrato do banco Inter?", "Conciliação bancária: como corrigir diferenças de saldo do banco Inter?", "Conciliação bancária: como bater saldo com Itaú", "Conciliação bancária: como interpretar o extrato do banco Itaú?", "Conciliação bancária: como corrigir diferenças de saldo do banco Itaú?", "Conciliação bancária: como bater saldo com Nubank", "Conciliação bancária: como interpretar o extrato do Nubank?", "Conciliação bancária: como corrigir diferenças de saldo do Nubank?", "Conciliação bancária: como bater saldo com PagSeguro e Stone", "Conciliação bancária: como bater saldo com Safra", "Conciliação bancária: como interpretar o extrato do banco Safra?", "Conciliação bancária: como corrigir diferenças de saldo do banco Safra?", "Conciliação bancária: como bater saldo com Santander", "Conciliação bancária: como interpretar o extrato do banco Santander?", "Conciliação bancária: como corrigir diferenças de saldo do banco Santander?", "Conciliação bancária: como bater saldo com Sicoob", "Conciliação bancária: como interpretar o extrato do banco Sicoob?", "Conciliação bancária: como corrigir diferenças de saldo do banco Sicoob?", "Conciliação bancária: como bater saldo com Sicredi", "Conciliação bancária: como interpretar o extrato do banco Sicredi?", "Conciliação bancária: como corrigir diferenças de saldo do banco Sicredi?"]},
    {name:"Corrigir a integra\u00e7\u00e3o autom\u00e1tica", arts:19, articles:["Integração bancária: não importou os lançamentos do meu banco, o que fazer?", "Integração bancária automática: como recuperar lançamentos retroativos?", "Integração bancária: como refazer integração", "Open Finance |  Erro: Ops... Ocorreu um erro durante a jornada", "Integração bancária | Erro: Ocorreu um erro ao salvar as informações da sua integração - Request failed with status code 503, banco Itaú", "Integração bancária | Erro: Houve uma falha na comunicação com o banco Inter", "Integração bancária | Erro: O usuário não possui permissão para acessar a conta solicitante", "Integração bancária automática: como atualizar o certifica do banco Inter?", "Integração bancária | Erro: Conta bloqueada", "Integração bancária | Erro: A integração com o Sicoob está indisponível", "Integração bancária | Erro: Falha de comunicação com o banco", "Integração bancária | Erro: Indisponibilidade do servidor (Erro 500)", "Integração bancária | Erro: Verifique a agência e conta informados no cadastro", "Integração bancária | Erro: Integração paralisada temporariamente", "Integração bancária | Erro: Necessário completar a autorização com a instituição financeira", "Integração bancária | Erro: Senha expirada no Itaú", "Integração bancária | Erro: Verifique os dados informados na configuração da conta bancária", "Integração bancária | Erro: Verifique os dados informados e tente novamente [M416-000]", "Integração bancária | Erro: CNPJ divergente do consentimento"]},
    {name:"Bater saldo com sugest\u00f5es da IA", arts:4, articles:["Conciliação: como fazer conciliação bancária automática", "Conciliação bancária: perguntas frequentes sobre como bater saldo", "Conciliação: como bater o saldo do banco com o saldo da Conta Azul", "O que é integração bancária?"]},
    {name:"Entender o Extrato de movimenta\u00e7\u00f5es", arts:3, articles:["Extrato de movimentações: o que é e como funciona", "Conta Azul de Bolso: como funciona o Extrato de movimentações no app?", "Lançamentos financeiros: quais são as diferenças entre Extrato de movimentações e Contas a pagar ou a receber"]},
  ]},
  { id:"p9", name:"Organizar o financeiro", color:"#3a5a6a", sections:[
    {name:"Organizar contas financeiras da minha empresa", arts:17, articles:["Tela de Contas Financeiras", "Contas financeiras (Outras contas): como cadastrar contas correntes e outros tipos", "Contas financeiras (Outras contas): como funciona o cadastro de Conta Corrente na Conta Azul?", "Conta Corrente padrão: como definir", "Contas financeiras (Outras contas): como analisar as movimentações", "Contas financeiras (Outras contas): o que é apresentado em Ações da conta?", "Contas financeiras (Outras contas): o que é apresentado em Conciliações pendentes?", "Contas financeiras (Outras contas): o que é apresentado na tela de Movimentações?", "Contas financeiras (Outras contas): como cadastrar uma conta Poupança na Conta Azul", "Contas financeiras (Outras contas): como cadastrar uma conta Investimento e aplicação automática", "Contas financeiras (Outras contas): saldo inicial na criação da conta", "Contas financeiras (Outras contas): como informar o saldo inicial de uma conta já cadastrada?", "Contas financeiras (Outras contas): como corrigir apenas o saldo inicial de uma conta com conciliações?", "Conta Caixinha: o que é e como gerenciar", "Contas financeiras (Outras contas): como controlar a conta Meios de recebimento", "Contas financeiras (Outras contas): como filtrar lançamentos de uma conta bancária", "Contas financeiras (Outras contas): como excluir"]},
    {name:"Configurar categorias e centros de custo", arts:22, articles:["Categorias financeiras: o que é e como cadastrar", "Centro de custo: o que é e como cadastrar", "Categoria financeira e centro de custo: como fazer o rateio de centro de custo e categoria", "Categorias financeiras: como entender as categorias e subcategorias", "Categoria financeira e centro de custo: edição no lançamento financeiro", "Categoria financeira: como excluir", "Financeiro: como classificar reembolso de receita como redutor de despesa?", "Financeiro: como fazer o rateio de centro de custo de uma despesa?", "Financeiro: como categorizar múltiplos itens em uma única despesa?", "Financeiro: como excluir categorias financeiras existentes?", "Financeiro: como ratear a mesma categoria entre múltiplos centros de custo?", "Financeiro: como utilizar centros de custo e rateio para orçamento e alocação de despesas?", "Financeiro: como fazer rateio entre categorias de receita e despesa?", "Financeiro: como cadastrar categorias e subcategorias financeiras?", "Financeiro: como criar uma categoria de custos?", "Financeiro: como alterar a categoria de valores recebidos em lote?", "Financeiro: como reclassificar lançamentos entre categorias no sistema, incluindo a opção em lote?", "Financeiro: como migrar lançamentos entre categorias financeiras?", "Financeiro: como classificar transferências bancárias sem contas visíveis?", "Financeiro: como gerenciar centros de custo entre empresas do grupo?", "Financeiro: por que a função de associar categorias financeiras não aparece na minha conta?", "Financeiro: como remover categorias financeiras duplicadas?"]},
    {name:"Entender a Vis\u00e3o de compet\u00eancia vs caixa", arts:7, articles:["Visão de competência: o que é e como funciona", "Visão de competência: como criar lançamentos", "Visão de competência: como editar lançamentos", "Visão de competência: como personalizar colunas", "Visão de competência: como excluir lançamento", "Visão de competência: como conferir lançamentos e comparar com o DRE", "Lançamentos financeiros: quais são as diferenças entre Visão de Competência e Visão de Caixa"]},
    {name:"Auditar lan\u00e7amentos realizados por usu\u00e1rios", arts:2, articles:["Histórico financeiro: o que é e como funciona", "Histórico financeiro: entenda como as ações são registradas"]},
    {name:"Esclarecer d\u00favidas gerais do financeiro", arts:25, articles:["Lançamentos financeiros: perguntas frequentes", "Lançamentos financeiros: diretório de passo a passo financeiro", "Financeiro: por que a opção \"Excluir\" não aparece no financeiro?", "Financeiro: como lançar um imposto retido no financeiro da Conta Azul?", "Financeiro: como parcelar valores e pagamentos em aberto?", "Financeiro: como processar pagamentos com erro de limite?", "Financeiro: como encontrar lançamentos financeiros ocultos no sistema?", "Financeiro: como configurar despesas recorrentes duas vezes por semana?", "Financeiro: como obter comprovante de pagamento?", "Financeiro: como gerar comprovante para entrada de Saldo para IP?", "Financeiro: como identificar usuário e horário de lançamentos financeiros?", "Financeiro: como registrar um lançamento com múltiplos meios de pagamento?", "Financeiro: como registrar despesas com parcelamento além do limite do sistema?", "Financeiro: como evitar que transferências entre contas afetem o saldo da receita?", "Financeiro: como lançar uma despesa como recorrente?", "Financeiro: como provisionar despesas anuais sem lançar individualmente?", "Financeiro: qual a diferença entre provisão e baixa financeira na contabilização?", "Financeiro: por que minhas receitas e despesas não batem?", "Financeiro: como gerenciar empréstimos sem poluir o financeiro?", "Financeiro: como configurar o Controle de movimentações de caixa?", "Financeiro: o sistema calculou o valor das parcelas errado. O que fazer?", "Financeiro: é possível restaurar receitas ou despesas excluídas?", "Financeiro: onde encontro o lançamento recuperado do estorno?", "Financeiro: como configurar condição de pagamento na Conta Azul?", "Financeiro: por que aparecem lançamentos recuperados duplicados?"]},
    {name:"Organizar meus cadastros (Clientes, Fornecedores, Servi\u00e7os)", arts:54, articles:["Importação com IA: como cadastrar clientes, produtos, serviços, receitas e despesas", "Configurações gerais: como importar dados para a Conta Azul", "Configurações gerais: como preencher a planilha modelo para importar dados", "Importar dados: o que é importado via XML de venda", "Clientes: como cadastrar", "Clientes: como cadastrar clientes estrangeiros", "Meus cadastros: como salvar cliente, fornecedor e transportadora no mesmo cadastro?", "Conta Azul de Bolso: como cadastrar, consultar e gerenciar clientes", "Clientes: que ações posso realizar na tela de Clientes?", "Clientes: como importar planilha com clientes", "Clientes: como editar", "Clientes: como excluir ou inativar", "Serviços: como cadastrar", "Conta Azul de Bolso: como funciona o cadastro de produtos e serviços no app", "Conta Azul de Bolso: como cadastrar um novo Produto pelo aplicativo", "Conta Azul de Bolso: como cadastrar um novo Serviço pelo aplicativo", "Conta Azul de Bolso: como consultar, editar ou excluir um Produto pelo aplicativo", "Conta Azul de Bolso: como consultar, editar ou excluir um Serviço pelo aplicativo", "Serviços: como exportar serviços cadastrados", "Serviços: como excluir os serviços cadastrados", "Produtos: como importar planilha com produtos", "Produtos: como resolver erros na importação de planilha de produtos", "Fornecedores: como cadastrar", "Conta Azul de Bolso: Cadastro de fornecedor", "Fornecedores: como importar planilha com fornecedores", "Fornecedores: como editar", "Fornecedor: como reativar o cadastro de um fornecedor", "Meus cadastros: como a Conta Azul vai tratar os cadastros com CNPJ alfanumérico", "Fornecedores: como excluir ou inativar", "Transportadoras: como cadastrar", "Transportadoras: como editar", "Transportadoras: como excluir ou inativar", "Meus cadastros: como fazer o backup de dados cadastrados", "Meus cadastros: como exportar cadastros", "Meus cadastros: como exportar cadastros de clientes?", "Meus cadastros: como exportar cadastros de produtos?", "Meus cadastros: como exportar cadastros de serviços?", "Meus cadastros: como exportar cadastros de transportadoras?", "Meus cadastros: como exportar cadastros de fornecedores?", "Meus cadastros: como exportar as movimentações financeiras?", "Meus cadastros: perguntas frequentes sobre backup", "Meus cadastros: quais dados são exportados no backup?", "Meus cadastros: em qual horário posso baixar o backup?", "Meus cadastros: como abrir os dados do backup no Excel?", "Meus cadastros: como abrir os dados do backup no LibreOffice?", "Meus cadastros: como abrir os dados do backup nas Planilhas Google?", "Meus cadastros: como importar novamente os dados do backup?", "Meus cadastros: é possível fazer backup das notas de compras importadas?", "Lançamentos financeiros: por que preciso informar CPF ou CNPJ ao importar planilha?", "Vendas: como cadastrar um serviço?", "Meus cadastros: onde posso anexar documentos na Conta Azul?", "Meus cadastros: como limpar dados cadastrados", "Meus cadastros: quais as opções de limpeza de dados cadastrados", "Meus cadastros: como zerar todas as informações da minha conta"]},
  ]},
  { id:"p10", name:"Antecipar parcelas a receber de clientes", color:"#7a4a1a", sections:[
    {name:"Descobrir como funciona a antecipa\u00e7\u00e3o", arts:8, articles:["Crédito: o que é antecipação de recebíveis", "Crédito: quais os tipos de recebíveis e aceitação na Conta Azul?", "Crédito: quem pode solicitar a antecipação de recebíveis na Conta Azul?", "Crédito Cobranças da Conta Azul: o que é e como funciona", "Crédito Cobranças da Conta Azul: antecipação de boletos está disponível para minha empresa?", "Crédito: perguntas frequentes sobre antecipações de crédito das Cobranças da Conta Azul", "Crédito: perguntas frequentes das notas de produto (Adiante)", "Crédito NFS-e: antecipações de notas de serviço (indisponível)"]},
    {name:"Solicitar a antecipa\u00e7\u00e3o de receb\u00edveis", arts:7, articles:["Crédito: quais as formas de antecipar recebíveis na Conta Azul?", "Crédito: posso simular sem contratar a antecipação de recebíveis?", "Crédito Cobranças da Conta Azul: quero antecipar boletos, como faço?", "Crédito Cobranças da Conta Azul: quem pode aprovar a antecipação de recebíveis?", "Crédito Cobranças da Conta Azul: como autorizar a antecipação de recebíveis da Conta PJ da ContaAzul IP", "Crédito NF-e: como antecipar", "Crédito NF-e: como se cadastrar na antecipação de recebíveis"]},
    {name:"Monitorar limites e pagamentos da antecipa\u00e7\u00e3o", arts:12, articles:["Crédito NF-e: taxas e transferências da antecipação de recebíveis", "Crédito Cobranças da Conta Azul: como gerenciar os lançamentos financeiros da antecipação de recebíveis", "Crédito Cobranças da Conta Azul: como funciona e onde consultar o limite disponível?", "Crédito Cobranças da Conta Azul: como saber mais sobre taxas e limites?", "Crédito Cobranças da Conta Azul: fui reprovado na análise, e agora?", "Crédito Cobranças da Conta Azul: antecipação aparece em atraso após pagamento realizado, o que fazer?", "Crédito Cobranças da Conta Azul: o que fazer se o limite foi bloqueado, suspenso ou reduzido?", "Crédito Cobranças da Conta Azul: como consultar as parcelas das cobranças emitidas contra meus clientes?", "Crédito Cobranças da Conta Azul: como consultar as contas a receber da antecipação de recebível?", "Crédito Cobranças da Conta Azul: como consultar as contas a pagar da liquidação da antecipação de recebível?", "Crédito Cobranças da Conta Azul: o que acontece se atrasar o pagamento?", "Crédito NF-e: pagamento de valores antecipados"]},
  ]},
  { id:"p11", name:"Gerenciar compras e estoque", color:"#4a6030", sections:[
    {name:"Lan\u00e7ar e controlar compras de produto", arts:28, articles:["Compras: parcelas a pagar", "Compras: como controlar parcelas em aberto", "Compras: como zerar o valor da parcela a pagar", "Compras: é possível importar notas fiscais de compra em lote?", "Compras: como encontrar uma compra lançada na Conta Azul?", "Compras: como baixar XML e PDF de notas de compra em lote?", "Compras: como alterar a natureza da operação (categoria) da compra?", "Compras: como impedir que pedidos de compra gerem financeiro?", "Compras: como registrar pagamentos de fornecedores com cheque pré-datado?", "Compras: como excluir compras recorrentes futuras?", "Compras: como excluir uma compra na Conta Azul", "Compras: como a natureza da operação impacta estoque e financeiro", "Compras: quais cadastros posso fazer no menu Compras", "Compras: como lançar compra de produto", "Compras: como preencher compra de produto", "Compras: o que preencher em Informações da compra de produto?", "Compras: o que preencher em Itens da compra de produto?", "Compras: o que preencher em Informações de pagamento na compra de produto?", "Compras: como usar mais de uma forma de pagamento", "Compras: controle por tipo de movimento", "Compras: o que cada tipo de movimento representa?", "Compras: como cadastrar e editar o tipo de movimento", "Compras: como identificar a origem de uma nota fiscal de produto", "Compras: como editar", "Compras: como clonar compra de produto", "Compras: quais categorias da compra movimentam ou não o estoque", "Compras: por que a entrada da nota não atualizou o estoque?", "Compras: como excluir"]},
    {name:"Cadastrar e gerenciar produtos", arts:25, articles:["Produtos: como cadastrar", "Produtos: como preencher o cadastro", "Produtos: como clonar", "Produtos: como editar", "Produtos: como preencher o GTIN", "Produtos: informações de E-commerce no cadastro do produto", "Produtos: como gerar e imprimir etiquetas", "Produtos: como cadastrar marcas de produto", "Produtos: como excluir o cadastro de um produto", "Produtos: como inativar o cadastro de um produto", "Produtos: quando excluir e quando inativar um item do estoque", "Produtos: como reativar o cadastro de um produto", "Tabela de preços: o que é e como funciona", "Unidades de medida: o que é e como cadastrar", "Unidades de medida: como editar", "Custo médio do produto: como é calculado", "Custo médio do produto: como o sistema calcula o custo médio na primeira compra?", "Custo médio do produto: como consultar o custo médio do produto?", "Custo médio do produto: o custo médio será atualizado se eu emitir uma nota fiscal de devolução da compra?", "Custo médio do produto: o custo médio será atualizado se eu emitir uma nota fiscal de devolução de venda?", "Custo médio do produto: simulação de cálculo", "Vendas: como faço para excluir uma tabela de preços?", "Produtos: posso controlar o número de série dos meus produtos na Conta Azul Pro?", "Estoque: como cadastrar categoria de produto", "Estoque: como configurar a série e numeração das notas fiscais"]},
    {name:"Monitorar entrada e sa\u00edda de estoque", arts:19, articles:["Produtos: como realizar reserva no estoque", "Produtos: como funciona a conversão de unidade de medida", "Produtos: o que é estoque reservado e como liberar para venda", "Estoque: como editar disponibilidade e custo médio de vários produtos de uma vez", "Estoque: como funciona a tela de Situação do estoque", "Estoque: como funciona a tela de Movimentações manuais", "Estoque: Configurações do estoque", "Estoque: o que é e como funcionam os Locais de estoque", "Estoque: como cadastrar um local de estoque", "Estoque: como informar o local de estoque em Compras ou Vendas", "Estoque: como funciona o inventário por local de estoque", "Estoque: como transferir produtos entre locais de estoque", "Estoque: como funciona a verificação do saldo em estoque", "Estoque: como configurar a verificação do saldo em estoque", "Estoque: como configurar o alerta de estoque mínimo e máximo", "Estoque: o que fazer se o alerta de estoque mínimo não chegar?", "Estoque: como registrar saída manual sem emitir nota fiscal", "Estoque: por que os valores do Histórico de movimentações podem parecer diferentes?", "Vendas: quais naturezas de operação movimentam o estoque na venda?"]},
    {name:"Gerenciar movimenta\u00e7\u00e3o de estoque", arts:9, articles:["Gestão de estoque: o que é e como funciona", "Relatórios de estoque: quais são e como funcionam", "Relatórios de estoque: o que é e como funciona o relatório de Curva ABC", "Relatórios de estoque: como acessar e visualizar o relatório de Curva ABC", "Relatórios de estoque: como editar e personalizar o relatório de Curva ABC", "Relatórios de estoque: o que é e como gerenciar o Histórico de movimentações", "Relatórios de estoque: o que é e como é calculado o Giro de estoque", "Relatórios de estoque: o que é e como gerenciar o Posição de estoque", "Relatórios de estoque: como utilizar o Giro de Estoque e Posição de Estoque na nova tela de relatórios"]},
    {name:"Gerenciar kits e varia\u00e7\u00f5es de produtos", arts:7, articles:["Produtos: como funciona a variação ou grade", "Produtos: como vincular produtos com variação ou grade", "Produtos: como ajustar o saldo de estoque de cada produto com variação", "Produtos: é possível converter um produto com variação em produto simples?", "Kit de produtos: o que é e como funciona", "Kit de produtos: por que os itens aparecem separados na nota fiscal?", "Kit de produtos: como funciona o controle de estoque dos componentes"]},
    {name:"Manter o invent\u00e1rio atualizado", arts:6, articles:["Inventário: o que é e como fazer", "Inventário: é possível fazer com data retroativa?", "Inventário: por que o estoque não foi atualizado após a importação da planilha do inventário?", "Inventário: por que alguns produtos não aparecem e como incluir itens com saldo zero", "Inventário: como desfazer ou excluir um inventário?", "Inventário - Erro: Não é possível criar um inventário do tipo geral enquanto houver inventário(s) de produtos selecionados em andamento"]},
    {name:"Acompanhar relat\u00f3rios de compras e estoque", arts:2, articles:["Relatórios de compras: quais são e como funcionam", "Relatórios de compras: quais são os relatórios para análise de compras"]},
    {name:"Realizar backups e download de anexos", arts:6, articles:["Conta Azul Mais: como fazer o download de anexos da Conta Azul Pro", "Conta Azul Mais: como fazer o backup de ex-clientes da Conta Azul Pro", "Conta Azul Mais: atendimento de solicitações de clientes", "Conta Azul Mais: como fazer a reunião virtual com os colaboradores do seu escritório", "Conta Azul Mais: integração com SIEG", "Conta Azul Mais: integração com Nucont"]},
  ]},
  { id:"p12", name:"Analisar relat\u00f3rios e dashboards", color:"#6a3080", sections:[
    {name:"Acompanhar fluxo de caixa", arts:13, articles:["Relatórios financeiros: como interpretar o relatório Fluxo de caixa diário", "Relatórios financeiros: como acessar o relatório Fluxo de caixa diário", "Relatórios financeiros: relatório de Fluxo de Caixa Diário na nova tela de relatórios", "Relatórios financeiros: como interpretar o relatório Fluxo de caixa mensal", "Relatórios financeiros: como acessar o Fluxo de caixa mensal e quais os filtros disponíveis", "Relatórios financeiros: quais as versões do Fluxo de caixa mensal", "Relatórios financeiros: como os valores previstos e realizados são calculados no Fluxo de caixa mensal", "Relatórios financeiros: por que os valores do Fluxo de caixa mensal podem diferir do Extrato de movimentações", "Relatórios financeiros: como criar um lançamento que conste apenas como realizado no Fluxo de caixa mensal", "Relatórios financeiros: como interpretar o relatório de fluxo de caixa", "Relatórios financeiros: relatório de Fluxo de Caixa Mensal na nova tela de relatórios", "Relatórios financeiros: como análisar os relatórios de fluxo de caixa", "Conta Azul de Bolso: como funciona o Fluxo de caixa diário no app?"]},
    {name:"Analisar DRE: receitas, custos e despesas", arts:9, articles:["DRE: como acessar a Demonstração do Resultado do Exercício (DRE) Gerencial", "DRE: o que é e como funciona o relatório DRE gerencial", "DRE: como funciona o DRE Gerencial (Padrão) na nova tela de relatórios", "DRE: como interpretar o DRE Gerencial", "DRE: o que significam os grupos do DRE Gerencial", "DRE: quais são os tipos de relatórios DRE", "DRE: como configurar categorias financeiras padrão", "Relatório DRE: como configurar categorias e personalizar DRE", "DRE: quais são os relatórios para análise de visões de caixa"]},
    {name:"Monitorar a sa\u00fade financeira", arts:16, articles:["Relatórios financeiros: como acessar, filtrar e exportar relatórios", "Relatórios financeiros: como interpretar o relatório de análise de categorias financeiras", "Relatórios financeiros: como interpretar o relatório de centros de custo", "Relatórios financeiros: como interpretar o relatório de contas a pagar", "Relatórios financeiros: como interpretar o relatório de contas a receber", "Relatórios financeiros: o que encontro em Situação financeira por vendedores?", "Relatórios financeiros: relatório de análise de pagamento e recebimento na nova tela de relatórios", "Relatórios financeiros: relatório de lançamentos no caixa na nova tela de relatórios", "Relatórios financeiros: como interpretar o relatório Lançamentos no caixa", "Relatórios financeiros: como interpretar o relatório Posição de contas por cliente/fornecedor", "Relatórios financeiros: como analisar o rateio de centros de custo e categorias", "Relatórios financeiros: como analisar rateio em lançamentos baixados", "Relatórios financeiros: como analisar rateio em lançamentos em aberto", "Relatórios financeiros: o que é exportado no XLS de lançamentos com rateio", "Relatórios financeiros: como interpretar o relatório de análise de Recebimentos", "Relatórios financeiros: como interpretar o relatório de análise de Pagamentos"]},
    {name:"Entender a Vis\u00e3o de compet\u00eancia vs caixa", arts:7, articles:["Visão de competência: o que é e como funciona", "Visão de competência: como criar lançamentos", "Visão de competência: como editar lançamentos", "Visão de competência: como personalizar colunas", "Visão de competência: como excluir lançamento", "Visão de competência: como conferir lançamentos e comparar com o DRE", "Lançamentos financeiros: quais são as diferenças entre Visão de Competência e Visão de Caixa"]},
    {name:"Esclarecer d\u00favidas frequentes de relat\u00f3rios", arts:37, articles:["Relatórios: como funciona o inventário para Sped e Sintegra na Conta Azul?", "Relatórios: quando usar cada relatório financeiro?", "Relatórios: a Conta Azul possui Balanço Patrimonial?", "Relatórios: como gerar relatório que mostre a data de vencimento das vendas?", "Relatórios: como gerar o SPED Fiscal na Conta Azul?", "Relatórios: por que o total do gráfico de visão geral é diferente do painel de vendas?", "Relatórios: é possível gerar o Livro Caixa Rural na Conta Azul?", "Financeiro: como alterar ou remover a categoria automática de impostos nas vendas na Conta Azul?", "Financeiro: como fazer o relatório DRE Gerencial exibir planos de contas cadastrados?", "Financeiro: como organizar centros de custo em relatórios?", "Categoria financeira e centro de custo: como saber meu resultado de despesas por centro de custo", "Categoria financeira e centro de custo: como saber meu resultado de receitas por centro de custo", "Financeiro: como evitar que lançamentos não empresariais afetem o DRE?", "Financeiro: por que lançamentos aparecem no DRE e como gerenciar categorias?", "Financeiro: como garantir que dados de competência apareçam corretamente na DRE?", "Financeiro: por que vendas não aparecem no relatório DRE?", "Financeiro: como configurar categorias de DRE para exibir linhas específicas?", "Relatórios: como habilitar descontos incondicionais na DRE?", "Financeiro: como classificar corretamente lançamentos de impostos no DRE?", "Financeiro: como obter apoio para configurar plano de contas e analisar finanças?", "Financeiro: como garantir que o relatório DRE exiba todas as receitas?", "Financeiro: como vincular categorias de saída ao DRE?", "Financeiro: como resolver inconsistências no relatório DRE?", "Financeiro: como garantir que todas as receitas apareçam no DRE gerencial?", "Financeiro: como saber se categorias financeiras estão vinculadas ao DRE?", "Financeiro: como verificar se todas as categorias com lançamento estão contempladas no DRE?", "Financeiro: é possível criar subitens dentro das categorias da DRE?", "Financeiro: como alterar a classificação de despesas na DRE?", "Financeiro: como configurar categorias para que apareçam no DRE?", "Financeiro: como configurar e incluir categorias financeiras para que apareçam no relatório DRE, incluindo o DRE Gerencial?", "Financeiro: como personalizar a estrutura do DRE?", "Financeiro: como configurar, vincular e personalizar categorias no DRE, incluindo o DRE gerencial?", "Financeiro: como resolver divergência de valor em relatórios financeiros?", "Financeiro: por que o saldo diário e mensal do fluxo de caixa são inconsistentes?", "Financeiro: por que relatórios financeiros apresentam valores divergentes?", "Financeiro: por que os valores no fluxo de caixa não batem com contas a receber?", "Financeiro: o DRE considera a competência de cada parcela?"]},
    {name:"Explorar relat\u00f3rios gerenciais", arts:15, articles:["Relatórios: conheça os relatórios da Conta Azul", "Agendador de relatórios: o que é e como funciona", "Agendador de relatórios: como agendar o envio de relatórios pela Conta Azul Pro?", "Agendador de relatórios: como agendar o envio diário de relatórios pela Conta Azul Pro?", "Agendador de relatórios: como agendar o envio semanal de relatórios pela Conta Azul Pro?", "Agendador de relatórios: como agendar o envio mensal de relatórios pela Conta Azul Pro?", "Agendador de relatórios: como agendar o envio anual de relatórios pela Conta Azul Pro?", "Agendador de relatórios: como editar o agendamento do envio de relatórios pela Conta Azul Pro?", "Agendador de relatórios: como clonar o agendamento do envio de relatórios pela Conta Azul Pro?", "Agendador de relatórios: como excluir o agendamento do envio de relatórios pela Conta Azul Pro?", "Agendador de relatórios: é possível agendar o envio de relatórios por WhatsApp?", "Agendador de relatórios: não estou recebendo os e-mails com os relatórios agendados, o que fazer?", "Agendador de relatórios: como visualizar relatórios recebidos", "Visão geral: o que é e como funciona", "Relatórios: quais relatórios são exportados em PDF na Conta Azul?"]},
    {name:"Gerenciar o desempenho comercial", arts:11, articles:["Relatórios de vendas: como funcionam na nova tela de relatórios", "Relatórios de vendas: quais são e como funcionam", "Relatórios de vendas: quais são os relatórios para análise de margem de lucro", "Relatórios de vendas: quais são os relatórios para análise de clientes", "Relatórios de vendas: quais são os relatórios para análise de produtos", "Relatórios de vendas: quais são os relatórios para análise de serviços", "Relatórios de vendas: o que encontro em Situação financeira por cliente e serviço?", "Relatórios de vendas: o que encontro em Relação detalhada de serviços prestados?", "Relatórios de vendas: quais são os relatórios para análise de vendas e orçamentos", "Relatórios de vendas: quais são os relatórios para análise de vendedores", "Frente de caixa (PDV): como utilizar o Relatório Vendas por vendedor no PDV"]},
    {name:"Criar relat\u00f3rios personaliz\u00e1veis com IA", arts:6, articles:["Novidade: a nova tela de relatórios da Conta Azul", "Relatórios: como ativar o serviço adicional de Relatórios personalizados", "Relatórios: como criar e editar relatórios", "Relatórios: como começar a usar", "Relatórios: entenda o significado dos campos personalizáveis", "Relatórios: como exportar relatórios"]},
    {name:"Analisar dashboards de carteira de clientes", arts:16, articles:["Dashboard da Conta Azul Mais: o que é e como funciona", "Dashboard da Conta Azul Mais: visões disponíveis", "Dashboard da Conta Azul Mais: Dashboard de Fluxo de caixa", "Dashboard da Conta Azul Mais: Dashboard de Contas a pagar e receber", "Dashboard da Conta Azul Mais: Dashboard de DRE", "Dashboard da Conta Azul Mais: Dashboard de Vendas e contratos", "Dashboard da Conta Azul Mais: como liberar acesso para usuários", "Dashboard da Conta Azul Mais: como aplicar e remover filtros", "Dashboard da Conta Azul Mais: como visualizar e navegar entre clientes", "Dashboard da Conta Azul Mais: como funciona o Dashboard para grupos de clientes", "Dashboard da Conta Azul Mais: como criar um Dashboard por Grupo de clientes?", "Dashboard da Conta Azul Mais: como consultar as empresas que fazem parte de um Grupo de clientes?", "Dashboard da Conta Azul Mais: como editar o Grupo de clientes?", "Dashboard da Conta Azul Mais: como excluir o Grupo de clientes?", "Dashboard da Conta Azul Mais: quais análises estão disponíveis no Dashboard por Grupo de clientes?", "Dados de acesso: como obter acesso ao dashboard do Conta Azul?"]},
    {name:"Visualizar e organizar a base de clientes", arts:8, articles:["Conta Azul Mais: como acessar e gerenciar lista de clientes", "Conta Azul Mais: como filtrar e ordenar clientes por situação", "Conta Azul Mais: como acessar a Conta Azul Pro do cliente", "Conta Azul Mais: como acessar dados cadastrais de clientes conectados", "Conta Azul Mais: clientes compartilhados entre contador e BPO financeiro", "Conta Azul Mais: como utilizar o relatório de uso e alertas", "Conta Azul Mais: qual a diferença entre a listagem de clientes e a listagem de licenças?", "Conta Azul Mais: como desconectar cliente"]},
    {name:"Entender o Extrato de movimenta\u00e7\u00f5es", arts:3, articles:["Extrato de movimentações: o que é e como funciona", "Conta Azul de Bolso: como funciona o Extrato de movimentações no app?", "Lançamentos financeiros: quais são as diferenças entre Extrato de movimentações e Contas a pagar ou a receber"]},
  ]},
  { id:"p13", name:"Integrar via API com outros sistemas", color:"#4a3080", sections:[
    {name:"Conhecer aplicativos integrados", arts:42, articles:["Integração API: como acessar todas as integrações da Conta Azul", "Integração API: como ativar um aplicativo integrado na Conta Azul", "Integração API: por que é solicitado um código ao ativar um aplicativo?", "Integração API: como sugerir um novo aplicativo para integrar com a Conta Azul", "Integração API: o sistema que uso não está na lista de sistemas integrados, o que fazer?", "Integração API - Erro: mensagem genérica ao ativar ou desativar", "Integração API: como integrar Avisa App com a Conta Azul", "Integração API: como integrar Amazon com a Conta Azul", "Integração API: como integrar DJPDV com a Conta Azul", "Integração API: como integrar Erathos com a Conta Azul", "Integração API: como integrar Integrai com a Conta Azul", "Integração API: como integrar Gluo com a Conta Azul", "Integração API: como integrar InfinitePay com a Conta Azul", "Integração API: como integrar Zappy e Conta Azul", "Integração API: como integrar Tiflux com a Conta Azul", "Integração API: como integrar Shopee com a Conta Azul", "Integração API: como integrar Auvo com a Conta Azul", "Integração API: como integrar Controladoria Digital 4.0 com a Conta Azul", "Integração API: como integrar Guru com a Conta Azul", "Integração API: como integrar Hotmart com a Conta Azul", "Integração API: como integrar IRecebi com a Conta Azul", "Integração API: como integrar Jestor com a Conta Azul", "Integração API: como integrar LOG CT-e com a Conta Azul", "Integração API: como integrar Loja Integrada com a Conta Azul", "Integração API: como integrar Mercado Livre com a Conta Azul", "Integração API: como integrar Mercos com a Conta Azul", "Integração API: como integrar Moskit com a Conta Azul", "Integração API: como integrar Nekt com a Conta Azul", "Integração API: como integrar Nuvemshop com a Conta Azul", "Integração API: como integrar Paripassu com a Conta Azul", "Integração API: como integrar Pipedrive com a Conta Azul", "Integração API: como integrar a Pluga com a Conta Azul", "Integração API: como integrar Régua de Cobrança com a Conta Azul", "Integração API: como integrar RevTrack com a Conta Azul", "Integração API: como integrar SOC com a Conta Azul", "Integração API: como integrar Teamgram com a Conta Azul", "Integração API: como integrar Tray com a Conta Azul", "Integração API: como integrar T.I. Saúde com a Conta Azul", "Integração API: como integrar VExpenses com a Conta Azul", "Integração API: como integrar Vnda com a Conta Azul", "Integração API: como integrar WBudget com a Conta Azul", "Integração API: como funciona a integração com a Pluga?"]},
    {name:"Consultar a documenta\u00e7\u00e3o da API", arts:4, articles:["API Conta Azul: Desenvolva sua integração", "Integração API: como integrar a Conta Azul com outros sistemas via API", "Integração API: como criar minha própria integração?", "Integração API: perguntas frequentes de desenvolvedores"]},
    {name:"Buscar e transmitir notas fiscais automaticamente", arts:24, articles:["Conta Azul Mais: o que é e como fazer a Integração Fiscal", "Conta Azul Mais: como adicionar clientes por planilha na Integração Fiscal", "Conta Azul Mais: como adicionar clientes individualmente na Integração Fiscal", "Conta Azul Mais: como configurar a busca automática de notas fiscais na Integração Fiscal", "Conta Azul Mais: como configurar a transmissão automática de notas fiscais na Integração Fiscal", "Alterdata e Conta Azul: como configurar a transmissão automática de notas fiscais", "SCI e Conta Azul: como configurar a transmissão automática de notas fiscais", "SCI e Conta Azul: como configurar e obter o código na Conta Azul Mais", "SCI e Conta Azul: como importar notas fiscais da Conta Azul", "Busca automática de NFS-e: como gerar ou vincular à venda", "Busca automática de NF-e recebidas: como configurar", "Busca automática de NF-e recebidas: confirmar ou recusar compra", "Busca automática de NFS-e prestados: o que é e como funciona", "Compra de produto: vincular ou gerar compra na importação da nota fiscal", "Busca automática de NFS-e prestados: como fazer", "Busca automática de NFS-e tomados: como fazer", "Busca automática de NF-e recebidas: não está buscando as minhas notas de compra automaticamente. E agora?", "Busca automática de NFS-e prestados: consulte as cidades homologadas", "Busca automática de NF-e recebidas | Erro: 656 - Consumo Indevido", "Busca automática de NFS-e tomados: consulte as cidades homologadas", "Busca automática de NF-e recebidas | Erro: O código do produto está duplicado", "Importação de XML de NFS-e: como criar venda automática", "Busca automática: até quando a Conta Azul busca minhas notas já emitidas?", "Busca automática de NF-e recebidas: o que verificar quando as notas não são importadas automaticamente?"]},
  ]},
  { id:"p14", name:"Colaborar com o contador", color:"#3a6040", sections:[
    {name:"Conectar e automatizar envio ao contador", arts:11, articles:["Conexão com parceiros: como conectar seu contador ou BPO Financeiro à Conta Azul Pro", "Conexão com parceiros: quais são as vantagens de ter o contador conectado?", "Conexão com parceiros: quantos e quais parceiros posso conectar?", "Conexão com parceiros: o que significa autorizar o acesso do parceiro às informações do ERP?", "Conexão com parceiros: o que fazer se o parceiro não receber o convite de conexão", "Conexão com parceiros: como enviar notas fiscais ao contador", "Conexão com parceiros: o que significa o bloqueio do período ativo?", "Conexão com parceiros: enviar mensagens e documentos para o contador", "Conexão com parceiros: como funciona a reunião virtual com meu contador ou BPO financeiro", "Conexão com parceiros: como acessar a reunião virtual com meu contador ou BPO financeiro", "Conexão com parceiros: como desconectar um parceiro da Conta Azul Pro"]},
    {name:"Iniciar a jornada de conex\u00e3o com o Dom\u00ednio", arts:14, articles:["Domínio e Conta Azul: conheça a integração entre Domínio e Conta Azul", "Domínio e Conta Azul: como integrar", "Domínio e Conta Azul: passo a passo para a jornada de integração", "Domínio e Conta Azul: como realizar a integração com Domínio em lote", "Domínio e Conta Azul: como realizar a integração do plano de contas Domínio com a Conta Azul?", "Domínio e Conta Azul: como sincronizar plano de contas", "Domínio e Conta Azul: como sincronizar plano de contas na Etapa 2. Importar configurações", "Domínio e Conta Azul: como sincronizar plano de contas na Etapa 3. Contas analíticas", "Domínio e Conta Azul: como sincronizar plano de contas na Etapa 4. Regras auxiliares", "Domínio e Conta Azul: como sincronizar plano de contas na Etapa 5. Categorias financeiras", "Domínio e Conta Azul: como sincronizar plano de contas na Etapa 6. Concluído", "Domínio e Conta Azul: como visualizar o plano de contas sincronizado", "Domínio e Conta Azul: como sincronizar plano de contas na Conta Azul Mais?", "Domínio e Conta Azul: como salvar modelo de configurações"]},
    {name:"Realizar a exporta\u00e7\u00e3o e fechamento cont\u00e1bil", arts:14, articles:["Exportação contábil: como realizar a exportação contábil", "Exportação contábil: como configurar a Etapa 1 de Cliente ou Plano de contas", "Exportação contábil: como configurar a Etapa 2 de Contas bancárias", "Exportação contábil: como configurar as Etapas 3 e 4 de Recebimentos e Vendas & Pagamentos e Compras", "Exportação contábil: como configurar a Etapa  5 de Histórico", "Exportação contábil: como configurar a Etapa 6 de Resumo", "Exportação contábil: como configurar a Etapa 7 de Download", "Exportação contábil: como consultar última exportação contábil", "Exportação contábil: como realizar o bloqueio de período", "Exportação contábil: como bloquear um período?", "Exportação contábil: como liberar ou desbloquear um período?", "Conta Azul Mais: como fazer a exportação via arquivo para outros sistemas contábeis", "Exportação contábil: como realizar a importação do Plano de categorias", "Exportação contábil: qual a diferença entre Plano de categorias e Plano de contas?"]},
    {name:"Integrar a Conta Azul Mais com sistema cont\u00e1bil", arts:26, articles:["Conta Azul Mais: quais são os sistemas contábeis integrados com a Conta Azul Mais?", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Alterdata", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Athenas 3000", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Calima", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Contmatic", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Cuca fresca", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Dexion", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Domínio", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil E-Contab", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil EBS Cordilheira", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Exactus", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Folhamatic", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Fortes", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil JB Cepil", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Mastermaq", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Nasajon Gold", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Nasajon SQL", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Netspeed", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil PH Software", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Prosoft", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Questor", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Sage", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil SCI Linha Visual", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil SCI Único", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil Tron", "Exportação contábil: como integrar a Conta Azul Mais e o sistema contábil WK Radar"]},
    {name:"Transmitir lan\u00e7amentos e documentos fiscais", arts:16, articles:["Domínio e Conta Azul: como realizar a transmissão de lançamentos financeiros", "Domínio e Conta Azul: como ativar a rotina automática para importação de lançamentos contábeis", "Domínio e Conta Azul: como fazer a importação de contas a pagar", "Domínio e Conta Azul: como ver lançamentos da importação do Domínio no painel", "Domínio e Conta Azul: como importar configurações da exportação via arquivo para a integração automática", "Domínio e Conta Azul: como importar documentos fiscais e baixas para o Domínio", "Domínio e Conta Azul: como configurar a transmissão de notas fiscais", "Domínio e Conta Azul: como configurar as transmissões contábeis", "Domínio e Conta Azul: como configurar as Categorias financeiras na transmissão contábil", "Domínio e Conta Azul: como configurar a Provisão fiscal + baixas financeiras na transmissão contábil", "Domínio e Conta Azul: como configurar as Baixas financeiras por cliente e fornecedor na transmissão contábil", "Domínio e Conta Azul: como configurar as Baixas financeiras sem provisão na transmissão contábil", "Domínio e Conta Azul: como configurar as Regras auxiliares na transmissão contábil", "Domínio e Conta Azul: como configurar as Contas analíticas na transmissão contábil", "Domínio e Conta Azul: como ajustar as Configurações gerais na transmissão contábil", "Domínio e Conta Azul: como configurar a Provisão financeira + baixas financeiras na transmissão contábil"]},
    {name:"Monitorar hist\u00f3rico e status das transmiss\u00f5es", arts:8, articles:["Domínio e Conta Azul: como funciona o histórico de transmissão para o Domínio", "Domínio e Conta Azul: como resolver as pendências", "Domínio e Conta Azul: como resolver as pendências de Categorias financeiras", "Domínio e Conta Azul: como resolver as pendências de Contas contábeis analíticas", "Domínio e Conta Azul: como resolver as pendências de Centro de custos", "Domínio e Conta Azul: como resolver as pendências de Regras auxiliares", "Domínio e Conta Azul: como resolver as pendências de Transferências", "Domínio e Conta Azul: perguntas frequentes sobre a conexão Conta Azul e Domínio - Thomson Reuters"]},
    {name:"Controlar o fluxo de documentos enviados", arts:8, articles:["Conta AI Captura: como enviar documentos via e-mail para a Conta AI Captura?", "Conta AI Captura: como enviar documentos via WhatsApp para a Conta AI Captura?", "Conta AI Captura: como criar Grupo de WhatsApp para a Conta AI Captura?", "Conta AI Captura: como revisar e vincular boletos DDA importados via Conta AI Captura?", "Conta AI Captura: como revisar e editar lançamentos importados", "Conta AI Captura: como transformar receita em despesa (ou vice-versa)", "Conta AI Captura: como visualizar lançamentos importados no Financeiro", "Conta AI Captura: como acompanhar documentos importados e pendentes"]},
    {name:"Conectar e vincular empresas \u00e0 Conta Azul Mais", arts:6, articles:["Conta Azul Mais: como atribuir uma licença da Conta Azul Pro a um cliente pela Conta Azul Mais", "Conta Azul Mais: como vincular uma conta já adquirida da Conta Azul Pro a um cliente na Conta Azul Mais", "Conta Azul Mais: ativar licença para cliente conectado", "Conta Azul Mais: como adicionar seus clientes via planilha", "Conta Azul Mais: gerenciar clientes a partir da indicação", "Conta Azul Mais: acompanhar a jornada de clientes recomendados"]},
    {name:"Explorar o painel e treinamentos para parceiros", arts:14, articles:["O que é a Conta Azul Mais?", "Conta Azul Mais: treinamentos para parceiros", "Conta Azul Mais: primeiros passos para parceiros Conta Azul", "Conta Azul Mais: o que encontro no painel do parceiro", "Conta Azul Mais: como entender as configurações pendentes", "Conta Azul Mais: como entender as notificações no painel", "Conta Azul Mais: conta demonstrativa na CA Pro", "Conta Azul Mais: como informar e editar os dados da empresa", "Conta Azul Mais: como adicionar logotipo no painel", "Conta Azul Mais: cadastro de usuários", "Conta Azul Mais: como localizar o ID suporte", "Conta Azul Mais: como recuperar a senha de acesso", "Conta Azul Mais: como importar Certificado Digital A1 da contabilidade", "Dados de acesso: como resetar o acesso ao painel?"]},
    {name:"Conhecer a estrutura e n\u00edveis da parceria", arts:10, articles:["Programa de Parceria: Conheça o Programa de Parceria Conta Azul", "Programa de Parceria: tudo o que você precisa saber sobre o Portal da parceria", "Programa de Parceria: o que posso gerenciar na Conta Azul Mais", "Programa de Parceria: benefícios liberados por nível", "Programa de Parceria: os meus pontos podem ser descontados?", "Programa de Parceria: quais as vantagens de ser uma empresa Certificada Conta Azul", "Programa de Parceria: como conseguir mais pontos no novo Programa de Parceria Conta Azul?", "Programa de Parceria: quais são os benefícios por nível no novo Programa de Parceria Conta Azul?", "Programa de Parceria: como funciona a metodologia e pontos no novo Programa de Parceria da Conta Azul?", "Programa de Parceria: perguntas frequentes"]},
    {name:"Resgatar benef\u00edcios e gerenciar recompensas", arts:13, articles:["Programa de Parceria: como consultar o Extrato de pontos", "Programa de Parceria: como funciona e como solicitar o resgate", "Programa de Parceria: perguntas frequentes sobre a nota fiscal de serviço para resgate", "Programa de Parceria: como funciona a licença gratuita do escritório", "Programa de Parceria: como testar a licença gratuita do parceiro por 30 dias", "Programa de Parceria: como substituir a licença do lote pela licença gratuita do escritório", "Programa de Parceria: o que fazer se você já paga individualmente a licença do parceiro", "Programa de Parceria: o que acontece com a licença gratuita do escritório se eu perder pontos na parceria?", "Programa de Parceria: qual o valor da licença pelo Plano Exclusivo?", "Programa de Parceria: qual o período de solicitação e como funciona o recebimento do Resgate?", "Programa de Parceria: o que significa cada situação das movimentações do Resgate?", "Programa de Parceria: por que um valor de Resgate expira e como evitar?", "Programa de Parceria: Boleto da Conta Azul mais barato para parceiros Platina e Diamante"]},
    {name:"Conectar clientes \u00e0 plataforma e ganhar pontos", arts:9, articles:["Programa de Parceria: BPO financeiro na Conta Azul", "Programa de Parceria: como estruturar um BPO financeiro de sucesso com a Conta Azul", "Programa de Parceria: adicionar clientes", "Programa de Parceria: como recomendar a CA Pro para novos clientes", "Programa de Parceria: como recomendar a CA Pro para novos clientes via link", "Programa de Parceria: como recomendar as Cobranças da Conta Azul para seus clientes", "Programa de Parceria: como funciona a Recomendação das Cobranças da Conta Azul para seus clientes", "Programa de Parceria: como utilizar a Recomendação das Cobranças da Conta Azul", "Programa de Parceria: como transferir licença para outro parceiro"]},
    {name:"Controlar assinaturas e lote do Plano Exclusivo", arts:15, articles:["Plano Exclusivo: como vincular ao lote um cliente com licença individual?", "Plano Exclusivo: Política comercial para parceiros", "Plano Exclusivo: reajuste e atualização de planos para parceiros", "Plano Exclusivo: como gerenciar pagamentos", "Plano Exclusivo: como adquirir licenças para clientes", "Plano Exclusivo: alterar responsável pela assinatura", "Plano Exclusivo: quais as regras para alterar responsável pela assinatura?", "Plano Exclusivo: como acompanhar se o cliente aceitou ou recusou a alteração de responsável pela assinatura", "Plano Exclusivo: substituição de licenças pagas", "Plano Exclusivo: como funciona o cancelamento do lote", "Plano Exclusivo: é possível cancelar a licença para o cliente pagar diretamente?", "Plano Exclusivo: como funciona o estorno após o cancelamento do plano?", "Programa de Parceria: perguntas frequentes sobre o Plano Exclusivo e lote de licenças", "Plano Exclusivo: Unificação de Catálogo", "Plano Exclusivo: criar automaticamente cobrança de honorários no ERP do meu cliente"]},
    {name:"Encontrar um contador parceiro Conta Azul", arts:1, articles:["Encontre um Contador: o diretório de parceiros"]},
  ]},
  { id:"p15", name:"Conta Azul Mais", color:"#7a5229", sections:[
    {name:"Começar a jornada na Conta Azul Mais", arts:12, connMods:["m15"], articles:["O que é a Conta Azul Mais?","Conta Azul Mais: por onde começar","Conta Azul Mais: treinamentos para parceiros","Conta Azul Mais: primeiros passos para parceiros Conta Azul","Conta Azul Mais: o que encontro no painel do parceiro","Conta Azul Mais: como informar e editar os dados da empresa","Conta Azul Mais: como adicionar logotipo no painel","Conta Azul Mais: cadastro de usuários","Conta Azul Mais: como localizar o ID suporte","Conta Azul Mais: como recuperar a senha de acesso","Conta Azul Mais: como resetar o acesso ao painel?","Conta Azul Mais: conta demonstrativa na CA Pro"]},
    {name:"Potencializar as vantagens do Programa de Parceria", arts:25, connMods:["m15"], articles:["Programa de Parceria: Conheça o Programa de Parceria Conta Azul","Programa de Parceria: o que posso gerenciar na Conta Azul Mais","Programa de Parceria: benefícios liberados por nível","Programa de Parceria: como conseguir mais pontos?","Programa de Parceria: como funciona e como solicitar o resgate","Programa de Parceria: como funciona a licença gratuita do escritório","Programa de Parceria: perguntas frequentes","Controlar assinaturas e lote do Plano Exclusivo","Plano Exclusivo: como adquirir licenças para clientes","Plano Exclusivo: como gerenciar pagamentos","Plano Exclusivo: Política comercial para parceiros","Encontre um Contador: o diretório de parceiros"]},
    {name:"Gerenciar a carteira de clientes", arts:14, connMods:["m15"], articles:["Conta Azul Mais: como acessar e gerenciar lista de clientes","Conta Azul Mais: como filtrar e ordenar clientes por situação","Conta Azul Mais: como acessar a Conta Azul Pro do cliente","Conta Azul Mais: como acessar dados cadastrais de clientes conectados","Conta Azul Mais: clientes compartilhados entre contador e BPO financeiro","Conta Azul Mais: como utilizar o relatório de uso e alertas","Conta Azul Mais: como adicionar seus clientes via planilha","Conta Azul Mais: como desconectar cliente","Programa de Parceria: como recomendar a CA Pro para novos clientes","Programa de Parceria: como recomendar a CA Pro para novos clientes via link"]},
    {name:"Conectar Conta Azul Mais e Domínio", arts:14, connMods:["m15"], articles:["Domínio e Conta Azul: conheça a integração","Domínio e Conta Azul: como integrar","Domínio e Conta Azul: passo a passo para a jornada de integração","Domínio e Conta Azul: como realizar a integração com Domínio em lote","Domínio e Conta Azul: como sincronizar plano de contas","Domínio e Conta Azul: como configurar a transmissão de notas fiscais","Domínio e Conta Azul: como configurar as transmissões contábeis"]},
    {name:"Organizar as rotinas contábeis e fiscais", arts:20, connMods:["m15"], articles:["Exportação contábil: como realizar a exportação contábil","Exportação contábil: como configurar a Etapa 1 de Cliente ou Plano de contas","Exportação contábil: como realizar o bloqueio de período","Exportação contábil: como consultar última exportação contábil","Conta Azul Mais: como fazer a exportação via arquivo para outros sistemas contábeis","Domínio e Conta Azul: como realizar a transmissão de lançamentos financeiros","Domínio e Conta Azul: como importar documentos fiscais e baixas para o Domínio"]},
    {name:"Otimizar a produtividade com dashboards e IA", arts:18, connMods:["m15"], articles:["Dashboard da Conta Azul Mais: o que é e como funciona","Dashboard da Conta Azul Mais: visões disponíveis","Conta Azul Mais Conta AI Captura: como funciona","Conta Azul Mais Conta AI Captura: como integrar com DDA","Conta Azul Mais Conta AI Captura: como configurar o envio de documentos via WhatsApp","Conta Azul Mais: integração com Nucont","Conta Azul Mais: integração com SIEG"]},
    {name:"Explorar as melhorias e suporte especializado", arts:8, connMods:["m15"], articles:["Conheça os canais de atendimento oficiais do Programa de Parceria da Conta Azul","Conheça as novidades para parceiros da Conta Azul","Novidades da Conta Azul Mais em 2026","Novidades da Conta Azul Mais em 2025","Conta Azul Mais: Central de chamados","Conta Azul Mais: Descontinuação do módulo Obrigações"]},
  ]},
];


// ═══════════════════════════════════════════════════════
// MODULE SUMMARIES
// ═══════════════════════════════════════════════════════
const MOD_SUMMARIES = {
  "intro": {before:4, summary:"Apresentação da Conta Azul para novos atendentes: quem é o cliente, padrões de qualidade no atendimento, canais disponíveis e uso do Zendesk como ferramenta de suporte."},
  "m1":    {before:4, summary:"Configurações iniciais do ERP: cadastro de dados da empresa, controle de usuários e permissões de acesso, gerenciamento de plano e backup, e cadastros fundamentais para operação."},
  "m2":    {before:4, summary:"Fluxos de venda para cinco perfis de negócio — Serviço recorrente (contratos mensais), Serviço avulso (por demanda), Empresa para Empresa (NF-e), Empresa para Consumidor (PDV/frente de caixa) e E-commerce (integração com marketplaces). Cobre controle de recebimentos e cobranças integradas."},
  "m3":    {before:6, summary:"Emissão de notas fiscais de produto (NF-e), serviço (NFS-e) e consumidor (NFC-e). Inclui configuração do certificado digital, conferência de regras fiscais por tipo de empresa e busca automática de notas."},
  "m4":    {before:2, summary:"Registro de compras de produtos, importação de NF de compra via XML e seus reflexos automáticos no financeiro (contas a pagar) e no estoque. Nota fiscal de importação."},
  "m5":    {before:0, isNew:true, summary:"🆕 Módulo novo. Controle completo de despesas — da provisão (cadastro no contas a pagar) à baixa automática via conciliação bancária. Cobre despesas fixas, recorrentes e avulsas, além do pagamento em lote via Conta PJ. ⚠️ Regra de ouro: a baixa manual é exceção, nunca fluxo padrão."},
  "m6":    {before:1, summary:"Uso da IA para captura automática de documentos fiscais enviados via WhatsApp ou e-mail. A IA lê, classifica e lança o documento automaticamente no financeiro — reduzindo entrada manual de dados."},
  "m7":    {before:1, summary:"Ativação e uso diário da Conta PJ integrada ao ERP: cobranças por Pix, boleto e cartão; pagamentos em lote a fornecedores sem precisar acessar o internet banking."},
  "m8":    {before:3, summary:"Cadastro de contas financeiras, integração automática via Open Finance ou Conta PJ, e processo de conciliação — incluindo cartão de crédito. É aqui que o extrato bancário real bate com o financeiro do sistema, gerando baixas automáticas."},
  "m9":    {before:4, summary:"Cadastros financeiros, lançamento e controle de contas a pagar/receber, análise do financeiro e gestão de inadimplência. Módulo central de saúde financeira — mantém tudo organizado no ERP."},
  "m10":   {before:1, summary:"Gestão e solicitação de antecipação de recebíveis — permite ao cliente receber antes do prazo parcelas futuras, consultando o limite disponível e monitorando o histórico de antecipações."},
  "m11":   {before:3, summary:"Cadastro de produtos, kits e variações. Controle de saldos de entrada e saída. Fundamental para empresas de comércio e indústria que precisam rastrear o inventário e evitar ruptura de estoque."},
  "m12":   {before:2, summary:"Diferença entre telas financeiras (operacional) e relatórios gerenciais (estratégico). Fluxo de caixa, DRE, dashboards de performance e saúde financeira — para leitura e tomada de decisão."},
  "m13":   {before:1, summary:"Integrações via API com plataformas externas: marketplaces, sistemas de gestão, contabilidade. Inclui configuração e troubleshooting de conexões — busca automática de NF e transmissão de dados."},
  "m14":   {before:0, isNew:true, summary:"🆕 Módulo novo. Criação e acompanhamento de projetos dentro da Conta Azul com controle por centro de custo. Permite rastrear receitas, despesas e lucratividade por projeto — ideal para prestadores de serviço."},
  "m15":   {before:5, summary:"Programa de parceria Conta Azul Mais, conexão com o escritório contábil, exportação contábil e integração com o sistema Domínio. Fluxo completo do envio de documentos e dados ao contador parceiro."},
};

// ═══════════════════════════════════════════════════════
// VALIDATION DATA
// ═══════════════════════════════════════════════════════
const VALIDACOES = [
  {s:"ok", t:"Organizar vendas por tipo de negócio", d:"M2 expandido com 5 aulas: Serviço recorrente, Serviço avulso, Empresa para Empresa, Empresa para Consumidor e E-commerce. Mesma segmentação replicada na CA em 'Implantar na minha empresa'."},
  {s:"ok", t:"Criar módulo separado de Despesas", d:"M5 — Despesas criado como módulo independente, desvinculado do M4 Compras. Inclui roteiro do fluxo completo como aula destaque (🔥)."},
  {s:"ok", t:"Adicionar aula sobre importação de NF de compra", d:"Aula 'Importação de NF de compra + impacto financeiro/estoque' incluída no M4. Cobre o reflexo automático no contas a pagar e no saldo do estoque."},
  {s:"ok", t:"Criar módulo de Projetos (M14)", d:"M14 — Projetos criado antes da Conexão com o Contador. 5 aulas: conceito, o que são projetos, criação/configuração, lançamentos/tarefas e relatório de acompanhamento."},
  {s:"ok", t:"Apresentar Recebimentos integrado ao fluxo de vendas", d:"Coberto pela aula 'Controle de vendas e recebimento' no M2, com a aula de conceito introduzindo a jornada completa."},
  {s:"ok", t:"Artigos replicados em mais de uma seção (CA)", d:"119 artigos × 2 aparições. Exemplos: 'Vincular o certificado digital', 'Confirmar recebimentos e pagamentos', 'Configurar categorias e centros de custo'."},
  {s:"ok", t:"Separar Compras de Despesas/Contas a Pagar (CA)", d:"Categorias exclusivas 'Acompanhar despesas e pagamentos' e 'Gerenciar compras e estoque' — sem mistura."},
  {s:"ok", t:"Contas a Pagar conectada à conciliação como fluxo único", d:"'Acompanhar despesas e pagamentos' agrupa provisão, lançamento e confirmação de pagamento."},
  {s:"ok", t:"Categoria de Implantação com segmentação por tipo de negócio (CA)", d:"Nova categoria 'Implantar na minha empresa' com 5 seções: Serviço recorrente, Serviço avulso, Empresa para Empresa, Empresa para Consumidor, E-commerce."},
  {s:"ok", t:"Contas a Receber junto com vendas e cobrança (CA)", d:"Categoria 'Controlar vendas e recebimento de clientes' inclui parcelas a receber, confirmação de pagamentos e gestão de inadimplência."},
  {s:"ok", t:"Conta PJ integrada ao fluxo de despesas (CA)", d:"Categoria dedicada 'Otimizar cobranças com a Conta PJ' + artigos replicados em despesas e recebimentos."},
  {s:"ok", t:"Compras na perspectiva do cliente, não do produto (CA)", d:"Linguagem de quem usa: 'Lançar e controlar compras de produto', 'Cadastrar e gerenciar produtos', 'Monitorar entrada e saída'."},
  {s:"ok", t:"Linguagem 'despesa fixa' em vez de 'despesa recorrente'", d:"Terminologia atualizada conforme alinhamento com o Vini. O mapa e o checklist passam a registrar 'despesa fixa' como padrão de linguagem adotado."},
  {s:"ok", t:"Gestão de projetos e centro de custo como seção específica", d:"Registrado como aplicado conforme alinhamento. M14 conecta projetos ao centro de custo e à análise de lucratividade — o mapa reflete essa estrutura."},
  {s:"ok", t:"Explicação conceitual como primeira camada", d:"Registrado como aplicado conforme alinhamento. Cada módulo de treinamento passa a ter aula de conceito (🟣) como primeira camada antes dos artigos de tarefa."},
];

// ═══════════════════════════════════════════════════════
// COMMENTS SYSTEM
// — Visíveis para todos (sem filtro por autor)
// — Persistência em localStorage
// — Vinculáveis a um nó específico ou gerais
// ═══════════════════════════════════════════════════════
// ═══════════════════════════════════════════════════════
// SHARED PERSISTENCE — JSONbin.io (público, grátis)
// Comentários e posições de nós são compartilhados entre todos os leitores.
// O Bin ID fica em localStorage; na primeira abertura, um novo bin é criado
// e a URL com o ID é exibida para ser compartilhada.
// ═══════════════════════════════════════════════════════
const JSONBIN_API_KEY = '$2a$10$MuqPEVxTnz8KhRaKzuv7OOEgE8fPlzJT0HabkNS9OP0oSl7KwNRHi';
const JSONBIN_LS_KEY  = 'mapa_v6_bin_id';

let _binId = null;
let _sharedData = { comments: [], offsets: {} };
let _dataLoaded = false;

function getBinUrl(id) { return `https://api.jsonbin.io/v3/b/${id}`; }

async function ensureBin() {
  // Try URL param first (for sharing)
  const urlParams = new URLSearchParams(window.location.search);
  const paramId = urlParams.get('bin');
  if (paramId) { _binId = paramId; localStorage.setItem(JSONBIN_LS_KEY, paramId); return; }
  // Then localStorage
  _binId = localStorage.getItem(JSONBIN_LS_KEY);
  if (_binId) return;
  // Create a new bin
  try {
    const r = await fetch('https://api.jsonbin.io/v3/b', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Master-Key': JSONBIN_API_KEY,
        'X-Bin-Name': 'mapa_combinado_v6',
        'X-Private': 'false'
      },
      body: JSON.stringify({ comments: [], offsets: {} })
    });
    if (r.ok) {
      const json = await r.json();
      _binId = json.metadata?.id;
      if (_binId) {
        localStorage.setItem(JSONBIN_LS_KEY, _binId);
        // Show share hint
        setTimeout(() => {
          const shareUrl = window.location.href.split('?')[0] + '?bin=' + _binId;
          const hint = document.createElement('div');
          hint.style.cssText = 'position:fixed;bottom:60px;right:18px;background:#1e1c18;color:#f4f2ee;padding:10px 14px;border-radius:8px;font-size:11px;z-index:999;max-width:320px;line-height:1.5;box-shadow:0 4px 16px rgba(0,0,0,.3)';
          hint.innerHTML = `🔗 <strong>Bin criado!</strong> Compartilhe este link para que outros vejam comentários e posições:<br><a href="${shareUrl}" style="color:#f5c842;word-break:break-all">${shareUrl}</a><br><button onclick="this.parentNode.remove()" style="margin-top:6px;border:none;background:#444;color:#fff;border-radius:4px;padding:3px 10px;cursor:pointer;font-size:10px">Fechar</button>`;
          document.body.appendChild(hint);
        }, 1500);
      }
    }
  } catch(e) { console.warn('JSONbin create failed', e); }
}

async function loadSharedData() {
  await ensureBin();
  if (_binId) {
    try {
      const r = await fetch(getBinUrl(_binId) + '/latest', {
        headers: { 'X-Master-Key': JSONBIN_API_KEY }
      });
      if (r.ok) {
        const json = await r.json();
        _sharedData = json.record || { comments: [], offsets: {} };
        if (!_sharedData.comments) _sharedData.comments = [];
        if (!_sharedData.offsets)  _sharedData.offsets  = {};
        Object.assign(nodeOffsets, _sharedData.offsets);
      }
    } catch(e) { console.warn('JSONbin load failed, fallback to localStorage', e); }
  }
  // localStorage fallback
  if (!_sharedData.comments.length) {
    try { _sharedData.comments = JSON.parse(localStorage.getItem('mapa_v6_comments') || '[]'); } catch(e){}
  }
  if (!Object.keys(_sharedData.offsets||{}).length) {
    try { Object.assign(nodeOffsets, JSON.parse(localStorage.getItem('mapa_v6_offsets') || '{}')); } catch(e){}
  }
  _dataLoaded = true;
}

async function saveSharedData() {
  _sharedData.offsets = {...nodeOffsets};
  // localStorage fallback always
  try { localStorage.setItem('mapa_v6_comments', JSON.stringify(_sharedData.comments)); } catch(e){}
  try { localStorage.setItem('mapa_v6_offsets', JSON.stringify(nodeOffsets)); } catch(e){}
  if (!_binId) return;
  try {
    await fetch(getBinUrl(_binId), {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json', 'X-Master-Key': JSONBIN_API_KEY },
      body: JSON.stringify(_sharedData)
    });
  } catch(e) { console.warn('JSONbin save failed', e); }
}

let activeCommentNode = null; // {id, label, type} | null

const AVATAR_COLORS = ['#5b6bb0','#27500A','#501313','#633806','#4B1528','#3a6040','#0C447C','#4A1B0C','#3C3489'];
function avatarColor(name) {
  let h = 0; for (const c of name) h = (h * 31 + c.charCodeAt(0)) % AVATAR_COLORS.length;
  return AVATAR_COLORS[Math.abs(h)];
}
function initials(name) {
  const parts = name.trim().split(/\s+/);
  return parts.length >= 2 ? (parts[0][0]+parts[1][0]).toUpperCase() : name.slice(0,2).toUpperCase();
}

function loadComments() { return _sharedData.comments || []; }
function saveComments(list) {
  _sharedData.comments = list;
  saveSharedData();
  try { localStorage.setItem('mapa_v6_comments', JSON.stringify(list)); } catch(e){}
}

async function syncData() {
  const btn = document.getElementById('btn-sync');
  if (btn) { btn.textContent = '⏳'; btn.disabled = true; }
  await loadSharedData();
  render();
  updateCommentDots();
  renderComments();
  if (btn) { btn.textContent = '🔄 Sync'; btn.disabled = false; }
}

function toggleComments() {
  const panel = document.getElementById('comments-panel');
  const isOpen = panel.classList.contains('open');
  panel.classList.toggle('open', !isOpen);
  if (!isOpen) renderComments();
  // Adjust canvas offset when panel opens/closes
  const cw = document.getElementById('canvas-wrap');
  cw.style.marginLeft = (!isOpen) ? '340px' : '';
}

function setCommentNode(nodeId, nodeLabel, nodeType) {
  activeCommentNode = nodeId ? {id:nodeId, label:nodeLabel, type:nodeType} : null;
  const ctx = document.getElementById('cp-context');
  if (activeCommentNode) {
    ctx.innerHTML = `<strong style="color:#1e1c18">📌 Comentando em:</strong> <span style="color:#555">${activeCommentNode.label}</span>`;
  } else {
    ctx.textContent = 'Selecione um nó no mapa para comentar nele, ou adicione um comentário geral abaixo.';
  }
  renderComments();
}

function addComment() {
  const textEl = document.getElementById('cp-text');
  const authorEl = document.getElementById('cp-author-input');
  const text = textEl.value.trim();
  const author = authorEl.value.trim() || 'Anônimo';
  if (!text) { textEl.focus(); return; }

  const comments = loadComments();
  const comment = {
    id: Date.now() + '_' + Math.random().toString(36).slice(2,7),
    author,
    text,
    nodeId: activeCommentNode?.id || null,
    nodeLabel: activeCommentNode?.label || null,
    nodeType: activeCommentNode?.type || null,
    ts: new Date().toISOString(),
    pinned: false,
  };
  comments.unshift(comment);
  saveComments(comments);
  textEl.value = '';
  // persist author name
  try { localStorage.setItem('mapa_comment_author', author); } catch(e){}
  renderComments();
  updateCommentDots();
}

function deleteComment(id) {
  const comments = loadComments().filter(c => c.id !== id);
  saveComments(comments);
  renderComments();
  updateCommentDots();
}

function togglePin(id) {
  const comments = loadComments();
  const c = comments.find(x => x.id === id);
  if (c) c.pinned = !c.pinned;
  saveComments(comments);
  renderComments();
}

function formatDate(iso) {
  try {
    const d = new Date(iso);
    return d.toLocaleDateString('pt-BR', {day:'2-digit',month:'2-digit'}) + ' ' +
           d.toLocaleTimeString('pt-BR', {hour:'2-digit',minute:'2-digit'});
  } catch(e) { return ''; }
}

function renderComments() {
  const list = document.getElementById('cp-list');
  const countEl = document.getElementById('cp-count');
  const comments = loadComments();
  countEl.textContent = comments.length;

  // Sort: pinned first, then by date desc
  const sorted = [...comments].sort((a,b) => {
    if (a.pinned && !b.pinned) return -1;
    if (!a.pinned && b.pinned) return 1;
    return new Date(b.ts) - new Date(a.ts);
  });

  list.innerHTML = '';

  if (sorted.length === 0) {
    list.innerHTML = `<div style="text-align:center;color:#bbb;font-size:11px;padding:24px 0">
      <div style="font-size:28px;margin-bottom:8px">💬</div>
      <div>Nenhum comentário ainda.</div>
      <div style="margin-top:4px;font-size:10px">Todos os comentários são visíveis para qualquer pessoa que abrir este mapa.</div>
    </div>`;
    return;
  }

  for (const c of sorted) {
    const card = document.createElement('div');
    card.className = 'comment-card' + (c.pinned ? ' pinned' : '');
    const col = avatarColor(c.author);
    const ini = initials(c.author);
    const nodeTag = c.nodeLabel
      ? `<span class="cc-node" title="Nó relacionado">${c.nodeType === 'tr' ? '📚' : '📋'} ${c.nodeLabel}</span>`
      : `<span class="cc-node">🗺️ Geral</span>`;
    card.innerHTML = `
      <div class="cc-header">
        <div class="cc-avatar" style="background:${col}">${ini}</div>
        <span class="cc-author">${c.author}</span>
        <span class="cc-date">${formatDate(c.ts)}</span>
      </div>
      ${nodeTag}
      <div class="cc-text">${c.text.replace(/</g,'&lt;').replace(/>/g,'&gt;')}</div>
      <div class="cc-actions">
        <button class="cc-pin-btn ${c.pinned?'active':''}" title="${c.pinned?'Desafixar':'Fixar'}" onclick="togglePin('${c.id}')">
          ${c.pinned ? '📌 Fixado' : '📌 Fixar'}
        </button>
        <button class="cc-del-btn" title="Excluir comentário" onclick="deleteComment('${c.id}')">🗑</button>
      </div>`;
    // Click to navigate to the commented node
    if (c.nodeId) {
      card.style.cursor = 'pointer';
      card.addEventListener('click', e => {
        if (e.target.closest('.cc-actions')) return;
        const map = c.nodeType === 'tr' ? 'tr' : 'ca';
        // Expand relevant container first
        if (map === 'ca') { const cat = c.nodeId.split('_')[0]; collapsedCats.delete(cat); }
        if (map === 'tr') { const mod = c.nodeId.split('_')[0]; collapsedMods.delete(mod); }
        render(); scrollToNode(c.nodeId, map);
      });
    }
    list.appendChild(card);
  }
}

function updateCommentDots() {
  // After render(), add small yellow dots on nodes that have comments
  const comments = loadComments();
  const nodeIds = new Set(comments.filter(c=>c.nodeId).map(c=>c.nodeId));
  // Count per node
  const countMap = {};
  for (const c of comments) { if (c.nodeId) countMap[c.nodeId] = (countMap[c.nodeId]||0)+1; }

  document.querySelectorAll('[data-node-id]').forEach(el => {
    const id = el.getAttribute('data-node-id');
    // Remove existing dot
    el.querySelectorAll('.node-comment-dot').forEach(d=>d.remove());
    if (countMap[id]) {
      el.style.position = 'absolute'; // ensure positioned
      const dot = document.createElement('div');
      dot.className = 'node-comment-dot';
      dot.textContent = countMap[id] > 9 ? '9+' : countMap[id];
      dot.title = `${countMap[id]} comentário(s)`;
      el.appendChild(dot);
    }
  });
}

// Restore saved author name
try {
  const savedAuthor = localStorage.getItem('mapa_comment_author');
  if (savedAuthor) setTimeout(() => {
    const el = document.getElementById('cp-author-input');
    if (el) el.value = savedAuthor;
  }, 100);
} catch(e) {}

// ═══════════════════════════════════════════════════════
// ANTES — ESTRUTURA ORIGINAL DOS TREINAMENTOS
// ═══════════════════════════════════════════════════════
const MODS_BEFORE = [
  {id:"b-intro",num:"Intro",name:"Introdução",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-intro-0",n:"Quem é o cliente Conta Azul"},
    {id:"bl-intro-1",n:"Qualidade"},
    {id:"bl-intro-2",n:"Canais e Ferramentas"},
    {id:"bl-intro-3",n:"Zendesk"},
  ]},
  {id:"b-m1",num:"M1",name:"Configurações",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m1-0",n:"Introdução e Cadastro de Dados"},
    {id:"bl-m1-1",n:"Controle de Acesso e Segurança"},
    {id:"bl-m1-2",n:"Plano e Backup"},
    {id:"bl-m1-3",n:"Cadastros"},
  ]},
  {id:"b-m2",num:"M2",name:"Controle de vendas e recebimento",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m2-0",n:"Controle de vendas e recebimento de clientes"},
    {id:"bl-m2-1",n:"Consulta Serasa"},
    {id:"bl-m2-2",n:"Frente de Caixa"},
    {id:"bl-m2-3",n:"Configuração de recebimento (Terceiros e CPJ)"},
  ]},
  {id:"b-m3",num:"M3",name:"Emissão e gerenciamento de Notas Fiscais",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m3-0",n:"Certificado Digital e config. geral de NF"},
    {id:"bl-m3-1",n:"Nota Fiscal de Produto"},
    {id:"bl-m3-2",n:"Conferência de Regras Fiscais"},
    {id:"bl-m3-3",n:"Nota Fiscal de Serviço"},
    {id:"bl-m3-4",n:"Nota Fiscal de Consumidor"},
    {id:"bl-m3-5",n:"Busca Automática"},
  ]},
  {id:"b-m4",num:"M4",name:"Controle de Compras e Importação",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m4-0",n:"Registro de Compras"},
    {id:"bl-m4-1",n:"Nota Fiscal de Importação"},
  ]},
  {id:"b-m5",num:"M5",name:"Automatização de Lançamentos (IA Captura)",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m5-0",n:"Configuração e Usabilidade da IA Captura"},
  ]},
  {id:"b-m6",num:"M6",name:"Automatização de Pagamentos (Conta PJ)",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m6-0",n:"Ativação, Configuração e Uso no Dia a Dia"},
  ]},
  {id:"b-m7",num:"M7",name:"Conciliação Bancária",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m7-0",n:"Cadastro de Contas Financeiras e Integração"},
    {id:"bl-m7-1",n:"Conciliação"},
    {id:"bl-m7-2",n:"Conciliação Conta Cartão de Crédito"},
  ]},
  {id:"b-m8",num:"M8",name:"Organização Financeira",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m8-0",n:"Cadastros financeiros"},
    {id:"bl-m8-1",n:"Contas a Pagar/Receber"},
    {id:"bl-m8-2",n:"Análise do financeiro"},
    {id:"bl-m8-3",n:"Inadimplência"},
  ]},
  {id:"b-m9",num:"M9",name:"Antecipação de parcelas",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m9-0",n:"Gestão e Antecipação"},
  ]},
  {id:"b-m10",num:"M10",name:"Gestão de Estoque",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m10-0",n:"Produtos, Kit e Variações"},
    {id:"bl-m10-1",n:"Controle de saldos"},
  ]},
  {id:"b-m11",num:"M11",name:"Relatórios e Dashboards",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m11-0",n:"Diferença entre telas financeiras"},
    {id:"bl-m11-1",n:"Relatórios"},
  ]},
  {id:"b-m12",num:"M12",name:"Integração com outros sistemas",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m12-0",n:"Integrações com outros sistemas"},
  ]},
  {id:"b-m13",num:"M13",name:"Conexão com o Contador",col:"#888",bg:"#f0ede8",lessons:[
    {id:"bl-m13-0",n:"Programa de Parceria"},
    {id:"bl-m13-1",n:"Conexão e envio de dados"},
    {id:"bl-m13-2",n:"Conta Azul Mais — Painel do Contador"},
    {id:"bl-m13-3",n:"Exportação Contábil"},
    {id:"bl-m13-4",n:"Integração com a Domínio"},
  ]},
];

// Reuse map: AFTER lesson label → BEFORE lesson id (for drawing reuse lines)
// Matches by normalised label comparison
const REUSE_MAP = {
  // intro
  "Quem é o cliente Conta Azul":"bl-intro-0","Qualidade":"bl-intro-1",
  "Canais e ferramentas":"bl-intro-2","Zendesk":"bl-intro-3",
  // m1
  "Introdução e cadastro de dados":"bl-m1-0","Controle de acesso e segurança":"bl-m1-1",
  "Plano e backup":"bl-m1-2","Cadastros":"bl-m1-3",
  // m2
  "Controle de vendas e recebimento":"bl-m2-0","Consulta Serasa":"bl-m2-1",
  "Frente de caixa":"bl-m2-2","Config. recebimento (Terceiros e CPJ)":"bl-m2-3",
  // m3
  "Certificado digital e config. geral de NF":"bl-m3-0","Nota fiscal de produto":"bl-m3-1",
  "Conferência de regras fiscais":"bl-m3-2","Nota fiscal de serviço":"bl-m3-3",
  "Nota fiscal de consumidor":"bl-m3-4","Busca automática":"bl-m3-5",
  // m4
  "Registro de compras":"bl-m4-0","Nota fiscal de importação":"bl-m4-1",
  // m6 (AI Captura) reuses b-m5
  "Config. e usabilidade da IA Captura":"bl-m5-0",
  // m7 (Conta PJ) reuses b-m6
  "Ativação, configuração e uso no dia a dia":"bl-m6-0",
  // m8 (Conciliação) reuses b-m7
  "Cadastro de contas financeiras e integração":"bl-m7-0",
  "Conciliação":"bl-m7-1","Conciliação cartão de crédito":"bl-m7-2",
  // m9 (Org Financeira) reuses b-m8
  "Cadastros financeiros":"bl-m8-0","Contas a pagar/receber":"bl-m8-1",
  "Análise do financeiro":"bl-m8-2","Inadimplência":"bl-m8-3",
  // m10 reuses b-m9
  "Gestão e antecipação":"bl-m9-0",
  // m11 reuses b-m10
  "Produtos, kit e variações":"bl-m10-0","Controle de saldos":"bl-m10-1",
  // m12 reuses b-m11
  "Diferença entre telas financeiras":"bl-m11-0","Relatórios":"bl-m11-1",
  // m13 reuses b-m12
  "Integrações com outros sistemas":"bl-m12-0",
  // m15 reuses b-m13
  "Programa de parceria":"bl-m13-0","Conexão e envio de dados":"bl-m13-1",
  "Conta Azul Mais — painel do contador":"bl-m13-2",
  "Exportação contábil":"bl-m13-3","Integração com a Domínio":"bl-m13-4",
};

let showAntes = false;
let collapsedBefore = new Set();
let beforeNodes = [];

// ── CA ANTES — categories & sections from Zendesk export ──
// CA_BEFORE: 3-level structure — category → sections (each with subsections)
// Source: [Cajuca] Árvore de categorias — Conta Azul Pro only
const CA_BEFORE = [
  { id:'cab-p1', label:'Conta Azul Pro', secs:[
    { id:'cab-s01', label:'Começar a usar a Conta Azul Pro', subs:[
      'Aprender a utilizar a Conta Azul Pro',
      'Configurar os dados iniciais da empresa',
      'Controlar usuários e acessos',
      'Organizar meus cadastros (Clientes, Fornecedores, Serviços e Transportadoras)',
      'Simplificar a gestão com o app Conta Azul de Bolso',
      'Proteger meus dados e manter a conexão',
      'Administrar minha assinatura',
      'Falar com a Conta Azul',
    ]},
    { id:'cab-s02', label:'Controlar vendas e recebimento de clientes', subs:[
      'Organizar ordem de serviços e orçamentos',
      'Registrar e controlar vendas',
      'Acompanhar contratos recorrentes',
      'Realizar vendas no PDV',
      'Controlar parcelas a receber de clientes',
      'Consultar guias rápidos de vendas e recebimentos',
    ]},
    { id:'cab-s03', label:'Emitir e gerenciar notas fiscais', subs:[
      'Vincular meu certificado digital',
      'Emitir nota fiscal de produto (NF-e)',
      'Emitir nota fiscal de serviço (NFS-e)',
      'Emitir cupom fiscal (NFC-e)',
      'Corrigir, cancelar ou devolver nota fiscal',
      'Solucionar rejeições da nota fiscal',
      'Importar notas fiscais emitidas fora da Conta Azul',
      'Consultar termos e regras fiscais',
    ]},
    { id:'cab-s04', label:'Acompanhar despesas e pagamentos', subs:[
      'Gerenciar despesas e contas a pagar',
      'Importar boletos via DDA',
      'Automatizar pagamentos com a Conta PJ',
      'Pagar boletos, guias e transferências pelo ERP',
      'Controlar aprovações e multiusuários de pagamento',
    ]},
    { id:'cab-s05', label:'Automatizar lançamentos com Conta AI Captura', subs:[
      'Entender como funciona',
    ]},
    { id:'cab-s06', label:'Otimizar cobranças com a Conta PJ', subs:[
      'Conhecer a Conta PJ da Conta Azul',
      'Ativar minha Conta PJ Conta Azul',
      'Analisar extrato da Conta PJ Conta Azul',
      'Configurar o envio de cobranças via Pix, Boleto ou Link de cartão da Conta PJ',
      'Consultar taxas e prazos das Cobranças da Conta Azul',
      'Agilizar correções e garantir a segurança das Cobranças da Conta Azul',
    ]},
    { id:'cab-s07', label:'Realizar conciliação bancária', subs:[
      'Conectar com o banco',
      'Bater saldo (com sugestões da IA)',
      'Corrigir saldo e resolver pendências',
    ]},
    { id:'cab-s08', label:'Organizar o financeiro', subs:[
      'Configurar categorias e centros de custo',
      'Entender a Visão de competência vs caixa',
      'Entender o Extrato de movimentações',
      'Controlar contas a pagar e a receber',
      'Esclarecer dúvidas gerais do financeiro',
      'Automatizar a gestão de inadimplência',
      'Auditar lançamentos realizados por usuários',
    ]},
    { id:'cab-s09', label:'Antecipar parcelas a receber de clientes', subs:[
      'Antecipar cobranças e crédito',
    ]},
    { id:'cab-s10', label:'Gerenciar estoque', subs:[
      'Gerenciar kits e variações de produtos',
      'Manter o inventário atualizado',
      'Analisar cadastro de produtos',
    ]},
    { id:'cab-s11', label:'Analisar relatórios e dashboards', subs:[
      'Explorar relatórios gerenciais',
      'Analisar DRE: receitas, custos e despesas',
      'Acompanhar fluxo de caixa',
      'Monitorar a saúde financeira',
      'Gerenciar o desempenho comercial',
      'Acompanhar relatórios de compras',
      'Gerenciar movimentação de estoque',
      'Criar relatórios personalizáveis Inteligência Artificial',
      'Esclarecer dúvidas frequentes de relatórios',
    ]},
    { id:'cab-s12', label:'Integrar via API com outros sistemas', subs:[
      'Conhecer aplicativos integrados',
      'Consultar a documentação da API',
    ]},
    { id:'cab-s13', label:'Colaborar com o contador', subs:[
      'Conectar e automatizar envio de documentos ao contador',
      'Encontrar um contador ou BPO financeiro parceiro Conta Azul',
    ]},
  ]},
  { id:'cab-p2', label:'Conta Azul Mais', secs:[
    { id:'cab-m01', label:'Começar a jornada na Conta Azul Mais', subs:[] },
    { id:'cab-m02', label:'Potencializar as vantagens do Programa de Parceria', subs:[
      'Controlar assinaturas e lote do Plano Exclusivo',
    ]},
    { id:'cab-m03', label:'Gerenciar a carteira de clientes', subs:[] },
    { id:'cab-m04', label:'Conectar Conta Azul Mais e Domínio', subs:[] },
    { id:'cab-m05', label:'Organizar as rotinas contábeis e fiscais', subs:[] },
    { id:'cab-m06', label:'Otimizar a produtividade com dashboards e IA', subs:[] },
    { id:'cab-m07', label:'Explorar as melhorias e suporte especializado', subs:[] },
  ]},
];

let showAntesCa = false;
let collapsedBeforeCa = new Set();
let caBeforeNodes = [];

// ── ANTES layout (mirrors computeTR but uses MODS_BEFORE, placed to the LEFT of TR) ──
function computeBefore(offsetX) {
  const totalH = MODS_BEFORE.reduce((s,m) => s + modHeightBefore(m) + V_GAP_MOD, 0) - V_GAP_MOD;
  const rootCY = totalH / 2;
  const nodes  = [];
  const rootX  = offsetX;
  nodes.push({type:'root-before', x:rootX, y:rootCY - ROOT_H/2, w:ROOT_W, h:ROOT_H,
    label:'◌ Antes', id:'root-before'});
  let curY = 0;
  for (const mod of MODS_BEFORE) {
    const collapsed = collapsedBefore.has(mod.id);
    const mH   = modHeightBefore(mod);
    const modX = rootX + ROOT_W + H_GAP_TR;
    const modCY = curY + mH/2;
    const modY  = modCY - MOD_H/2;
    nodes.push({
      type:'mod-before', x:modX, y:modY, w:MOD_W, h:MOD_H,
      label:mod.num+' · '+mod.name, id:mod.id, col:'#aaa', bg:'#eeebe5',
      collapsed, cx:modX+MOD_W, cy:modCY,
    });
    if (!collapsed) {
      const lesX = modX + MOD_W + H_GAP_LES;
      let lesY = curY;
      for (let li=0; li<mod.lessons.length; li++) {
        const les = mod.lessons[li];
        nodes.push({
          type:'les-before', x:lesX, y:lesY, w:LES_W, h:LES_H,
          label:les.n, modId:mod.id,
          id:les.id,
          cy: lesY + LES_H/2,
          rightX: lesX + LES_W,
        });
        lesY += LES_H + V_GAP_LES;
      }
    }
    curY += mH + V_GAP_MOD;
  }
  const bWidth = ROOT_W + H_GAP_TR + MOD_W + H_GAP_LES + LES_W;
  return { nodes, bWidth, bHeight: totalH };
}

function modHeightBefore(mod) {
  if (collapsedBefore.has(mod.id)) return MOD_H;
  const h = mod.lessons.length * (LES_H + V_GAP_LES) - V_GAP_LES;
  return Math.max(MOD_H, h);
}

function renderBeforeNode(n) {
  const el = document.createElement('div');
  el.style.cssText = `position:absolute;left:${n.x}px;top:${n.y}px;width:${n.w}px;height:${n.h}px;`;

  if (n.type === 'root-before') {
    el.style.cssText += `background:#4a4540;border-radius:7px;display:flex;align-items:center;justify-content:center;cursor:default;`;
    el.innerHTML = `<span style="font-size:11px;font-weight:600;color:#f4f2ee;text-align:center;line-height:1.3">◌ Antes</span>`;

  } else if (n.type === 'mod-before') {
    el.style.cssText += `background:#c8c3bc;border:1.5px solid #8a847c;border-radius:6px;
      display:flex;align-items:center;padding:0 8px;cursor:pointer;user-select:none;`;
    el.setAttribute('data-node','mod-before');
    el.innerHTML = `<span style="font-size:10px;font-weight:600;color:#2e2c28;line-height:1.2;flex:1">${truncate(n.label,38)}</span>
      <span style="font-size:8px;color:#6b6560;margin-left:4px">${n.collapsed?'▶':'▼'}</span>`;
    el.addEventListener('click', () => {
      collapsedBefore.has(n.id) ? collapsedBefore.delete(n.id) : collapsedBefore.add(n.id);
      render(); fitView(false);
    });

  } else if (n.type === 'les-before') {
    el.style.cssText += `background:#d8d3cc;border:1.2px solid #8a847c;border-radius:5px;
      display:flex;align-items:center;padding:0 7px;gap:4px;`;
    el.innerHTML = `<span style="font-size:10px;color:#3a3632;flex:1;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;">${n.label}</span>`;
  }

  beforeContainer.appendChild(el);
}

// ── TAB SWITCHING: Agora / Antes ──
let currentTab = 'after';

function switchTab(tab, btn) {
  currentTab = tab;
  document.querySelectorAll('#topbar .tb-btn').forEach(b => {
    if (b.id === 'btn-after' || b.id === 'btn-antes') b.classList.remove('active');
  });
  btn.classList.add('active');

  if (tab === 'antes') {
    showAntes = true;
    showAntesCa = true;
  } else {
    showAntes = false;
    showAntesCa = false;
  }
  render(); fitView(false);
}

// Legacy stubs (kept for compatibility with any remaining references)
function toggleAntes(btn) { switchTab(showAntes ? 'after' : 'antes', document.getElementById(showAntes ? 'btn-after' : 'btn-antes')); }

// ── CA ANTES — layout: 3 levels (category → section → subsection) ──
const CAB_ROOT_W = 130, CAB_CAT_W = 160, CAB_SEC_W = 200, CAB_SUB_W = 240;
const CAB_H_GAP_CAT = 36, CAB_H_GAP_SEC = 30, CAB_H_GAP_SUB = 28;
const CAB_CAT_H = 28, CAB_SEC_H = 22, CAB_SUB_H = 18;
const CAB_V_GAP_CAT = 14, CAB_V_GAP_SEC = 8, CAB_V_GAP_SUB = 3;

function cabSecHeight(sec) {
  const secId = sec.id;
  if (collapsedBeforeCa.has(secId)) return CAB_SEC_H;
  if (!sec.subs || sec.subs.length === 0) return CAB_SEC_H;
  const h = sec.subs.length * (CAB_SUB_H + CAB_V_GAP_SUB) - CAB_V_GAP_SUB;
  return Math.max(CAB_SEC_H, h);
}

function cabCatHeight(cat) {
  if (collapsedBeforeCa.has(cat.id)) return CAB_CAT_H;
  if (!cat.secs || cat.secs.length === 0) return CAB_CAT_H;
  const h = cat.secs.reduce((s, sec) => s + cabSecHeight(sec) + CAB_V_GAP_SEC, 0) - CAB_V_GAP_SEC;
  return Math.max(CAB_CAT_H, h);
}

function computeCABefore(offsetX) {
  const totalH = CA_BEFORE.reduce((s,c) => s + cabCatHeight(c) + CAB_V_GAP_CAT, 0) - CAB_V_GAP_CAT;
  const rootCY = totalH / 2;
  const nodes = [];
  const rootX = offsetX;
  nodes.push({type:'cab-root', x:rootX, y:rootCY - ROOT_H/2, w:CAB_ROOT_W, h:ROOT_H,
    label:'◌ Antes', id:'cab-root'});
  let curY = 0;
  for (const cat of CA_BEFORE) {
    const collapsedCat = collapsedBeforeCa.has(cat.id);
    const cH = cabCatHeight(cat);
    const catX = rootX + CAB_ROOT_W + CAB_H_GAP_CAT;
    const catCY = curY + cH / 2;
    const catY  = catCY - CAB_CAT_H / 2;
    nodes.push({
      type:'cab-cat', x:catX, y:catY, w:CAB_CAT_W, h:CAB_CAT_H,
      label:cat.label, id:cat.id, collapsed:collapsedCat,
      leftX:catX, cx:catX + CAB_CAT_W, cy:catCY,
    });
    if (!collapsedCat && cat.secs) {
      const secX = catX + CAB_CAT_W + CAB_H_GAP_SEC;
      let secCurY = curY;
      for (const sec of cat.secs) {
        const collapsedSec = collapsedBeforeCa.has(sec.id);
        const sH = cabSecHeight(sec);
        const secCY = secCurY + sH / 2;
        const secY  = secCY - CAB_SEC_H / 2;
        nodes.push({
          type:'cab-sec', x:secX, y:secY, w:CAB_SEC_W, h:CAB_SEC_H,
          label:sec.label, id:sec.id, catId:cat.id, collapsed:collapsedSec,
          leftX:secX, cx:secX + CAB_SEC_W, cy:secCY,
        });
        if (!collapsedSec && sec.subs && sec.subs.length > 0) {
          const subX = secX + CAB_SEC_W + CAB_H_GAP_SUB;
          let subCurY = secCurY;
          for (const sub of sec.subs) {
            const subId = sec.id + '-' + sub.replace(/\W/g,'').slice(0,18);
            nodes.push({
              type:'cab-sub', x:subX, y:subCurY, w:CAB_SUB_W, h:CAB_SUB_H,
              label:sub, id:subId, secId:sec.id,
              leftX:subX, cy:subCurY + CAB_SUB_H/2,
            });
            subCurY += CAB_SUB_H + CAB_V_GAP_SUB;
          }
        }
        secCurY += sH + CAB_V_GAP_SEC;
      }
    }
    curY += cH + CAB_V_GAP_CAT;
  }
  const cabWidth = CAB_ROOT_W + CAB_H_GAP_CAT + CAB_CAT_W + CAB_H_GAP_SEC + CAB_SEC_W + CAB_H_GAP_SUB + CAB_SUB_W;
  return { nodes, cabWidth, cabHeight: totalH };
}

function renderCABeforeNode(n) {
  const off = getNodeOffset(n.id);
  const el = document.createElement('div');
  el.style.cssText = `position:absolute;left:${n.x + off.dx}px;top:${n.y + off.dy}px;width:${n.w}px;height:${n.h}px;`;

  if (n.type === 'cab-root') {
    el.style.cssText += `background:#3d3832;border-radius:7px;display:flex;align-items:center;
      justify-content:center;cursor:default;`;
    el.innerHTML = `<span style="font-size:10px;font-weight:600;color:#f0ede8;text-align:center;line-height:1.3;padding:0 6px">◌ Antes</span>`;

  } else if (n.type === 'cab-cat') {
    el.style.cssText += `background:#b8b2aa;border:1.5px solid #8a847c;border-radius:6px;
      display:flex;align-items:center;padding:0 8px;cursor:pointer;user-select:none;`;
    el.setAttribute('data-node','cab-cat');
    el.setAttribute('data-draggable','1');
    el.setAttribute('data-node-id', n.id);
    el.innerHTML = `<span style="font-size:10px;font-weight:600;color:#2c2a26;line-height:1.2;flex:1">${truncate(n.label,26)}</span>
      <span style="font-size:8px;color:#6b6560;margin-left:4px">${n.collapsed?'▶':'▼'}</span>`;
    el.addEventListener('click', (e) => {
      if (e.target.closest('[data-draggable]') && Math.abs(e.movementX||0) + Math.abs(e.movementY||0) > 2) return;
      collapsedBeforeCa.has(n.id) ? collapsedBeforeCa.delete(n.id) : collapsedBeforeCa.add(n.id);
      render(); fitView(false);
    });
    el.addEventListener('contextmenu', (e) => { e.preventDefault(); setCommentNode(n.id, n.label, 'cab-cat'); document.getElementById('comments-panel').classList.add('open'); renderComments(); });

  } else if (n.type === 'cab-sec') {
    el.style.cssText += `background:#c8c3bb;border:1.3px solid #8a847c;border-radius:5px;
      display:flex;align-items:center;padding:0 7px;cursor:pointer;user-select:none;`;
    el.setAttribute('data-node','cab-sec');
    el.setAttribute('data-draggable','1');
    el.setAttribute('data-node-id', n.id);
    // Comment dot
    const cmts = loadComments().filter(c => c.nodeId === n.id);
    const dotHtml = cmts.length ? `<span style="width:7px;height:7px;border-radius:50%;background:#f5c842;margin-left:4px;flex-shrink:0;display:inline-block" title="${cmts.length} comentário(s)"></span>` : '';
    el.innerHTML = `<span style="font-size:9.5px;font-weight:500;color:#2e2b27;flex:1;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;">${n.label}</span>
      ${dotHtml}<span style="font-size:7px;color:#7a756e;margin-left:3px">${n.collapsed?'▶':'▼'}</span>`;
    el.addEventListener('click', (e) => {
      if (e.target.closest('[data-draggable]') && Math.abs(e.movementX||0) + Math.abs(e.movementY||0) > 2) return;
      collapsedBeforeCa.has(n.id) ? collapsedBeforeCa.delete(n.id) : collapsedBeforeCa.add(n.id);
      render(); fitView(false);
    });
    el.addEventListener('contextmenu', (e) => { e.preventDefault(); setCommentNode(n.id, n.label, 'cab-sec'); document.getElementById('comments-panel').classList.add('open'); renderComments(); });

  } else if (n.type === 'cab-sub') {
    el.style.cssText += `background:#d8d4cc;border:1px solid #9a958e;border-radius:4px;
      display:flex;align-items:center;padding:0 6px;cursor:grab;`;
    el.setAttribute('data-draggable','1');
    el.setAttribute('data-node-id', n.id);
    // Comment dot
    const cmts = loadComments().filter(c => c.nodeId === n.id);
    const dotHtml = cmts.length ? `<span style="width:6px;height:6px;border-radius:50%;background:#f5c842;margin-left:3px;flex-shrink:0;display:inline-block" title="${cmts.length} comentário(s)"></span>` : '';
    el.innerHTML = `<span style="font-size:9px;color:#3a3630;flex:1;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;">${n.label}</span>${dotHtml}`;
    el.addEventListener('contextmenu', (e) => { e.preventDefault(); setCommentNode(n.id, n.label, 'cab-sub'); document.getElementById('comments-panel').classList.add('open'); renderComments(); });
  }

  caBeforeContainer.appendChild(el);
}

function toggleAntesCa(btn) { /* merged into switchTab */ }

// ═══════════════════════════════════════════════════════
// BEFORE/AFTER MODE
// ═══════════════════════════════════════════════════════
let viewMode = 'after'; // 'after' | 'before'

function setViewMode(mode, btn) {
  viewMode = mode;
  document.querySelectorAll('#topbar .tb-btn').forEach(b => {
    if (b.id === 'btn-after' || b.id === 'btn-before' || b.id === 'btn-split') b.classList.remove('active');
  });
  btn.classList.add('active');
  const banner = document.getElementById('before-banner');
  const splitPanel = document.getElementById('split-panel');
  const stage = document.getElementById('stage');

  if (mode === 'split') {
    banner.style.display = 'none';
    splitPanel.style.display = 'block';
    stage.style.display = 'none';
    document.getElementById('app').style.top = '44px';
    buildSplitView();
    return;
  }

  splitPanel.style.display = 'none';
  stage.style.display = '';
  banner.style.display = mode === 'before' ? 'block' : 'none';
  document.getElementById('app').style.top = mode === 'before' ? '71px' : '44px';

  if (mode === 'before') {
    collapsedMods.add('m5');
    collapsedMods.add('m14');
    collapsedCats.add('p2');
  } else {
    collapsedMods.delete('m5');
    collapsedMods.delete('m14');
    collapsedCats.delete('p2');
  }
  render();
  fitView(false);
}

function buildSplitView() {
  // Build minimal self-contained HTML for each half using current page's data
  // We use a simplified canvas approach embedded in each iframe
  const beforeSrc = buildHalfHTML('before');
  const afterSrc = buildHalfHTML('after');
  const bf = document.getElementById('split-before-frame');
  const af = document.getElementById('split-after-frame');
  bf.srcdoc = beforeSrc;
  af.srcdoc = afterSrc;
}

function buildHalfHTML(mode) {
  const isBefore = mode === 'before';
  const accent = isBefore ? '#6b5a3a' : '#1a5a7a';
  const label = isBefore ? 'ANTES' : 'AGORA';

  // Build category list
  let html = `<!DOCTYPE html><html><head><meta charset="UTF-8">
  <style>
    *{box-sizing:border-box;margin:0;padding:0;font-family:system-ui,sans-serif}
    body{background:#f4f2ee;padding:12px;overflow:auto}
    h2{font-size:11px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;color:${accent};margin-bottom:10px;border-bottom:2px solid ${accent};padding-bottom:5px}
    .block{margin-bottom:8px}
    .cat{border-radius:5px;padding:5px 9px;font-size:11px;font-weight:600;color:#fff;margin-bottom:3px}
    .sec{background:#fff;border-radius:4px;padding:4px 8px;font-size:10px;color:#444;margin:2px 0 2px 12px;border-left:3px solid #ddd;line-height:1.4}
    .sec.new{border-left-color:#f5c842;background:#fffbe6}
    .sec.ghost{opacity:.35;background:#eee;color:#aaa}
    .badge{display:inline-block;font-size:8px;background:#f0ede7;border-radius:2px;padding:1px 4px;margin-left:4px;color:#888;font-family:monospace}
    .map-sep{margin:14px 0 8px;font-size:10px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:#27500A;border-bottom:2px solid #27500A;padding-bottom:5px}
    .mod{border-radius:4px;padding:4px 8px;font-size:10px;font-weight:600;color:#fff;margin-bottom:2px}
    .les{background:#fff;border-radius:3px;padding:3px 7px;font-size:9.5px;color:#555;margin:2px 0 2px 10px;border-left:2px solid #ddd}
    .les.new-les{border-left-color:#f5c842;background:#fffbe6}
    .les.ghost{opacity:.3;background:#eee}
    .new-badge{display:inline-block;font-size:7px;background:#f5c842;color:#1e1c18;border-radius:2px;padding:1px 4px;margin-right:3px;vertical-align:middle}
    .concept-badge{display:inline-block;font-size:7px;background:#e8e4da;color:#555;border-radius:2px;padding:1px 4px;margin-right:3px;vertical-align:middle}
    .mod-ghost{opacity:.4;background:#ddd!important;color:#aaa!important}
    .ghost-label{font-size:8px;color:#bbb;margin-left:5px;font-style:italic}
    h3{font-size:9px;font-weight:700;text-transform:uppercase;letter-spacing:.06em;color:#bbb;margin:4px 0 2px}
  </style></head><body>`;

  html += `<h2>${label === 'ANTES' ? '◌' : '✦'} ${label} — Central de Ajuda</h2>`;

  for (const cat of CATS) {
    // In before mode, hide p2 (Implantar na minha empresa)
    const ghost = isBefore && cat.id === 'p2';
    html += `<div class="block">`;
    html += `<div class="cat${ghost?' mod-ghost':''}" style="background:${ghost?'#ccc':cat.color}">${ghost?'[Não existia] ':''}${cat.name}</div>`;
    if (!ghost) {
      for (const sec of cat.sections) {
        const isNew = !!RENAMED[sec.name];
        html += `<div class="sec${isNew?' new':''}">${isNew?'★ ':''}${sec.name}<span class="badge">${sec.arts}</span></div>`;
      }
    }
    html += `</div>`;
  }

  html += `<div class="map-sep">${label === 'ANTES' ? '◌' : '✦'} ${label} — Treinamentos</div>`;

  for (const mod of MODS) {
    const sumInfo = MOD_SUMMARIES[mod.id] || {};
    const isNewMod = !!sumInfo.isNew;
    const ghost = isBefore && isNewMod;
    html += `<div class="block">`;
    html += `<div class="mod${ghost?' mod-ghost':''}" style="background:${ghost?'#ccc':mod.col}">`;
    if (!ghost) html += isNewMod ? `<span class="new-badge">⭐</span>` : '';
    html += `${ghost?'[Não existia] ':''}${mod.num} · ${mod.name}`;
    if (isBefore && !ghost && sumInfo.before !== undefined) html += ` <span style="font-size:8px;opacity:.7">(${sumInfo.before} aulas)</span>`;
    html += `</div>`;
    if (!ghost) {
      for (const les of mod.lessons) {
        const showNew = !isBefore && les.nw;
        html += `<div class="les${showNew?' new-les':''}">`;
        if (showNew) html += `<span class="new-badge">★</span>`;
        if (les.c) html += `<span class="concept-badge">🟣</span>`;
        if (les.d) html += `<span class="concept-badge">🔥</span>`;
        html += `${les.n}</div>`;
      }
    }
    html += `</div>`;
  }

  html += `</body></html>`;
  return html;
}

function toggleValidacao() {
  const panel = document.getElementById('validacao-panel');
  const isOpen = panel.style.display !== 'none';
  panel.style.display = isOpen ? 'none' : 'block';
}

function renderValidacoes() {
  const list = document.getElementById('val-list');
  if (!list) return;
  const icons = {ok:'✅', partial:'⚠️', no:'❌'};
  const labels = {ok:'Aplicado', partial:'Parcialmente aplicado', no:'Não aplicado'};
  const colors = {ok:'#15803d', partial:'#b45309', no:'#dc2626'};
  const bgs = {ok:'#f0fdf4', partial:'#fefce8', no:'#fef2f2'};
  const borders = {ok:'#86efac', partial:'#fde68a', no:'#fecaca'};
  list.innerHTML = VALIDACOES.map(v => `
    <div style="background:${bgs[v.s]};border:1px solid ${borders[v.s]};border-radius:8px;padding:10px 12px">
      <div style="display:flex;align-items:flex-start;gap:8px">
        <span style="font-size:16px;flex-shrink:0">${icons[v.s]}</span>
        <div style="flex:1">
          <div style="font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.06em;color:${colors[v.s]};margin-bottom:2px">${labels[v.s]}</div>
          <div style="font-size:12px;font-weight:600;color:#1a1815;margin-bottom:4px;line-height:1.4">${v.t}</div>
          <div style="font-size:11px;color:#555;line-height:1.5">${v.d}</div>
          ${v.note ? `<div style="margin-top:6px;padding:5px 8px;background:rgba(0,0,0,0.04);border-left:2px solid ${colors[v.s]};border-radius:0 4px 4px 0;font-size:10.5px;color:#444;line-height:1.5"><strong>→ Próximo passo:</strong> ${v.note}</div>` : ''}
        </div>
      </div>
    </div>
  `).join('');
}

// ═══════════════════════════════════════════════════════
// LAYOUT CONSTANTS
// ═══════════════════════════════════════════════════════
const ROOT_W=120, ROOT_H=38;
const CAT_W=182, CAT_H=32;
const SEC_W=230, SEC_H=28;
const MOD_W=200, MOD_H=32;
const LES_W=210, LES_H=24;
const H_GAP_CA=50, H_GAP_SEC=44;
const H_GAP_TR=50, H_GAP_LES=44;
const V_GAP_SEC=5, V_GAP_CAT=16;
const V_GAP_LES=5, V_GAP_MOD=14;
const MAP_GAP = 280; // horizontal gap between the two maps

let collapsedCats = new Set();
let collapsedMods = new Set();
let selectedNode = null;
let showConnections = true;

// ═══════════════════════════════════════════════════════
// LAYOUT COMPUTATION
// ═══════════════════════════════════════════════════════
function secHeight(sec) {
  // Estimate height based on label length (approx 28 chars per line at SEC_W)
  const lines = Math.ceil(sec.name.length / 30);
  return Math.max(SEC_H, lines * 16 + 8);
}
function catHeight(cat) {
  if (collapsedCats.has(cat.id)) return CAT_H;
  const h = cat.sections.reduce((s,sec) => s + secHeight(sec) + V_GAP_SEC, 0) - V_GAP_SEC;
  return Math.max(CAT_H, h);
}

function modHeight(mod) {
  if (collapsedMods.has(mod.id)) return MOD_H;
  const h = mod.lessons.length * (LES_H + V_GAP_LES) - V_GAP_LES;
  return Math.max(MOD_H, h);
}

function computeCA(offsetX=20) {
  // Returns { nodes, caWidth, caHeight, rootCY }
  const totalH = CATS.reduce((s,c) => s + catHeight(c) + V_GAP_CAT, 0) - V_GAP_CAT;
  const rootCY = totalH / 2;

  const nodes = [];
  const rootX = offsetX, rootY = rootCY - ROOT_H/2;
  nodes.push({type:'root-ca', x:rootX, y:rootY, w:ROOT_W, h:ROOT_H, label:'Central\nde Ajuda', id:'root-ca'});

  let curY = 0;
  for (const cat of CATS) {
    const cH = catHeight(cat);
    const catX = rootX + ROOT_W + H_GAP_CA;
    const catCY = curY + cH/2;
    const catY = catCY - CAT_H/2;
    nodes.push({
      type:'cat', x:catX, y:catY, w:CAT_W, h:CAT_H,
      label:cat.name, id:cat.id, color:cat.color,
      collapsed:collapsedCats.has(cat.id),
      cx: catX + CAT_W/2, cy: catCY
    });
    if (!collapsedCats.has(cat.id)) {
      const secX = catX + CAT_W + H_GAP_SEC;
      let secY = curY;
      for (let si=0; si<cat.sections.length; si++) {
        const sec = cat.sections[si];
          const sH = secHeight(sec);
        const secCY = secY + sH/2;
        const isRenamed = !!RENAMED[sec.name];
        const connMods = CONNECTIONS[sec.name] || [];
        nodes.push({
          type:'sec', x:secX, y:secY, w:SEC_W, h:sH,
          label:sec.name, arts:sec.arts, articles:sec.articles,
          catId:cat.id, color:cat.color,
          isRenamed, oldName:RENAMED[sec.name]||null,
          connMods,
          id:`${cat.id}_s${si}`,
          cx: secX + SEC_W, cy: secCY  // right edge center for connections
        });
        secY += sH + V_GAP_SEC;
      }
    }
    curY += cH + V_GAP_CAT;
  }
  const caWidth = ROOT_W + H_GAP_CA + CAT_W + H_GAP_SEC + SEC_W;
  return { nodes, caWidth, caHeight: totalH, rootCY };
}

function computeTR(offsetX) {
  const totalH = MODS.reduce((s,m) => s + modHeight(m) + V_GAP_MOD, 0) - V_GAP_MOD;
  const rootCY = totalH / 2;
  const nodes = [];
  const rootX = offsetX;
  nodes.push({type:'root-tr', x:rootX, y:rootCY - ROOT_H/2, w:ROOT_W, h:ROOT_H, label:'Portal do\nCurso', id:'root-tr'});

  let curY = 0;
  for (const mod of MODS) {
    const mH = modHeight(mod);
    const modX = rootX + ROOT_W + H_GAP_TR;
    const modCY = curY + mH/2;
    const modY = modCY - MOD_H/2;
    nodes.push({
      type:'mod', x:modX, y:modY, w:MOD_W, h:MOD_H,
      label:(mod.nw?'★ ':'')+mod.num+' · '+mod.name,
      id:mod.id, col:mod.col, bg:mod.bg,
      collapsed:collapsedMods.has(mod.id),
      cx:modX, cy:modCY  // left edge center for connections
    });
    if (!collapsedMods.has(mod.id)) {
      const lesX = modX + MOD_W + H_GAP_LES;
      let lesY = curY;
      for (let li=0; li<mod.lessons.length; li++) {
        const les = mod.lessons[li];
        const lesCY = lesY + LES_H/2;
        nodes.push({
          type:'les', x:lesX, y:lesY, w:LES_W, h:LES_H,
          label:les.n, modId:mod.id, col:mod.col, bg:mod.bg,
          isConceit:!!les.c, isNew:!!les.nw,
          id:`${mod.id}_l${li}`,
          cx:lesX, cy:lesCY
        });
        lesY += LES_H + V_GAP_LES;
      }
    }
    curY += mH + V_GAP_MOD;
  }
  const trWidth = ROOT_W + H_GAP_TR + MOD_W + H_GAP_LES + LES_W;
  return { nodes, trWidth, trHeight: totalH, rootCY };
}

// ═══════════════════════════════════════════════════════
// RENDER
// ═══════════════════════════════════════════════════════
const ns = 'http://www.w3.org/2000/svg';
const stage = document.getElementById('stage');
const mainSvg = document.getElementById('main-svg');
const caContainer = document.getElementById('ca-container');
const trContainer = document.getElementById('tr-container');
const beforeContainer = document.getElementById('before-container');
const caBeforeContainer = document.getElementById('ca-before-container');

function svgN(tag, attrs) {
  const el = document.createElementNS(ns, tag);
  for (const [k,v] of Object.entries(attrs)) el.setAttribute(k,v);
  return el;
}

function mkDiv(cls, style='') {
  const d = document.createElement('div');
  if (cls) d.className = cls;
  if (style) d.style.cssText = style;
  return d;
}

function truncate(text, maxLen) {
  return text.length > maxLen ? text.slice(0, maxLen)+'…' : text;
}

// Connection colors per module
function connColor(modId) {
  const mod = MODS.find(m=>m.id===modId);
  return mod ? mod.col : '#3b82f6';
}

let caNodes=[], trNodes=[];

function render() {
  // Clear containers
  while (caContainer.firstChild) caContainer.removeChild(caContainer.firstChild);
  while (trContainer.firstChild) trContainer.removeChild(trContainer.firstChild);
  while (mainSvg.firstChild) mainSvg.removeChild(mainSvg.firstChild);
  while (beforeContainer.firstChild) beforeContainer.removeChild(beforeContainer.firstChild);
  while (caBeforeContainer.firstChild) caBeforeContainer.removeChild(caBeforeContainer.firstChild);
  _labelSpecs = [];

  // ── Compute ANTES (if shown) ──
  let beforeData = null;
  let trOffsetX = 20;
  if (showAntes) {
    beforeData = computeBefore(20);
    beforeNodes = beforeData.nodes;
    trOffsetX = 20 + beforeData.bWidth + 260; // gap between ANTES and AGORA TR
  } else {
    beforeNodes = [];
    trOffsetX = 20;
  }

  const trData = computeTR(trOffsetX);
  const totalW = trOffsetX + trData.trWidth + MAP_GAP;
  const caData = computeCA(totalW);

  caNodes = caData.nodes;
  trNodes = trData.nodes;

  // ── CA ANTES (far right) — shown when Antes OR Antes CA is active ──
  const showCaAntes = showAntes || showAntesCa;
  let caBeforeData = null;
  const caBeforeOffsetX = totalW + caData.caWidth + (showCaAntes ? MAP_GAP : 0);
  if (showCaAntes) {
    caBeforeData = computeCABefore(caBeforeOffsetX);
    caBeforeNodes = caBeforeData.nodes;
  } else {
    caBeforeNodes = [];
  }

  const totalH = Math.max(
    caData.caHeight,
    trData.trHeight,
    beforeData ? beforeData.bHeight : 0,
    caBeforeData ? caBeforeData.cabHeight : 0
  ) + 40;

  const svgW = caBeforeOffsetX + (caBeforeData ? caBeforeData.cabWidth : caData.caWidth) + 60;
  mainSvg.setAttribute('width', svgW);
  mainSvg.setAttribute('height', totalH);
  stage.style.width  = svgW + 'px';
  stage.style.height = totalH + 'px';

  // ── MAP LABELS ──
  if (showAntes) addLabel('◌  ANTES', 20, -28, '#3d3a36');
  addLabel('TREINAMENTOS (ONBOARDING)', trOffsetX + 20, -28, '#1a3a08');
  addLabel('CENTRAL DE AJUDA', totalW + 20, -28, '#0d3550');
  if (showCaAntes) addLabel('◌  ANTES', caBeforeOffsetX + 20, -28, '#3d3a36');

  // ── ANTES EDGES (root→mod, mod→les) — gray ──
  const edgeSvg = svgN('g', {});
  if (showAntes && beforeData) {
    for (const n of beforeNodes) {
      if (n.type === 'mod-before') {
        const root = beforeNodes.find(x=>x.type==='root-before');
        drawEdge(edgeSvg, root.x+ROOT_W, root.y+ROOT_H/2, n.x, n.cy, '#ccc', 1.8, 0.6);
      }
      if (n.type === 'les-before') {
        const mod = beforeNodes.find(x=>x.id===n.modId);
        if (mod) drawEdge(edgeSvg, mod.cx, mod.cy, n.x, n.cy, '#ccc', 1.2, 0.4);
      }
    }
  }

  // ── CA EDGES (root→cat, cat→sec) ──
  for (const n of caNodes) {
    if (n.type === 'cat') {
      const root = caNodes.find(x=>x.type==='root-ca');
      drawEdge(edgeSvg, root.x+ROOT_W, root.y+ROOT_H/2, n.x, n.cy, n.color, 2, 0.55);
    }
    if (n.type === 'sec') {
      const cat = caNodes.find(x=>x.id===n.catId);
      if (cat) drawEdge(edgeSvg, cat.x+CAT_W, cat.cy, n.x, n.cy, n.color, 1.2, 0.38);
    }
  }
  mainSvg.appendChild(edgeSvg);

  // ── TR EDGES (root→mod, mod→les) ──
  for (const n of trNodes) {
    if (n.type === 'mod') {
      const root = trNodes.find(x=>x.type==='root-tr');
      drawEdge(edgeSvg, root.x+ROOT_W, root.y+ROOT_H/2, n.x, n.cy, n.col, 2, 0.5);
    }
    if (n.type === 'les') {
      const mod = trNodes.find(x=>x.id===n.modId);
      if (mod) drawEdge(edgeSvg, mod.x+MOD_W, mod.cy, n.x, n.cy, n.col, 1.2, 0.35);
    }
  }

  // ── REUSE LINES (ANTES → AGORA TR, dashed gray, always visible even collapsed) ──
  if (showAntes && beforeData) {
    const reuseG = svgN('g', {id:'reuse-layer'});

    // Build lookup maps from computed nodes
    const beforeLesMap = {}, beforeModMap = {}, afterModMap = {}, afterLesMap = {};
    for (const n of beforeNodes) {
      if (n.type==='les-before') beforeLesMap[n.id] = n;
      if (n.type==='mod-before') beforeModMap[n.id] = n;
    }
    for (const n of trNodes) {
      if (n.type==='mod') afterModMap[n.id] = n;
      if (n.type==='les') afterLesMap[n.label] = n;
    }

    // Walk REUSE_MAP directly — covers ALL pairs regardless of collapse state
    const drawnModPairs = new Set();
    for (const [afterLabel, beforeId] of Object.entries(REUSE_MAP)) {
      // Find the before lesson (may not exist as node if before mod is collapsed)
      const beforeLes = beforeLesMap[beforeId];
      // Find the before mod via the MODS_BEFORE data (always available)
      const bMod = MODS_BEFORE.find(m => m.lessons.some(l => l.id === beforeId));
      if (!bMod) continue;
      const beforeModNode = beforeModMap[bMod.id];
      if (!beforeModNode) continue;

      // Find the after lesson (may not exist if after mod is collapsed)
      const afterLes = afterLesMap[afterLabel];
      // Find the after mod via MODS data
      const aMod = MODS.find(m => m.lessons.some(l => l.n.toLowerCase() === afterLabel.toLowerCase()));
      const afterModNode = aMod ? afterModMap[aMod.id] : null;
      if (!afterModNode) continue;

      const isBefExpanded = !collapsedBefore.has(bMod.id);
      const isAftExpanded = !collapsedMods.has(aMod.id);

      if (isBefExpanded && isAftExpanded && beforeLes && afterLes) {
        // Both expanded: les → les (precise)
        const x1 = beforeLes.rightX, y1 = beforeLes.cy;
        const x2 = afterLes.x,       y2 = afterLes.cy;
        const mx = (x1+x2)/2;
        reuseG.appendChild(svgN('path',{
          d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
          fill:'none', stroke:'#999', 'stroke-width':'1.2',
          'stroke-dasharray':'5,3', opacity:'0.55'
        }));
        reuseG.appendChild(svgN('polygon',{
          points:`${x2},${y2} ${x2-6},${y2-2.5} ${x2-6},${y2+2.5}`,
          fill:'#999', opacity:'0.65'
        }));
      } else {
        // At least one side collapsed: draw mod → mod (deduplicated)
        const pairKey = bMod.id + '↔' + aMod.id;
        if (drawnModPairs.has(pairKey)) continue;
        drawnModPairs.add(pairKey);
        // Source: rightmost point of before mod
        const x1 = isBefExpanded && beforeLes
          ? beforeLes.rightX
          : beforeModNode.x + MOD_W;
        const y1 = isBefExpanded && beforeLes ? beforeLes.cy : beforeModNode.cy;
        // Target: leftmost point of after mod
        const x2 = isAftExpanded && afterLes ? afterLes.x : afterModNode.x;
        const y2 = isAftExpanded && afterLes ? afterLes.cy : afterModNode.cy;
        const mx = (x1+x2)/2;
        reuseG.appendChild(svgN('path',{
          d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
          fill:'none', stroke:'#999', 'stroke-width':'1.5',
          'stroke-dasharray':'6,3', opacity:'0.5'
        }));
        reuseG.appendChild(svgN('polygon',{
          points:`${x2},${y2} ${x2-7},${y2-3} ${x2-7},${y2+3}`,
          fill:'#999', opacity:'0.6'
        }));
      }
    }
    mainSvg.appendChild(reuseG);
  }

  // ── CONNECTION ARROWS (TR mod → CA sec, always visible even when collapsed) ──
  if (showConnections) {
    const connG = svgN('g', {id:'conn-layer'});

    // Build lookups for efficient access
    const caSecMap  = {};  // secName → secNode (only visible/expanded secs)
    const caCatMap  = {};  // catId → catNode
    const caCatBySecName = {}; // secName → catNode (for collapsed fallback)
    for (const n of caNodes) {
      if (n.type === 'cat') caCatMap[n.id] = n;
      if (n.type === 'sec') caSecMap[n.label] = n;
    }
    // Map section names to their parent cat (from CATS data)
    for (const cat of CATS) {
      for (const sec of cat.sections) {
        caCatBySecName[sec.name] = cat.id;
      }
    }

    const trModMap = {};  // modId → modNode (always present)
    for (const n of trNodes) {
      if (n.type === 'mod') trModMap[n.id] = n;
    }

    // Collect all connections: {x1,y1,x2,y2,col} — deduplicated mod→cat fallbacks
    const drawnKeys = new Set();

    // Walk CONNECTIONS to find all sec→mod pairs
    for (const [secName, modIds] of Object.entries(CONNECTIONS)) {
      for (const modId of modIds) {
        const modNode = trModMap[modId];
        if (!modNode) continue;
        const col = connColor(modId);

        // Try to find visible sec node
        const secNode = caSecMap[secName];
        if (secNode) {
          // Sec is visible — draw mod → sec
          const x1 = modNode.x + MOD_W, y1 = modNode.cy;
          const x2 = secNode.x,          y2 = secNode.cy;
          const mx = (x1+x2)/2;
          connG.appendChild(svgN('path',{
            d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
            fill:'none', stroke:col, 'stroke-width':'1.4',
            'stroke-dasharray':'5,3', opacity:'0.55'
          }));
          connG.appendChild(svgN('polygon',{
            points:`${x2},${y2} ${x2-7},${y2-3} ${x2-7},${y2+3}`,
            fill:col, opacity:'0.7'
          }));
        } else {
          // Sec collapsed — fallback: draw mod → parent cat node
          const catId = caCatBySecName[secName];
          const catNode = catId ? caCatMap[catId] : null;
          if (!catNode) continue;
          const key = modId + '→' + catNode.id;
          if (drawnKeys.has(key)) continue;
          drawnKeys.add(key);
          const x1 = modNode.x + MOD_W, y1 = modNode.cy;
          const x2 = catNode.x,          y2 = catNode.cy;
          const mx = (x1+x2)/2;
          connG.appendChild(svgN('path',{
            d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
            fill:'none', stroke:col, 'stroke-width':'1.6',
            'stroke-dasharray':'5,3', opacity:'0.45'
          }));
          connG.appendChild(svgN('polygon',{
            points:`${x2},${y2} ${x2-7},${y2-3} ${x2-7},${y2+3}`,
            fill:col, opacity:'0.6'
          }));
        }
      }
    }
    mainSvg.appendChild(connG);
  }

  // ── RENDER BEFORE NODES ──
  if (showAntes) for (const n of beforeNodes) renderBeforeNode(n);

  // ── RENDER CA NODES ──
  for (const n of caNodes) renderCANode(n);

  // ── RENDER TR NODES ──
  for (const n of trNodes) renderTRNode(n);

  // ── RENDER CA ANTES NODES & EDGES ──
  if (showCaAntes && caBeforeData) {
    // Edges: root→cat, cat→sec, sec→sub
    const cabEdgeSvg = svgN('g', {});
    const rootNode = caBeforeNodes.find(n => n.type === 'cab-root');
    for (const n of caBeforeNodes) {
      if (n.type === 'cab-cat') {
        if (rootNode) drawEdge(cabEdgeSvg, rootNode.x+CAB_ROOT_W, rootNode.y+ROOT_H/2, n.leftX, n.cy, '#aaa', 1.4, 0.5);
        if (!n.collapsed) {
          const secs = caBeforeNodes.filter(s => s.type==='cab-sec' && s.catId===n.id);
          for (const s of secs) drawEdge(cabEdgeSvg, n.cx, n.cy, s.leftX, s.cy, '#bbb', 1.1, 0.4);
        }
      }
      if (n.type === 'cab-sec' && !n.collapsed) {
        const subs = caBeforeNodes.filter(s => s.type==='cab-sub' && s.secId===n.id);
        for (const sub of subs) drawEdge(cabEdgeSvg, n.cx, n.cy, sub.leftX, sub.cy, '#ccc', 0.9, 0.35);
      }
    }

    // ── CONNECTION LINES: current CA cats → CA_BEFORE (always visible, Antes tab only) ──
    if (showAntes && rootNode) {
      const caConnG = svgN('g', {id:'ca-antes-conn-layer'});
      const caRootNode = caNodes.find(n => n.type === 'root-ca');
      if (caRootNode) {
        const x1 = caRootNode.x + ROOT_W, y1 = caRootNode.y + ROOT_H/2;
        const x2 = rootNode.x, y2 = rootNode.y + ROOT_H/2;
        const mx = (x1 + x2) / 2;
        caConnG.appendChild(svgN('path',{
          d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
          fill:'none', stroke:'#aaa', 'stroke-width':'1.8',
          'stroke-dasharray':'6,4', opacity:'0.6'
        }));
        caConnG.appendChild(svgN('polygon',{
          points:`${x2},${y2} ${x2+7},${y2-3} ${x2+7},${y2+3}`,
          fill:'#aaa', opacity:'0.7'
        }));
      }

      // Build fast lookups — sec nodes may not exist if cab-cat is collapsed
      const cabSecByName = {};    // secLabel→node (when sec expanded)
      const cabCatByLabel = {};   // catLabel→catNode
      const cabCatBySecLabel = {}; // secLabel→parent catNode (from CA_BEFORE data)
      for (const cn of caBeforeNodes) {
        if (cn.type === 'cab-sec') cabSecByName[cn.label.trim().toLowerCase()] = cn;
        if (cn.type === 'cab-cat') cabCatByLabel[cn.label.trim().toLowerCase()] = cn;
      }
      // Map sec names → parent cat (from static CA_BEFORE data)
      for (const cabCat of CA_BEFORE) {
        for (const cabSec of (cabCat.secs||[])) {
          cabCatBySecLabel[cabSec.label.trim().toLowerCase()] = cabCat.id;
        }
      }

      const drawnCaKeys = new Set();
      for (const caNode of caNodes) {
        if (caNode.type !== 'cat') continue;
        const nameLower = caNode.label.trim().toLowerCase();

        // 1. Try: current CA cat name matches a CA_BEFORE sec name (direct sec match)
        const matchSec = cabSecByName[nameLower];
        if (matchSec) {
          const x1 = caNode.x + CAT_W, y1 = caNode.cy;
          const x2 = matchSec.leftX, y2 = matchSec.cy;
          const mx = (x1 + x2) / 2;
          caConnG.appendChild(svgN('path',{
            d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
            fill:'none', stroke:'#aaa', 'stroke-width':'1.5',
            'stroke-dasharray':'5,3', opacity:'0.65'
          }));
          caConnG.appendChild(svgN('polygon',{
            points:`${x2},${y2} ${x2+6},${y2-2.5} ${x2+6},${y2+2.5}`,
            fill:'#aaa', opacity:'0.7'
          }));
          continue;
        }

        // 2. Try: CA cat name matches a CA_BEFORE sec name (sec collapsed → show to parent cat)
        const befCatId = cabCatBySecLabel[nameLower];
        if (befCatId) {
          const befCatNode = caBeforeNodes.find(n => n.id === befCatId);
          if (befCatNode) {
            const key = caNode.id + '→' + befCatId;
            if (!drawnCaKeys.has(key)) {
              drawnCaKeys.add(key);
              const x1 = caNode.x + CAT_W, y1 = caNode.cy;
              const x2 = befCatNode.leftX, y2 = befCatNode.cy;
              const mx = (x1 + x2) / 2;
              caConnG.appendChild(svgN('path',{
                d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
                fill:'none', stroke:'#bbb', 'stroke-width':'1.3',
                'stroke-dasharray':'5,3', opacity:'0.5'
              }));
              caConnG.appendChild(svgN('polygon',{
                points:`${x2},${y2} ${x2+6},${y2-2.5} ${x2+6},${y2+2.5}`,
                fill:'#bbb', opacity:'0.6'
              }));
            }
            continue;
          }
        }

        // 3. Fallback: CA cat label matches CA_BEFORE cat label directly (e.g., "Conta Azul Mais")
        const matchCat = cabCatByLabel[nameLower];
        if (matchCat) {
          const x1 = caNode.x + CAT_W, y1 = caNode.cy;
          const x2 = matchCat.leftX, y2 = matchCat.cy;
          const mx = (x1 + x2) / 2;
          caConnG.appendChild(svgN('path',{
            d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
            fill:'none', stroke:'#aaa', 'stroke-width':'1.5',
            'stroke-dasharray':'5,3', opacity:'0.65'
          }));
          caConnG.appendChild(svgN('polygon',{
            points:`${x2},${y2} ${x2+6},${y2-2.5} ${x2+6},${y2+2.5}`,
            fill:'#aaa', opacity:'0.7'
          }));
        }
      }
      mainSvg.appendChild(caConnG);
    }

    mainSvg.appendChild(cabEdgeSvg);
    for (const n of caBeforeNodes) renderCABeforeNode(n);
  }

  updateLegend();
  updateTotal();
  // Defer dot rendering until DOM nodes are placed
  requestAnimationFrame(updateCommentDots);
  requestAnimationFrame(updateMapLabels);
}

// ── MAP LABELS — rendered outside stage in fixed screen coords ──
// Store label specs; redrawn on every transform
let _labelSpecs = []; // [{stageX, text, color}]

function addLabel(text, stageX, _ignored, color) {
  _labelSpecs.push({stageX, text, color});
}

function updateMapLabels() {
  const bar = document.getElementById('map-labels-bar');
  if (!bar) return;
  bar.innerHTML = '';
  const wrap = document.getElementById('canvas-wrap');
  for (const {stageX, text, color} of _labelSpecs) {
    const screenX = stageX * scale + vx;
    if (screenX > wrap.clientWidth - 10) continue;
    const el = document.createElement('div');
    el.style.cssText = `
      position:absolute;left:${Math.max(4, screenX)}px;top:5px;
      color:${color};background:rgba(255,255,255,0.95);
      padding:3px 12px;border-radius:20px;border:1.5px solid ${color}55;
      box-shadow:0 1px 6px rgba(0,0,0,0.10);
      font-size:10px;font-weight:700;letter-spacing:.1em;
      text-transform:uppercase;white-space:nowrap;
      pointer-events:none;font-family:'IBM Plex Sans',sans-serif;
    `;
    el.textContent = text;
    bar.appendChild(el);
  }
}

function drawEdge(parent, x1,y1,x2,y2, color, width, opacity) {
  const mx = (x1+x2)/2;
  const path = svgN('path', {
    d:`M${x1},${y1} C${mx},${y1} ${mx},${y2} ${x2},${y2}`,
    fill:'none', stroke:color, 'stroke-width':width, opacity
  });
  parent.appendChild(path);
}

function renderCANode(n) {
  const el = document.createElement('div');
  el.style.cssText = `position:absolute;left:${n.x}px;top:${n.y}px;width:${n.w}px;height:${n.h}px;`;

  if (n.type === 'root-ca') {
    el.style.cssText += `background:#1e1c18;border-radius:7px;display:flex;align-items:center;justify-content:center;cursor:default;`;
    el.innerHTML = `<span style="font-size:11px;font-weight:600;color:#f4f2ee;text-align:center;line-height:1.3">${n.label.replace('\n','<br>')}</span>`;

  } else if (n.type === 'cat') {
    el.style.cssText += `background:${n.color};border-radius:6px;display:flex;align-items:center;padding:0 8px;cursor:pointer;user-select:none;`;
    el.setAttribute('data-node','cat');
    el.innerHTML = `<span style="font-size:10.5px;font-weight:500;color:#fff;line-height:1.2;flex:1">${truncate(n.label,36)}</span>
      <span style="font-size:8px;color:rgba(255,255,255,0.5);margin-left:4px">${n.collapsed?'▶':'▼'}</span>`;
    el.addEventListener('click', () => {
      collapsedCats.has(n.id) ? collapsedCats.delete(n.id) : collapsedCats.add(n.id);
      render(); fitView(false);
    });
    el.title = n.label; // show full title on hover

  } else if (n.type === 'sec') {
    const isSelected = selectedNode === n.id;
    const hasCon = (n.connMods||[]).length > 0;
    const borderCol = n.isRenamed ? '#f5c842' : (isSelected ? '#3b82f6' : n.color);
    const bw = (n.isRenamed||isSelected) ? 2 : 1.2;
    const bg = isSelected ? '#fffbe6' : '#fff';
    el.style.cssText += `background:${bg};border-radius:5px;border:${bw}px solid ${borderCol};
      display:flex;align-items:center;padding:3px 6px;cursor:grab;min-height:${SEC_H}px;`;
    el.setAttribute('data-node','sec');
    el.setAttribute('data-draggable','1');
    el.setAttribute('data-node-id', n.id);
    if (hasCon) el.style.cssText += `box-shadow:0 0 0 1.5px ${borderCol}22;`;

    const star = n.isRenamed ? `<span style="font-size:9px;background:#f5c842;color:#1e1c18;border-radius:50%;width:14px;height:14px;display:inline-flex;align-items:center;justify-content:center;margin-right:4px;flex-shrink:0">★</span>` : '';
    const connDot = hasCon ? `<span style="width:6px;height:6px;border-radius:50%;background:#3b82f6;opacity:0.7;flex-shrink:0;margin-left:3px" title="${(n.connMods||[]).join(', ')}"></span>` : '';
    const badge = `<span style="font-size:9px;font-family:'IBM Plex Mono',monospace;color:#888;background:#f0ede7;border-radius:3px;padding:1px 5px;flex-shrink:0;margin-left:4px">${n.arts}</span>`;

    el.innerHTML = `${star}<span style="font-size:10px;color:#333;flex:1;line-height:1.3;white-space:normal;word-break:break-word;pointer-events:none">${n.label}</span>${connDot}${badge}`;
    el.title = n.label + (n.arts ? ` (${n.arts} artigos)` : '');
    // Apply saved offset
    const off = getNodeOffset(n.id);
    if (off.dx || off.dy) { el.style.left=(n.x+off.dx)+'px'; el.style.top=(n.y+off.dy)+'px'; }
    el.addEventListener('click', e => { if(Math.abs(e.movementX||0)<3 && Math.abs(e.movementY||0)<3){ e.stopPropagation(); selectedNode=n.id; openSidebar(n,'ca'); render(); } });
    caContainer.appendChild(el);
    return;
  }
  caContainer.appendChild(el);
}

function renderTRNode(n) {
  const el = document.createElement('div');
  el.style.cssText = `position:absolute;left:${n.x}px;top:${n.y}px;width:${n.w}px;height:${n.h}px;`;

  if (n.type === 'root-tr') {
    el.style.cssText += `background:#1a1916;border-radius:7px;display:flex;align-items:center;justify-content:center;cursor:default;`;
    el.innerHTML = `<span style="font-size:11px;font-weight:600;color:#f4f2ee;text-align:center;line-height:1.3">${n.label.replace('\n','<br>')}</span>`;

  } else if (n.type === 'mod') {
    const sumInfo = MOD_SUMMARIES[n.id] || {};
    const isBeforeMode = viewMode === 'before';
    const isNewMod = sumInfo.isNew;
    const isGrayed = isBeforeMode && isNewMod;
    const bg = isGrayed ? '#f0ede8' : n.bg;
    const col = isGrayed ? '#aaa' : n.col;
    const bord = isGrayed ? '#ccc' : n.col+'55';
    const beforeBadge = isBeforeMode && !isNewMod && sumInfo.before !== undefined && sumInfo.before !== (MODS.find(m=>m.id===n.id)?.lessons?.length||0)
      ? `<span style="font-size:8px;background:#fff;border:1px solid ${n.col}44;border-radius:3px;padding:1px 4px;margin-left:3px;color:${n.col};flex-shrink:0">${sumInfo.before}aulas</span>` : '';
    const newBadge = isNewMod && !isBeforeMode ? `<span style="font-size:8px;background:${n.col};color:#fff;border-radius:3px;padding:1px 4px;flex-shrink:0;margin-right:3px">⭐</span>` : '';
    const ghostLabel = isGrayed ? '<span style="font-size:8px;color:#bbb;margin-left:4px">não existia</span>' : '';
    el.style.cssText += `background:${bg};border:1.5px solid ${bord};border-radius:6px;
      display:flex;align-items:center;padding:0 8px;cursor:pointer;user-select:none;${isGrayed?'opacity:0.45':''}`;
    el.setAttribute('data-node','mod');
    el.innerHTML = `${newBadge}<span style="font-size:10px;font-weight:600;color:${col};line-height:1.2;flex:1">${truncate(n.label,38)}</span>${ghostLabel}${beforeBadge}
      <span style="font-size:8px;color:${col}88;margin-left:4px">${n.collapsed?'▶':'▼'}</span>`;
    el.addEventListener('click', (e) => {
      if (e.detail === 2) {
        // Double-click: open sidebar summary
        openModSidebar(n);
      } else {
        collapsedMods.has(n.id) ? collapsedMods.delete(n.id) : collapsedMods.add(n.id);
        render(); fitView(false);
      }
    });

  } else if (n.type === 'les') {
    const isSelected = selectedNode === n.id;
    const bg = isSelected ? n.bg : '#fff';
    const bord = isSelected ? n.col : n.col+'44';
    el.style.cssText += `background:${bg};border:1.2px solid ${bord};border-radius:5px;
      display:flex;align-items:center;padding:0 7px;cursor:grab;gap:4px;`;
    el.setAttribute('data-node','les');
    el.setAttribute('data-draggable','1');
    el.setAttribute('data-node-id', n.id);
    const newBadge = n.isNew ? `<span style="font-size:8px;background:${n.col};color:#fff;border-radius:3px;padding:1px 4px;flex-shrink:0;pointer-events:none">★</span>` : '';
    const concBadge = n.isConceit ? `<span style="font-size:8px;background:#e8e4da;color:#666;border-radius:3px;padding:1px 4px;flex-shrink:0;pointer-events:none">C</span>` : '';
    el.innerHTML = `${newBadge}${concBadge}<span style="font-size:10px;color:${n.col};flex:1;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;pointer-events:none">${n.label}</span>`;
    // Apply saved offset
    const off = getNodeOffset(n.id);
    if (off.dx || off.dy) { el.style.left=(n.x+off.dx)+'px'; el.style.top=(n.y+off.dy)+'px'; }
    el.addEventListener('click', e => { if(Math.abs(e.movementX||0)<3 && Math.abs(e.movementY||0)<3){ e.stopPropagation(); selectedNode=n.id; openSidebar(n,'tr'); render(); } });
    trContainer.appendChild(el);
    return;
  }
  trContainer.appendChild(el);
}

// ═══════════════════════════════════════════════════════
// LEGEND
// ═══════════════════════════════════════════════════════
function updateLegend() {
  const legCA = document.getElementById('leg-ca');
  legCA.innerHTML = '';
  for (const cat of CATS) {
    const item = document.createElement('div');
    item.className = 'leg-item';
    item.innerHTML = `<div class="leg-dot" style="background:${cat.color}"></div><span>${cat.name}</span>`;
    item.addEventListener('click', () => scrollToNode(cat.id, 'ca'));
    legCA.appendChild(item);
  }
  const legTR = document.getElementById('leg-tr');
  legTR.innerHTML = '';
  for (const mod of MODS) {
    const item = document.createElement('div');
    item.className = 'leg-item';
    item.innerHTML = `<div class="leg-dot" style="background:${mod.col}"></div><span>${mod.num} · ${mod.name}</span>`;
    item.addEventListener('click', () => scrollToNode(mod.id, 'tr'));
    legTR.appendChild(item);
  }
}

function updateTotal() {
  const secs = CATS.reduce((s,c)=>s+c.sections.length,0);
  const mods = MODS.length;
  const les = MODS.reduce((s,m)=>s+m.lessons.length,0);
  document.getElementById('total-label').textContent = `${secs} seções · ${mods} módulos · ${les} aulas`;
}

function scrollToNode(id, map) {
  const nodes = map === 'ca' ? caNodes : trNodes;
  const n = nodes.find(x=>x.id===id);
  if (!n) return;
  const wrap = document.getElementById('canvas-wrap');
  const cw = wrap.clientWidth, ch = wrap.clientHeight;
  vx = cw/2 - (n.x + n.w/2) * scale;
  vy = ch/2 - (n.y + n.h/2) * scale;
  updateTransform();
}

// ═══════════════════════════════════════════════════════
// SIDEBAR
// ═══════════════════════════════════════════════════════

// ═══════════════════════════════════════════════════════
// SIDEBAR FOR MODULE (double-click)
// ═══════════════════════════════════════════════════════
function openModSidebar(n) {
  const sb = document.getElementById('sidebar');
  const tag = document.getElementById('sb-tag');
  const name = document.getElementById('sb-name');
  const count = document.getElementById('sb-count');
  const rb = document.getElementById('sb-renamed-box');
  const lbox = document.getElementById('sb-links');
  const llist = document.getElementById('sb-links-list');
  const body = document.getElementById('sb-body');

  const mod = MODS.find(m=>m.id===n.id);
  const sumInfo = MOD_SUMMARIES[n.id] || {};
  tag.textContent = `${mod.num} · Módulo de Treinamento`;
  name.textContent = mod.name;
  const nowAulas = mod?.lessons?.length || 0;
  count.textContent = `${nowAulas} aulas` + (sumInfo.before !== undefined ? ` (antes: ${sumInfo.before||'não existia'})` : '');
  rb.style.display = 'none';

  // Show summary
  lbox.style.display = 'block';
  document.querySelector('#sb-links .link-title').textContent = '📝 Resumo do módulo';
  llist.innerHTML = '';
  const modNote = document.createElement('div');
  modNote.style.cssText = 'font-size:11.5px;color:#333;line-height:1.7;margin-bottom:8px';
  modNote.textContent = sumInfo.summary || '';
  llist.appendChild(modNote);
  if (sumInfo.isNew) {
    const badge = document.createElement('div');
    badge.style.cssText = 'font-size:10px;background:#fef3c7;border:1px solid #fcd34d;border-radius:5px;padding:5px 8px;color:#92400e;margin-bottom:4px';
    badge.textContent = '⭐ Módulo novo — não existia na estrutura anterior';
    llist.appendChild(badge);
  }

  // List all lessons
  body.innerHTML = '<div style="font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.06em;color:#aaa;margin-bottom:6px">Aulas</div>';
  (mod.lessons||[]).forEach((les, i) => {
    const row = document.createElement('div');
    row.className = 'art-row';
    const icon = les.d ? '🔥' : les.c ? '🟣' : les.nw ? '⭐' : '📖';
    row.innerHTML = `<span class="art-n">${String(i+1).padStart(2,'0')}</span><span>${icon} ${les.n}</span>`;
    body.appendChild(row);
  });
  // Wire comments panel context to this module
  setCommentNode(n.id, mod.num + ' · ' + mod.name, 'tr');
  sb.classList.add('open');
}

function openSidebar(n, map) {
  const sb = document.getElementById('sidebar');
  const tag = document.getElementById('sb-tag');
  const name = document.getElementById('sb-name');
  const count = document.getElementById('sb-count');
  const rb = document.getElementById('sb-renamed-box');
  const lbox = document.getElementById('sb-links');
  const llist = document.getElementById('sb-links-list');
  const body = document.getElementById('sb-body');

  if (map === 'ca') {
    const cat = CATS.find(c=>c.id===n.catId);
    tag.textContent = `Seção · ${cat?.name||''}`;
    name.textContent = n.label;
    count.textContent = `${n.arts} artigos`;

    rb.style.display = n.isRenamed ? 'block' : 'none';
    if (n.isRenamed) document.getElementById('sb-old-name').textContent = 'Era: "'+n.oldName+'"';

    const mods = n.connMods || [];
    lbox.style.display = mods.length ? 'block' : 'none';
    llist.innerHTML = '';
    for (const mid of mods) {
      const mod = MODS.find(m=>m.id===mid);
      if (!mod) continue;
      const row = document.createElement('div');
      row.className = 'link-mod';
      row.innerHTML = `<div class="link-mod-dot" style="background:${mod.col}"></div><span>${mod.num} · ${mod.name}</span>`;
      row.style.cursor = 'pointer';
      row.addEventListener('click', () => scrollToNode(mid, 'tr'));
      llist.appendChild(row);
    }

    body.innerHTML = '';
    (n.articles||[]).forEach((a,i) => {
      const row = document.createElement('div');
      row.className = 'art-row';
      row.innerHTML = `<span class="art-n">${String(i+1).padStart(2,'0')}</span><span>${a}</span>`;
      body.appendChild(row);
    });

    // Wire comments panel context to this node
    setCommentNode(n.id, n.label, 'ca');

  } else {
    // training lesson
    const mod = MODS.find(m=>m.id===n.modId);
    const sumInfo = MOD_SUMMARIES[n.modId] || {};
    tag.textContent = `Aula · ${mod?.num} ${mod?.name||''}`;
    name.textContent = n.label;
    const typeLabel = n.isConceit ? '🟣 Aula de conceito — fundamento do módulo' : n.isNew ? '⭐ Aula nova nesta revisão' : '📖 Aula técnica padrão';
    count.textContent = typeLabel;
    rb.style.display = 'none';
    // Show module summary in lbox
    lbox.style.display = 'block';
    document.querySelector('#sb-links .link-title').textContent = '📝 Sobre o módulo';
    llist.innerHTML = '';
    const modNote = document.createElement('div');
    modNote.style.cssText = 'font-size:11px;color:#333;line-height:1.6;margin-bottom:8px';
    modNote.textContent = sumInfo.summary || '';
    llist.appendChild(modNote);
    if (sumInfo.before !== undefined && sumInfo.before !== mod?.lessons?.length) {
      const diffRow = document.createElement('div');
      diffRow.style.cssText = 'font-size:10px;background:#f0f9ff;border:1px solid #bae6fd;border-radius:5px;padding:5px 8px;color:#0369a1';
      const nowAulas = mod?.lessons?.length || 0;
      diffRow.innerHTML = sumInfo.before === 0
        ? '⭐ <strong>Módulo novo</strong> — não existia na versão anterior'
        : `Antes: <strong>${sumInfo.before} aulas</strong> → Agora: <strong>${nowAulas} aulas</strong> (+${nowAulas - sumInfo.before})`;
      llist.appendChild(diffRow);
    }
    body.innerHTML = `<div style="font-size:11px;color:#888;padding:4px 0 8px">Clique em outro módulo para ver seu resumo.</div>`;

    // Wire comments panel context to this node
    setCommentNode(n.id, n.label, 'tr');
  }

  sb.classList.add('open');
}

document.getElementById('sb-close').addEventListener('click', () => {
  document.getElementById('sidebar').classList.remove('open');
  selectedNode = null;
  setCommentNode(null, null, null);
  render();
});

// ═══════════════════════════════════════════════════════
// NODE DRAG OFFSETS (for repositioning nodes)
// ═══════════════════════════════════════════════════════
const nodeOffsets = {}; // id -> {dx, dy}
let draggingNode = null, nodeDragStart = null;

function getNodeOffset(id) {
  return nodeOffsets[id] || {dx:0, dy:0};
}

// Load shared data then render
(async () => {
  await loadSharedData();
  // Restore localStorage fallback offsets if JSONbin was empty
  if (!Object.keys(nodeOffsets).length) {
    try {
      const saved = JSON.parse(localStorage.getItem('mapa_v6_offsets') || '{}');
      Object.assign(nodeOffsets, saved);
    } catch(e){}
  }
  render();
  fitView();
  updateCommentDots();
  // Show drag hint briefly
  setTimeout(() => {
    const hint = document.getElementById('drag-hint');
    if (hint) { hint.style.opacity='1'; setTimeout(()=>hint.style.opacity='0', 2800); }
  }, 800);
})();

// Node drag via mousedown on nodes
document.getElementById('canvas-wrap').addEventListener('mousedown', e => {
  const nodeEl = e.target.closest('[data-draggable]');
  if (!nodeEl) return;
  e.stopPropagation();
  e.preventDefault();
  draggingNode = nodeEl;
  const nodeId = nodeEl.getAttribute('data-node-id');
  const off = getNodeOffset(nodeId);
  const curLeft = parseInt(nodeEl.style.left) || 0;
  const curTop  = parseInt(nodeEl.style.top)  || 0;
  nodeDragStart = {
    mx: e.clientX, my: e.clientY,
    origLeft: curLeft - off.dx,
    origTop:  curTop  - off.dy,
    id: nodeId
  };
  nodeEl.style.zIndex = '99';
  nodeEl.style.boxShadow = '0 4px 16px rgba(0,0,0,.18)';
  nodeEl.style.cursor = 'grabbing';
}, true);

window.addEventListener('mousemove', e => {
  if (!draggingNode || !nodeDragStart) return;
  const dx = (e.clientX - nodeDragStart.mx) / scale;
  const dy = (e.clientY - nodeDragStart.my) / scale;
  const newLeft = nodeDragStart.origLeft + dx;
  const newTop  = nodeDragStart.origTop  + dy;
  draggingNode.style.left = newLeft + 'px';
  draggingNode.style.top  = newTop  + 'px';
  // Save offset
  nodeOffsets[nodeDragStart.id] = {
    dx: newLeft - nodeDragStart.origLeft,
    dy: newTop  - nodeDragStart.origTop
  };
}, true);

window.addEventListener('mouseup', e => {
  if (!draggingNode) return;
  draggingNode.style.zIndex = '';
  draggingNode.style.boxShadow = '';
  draggingNode.style.cursor = '';
  draggingNode = null;
  nodeDragStart = null;
  // Persist drag offsets to shared storage
  saveSharedData();
}, true);

// Reset node positions button
document.getElementById('btn-reset').addEventListener('click', () => {
  Object.keys(nodeOffsets).forEach(k => delete nodeOffsets[k]);
  collapsedCats.clear(); collapsedMods.clear(); render(); fitView();
});

// ═══════════════════════════════════════════════════════
// PAN / ZOOM
// ═══════════════════════════════════════════════════════
let scale=1, vx=0, vy=0;
function updateTransform() {
  const s = document.getElementById('stage');
  s.style.transform = `translate(${vx}px,${vy}px) scale(${scale})`;
  s.style.transformOrigin = '0 0';
  document.getElementById('zoom-pct').textContent = Math.round(scale*100)+'%';
  requestAnimationFrame(updateMapLabels);
}

function fitView(animate=true) {
  const wrap = document.getElementById('canvas-wrap');
  let trOffX = 20;
  if (showAntes) {
    const bd = computeBefore(20);
    trOffX = 20 + bd.bWidth + 260;
  }
  const trD = computeTR(trOffX);
  const caD = computeCA(trOffX + trD.trWidth + MAP_GAP);
  const caBaseOffX = trOffX + trD.trWidth + MAP_GAP;
  let totalW = caBaseOffX + caD.caWidth + 60;
  if (showAntes || showAntesCa) {
    const cabD = computeCABefore(caBaseOffX + caD.caWidth + MAP_GAP);
    totalW = caBaseOffX + caD.caWidth + MAP_GAP + cabD.cabWidth + 60;
  }
  const totalH = Math.max(caD.caHeight, trD.trHeight) + 40;
  const cw = wrap.clientWidth - 40, ch = wrap.clientHeight - 40;
  scale = Math.min(cw/totalW, ch/totalH, 1.0);
  vx = 20; vy = 50;
  updateTransform();
}

const wrap = document.getElementById('canvas-wrap');
let dragging=false, lx=0, ly=0, dragMoved=false;

wrap.addEventListener('mousedown', e => {
  // Allow drag from anywhere EXCEPT elements that have a click handler (nodes)
  // We detect nodes by their data attributes set during render
  const isNode = e.target.closest('[data-node]');
  const isDraggable = e.target.closest('[data-draggable]');
  if (isNode || isDraggable) return;
  dragging=true; dragMoved=false;
  lx=e.clientX; ly=e.clientY;
  wrap.classList.add('dragging');
  e.preventDefault();
});
window.addEventListener('mousemove', e => {
  if (!dragging) return;
  const dx=e.clientX-lx, dy=e.clientY-ly;
  if (Math.abs(dx)>2||Math.abs(dy)>2) dragMoved=true;
  vx+=dx; vy+=dy; lx=e.clientX; ly=e.clientY;
  updateTransform();
});
window.addEventListener('mouseup', e => {
  if (!dragging) return;
  dragging=false; wrap.classList.remove('dragging');
  // If we barely moved, treat as a background click → close sidebar
  if (!dragMoved) {
    const isNode = e.target.closest('[data-node]');
    const isDraggable = e.target.closest('[data-draggable]');
    if (!isNode && !isDraggable) {
      document.getElementById('sidebar').classList.remove('open');
      selectedNode=null; render();
    }
  }
});
wrap.addEventListener('wheel', e => {
  e.preventDefault();
  const f = e.deltaY>0?0.92:1.09;
  const rect = wrap.getBoundingClientRect();
  const mx=e.clientX-rect.left, my=e.clientY-rect.top;
  vx=mx-(mx-vx)*f; vy=my-(my-vy)*f;
  scale=Math.max(0.08,Math.min(3,scale*f)); updateTransform();
},{passive:false});

// Touch support
let ltd=null;
wrap.addEventListener('touchstart', e => {
  if (e.touches.length===1) {
    const isNode = e.target.closest('[data-node]');
    if (isNode) return;
    dragging=true; dragMoved=false;
    lx=e.touches[0].clientX; ly=e.touches[0].clientY;
  } else if (e.touches.length===2) {
    ltd=Math.hypot(e.touches[0].clientX-e.touches[1].clientX, e.touches[0].clientY-e.touches[1].clientY);
  }
},{passive:true});
wrap.addEventListener('touchmove', e => {
  e.preventDefault();
  if (e.touches.length===1 && dragging) {
    const dx=e.touches[0].clientX-lx, dy=e.touches[0].clientY-ly;
    if (Math.abs(dx)>2||Math.abs(dy)>2) dragMoved=true;
    vx+=dx; vy+=dy; lx=e.touches[0].clientX; ly=e.touches[0].clientY;
    updateTransform();
  } else if (e.touches.length===2 && ltd) {
    const d=Math.hypot(e.touches[0].clientX-e.touches[1].clientX, e.touches[0].clientY-e.touches[1].clientY);
    const cx=(e.touches[0].clientX+e.touches[1].clientX)/2;
    const cy=(e.touches[0].clientY+e.touches[1].clientY)/2;
    const f=d/ltd;
    vx=cx-(cx-vx)*f; vy=cy-(cy-vy)*f;
    scale=Math.max(0.08,Math.min(3,scale*f));
    updateTransform(); ltd=d;
  }
},{passive:false});
wrap.addEventListener('touchend', () => { dragging=false; ltd=null; },{passive:true});

// Buttons
document.getElementById('btn-zin').addEventListener('click',()=>{
  const cx=wrap.clientWidth/2,cy=wrap.clientHeight/2;
  scale=Math.min(3,scale*1.2);vx=cx-(cx-vx)*1.2;vy=cy-(cy-vy)*1.2;updateTransform();
});
document.getElementById('btn-zout').addEventListener('click',()=>{
  const cx=wrap.clientWidth/2,cy=wrap.clientHeight/2;
  scale=Math.max(0.08,scale/1.2);vx=cx-(cx-vx)/1.2;vy=cy-(cy-vy)/1.2;updateTransform();
});
document.getElementById('btn-fit').addEventListener('click',()=>fitView());
document.getElementById('btn-reset').addEventListener('click',()=>{
  // Handled above (clears node offsets too)
});
document.getElementById('btn-expand').addEventListener('click',()=>{
  collapsedCats.clear();collapsedMods.clear();render();fitView(false);
});
document.getElementById('btn-collapse').addEventListener('click',()=>{
  CATS.forEach(c=>collapsedCats.add(c.id));
  MODS.forEach(m=>collapsedMods.add(m.id));
  render();fitView(false);
});
document.getElementById('show-connections').addEventListener('change', e=>{
  showConnections=e.target.checked; render();
});

// ═══════════════════════════════════════════════════════
// SEARCH
// ═══════════════════════════════════════════════════════
const searchInput = document.getElementById('search-input');
const searchResults = document.getElementById('search-results');
searchInput.addEventListener('input', () => {
  const q = searchInput.value.trim().toLowerCase();
  if (!q) { searchResults.style.display='none'; return; }
  const results = [];
  for (const cat of CATS) {
    for (const sec of cat.sections) {
      if (sec.name.toLowerCase().includes(q)) {
        results.push({type:'sec', label:sec.name, sub:cat.name, catId:cat.id, isRenamed:!!RENAMED[sec.name]});
      } else {
        const a = (sec.articles||[]).find(x=>x.toLowerCase().includes(q));
        if (a) results.push({type:'art', label:sec.name, sub:cat.name, catId:cat.id, art:a});
      }
    }
  }
  for (const mod of MODS) {
    if (mod.name.toLowerCase().includes(q)) results.push({type:'mod', label:mod.num+' · '+mod.name, sub:'Treinamento', modId:mod.id});
    for (const les of mod.lessons) {
      if (les.n.toLowerCase().includes(q)) results.push({type:'les', label:les.n, sub:mod.num+' · '+mod.name, modId:mod.id});
    }
  }
  if (!results.length) { searchResults.style.display='none'; return; }
  searchResults.style.display='block';
  searchResults.innerHTML='';
  results.slice(0,14).forEach(r=>{
    const item=document.createElement('div');
    item.className='sr-item';
    const star = r.isRenamed ? ' <span class="sr-star">★</span>' : '';
    const art = r.art ? `<div class="sr-art">↳ ${r.art}</div>` : '';
    const typeTag = r.type==='mod'||r.type==='les' ? `<span style="font-size:9px;background:#EAF3DE;color:#27500A;border-radius:3px;padding:1px 5px;margin-left:4px">Treino</span>` : '';
    item.innerHTML=`<div><div class="sr-name">${r.label}${star}${typeTag}</div><div class="sr-cat">${r.sub}</div>${art}</div>`;
    item.addEventListener('click',()=>{
      searchResults.style.display='none';
      searchInput.value='';
      if (r.catId) { collapsedCats.delete(r.catId); render(); scrollToNode(r.catId,'ca'); }
      if (r.modId) { collapsedMods.delete(r.modId); render(); scrollToNode(r.modId,'tr'); }
    });
    searchResults.appendChild(item);
  });
});
document.addEventListener('click',e=>{
  if(!e.target.closest('#search-wrap')&&!e.target.closest('#search-results')) searchResults.style.display='none';
});

// ═══════════════════════════════════════════════════════
// INIT
// ═══════════════════════════════════════════════════════
render();
requestAnimationFrame(()=>fitView());
renderValidacoes();
renderComments();
window.addEventListener('resize',()=>{ fitView(false); updateMapLabels(); });
</script>
</body>
</html>
