# Blindagem PME — Protótipo Navegável

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Construir um protótipo navegável de alta fidelidade em HTML/CSS/JS puro, com 11 telas cobrindo 3 personas (CFO, Corretor, Funcionário), hospedado em GitHub Pages com link compartilhável.

**Architecture:** Único arquivo `index.html` contendo todas as 11 telas como seções `<div id="screen-*">`. A função `navigate(id)` esconde todas as seções e exibe apenas a alvo. Todo CSS fica num bloco `<style>` no `<head>`, sem frameworks ou CDNs externos.

**Tech Stack:** HTML5, CSS3, JavaScript vanilla — zero dependências externas. Deploy via GitHub Pages (branch `main`, root `/`).

**Spec:** `docs/superpowers/specs/2026-03-30-blindagem-pme-prototipo-design.md`

---

## Arquivos

| Arquivo | Responsabilidade |
|---------|-----------------|
| `index.html` | Único arquivo do protótipo — todas as telas, CSS e JS |
| `README.md` | Instruções de deploy e link final |

---

## Task 1: Esqueleto HTML + Sistema de Navegação + CSS Base

**Files:**
- Create: `index.html`

- [ ] **Step 1: Criar o esqueleto do index.html com CSS base e função navigate()**

Crie `/Users/diego_santos/Workspace/FIAP/Desafio Google - Prototipo/index.html` com o conteúdo abaixo. Este é o esqueleto completo — as tarefas seguintes vão preencher cada `<section>`.

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Blindagem PME — Protótipo</title>
  <style>
    /* ── Reset & Base ── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html { scroll-behavior: smooth; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: #f8fafc;
      color: #1e293b;
      min-height: 100vh;
    }

    /* ── Telas ── */
    .screen { display: none; min-height: 100vh; flex-direction: column; }
    .screen.active { display: flex; }

    /* ── Cores e variáveis ── */
    :root {
      --green: #10b981;
      --green-dark: #065f46;
      --green-light: #d1fae5;
      --gold: #fbbf24;
      --gold-dark: #b45309;
      --blue: #2563eb;
      --blue-light: #dbeafe;
      --purple: #7c3aed;
      --purple-light: #ede9fe;
      --dark: #0f172a;
      --slate: #1e293b;
      --muted: #64748b;
      --border: #e2e8f0;
      --white: #ffffff;
    }

    /* ── Topbar ── */
    .topbar {
      background: var(--dark);
      color: var(--white);
      padding: 14px 24px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-shrink: 0;
    }
    .topbar-logo {
      font-size: 18px;
      font-weight: 800;
      color: var(--green);
      letter-spacing: -0.5px;
    }
    .topbar-logo span { color: var(--gold); }
    .topbar-back {
      background: rgba(255,255,255,0.1);
      border: 1px solid rgba(255,255,255,0.2);
      color: var(--white);
      padding: 6px 14px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 13px;
      font-weight: 600;
      transition: background 0.15s;
    }
    .topbar-back:hover { background: rgba(255,255,255,0.2); }

    /* ── Conteúdo principal ── */
    .main { flex: 1; padding: 32px 24px; max-width: 960px; margin: 0 auto; width: 100%; }
    .main-wide { flex: 1; padding: 32px 24px; max-width: 1100px; margin: 0 auto; width: 100%; }

    /* ── Tipografia ── */
    h1 { font-size: 36px; font-weight: 800; line-height: 1.2; }
    h2 { font-size: 26px; font-weight: 700; line-height: 1.3; }
    h3 { font-size: 18px; font-weight: 700; }
    h4 { font-size: 15px; font-weight: 600; }
    p { line-height: 1.6; color: var(--muted); }
    .text-white { color: var(--white) !important; }
    .text-green { color: var(--green); }
    .text-gold { color: var(--gold); }
    .text-muted { color: var(--muted); }

    /* ── Botões ── */
    .btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 12px 24px;
      border-radius: 8px;
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      border: none;
      transition: opacity 0.15s, transform 0.1s;
      text-decoration: none;
    }
    .btn:hover { opacity: 0.88; transform: translateY(-1px); }
    .btn-primary { background: var(--green); color: var(--white); }
    .btn-gold { background: var(--gold); color: var(--dark); }
    .btn-outline { background: transparent; border: 2px solid var(--green); color: var(--green); }
    .btn-dark { background: var(--dark); color: var(--white); }
    .btn-sm { padding: 8px 16px; font-size: 13px; }
    .btn-lg { padding: 16px 32px; font-size: 17px; }

    /* ── Cards ── */
    .card {
      background: var(--white);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 24px;
      box-shadow: 0 1px 4px rgba(0,0,0,0.06);
    }
    .card-grid { display: grid; gap: 16px; }
    .card-grid-2 { grid-template-columns: repeat(2, 1fr); }
    .card-grid-3 { grid-template-columns: repeat(3, 1fr); }
    .card-grid-4 { grid-template-columns: repeat(4, 1fr); }

    /* ── KPI ── */
    .kpi {
      background: var(--white);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 20px;
      text-align: center;
    }
    .kpi-value { font-size: 32px; font-weight: 800; line-height: 1; margin-bottom: 4px; }
    .kpi-label { font-size: 13px; color: var(--muted); font-weight: 500; }

    /* ── Badges ── */
    .badge {
      display: inline-block;
      padding: 3px 10px;
      border-radius: 99px;
      font-size: 12px;
      font-weight: 700;
    }
    .badge-green { background: var(--green-light); color: var(--green-dark); }
    .badge-red { background: #fee2e2; color: #dc2626; }
    .badge-yellow { background: #fef3c7; color: var(--gold-dark); }
    .badge-blue { background: var(--blue-light); color: var(--blue); }
    .badge-purple { background: var(--purple-light); color: var(--purple); }

    /* ── Tabela ── */
    table { width: 100%; border-collapse: collapse; font-size: 14px; }
    th { text-align: left; padding: 10px 12px; font-size: 12px; text-transform: uppercase; letter-spacing: 0.5px; color: var(--muted); border-bottom: 2px solid var(--border); }
    td { padding: 12px; border-bottom: 1px solid var(--border); vertical-align: middle; }
    tr:last-child td { border-bottom: none; }
    tr:hover td { background: #f8fafc; }

    /* ── Alert ── */
    .alert {
      padding: 14px 18px;
      border-radius: 10px;
      font-size: 14px;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .alert-green { background: var(--green-light); color: var(--green-dark); border: 1px solid #a7f3d0; }
    .alert-yellow { background: #fef3c7; color: var(--gold-dark); border: 1px solid #fde68a; }
    .alert-red { background: #fee2e2; color: #dc2626; border: 1px solid #fecaca; }

    /* ── Gráfico de barras simples ── */
    .bar-chart { display: flex; align-items: flex-end; gap: 8px; height: 80px; }
    .bar-col { display: flex; flex-direction: column; align-items: center; gap: 4px; flex: 1; }
    .bar { border-radius: 4px 4px 0 0; width: 100%; transition: height 0.3s; }
    .bar-label { font-size: 10px; color: var(--muted); white-space: nowrap; }
    .bar-value { font-size: 10px; font-weight: 700; }

    /* ── Gráfico de linha simples (SVG) ── */
    .line-chart-wrap { width: 100%; overflow-x: auto; }

    /* ── Divider ── */
    .divider { border: none; border-top: 1px solid var(--border); margin: 24px 0; }

    /* ── Seção com título ── */
    .section-title { font-size: 13px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.8px; color: var(--muted); margin-bottom: 12px; }

    /* ── Gap utilities ── */
    .gap-8 { gap: 8px; }
    .gap-12 { gap: 12px; }
    .gap-16 { gap: 16px; }
    .gap-24 { gap: 24px; }
    .mb-8 { margin-bottom: 8px; }
    .mb-12 { margin-bottom: 12px; }
    .mb-16 { margin-bottom: 16px; }
    .mb-24 { margin-bottom: 24px; }
    .mb-32 { margin-bottom: 32px; }
    .mt-16 { margin-top: 16px; }
    .mt-24 { margin-top: 24px; }
    .mt-32 { margin-top: 32px; }
    .flex { display: flex; }
    .flex-col { flex-direction: column; }
    .items-center { align-items: center; }
    .justify-between { justify-content: space-between; }
    .justify-center { justify-content: center; }
    .flex-wrap { flex-wrap: wrap; }
    .w-full { width: 100%; }
    .text-center { text-align: center; }
    .text-right { text-align: right; }

    /* ── Rodapé ── */
    .footer {
      background: var(--dark);
      color: rgba(255,255,255,0.5);
      padding: 16px 24px;
      font-size: 12px;
      text-align: center;
      line-height: 1.8;
      flex-shrink: 0;
    }
    .footer strong { color: var(--green); }

    /* ── Mobile frame (fluxo funcionário) ── */
    .mobile-frame-wrap {
      flex: 1;
      display: flex;
      align-items: flex-start;
      justify-content: center;
      padding: 24px;
      background: #f1f5f9;
    }
    .mobile-frame {
      width: 390px;
      min-height: 700px;
      background: var(--dark);
      border-radius: 40px;
      overflow: hidden;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
      display: flex;
      flex-direction: column;
    }
    .mobile-status-bar {
      background: var(--dark);
      padding: 14px 24px 8px;
      display: flex;
      justify-content: space-between;
      font-size: 12px;
      color: rgba(255,255,255,0.7);
    }
    .mobile-content { flex: 1; padding: 24px; display: flex; flex-direction: column; }
    .mobile-btn {
      background: var(--green);
      color: var(--white);
      border: none;
      border-radius: 12px;
      padding: 16px;
      font-size: 16px;
      font-weight: 700;
      cursor: pointer;
      width: 100%;
      margin-top: auto;
      transition: opacity 0.15s;
    }
    .mobile-btn:hover { opacity: 0.88; }
    .mobile-input {
      background: rgba(255,255,255,0.08);
      border: 1px solid rgba(255,255,255,0.15);
      border-radius: 10px;
      padding: 14px 16px;
      font-size: 15px;
      color: var(--white);
      width: 100%;
      margin-bottom: 12px;
      outline: none;
    }
    .mobile-input::placeholder { color: rgba(255,255,255,0.4); }

    /* ── Animação check-in ── */
    @keyframes spin { to { transform: rotate(360deg); } }
    @keyframes pop { 0%{transform:scale(0);opacity:0} 70%{transform:scale(1.15)} 100%{transform:scale(1);opacity:1} }
    .spinner {
      width: 64px; height: 64px;
      border: 4px solid rgba(16,185,129,0.2);
      border-top-color: var(--green);
      border-radius: 50%;
      animation: spin 0.9s linear infinite;
    }
    .check-anim {
      width: 80px; height: 80px;
      background: var(--green);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 36px;
      animation: pop 0.4s ease-out forwards;
    }

    /* ── Persona cards no hub ── */
    .persona-card {
      background: var(--white);
      border: 2px solid var(--border);
      border-radius: 16px;
      padding: 28px 24px;
      cursor: pointer;
      transition: border-color 0.2s, transform 0.15s, box-shadow 0.2s;
      text-align: center;
    }
    .persona-card:hover {
      border-color: var(--green);
      transform: translateY(-3px);
      box-shadow: 0 8px 24px rgba(16,185,129,0.15);
    }
    .persona-icon { font-size: 48px; margin-bottom: 12px; }
    .persona-title { font-size: 17px; font-weight: 800; color: var(--slate); margin-bottom: 6px; }
    .persona-desc { font-size: 13px; color: var(--muted); line-height: 1.5; margin-bottom: 16px; }

    /* ── Componente solução ── */
    .solucao-card {
      background: linear-gradient(135deg, var(--green-dark), var(--green));
      color: var(--white);
      border-radius: 12px;
      padding: 20px;
    }
    .solucao-card h4 { color: var(--white); margin-bottom: 6px; font-size: 15px; }
    .solucao-card p { color: rgba(255,255,255,0.8); font-size: 13px; }

    @media (max-width: 640px) {
      .card-grid-2, .card-grid-3, .card-grid-4 { grid-template-columns: 1fr; }
      h1 { font-size: 28px; }
      h2 { font-size: 22px; }
    }
  </style>
</head>
<body>

  <!-- ══════════════════════════════════
       TELA 1 — Landing Page
  ══════════════════════════════════ -->
  <section id="screen-1" class="screen active">
    <!-- PLACEHOLDER: Task 2 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 2 — Hub Central
  ══════════════════════════════════ -->
  <section id="screen-2" class="screen">
    <!-- PLACEHOLDER: Task 3 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 3 — CFO Dashboard
  ══════════════════════════════════ -->
  <section id="screen-3" class="screen">
    <!-- PLACEHOLDER: Task 4 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 4 — CFO Auditorias
  ══════════════════════════════════ -->
  <section id="screen-4" class="screen">
    <!-- PLACEHOLDER: Task 5 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 5 — CFO Relatório
  ══════════════════════════════════ -->
  <section id="screen-5" class="screen">
    <!-- PLACEHOLDER: Task 6 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 6 — Corretor Painel
  ══════════════════════════════════ -->
  <section id="screen-6" class="screen">
    <!-- PLACEHOLDER: Task 7 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 7 — Corretor Relatório
  ══════════════════════════════════ -->
  <section id="screen-7" class="screen">
    <!-- PLACEHOLDER: Task 8 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 8 — Corretor Proposta
  ══════════════════════════════════ -->
  <section id="screen-8" class="screen">
    <!-- PLACEHOLDER: Task 9 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 9 — Funcionário Login
  ══════════════════════════════════ -->
  <section id="screen-9" class="screen">
    <!-- PLACEHOLDER: Task 10 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 10 — Check-in Biométrico
  ══════════════════════════════════ -->
  <section id="screen-10" class="screen">
    <!-- PLACEHOLDER: Task 11 -->
  </section>

  <!-- ══════════════════════════════════
       TELA 11 — Confirmação
  ══════════════════════════════════ -->
  <section id="screen-11" class="screen">
    <!-- PLACEHOLDER: Task 12 -->
  </section>

  <script>
    function navigate(id) {
      document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
      const target = document.getElementById('screen-' + id);
      if (target) {
        target.classList.add('active');
        window.scrollTo({ top: 0, behavior: 'smooth' });
      }
    }

    function simulateCheckin() {
      const btn = document.getElementById('checkin-btn');
      const spinner = document.getElementById('checkin-spinner');
      const status = document.getElementById('checkin-status');
      btn.disabled = true;
      btn.textContent = 'Verificando...';
      spinner.style.display = 'flex';
      status.textContent = 'Verificando identidade e localização...';
      setTimeout(() => { status.textContent = 'Reconhecimento facial confirmado ✓'; }, 1200);
      setTimeout(() => { status.textContent = 'Geolocalização verificada ✓'; }, 2400);
      setTimeout(() => { navigate(11); }, 3200);
    }

    function calcROI() {
      const vidas = parseInt(document.getElementById('slider-vidas').value) || 50;
      const sinistralidade = parseInt(document.getElementById('slider-sinistralidade').value) || 80;
      document.getElementById('vidas-val').textContent = vidas;
      document.getElementById('sinistralidade-val').textContent = sinistralidade + '%';
      const premioMensal = vidas * 680;
      const economiaAnual = Math.round(premioMensal * 12 * ((sinistralidade - 68) / 100) * 0.7);
      const custoAnual = vidas * 120 * 12;
      const roi = economiaAnual > 0 ? Math.round(((economiaAnual - custoAnual) / custoAnual) * 100) : 0;
      document.getElementById('economia-estimada').textContent = 'R$ ' + economiaAnual.toLocaleString('pt-BR');
      document.getElementById('roi-estimado').textContent = roi + '%';
    }
  </script>

</body>
</html>
```

- [ ] **Step 2: Verificar que o arquivo foi criado e abre no browser sem erros**

Abra `index.html` no browser. A tela deve estar em branco (apenas o esqueleto). Abra o console do browser (F12) — não deve haver erros JS.

- [ ] **Step 3: Commit**

```bash
cd "/Users/diego_santos/Workspace/FIAP/Desafio Google - Prototipo"
git init
git add index.html
git commit -m "feat: esqueleto HTML + CSS base + sistema de navegação"
```

---

## Task 2: Tela 1 — Landing Page

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 2 -->` dentro de `#screen-1`

- [ ] **Step 1: Substituir o placeholder da Tela 1 pelo conteúdo completo**

Substitua o comentário `<!-- PLACEHOLDER: Task 2 -->` dentro de `<section id="screen-1">` pelo HTML abaixo:

```html
    <!-- Hero -->
    <div style="background: linear-gradient(160deg, #0f172a 0%, #065f46 100%); flex: 1; display: flex; flex-direction: column;">
      <nav style="padding: 20px 32px; display: flex; align-items: center; justify-content: space-between;">
        <div style="font-size: 22px; font-weight: 900; color: #10b981; letter-spacing: -1px;">
          🛡️ Blindagem <span style="color: #fbbf24;">PME</span>
        </div>
        <div style="font-size: 12px; color: rgba(255,255,255,0.5);">FIAP · Google Challenge 2026</div>
      </nav>

      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 40px 24px; text-align: center; max-width: 760px; margin: 0 auto;">
        <div style="background: rgba(16,185,129,0.15); border: 1px solid rgba(16,185,129,0.3); border-radius: 99px; padding: 6px 18px; font-size: 13px; font-weight: 700; color: #10b981; margin-bottom: 24px; display: inline-block;">
          HealthTech · Anti-Fraude · SaaS B2B
        </div>
        <h1 style="color: white; font-size: 44px; font-weight: 900; line-height: 1.1; margin-bottom: 20px;">
          A blindagem tecnológica que protege o plano de saúde da sua empresa
        </h1>
        <p style="font-size: 18px; color: rgba(255,255,255,0.7); max-width: 560px; line-height: 1.6; margin-bottom: 40px;">
          PMEs perdem até 20% do custo do plano para fraudes invisíveis. O Blindagem PME intercepta automaticamente — sem burocracia, sem atrito.
        </p>

        <!-- KPIs de impacto -->
        <div style="display: grid; grid-template-columns: repeat(3,1fr); gap: 16px; margin-bottom: 48px; width: 100%; max-width: 600px;">
          <div style="background: rgba(255,255,255,0.07); border: 1px solid rgba(255,255,255,0.12); border-radius: 12px; padding: 20px;">
            <div style="font-size: 30px; font-weight: 900; color: #fbbf24; line-height: 1;">R$34bi</div>
            <div style="font-size: 12px; color: rgba(255,255,255,0.55); margin-top: 4px;">perdidos em fraudes/ano</div>
          </div>
          <div style="background: rgba(255,255,255,0.07); border: 1px solid rgba(255,255,255,0.12); border-radius: 12px; padding: 20px;">
            <div style="font-size: 30px; font-weight: 900; color: #fbbf24; line-height: 1;">85%</div>
            <div style="font-size: 12px; color: rgba(255,255,255,0.55); margin-top: 4px;">sinistralidade média das PMEs</div>
          </div>
          <div style="background: rgba(255,255,255,0.07); border: 1px solid rgba(255,255,255,0.12); border-radius: 12px; padding: 20px;">
            <div style="font-size: 30px; font-weight: 900; color: #fbbf24; line-height: 1;">+20%</div>
            <div style="font-size: 12px; color: rgba(255,255,255,0.55); margin-top: 4px;">ajuste abusivo de prêmio</div>
          </div>
        </div>

        <button class="btn btn-gold btn-lg" onclick="navigate(2)">Conheça a solução →</button>
      </div>
    </div>

    <!-- Rodapé -->
    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Abra `index.html`. Deve aparecer a landing page com fundo escuro degradê, os 3 KPIs dourados (R$34bi, 85%, +20%) e o botão "Conheça a solução →". Clicar no botão não faz nada ainda (Tela 2 está vazia).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 1 - landing page com hero e KPIs"
```

---

## Task 3: Tela 2 — Hub Central

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 3 -->` dentro de `#screen-2`

- [ ] **Step 1: Substituir o placeholder da Tela 2**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span></div>
      <button class="topbar-back" onclick="navigate(1)">← Início</button>
    </div>

    <div class="main">
      <div class="mb-32" style="text-align: center;">
        <h2 class="mb-8">Como o Blindagem PME funciona?</h2>
        <p style="max-width: 540px; margin: 0 auto;">Uma camada invisível de proteção que atua no backend — sem burocracia para RH ou funcionários.</p>
      </div>

      <!-- 4 componentes da solução -->
      <div class="card-grid card-grid-4 mb-32" style="gap:12px;">
        <div class="solucao-card">
          <div style="font-size: 28px; margin-bottom: 10px;">🤖</div>
          <h4>Gestor Virtual</h4>
          <p>Monitora o uso do plano automaticamente e bloqueia compartilhamento de credenciais.</p>
        </div>
        <div class="solucao-card">
          <div style="font-size: 28px; margin-bottom: 10px;">📍</div>
          <h4>Biometria & Geo</h4>
          <p>Confirma presença física na clínica. Sem check-in = sem validação de atendimento.</p>
        </div>
        <div class="solucao-card">
          <div style="font-size: 28px; margin-bottom: 10px;">🔍</div>
          <h4>Pré-auditoria</h4>
          <p>Bloqueia reembolsos fraudulentos antes de chegarem à operadora. Dinheiro não sai.</p>
        </div>
        <div class="solucao-card">
          <div style="font-size: 28px; margin-bottom: 10px;">⚙️</div>
          <h4>Integração Sem Atrito</h4>
          <p>Roda 100% no backend. Nenhum esforço adicional para o RH ou gestores.</p>
        </div>
      </div>

      <hr class="divider"/>

      <div class="mb-16" style="text-align: center;">
        <h3 class="mb-8">Escolha uma perspectiva para explorar</h3>
        <p>Cada perfil vive uma experiência diferente com o Blindagem PME</p>
      </div>

      <!-- 3 personas -->
      <div class="card-grid card-grid-3" style="gap: 20px;">
        <div class="persona-card" onclick="navigate(3)">
          <div class="persona-icon">👔</div>
          <div class="persona-title">CFO / Sócio de PME</div>
          <div class="persona-desc">Acompanha o dashboard de sinistralidade, vê fraudes bloqueadas e comprova a economia gerada no plano de saúde.</div>
          <button class="btn btn-primary w-full" style="justify-content: center;">Explorar como CFO →</button>
        </div>
        <div class="persona-card" onclick="navigate(6)">
          <div class="persona-icon">🤝</div>
          <div class="persona-title">Corretor de Saúde</div>
          <div class="persona-desc">Gerencia sua carteira de clientes PME, gera relatórios de resultado e cria propostas comerciais com ROI simulado.</div>
          <button class="btn btn-primary w-full" style="justify-content: center;">Explorar como Corretor →</button>
        </div>
        <div class="persona-card" onclick="navigate(9)">
          <div class="persona-icon">👤</div>
          <div class="persona-title">Funcionário / Beneficiário</div>
          <div class="persona-desc">Usa o app mobile para validar presença na clínica com biometria e geolocalização. Experiência simples e transparente.</div>
          <button class="btn btn-primary w-full" style="justify-content: center;">Explorar como Funcionário →</button>
        </div>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Clique em "Conheça a solução →" na landing page. Deve aparecer o Hub com os 4 cards verdes e os 3 cards de persona. Hover nos cards de persona deve mostrar borda verde. Os botões "Explorar como..." não funcionam ainda.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 2 - hub central com componentes e personas"
```

---

## Task 4: Tela 3 — CFO Dashboard Principal

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 4 -->` dentro de `#screen-3`

- [ ] **Step 1: Substituir o placeholder da Tela 3**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span> <span style="font-size:13px; color: rgba(255,255,255,0.5); font-weight:400; margin-left:8px;">| Visão CFO</span></div>
      <button class="topbar-back" onclick="navigate(2)">← Hub</button>
    </div>

    <div class="main">
      <div class="flex items-center justify-between mb-24">
        <div>
          <h2 class="mb-8">Dashboard de Saúde do Plano</h2>
          <p>Empresa: <strong>TechSol Soluções Ltda</strong> · 48 vidas · Plano Amil PME Plus</p>
        </div>
        <div class="badge badge-green" style="font-size: 13px; padding: 6px 14px;">● Plano Protegido</div>
      </div>

      <!-- KPIs -->
      <div class="card-grid card-grid-4 mb-24">
        <div class="kpi">
          <div class="kpi-value text-green">68%</div>
          <div class="kpi-label">Sinistralidade atual</div>
          <div style="margin-top: 8px;"><span class="badge badge-green">↓ Abaixo da meta (70%)</span></div>
        </div>
        <div class="kpi">
          <div class="kpi-value" style="color: #fbbf24;">R$12.400</div>
          <div class="kpi-label">Economia este mês</div>
          <div style="margin-top: 8px;"><span class="badge badge-yellow">vs. mês anterior</span></div>
        </div>
        <div class="kpi">
          <div class="kpi-value" style="color: #2563eb;">7</div>
          <div class="kpi-label">Fraudes bloqueadas</div>
          <div style="margin-top: 8px;"><span class="badge badge-blue">no mês atual</span></div>
        </div>
        <div class="kpi">
          <div class="kpi-value text-green">R$37.200</div>
          <div class="kpi-label">Economia acumulada</div>
          <div style="margin-top: 8px;"><span class="badge badge-green">últimos 3 meses</span></div>
        </div>
      </div>

      <!-- Alerta e gráfico -->
      <div class="card-grid card-grid-2 mb-24" style="gap: 20px;">
        <div class="card">
          <div class="section-title">Sinistralidade — Últimos 6 Meses</div>
          <div style="display: flex; align-items: flex-end; gap: 10px; height: 120px; margin-bottom: 8px;">
            <div style="display:flex;flex-direction:column;align-items:center;gap:4px;flex:1;">
              <span style="font-size:10px;font-weight:700;color:#ef4444;">84%</span>
              <div style="height:100px;background:#fee2e2;border-radius:4px 4px 0 0;width:100%;"></div>
              <span style="font-size:10px;color:#94a3b8;">Out</span>
            </div>
            <div style="display:flex;flex-direction:column;align-items:center;gap:4px;flex:1;">
              <span style="font-size:10px;font-weight:700;color:#ef4444;">82%</span>
              <div style="height:97px;background:#fecaca;border-radius:4px 4px 0 0;width:100%;"></div>
              <span style="font-size:10px;color:#94a3b8;">Nov</span>
            </div>
            <div style="display:flex;flex-direction:column;align-items:center;gap:4px;flex:1;">
              <span style="font-size:10px;font-weight:700;color:#b45309;">79%</span>
              <div style="height:94px;background:#fde68a;border-radius:4px 4px 0 0;width:100%;"></div>
              <span style="font-size:10px;color:#94a3b8;">Dez</span>
            </div>
            <div style="display:flex;flex-direction:column;align-items:center;gap:4px;flex:1;">
              <span style="font-size:10px;font-weight:700;color:#b45309;">75%</span>
              <div style="height:89px;background:#fef08a;border-radius:4px 4px 0 0;width:100%;"></div>
              <span style="font-size:10px;color:#94a3b8;">Jan</span>
            </div>
            <div style="display:flex;flex-direction:column;align-items:center;gap:4px;flex:1;">
              <span style="font-size:10px;font-weight:700;color:#065f46;">71%</span>
              <div style="height:84px;background:#a7f3d0;border-radius:4px 4px 0 0;width:100%;"></div>
              <span style="font-size:10px;color:#94a3b8;">Fev</span>
            </div>
            <div style="display:flex;flex-direction:column;align-items:center;gap:4px;flex:1;">
              <span style="font-size:10px;font-weight:700;color:#065f46;">68%</span>
              <div style="height:80px;background:#10b981;border-radius:4px 4px 0 0;width:100%;"></div>
              <span style="font-size:10px;color:#94a3b8;">Mar</span>
            </div>
          </div>
          <div style="border-top: 1px dashed #e2e8f0; padding-top: 8px; display:flex; justify-content: space-between;">
            <span style="font-size:11px;color:#94a3b8;">Meta: 70%</span>
            <span style="font-size:11px;color:#10b981;font-weight:700;">▼ -16% vs. início</span>
          </div>
        </div>

        <div class="card" style="display:flex;flex-direction:column;gap:16px;">
          <div class="alert alert-green">✅ Plano protegido. Sem risco de ajuste de prêmio para o próximo trimestre.</div>
          <div class="alert alert-yellow">⚠️ 2 solicitações de reembolso em análise — pré-auditoria ativa.</div>
          <div style="margin-top:auto;">
            <div class="section-title">Última atividade</div>
            <div style="font-size:13px;color:#64748b;line-height:2;">
              <div>🔍 Reembolso R$380 bloqueado — hoje 10:14</div>
              <div>✅ Check-in validado: Ana Silva — hoje 09:52</div>
              <div>🔍 Reembolso R$1.200 bloqueado — ontem 15:30</div>
            </div>
          </div>
        </div>
      </div>

      <div class="flex justify-between items-center">
        <button class="btn btn-outline" onclick="navigate(2)">← Hub Central</button>
        <button class="btn btn-primary" onclick="navigate(4)">Ver auditorias bloqueadas →</button>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Acesse o Hub → clique em "Explorar como CFO →". O dashboard deve mostrar os 4 KPIs, o gráfico de barras com evolução da sinistralidade e os alertas.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 3 - CFO dashboard com KPIs e gráfico de sinistralidade"
```

---

## Task 5: Tela 4 — CFO Auditorias Bloqueadas

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 5 -->` dentro de `#screen-4`

- [ ] **Step 1: Substituir o placeholder da Tela 4**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span> <span style="font-size:13px; color: rgba(255,255,255,0.5); font-weight:400; margin-left:8px;">| Auditorias</span></div>
      <button class="topbar-back" onclick="navigate(2)">← Hub</button>
    </div>

    <div class="main">
      <div class="flex items-center justify-between mb-24">
        <div>
          <h2 class="mb-8">Solicitações Interceptadas</h2>
          <p>Reembolsos e atendimentos bloqueados pela pré-auditoria automática · Março 2026</p>
        </div>
        <div style="text-align: right;">
          <div style="font-size: 28px; font-weight: 900; color: #10b981;">R$8.200</div>
          <div style="font-size: 12px; color: #64748b;">total bloqueado no mês</div>
        </div>
      </div>

      <div class="card mb-24">
        <table>
          <thead>
            <tr>
              <th>Tipo</th>
              <th>Beneficiário</th>
              <th>Valor</th>
              <th>Motivo</th>
              <th>Status</th>
              <th>Data</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><span style="font-size:13px;">🔍 Reembolso</span></td>
              <td>Carlos Mendes</td>
              <td style="font-weight:700;">R$ 1.200</td>
              <td><span style="font-size:12px;color:#64748b;">Nota fiscal sem CNPJ válido</span></td>
              <td><span class="badge badge-red">Bloqueado</span></td>
              <td style="font-size:12px;color:#94a3b8;">28/03/2026</td>
            </tr>
            <tr>
              <td><span style="font-size:13px;">📍 Check-in</span></td>
              <td>Marcos Lima</td>
              <td style="font-weight:700;">R$ 380</td>
              <td><span style="font-size:12px;color:#64748b;">Geolocalização divergente</span></td>
              <td><span class="badge badge-red">Bloqueado</span></td>
              <td style="font-size:12px;color:#94a3b8;">27/03/2026</td>
            </tr>
            <tr>
              <td><span style="font-size:13px;">🔍 Reembolso</span></td>
              <td>Fernanda Costa</td>
              <td style="font-weight:700;">R$ 2.400</td>
              <td><span style="font-size:12px;color:#64748b;">Procedimento duplicado (30 dias)</span></td>
              <td><span class="badge badge-red">Bloqueado</span></td>
              <td style="font-size:12px;color:#94a3b8;">25/03/2026</td>
            </tr>
            <tr>
              <td><span style="font-size:13px;">👤 Credencial</span></td>
              <td>João Pereira</td>
              <td style="font-weight:700;">R$ 1.820</td>
              <td><span style="font-size:12px;color:#64748b;">Biometria não correspondeu</span></td>
              <td><span class="badge badge-red">Bloqueado</span></td>
              <td style="font-size:12px;color:#94a3b8;">22/03/2026</td>
            </tr>
            <tr>
              <td><span style="font-size:13px;">🔍 Reembolso</span></td>
              <td>Ana Silva</td>
              <td style="font-weight:700;">R$ 2.400</td>
              <td><span style="font-size:12px;color:#64748b;">Em análise — suspeita de superfaturamento</span></td>
              <td><span class="badge badge-yellow">Em análise</span></td>
              <td style="font-size:12px;color:#94a3b8;">20/03/2026</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="card mb-24" style="background: #f0fdf4; border-color: #a7f3d0;">
        <div class="flex items-center gap-12" style="gap:16px;">
          <div style="font-size:36px;">💡</div>
          <div>
            <div style="font-weight:700; color:#065f46; margin-bottom:4px;">Como funciona a pré-auditoria?</div>
            <p style="font-size:13px; color:#065f46; margin:0;">O Blindagem PME analisa cada solicitação antes de enviá-la à operadora. Documentos inválidos, geolocalização divergente e padrões suspeitos são bloqueados automaticamente — sem intervenção humana.</p>
          </div>
        </div>
      </div>

      <div class="flex justify-between items-center">
        <button class="btn btn-outline" onclick="navigate(3)">← Dashboard</button>
        <button class="btn btn-primary" onclick="navigate(5)">Ver relatório mensal →</button>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

No dashboard CFO, clique em "Ver auditorias bloqueadas →". Deve aparecer a tabela com 5 linhas, badges coloridos e o total de R$8.200 no canto.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 4 - CFO auditorias bloqueadas com tabela"
```

---

## Task 6: Tela 5 — CFO Relatório Mensal

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 6 -->` dentro de `#screen-5`

- [ ] **Step 1: Substituir o placeholder da Tela 5**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span> <span style="font-size:13px; color: rgba(255,255,255,0.5); font-weight:400; margin-left:8px;">| Relatório Mensal</span></div>
      <button class="topbar-back" onclick="navigate(2)">← Hub</button>
    </div>

    <div class="main">
      <div class="flex items-center justify-between mb-24">
        <div>
          <h2 class="mb-8">Relatório Março 2026</h2>
          <p>TechSol Soluções Ltda · Gerado automaticamente pelo Blindagem PME</p>
        </div>
        <button class="btn btn-outline btn-sm" style="opacity:0.6;cursor:default;" title="Funcionalidade simulada">⬇ Baixar PDF</button>
      </div>

      <!-- Comparativo antes/depois -->
      <div class="card mb-24">
        <div class="section-title mb-16">Comparativo: Antes vs. Depois do Blindagem PME</div>
        <div class="card-grid card-grid-2" style="gap: 16px;">
          <div style="background: #fff7ed; border: 1px solid #fed7aa; border-radius: 12px; padding: 20px;">
            <div style="font-size: 12px; font-weight: 700; color: #c2410c; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 16px;">Antes (Outubro 2025)</div>
            <div class="flex justify-between mb-8">
              <span style="font-size: 14px; color:#64748b;">Sinistralidade</span>
              <span style="font-size: 16px; font-weight: 800; color: #ef4444;">84%</span>
            </div>
            <div class="flex justify-between mb-8">
              <span style="font-size: 14px; color:#64748b;">Prêmio mensal</span>
              <span style="font-size: 16px; font-weight: 800; color: #ef4444;">R$ 38.000</span>
            </div>
            <div class="flex justify-between mb-8">
              <span style="font-size: 14px; color:#64748b;">Fraudes bloqueadas</span>
              <span style="font-size: 16px; font-weight: 800; color: #94a3b8;">0</span>
            </div>
            <div class="flex justify-between">
              <span style="font-size: 14px; color:#64748b;">Risco de ajuste</span>
              <span style="font-size: 16px; font-weight: 800; color: #ef4444;">+20%</span>
            </div>
          </div>
          <div style="background: #f0fdf4; border: 1px solid #a7f3d0; border-radius: 12px; padding: 20px;">
            <div style="font-size: 12px; font-weight: 700; color: #065f46; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 16px;">Depois (Março 2026)</div>
            <div class="flex justify-between mb-8">
              <span style="font-size: 14px; color:#64748b;">Sinistralidade</span>
              <span style="font-size: 16px; font-weight: 800; color: #10b981;">68% ↓</span>
            </div>
            <div class="flex justify-between mb-8">
              <span style="font-size: 14px; color:#64748b;">Prêmio mensal</span>
              <span style="font-size: 16px; font-weight: 800; color: #10b981;">R$ 33.500 ↓</span>
            </div>
            <div class="flex justify-between mb-8">
              <span style="font-size: 14px; color:#64748b;">Fraudes bloqueadas</span>
              <span style="font-size: 16px; font-weight: 800; color: #10b981;">7 ✓</span>
            </div>
            <div class="flex justify-between">
              <span style="font-size: 14px; color:#64748b;">Risco de ajuste</span>
              <span style="font-size: 16px; font-weight: 800; color: #10b981;">Nenhum ✓</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Economia acumulada -->
      <div class="card mb-24" style="background: linear-gradient(135deg, #065f46, #10b981); color: white; border: none;">
        <div class="flex items-center justify-between flex-wrap" style="gap: 16px;">
          <div>
            <div style="font-size: 13px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.5px; color: rgba(255,255,255,0.7); margin-bottom: 6px;">Economia Acumulada · Últimos 3 meses</div>
            <div style="font-size: 48px; font-weight: 900; color: #fbbf24; line-height: 1;">R$ 37.200</div>
          </div>
          <div style="text-align: right;">
            <div style="font-size: 13px; color: rgba(255,255,255,0.7);">ROI do investimento</div>
            <div style="font-size: 36px; font-weight: 900; color: white;">312%</div>
          </div>
        </div>
      </div>

      <div class="alert alert-green mb-24">🏆 Parabéns! TechSol Soluções Ltda está entre os 15% de PMEs com menor sinistralidade da operadora em março/2026.</div>

      <div class="flex justify-between items-center">
        <button class="btn btn-outline" onclick="navigate(4)">← Auditorias</button>
        <button class="btn btn-dark" onclick="navigate(2)">← Hub Central</button>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Navegue CFO → Dashboard → Auditorias → Relatório. O comparativo antes/depois deve mostrar o fundo laranja-avermelhado à esquerda e o fundo verde à direita. O card de economia acumulada deve ter fundo degradê verde com R$37.200 em dourado.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 5 - CFO relatório mensal com comparativo antes/depois"
```

---

## Task 7: Tela 6 — Corretor Painel

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 7 -->` dentro de `#screen-6`

- [ ] **Step 1: Substituir o placeholder da Tela 6**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span> <span style="font-size:13px; color: rgba(255,255,255,0.5); font-weight:400; margin-left:8px;">| Painel Corretor</span></div>
      <button class="topbar-back" onclick="navigate(2)">← Hub</button>
    </div>

    <div class="main-wide">
      <div class="flex items-center justify-between mb-24">
        <div>
          <h2 class="mb-8">Minha Carteira de Clientes</h2>
          <p>Olá, <strong>Roberto Alves</strong> · Corretor parceiro Blindagem PME · 4 clientes ativos</p>
        </div>
        <div class="badge badge-purple" style="font-size: 13px; padding: 6px 14px;">Corretor Parceiro ✓</div>
      </div>

      <!-- KPIs do corretor -->
      <div class="card-grid card-grid-4 mb-24">
        <div class="kpi">
          <div class="kpi-value" style="color: #7c3aed;">4</div>
          <div class="kpi-label">Clientes ativos</div>
        </div>
        <div class="kpi">
          <div class="kpi-value text-green">3</div>
          <div class="kpi-label">Planos protegidos</div>
        </div>
        <div class="kpi">
          <div class="kpi-value" style="color: #ef4444;">1</div>
          <div class="kpi-label">Em risco</div>
        </div>
        <div class="kpi">
          <div class="kpi-value" style="color: #fbbf24;">R$78k</div>
          <div class="kpi-label">Economia gerada (trimestre)</div>
        </div>
      </div>

      <!-- Tabela de clientes -->
      <div class="card mb-24">
        <table>
          <thead>
            <tr>
              <th>Empresa</th>
              <th>Vidas</th>
              <th>Sinistralidade</th>
              <th>Fraudes bloq.</th>
              <th>Economia/mês</th>
              <th>Status</th>
              <th>Ação</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><strong>TechSol Soluções Ltda</strong></td>
              <td>48</td>
              <td style="color:#10b981;font-weight:700;">68%</td>
              <td>7</td>
              <td style="font-weight:700;">R$12.400</td>
              <td><span class="badge badge-green">Protegido ✓</span></td>
              <td><button class="btn btn-sm btn-primary" onclick="navigate(7)">Ver relatório</button></td>
            </tr>
            <tr>
              <td><strong>Alfa Comércio S.A.</strong></td>
              <td>62</td>
              <td style="color:#10b981;font-weight:700;">71%</td>
              <td>12</td>
              <td style="font-weight:700;">R$18.600</td>
              <td><span class="badge badge-green">Protegido ✓</span></td>
              <td><button class="btn btn-sm btn-outline" onclick="navigate(7)">Ver relatório</button></td>
            </tr>
            <tr>
              <td><strong>Beta Serviços ME</strong></td>
              <td>31</td>
              <td style="color:#10b981;font-weight:700;">69%</td>
              <td>4</td>
              <td style="font-weight:700;">R$7.200</td>
              <td><span class="badge badge-green">Protegido ✓</span></td>
              <td><button class="btn btn-sm btn-outline" onclick="navigate(7)">Ver relatório</button></td>
            </tr>
            <tr>
              <td><strong>Gama Indústria Ltda</strong></td>
              <td>55</td>
              <td style="color:#ef4444;font-weight:700;">83%</td>
              <td>0</td>
              <td style="font-weight:700;color:#94a3b8;">—</td>
              <td><span class="badge badge-red">Em risco ⚠</span></td>
              <td><button class="btn btn-sm btn-gold" onclick="navigate(8)">Gerar proposta</button></td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="alert alert-yellow mb-24">⚠ <strong>Gama Indústria Ltda</strong> está com sinistralidade de 83% e risco de reajuste de +20% em 60 dias. Oportunidade de converter para Blindagem PME.</div>

      <div class="flex justify-between items-center">
        <button class="btn btn-outline" onclick="navigate(2)">← Hub Central</button>
        <button class="btn btn-primary" onclick="navigate(8)">Criar proposta para novo cliente →</button>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Hub → "Explorar como Corretor →". Deve aparecer a tabela com 4 clientes, badges coloridos e o alerta amarelo sobre a Gama Indústria.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 6 - corretor painel com carteira de clientes"
```

---

## Task 8: Tela 7 — Corretor Relatório para Cliente

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 8 -->` dentro de `#screen-7`

- [ ] **Step 1: Substituir o placeholder da Tela 7**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span> <span style="font-size:13px; color: rgba(255,255,255,0.5); font-weight:400; margin-left:8px;">| Relatório do Cliente</span></div>
      <button class="topbar-back" onclick="navigate(6)">← Carteira</button>
    </div>

    <div class="main">
      <div class="flex items-center justify-between mb-24">
        <div>
          <h2 class="mb-8">Relatório de Resultados</h2>
          <p><strong>TechSol Soluções Ltda</strong> · 48 vidas · Preparado por Roberto Alves · Março 2026</p>
        </div>
        <button class="btn btn-outline btn-sm" style="opacity:0.6;cursor:default;">⬇ Exportar PDF</button>
      </div>

      <!-- Resultados do trimestre -->
      <div class="card-grid card-grid-3 mb-24">
        <div class="kpi">
          <div class="kpi-value text-green">-16pp</div>
          <div class="kpi-label">Redução na sinistralidade</div>
          <div style="margin-top:8px;font-size:12px;color:#64748b;">84% → 68%</div>
        </div>
        <div class="kpi">
          <div class="kpi-value" style="color:#fbbf24;">R$37.200</div>
          <div class="kpi-label">Economia no trimestre</div>
          <div style="margin-top:8px;"><span class="badge badge-green">ROI de 312%</span></div>
        </div>
        <div class="kpi">
          <div class="kpi-value" style="color:#2563eb;">23</div>
          <div class="kpi-label">Fraudes interceptadas</div>
          <div style="margin-top:8px;font-size:12px;color:#64748b;">no período</div>
        </div>
      </div>

      <!-- Gráfico linha (SVG simples) -->
      <div class="card mb-24">
        <div class="section-title mb-16">Evolução da Sinistralidade — Últimos 6 meses</div>
        <svg viewBox="0 0 600 120" xmlns="http://www.w3.org/2000/svg" style="width:100%;height:120px;">
          <!-- Grid lines -->
          <line x1="0" y1="20" x2="600" y2="20" stroke="#f1f5f9" stroke-width="1"/>
          <line x1="0" y1="50" x2="600" y2="50" stroke="#f1f5f9" stroke-width="1"/>
          <line x1="0" y1="80" x2="600" y2="80" stroke="#f1f5f9" stroke-width="1"/>
          <!-- Linha de meta -->
          <line x1="0" y1="65" x2="600" y2="65" stroke="#10b981" stroke-width="1" stroke-dasharray="6,4"/>
          <text x="4" y="62" font-size="10" fill="#10b981">Meta 70%</text>
          <!-- Linha de dados: Out=84→20, Nov=82→28, Dez=79→37, Jan=75→50, Fev=71→62, Mar=68→71 -->
          <polyline points="50,20 150,28 250,37 350,50 450,62 550,71" fill="none" stroke="#ef4444" stroke-width="2.5" stroke-linejoin="round"/>
          <!-- Pontos -->
          <circle cx="50" cy="20" r="5" fill="#ef4444"/>
          <circle cx="150" cy="28" r="5" fill="#ef4444"/>
          <circle cx="250" cy="37" r="5" fill="#fbbf24"/>
          <circle cx="350" cy="50" r="5" fill="#fbbf24"/>
          <circle cx="450" cy="62" r="5" fill="#10b981"/>
          <circle cx="550" cy="71" r="5" fill="#10b981"/>
          <!-- Labels eixo X -->
          <text x="38" y="110" font-size="10" fill="#94a3b8">Out</text>
          <text x="138" y="110" font-size="10" fill="#94a3b8">Nov</text>
          <text x="238" y="110" font-size="10" fill="#94a3b8">Dez</text>
          <text x="338" y="110" font-size="10" fill="#94a3b8">Jan</text>
          <text x="438" y="110" font-size="10" fill="#94a3b8">Fev</text>
          <text x="535" y="110" font-size="10" fill="#94a3b8">Mar</text>
          <!-- Labels valores -->
          <text x="38" y="16" font-size="10" fill="#ef4444" font-weight="bold">84%</text>
          <text x="138" y="24" font-size="10" fill="#ef4444" font-weight="bold">82%</text>
          <text x="238" y="33" font-size="10" fill="#b45309" font-weight="bold">79%</text>
          <text x="338" y="46" font-size="10" fill="#b45309" font-weight="bold">75%</text>
          <text x="438" y="58" font-size="10" fill="#10b981" font-weight="bold">71%</text>
          <text x="532" y="67" font-size="10" fill="#10b981" font-weight="bold">68%</text>
        </svg>
      </div>

      <div class="alert alert-green mb-24">✅ Este relatório pode ser compartilhado diretamente com o CFO da TechSol para comprovar o valor gerado pelo Blindagem PME no trimestre.</div>

      <div class="flex justify-between items-center">
        <button class="btn btn-outline" onclick="navigate(6)">← Carteira</button>
        <button class="btn btn-primary" onclick="navigate(8)">Criar proposta para novo cliente →</button>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Corretor → "Ver relatório" da TechSol. Deve aparecer o gráfico de linha SVG com a curva declinante da sinistralidade e os 3 KPIs do trimestre.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 7 - corretor relatório do cliente com gráfico SVG"
```

---

## Task 9: Tela 8 — Corretor Simulador de Proposta

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 9 -->` dentro de `#screen-8`

- [ ] **Step 1: Substituir o placeholder da Tela 8**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span> <span style="font-size:13px; color: rgba(255,255,255,0.5); font-weight:400; margin-left:8px;">| Simulador de Proposta</span></div>
      <button class="topbar-back" onclick="navigate(6)">← Carteira</button>
    </div>

    <div class="main">
      <div class="mb-24">
        <h2 class="mb-8">Simulador de ROI</h2>
        <p>Mostre ao prospect quanto ele pode economizar com o Blindagem PME</p>
      </div>

      <div class="card-grid card-grid-2 mb-24" style="gap: 24px;">
        <!-- Inputs -->
        <div class="card">
          <h3 class="mb-16">Dados do Prospect</h3>

          <div class="mb-16">
            <label style="display:block; font-size:13px; font-weight:600; color:#475569; margin-bottom:8px;">
              Número de Vidas: <span id="vidas-val" style="color:#10b981;">50</span>
            </label>
            <input id="slider-vidas" type="range" min="30" max="99" value="50" oninput="calcROI()"
              style="width:100%; accent-color: #10b981; cursor:pointer; height:6px;">
            <div class="flex justify-between" style="font-size:11px;color:#94a3b8;margin-top:4px;">
              <span>30 vidas</span><span>99 vidas</span>
            </div>
          </div>

          <div class="mb-16">
            <label style="display:block; font-size:13px; font-weight:600; color:#475569; margin-bottom:8px;">
              Sinistralidade Atual: <span id="sinistralidade-val" style="color:#ef4444;">80%</span>
            </label>
            <input id="slider-sinistralidade" type="range" min="70" max="95" value="80" oninput="calcROI()"
              style="width:100%; accent-color: #ef4444; cursor:pointer; height:6px;">
            <div class="flex justify-between" style="font-size:11px;color:#94a3b8;margin-top:4px;">
              <span>70%</span><span>95%</span>
            </div>
          </div>

          <div style="background:#f8fafc;border-radius:8px;padding:16px;font-size:13px;color:#64748b;line-height:1.8;">
            <div>📋 Prêmio estimado: <strong style="color:#1e293b;">R$ <span id="premio-val">34.000</span>/mês</strong></div>
            <div>🎯 Meta de sinistralidade: <strong style="color:#10b981;">68%</strong></div>
          </div>
        </div>

        <!-- Resultado -->
        <div class="card" style="background: linear-gradient(160deg, #0f172a, #065f46); border: none; color: white;">
          <div style="font-size:13px;font-weight:700;text-transform:uppercase;letter-spacing:0.5px;color:rgba(255,255,255,0.6);margin-bottom:20px;">Resultado Estimado</div>

          <div class="mb-16">
            <div style="font-size:13px;color:rgba(255,255,255,0.6);">Economia anual estimada</div>
            <div id="economia-estimada" style="font-size:40px;font-weight:900;color:#fbbf24;line-height:1.1;">R$ 48.000</div>
          </div>

          <div class="mb-24">
            <div style="font-size:13px;color:rgba(255,255,255,0.6);">ROI no primeiro ano</div>
            <div id="roi-estimado" style="font-size:40px;font-weight:900;color:#10b981;line-height:1.1;">280%</div>
          </div>

          <button class="btn btn-gold w-full" style="justify-content:center;font-size:16px;" onclick="alert('Proposta enviada! (simulação)')">
            📧 Enviar Proposta ao Prospect
          </button>
        </div>
      </div>

      <div class="alert alert-green mb-24">💡 Dica: Clientes com sinistralidade acima de 78% têm maior urgência de solução. Foque a abordagem no risco de reajuste de prêmio.</div>

      <div class="flex justify-between items-center">
        <button class="btn btn-outline" onclick="navigate(6)">← Carteira</button>
        <button class="btn btn-dark" onclick="navigate(2)">← Hub Central</button>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Acesse a tela 8. Mova os sliders — os valores de "Economia anual estimada" e "ROI" devem atualizar em tempo real (função `calcROI()` já está no script do esqueleto). O botão "Enviar Proposta" deve mostrar um `alert()`.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 8 - simulador de proposta com sliders interativos"
```

---

## Task 10: Tela 9 — Funcionário App Login

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 10 -->` dentro de `#screen-9`

- [ ] **Step 1: Substituir o placeholder da Tela 9**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span></div>
      <button class="topbar-back" onclick="navigate(2)">← Hub</button>
    </div>

    <div class="mobile-frame-wrap">
      <div class="mobile-frame">
        <div class="mobile-status-bar">
          <span>9:41</span>
          <span>📶 ●●●●</span>
        </div>
        <div class="mobile-content" style="background: #0f172a;">
          <!-- Logo -->
          <div style="text-align:center;margin-bottom:32px;margin-top:16px;">
            <div style="font-size:48px;margin-bottom:8px;">🛡️</div>
            <div style="font-size:22px;font-weight:900;color:#10b981;">Blindagem PME</div>
            <div style="font-size:13px;color:rgba(255,255,255,0.5);margin-top:4px;">Seu plano protegido</div>
          </div>

          <!-- Formulário -->
          <div style="margin-bottom:8px;font-size:13px;font-weight:600;color:rgba(255,255,255,0.7);">Nome do funcionário</div>
          <input class="mobile-input" id="nome-funcionario" type="text" placeholder="Ex: Ana Silva" value="Ana Silva"/>

          <div style="margin-bottom:8px;margin-top:12px;font-size:13px;font-weight:600;color:rgba(255,255,255,0.7);">Empresa</div>
          <input class="mobile-input" type="text" placeholder="Sua empresa" value="TechSol Soluções Ltda" readonly style="opacity:0.7;cursor:default;"/>

          <div style="margin-top:16px;background:rgba(16,185,129,0.1);border:1px solid rgba(16,185,129,0.3);border-radius:10px;padding:12px;font-size:12px;color:rgba(255,255,255,0.6);text-align:center;">
            Seu plano de saúde está protegido pelo Blindagem PME
          </div>

          <button class="mobile-btn" onclick="navigate(10)" style="margin-top:24px;">
            Entrar →
          </button>
        </div>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Hub → "Explorar como Funcionário →". O mockup de smartphone deve aparecer centralizado com fundo cinza, mostrando a tela de login escura dentro do frame. O botão "Entrar →" deve navegar para a tela 10.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 9 - funcionário login com mobile frame"
```

---

## Task 11: Tela 10 — Check-in Biométrico

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 11 -->` dentro de `#screen-10`

- [ ] **Step 1: Substituir o placeholder da Tela 10**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span></div>
      <button class="topbar-back" onclick="navigate(9)">← Voltar</button>
    </div>

    <div class="mobile-frame-wrap">
      <div class="mobile-frame">
        <div class="mobile-status-bar">
          <span>9:52</span>
          <span>📶 ●●●●</span>
        </div>
        <div class="mobile-content" style="background: #0f172a; align-items: center; text-align: center;">
          <div style="font-size:14px;font-weight:700;color:#10b981;margin-bottom:4px;margin-top:8px;">CHECK-IN NA CLÍNICA</div>
          <div style="font-size:13px;color:rgba(255,255,255,0.6);margin-bottom:32px;">📍 Clínica São Lucas · Av. Paulista, 1000</div>

          <!-- Spinner / ícone check-in -->
          <div id="checkin-spinner" style="display:none;align-items:center;justify-content:center;width:80px;height:80px;margin:0 auto 24px;">
            <div class="spinner"></div>
          </div>
          <div id="checkin-icon" style="font-size:72px;margin-bottom:24px;">🤳</div>

          <div style="font-size:16px;font-weight:700;color:white;margin-bottom:8px;">Confirmar presença</div>
          <div style="font-size:13px;color:rgba(255,255,255,0.5);margin-bottom:32px;line-height:1.5;">
            Olhe para a câmera para confirmar<br/>sua identidade na clínica
          </div>

          <div id="checkin-status" style="font-size:13px;color:rgba(255,255,255,0.5);margin-bottom:24px;min-height:20px;"></div>

          <div style="background:rgba(255,255,255,0.05);border-radius:10px;padding:12px;font-size:12px;color:rgba(255,255,255,0.5);margin-bottom:24px;">
            <div>👨‍⚕️ Dr. Carlos Mendes — Clínico Geral</div>
            <div>📅 Hoje · 14h00</div>
          </div>

          <button id="checkin-btn" class="mobile-btn" onclick="simulateCheckin()">
            🔒 Iniciar verificação biométrica
          </button>
        </div>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar no browser**

Login do funcionário → "Entrar →" → tela de check-in. Clique em "Iniciar verificação biométrica". O spinner deve aparecer e o texto de status deve mudar em sequência (1.2s → "Reconhecimento facial ✓", 2.4s → "Geolocalização ✓"). Após 3.2s deve navegar para a tela 11.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 10 - check-in biométrico com animação simulada"
```

---

## Task 12: Tela 11 — Confirmação de Atendimento

**Files:**
- Modify: `index.html` — substituir `<!-- PLACEHOLDER: Task 12 -->` dentro de `#screen-11`

- [ ] **Step 1: Substituir o placeholder da Tela 11**

```html
    <div class="topbar">
      <div class="topbar-logo">🛡️ Blindagem <span>PME</span></div>
      <button class="topbar-back" onclick="navigate(2)">← Hub</button>
    </div>

    <div class="mobile-frame-wrap">
      <div class="mobile-frame">
        <div class="mobile-status-bar">
          <span>9:52</span>
          <span>📶 ●●●●</span>
        </div>
        <div class="mobile-content" style="background: #0f172a; align-items: center; text-align: center; justify-content: center;">

          <!-- Check animado -->
          <div class="check-anim" style="margin: 0 auto 24px;">✓</div>

          <div style="font-size:22px;font-weight:900;color:white;margin-bottom:8px;">Atendimento validado!</div>
          <div style="font-size:14px;color:rgba(255,255,255,0.6);margin-bottom:32px;line-height:1.6;">
            Sua presença foi confirmada.<br/>Seu plano está protegido.
          </div>

          <!-- Detalhes -->
          <div style="background:rgba(16,185,129,0.1);border:1px solid rgba(16,185,129,0.3);border-radius:14px;padding:20px;width:100%;text-align:left;margin-bottom:32px;">
            <div style="font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:0.5px;color:#10b981;margin-bottom:12px;">Detalhes do atendimento</div>
            <div style="font-size:13px;color:rgba(255,255,255,0.7);line-height:2;">
              <div>🏥 Clínica São Lucas</div>
              <div>📍 Av. Paulista, 1000 — Bela Vista</div>
              <div>👨‍⚕️ Dr. Carlos Mendes — Clínico Geral</div>
              <div>🕐 Hoje, 14h32</div>
              <div>✅ Biometria confirmada</div>
              <div>✅ Geolocalização verificada</div>
            </div>
          </div>

          <div style="background:rgba(255,255,255,0.05);border-radius:10px;padding:12px;font-size:12px;color:rgba(255,255,255,0.4);margin-bottom:24px;text-align:center;">
            🛡️ Proteção ativa — seu plano não corre risco
          </div>

          <button class="mobile-btn" onclick="navigate(2)" style="background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);">
            ← Voltar ao início
          </button>
        </div>
      </div>
    </div>

    <footer class="footer">
      <strong>Blindagem PME</strong> · FIAP Google Challenge 2026<br/>
      Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591 · Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
    </footer>
```

- [ ] **Step 2: Verificar o fluxo completo do funcionário**

Execute o fluxo inteiro: Hub → "Explorar como Funcionário →" → "Entrar →" → "Iniciar verificação biométrica" → aguardar 3.2s → tela de confirmação com o check verde animado e os detalhes do atendimento.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: tela 11 - confirmação de atendimento com check animado"
```

---

## Task 13: Ajustes Finais de Polish + Correção de Sliders

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Adicionar cálculo inicial do ROI na Tela 8**

Localize o bloco `<script>` no final do `index.html`. Adicione ao final, antes de `</script>`:

```javascript
    // Inicializa valores do simulador ao exibir a tela 8
    document.getElementById('screen-8').addEventListener('transitionend', calcROI);
    // Fallback: recalcula quando a tela 8 fica ativa
    const observer = new MutationObserver(() => {
      if (document.getElementById('screen-8').classList.contains('active')) calcROI();
    });
    observer.observe(document.getElementById('screen-8'), { attributes: true, attributeFilter: ['class'] });
```

- [ ] **Step 2: Atualizar a função calcROI para calcular o prêmio estimado**

Localize a função `calcROI()` no bloco `<script>` e substitua pelo código abaixo:

```javascript
    function calcROI() {
      const vidas = parseInt(document.getElementById('slider-vidas').value) || 50;
      const sinistralidade = parseInt(document.getElementById('slider-sinistralidade').value) || 80;
      document.getElementById('vidas-val').textContent = vidas;
      document.getElementById('sinistralidade-val').textContent = sinistralidade + '%';
      const premioMensal = vidas * 680;
      document.getElementById('premio-val').textContent = premioMensal.toLocaleString('pt-BR');
      const economiaAnual = Math.max(0, Math.round(premioMensal * 12 * ((sinistralidade - 68) / 100) * 0.7));
      const custoAnual = vidas * 120 * 12;
      const roi = economiaAnual > 0 ? Math.round(((economiaAnual - custoAnual) / custoAnual) * 100) : 0;
      document.getElementById('economia-estimada').textContent = 'R$ ' + economiaAnual.toLocaleString('pt-BR');
      document.getElementById('roi-estimado').textContent = Math.max(0, roi) + '%';
    }
```

- [ ] **Step 3: Verificar fluxo completo de navegação**

Teste todos os caminhos de navegação:
1. Landing → Hub → CFO (Telas 3 → 4 → 5) → Hub
2. Hub → Corretor (Telas 6 → 7 → 8) → Hub
3. Hub → Funcionário (Telas 9 → 10 → 11) → Hub
4. Verificar que o botão "← Hub" funciona em todas as telas internas

- [ ] **Step 4: Commit final**

```bash
git add index.html
git commit -m "feat: polish final e simulador ROI corrigido"
```

---

## Task 14: Deploy no GitHub Pages

**Files:**
- Create: `README.md`

- [ ] **Step 1: Criar o README**

Crie `/Users/diego_santos/Workspace/FIAP/Desafio Google - Prototipo/README.md`:

```markdown
# Blindagem PME — Protótipo Navegável

Protótipo de alta fidelidade desenvolvido para o FIAP Google Challenge 2026.

**[▶ Acessar o protótipo](https://diegosantos90.github.io/blindagem-pme)**

## Equipe

| Nome | RM |
|------|----|
| Diego Rodolfo dos Santos | 360717 |
| Vitor Cidreira Garcia | 360591 |
| Pedro Henrique de Aquino Ramim | 360590 |
| Raphael Lopes Dantas | 362263 |

## Como navegar

1. **Landing Page** → clique em "Conheça a solução →"
2. **Hub Central** → escolha uma das 3 personas
3. Navegue pelas telas de cada persona usando os botões de navegação
```

- [ ] **Step 2: Criar repositório no GitHub e fazer push**

```bash
cd "/Users/diego_santos/Workspace/FIAP/Desafio Google - Prototipo"
git add README.md
git commit -m "docs: README com link do protótipo e equipe"
gh repo create DiegoSantos90/blindagem-pme --public --source=. --remote=origin --push
```

Se `gh` não estiver instalado ou não autenticado, execute manualmente:
1. Acesse https://github.com/new
2. Crie um repositório público chamado `blindagem-pme`
3. Execute:
```bash
git remote add origin https://github.com/DiegoSantos90/blindagem-pme.git
git push -u origin main
```

- [ ] **Step 3: Ativar GitHub Pages**

```bash
gh api repos/DiegoSantos90/blindagem-pme/pages \
  --method POST \
  -H "Accept: application/vnd.github+json" \
  -f source='{"branch":"main","path":"/"}'
```

Se preferir via interface: acesse **github.com/DiegoSantos90/blindagem-pme → Settings → Pages → Source: Deploy from branch → main → / (root) → Save**.

- [ ] **Step 4: Verificar o link público**

Aguarde ~2 minutos e acesse:
```
https://diegosantos90.github.io/blindagem-pme
```

O protótipo deve carregar com a landing page. Teste a navegação completa pelo link público.

- [ ] **Step 5: Commit de confirmação**

```bash
git add .
git commit -m "chore: deploy confirmado no GitHub Pages" --allow-empty
```
