<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Formulário de Peritagem Técnica — Fluidcom</title>
<style>
  * { -webkit-print-color-adjust: exact; print-color-adjust: exact; box-sizing: border-box; }
  :root{
    --navy:#14213D;
    --navy-2:#1F2F5C;
    --orange:#F2661D;
    --orange-light:#FDECE1;
    --bg:#F5F6FA;
    --white:#FFFFFF;
    --border:#E2E5EE;
    --green:#1B9C57;
    --amber:#E8A33D;
    --red:#D64545;
    --text:#1C2233;
    --text-muted:#5C6478;
    --mono: ui-monospace, SFMono-Regular, Consolas, "Liberation Mono", Menlo, monospace;
    --sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    --radius:10px;
    --shadow:0 2px 10px rgba(20,33,61,0.08);
  }
  html,body{ margin:0; padding:0; background:var(--bg); color:var(--text); font-family:var(--sans); }
  body{ padding-bottom:60px; }
  h1,h2,h3{ margin:0; }
  .container{ max-width:880px; margin:0 auto; padding:0 18px; }

  /* Topbar */
  .topbar{ background:var(--navy); border-bottom:4px solid var(--orange); }
  .topbar-inner{ max-width:880px; margin:0 auto; padding:16px 18px; display:flex; align-items:center; gap:18px; }
  .topbar-text h1{ font-size:16px; color:#fff; font-weight:700; letter-spacing:.02em; }
  .topbar-text p{ font-size:12px; color:#AEB7CE; margin:2px 0 0; }

  /* Save status badge */
  #saveStatus{ position:fixed; top:14px; right:14px; background:#fff; border:1px solid var(--border); border-radius:20px; padding:6px 14px; font-size:12px; font-weight:600; color:var(--text-muted); display:flex; align-items:center; gap:7px; box-shadow:var(--shadow); z-index:900; }
  #saveStatus .dot{ width:8px; height:8px; border-radius:50%; background:var(--green); flex-shrink:0; }
  #saveStatus.saving .dot{ background:var(--amber); animation:pulse 1s infinite; }
  #saveStatus.idle .dot{ background:#B7BDCC; }
  @keyframes pulse{ 0%,100%{opacity:1;} 50%{opacity:.35;} }

  /* Stepper */
  .stepper{ position:relative; display:flex; justify-content:space-between; max-width:520px; margin:26px auto 30px; padding:0 10px; }
  .stepper-line{ position:absolute; top:19px; left:44px; right:44px; height:3px; background:var(--border); z-index:0; border-radius:3px; overflow:hidden; }
  .stepper-line-fill{ height:100%; width:0%; background:var(--orange); transition:width .35s ease; }
  .step-item{ position:relative; z-index:1; display:flex; flex-direction:column; align-items:center; gap:7px; }
  .step-circle{ width:40px; height:40px; border-radius:50%; background:#fff; border:3px solid var(--border); display:flex; align-items:center; justify-content:center; font-family:var(--mono); font-weight:700; font-size:15px; color:var(--text-muted); transition:all .25s; }
  .step-item.active .step-circle{ border-color:var(--orange); color:var(--orange); background:var(--orange-light); }
  .step-item.completed .step-circle{ border-color:var(--orange); background:var(--orange); color:#fff; }
  .step-label{ font-size:11.5px; text-transform:uppercase; letter-spacing:.04em; color:var(--text-muted); font-weight:700; text-align:center; }
  .step-item.active .step-label{ color:var(--navy); }

  /* Resume banner */
  .resume-banner{ background:var(--orange-light); border:1.5px solid var(--orange); border-radius:var(--radius); padding:16px 18px; margin:18px 0; display:flex; justify-content:space-between; align-items:center; gap:16px; flex-wrap:wrap; }
  .resume-banner strong{ color:var(--navy); }
  .resume-banner p{ margin:4px 0 0; font-size:13px; color:var(--text-muted); }
  .resume-actions{ display:flex; gap:10px; flex-shrink:0; }

  /* Panels */
  .panel{ margin-bottom:22px; border-radius:var(--radius); overflow:hidden; box-shadow:var(--shadow); }
  .section-header{ background:var(--orange); color:#fff; font-weight:700; padding:13px 18px; font-size:14px; letter-spacing:.03em; text-transform:uppercase; display:flex; align-items:center; gap:10px; }
  .section-header .num{ font-family:var(--mono); background:rgba(255,255,255,.28); width:24px; height:24px; border-radius:6px; display:flex; align-items:center; justify-content:center; font-size:12.5px; flex-shrink:0; }
  .card{ background:#fff; border:1px solid var(--border); border-top:none; padding:22px; }

  /* Form elements */
  .field{ margin-bottom:16px; }
  label{ font-size:13px; font-weight:700; color:var(--navy); margin-bottom:6px; display:block; }
  label.required::after{ content:" *"; color:var(--red); }
  input[type=text], input[type=date], textarea, select{
    width:100%; padding:10px 12px; border:1.5px solid var(--border); border-radius:8px;
    font-size:14px; font-family:var(--sans); color:var(--text); background:#fff; transition:border-color .15s, box-shadow .15s;
  }
  input:focus, textarea:focus, select:focus{ outline:none; border-color:var(--orange); box-shadow:0 0 0 3px var(--orange-light); }
  input.invalid, textarea.invalid{ border-color:var(--red); box-shadow:0 0 0 3px #FBE4E4; }
  textarea{ min-height:90px; resize:vertical; font-family:var(--sans); }
  .hint{ font-size:12px; color:var(--text-muted); margin-top:5px; }
  .grid-2{ display:grid; grid-template-columns:1fr 1fr; gap:16px; }
  .grid-3{ display:grid; grid-template-columns:1fr 1fr 1fr; gap:16px; }
  @media(max-width:640px){ .grid-2, .grid-3{ grid-template-columns:1fr; } }
  .legend{ font-size:12.5px; color:var(--text-muted); margin-bottom:18px; }
  .legend .red{ color:var(--red); font-weight:700; }

  /* Buttons */
  .btn{ padding:12px 22px; border-radius:8px; font-weight:700; font-size:14px; border:none; cursor:pointer;
        display:inline-flex; align-items:center; gap:8px; transition:transform .1s, box-shadow .15s, background .15s; font-family:var(--sans); }
  .btn:active{ transform:scale(.97); }
  .btn-primary{ background:var(--orange); color:#fff; }
  .btn-primary:hover{ box-shadow:0 4px 14px rgba(242,102,29,.35); }
  .btn-secondary{ background:#fff; color:var(--navy); border:1.5px solid var(--border); }
  .btn-secondary:hover{ border-color:var(--navy); }
  .btn-danger{ background:#fff; color:var(--red); border:1.5px solid #F3D2D2; }
  .btn-danger:hover{ background:#FDF0F0; }
  .btn-sm{ padding:7px 13px; font-size:12.5px; }
  .btn:disabled{ opacity:.5; cursor:not-allowed; }
  .btn-row{ display:flex; justify-content:space-between; gap:12px; margin-top:6px; flex-wrap:wrap; }
  .btn-row-end{ display:flex; justify-content:flex-end; gap:12px; margin-top:6px; flex-wrap:wrap; }

  /* Image preview / drop area */
  .imgdrop{ border:2px dashed var(--border); border-radius:10px; padding:18px; text-align:center; background:#FAFBFD; cursor:pointer; }
  .imgdrop:hover{ border-color:var(--orange); background:var(--orange-light); }
  .imgdrop input{ display:none; }
  .imgdrop-label{ font-size:13.5px; color:var(--text-muted); font-weight:600; }
  #achadoPreviewWrap{ display:none; margin-top:14px; text-align:center; }
  #achadoPreviewWrap img{ max-width:100%; max-height:260px; border-radius:8px; border:1px solid var(--border); }
  #achadoPreviewMeta{ font-size:12px; color:var(--text-muted); margin-top:8px; font-family:var(--mono); }

  /* OS summary card (compact, shown in step 2/3) */
  .os-summary{ background:#fff; border:1px solid var(--border); border-radius:var(--radius); padding:16px 18px; margin-bottom:20px; box-shadow:var(--shadow); }
  .os-summary-head{ display:flex; justify-content:space-between; align-items:center; margin-bottom:10px; }
  .os-summary-head h3{ font-size:13px; text-transform:uppercase; letter-spacing:.04em; color:var(--text-muted); }
  .os-summary-grid{ display:grid; grid-template-columns:repeat(3,1fr); gap:10px 20px; }
  @media(max-width:640px){ .os-summary-grid{ grid-template-columns:1fr 1fr; } }
  .os-summary-item .k{ font-size:11px; color:var(--text-muted); text-transform:uppercase; letter-spacing:.03em; }
  .os-summary-item .v{ font-size:13.5px; font-weight:600; color:var(--navy); font-family:var(--mono); word-break:break-word; }

  /* Achado cards */
  .achado-card{ display:flex; gap:16px; background:#fff; border:1px solid var(--border); border-radius:10px; padding:14px; margin-bottom:14px; box-shadow:0 1px 4px rgba(20,33,61,.05); }
  .achado-thumb{ width:100px; height:100px; object-fit:cover; border-radius:8px; border:1px solid var(--border); flex-shrink:0; background:#F0F1F5; }
  .achado-thumb-full{ width:220px; height:auto; max-height:260px; object-fit:cover; border-radius:8px; border:1px solid var(--border); flex-shrink:0; background:#F0F1F5; }
  @media(max-width:640px){ .achado-thumb-full{ width:100%; max-height:220px; } .achado-card{ flex-direction:column; } }
  .achado-info{ flex:1; min-width:0; }
  .achado-badge{ font-family:var(--mono); font-size:11px; background:var(--navy); color:#fff; padding:2px 8px; border-radius:50px; display:inline-block; margin-bottom:6px; }
  .achado-title{ font-weight:700; color:var(--navy); margin-bottom:4px; font-size:15px; }
  .achado-desc{ color:var(--text); font-size:13.5px; line-height:1.5; white-space:pre-wrap; }
  .achado-time{ font-size:11px; color:var(--text-muted); font-family:var(--mono); margin-top:6px; }
  .achado-actions{ display:flex; gap:8px; margin-top:10px; }
  .empty-note{ text-align:center; padding:30px 20px; color:var(--text-muted); font-size:13.5px; background:#FAFBFD; border:1.5px dashed var(--border); border-radius:10px; }

  /* Report meta (step 3) */
  .report-meta{ display:flex; gap:22px; flex-wrap:wrap; margin-bottom:18px; }
  .report-meta .stat{ background:#fff; border:1px solid var(--border); border-radius:10px; padding:12px 18px; box-shadow:var(--shadow); }
  .report-meta .stat .num{ font-family:var(--mono); font-size:22px; font-weight:700; color:var(--orange); }
  .report-meta .stat .lbl{ font-size:11px; color:var(--text-muted); text-transform:uppercase; letter-spacing:.03em; }

  /* Toast */
  .toast{ position:fixed; bottom:22px; right:22px; left:auto; max-width:90vw; background:var(--navy); color:#fff; padding:12px 20px; border-radius:8px;
          box-shadow:0 6px 20px rgba(0,0,0,.22); font-size:13.5px; font-weight:600; display:flex; align-items:center; gap:9px;
          opacity:0; transform:translateY(12px); transition:all .3s; z-index:1000; pointer-events:none; }
  .toast.show{ opacity:1; transform:translateY(0); }
  .toast.success{ background:var(--green); }
  .toast.error{ background:var(--red); }

  .step-section{ display:none; }
  .step-section.active{ display:block; }

  footer.appfoot{ text-align:center; font-size:11.5px; color:var(--text-muted); margin-top:30px; padding-bottom:10px; }

  /* Print */
  @media print{
    #saveStatus, .stepper, .no-print, .report-meta, .resume-banner { display:none !important; }
    body{ background:#fff; padding:0; margin:0; }
    .container { max-width: 100%; width: 100%; padding: 0; margin: 0; }
    
    /* Garantir que a seção ativa de impressão apareça */
    .step-section { display: none !important; }
    .step-section.active { display: block !important; }
    
    /* Identificação */
    #osSummaryStep3 { 
      margin-bottom: 20px; 
      border: 1px solid #ccc;
      padding: 20px;
      break-inside: avoid;
    }
    
    /* Achados: Resto do documento sem cortes */
    .achado-card { 
      break-inside: avoid; 
      border: 1px solid #ccc; 
      margin-bottom: 20px;
      page-break-inside: avoid;
      flex-direction: column !important;
      align-items: center;
    }
    
    /* Imagem completa, sem cortes e mantendo proporção */
    .achado-thumb-full {
      width: 100% !important;
      height: auto !important;
      max-height: none !important;
      object-fit: contain !important;
      flex-shrink: 0;
    }
    /* Garantir que a imagem não ultrapasse a largura */
    .achado-card img {
      max-width: 100% !important;
      height: auto !important;
    }
    
    .panel { box-shadow:none; border:1px solid #ccc; break-inside:avoid; }
    .topbar { break-inside:avoid; }
  }
</style>
</head>
<body>

  <div id="saveStatus" class="idle"><span class="dot"></span><span id="saveStatusText">Aguardando...</span></div>

  <div class="topbar">
    <div class="topbar-inner">
      <div class="topbar-text">
        <h1>Formulário de Peritagem Técnica</h1>
        <p>Comércio e Serviços Hidráulicos — uso offline, sem necessidade de internet</p>
      </div>
    </div>
  </div>

  <div class="container">

    <div class="stepper" id="stepper">
      <div class="stepper-line"><div class="stepper-line-fill" id="stepperFill"></div></div>
      <div class="step-item" data-step="1">
        <div class="step-circle">1</div>
        <div class="step-label">Identificação</div>
      </div>
      <div class="step-item" data-step="2">
        <div class="step-circle">2</div>
        <div class="step-label">Achados</div>
      </div>
      <div class="step-item" data-step="3">
        <div class="step-circle">3</div>
        <div class="step-label">Revisão Final</div>
      </div>
    </div>

    <div id="resumeBanner" class="resume-banner" style="display:none;">
      <div>
        <strong>Peritagem em andamento encontrada.</strong>
        <p id="resumeInfo"></p>
      </div>
      <div class="resume-actions">
        <button class="btn btn-secondary btn-sm" id="btnStartNew">Iniciar Novo Relatório</button>
        <button class="btn btn-primary btn-sm" id="btnResume">Continuar Este Relatório</button>
      </div>
    </div>

    <!-- STEP 1 -->
    <section class="step-section" id="step1">
      <form id="osForm" novalidate>
        <div class="panel">
          <div class="section-header"><span class="num">1</span> Identificação da Ordem de Serviço</div>
          <div class="card">
            <p class="legend">Campos marcados com <span class="red">*</span> são obrigatórios.</p>

            <div class="grid-3">
              <div class="field">
                <label class="required" for="os-numero">Número da OS</label>
                <input type="text" id="os-numero" placeholder="Ex: 0000">
              </div>
              <div class="field">
                <label class="required" for="os-cliente">Cliente</label>
                <input type="text" id="os-cliente" placeholder="Nome do cliente">
              </div>
              <div class="field">
                <label class="required" for="os-data">Data de Execução</label>
                <input type="date" id="os-data">
              </div>
            </div>

            <div class="grid-2">
              <div class="field">
                <label class="required" for="os-equipamento">Equipamento</label>
                <input type="text" id="os-equipamento" placeholder="Ex: Bomba hidráulica, redutor, etc.">
              </div>
              <div class="field">
                <label for="os-tag">TAG</label>
                <input type="text" id="os-tag" placeholder="Ex: BM-1023 (se houver)">
              </div>
            </div>

            <div class="grid-2">
              <div class="field">
                <label for="os-fabricante">Fabricante</label>
                <input type="text" id="os-fabricante" placeholder="Ex: Rexroth, Parker...">
              </div>
              <div class="field">
                <label for="os-modelo">Modelo</label>
                <input type="text" id="os-modelo" placeholder="Modelo do equipamento">
              </div>
            </div>

            <div class="grid-2">
              <div class="field">
                <label for="os-tipoServico">Tipo de Serviço</label>
                <input type="text" id="os-tipoServico" placeholder="Peritagem técnica">
              </div>
              <div class="field">
                <label class="required" for="os-responsavel">Responsável Técnico</label>
                <input type="text" id="os-responsavel" placeholder="Nome do perito">
              </div>
            </div>

            <div class="btn-row-end">
              <button type="submit" class="btn btn-primary">Salvar e Continuar →</button>
            </div>
          </div>
        </div>
      </form>
    </section>

    <!-- STEP 2 -->
    <section class="step-section" id="step2">
      <div id="osSummaryStep2"></div>

      <div class="panel">
        <div class="section-header"><span class="num">2</span> Registrar Achado da Peritagem</div>
        <div class="card">
          <form id="achadoForm" novalidate>
            <div class="field">
              <label class="required">Imagem do Achado</label>
              <div class="imgdrop" id="imgDropArea">
                <div class="imgdrop-label" id="imgDropLabel">📷 Toque para selecionar ou tirar uma foto (máx. 10 MB)</div>
                <input type="file" id="achado-imagem" accept="image/*">
              </div>
              <div id="achadoPreviewWrap">
                <img id="achadoPreviewImg" src="" alt="Pré-visualização">
                <div id="achadoPreviewMeta"></div>
              </div>
            </div>

            <div class="field">
              <label class="required" for="achado-titulo">Identificação do Item / Título</label>
              <input type="text" id="achado-titulo" placeholder="Ex: Desgaste no rolamento traseiro">
            </div>

            <div class="field">
              <label class="required" for="achado-descricao">Descrição do Problema</label>
              <textarea id="achado-descricao" placeholder="Descreva o que foi encontrado e qual problema representa..."></textarea>
            </div>

            <div class="btn-row-end">
              <button type="button" class="btn btn-secondary" id="achadoCancelBtn" style="display:none;">Cancelar Edição</button>
              <button type="submit" class="btn btn-primary" id="achadoSubmitBtn">+ Adicionar Achado</button>
            </div>
          </form>
        </div>
      </div>

      <h3 style="font-size:13px;text-transform:uppercase;letter-spacing:.04em;color:var(--text-muted);margin:22px 0 12px;">Achados Registrados (<span id="achadoCountStep2">0</span>)</h3>
      <div id="achadosListStep2"></div>

      <div class="btn-row">
        <button class="btn btn-secondary" id="btnBackTo1">← Editar Identificação</button>
        <button class="btn btn-primary" id="btnGoTo3">Revisar Relatório Final →</button>
      </div>
    </section>

    <!-- STEP 3 -->
    <section class="step-section" id="step3">
      <div id="osSummaryStep3"></div>

      <div class="report-meta">
        <div class="stat"><div class="num" id="metaCount">0</div><div class="lbl">Achados registrados</div></div>
        <div class="stat"><div class="num" id="metaSize">0 KB</div><div class="lbl">Tamanho total de imagens</div></div>
        <div class="stat"><div class="num" id="metaDate">--</div><div class="lbl">Revisão gerada em</div></div>
      </div>

      <div id="achadosListStep3"></div>

      <div class="btn-row no-print">
        <button class="btn btn-secondary" id="btnBackTo2">← Voltar aos Achados</button>
        <div style="display:flex; gap:10px; flex-wrap:wrap; justify-content:flex-end;">
          <button class="btn btn-secondary" id="btnExportJSON">⬇ Backup (.json)</button>
          <button class="btn btn-secondary" id="btnImportJSONTrigger">📤 Restaurar Backup</button>
          <input type="file" id="importJSONInput" accept="application/json" style="display:none;">
          <button class="btn btn-secondary" id="btnPrint">🖨 Imprimir / Salvar PDF</button>
          <button class="btn btn-danger" id="btnNewReport">🆕 Novo Relatório</button>
        </div>
      </div>
    </section>

    <footer class="appfoot no-print">Fluidcom Comércio e Serviços Hidráulicos — Ferramenta interna de peritagem, funciona 100% offline neste arquivo.</footer>
  </div>

  <div class="toast" id="toast"><span id="toastMsg"></span></div>

<script>
(function(){
  "use strict";

  /* ---------------- Storage layer (IndexedDB with in-memory fallback) ---------------- */
  function idbRequestToPromise(req){
    return new Promise(function(resolve,reject){
      req.onsuccess = function(){ resolve(req.result); };
      req.onerror = function(){ reject(req.error); };
    });
  }
  function idbTxDone(tx){
    return new Promise(function(resolve,reject){
      tx.oncomplete = function(){ resolve(); };
      tx.onerror = function(){ reject(tx.error); };
      tx.onabort = function(){ reject(tx.error); };
    });
  }

  var Store = {
    db: null,
    fallback: false,
    memory: { os: null, achados: [] },
    fallbackNextId: 1,

    init: function(){
      var self = this;
      if(!window.indexedDB){ self.fallback = true; return Promise.resolve(); }
      return new Promise(function(resolve){
        try{
          var req = indexedDB.open('fluidcomPeritagemDB', 1);
          req.onupgradeneeded = function(e){
            var db = e.target.result;
            if(!db.objectStoreNames.contains('ordemServico')) db.createObjectStore('ordemServico', {keyPath:'id'});
            if(!db.objectStoreNames.contains('achados')) db.createObjectStore('achados', {keyPath:'id', autoIncrement:true});
          };
          req.onsuccess = function(e){ self.db = e.target.result; resolve(); };
          req.onerror = function(){ console.warn('IndexedDB indisponível, usando memória de sessão.'); self.fallback = true; resolve(); };
        }catch(err){
          console.warn('IndexedDB indisponível, usando memória de sessão.', err);
          self.fallback = true;
          resolve();
        }
      });
    },

    saveOS: function(data){
      var record = Object.assign({id:'atual'}, data);
      if(this.fallback){ this.memory.os = record; return Promise.resolve(record); }
      var tx = this.db.transaction('ordemServico','readwrite');
      tx.objectStore('ordemServico').put(record);
      return idbTxDone(tx).then(function(){ return record; });
    },

    getOS: function(){
      if(this.fallback) return Promise.resolve(this.memory.os);
      var tx = this.db.transaction('ordemServico','readonly');
      var req = tx.objectStore('ordemServico').get('atual');
      return idbRequestToPromise(req);
    },

    addAchado: function(achado){
      var self = this;
      if(this.fallback){
        var id = this.fallbackNextId++;
        var record = Object.assign({id:id}, achado);
        this.memory.achados.push(record);
        return Promise.resolve(id);
      }
      var tx = this.db.transaction('achados','readwrite');
      var req = tx.objectStore('achados').add(achado);
      return idbRequestToPromise(req).then(function(id){
        return idbTxDone(tx).then(function(){ return id; });
      });
    },

    updateAchado: function(id, achado){
      var record = Object.assign({}, achado, {id:id});
      if(this.fallback){
        var idx = this.memory.achados.findIndex(function(a){ return a.id === id; });
        if(idx > -1) this.memory.achados[idx] = record;
        return Promise.resolve();
      }
      var tx = this.db.transaction('achados','readwrite');
      tx.objectStore('achados').put(record);
      return idbTxDone(tx);
    },

    deleteAchado: function(id){
      if(this.fallback){
        this.memory.achados = this.memory.achados.filter(function(a){ return a.id !== id; });
        return Promise.resolve();
      }
      var tx = this.db.transaction('achados','readwrite');
      tx.objectStore('achados').delete(id);
      return idbTxDone(tx);
    },

    getAllAchados: function(){
      if(this.fallback) return Promise.resolve(this.memory.achados.slice());
      var tx = this.db.transaction('achados','readonly');
      var req = tx.objectStore('achados').getAll();
      return idbRequestToPromise(req);
    },

    clearAll: function(){
      if(this.fallback){ this.memory.os = null; this.memory.achados = []; this.fallbackNextId = 1; return Promise.resolve(); }
      var tx = this.db.transaction(['ordemServico','achados'],'readwrite');
      tx.objectStore('ordemServico').clear();
      tx.objectStore('achados').clear();
      return idbTxDone(tx);
    }
  };

  /* ---------------- App state ---------------- */
  var currentOS = null;
  var achados = [];
  var editingAchadoId = null;
  var pendingImageData = null;
  var pendingImageName = null;
  var pendingImageSize = 0;
  var MAX_IMAGE_BYTES = 10 * 1024 * 1024;

  /* ---------------- Utilities ---------------- */
  function escapeHtml(str){
    if(str === null || str === undefined) return '';
    return String(str).replace(/[&<>"']/g, function(c){
      return ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'})[c];
    });
  }
  function formatBytes(bytes){
    if(!bytes) return '0 KB';
    var kb = bytes / 1024;
    if(kb < 1024) return kb.toFixed(1) + ' KB';
    return (kb/1024).toFixed(2) + ' MB';
  }
  function formatDateBR(isoDate){
    if(!isoDate) return '—';
    var parts = isoDate.split('-');
    if(parts.length !== 3) return isoDate;
    return parts[2] + '/' + parts[1] + '/' + parts[0];
  }
  function nowStamp(){
    var d = new Date();
    function p(n){ return String(n).padStart(2,'0'); }
    return p(d.getDate()) + '/' + p(d.getMonth()+1) + '/' + d.getFullYear() + ' às ' + p(d.getHours()) + ':' + p(d.getMinutes());
  }
  function todayISO(){
    var d = new Date();
    return d.getFullYear() + '-' + String(d.getMonth()+1).padStart(2,'0') + '-' + String(d.getDate()).padStart(2,'0');
  }

  var toastTimer = null;
  function showToast(msg, type){
    var toast = document.getElementById('toast');
    var toastMsg = document.getElementById('toastMsg');
    toast.className = 'toast show' + (type ? (' ' + type) : '');
    toastMsg.textContent = msg;
    clearTimeout(toastTimer);
    toastTimer = setTimeout(function(){ toast.className = 'toast'; }, 2600);
  }

  function setSaveStatus(state, text){
    var el = document.getElementById('saveStatus');
    el.className = state;
    document.getElementById('saveStatusText').textContent = text;
  }

  /* ---------------- Stepper ---------------- */
  function goToStep(n){
    document.querySelectorAll('.step-section').forEach(function(sec){ sec.classList.remove('active'); });
    document.getElementById('step' + n).classList.add('active');

    document.querySelectorAll('.step-item').forEach(function(item){
      var s = parseInt(item.getAttribute('data-step'), 10);
      item.classList.remove('active','completed');
      if(s < n) item.classList.add('completed');
      else if(s === n) item.classList.add('active');
    });
    var fillPct = n === 1 ? 0 : (n === 2 ? 50 : 100);
    document.getElementById('stepperFill').style.width = fillPct + '%';

    if(n === 2){ renderOSSummary('osSummaryStep2', false); renderAchadosList('achadosListStep2', false); }
    if(n === 3){ renderOSSummary('osSummaryStep3', true); renderAchadosList('achadosListStep3', true); renderReportMeta(); }

    window.scrollTo({top:0, behavior:'smooth'});
  }

  /* ---------------- OS Form (Step 1) ---------------- */
  var osRequiredFields = ['os-numero','os-cliente','os-equipamento','os-responsavel','os-data'];

  function fillOSForm(data){
    if(!data) return;
    document.getElementById('os-numero').value = data.numero || '';
    document.getElementById('os-cliente').value = data.cliente || '';
    document.getElementById('os-equipamento').value = data.equipamento || '';
    document.getElementById('os-tag').value = data.tag || '';
    document.getElementById('os-fabricante').value = data.fabricante || '';
    document.getElementById('os-modelo').value = data.modelo || '';
    document.getElementById('os-tipoServico').value = data.tipoServico || '';
    document.getElementById('os-responsavel').value = data.responsavel || '';
    document.getElementById('os-data').value = data.data || '';
  }

  function collectOSForm(){
    return {
      numero: document.getElementById('os-numero').value.trim(),
      cliente: document.getElementById('os-cliente').value.trim(),
      equipamento: document.getElementById('os-equipamento').value.trim(),
      tag: document.getElementById('os-tag').value.trim(),
      fabricante: document.getElementById('os-fabricante').value.trim(),
      modelo: document.getElementById('os-modelo').value.trim(),
      tipoServico: document.getElementById('os-tipoServico').value.trim() || 'Peritagem técnica',
      responsavel: document.getElementById('os-responsavel').value.trim(),
      data: document.getElementById('os-data').value
    };
  }

  function validateOSForm(){
    var ok = true;
    var firstInvalid = null;
    osRequiredFields.forEach(function(id){
      var el = document.getElementById(id);
      if(!el.value || !el.value.trim()){
        el.classList.add('invalid');
        ok = false;
        if(!firstInvalid) firstInvalid = el;
      } else {
        el.classList.remove('invalid');
      }
    });
    if(!ok){
      showToast('Preencha os campos obrigatórios da identificação.', 'error');
      if(firstInvalid) firstInvalid.focus();
    }
    return ok;
  }

  document.getElementById('osForm').addEventListener('submit', function(e){
    e.preventDefault();
    if(!validateOSForm()) return;
    var data = collectOSForm();
    setSaveStatus('saving', 'Salvando...');
    Store.saveOS(data).then(function(record){
      currentOS = record;
      setSaveStatus('idle', 'Salvo às ' + new Date().toLocaleTimeString('pt-BR'));
      showToast('Identificação salva com sucesso.', 'success');
      goToStep(2);
    }).catch(function(err){
      console.error(err);
      setSaveStatus('idle', 'Erro ao salvar');
      showToast('Erro ao salvar. Tente novamente.', 'error');
    });
  });

  document.getElementById('btnBackTo1').addEventListener('click', function(){ goToStep(1); });

  /* ---------------- OS Summary rendering ---------------- */
  function renderOSSummary(containerId, editable){
    var container = document.getElementById(containerId);
    if(!currentOS){ container.innerHTML = ''; return; }
    var editBtn = '<button class="btn btn-secondary btn-sm no-print" onclick="window.__editOS()">✎ Editar Identificação</button>';
    container.innerHTML =
      '<div class="os-summary">' +
        '<div class="os-summary-head"><h3>Identificação da Ordem de Serviço</h3>' + editBtn + '</div>' +
        '<div class="os-summary-grid">' +
          osField('Nº da OS', currentOS.numero) +
          osField('Cliente', currentOS.cliente) +
          osField('Data', formatDateBR(currentOS.data)) +
          osField('Equipamento', currentOS.equipamento) +
          osField('TAG', currentOS.tag || '—') +
          osField('Fabricante', currentOS.fabricante || '—') +
          osField('Modelo', currentOS.modelo || '—') +
          osField('Tipo de Serviço', currentOS.tipoServico) +
          osField('Responsável Técnico', currentOS.responsavel) +
        '</div>' +
      '</div>';
  }
  function osField(k,v){
    return '<div class="os-summary-item"><div class="k">' + escapeHtml(k) + '</div><div class="v">' + escapeHtml(v || '—') + '</div></div>';
  }
  window.__editOS = function(){ goToStep(1); };

  /* ---------------- Achado image handling ---------------- */
  document.getElementById('imgDropArea').addEventListener('click', function(){
    document.getElementById('achado-imagem').click();
  });

  document.getElementById('achado-imagem').addEventListener('change', function(e){
    var file = e.target.files && e.target.files[0];
    if(!file) return;
    if(file.type.indexOf('image/') !== 0){
      showToast('Selecione um arquivo de imagem válido (JPG, PNG, etc).', 'error');
      e.target.value = '';
      return;
    }
    if(file.size > MAX_IMAGE_BYTES){
      showToast('Imagem muito grande (' + formatBytes(file.size) + '). O limite é 10 MB.', 'error');
      e.target.value = '';
      return;
    }
    var reader = new FileReader();
    reader.onload = function(ev){
      pendingImageData = ev.target.result;
      pendingImageName = file.name;
      pendingImageSize = file.size;
      document.getElementById('achadoPreviewImg').src = pendingImageData;
      document.getElementById('achadoPreviewMeta').textContent = file.name + ' — ' + formatBytes(file.size);
      document.getElementById('achadoPreviewWrap').style.display = 'block';
      document.getElementById('imgDropLabel').textContent = '📷 Imagem selecionada — toque para trocar';
    };
    reader.readAsDataURL(file);
  });

  function resetAchadoForm(){
    document.getElementById('achadoForm').reset();
    document.getElementById('achadoPreviewWrap').style.display = 'none';
    document.getElementById('imgDropLabel').textContent = '📷 Toque para selecionar ou tirar uma foto (máx. 10 MB)';
    pendingImageData = null; pendingImageName = null; pendingImageSize = 0;
    editingAchadoId = null;
    document.getElementById('achadoSubmitBtn').textContent = '+ Adicionar Achado';
    document.getElementById('achadoCancelBtn').style.display = 'none';
    document.querySelectorAll('#achadoForm input, #achadoForm textarea').forEach(function(el){ el.classList.remove('invalid'); });
  }

  document.getElementById('achadoCancelBtn').addEventListener('click', function(){ resetAchadoForm(); });

  document.getElementById('achadoForm').addEventListener('submit', function(e){
    e.preventDefault();
    var titulo = document.getElementById('achado-titulo').value.trim();
    var descricao = document.getElementById('achado-descricao').value.trim();
    var tituloEl = document.getElementById('achado-titulo');
    var descEl = document.getElementById('achado-descricao');
    var ok = true;

    if(!titulo){ tituloEl.classList.add('invalid'); ok = false; } else { tituloEl.classList.remove('invalid'); }
    if(!descricao){ descEl.classList.add('invalid'); ok = false; } else { descEl.classList.remove('invalid'); }
    if(!pendingImageData){ showToast('Selecione uma imagem para este achado.', 'error'); ok = false; }

    if(!ok){
      if(!titulo || !descricao) showToast('Preencha a identificação do item e a descrição.', 'error');
      return;
    }

    var achadoObj = {
      titulo: titulo,
      descricao: descricao,
      imagemData: pendingImageData,
      imagemNome: pendingImageName,
      imagemTamanho: pendingImageSize,
      criadoEm: editingAchadoId ? (achados.find(function(a){return a.id===editingAchadoId;}) || {}).criadoEm || nowStamp() : nowStamp(),
      atualizadoEm: nowStamp()
    };

    setSaveStatus('saving', 'Salvando...');

    if(editingAchadoId){
      Store.updateAchado(editingAchadoId, achadoObj).then(function(){
        var idx = achados.findIndex(function(a){ return a.id === editingAchadoId; });
        if(idx > -1) achados[idx] = Object.assign({id:editingAchadoId}, achadoObj);
        setSaveStatus('idle', 'Atualizado às ' + new Date().toLocaleTimeString('pt-BR'));
        showToast('Achado atualizado com sucesso.', 'success');
        resetAchadoForm();
        renderAchadosList('achadosListStep2', false);
      }).catch(function(err){ console.error(err); showToast('Erro ao atualizar achado.', 'error'); });
    } else {
      Store.addAchado(achadoObj).then(function(id){
        achados.push(Object.assign({id:id}, achadoObj));
        setSaveStatus('idle', 'Salvo às ' + new Date().toLocaleTimeString('pt-BR'));
        showToast('Achado adicionado com sucesso.', 'success');
        resetAchadoForm();
        renderAchadosList('achadosListStep2', false);
      }).catch(function(err){ console.error(err); showToast('Erro ao salvar achado.', 'error'); });
    }
  });

  function startEditAchado(id){
    var achado = achados.find(function(a){ return a.id === id; });
    if(!achado) return;
    editingAchadoId = id;
    document.getElementById('achado-titulo').value = achado.titulo;
    document.getElementById('achado-descricao').value = achado.descricao;
    pendingImageData = achado.imagemData;
    pendingImageName = achado.imagemNome;
    pendingImageSize = achado.imagemTamanho;
    document.getElementById('achadoPreviewImg').src = achado.imagemData;
    document.getElementById('achadoPreviewMeta').textContent = (achado.imagemNome || 'imagem') + ' — ' + formatBytes(achado.imagemTamanho);
    document.getElementById('achadoPreviewWrap').style.display = 'block';
    document.getElementById('imgDropLabel').textContent = '📷 Imagem selecionada — toque para trocar';
    document.getElementById('achadoSubmitBtn').textContent = '💾 Salvar Alteração';
    document.getElementById('achadoCancelBtn').style.display = 'inline-flex';
    goToStep(2);
    document.getElementById('achadoForm').scrollIntoView({behavior:'smooth', block:'start'});
  }

  function removeAchado(id){
    if(!confirm('Remover este achado da peritagem? Esta ação não pode ser desfeita.')) return;
    setSaveStatus('saving', 'Removendo...');
    Store.deleteAchado(id).then(function(){
      achados = achados.filter(function(a){ return a.id !== id; });
      setSaveStatus('idle', 'Removido às ' + new Date().toLocaleTimeString('pt-BR'));
      showToast('Achado removido.', 'success');
      renderAchadosList('achadosListStep2', false);
      renderAchadosList('achadosListStep3', true);
      renderReportMeta();
      if(editingAchadoId === id) resetAchadoForm();
    }).catch(function(err){ console.error(err); showToast('Erro ao remover achado.', 'error'); });
  }

  window.__startEditAchado = startEditAchado;
  window.__removeAchado = removeAchado;

  /* ---------------- Achados list rendering ---------------- */
  function renderAchadosList(containerId, fullView){
    var container = document.getElementById(containerId);
    if(document.getElementById('achadoCountStep2')) document.getElementById('achadoCountStep2').textContent = achados.length;

    if(achados.length === 0){
      container.innerHTML = '<div class="empty-note">Nenhum achado registrado até o momento. Use o formulário acima para adicionar fotos e descrições dos problemas encontrados.</div>';
      return;
    }
    var html = '';
    achados.forEach(function(a, i){
      var thumbClass = fullView ? 'achado-thumb-full' : 'achado-thumb';
      var actions = fullView
        ? '<div class="achado-actions no-print">' +
            '<button class="btn btn-secondary btn-sm" onclick="window.__startEditAchado(' + a.id + ')">✎ Editar</button>' +
            '<button class="btn btn-danger btn-sm" onclick="window.__removeAchado(' + a.id + ')">🗑 Remover</button>' +
          '</div>'
        : '<div class="achado-actions">' +
            '<button type="button" class="btn btn-secondary btn-sm" onclick="window.__startEditAchado(' + a.id + ')">✎ Editar</button>' +
            '<button type="button" class="btn btn-danger btn-sm" onclick="window.__removeAchado(' + a.id + ')">🗑 Remover</button>' +
          '</div>';
      html +=
        '<div class="achado-card">' +
          '<img class="' + thumbClass + '" src="' + a.imagemData + '" alt="Achado ' + (i+1) + '">' +
          '<div class="achado-info">' +
            '<span class="achado-badge">ACHADO #' + (i+1) + '</span>' +
            '<div class="achado-title">' + escapeHtml(a.titulo) + '</div>' +
            '<div class="achado-desc">' + escapeHtml(a.descricao) + '</div>' +
            '<div class="achado-time">Registrado em ' + escapeHtml(a.criadoEm || '') + (a.atualizadoEm && a.atualizadoEm !== a.criadoEm ? ' · atualizado em ' + escapeHtml(a.atualizadoEm) : '') + '</div>' +
            actions +
          '</div>' +
        '</div>';
    });
    container.innerHTML = html;
  }

  function renderReportMeta(){
    document.getElementById('metaCount').textContent = achados.length;
    var totalBytes = achados.reduce(function(sum,a){ return sum + (a.imagemTamanho||0); }, 0);
    document.getElementById('metaSize').textContent = formatBytes(totalBytes);
    document.getElementById('metaDate').textContent = nowStamp();
  }

  /* ---------------- Step navigation buttons ---------------- */
  document.getElementById('btnGoTo3').addEventListener('click', function(){
    if(achados.length === 0){
      if(!confirm('Nenhum achado foi registrado ainda. Deseja mesmo avançar para a revisão final?')) return;
    }
    goToStep(3);
  });
  document.getElementById('btnBackTo2').addEventListener('click', function(){ goToStep(2); });

  /* ---------------- Export / Import / Print / New report ---------------- */
  document.getElementById('btnExportJSON').addEventListener('click', function(){
    var payload = { ordemServico: currentOS, achados: achados, exportadoEm: nowStamp(), versao: 1 };
    var blob = new Blob([JSON.stringify(payload, null, 2)], {type:'application/json'});
    var url = URL.createObjectURL(blob);
    var a = document.createElement('a');
    var numero = (currentOS && currentOS.numero) ? currentOS.numero.replace(/[^a-zA-Z0-9-_]/g,'') : 'sem_numero';
    var dataStr = (currentOS && currentOS.data) ? currentOS.data : todayISO();
    a.href = url;
    a.download = 'Peritagem_OS' + numero + '_' + dataStr + '.json';
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    setTimeout(function(){ URL.revokeObjectURL(url); }, 2000);
    showToast('Backup baixado com sucesso.', 'success');
  });

  document.getElementById('btnImportJSONTrigger').addEventListener('click', function(){
    document.getElementById('importJSONInput').click();
  });

  document.getElementById('importJSONInput').addEventListener('change', function(e){
    var file = e.target.files && e.target.files[0];
    if(!file) return;
    var reader = new FileReader();
    reader.onload = function(ev){
      var data;
      try{ data = JSON.parse(ev.target.result); }
      catch(err){ showToast('Arquivo de backup inválido.', 'error'); return; }
      if(!data || typeof data !== 'object' || !('ordemServico' in data)){
        showToast('Arquivo de backup em formato não reconhecido.', 'error');
        return;
      }
      if(!confirm('Restaurar este backup substituirá todos os dados atuais do formulário. Deseja continuar?')) return;

      setSaveStatus('saving', 'Restaurando...');
      Store.clearAll().then(function(){
        var chain = Promise.resolve();
        if(data.ordemServico){
          chain = chain.then(function(){ return Store.saveOS(data.ordemServico); }).then(function(rec){ currentOS = rec; });
        }
        var novosAchados = [];
        (data.achados || []).forEach(function(a){
          chain = chain.then(function(){
            var clean = { titulo:a.titulo, descricao:a.descricao, imagemData:a.imagemData, imagemNome:a.imagemNome, imagemTamanho:a.imagemTamanho, criadoEm:a.criadoEm, atualizadoEm:a.atualizadoEm };
            return Store.addAchado(clean).then(function(id){ novosAchados.push(Object.assign({id:id}, clean)); });
          });
        });
        return chain.then(function(){
          achados = novosAchados;
          fillOSForm(currentOS);
          setSaveStatus('idle', 'Backup restaurado');
          showToast('Backup restaurado com sucesso.', 'success');
          goToStep(3);
        });
      }).catch(function(err){ console.error(err); showToast('Erro ao restaurar backup.', 'error'); });
    };
    reader.readAsText(file);
    e.target.value = '';
  });

  document.getElementById('btnPrint').addEventListener('click', function(){
    window.print();
  });

  document.getElementById('btnNewReport').addEventListener('click', function(){
    if(!confirm('Iniciar um novo relatório apagará todos os dados da peritagem atual (identificação e achados). Deseja continuar?')) return;
    setSaveStatus('saving', 'Limpando...');
    Store.clearAll().then(function(){
      currentOS = null;
      achados = [];
      editingAchadoId = null;
      document.getElementById('osForm').reset();
      resetAchadoForm();
      document.getElementById('os-data').value = todayISO();
      document.getElementById('os-tipoServico').value = 'Peritagem técnica';
      setSaveStatus('idle', 'Pronto para novo relatório');
      showToast('Novo relatório iniciado.', 'success');
      goToStep(1);
    }).catch(function(err){ console.error(err); showToast('Erro ao iniciar novo relatório.', 'error'); });
  });

  /* ---------------- Init ---------------- */
  document.getElementById('btnResume').addEventListener('click', function(){
    document.getElementById('resumeBanner').style.display = 'none';
    fillOSForm(currentOS);
    goToStep(2);
  });
  document.getElementById('btnStartNew').addEventListener('click', function(){
    if(!confirm('Iniciar um novo relatório apagará o relatório em andamento (identificação e achados). Deseja continuar?')) return;
    document.getElementById('resumeBanner').style.display = 'none';
    setSaveStatus('saving', 'Limpando...');
    Store.clearAll().then(function(){
      currentOS = null;
      achados = [];
      document.getElementById('os-data').value = todayISO();
      document.getElementById('os-tipoServico').value = 'Peritagem técnica';
      setSaveStatus('idle', 'Pronto para novo relatório');
      goToStep(1);
    });
  });

  function init(){
    document.getElementById('os-data').value = todayISO();
    document.getElementById('os-tipoServico').value = 'Peritagem técnica';

    Store.init().then(function(){
      if(Store.fallback){
        showToast('Armazenamento persistente indisponível neste navegador — os dados serão mantidos apenas durante esta sessão. Use o backup (.json) com frequência.', 'error');
      }
      return Promise.all([Store.getOS(), Store.getAllAchados()]);
    }).then(function(results){
      var os = results[0];
      var achadosSalvos = results[1] || [];
      if(os){
        currentOS = os;
        achados = achadosSalvos;
        document.getElementById('resumeInfo').textContent =
          'OS ' + (os.numero || '—') + ' · Cliente: ' + (os.cliente || '—') + ' · Equipamento: ' + (os.equipamento || '—') + ' · ' + achados.length + ' achado(s) registrado(s).';
        document.getElementById('resumeBanner').style.display = 'flex';
        goToStep(1);
      } else {
        goToStep(1);
      }
      setSaveStatus('idle', Store.fallback ? 'Modo sessão (sem persistência)' : 'Dados salvos localmente');
    }).catch(function(err){
      console.error(err);
      goToStep(1);
    });
  }

  init();
})();
</script>
</body>
</html>
