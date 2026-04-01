# Design Spec — Protótipo Navegável Blindagem PME

**Data:** 2026-03-30
**Prazo de entrega:** 2026-04-12
**Repositório:** https://github.com/DiegoSantos90/blindagem-pme
**URL final:** https://diegosantos90.github.io/blindagem-pme

---

## Equipe

| Nome | RM |
|------|----|
| Diego Rodolfo dos Santos | 360717 |
| Vitor Cidreira Garcia | 360591 |
| Pedro Henrique de Aquino Ramim | 360590 |
| Raphael Lopes Dantas | 362263 |

---

## Objetivo

Protótipo de alta fidelidade navegável simulando a experiência de uso da solução Blindagem PME para três personas distintas. Entregue como link público no GitHub Pages.

---

## Decisões Técnicas

- **Formato:** Único arquivo `index.html` com navegação via JavaScript (show/hide de seções)
- **Estilo:** CSS inline + bloco `<style>` — zero dependências externas, zero build step
- **Identidade visual:** Verde HealthTech (#10b981) com acento dourado (#fbbf24), fundo escuro (#0f172a) nas telas de app
- **Deploy:** GitHub Pages no repositório `DiegoSantos90/blindagem-pme`
- **Navegação:** Hub Central com botões por persona; botões "Próxima →" e "← Voltar" dentro de cada fluxo; botão "← Hub" em todas as telas internas

---

## Estrutura de Telas (11 telas)

### Entrada (2 telas)

**Tela 1 — Landing Page**
- Hero com logo Blindagem PME e tagline: *"A blindagem tecnológica que protege o plano de saúde da sua empresa"*
- Painel de impacto: R$34 bilhões perdidos/ano, sinistralidade média 85%, ajuste de prêmio até 20%
- CTA: botão "Conheça a solução →" navega para Hub Central

**Tela 2 — Hub Central**
- Apresenta os 4 componentes da solução (cards: Gestor Virtual, Biometria, Pré-auditoria, Integração Sem Atrito)
- 3 cards de persona com CTA:
  - 👔 "Explorar como CFO / Sócio" → Tela 3
  - 🤝 "Explorar como Corretor" → Tela 6
  - 👤 "Explorar como Funcionário" → Tela 9

---

### Fluxo CFO / Sócio de PME (3 telas)

**Tela 3 — Dashboard Principal**
- KPIs: Sinistralidade atual 68% (meta ✓), Economia gerada R$12.400/mês, Fraudes bloqueadas: 7
- Gráfico de barras simplificado: sinistralidade mês a mês (antes vs. depois do Blindagem PME)
- Alerta verde: "Seu plano está protegido. Ajuste abusivo contestado automaticamente."
- Botão: "Ver auditorias →"

**Tela 4 — Auditorias Bloqueadas**
- Tabela com 5 solicitações de reembolso interceptadas: tipo, valor, status (bloqueado/em análise), economia
- Destaque: total bloqueado no mês R$8.200
- Botão: "Ver relatório mensal →"

**Tela 5 — Relatório Mensal**
- Comparativo antes/depois: sinistralidade 84% → 68%, prêmio mensal R$38.000 → R$33.500
- Economia acumulada no trimestre: R$37.200
- Mensagem: "Relatório disponível para download" (botão simulado — não funcional)
- Botão: "← Hub Central"

---

### Fluxo Corretor de Saúde (3 telas)

**Tela 6 — Painel do Corretor**
- Lista de 4 clientes PME com status: sinistralidade, alertas ativos, economia gerada
- Badge "Em risco" para cliente com sinistralidade > 80%, badge "Protegido ✓" para demais
- Botão: "Ver relatório do cliente →" (abre Tela 7 com dados do cliente selecionado)

**Tela 7 — Relatório para Cliente**
- Dados do cliente selecionado: nome fictício "Empresa Alfa Ltda", 45 vidas
- Economia gerada pelo Blindagem PME no último trimestre: R$28.500
- Gráfico de linha: evolução da sinistralidade (últimos 6 meses)
- Botão: "Gerar proposta para novo cliente →"

**Tela 8 — Simulador de Proposta Comercial**
- Formulário pré-preenchido: número de vidas (slider), sinistralidade atual (%)
- Simulação de ROI: economia estimada anual vs. custo do Blindagem PME
- CTA: "Enviar proposta" (simulado)
- Botão: "← Hub Central"

---

### Fluxo Funcionário / Beneficiário — Mobile (3 telas)

> Estas telas são renderizadas em formato de smartphone (max-width 390px, centralizado) para simular experiência mobile.

**Tela 9 — App Login**
- Tela de boas-vindas do app Blindagem PME
- Campo "Nome do funcionário" + botão "Entrar"
- Mensagem: "Seu plano de saúde está protegido por Blindagem PME"

**Tela 10 — Check-in Biométrico**
- Ícone de câmera + texto: "Confirmando sua presença na clínica"
- Indicador de geolocalização: "📍 Clínica São Lucas — Av. Paulista, 1000"
- Animação de progresso (CSS) simulando reconhecimento facial
- Status: "Verificando identidade..." → após 2s → botão "Confirmar check-in"

**Tela 11 — Confirmação de Atendimento**
- Check verde animado: "✓ Atendimento validado!"
- Mensagem: "Sua presença foi confirmada. Seu plano está protegido."
- Detalhe: "Clínica São Lucas · 14h32 · Dr. Carlos Mendes"
- Botão: "← Hub Central"

---

## Rodapé Global

Presente em todas as 11 telas:

```
Blindagem PME · FIAP 2026
Diego Rodolfo dos Santos RM 360717 · Vitor Cidreira Garcia RM 360591
Pedro Henrique de Aquino Ramim RM 360590 · Raphael Lopes Dantas RM 362263
```

---

## Arquitetura do index.html

```
index.html
├── <style> — variáveis CSS, componentes reutilizáveis (card, btn, badge, kpi)
├── <div id="screen-*"> — 11 seções, todas com display:none exceto a ativa
├── <script>
│   ├── navigate(screenId) — esconde tudo, mostra a tela alvo, scroll to top
│   └── simulateCheckin() — setTimeout para animação da tela 10
└── Sem frameworks, sem CDN, sem fontes externas — 100% offline-capable
```

---

## Deploy

1. Criar repositório `blindagem-pme` em github.com/DiegoSantos90
2. Commitar `index.html` na branch `main`
3. Ativar GitHub Pages: Settings → Pages → Source: `main` / `/ (root)`
4. URL pública gerada: `https://diegosantos90.github.io/blindagem-pme`
