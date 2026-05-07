<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Leads → Trello | iGreen</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600&family=DM+Mono:wght@400;500&display=swap');

  :root {
    --green: #1D9E75;
    --green-light: #E1F5EE;
    --green-dark: #0F6E56;
    --blue: #185FA5;
    --blue-light: #E6F1FB;
    --gray: #5F5E5A;
    --gray-light: #F1EFE8;
    --red: #A32D2D;
    --red-light: #FCEBEB;
    --text: #2C2C2A;
    --text-muted: #888780;
    --border: rgba(44,44,42,0.12);
    --bg: #FAFAF8;
    --card: #FFFFFF;
    --radius: 12px;
    --radius-sm: 8px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 32px 24px;
  }

  .header {
    max-width: 760px;
    margin: 0 auto 28px;
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .logo {
    width: 40px; height: 40px;
    background: var(--green);
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
  }

  .logo svg { width: 22px; height: 22px; fill: white; }

  .header-text h1 { font-size: 20px; font-weight: 600; letter-spacing: -0.3px; }
  .header-text p { font-size: 13px; color: var(--text-muted); margin-top: 2px; }

  .container { max-width: 760px; margin: 0 auto; display: flex; flex-direction: column; gap: 16px; }

  .card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px 24px;
  }

  .section-label {
    font-size: 11px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text-muted);
    margin-bottom: 14px;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .section-label span {
    background: var(--gray-light);
    color: var(--gray);
    width: 20px; height: 20px;
    border-radius: 50%;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: 11px;
    font-weight: 600;
  }

  /* Drop area */
  .drop-zone {
    border: 1.5px dashed var(--border);
    border-radius: var(--radius-sm);
    padding: 32px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    background: var(--bg);
  }

  .drop-zone:hover, .drop-zone.active {
    border-color: var(--green);
    background: var(--green-light);
  }

  .drop-icon {
    width: 44px; height: 44px;
    background: var(--green-light);
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    margin: 0 auto 12px;
  }

  .drop-icon svg { width: 22px; height: 22px; stroke: var(--green); fill: none; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }

  .drop-zone h3 { font-size: 15px; font-weight: 500; margin-bottom: 4px; }
  .drop-zone p { font-size: 13px; color: var(--text-muted); }

  /* Stats */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-bottom: 16px;
  }

  .stat-box {
    background: var(--bg);
    border-radius: var(--radius-sm);
    padding: 12px 14px;
    border: 1px solid var(--border);
  }

  .stat-box .label { font-size: 12px; color: var(--text-muted); margin-bottom: 4px; }
  .stat-box .value { font-size: 26px; font-weight: 600; letter-spacing: -1px; }
  .stat-box.ok .value { color: var(--green-dark); }
  .stat-box.skip .value { color: var(--text-muted); }

  /* Table */
  .table-wrap { overflow-x: auto; border-radius: var(--radius-sm); border: 1px solid var(--border); }

  table { width: 100%; border-collapse: collapse; font-size: 13px; }
  thead th {
    padding: 8px 12px;
    text-align: left;
    font-size: 11px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    color: var(--text-muted);
    background: var(--bg);
    border-bottom: 1px solid var(--border);
  }

  tbody tr { transition: background 0.1s; }
  tbody tr:hover { background: var(--bg); }
  tbody tr.dimmed { opacity: 0.4; }
  tbody td { padding: 9px 12px; border-bottom: 1px solid var(--border); color: var(--text); }
  tbody tr:last-child td { border-bottom: none; }

  .badge {
    display: inline-block;
    font-size: 11px;
    font-weight: 500;
    padding: 3px 9px;
    border-radius: 99px;
  }

  .badge-ok { background: var(--green-light); color: var(--green-dark); }
  .badge-skip { background: var(--gray-light); color: var(--gray); }
  .badge-src { background: var(--blue-light); color: var(--blue); }

  /* Lists selector */
  .lists-wrap { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 10px; }

  .list-chip {
    border: 1px solid var(--border);
    border-radius: 99px;
    padding: 6px 16px;
    font-size: 13px;
    font-weight: 500;
    cursor: pointer;
    background: var(--card);
    color: var(--text);
    transition: all 0.15s;
  }

  .list-chip:hover { border-color: var(--green); color: var(--green-dark); }
  .list-chip.selected { background: var(--green); color: white; border-color: var(--green); }

  /* Textarea */
  textarea {
    width: 100%;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 10px 14px;
    font-size: 14px;
    font-family: 'DM Sans', sans-serif;
    resize: vertical;
    min-height: 80px;
    background: var(--bg);
    color: var(--text);
    transition: border-color 0.15s;
    outline: none;
  }

  textarea:focus { border-color: var(--green); }

  .hint { font-size: 12px; color: var(--text-muted); margin-top: 6px; }

  /* Send button */
  .send-btn {
    width: 100%;
    padding: 14px;
    border: none;
    border-radius: var(--radius-sm);
    background: var(--green);
    color: white;
    font-size: 15px;
    font-weight: 600;
    font-family: 'DM Sans', sans-serif;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: -0.2px;
  }

  .send-btn:hover:not(:disabled) { background: var(--green-dark); transform: translateY(-1px); }
  .send-btn:active:not(:disabled) { transform: translateY(0); }
  .send-btn:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }
  .send-btn.done { background: var(--green-dark); }

  /* Log */
  .log-box {
    background: #1a1a18;
    border-radius: var(--radius-sm);
    padding: 12px 14px;
    font-family: 'DM Mono', monospace;
    font-size: 12px;
    max-height: 180px;
    overflow-y: auto;
    margin-top: 12px;
  }

  .log-line { margin-bottom: 3px; }
  .log-ok { color: #5DCAA5; }
  .log-err { color: #F09595; }
  .log-info { color: #85B7EB; }
  .log-time { color: #5F5E5A; margin-right: 8px; }

  .hidden { display: none !important; }

  #file-input { display: none; }

  .progress-bar-wrap {
    height: 4px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
    margin: 12px 0 0;
  }

  .progress-bar {
    height: 100%;
    background: var(--green);
    border-radius: 99px;
    transition: width 0.3s;
    width: 0%;
  }
</style>
</head>
<body>

<div class="header">
  <div class="logo">
    <svg viewBox="0 0 24 24"><path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/></svg>
  </div>
  <div class="header-text">
    <h1>Leads → Trello</h1>
    <p>iGreen Condomínios · Importador automático</p>
  </div>
</div>

<div class="container">

  <!-- Step 1: Upload -->
  <div class="card">
    <div class="section-label"><span>1</span> Carregar CSV de leads</div>
    <div class="drop-zone" id="drop-zone" onclick="document.getElementById('file-input').click()">
      <div class="drop-icon">
        <svg viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="12" y1="18" x2="12" y2="12"/><polyline points="9 15 12 12 15 15"/></svg>
      </div>
      <h3>Arraste o CSV aqui ou clique para selecionar</h3>
      <p>Exportado do Leads Extractor (LinkedIn, Google, Instagram...)</p>
    </div>
    <input type="file" id="file-input" accept=".csv,.txt">
  </div>

  <!-- Step 2: Preview -->
  <div class="card hidden" id="preview-card">
    <div class="section-label"><span>2</span> Leads encontrados</div>
    <div class="stats-grid">
      <div class="stat-box">
        <div class="label">Total de leads</div>
        <div class="value" id="stat-total">0</div>
      </div>
      <div class="stat-box ok">
        <div class="label">Com WhatsApp ✓</div>
        <div class="value" id="stat-ok">0</div>
      </div>
      <div class="stat-box skip">
        <div class="label">Ignorados</div>
        <div class="value" id="stat-skip">0</div>
      </div>
    </div>
    <div class="table-wrap">
      <table>
        <thead>
          <tr>
            <th>Nome / Empresa</th>
            <th>Número</th>
            <th>Fonte</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody id="leads-body"></tbody>
      </table>
    </div>
  </div>

  <!-- Step 3: Trello lists -->
  <div class="card hidden" id="trello-card">
    <div class="section-label"><span>3</span> Escolher lista do Trello</div>
    <div id="lists-loading" style="font-size:13px;color:var(--text-muted)">Carregando listas do board iGreen...</div>
    <div class="lists-wrap hidden" id="lists-wrap"></div>
  </div>

  <!-- Step 4: Message -->
  <div class="card hidden" id="msg-card">
    <div class="section-label"><span>4</span> Mensagem para o card (opcional)</div>
    <textarea id="msg-text" placeholder="Olá {nome}, tudo bem? Vi que você tem interesse em condomínios na região..."></textarea>
    <p class="hint">Use {nome} para personalizar. Essa mensagem fica salva no card do Trello para você copiar no OmegaChat.</p>
  </div>

  <!-- Step 5: Send -->
  <div class="card hidden" id="send-card">
    <div class="section-label"><span>5</span> Criar cards no Trello</div>
    <button class="send-btn" id="send-btn" onclick="sendToTrello()">
      Criar cards no Trello →
    </button>
    <div class="progress-bar-wrap hidden" id="progress-wrap">
      <div class="progress-bar" id="progress-bar"></div>
    </div>
    <div class="log-box hidden" id="log-box"></div>
  </div>

</div>

<script>
const API_KEY = 'b7bb29fa77e481a6228f6f53805cae76';
const TOKEN   = 'ATTA30f5314937d871c061611e8822155c5a9b81714f9d6cbe7ff56a116df21a180e7E35B5B3';
const BOARD_ID = '69fb7a054034e53cffd875f9';

let validLeads = [];
let selectedListId = null;

function formatPhone(raw) {
  if (!raw) return null;
  let n = String(raw).replace(/\D/g, '');
  if (!n) return null;
  if (n.startsWith('55') && n.length >= 12) return n;
  if (n.length === 11 && n[2] === '9') return '55' + n;
  if (n.length === 13 && n.startsWith('55')) return n;
  return null;
}

function detectSource(link) {
  if (!link) return 'Outro';
  const l = link.toLowerCase();
  if (l.includes('linkedin')) return 'LinkedIn';
  if (l.includes('instagram')) return 'Instagram';
  if (l.includes('facebook')) return 'Facebook';
  if (l.includes('google') || l.includes('maps')) return 'Google';
  return 'Outro';
}

function parseCSV(text) {
  const lines = text.split(/\r?\n/).filter(l => l.trim());
  if (lines.length < 2) return [];
  const sep = lines[0].includes('\t') ? '\t' : ',';
  const headers = lines[0].split(sep).map(h => h.trim().toLowerCase().replace(/['"]/g, ''));
  const rows = [];
  for (let i = 1; i < lines.length; i++) {
    const cols = lines[i].split(sep);
    const obj = {};
    headers.forEach((h, idx) => {
      obj[h] = (cols[idx] || '').trim().replace(/^"|"$/g, '');
    });
    rows.push(obj);
  }
  return rows;
}

function processLeads(rows) {
  return rows.map(r => {
    const name   = r['título'] || r['title'] || r['nome'] || r['name'] || r['empresa'] || r['company'] || 'Sem nome';
    const raw    = r['telefone'] || r['phone'] || r['celular'] || r['whatsapp'] || r['fone'] || '';
    const desc   = r['descrição'] || r['description'] || r['desc'] || '';
    const link   = r['link de origem'] || r['link'] || r['url'] || '';
    const phone  = formatPhone(raw);
    const source = detectSource(link);
    return { name, raw, phone, desc, link, source, valid: !!phone };
  });
}

function renderLeads(leads) {
  const body = document.getElementById('leads-body');
  body.innerHTML = '';
  const show = leads.slice(0, 60);
  show.forEach(l => {
    const tr = document.createElement('tr');
    if (!l.valid) tr.classList.add('dimmed');
    tr.innerHTML = `
      <td style="max-width:200px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap">${l.name}</td>
      <td style="font-family:'DM Mono',monospace;font-size:12px">${l.valid ? l.phone : (l.raw || '—')}</td>
      <td><span class="badge badge-src">${l.source}</span></td>
      <td><span class="badge ${l.valid ? 'badge-ok' : 'badge-skip'}">${l.valid ? '✓ WhatsApp' : 'ignorado'}</span></td>
    `;
    body.appendChild(tr);
  });
  if (leads.length > 60) {
    const tr = document.createElement('tr');
    tr.innerHTML = `<td colspan="4" style="color:var(--text-muted);font-size:12px;padding:10px 12px">+ ${leads.length - 60} leads não exibidos na prévia</td>`;
    body.appendChild(tr);
  }
}

function jsonp(url, callbackName) {
  return new Promise((resolve, reject) => {
    const name = callbackName || 'cb_' + Date.now();
    window[name] = (data) => { resolve(data); delete window[name]; script.remove(); };
    const script = document.createElement('script');
    script.src = url + (url.includes('?') ? '&' : '?') + 'callback=' + name;
    script.onerror = () => reject(new Error('Falha de rede'));
    document.head.appendChild(script);
  });
}

async function loadLists() {
  try {
    const lists = await jsonp(
      `https://api.trello.com/1/boards/${BOARD_ID}/lists?key=${API_KEY}&token=${TOKEN}`
    );
    document.getElementById('lists-loading').classList.add('hidden');
    const wrap = document.getElementById('lists-wrap');
    wrap.classList.remove('hidden');
    lists.forEach((list, i) => {
      const btn = document.createElement('button');
      btn.className = 'list-chip' + (i === 0 ? ' selected' : '');
      btn.textContent = list.name;
      btn.dataset.id = list.id;
      if (i === 0) selectedListId = list.id;
      btn.onclick = () => {
        document.querySelectorAll('.list-chip').forEach(b => b.classList.remove('selected'));
        btn.classList.add('selected');
        selectedListId = list.id;
      };
      wrap.appendChild(btn);
    });
  } catch(e) {
    document.getElementById('lists-loading').textContent = '⚠ Erro: ' + e.message;
    document.getElementById('lists-loading').style.color = 'var(--red)';
  }
}

async function trelloPost(endpoint, body) {
  return new Promise((resolve, reject) => {
    const name = 'cb_' + Date.now() + '_' + Math.random().toString(36).slice(2);
    window[name] = (data) => { resolve(data); delete window[name]; };
    const params = new URLSearchParams({ ...body, key: API_KEY, token: TOKEN, callback: name });
    const script = document.createElement('script');
    script.src = `https://api.trello.com/1/${endpoint}?${params}`;
    script.onerror = () => { reject(new Error('Falha')); delete window[name]; script.remove(); };
    document.head.appendChild(script);
  });
}

function log(msg, type = 'info') {
  const box = document.getElementById('log-box');
  box.classList.remove('hidden');
  const t = new Date().toLocaleTimeString('pt-BR', {hour:'2-digit',minute:'2-digit',second:'2-digit'});
  const line = document.createElement('div');
  line.className = 'log-line';
  line.innerHTML = `<span class="log-time">${t}</span><span class="log-${type}">${msg}</span>`;
  box.appendChild(line);
  box.scrollTop = box.scrollHeight;
}

async function sendToTrello() {
  if (!selectedListId) { alert('Selecione uma lista do Trello'); return; }
  const btn = document.getElementById('send-btn');
  btn.disabled = true;
  btn.textContent = 'Enviando...';
  const progWrap = document.getElementById('progress-wrap');
  const progBar  = document.getElementById('progress-bar');
  progWrap.classList.remove('hidden');
  const msgTemplate = document.getElementById('msg-text').value.trim();
  let ok = 0, err = 0;
  for (let i = 0; i < validLeads.length; i++) {
    const lead = validLeads[i];
    const msg  = msgTemplate.replace(/\{nome\}/gi, lead.name);
    const desc = [
      `📱 WhatsApp: https://wa.me/${lead.phone}`,
      lead.raw ? `📞 Tel original: ${lead.raw}` : '',
      `🔗 Fonte: ${lead.source}`,
      lead.link ? `🌐 ${lead.link}` : '',
      lead.desc ? `📝 ${lead.desc}` : '',
      msg ? `\n💬 Mensagem:\n${msg}` : ''
    ].filter(Boolean).join('\n');
    try {
      await trelloPost('cards', { name: lead.name, desc, idList: selectedListId });
      ok++;
      log('✓ ' + lead.name + ' — ' + lead.phone, 'ok');
    } catch(e) {
      err++;
      log('✗ Erro: ' + lead.name, 'err');
    }
    progBar.style.width = Math.round(((i+1) / validLeads.length) * 100) + '%';
    await new Promise(res => setTimeout(res, 150));
  }
  log(`─── Concluído: ${ok} cards criados, ${err} erros ───`, 'info');
  btn.classList.add('done');
  btn.textContent = `✓ ${ok} cards criados no Trello!`;
}

function handleFile(file) {
  const reader = new FileReader();
  reader.onload = e => {
    const rows  = parseCSV(e.target.result);
    const leads = processLeads(rows);
    validLeads  = leads.filter(l => l.valid);
    document.getElementById('stat-total').textContent = leads.length;
    document.getElementById('stat-ok').textContent    = validLeads.length;
    document.getElementById('stat-skip').textContent  = leads.length - validLeads.length;
    renderLeads(leads);
    ['preview-card','trello-card','msg-card','send-card'].forEach(id => {
      document.getElementById(id).classList.remove('hidden');
    });
    loadLists();
    document.getElementById('drop-zone').querySelector('h3').textContent = '✓ ' + file.name;
    document.getElementById('drop-zone').style.borderColor = 'var(--green)';
    document.getElementById('drop-zone').style.background  = 'var(--green-light)';
  };
  reader.readAsText(file, 'UTF-8');
}

document.getElementById('file-input').addEventListener('change', e => {
  if (e.target.files[0]) handleFile(e.target.files[0]);
});

const dz = document.getElementById('drop-zone');
dz.addEventListener('dragover', e => { e.preventDefault(); dz.classList.add('active'); });
dz.addEventListener('dragleave', () => dz.classList.remove('active'));
dz.addEventListener('drop', e => {
  e.preventDefault();
  dz.classList.remove('active');
  if (e.dataTransfer.files[0]) handleFile(e.dataTransfer.files[0]);
});
</script>
</body>
</html>
