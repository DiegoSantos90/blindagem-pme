# Redesign Sidebar — Blindagem PME Protótipo

**Data:** 2026-04-01
**Branch:** `redesign/sidebar-profiles`
**Versão anterior preservada em:** `main`

---

## Objetivo

Redesenhar o protótipo `index.html` para ter layout com sidebar lateral fixa + switcher de perfis (CFO, Corretor, Paciente), mantendo todo o conteúdo existente e o tema escuro.

---

## Abordagem escolhida

**Refatoração gradual (Abordagem 1):** as 11 `<section>` atuais são convertidas em `.panel` dentro de um `#content-area`. A camada de sidebar + switcher é adicionada por cima. A função `navigate(n)` é substituída por `showPanel(id)` + `switchProfile(key)`. Nenhum conteúdo é reescrito.

---

## Estrutura HTML

```
<body>
├── #sidebar (240px, fixed left, 100vh)
│   ├── .sidebar-brand          → logo 🛡️ Blindagem PME
│   ├── .sidebar-tabs           → 3 abas: [👔 CFO] [🤝 Corretor] [📱 Paciente]
│   ├── #sidebar-nav            → itens de nav dinâmicos por perfil
│   └── .sidebar-user           → avatar (iniciais) + nome + cargo
│
└── #main-area (flex: 1, overflow-y: auto)
    ├── .main-topbar            → título da página + badge do perfil ativo
    └── #content-area           → painéis (.panel / .panel.active)
```

### Painéis por perfil

| Perfil   | Painel ID             | Label sidebar         | Ícone |
|----------|-----------------------|-----------------------|-------|
| CFO      | `cfo-dashboard`       | Dashboard             | 📊    |
| CFO      | `cfo-auditorias`      | Auditorias            | 🔍    |
| CFO      | `cfo-relatorio`       | Relatório Mensal      | 📈    |
| Corretor | `corretor-carteira`   | Carteira              | 👥    |
| Corretor | `corretor-relatorio`  | Relatório do Cliente  | 📄    |
| Corretor | `corretor-simulador`  | Simulador ROI         | 🧮    |
| Paciente | `paciente-checkin`    | Check-in Biométrico   | 📱    |

Cada `<section id="screen-N">` vira `<div class="panel" id="<painel-id>">`. Os painéis de CFO correspondem às antigas screens 3–5; Corretor às screens 6–8; Paciente às screens 9–11 (unificadas em um único painel dividido).

---

## Design Visual

### Cores (tema escuro mantido)

| Elemento               | Valor                          |
|------------------------|--------------------------------|
| body background        | `#0f172a`                      |
| sidebar background     | `#1e293b`                      |
| sidebar border-right   | `1px solid #334155`            |
| main-area background   | `#0f172a`                      |
| cards                  | `#1e293b` (padrão atual)       |

### Sidebar tabs (switcher)

- 3 botões horizontais com largura igual (`flex: 1`)
- **Ativo:** background `#10b981`, texto branco, font-weight 700
- **Inativo:** texto `#64748b`, hover `rgba(255,255,255,0.05)`
- Labels: `👔 CFO` / `🤝 Corretor` / `📱 Paciente`

### Itens de navegação (`#sidebar-nav`)

- Padding: 12px 20px
- **Ativo:** borda esquerda verde 3px + background `rgba(16,185,129,0.1)` + texto `#10b981`
- **Hover:** background `rgba(255,255,255,0.05)`
- Ícone + texto em linha

### Card de usuário (rodapé da sidebar)

- Avatar circular com iniciais (2 letras), background `#10b981`
- Nome do usuário ativo + cargo abaixo

### Topbar do conteúdo

- Título da página atual (branco, 20px, font-weight 700)
- Badge do perfil ativo à direita (ex: `👔 CFO · TechSol`)

### Painel Paciente (dividido)

- Grid 2 colunas
- Esquerda: frame mobile existente (telas 9→10→11 preservadas com `simulateCheckin()`)
- Direita: card "Como funciona" com 3 steps: Login → Check-in → Confirmação

---

## JavaScript

### Objeto de dados dos perfis

```javascript
const PROFILES = {
  cfo: {
    name: 'Maria Andrade',
    role: 'CFO · TechSol',
    avatar: 'MA',
    defaultPanel: 'cfo-dashboard',
    nav: [
      { id: 'cfo-dashboard',  icon: '📊', label: 'Dashboard'        },
      { id: 'cfo-auditorias', icon: '🔍', label: 'Auditorias'       },
      { id: 'cfo-relatorio',  icon: '📈', label: 'Relatório Mensal' }
    ]
  },
  corretor: {
    name: 'Roberto Alves',
    role: 'Corretor Parceiro',
    avatar: 'RA',
    defaultPanel: 'corretor-carteira',
    nav: [
      { id: 'corretor-carteira',  icon: '👥', label: 'Carteira'             },
      { id: 'corretor-relatorio', icon: '📄', label: 'Relatório do Cliente' },
      { id: 'corretor-simulador', icon: '🧮', label: 'Simulador ROI'        }
    ]
  },
  paciente: {
    name: 'Ana Silva',
    role: 'Funcionária · TechSol',
    avatar: 'AS',
    defaultPanel: 'paciente-checkin',
    nav: [
      { id: 'paciente-checkin', icon: '📱', label: 'Check-in Biométrico' }
    ]
  }
};
```

### Funções

**`switchProfile(key)`**
1. Atualiza classe `active` nas 3 abas do switcher
2. Chama `renderNav(key)` para repopular `#sidebar-nav`
3. Atualiza `.sidebar-user` com avatar, nome e cargo
4. Chama `showPanel(PROFILES[key].defaultPanel)`

**`renderNav(key)`**
- Limpa `#sidebar-nav`
- Para cada item em `PROFILES[key].nav`: cria `<button>` com ícone + label, `onclick="showPanel('id')"`

**`showPanel(panelId, clienteKey)`**
1. Remove `active` de todos os `.panel`
2. Adiciona `active` no painel alvo
3. Atualiza `#page-title` com o label do painel
4. Atualiza item ativo em `#sidebar-nav`
5. Se `panelId === 'corretor-relatorio'` e `clienteKey` fornecido: chama `loadRelatorio(clienteKey)`

**`loadRelatorio(key)`** — mantida sem alteração, chama `showPanel('corretor-relatorio')` no final

**`calcROI()`** — mantida sem alteração

**`simulateCheckin()`** — mantida sem alteração

### Estado inicial

```javascript
switchProfile('cfo'); // executa no DOMContentLoaded
```

---

## O que NÃO muda

- Todo o conteúdo HTML dos painéis (KPIs, tabelas, gráficos SVG, sliders)
- As funções `loadRelatorio`, `calcROI`, `simulateCheckin`
- O objeto `_clientes` com dados das empresas
- O `MutationObserver` do simulador ROI (substituído pelo `showPanel` observer)
- O footer com nomes e RMs da equipe (removido das telas individuais, consolidado em um único footer no `#main-area`)

---

## O que é removido

- A `<section id="screen-1">` (landing page) e `<section id="screen-2">` (hub central) — substituídas pelo próprio shell de sidebar
- A função `navigate(n)` e todos os `onclick="navigate(N)"`
- Os `<div class="topbar">` internos de cada tela (substituídos pela `.main-topbar` global)
- Os footers individuais de cada tela (consolidados em um único)
