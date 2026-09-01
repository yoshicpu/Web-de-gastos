<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="theme-color" content="#0a0a0a">
  <title>Guardar Dinheiro</title>
  <style>
    :root {
      --bg: #0a0a0a;
      --surface: #161616;
      --surface2: #1f1f1f;
      --border: #2a2a2a;
      --text: #f0f0f0;
      --text-muted: #888;
      --green: #22c55e;
      --red: #ef4444;
      --blue: #3b82f6;
      --purple: #a855f7;
      --orange: #f97316;
      --accent: #22c55e;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Segoe UI", Roboto, sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      min-height: -webkit-fill-available;
      padding: 0;
      overflow-x: hidden;
      padding-bottom: 80px;
    }

    .container {
      max-width: 480px;
      margin: 0 auto;
      padding: 16px 16px 20px;
    }

    /* Header */
    .topbar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 0 18px;
    }
    .topbar h1 {
      font-size: 1.35rem;
      font-weight: 700;
      letter-spacing: -0.4px;
      background: linear-gradient(135deg, #22c55e, #4ade80);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .menu-btn {
      width: 40px;
      height: 40px;
      border-radius: 10px;
      background: var(--surface);
      border: 1px solid var(--border);
      color: var(--text);
      font-size: 1.2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
    }

    /* Drawer */
    .overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.6);
      z-index: 90;
      opacity: 0;
      visibility: hidden;
      transition: all 0.25s;
    }
    .overlay.open { opacity: 1; visibility: visible; }

    .drawer {
      position: fixed;
      top: 0;
      right: 0;
      width: 280px;
      max-width: 80vw;
      height: 100%;
      background: var(--surface);
      border-left: 1px solid var(--border);
      z-index: 100;
      transform: translateX(100%);
      transition: transform 0.28s ease;
      padding: 24px 16px;
      display: flex;
      flex-direction: column;
      overflow-y: auto;
    }
    .drawer.open { transform: translateX(0); }

    .drawer h3 {
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 1px;
      color: var(--text-muted);
      margin-bottom: 12px;
      margin-top: 8px;
    }
    .drawer h3:first-child { margin-top: 0; }

    .drawer-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 13px 12px;
      border-radius: 12px;
      color: var(--text);
      font-weight: 500;
      font-size: 0.95rem;
      cursor: pointer;
      border: none;
      background: transparent;
      width: 100%;
      text-align: left;
      margin-bottom: 2px;
    }
    .drawer-item:active, .drawer-item.active {
      background: var(--surface2);
    }
    .drawer-item.active { color: var(--green); }
    .drawer-item .icon { font-size: 1.2rem; width: 26px; text-align: center; }

    .drawer-footer {
      margin-top: auto;
      padding-top: 16px;
      border-top: 1px solid var(--border);
      font-size: 0.72rem;
      color: #555;
      text-align: center;
    }

    /* Totals */
    .totals {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-bottom: 14px;
    }
    .total-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 14px 12px;
      text-align: center;
    }
    .total-card.full { grid-column: 1 / -1; }
    .total-card.balance {
      background: linear-gradient(145deg, #0f1a12, #0a0a0a);
      border-color: #1a3a24;
    }
    .total-card.invest {
      background: linear-gradient(145deg, #120f1a, #0a0a0a);
      border-color: #2a1a3a;
    }
    .total-card .label {
      font-size: 0.7rem;
      text-transform: uppercase;
      letter-spacing: 0.7px;
      color: var(--text-muted);
      margin-bottom: 4px;
    }
    .total-card .value {
      font-size: 1.25rem;
      font-weight: 700;
      font-variant-numeric: tabular-nums;
    }
    .total-card.full .value { font-size: 1.5rem; }
    .value.positive { color: var(--green); }
    .value.negative { color: var(--red); }
    .value.neutral { color: var(--text); }
    .value.purple { color: var(--purple); }

    /* Metas chips */
    .metas {
      display: flex;
      gap: 8px;
      overflow-x: auto;
      padding-bottom: 6px;
      margin-bottom: 14px;
      -webkit-overflow-scrolling: touch;
    }
    .metas::-webkit-scrollbar { display: none; }
    .meta-chip {
      flex-shrink: 0;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 8px 14px;
      font-size: 0.85rem;
      font-weight: 600;
      color: var(--text-muted);
      white-space: nowrap;
    }
    .meta-chip .val { color: var(--green); margin-left: 4px; }

    /* Bottom Nav */
    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background: var(--surface);
      border-top: 1px solid var(--border);
      display: flex;
      justify-content: space-around;
      padding: 8px 0 calc(8px + env(safe-area-inset-bottom));
      z-index: 50;
      max-width: 480px;
      margin: 0 auto;
    }
    .nav-item {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 2px;
      background: none;
      border: none;
      color: var(--text-muted);
      font-size: 0.7rem;
      font-weight: 600;
      padding: 6px 4px;
      cursor: pointer;
    }
    .nav-item .icon { font-size: 1.3rem; }
    .nav-item.active { color: var(--green); }
    .nav-item.gastos.active { color: var(--red); }
    .nav-item.invest.active { color: var(--purple); }

    /* Pages */
    .page { display: none; }
    .page.active { display: block; }

    .section-title {
      font-size: 1.05rem;
      font-weight: 600;
      margin-bottom: 12px;
    }

    /* Forms */
    .input-group { margin-bottom: 12px; }
    .input-group label {
      display: block;
      font-size: 0.78rem;
      color: var(--text-muted);
      margin-bottom: 5px;
      font-weight: 500;
    }

    input[type="number"],
    input[type="text"],
    select {
      width: 100%;
      padding: 13px 14px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      color: var(--text);
      font-size: 1rem;
      outline: none;
      -webkit-appearance: none;
      appearance: none;
    }
    input:focus, select:focus { border-color: var(--accent); }
    input::placeholder { color: #555; }

    select {
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' fill='%23888' viewBox='0 0 16 16'%3E%3Cpath d='M8 11L3 6h10l-5 5z'/%3E%3C/svg%3E");
      background-repeat: no-repeat;
      background-position: right 14px center;
      padding-right: 36px;
    }

    input[type="number"]::-webkit-inner-spin-button,
    input[type="number"]::-webkit-outer-spin-button {
      -webkit-appearance: none;
    }

    .btn {
      width: 100%;
      padding: 14px;
      border: none;
      border-radius: 12px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      transition: transform 0.12s;
    }
    .btn:active { transform: scale(0.98); }
    .btn-green { background: var(--green); color: #000; }
    .btn-red { background: var(--red); color: #fff; }
    .btn-purple { background: var(--purple); color: #fff; }
    .btn-blue { background: var(--blue); color: #fff; }
    .btn-outline {
      background: transparent;
      border: 1px solid var(--border);
      color: var(--text-muted);
      padding: 10px;
      font-size: 0.9rem;
    }

    /* Quick buttons */
    .quick {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 14px;
    }
    .quick-btn {
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 7px 12px;
      font-size: 0.8rem;
      color: var(--text-muted);
      cursor: pointer;
    }
    .quick-btn:active {
      border-color: var(--green);
      color: var(--green);
    }

    /* History */
    .history { margin-top: 22px; }
    .history h2 {
      font-size: 0.95rem;
      font-weight: 600;
      margin-bottom: 10px;
      color: var(--text-muted);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .history h2 span { font-size: 0.78rem; font-weight: 400; }

    .empty {
      text-align: center;
      padding: 28px 12px;
      color: var(--text-muted);
      font-size: 0.9rem;
    }

    .list { list-style: none; }

    .item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px 14px;
      margin-bottom: 8px;
      animation: fadeIn 0.22s ease;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(5px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .item-left {
      display: flex;
      flex-direction: column;
      gap: 2px;
      min-width: 0;
    }
    .item-desc {
      font-weight: 500;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    .item-meta {
      font-size: 0.72rem;
      color: var(--text-muted);
    }
    .item-tag {
      display: inline-block;
      font-size: 0.68rem;
      padding: 1px 7px;
      border-radius: 6px;
      margin-right: 4px;
      font-weight: 600;
      background: rgba(34,197,94,0.15);
      color: #4ade80;
    }
    .tag-invest {
      background: rgba(168,85,247,0.18);
      color: #c084fc;
    }

    .item-right {
      display: flex;
      align-items: center;
      gap: 10px;
      flex-shrink: 0;
    }
    .item-value {
      font-weight: 700;
      font-variant-numeric: tabular-nums;
      font-size: 0.95rem;
    }
    .item-value.guardar { color: var(--green); }
    .item-value.gasto { color: var(--red); }
    .item-value.investimento { color: var(--purple); }

    .btn-delete {
      background: transparent;
      border: none;
      color: #555;
      font-size: 1.05rem;
      padding: 4px 5px;
      cursor: pointer;
      border-radius: 6px;
      line-height: 1;
    }
    .btn-delete:active {
      color: var(--red);
      background: rgba(239,68,68,0.12);
    }

    .btn-clear {
      background: transparent;
      border: 1px solid var(--border);
      color: var(--text-muted);
      padding: 8px 12px;
      border-radius: 8px;
      font-size: 0.8rem;
      cursor: pointer;
      margin-top: 10px;
      width: 100%;
    }

    /* ===== Gerenciar Metas ===== */
    .meta-manage-list {
      list-style: none;
      margin-bottom: 16px;
    }
    .meta-manage-item {
      display: flex;
      align-items: center;
      gap: 10px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 10px 12px;
      margin-bottom: 8px;
    }
    .meta-manage-item input {
      flex: 1;
      padding: 8px 10px;
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 8px;
      color: var(--text);
      font-size: 0.95rem;
      outline: none;
    }
    .meta-manage-item input:focus { border-color: var(--accent); }

    .meta-actions {
      display: flex;
      gap: 6px;
      flex-shrink: 0;
    }
    .btn-icon {
      width: 34px;
      height: 34px;
      border-radius: 8px;
      border: 1px solid var(--border);
      background: var(--surface2);
      color: var(--text-muted);
      font-size: 0.9rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .btn-icon:active { background: var(--border); }
    .btn-icon.danger:active { color: var(--red); border-color: var(--red); }

    .add-meta-row {
      display: flex;
      gap: 8px;
      margin-bottom: 8px;
    }
    .add-meta-row input {
      flex: 1;
    }
    .add-meta-row .btn {
      width: auto;
      padding: 12px 16px;
      white-space: nowrap;
    }

    .hint {
      font-size: 0.78rem;
      color: #666;
      margin-top: 8px;
      line-height: 1.4;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Top bar -->
    <div class="topbar">
      <h1>💰 Guardar Dinheiro</h1>
      <button class="menu-btn" id="menu-btn" aria-label="Menu">☰</button>
    </div>

    <!-- Totals -->
    <div class="totals">
      <div class="total-card">
        <div class="label">Total Guardado</div>
        <div class="value positive" id="totalGuardado">R$ 0,00</div>
      </div>
      <div class="total-card">
        <div class="label">Total Gastos</div>
        <div class="value negative" id="totalGastos">R$ 0,00</div>
      </div>
      <div class="total-card full balance">
        <div class="label">Saldo (Guardado − Gastos)</div>
        <div class="value neutral" id="saldo">R$ 0,00</div>
      </div>
      <div class="total-card full invest">
        <div class="label">Total em Investimentos</div>
        <div class="value purple" id="totalInvest">R$ 0,00</div>
      </div>
    </div>

    <!-- Metas chips -->
    <div class="metas" id="metas-chips"></div>

    <!-- ========== PAGE: GUARDAR ========== -->
    <div class="page active" id="page-guardar">
      <p class="section-title">Adicionar valor guardado</p>

      <div class="quick" id="quick-metas"></div>

      <div class="input-group">
        <label for="meta-guardar">Para qual meta?</label>
        <select id="meta-guardar"></select>
      </div>

      <div class="input-group">
        <label for="valor-guardar">Valor guardado (R$)</label>
        <input type="number" id="valor-guardar" placeholder="0,00" step="0.01" min="0" inputmode="decimal">
      </div>

      <div class="input-group">
        <label for="desc-guardar">Descrição (opcional)</label>
        <input type="text" id="desc-guardar" placeholder="Ex: Parte do salário...">
      </div>

      <button class="btn btn-green" id="btn-add-guardar">Adicionar Guardado</button>

      <div class="history">
        <h2>Histórico de Guardados <span id="count-guardar">0</span></h2>
        <ul class="list" id="lista-guardar"></ul>
        <div class="empty" id="empty-guardar">Nenhum valor guardado ainda.</div>
      </div>
    </div>

    <!-- ========== PAGE: GASTOS ========== -->
    <div class="page" id="page-gastos">
      <p class="section-title">Adicionar gasto</p>

      <div class="input-group">
        <label for="valor-gasto">Valor do gasto (R$)</label>
        <input type="number" id="valor-gasto" placeholder="0,00" step="0.01" min="0" inputmode="decimal">
      </div>

      <div class="input-group">
        <label for="desc-gasto">Descrição (opcional)</label>
        <input type="text" id="desc-gasto" placeholder="Ex: Mercado, luz, uber...">
      </div>

      <button class="btn btn-red" id="btn-add-gasto">Adicionar Gasto</button>

      <div class="history">
        <h2>Histórico de Gastos <span id="count-gastos">0</span></h2>
        <ul class="list" id="lista-gastos"></ul>
        <div class="empty" id="empty-gastos">Nenhum gasto registrado.</div>
      </div>
    </div>

    <!-- ========== PAGE: INVESTIMENTOS ========== -->
    <div class="page" id="page-invest">
      <p class="section-title">Adicionar investimento</p>

      <div class="input-group">
        <label for="tipo-invest">Tipo de investimento</label>
        <select id="tipo-invest">
          <option value="Ações">📈 Ações</option>
          <option value="Fundos">🏦 Fundos</option>
          <option value="Tesouro">🇧🇷 Tesouro Direto</option>
          <option value="Criptomoedas">₿ Criptomoedas</option>
          <option value="Poupança">🐷 Poupança</option>
          <option value="Outro">📦 Outro</option>
        </select>
      </div>

      <div class="input-group">
        <label for="valor-invest">Valor investido (R$)</label>
        <input type="number" id="valor-invest" placeholder="0,00" step="0.01" min="0" inputmode="decimal">
      </div>

      <div class="input-group">
        <label for="desc-invest">Descrição (opcional)</label>
        <input type="text" id="desc-invest" placeholder="Ex: PETR4, Bitcoin...">
      </div>

      <button class="btn btn-purple" id="btn-add-invest">Adicionar Investimento</button>

      <div class="history">
        <h2>Histórico de Investimentos <span id="count-invest">0</span></h2>
        <ul class="list" id="lista-invest"></ul>
        <div class="empty" id="empty-invest">Nenhum investimento ainda.</div>
      </div>
    </div>

    <!-- ========== PAGE: GERENCIAR METAS ========== -->
    <div class="page" id="page-metas">
      <p class="section-title">Gerenciar onde guardar</p>
      <p class="hint" style="margin-bottom:14px;">
        Edite os nomes ou adicione novas opções (ex: Viagem, Emergência, Faculdade...).
      </p>

      <ul class="meta-manage-list" id="meta-manage-list"></ul>

      <div class="add-meta-row">
        <input type="text" id="nova-meta" placeholder="Nome da nova meta..." maxlength="30">
        <button class="btn btn-blue" id="btn-add-meta">＋</button>
      </div>

      <p class="hint">
        Toque no nome para editar. Use ✕ para excluir (só se não tiver valores vinculados).
      </p>
    </div>

    <button class="btn-clear" id="btn-clear" style="display:none; margin-top:16px;">Limpar todos os dados</button>
  </div>

  <!-- Bottom Navigation -->
  <nav class="bottom-nav">
    <button class="nav-item active" data-page="guardar">
      <span class="icon">💰</span>
      Guardar
    </button>
    <button class="nav-item gastos" data-page="gastos">
      <span class="icon">📉</span>
      Gastos
    </button>
    <button class="nav-item invest" data-page="invest">
      <span class="icon">📊</span>
      Investir
    </button>
  </nav>

  <!-- Side Menu -->
  <div class="overlay" id="overlay"></div>
  <aside class="drawer" id="drawer">
    <h3>Navegação</h3>
    <button class="drawer-item active" data-page="guardar">
      <span class="icon">💰</span> Guardar Dinheiro
    </button>
    <button class="drawer-item" data-page="gastos">
      <span class="icon">📉</span> Gastos
    </button>
    <button class="drawer-item" data-page="invest">
      <span class="icon">📊</span> Investimentos
    </button>

    <h3>Configurações</h3>
    <button class="drawer-item" data-page="metas">
      <span class="icon">✏️</span> Editar Metas
    </button>

    <div class="drawer-footer">
      Dados salvos neste dispositivo<br>
      Tema escuro • Safari
    </div>
  </aside>

  <script>
    // ===== Default Metas =====
    const DEFAULT_METAS = ['Carro', 'Casa', 'Geral', 'Outro'];

    // ===== State =====
    let lancamentos = JSON.parse(localStorage.getItem('guardarDinheiro_v2')) || [];
    let metas = JSON.parse(localStorage.getItem('guardarDinheiro_metas')) || [...DEFAULT_METAS];

    // ===== Helpers =====
    function formatBRL(v) {
      return v.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    }

    function formatDate(iso) {
      const d = new Date(iso);
      return d.toLocaleDateString('pt-BR', {
        day: '2-digit', month: '2-digit', year: '2-digit',
        hour: '2-digit', minute: '2-digit'
      });
    }

    function save() {
      localStorage.setItem('guardarDinheiro_v2', JSON.stringify(lancamentos));
      localStorage.setItem('guardarDinheiro_metas', JSON.stringify(metas));
    }

    // ===== Metas UI =====
    function atualizarSelectMetas() {
      const select = document.getElementById('meta-guardar');
      const valorAtual = select.value;
      select.innerHTML = '';
      metas.forEach(m => {
        const opt = document.createElement('option');
        opt.value = m;
        opt.textContent = m;
        select.appendChild(opt);
      });
      // Tenta manter a seleção anterior
      if (metas.includes(valorAtual)) select.value = valorAtual;
    }

    function atualizarQuickMetas() {
      const container = document.getElementById('quick-metas');
      container.innerHTML = '';
      metas.forEach(m => {
        const btn = document.createElement('button');
        btn.className = 'quick-btn';
        btn.textContent = m;
        btn.addEventListener('click', () => {
          document.getElementById('meta-guardar').value = m;
        });
        container.appendChild(btn);
      });
    }

    function renderGerenciarMetas() {
      const list = document.getElementById('meta-manage-list');
      list.innerHTML = '';

      metas.forEach((nome, index) => {
        const li = document.createElement('li');
        li.className = 'meta-manage-item';

        const input = document.createElement('input');
        input.type = 'text';
        input.value = nome;
        input.maxLength = 30;
        input.dataset.index = index;

        // Salva ao perder o foco ou apertar Enter
        const salvarNome = () => {
          const novoNome = input.value.trim();
          if (!novoNome) {
            input.value = metas[index]; // reverte
            return;
          }
          if (novoNome === metas[index]) return;

          // Verifica duplicata
          if (metas.some((m, i) => i !== index && m.toLowerCase() === novoNome.toLowerCase())) {
            alert('Já existe uma meta com esse nome.');
            input.value = metas[index];
            return;
          }

          const antigo = metas[index];
          metas[index] = novoNome;

          // Atualiza lançamentos que usavam o nome antigo
          lancamentos.forEach(l => {
            if (l.tipo === 'guardar' && l.meta === antigo) {
              l.meta = novoNome;
            }
          });

          save();
          atualizarSelectMetas();
          atualizarQuickMetas();
          render();
        };

        input.addEventListener('blur', salvarNome);
        input.addEventListener('keydown', e => {
          if (e.key === 'Enter') {
            input.blur();
          }
        });

        const actions = document.createElement('div');
        actions.className = 'meta-actions';

        const btnDel = document.createElement('button');
        btnDel.className = 'btn-icon danger';
        btnDel.textContent = '✕';
        btnDel.title = 'Excluir';
        btnDel.addEventListener('click', () => {
          const temValores = lancamentos.some(l => l.tipo === 'guardar' && l.meta === metas[index]);
          if (temValores) {
            alert('Não é possível excluir: existem valores guardados nessa meta.\nApague ou mova os valores primeiro.');
            return;
          }
          if (metas.length <= 1) {
            alert('Você precisa ter pelo menos uma meta.');
            return;
          }
          if (confirm(`Excluir a meta "${metas[index]}"?`)) {
            metas.splice(index, 1);
            save();
            atualizarSelectMetas();
            atualizarQuickMetas();
            renderGerenciarMetas();
            render();
          }
        });

        actions.appendChild(btnDel);
        li.appendChild(input);
        li.appendChild(actions);
        list.appendChild(li);
      });
    }

    // ===== Calculate & Render =====
    function calcularTotais() {
      let guardado = 0, gastos = 0, invest = 0;
      const totaisMeta = {};

      metas.forEach(m => { totaisMeta[m] = 0; });

      lancamentos.forEach(l => {
        if (l.tipo === 'guardar') {
          guardado += l.valor;
          if (totaisMeta[l.meta] !== undefined) {
            totaisMeta[l.meta] += l.valor;
          } else {
            // Meta antiga que não existe mais → conta como "Outro" ou cria temporário
            if (!totaisMeta['_outros']) totaisMeta['_outros'] = 0;
            totaisMeta['_outros'] += l.valor;
          }
        } else if (l.tipo === 'gasto') {
          gastos += l.valor;
        } else if (l.tipo === 'investimento') {
          invest += l.valor;
        }
      });

      const saldo = guardado - gastos;

      document.getElementById('totalGuardado').textContent = formatBRL(guardado);
      document.getElementById('totalGastos').textContent = formatBRL(gastos);
      document.getElementById('totalInvest').textContent = formatBRL(invest);

      const saldoEl = document.getElementById('saldo');
      saldoEl.textContent = formatBRL(saldo);
      saldoEl.className = 'value ' + (saldo > 0 ? 'positive' : saldo < 0 ? 'negative' : 'neutral');

      // Chips
      const chips = document.getElementById('metas-chips');
      chips.innerHTML = '';
      Object.entries(totaisMeta).forEach(([nome, val]) => {
        if (val > 0) {
          const chip = document.createElement('div');
          chip.className = 'meta-chip';
          const label = nome === '_outros' ? 'Outros' : nome;
          chip.innerHTML = `${label} <span class="val">${formatBRL(val)}</span>`;
          chips.appendChild(chip);
        }
      });
    }

    function renderLista(tipo, listaId, emptyId, countId) {
      const lista = document.getElementById(listaId);
      const empty = document.getElementById(emptyId);
      const count = document.getElementById(countId);

      const items = lancamentos.filter(l => l.tipo === tipo);
      lista.innerHTML = '';

      if (items.length === 0) {
        empty.style.display = 'block';
        count.textContent = '0';
      } else {
        empty.style.display = 'none';
        count.textContent = items.length;

        [...items].reverse().forEach(l => {
          const realIndex = lancamentos.indexOf(l);
          const li = document.createElement('li');
          li.className = 'item';

          let tagHtml = '';
          if (tipo === 'guardar') {
            tagHtml = `<span class="item-tag">${l.meta || 'Geral'}</span>`;
          } else if (tipo === 'investimento') {
            tagHtml = `<span class="item-tag tag-invest">${l.meta || 'Invest'}</span>`;
          }

          const sinal = tipo === 'gasto' ? '−' : '+';
          const classeValor = tipo === 'gasto' ? 'gasto' : (tipo === 'investimento' ? 'investimento' : 'guardar');

          li.innerHTML = `
            <div class="item-left">
              <span class="item-desc">${l.descricao || (tipo === 'guardar' ? 'Guardado' : tipo === 'gasto' ? 'Gasto' : 'Investimento')}</span>
              <span class="item-meta">${tagHtml}${formatDate(l.data)}</span>
            </div>
            <div class="item-right">
              <span class="item-value ${classeValor}">${sinal} ${formatBRL(l.valor)}</span>
              <button class="btn-delete" data-index="${realIndex}" aria-label="Excluir">✕</button>
            </div>
          `;
          lista.appendChild(li);
        });
      }
    }

    function render() {
      calcularTotais();
      renderLista('guardar', 'lista-guardar', 'empty-guardar', 'count-guardar');
      renderLista('gasto', 'lista-gastos', 'empty-gastos', 'count-gastos');
      renderLista('investimento', 'lista-invest', 'empty-invest', 'count-invest');

      const hasData = lancamentos.length > 0;
      document.getElementById('btn-clear').style.display = hasData ? 'block' : 'none';
    }

    function adicionar(tipo) {
      let valorInput, descInput, meta = null;

      if (tipo === 'guardar') {
        valorInput = document.getElementById('valor-guardar');
        descInput = document.getElementById('desc-guardar');
        meta = document.getElementById('meta-guardar').value;
      } else if (tipo === 'gasto') {
        valorInput = document.getElementById('valor-gasto');
        descInput = document.getElementById('desc-gasto');
      } else {
        valorInput = document.getElementById('valor-invest');
        descInput = document.getElementById('desc-invest');
        meta = document.getElementById('tipo-invest').value;
      }

      const valor = parseFloat(valorInput.value.replace(',', '.'));
      if (isNaN(valor) || valor <= 0) {
        valorInput.focus();
        valorInput.style.borderColor = '#ef4444';
        setTimeout(() => valorInput.style.borderColor = '', 1400);
        return;
      }

      lancamentos.push({
        tipo,
        valor,
        meta,
        descricao: descInput.value.trim(),
        data: new Date().toISOString()
      });

      save();
      render();

      valorInput.value = '';
      descInput.value = '';
      valorInput.focus();
    }

    // ===== Navigation =====
    function goToPage(page) {
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.getElementById('page-' + page).classList.add('active');

      document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
      const navBtn = document.querySelector(`.nav-item[data-page="${page}"]`);
      if (navBtn) navBtn.classList.add('active');

      document.querySelectorAll('.drawer-item').forEach(d => d.classList.remove('active'));
      const drawerBtn = document.querySelector(`.drawer-item[data-page="${page}"]`);
      if (drawerBtn) drawerBtn.classList.add('active');

      if (page === 'metas') {
        renderGerenciarMetas();
      }

      closeMenu();
    }

    function openMenu() {
      document.getElementById('drawer').classList.add('open');
      document.getElementById('overlay').classList.add('open');
    }
    function closeMenu() {
      document.getElementById('drawer').classList.remove('open');
      document.getElementById('overlay').classList.remove('open');
    }

    // ===== Events =====
    document.getElementById('menu-btn').addEventListener('click', openMenu);
    document.getElementById('overlay').addEventListener('click', closeMenu);

    document.querySelectorAll('.nav-item').forEach(btn => {
      btn.addEventListener('click', () => goToPage(btn.dataset.page));
    });
    document.querySelectorAll('.drawer-item').forEach(btn => {
      btn.addEventListener('click', () => goToPage(btn.dataset.page));
    });

    document.getElementById('btn-add-guardar').addEventListener('click', () => adicionar('guardar'));
    document.getElementById('btn-add-gasto').addEventListener('click', () => adicionar('gasto'));
    document.getElementById('btn-add-invest').addEventListener('click', () => adicionar('investimento'));

    // Adicionar nova meta
    document.getElementById('btn-add-meta').addEventListener('click', () => {
      const input = document.getElementById('nova-meta');
      const nome = input.value.trim();
      if (!nome) {
        input.focus();
        return;
      }
      if (metas.some(m => m.toLowerCase() === nome.toLowerCase())) {
        alert('Já existe uma meta com esse nome.');
        return;
      }
      metas.push(nome);
      save();
      input.value = '';
      atualizarSelectMetas();
      atualizarQuickMetas();
      renderGerenciarMetas();
      render();
    });

    document.getElementById('nova-meta').addEventListener('keydown', e => {
      if (e.key === 'Enter') document.getElementById('btn-add-meta').click();
    });

    // Enter key
    ['valor-guardar', 'desc-guardar'].forEach(id => {
      document.getElementById(id).addEventListener('keydown', e => { if (e.key === 'Enter') adicionar('guardar'); });
    });
    ['valor-gasto', 'desc-gasto'].forEach(id => {
      document.getElementById(id).addEventListener('keydown', e => { if (e.key === 'Enter') adicionar('gasto'); });
    });
    ['valor-invest', 'desc-invest'].forEach(id => {
      document.getElementById(id).addEventListener('keydown', e => { if (e.key === 'Enter') adicionar('investimento'); });
    });

    // Delete lançamento
    document.addEventListener('click', e => {
      if (e.target.classList.contains('btn-delete') && e.target.dataset.index !== undefined) {
        const index = parseInt(e.target.dataset.index);
        lancamentos.splice(index, 1);
        save();
        render();
      }
    });

    document.getElementById('btn-clear').addEventListener('click', () => {
      if (confirm('Apagar TODOS os lançamentos? Essa ação não pode ser desfeita.')) {
        lancamentos = [];
        save();
        render();
      }
    });

    // Init
    atualizarSelectMetas();
    atualizarQuickMetas();
    render();
  </script>
</body>
</html>
