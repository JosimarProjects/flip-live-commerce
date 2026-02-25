# Flip Live Commerce - UX Specification

> Documento de especificacao detalhada para desenvolvimento das 4 paginas do Live Commerce.
> Data: 2026-02-25 | Autor: UX Specialist Agent

---

## INDICE

1. [Principios de UX](#1-principios-de-ux)
2. [Page 1: minhas-lives.html](#2-minhas-liveshtml---painel-de-lives)
3. [Page 2: criar-live.html](#3-criar-livehtml---wizard-de-criacao)
4. [Page 3: sala-live.html](#4-sala-livehtml---sala-de-controle)
5. [Page 4: compartilhar-live.html](#5-compartilhar-livehtml---compartilhamento)
6. [Padroes Globais](#6-padroes-globais)

---

## 1. PRINCIPIOS DE UX

### 1.1 Hierarquia Visual
- **Nivel 1 (H1):** Titulo da pagina - `font-size: 22px; font-weight: 700; color: #1D1D1D`
- **Nivel 2 (H2):** Subtitulo/secao - `font-size: 16px; font-weight: 600; color: #3E4956`
- **Nivel 3 (Label):** Rotulos - `font-size: 12px; font-weight: 600; color: #79899D; text-transform: uppercase; letter-spacing: 0.5px`
- **Body:** Texto corrido - `font-size: 14px; font-weight: 400; color: #3E4956`
- **Caption:** Texto auxiliar - `font-size: 12px; color: #79899D`

### 1.2 Espacamento Consistente
- **Entre secoes:** `24px`
- **Entre cards no grid:** `16px`
- **Padding interno de cards:** `20px` (desktop), `16px` (mobile)
- **Gap entre elementos dentro de cards:** `12px`
- **Margem inferior de titulos:** `16px`
- **Margem inferior de subtitulos:** `8px`

### 1.3 Botoes - Regra de Prioridade
1. **Primario (pink):** Uma UNICA acao principal por tela. Ex: "Criar Nova Live", "Iniciar Agora"
2. **Outline:** Acoes secundarias. Ex: "Ver Relatorio", "Agendar"
3. **Ghost:** Acoes terciarias. Ex: "Cancelar", "Voltar", links de navegacao

### 1.4 Acessibilidade
- Todos os botoes e links devem ter `min-height: 44px` para area de toque em mobile
- Contraste minimo WCAG AA: texto sobre fundo branco usa `#3E4956` (ratio 7.4:1)
- Focus states visiveis: `outline: 2px solid #ff56a1; outline-offset: 2px;`
- Icones acompanham texto (nunca icone sozinho sem tooltip/aria-label)
- Status badges usam icone + texto + cor (nunca apenas cor para comunicar estado)

### 1.5 Animacoes e Transicoes
- **Transicoes padrao:** `transition: all 0.2s ease`
- **Hover em cards:** Sutil elevacao `transform: translateY(-1px); box-shadow: 0 4px 12px rgba(0,0,0,0.08)`
- **Pulse "Ao Vivo":** Animacao de pulso no badge vermelho (keyframe abaixo)
- **Skeleton loading:** Blocos cinza animados com shimmer `linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%)`
- **NUNCA:** Animacoes que duram mais de 0.3s, bounce effects, parallax

```css
@keyframes pulse-live {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.6; }
}
.badge-live { animation: pulse-live 2s infinite; }

/* Pulse dot antes do texto "AO VIVO" */
.badge-live::before {
  content: '';
  width: 6px; height: 6px;
  background: #fff;
  border-radius: 50%;
  animation: pulse-live 1.5s infinite;
}
```

### 1.6 Estados de Erro
- Cor: `#C10015`
- Input com erro: `border-color: #C10015; box-shadow: 0 0 0 3px rgba(193,0,21,0.1)`
- Mensagem de erro abaixo do input: `font-size: 12px; color: #C10015; margin-top: 4px`
- Toast de erro: Faixa no topo da pagina com background `#C10015`, texto branco, auto-dismiss em 5s

### 1.7 Estados Vazios (Empty States)
- Ilustracao ou icone grande centralizado: `font-size: 64px; color: #E2E8F0`
- Titulo: `font-size: 16px; font-weight: 600; color: #3E4956`
- Subtitulo: `font-size: 14px; color: #79899D; max-width: 400px; text-align: center`
- CTA button abaixo do texto

---

## 2. MINHAS-LIVES.HTML - Painel de Lives

### 2.1 Objetivo
Dashboard central do parceiro para gerenciar todas as suas lives: visualizar status, acessar sala, criar novas, e ver metricas.

### 2.2 Breadcrumb
`OBOTICARIO > Minhas Lives`

### 2.3 Wireframe ASCII - Desktop

```
+----------------------------------------------------------------+
| HEADER: OBOTICARIO > Minhas Lives       [Meu Espaco] [Bell] [D]|
+------+---------------------------------------------------------+
|SIDE  |                                                         |
|BAR   | Minhas Lives                     [+ Criar Nova Live] btn|
|57px  |                                                         |
|      | +----------+ +----------+ +----------+ +----------+     |
|      | | AO VIVO  | | AGENDADAS| |ENCERRADAS| | TOTAL    |     |
|      | |    1     | |    3     | |    8     | | R$ 3.240 |     |
|      | |          | |          | |          | | receita  |     |
|      | +----------+ +----------+ +----------+ +----------+     |
|      |                                                         |
|      | [Todas] [Ao Vivo] [Agendadas] [Encerradas]  [Buscar...]|
|      |                                                         |
|      | +-----------------------------------------------------+ |
|      | | CARD: Live Ao Vivo (borda esquerda vermelha)         | |
|      | | [pulse] AO VIVO  "Kit Lily Blanche"    45min  23ppl  | |
|      | | 15 vendas | R$ 890  [Entrar na Sala] [Compartilhar] | |
|      | +-----------------------------------------------------+ |
|      |                                                         |
|      | +-----------------------------------------------------+ |
|      | | CARD: Live Agendada (borda esquerda azul)            | |
|      | | [clock] AGENDADA  "Promo Verao Lily"   28/02 19:00   | |
|      | | 5 produtos          [Editar] [Compartilhar] [Excluir]| |
|      | +-----------------------------------------------------+ |
|      |                                                         |
|      | +-----------------------------------------------------+ |
|      | | CARD: Live Encerrada (borda esquerda cinza)          | |
|      | | ENCERRADA  "Lancamento Perfumes"  20/02  1h23min     | |
|      | | 45 vendas | R$ 2.350 | 156 viewers   [Ver Relatorio]| |
|      | +-----------------------------------------------------+ |
|      |                                                         |
+------+---------------------------------------------------------+
```

### 2.4 Stats Cards (Topo)

Grid com 4 stat cards, estilo identico ao `dashboard.html` (`.stats-grid` com `grid-template-columns: repeat(4, 1fr)`).

| Card | Label | Valor | Sub-texto | Estilo |
|------|-------|-------|-----------|--------|
| 1 | AO VIVO AGORA | 1 | live ativa | `.stat-card` com borda esquerda `4px solid #C10015` |
| 2 | AGENDADAS | 3 | proxima: 28/02 | `.stat-card` com borda esquerda `4px solid #027BE3` |
| 3 | ENCERRADAS | 8 | este mes | `.stat-card` normal |
| 4 | RECEITA DE LIVES | R$ 3.240 | 4 lives realizadas | `.stat-card-live` (gradiente pink, identico ao dashboard) |

- Card "AO VIVO AGORA" deve ter o valor pulsando (animacao `pulse-live`)
- Cada card e clicavel e filtra a lista abaixo pelo status correspondente

### 2.5 Barra de Filtros + Busca

Posicionada abaixo dos stat cards, `margin-bottom: 16px`.

```
+----------------------------------------------------------------------+
| [Todas (12)] [Ao Vivo (1)] [Agendadas (3)] [Encerradas (8)]  |Buscar|
+----------------------------------------------------------------------+
```

- **Tabs/pills de filtro:**
  - Container: `display: flex; gap: 0; border: 1px solid #E2E8F0; border-radius: 6px; overflow: hidden;`
  - Cada tab: `padding: 8px 16px; font-size: 13px; font-weight: 500; color: #79899D; background: #fff; border-right: 1px solid #E2E8F0; cursor: pointer;`
  - Tab ativa: `background: #ff56a1; color: #fff; font-weight: 700;`
  - Contagem entre parenteses: `font-weight: 400;`
- **Campo de busca (direita):**
  - `.form-input` com icone `search` a esquerda
  - `width: 240px; max-width: 100%;`
  - Placeholder: "Buscar por nome da live..."

Layout: `display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 12px;`

### 2.6 Live Cards - Estrutura Detalhada

Cada live e representada como um card horizontal. Background branco, `border-radius: 8px`, box-shadow padrao.

#### Estrutura do Card:

```css
.live-card {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  padding: 16px 20px;
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 12px;
  border-left: 4px solid transparent;
  transition: all 0.2s ease;
}
.live-card:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}
```

#### Variantes por Status:

**AO VIVO (badge-live):**
- `border-left-color: #C10015`
- Badge: `.badge-live` com pulse animation e dot branco
- Mostra: titulo, duracao decorrida ("45 min"), viewers ao vivo, vendas em tempo real, receita
- Acoes: **[Entrar na Sala]** (btn-primary) + **[Compartilhar]** (btn-outline)
- Background sutil: `rgba(193,0,21,0.02)`

**AGENDADA (badge-scheduled):**
- `border-left-color: #027BE3`
- Badge: `.badge-scheduled`
- Mostra: titulo, data/hora agendada, quantidade de produtos selecionados
- Acoes: **[Editar]** (btn-outline) + **[Compartilhar]** (btn-outline) + **[Excluir]** (btn-ghost, cor #C10015)

**ENCERRADA (badge-ended):**
- `border-left-color: #E0E0E0`
- Badge: `.badge-ended`
- Mostra: titulo, data da live, duracao total, vendas, receita, pico de viewers
- Acoes: **[Ver Relatorio]** (btn-outline)
- Opacidade levemente reduzida: `opacity: 0.85` no hover volta a `1`

#### Layout interno do card:

```
+--+----+----------------------------------------------+----------------+
|  | TH | [BADGE] Titulo da Live                       |  [Btn1] [Btn2] |
|BL| UMB| Stat1 | Stat2 | Stat3                       |                |
|  |    |                                              |                |
+--+----+----------------------------------------------+----------------+
BL = border-left colorido
THUMB = thumbnail 60x60px, border-radius: 8px (imagem da live ou placeholder cinza com icone videocam)
```

- **Thumbnail:** `width: 60px; height: 60px; border-radius: 8px; object-fit: cover; background: #f5f5f5; flex-shrink: 0;`
  - Placeholder (sem thumbnail): Fundo `#f0f0f0` com icone `videocam` centralizado em `#bbb`
- **Info area:** `flex: 1; min-width: 0;`
  - Linha 1: Badge + Titulo (`font-size: 15px; font-weight: 600; color: #3E4956;`)
  - Linha 2: Stats inline separados por `|` (`font-size: 12px; color: #79899D;`)
    - Stats possiveis: duracao, viewers, vendas, receita, data, produtos
    - Separador `|` com `margin: 0 8px; color: #E2E8F0;`
- **Actions area:** `flex-shrink: 0; display: flex; gap: 8px; align-items: center;`

### 2.7 Empty State

Quando nao ha nenhuma live cadastrada:

```
+---------------------------------------------+
|                                             |
|           [icone: videocam_off]             |
|           64px, color: #E2E8F0              |
|                                             |
|     Voce ainda nao criou nenhuma live       |
|     16px, fw:600, #3E4956                   |
|                                             |
|     Crie sua primeira live e comece a       |
|     vender ao vivo para seu publico!        |
|     14px, #79899D, max-width: 400px         |
|                                             |
|        [+ Criar Minha Primeira Live]        |
|           btn-primary, grande               |
|                                             |
+---------------------------------------------+
```

- Centralizado vertical e horizontalmente no espaco de conteudo
- Icone: `videocam_off`, `font-size: 64px`, `color: #E2E8F0`
- Botao: `.btn-primary` com padding maior `padding: 12px 28px; font-size: 14px;`

### 2.8 Botao "Criar Nova Live" (Header da pagina)

Posicionado na mesma linha do titulo "Minhas Lives", alinhado a direita.

```css
.page-title-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
```

- Titulo: `font-size: 22px; font-weight: 700; color: #1D1D1D;`
- Botao: `.btn-primary` com icone `add` a esquerda - texto "Criar Nova Live"
- Link: `criar-live.html`

### 2.9 Dados Mockados

| # | Titulo | Status | Data | Duracao | Viewers | Vendas | Receita | Produtos |
|---|--------|--------|------|---------|---------|--------|---------|----------|
| 1 | Kit Lily Blanche - Especial | Ao Vivo | agora | 45min | 23 | 15 | R$ 890 | 4 |
| 2 | Promo Verao Lily | Agendada | 28/02 19:00 | - | - | - | - | 5 |
| 3 | Lancamento Nativa Spa | Agendada | 05/03 20:00 | - | - | - | - | 6 |
| 4 | Flash Sale Batons | Agendada | 10/03 18:30 | - | - | - | - | 3 |
| 5 | Lancamento Perfumes 2026 | Encerrada | 20/02 | 1h23min | 156 | 45 | R$ 2.350 | 7 |
| 6 | Dia dos Namorados Lily | Encerrada | 15/02 | 58min | 89 | 28 | R$ 1.680 | 5 |
| 7 | Kit Presente Maes | Encerrada | 10/02 | 1h05min | 67 | 19 | R$ 945 | 4 |
| 8 | Promo Relampago Siage | Encerrada | 05/02 | 42min | 45 | 12 | R$ 520 | 3 |

### 2.10 Responsivo

**Tablet (max-width: 1024px):**
- Stats grid: `grid-template-columns: repeat(2, 1fr);`
- Cards mantem layout horizontal
- Sidebar escondida

**Mobile (max-width: 768px):**
- Stats grid: `grid-template-columns: repeat(2, 1fr);`
- Filtros empilham: tabs ocupam 100% width, busca abaixo em 100%
- Cards: thumbnail esconde, layout ajusta para empilhado
- Botoes de acao empilham vertical abaixo das info

**Small mobile (max-width: 480px):**
- Stats grid: `grid-template-columns: 1fr 1fr;` com valores menores
- Cards: informacoes secundarias escondem (viewers, duracao)
- Apenas um botao de acao visivel (o primario), restante em menu "..." dropdown

---

## 3. CRIAR-LIVE.HTML - Wizard de Criacao

### 3.1 Objetivo
Formulario em 4 etapas para criar e agendar (ou iniciar imediatamente) uma live com produtos, promocao e configuracoes.

### 3.2 Breadcrumb
`OBOTICARIO > Minhas Lives > Criar Live`

### 3.3 Wireframe ASCII - Desktop

```
+----------------------------------------------------------------+
| HEADER: OBOTICARIO > Minhas Lives > Criar Live  [Meu Espaco]   |
+------+---------------------------------------------------------+
|SIDE  |                                                         |
|BAR   | Criar Nova Live                                         |
|57px  |                                                         |
|      | +-----------------------------------------------------+ |
|      | | PROGRESS BAR                                        | |
|      | | (1)Info ----(2)Produtos ----(3)Promocao ----(4)Revisar|
|      | |  [ativo]     [pendente]     [pendente]    [pendente] | |
|      | +-----------------------------------------------------+ |
|      |                                                         |
|      | +-----------------------------------------------------+ |
|      | | STEP CONTENT AREA                                   | |
|      | |                                                     | |
|      | | [Conteudo dinamico por step]                         | |
|      | |                                                     | |
|      | +-----------------------------------------------------+ |
|      |                                                         |
|      | +-----------------------------------------------------+ |
|      | | FOOTER BAR:  [Cancelar]   [Voltar]   [Proximo >>]   | |
|      | +-----------------------------------------------------+ |
|      |                                                         |
+------+---------------------------------------------------------+
```

### 3.4 Progress Indicator (Stepper)

Barra horizontal fixa no topo da area de conteudo.

```css
.wizard-stepper {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0;
  padding: 20px 0;
  margin-bottom: 24px;
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
}

.wizard-step {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 24px;
  position: relative;
}

.wizard-step-circle {
  width: 32px; height: 32px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  font-weight: 700;
  flex-shrink: 0;
}

/* Estados do step */
.wizard-step.active .wizard-step-circle {
  background: #ff56a1;
  color: #fff;
}
.wizard-step.completed .wizard-step-circle {
  background: #21BA45;
  color: #fff;
  /* icone check em vez de numero */
}
.wizard-step.pending .wizard-step-circle {
  background: #f0f0f0;
  color: #79899D;
}

.wizard-step-label {
  font-size: 13px;
  font-weight: 500;
}
.wizard-step.active .wizard-step-label { color: #3E4956; font-weight: 600; }
.wizard-step.completed .wizard-step-label { color: #21BA45; }
.wizard-step.pending .wizard-step-label { color: #79899D; }

/* Linha conectora entre steps */
.wizard-connector {
  width: 60px;
  height: 2px;
  background: #E2E8F0;
}
.wizard-connector.completed { background: #21BA45; }
```

Steps:
1. **Informacoes** (icone: `info`)
2. **Produtos** (icone: `shopping_bag`)
3. **Promocao** (icone: `local_offer`)
4. **Revisar** (icone: `check_circle`)

### 3.5 Step 1: Informacoes Basicas

Card branco contendo o formulario.

#### Campos:

| Campo | Tipo | Label | Placeholder | Obrigatorio | Validacao |
|-------|------|-------|-------------|-------------|-----------|
| Titulo | text input | TITULO DA LIVE | Ex: "Especial Kit Lily Blanche" | Sim | Min 5, max 80 chars |
| Descricao | textarea | DESCRICAO | Descreva o que voce vai apresentar... | Nao | Max 500 chars, mostrar contador |
| Thumbnail | file upload | IMAGEM DE CAPA | - | Nao | JPG/PNG, max 2MB |
| Modo | toggle | QUANDO INICIAR | - | Sim | - |
| Data | date input | DATA | dd/mm/aaaa | Se agendada | Data futura |
| Hora | time input | HORARIO | hh:mm | Se agendada | - |

#### Toggle "Quando Iniciar":

```
+-------------------------------------------+
| [  Iniciar Agora  ] | [  Agendar  ]       |
+-------------------------------------------+
  Segmented control, 2 opcoes
```

- **Segmented control:** `display: inline-flex; border: 1px solid #E2E8F0; border-radius: 6px; overflow: hidden;`
- Opcao ativa: `background: #ff56a1; color: #fff;`
- Opcao inativa: `background: #fff; color: #79899D;`
- Cada opcao: `padding: 10px 24px; font-size: 13px; font-weight: 600; cursor: pointer;`

Quando "Iniciar Agora" selecionado:
- Esconder campos de Data e Hora
- Mostrar mensagem info: "A live sera iniciada assim que voce confirmar na ultima etapa."

Quando "Agendar" selecionado:
- Mostrar campos de Data e Hora lado a lado (`display: grid; grid-template-columns: 1fr 1fr; gap: 16px;`)

#### Upload de Thumbnail:

```
+-------------------------------------------+
| +-------+                                 |
| |       |  Arraste uma imagem ou          |
| | icone |  [Escolher Arquivo]             |
| | image |                                 |
| |       |  JPG ou PNG, max 2MB            |
| +-------+                                 |
+-------------------------------------------+
```

- Area de drop: `border: 2px dashed #E2E8F0; border-radius: 8px; padding: 24px; text-align: center;`
- Hover: `border-color: #ff56a1; background: rgba(255,86,161,0.02);`
- Preview apos upload: Substituir area por imagem com botao "X" para remover
- Dimensoes sugeridas: "Recomendado: 1280x720px (16:9)"

#### Layout do Step 1:

```
+-------------------------------------------------------+
| TITULO DA LIVE                                        |
| [________________________________]                    |
|                                                       |
| DESCRICAO                                             |
| [________________________________]  0/500             |
| [________________________________]                    |
|                                                       |
| IMAGEM DE CAPA                                        |
| +------------------+                                  |
| |   drag & drop    |                                  |
| +------------------+                                  |
|                                                       |
| QUANDO INICIAR                                        |
| [ Iniciar Agora ] [ Agendar ]                         |
|                                                       |
| DATA              HORARIO                             |
| [dd/mm/aaaa]      [hh:mm]                             |
+-------------------------------------------------------+
```

O form usa max-width: `640px` centralizado dentro do card para nao ficar muito largo.

### 3.6 Step 2: Selecao de Produtos

#### Wireframe:

```
+-------------------------------------------------------+
| Selecionar Produtos              Selecionados: 3 de 8 |
|                                                       |
| [Buscar produto...]                                   |
|                                                       |
| +--------+ +--------+ +--------+ +--------+          |
| |  img   | |  img   | |  img   | |  img   |          |
| |        | |  [v]   | |        | |  [v]   |          |
| | Nome   | | Nome   | | Nome   | | Nome   |          |
| | R$ XXX | | R$ XXX | | R$ XXX | | R$ XXX |          |
| | [ ]    | | [x]    | | [ ]    | | [x]    |          |
| +--------+ +--------+ +--------+ +--------+          |
|                                                       |
| +--------+ +--------+ +--------+ +--------+          |
| |  img   | |  img   | |  img   | |  img   |          |
| |        | |        | |  [v]   | |        |          |
| | Nome   | | Nome   | | Nome   | | Nome   |          |
| | R$ XXX | | R$ XXX | | R$ XXX | | R$ XXX |          |
| | [ ]    | | [ ]    | | [x]    | | [ ]    |          |
| +--------+ +--------+ +--------+ +--------+          |
+-------------------------------------------------------+
```

#### Grid de Produtos:

```css
.product-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

/* Tablet: 3 colunas */
@media (max-width: 1024px) { .product-grid { grid-template-columns: repeat(3, 1fr); } }
/* Mobile: 2 colunas */
@media (max-width: 768px) { .product-grid { grid-template-columns: repeat(2, 1fr); } }
```

#### Product Selection Card:

```css
.product-select-card {
  background: #fff;
  border: 2px solid #E2E8F0;
  border-radius: 8px;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.2s;
  position: relative;
}
.product-select-card:hover {
  border-color: #ff56a1;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}
.product-select-card.selected {
  border-color: #ff56a1;
  background: rgba(255,86,161,0.02);
}
```

- **Imagem:** `width: 100%; aspect-ratio: 1/1; object-fit: cover;`
- **Check overlay (quando selecionado):** Canto superior direito, circulo rosa com check branco
  ```css
  .product-check {
    position: absolute;
    top: 8px; right: 8px;
    width: 24px; height: 24px;
    border-radius: 50%;
    background: #ff56a1;
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    opacity: 0;
    transition: opacity 0.2s;
  }
  .product-select-card.selected .product-check { opacity: 1; }
  ```
- **Info area (abaixo da imagem):**
  - Padding: `12px`
  - Nome: `font-size: 13px; font-weight: 500; color: #3E4956; line-clamp: 2;`
  - Preco: `font-size: 14px; font-weight: 700; color: #ff56a1; margin-top: 4px;`

#### Contador de Selecionados:

Posicionado no header da secao, ao lado do titulo.

```
Selecionar Produtos              Selecionados: [3] de 8
```

- "Selecionados:" em `#79899D`
- Numero `[3]` em badge pink: `background: #ff56a1; color: #fff; padding: 2px 8px; border-radius: 10px; font-weight: 700;`
- "de 8" em `#79899D`

#### Campo de Busca:

- Posicionado acima do grid
- `.form-input` com icone `search` a esquerda
- `width: 100%; margin-bottom: 16px;`
- Placeholder: "Buscar produto pelo nome..."
- Filtra em tempo real os cards visiveis

### 3.7 Step 3: Configuracao de Promocao

#### Wireframe:

```
+-------------------------------------------------------+
| Configurar Promocao                                   |
|                                                       |
| CUPOM DE DESCONTO                                     |
| [LIVE10_______________]  [Gerar Codigo]               |
|                                                       |
| DESCONTO                                              |
| (o) Porcentagem    ( ) Valor fixo                     |
| [15] %                                                |
|                                                       |
| FRETE GRATIS                                          |
| [toggle switch ON/OFF]  Ativar frete gratis na live   |
|                                                       |
| MENSAGEM PROMOCIONAL                                  |
| [________________________________]  0/150             |
| Sera exibida para os viewers no banner de promocao    |
|                                                       |
| +---------------------------------------------------+ |
| | PREVIEW DO BANNER:                                | |
| | +-----------------------------------------------+ | |
| | | [tag] CUPOM: LIVE10 | 15% OFF | FRETE GRATIS  | | |
| | +-----------------------------------------------+ | |
| +---------------------------------------------------+ |
+-------------------------------------------------------+
```

#### Campos:

| Campo | Tipo | Label | Default | Notas |
|-------|------|-------|---------|-------|
| Cupom | text + botao | CUPOM DE DESCONTO | LIVE10 | Botao "Gerar Codigo" gera aleatorio. Uppercase only. |
| Tipo Desconto | radio | TIPO DE DESCONTO | Porcentagem | 2 opcoes: Porcentagem / Valor fixo (R$) |
| Valor Desconto | number | DESCONTO | 15 | Sufixo "%" ou "R$" conforme tipo |
| Frete Gratis | toggle switch | FRETE GRATIS | OFF | Switch estilo iOS |
| Mensagem | textarea | MENSAGEM PROMOCIONAL | "" | Max 150 chars, contador visivel |

#### Toggle Switch:

```css
.toggle-switch {
  position: relative;
  width: 44px; height: 24px;
  background: #E2E8F0;
  border-radius: 12px;
  cursor: pointer;
  transition: background 0.2s;
}
.toggle-switch.active { background: #ff56a1; }
.toggle-switch::after {
  content: '';
  position: absolute;
  top: 2px; left: 2px;
  width: 20px; height: 20px;
  background: #fff;
  border-radius: 50%;
  transition: transform 0.2s;
  box-shadow: 0 1px 3px rgba(0,0,0,0.2);
}
.toggle-switch.active::after { transform: translateX(20px); }
```

#### Preview do Banner:

Card com fundo `#f5f5f5`, mostrando como o banner aparecera para os viewers.

```css
.promo-preview {
  background: #f5f5f5;
  border-radius: 8px;
  padding: 16px;
  margin-top: 20px;
}
.promo-preview-label {
  font-size: 11px;
  font-weight: 600;
  color: #79899D;
  text-transform: uppercase;
  margin-bottom: 8px;
}
.promo-banner {
  background: linear-gradient(135deg, #ff56a1, #d6006e);
  color: #fff;
  border-radius: 6px;
  padding: 10px 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 13px;
  font-weight: 600;
}
```

Nota: Toda esta secao e **opcional**. Deve haver um checkbox ou toggle no topo: "Adicionar promocao a esta live" - se desmarcado, todos os campos ficam colapsados/ocultos.

### 3.8 Step 4: Revisao e Confirmacao

#### Wireframe:

```
+-------------------------------------------------------+
| Revisar e Confirmar                                   |
|                                                       |
| +---------------------------------------------------+ |
| | INFORMACOES BASICAS                    [Editar >]  | |
| | Titulo: Kit Lily Blanche - Especial                | |
| | Descricao: Apresentacao dos novos kits...          | |
| | Quando: Agendar - 28/02/2026 as 19:00             | |
| +---------------------------------------------------+ |
|                                                       |
| +---------------------------------------------------+ |
| | PRODUTOS SELECIONADOS (4)              [Editar >]  | |
| | [img] Combo L'eau de Lily - R$ 329,80             | |
| | [img] Body Spray Lily - R$ 49,90                  | |
| | [img] Combo Lily Blanche - R$ 364,80              | |
| | [img] Kit Nativa Spa - R$ 136,90                  | |
| +---------------------------------------------------+ |
|                                                       |
| +---------------------------------------------------+ |
| | PROMOCAO                               [Editar >]  | |
| | Cupom: LIVE10 | 15% OFF | Frete Gratis: Sim      | |
| | Mensagem: "Aproveite 15% OFF em todos..."         | |
| +---------------------------------------------------+ |
|                                                       |
|            [Cancelar]    [<< Agendar Live >>]         |
|                     OU                                |
|            [Cancelar]    [<< Iniciar Agora >>]        |
+-------------------------------------------------------+
```

#### Secoes de Revisao:

Cada secao e um card branco com:
- Header: Titulo da secao (label style) + link "Editar" a direita (`.btn-ghost` com icone `edit`)
- Conteudo: Resumo das informacoes em formato compacto
- Clicar em "Editar" volta ao step correspondente

#### Lista de Produtos no Resumo:

Versao compacta dos produtos selecionados, similar ao `.product-row` do dashboard:
- Mini imagem `36x36px`
- Nome truncado
- Preco em pink

#### Botoes Finais:

Dependem do modo selecionado no Step 1:

**Se "Agendar":**
- Botao principal: "Agendar Live" (`.btn-primary`, grande, icone `calendar_today`)
- Ao clicar: Redireciona para `minhas-lives.html`

**Se "Iniciar Agora":**
- Botao principal: "Iniciar Live Agora" (`.btn-primary`, grande, icone `play_circle`, com fundo gradiente pink)
- Ao clicar: Redireciona para `sala-live.html`

Botao secundario sempre: "Cancelar" (`.btn-ghost`)

### 3.9 Footer de Navegacao do Wizard

Barra fixa na parte inferior da area de conteudo (nao sticky, apenas abaixo do conteudo do step).

```css
.wizard-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  margin-top: 24px;
  border-top: 1px solid #E2E8F0;
}
```

- **Lado esquerdo:** "Cancelar" (`.btn-ghost`, link para `minhas-lives.html`)
- **Lado direito:** "Voltar" (`.btn-outline`, icone `arrow_back`) + "Proximo" (`.btn-primary`, icone `arrow_forward`)
- Step 1: Esconde "Voltar"
- Step 4: "Proximo" muda para "Agendar Live" ou "Iniciar Live Agora"

### 3.10 Validacao

| Step | Validacao | Mensagem de Erro |
|------|-----------|------------------|
| 1 | Titulo vazio | "Informe o titulo da live" |
| 1 | Titulo < 5 chars | "O titulo deve ter pelo menos 5 caracteres" |
| 1 | Modo "Agendar" sem data | "Selecione a data da live" |
| 1 | Modo "Agendar" sem hora | "Selecione o horario da live" |
| 1 | Data no passado | "A data deve ser futura" |
| 2 | Nenhum produto selecionado | "Selecione pelo menos 1 produto" |
| 3 | Cupom com chars especiais | "Use apenas letras e numeros" |
| 3 | Desconto > 100% | "O desconto nao pode exceder 100%" |

Validacao ocorre ao clicar "Proximo". Campos com erro recebem borda vermelha e mensagem abaixo.

### 3.11 Responsivo

**Mobile (max-width: 768px):**
- Stepper: Mostra apenas numeros (sem labels), layout horizontal comprimido
- Campos de data/hora: empilham vertical (`grid-template-columns: 1fr`)
- Grid de produtos: 2 colunas
- Footer wizard: botoes empilham se necessario

**Small mobile (max-width: 480px):**
- Stepper: Numeros com dots entre eles, minimo
- Grid de produtos: 2 colunas com cards menores
- Upload area: menor

---

## 4. SALA-LIVE.HTML - Sala de Controle

### 4.1 Objetivo
Painel de controle em tempo real durante uma live. Esta e a pagina mais complexa. Simula a interface que o parceiro usa para gerenciar video, produtos, chat, stats e compartilhamento durante a transmissao.

### 4.2 Breadcrumb
`OBOTICARIO > Minhas Lives > Sala da Live`

### 4.3 Wireframe ASCII - Desktop (Layout 3 colunas)

```
+------------------------------------------------------------------+
| HEADER: OBOTICARIO > Sala da Live   [badge AO VIVO]  [Encerrar] |
+------+-----------------------------------------------------------+
|SIDE  |                                                           |
|BAR   | +-----------------+ +-------------------+ +------------+ |
|57px  | | COLUNA 1        | | COLUNA 2          | | COLUNA 3   | |
|      | | VIDEO PREVIEW   | | PRODUTOS DA LIVE  | | STATS      | |
|      | |                 | |                    | |            | |
|      | | +-------------+ | | [produto destaque] | | Viewers 23 | |
|      | | |             | | | [produto 2]        | | Vendas  15 | |
|      | | |   VIDEO     | | | [produto 3]        | | R$ 890,00 | |
|      | | |   PREVIEW   | | | [produto 4]        | | Tempo 45m  | |
|      | | |  (imagem)   | | |                    | |            | |
|      | | |             | | +--------------------+ | +----------+|
|      | | +-------------+ | |                    | | | QR CODE  ||
|      | |                 | | CHAT               | | |          ||
|      | | [cam] [mic]     | | [msg1]             | | |  [QR]   ||
|      | | [screen] [fx]   | | [msg2]             | | |          ||
|      | |                 | | [msg3]             | | | Download ||
|      | | CUPOM RAPIDO    | | [msg4]             | | +----------+|
|      | | [LIVE10] [Criar]| | [enviar msg...]    | |            | |
|      | +-----------------+ +-------------------+ | FERRAMENTAS| |
|      |                                           | [Cupom]    | |
|      |                                           | [Share]    | |
|      |                                           | [Config]   | |
|      |                                           +------------+ |
+------+-----------------------------------------------------------+
```

### 4.4 Layout Principal

```css
.sala-layout {
  display: grid;
  grid-template-columns: 1fr 340px 280px;
  gap: 16px;
  height: calc(100vh - 61px - 40px); /* viewport - header - padding */
}

/* Tablet: 2 colunas */
@media (max-width: 1200px) {
  .sala-layout {
    grid-template-columns: 1fr 300px;
    /* Coluna 3 (stats) vai para baixo da coluna 1 */
  }
}

/* Mobile: stack */
@media (max-width: 768px) {
  .sala-layout {
    grid-template-columns: 1fr;
    height: auto;
  }
}
```

### 4.5 Coluna 1: Video + Controles + Cupom

#### Header da Live (acima do video):

```
+---------------------------------------------------+
| [pulse] AO VIVO     Kit Lily Blanche - Especial   |
|                                     45:23 duracao  |
+---------------------------------------------------+
```

- Badge "AO VIVO" com pulse animation (`.badge-live`)
- Titulo da live ao lado
- Timer de duracao a direita: `font-size: 14px; font-weight: 600; font-variant-numeric: tabular-nums; color: #3E4956;`

#### Area de Video:

```css
.video-preview {
  width: 100%;
  aspect-ratio: 16/9;
  background: #1a1a1a;
  border-radius: 8px;
  overflow: hidden;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

- **Conteudo:** Imagem estatica simulando video (usar imagem de um dos produtos como fundo com blur, ou fundo escuro com icone de camera grande)
- **Overlay superior esquerdo:** Badge "AO VIVO" + contagem de viewers
  ```css
  .video-overlay-top {
    position: absolute;
    top: 12px; left: 12px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .viewers-badge {
    background: rgba(0,0,0,0.6);
    color: #fff;
    padding: 4px 10px;
    border-radius: 4px;
    font-size: 12px;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 4px;
  }
  ```
- **Overlay inferior:** Controles de video
  ```css
  .video-controls {
    position: absolute;
    bottom: 0; left: 0; right: 0;
    background: linear-gradient(transparent, rgba(0,0,0,0.7));
    padding: 20px 16px 12px;
    display: flex;
    justify-content: center;
    gap: 12px;
  }
  ```

#### Controles de Video:

Botoes circulares sobre o video:

| Icone | Label | Estado | Cor |
|-------|-------|--------|-----|
| `videocam` | Camera | Ativo (branco) | `rgba(255,255,255,0.2)` backdrop |
| `mic` | Microfone | Ativo (branco) | `rgba(255,255,255,0.2)` |
| `screen_share` | Compartilhar tela | Inativo (cinza) | `rgba(255,255,255,0.1)` |
| `auto_fix_high` | Efeitos | Inativo | `rgba(255,255,255,0.1)` |

```css
.video-control-btn {
  width: 44px; height: 44px;
  border-radius: 50%;
  border: none;
  background: rgba(255,255,255,0.2);
  color: #fff;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}
.video-control-btn:hover { background: rgba(255,255,255,0.3); }
.video-control-btn.muted { background: #C10015; } /* quando mudo */
```

#### Cupom Rapido (Abaixo do video):

Card compacto para criar/exibir cupom durante a live.

```
+---------------------------------------------------+
| CUPOM ATIVO                                       |
| +---------------------------------------------+  |
| | [tag] LIVE10 - 15% OFF    [Copiar] [Editar] |  |
| +---------------------------------------------+  |
|                                                   |
| [+ Criar Novo Cupom]                              |
+---------------------------------------------------+
```

- Card branco com borda `1px solid #E2E8F0`
- Cupom ativo em destaque: fundo `rgba(255,86,161,0.05)`, borda esquerda `3px solid #ff56a1`
- Botao "Criar Novo Cupom": `.btn-outline` com icone `add`

### 4.6 Coluna 2: Produtos + Chat

A coluna 2 e dividida verticalmente: Produtos na metade superior, Chat na metade inferior.

#### Produtos da Live (metade superior):

```
+------------------------------------------+
| PRODUTOS (4)                 [+ Adicionar]|
+------------------------------------------+
| [star] DESTAQUE                          |
| +--------------------------------------+ |
| | [img] Combo L'eau de Lily  R$ 329,80 | |
| |       12 vendas | [Remover Destaque] | |
| +--------------------------------------+ |
|                                          |
| [drag] [img] Body Spray Lily    R$49,90  |
|        3 vendas    [Destacar] [Remover]  |
|                                          |
| [drag] [img] Combo Lily Blanc  R$364,80  |
|        0 vendas    [Destacar] [Remover]  |
|                                          |
| [drag] [img] Kit Nativa Spa   R$136,90   |
|        0 vendas    [Destacar] [Remover]  |
+------------------------------------------+
```

**Produto em destaque:** Possui borda especial e marca visual.

```css
.product-featured {
  background: rgba(255,86,161,0.05);
  border: 1px solid #ff56a1;
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 8px;
}
.product-featured-badge {
  font-size: 10px;
  font-weight: 700;
  color: #ff56a1;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  display: flex;
  align-items: center;
  gap: 4px;
  margin-bottom: 6px;
}
```

**Demais produtos - lista:**

```css
.product-queue-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 12px;
  border-bottom: 1px solid #f0f0f0;
  transition: background 0.2s;
}
.product-queue-item:hover { background: #fafafa; }
```

- **Drag handle:** `icone drag_indicator`, cor `#bbb`, `cursor: grab`
- **Imagem:** `36x36px, border-radius: 6px`
- **Nome:** `font-size: 13px; font-weight: 500;` truncado
- **Preco:** `font-size: 12px; font-weight: 700; color: #ff56a1;`
- **Vendas:** `font-size: 11px; color: #79899D;` ex: "3 vendas"
- **Acoes:** Icone `star` (destacar) + icone `close` (remover) - botoes ghost pequenos

#### Chat (metade inferior):

```
+------------------------------------------+
| CHAT AO VIVO                     23 msgs |
+------------------------------------------+
| [avatar] Maria S.    14:32              |
| Amei esse kit! Ja quero!                 |
|                                          |
| [avatar] Pedro L.    14:33              |
| Tem frete gratis?                        |
|                                          |
| [avatar] Ana Costa   14:33              |
| Acabei de comprar!! Obrigada             |
|                                          |
| [avatar] Voce        14:34              |
| Sim, frete gratis com o cupom LIVE10!    |
|                                          |
+------------------------------------------+
| [Enviar mensagem...]              [Send] |
+------------------------------------------+
```

```css
.chat-panel {
  display: flex;
  flex-direction: column;
  border-top: 1px solid #E2E8F0;
  flex: 1;
  min-height: 0;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 12px;
}

.chat-message {
  display: flex;
  gap: 8px;
  margin-bottom: 12px;
}

.chat-avatar {
  width: 28px; height: 28px;
  border-radius: 50%;
  background: #E2E8F0;
  color: #79899D;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
  font-weight: 700;
  flex-shrink: 0;
}

.chat-bubble {
  flex: 1;
}

.chat-name {
  font-size: 11px;
  font-weight: 600;
  color: #3E4956;
}
.chat-name .chat-time {
  font-weight: 400;
  color: #79899D;
  margin-left: 6px;
}
.chat-text {
  font-size: 13px;
  color: #3E4956;
  margin-top: 2px;
  line-height: 1.4;
}

/* Mensagem do parceiro (voce) */
.chat-message.self .chat-avatar { background: #ff56a1; color: #fff; }
.chat-message.self .chat-name { color: #ff56a1; }

.chat-input-bar {
  display: flex;
  gap: 8px;
  padding: 10px 12px;
  border-top: 1px solid #E2E8F0;
  background: #fff;
}
.chat-input-bar input {
  flex: 1;
  border: 1px solid #E2E8F0;
  border-radius: 20px;
  padding: 8px 16px;
  font-size: 13px;
  font-family: inherit;
}
.chat-input-bar input:focus { outline: none; border-color: #ff56a1; }
.chat-send-btn {
  width: 36px; height: 36px;
  border-radius: 50%;
  background: #ff56a1;
  color: #fff;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

**Mensagens mockadas para o chat:**

| Autor | Hora | Mensagem |
|-------|------|----------|
| Maria S. | 14:32 | Amei esse kit! Ja quero! |
| Pedro L. | 14:33 | Tem frete gratis? |
| Ana Costa | 14:33 | Acabei de comprar!! |
| Juliana M. | 14:34 | Qual o cupom mesmo? |
| Voce | 14:34 | O cupom e LIVE10, meninas! 15% de desconto e frete gratis! |
| Lucas R. | 14:35 | Comprei o combo da Lily, muito bom! |
| Fernanda B. | 14:35 | Esse body spray e maravilhoso, recomendo demais |
| Carla S. | 14:36 | Tem pra pronta entrega? |

### 4.7 Coluna 3: Stats + QR Code + Ferramentas

#### Stats Panel (topo):

```
+-----------------------------+
| ESTATISTICAS EM TEMPO REAL  |
+-----------------------------+
| [icon] Viewers              |
|        23 assistindo        |
+-----------------------------+
| [icon] Vendas               |
|        15 pedidos           |
+-----------------------------+
| [icon] Receita              |
|        R$ 890,00            |
+-----------------------------+
| [icon] Duracao              |
|        00:45:23             |
+-----------------------------+
| [icon] Pico de Viewers      |
|        31 (as 14:20)        |
+-----------------------------+
```

```css
.stats-panel {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  overflow: hidden;
}
.stats-panel-header {
  padding: 12px 16px;
  font-size: 11px;
  font-weight: 700;
  color: #79899D;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  border-bottom: 1px solid #E2E8F0;
}
.stat-row {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 16px;
  border-bottom: 1px solid #f0f0f0;
}
.stat-row:last-child { border-bottom: none; }

.stat-row-icon {
  width: 32px; height: 32px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
}
.stat-row-icon.viewers { background: rgba(2,123,227,0.1); color: #027BE3; }
.stat-row-icon.sales { background: rgba(33,186,69,0.1); color: #21BA45; }
.stat-row-icon.revenue { background: rgba(255,86,161,0.1); color: #ff56a1; }
.stat-row-icon.duration { background: rgba(62,73,86,0.1); color: #3E4956; }
.stat-row-icon.peak { background: rgba(242,192,55,0.1); color: #F2C037; }

.stat-row-label { font-size: 11px; color: #79899D; }
.stat-row-value { font-size: 18px; font-weight: 600; color: #1D1D1D; }
```

Dados mockados:
- Viewers: 23 assistindo
- Vendas: 15 pedidos
- Receita: R$ 890,00
- Duracao: 00:45:23
- Pico: 31 (as 14:20)

#### QR Code Panel (meio):

```
+-----------------------------+
| QR CODE DA LIVE             |
| +-------------------------+ |
| |                         | |
| |      [QR CODE]          | |
| |     200x200px           | |
| |                         | |
| +-------------------------+ |
| flip.net/live/daicalmon     |
| [Copiar Link] [Download]   |
+-----------------------------+
```

```css
.qr-panel {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  padding: 16px;
  text-align: center;
  margin-top: 16px;
}
.qr-code-img {
  width: 160px; height: 160px;
  margin: 12px auto;
  background: #f5f5f5;
  border-radius: 8px;
  /* Placeholder: imagem QR gerada ou placeholder quadrado com icone qr_code */
}
.qr-link {
  font-size: 12px;
  color: #79899D;
  word-break: break-all;
  margin: 8px 0;
}
.qr-actions {
  display: flex;
  gap: 8px;
  justify-content: center;
  margin-top: 8px;
}
```

- O QR Code e um placeholder estatico (imagem ou div com icone `qr_code_2` grande)
- Link exibido abaixo: `flip.net/live/daicalmon`
- Botoes: "Copiar Link" (`.btn-outline` pequeno) + "Download" (`.btn-outline` pequeno com icone `download`)

#### Ferramentas Rapidas (inferior):

```
+-----------------------------+
| FERRAMENTAS                 |
| [local_offer] Cupom Rapido  |
| [share]       Compartilhar  |
| [settings]    Configuracoes |
+-----------------------------+
```

Cada ferramenta e um botao/link clicavel:

```css
.tool-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 16px;
  cursor: pointer;
  border-radius: 6px;
  transition: background 0.2s;
  font-size: 13px;
  color: #3E4956;
}
.tool-item:hover { background: #f5f5f5; }
.tool-item .material-icons { font-size: 18px; color: #79899D; }
```

- "Cupom Rapido": abre o cupom na coluna 1 (scroll ou destaque)
- "Compartilhar": link para `compartilhar-live.html` (abriria em nova aba durante live real)
- "Configuracoes": placeholder

### 4.8 Botao "Encerrar Live"

Posicionado no **header** da pagina, a direita, antes dos icones de notificacao.

```css
.btn-end-live {
  background: transparent;
  color: #C10015;
  border: 1px solid #C10015;
  border-radius: 6px;
  padding: 6px 14px;
  font-size: 12px;
  font-weight: 700;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 4px;
  transition: all 0.2s;
}
.btn-end-live:hover {
  background: #C10015;
  color: #fff;
}
```

#### Confirmacao de Encerramento:

Ao clicar, exibe um **modal overlay**:

```
+-------------------------------------------+
| OVERLAY ESCURO (rgba(0,0,0,0.5))          |
|                                           |
|   +-----------------------------------+   |
|   | Encerrar Live?                    |   |
|   |                                   |   |
|   | [warning icon]                    |   |
|   | Esta acao nao pode ser desfeita.  |   |
|   | A live sera encerrada para        |   |
|   | todos os espectadores.            |   |
|   |                                   |   |
|   | Stats finais:                     |   |
|   | 23 viewers | 15 vendas | R$ 890   |   |
|   |                                   |   |
|   | [Cancelar]  [Encerrar Live]       |   |
|   +-----------------------------------+   |
+-------------------------------------------+
```

```css
.modal-overlay {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}
.modal-card {
  background: #fff;
  border-radius: 12px;
  padding: 32px;
  max-width: 420px;
  width: 90%;
  text-align: center;
}
.modal-title {
  font-size: 18px;
  font-weight: 700;
  color: #1D1D1D;
  margin-bottom: 16px;
}
.modal-text {
  font-size: 14px;
  color: #79899D;
  line-height: 1.5;
  margin-bottom: 20px;
}
.modal-actions {
  display: flex;
  gap: 12px;
  justify-content: center;
}
```

- "Cancelar": `.btn-outline`
- "Encerrar Live": botao vermelho `background: #C10015; color: #fff;`
- Ao confirmar: redireciona para `minhas-lives.html`

### 4.9 Responsivo

**Tablet (max-width: 1200px):**
- Layout: 2 colunas (video+cupom | produtos+chat)
- Stats e QR Code vao para baixo do video, em grid horizontal 2x2

**Mobile (max-width: 768px):**
- Layout: 1 coluna empilhada
- Ordem: Video > Stats (horizontal scroll) > Produtos > QR Code > Chat
- Chat pode ser colapsavel (toggle mostrar/esconder)
- Controles de video ficam fixed na parte inferior durante scroll

---

## 5. COMPARTILHAR-LIVE.HTML - Compartilhamento

### 5.1 Objetivo
Pagina dedicada para o parceiro compartilhar e promover sua live nas redes sociais e outros canais. Centraliza QR Code, links, cupom, embed code e card de divulgacao.

### 5.2 Breadcrumb
`OBOTICARIO > Minhas Lives > Compartilhar Live`

### 5.3 Wireframe ASCII - Desktop

```
+------------------------------------------------------------------+
| HEADER: OBOTICARIO > Compartilhar Live  [Meu Espaco] [Bell] [D]  |
+------+-----------------------------------------------------------+
|SIDE  |                                                           |
|BAR   | Compartilhar Live               "Kit Lily Blanche"        |
|57px  | [badge AGENDADA] 28/02/2026 as 19:00                      |
|      |                                                           |
|      | +---------------------+  +--------------------------+     |
|      | | QR CODE             |  | COMPARTILHAR NAS REDES   |     |
|      | |                     |  |                          |     |
|      | |     [QR CODE]       |  | [WhatsApp]  [Facebook]   |     |
|      | |     240x240px       |  | [Instagram] [Telegram]   |     |
|      | |                     |  |                          |     |
|      | | flip.net/live/dai   |  | LINK DA LIVE             |     |
|      | | [Copiar] [Download] |  | [https://flip....] [Copy]|     |
|      | +---------------------+  |                          |     |
|      |                          | CUPOM                    |     |
|      |                          | [LIVE10] [Copiar]        |     |
|      |                          +--------------------------+     |
|      |                                                           |
|      | +------------------------------------------------------+  |
|      | | CARD DE DIVULGACAO (preview visual)                   |  |
|      | | +--------------------------------------------------+ |  |
|      | | | [gradient bg]                                    | |  |
|      | | | [logo] LIVE COMMERCE                             | |  |
|      | | | Kit Lily Blanche - Especial                      | |  |
|      | | | 28/02 as 19:00 | Cupom: LIVE10 - 15% OFF       | |  |
|      | | | [QR mini] flip.net/live/daicalmon                | |  |
|      | | +--------------------------------------------------+ |  |
|      | | [Baixar Imagem]                                      |  |
|      | +------------------------------------------------------+  |
|      |                                                           |
|      | +------------------------------------------------------+  |
|      | | CODIGO EMBED                                         |  |
|      | | <iframe src="..."></iframe>          [Copiar Codigo]  |  |
|      | +------------------------------------------------------+  |
|      |                                                           |
|      | +------------------------------------------------------+  |
|      | | ANALYTICS DE COMPARTILHAMENTO                         |  |
|      | | [Views: 234] [Cliques: 45] [Conversoes: 12]          |  |
|      | +------------------------------------------------------+  |
|      |                                                           |
+------+-----------------------------------------------------------+
```

### 5.4 Layout Principal

```css
.share-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 24px;
}

@media (max-width: 768px) {
  .share-layout { grid-template-columns: 1fr; }
}
```

### 5.5 Header da Pagina

```
+-------------------------------------------------------+
| Compartilhar Live                                     |
| [AGENDADA] Kit Lily Blanche - Especial                |
| 28/02/2026 as 19:00 | 5 produtos | Cupom: LIVE10     |
+-------------------------------------------------------+
```

- Titulo: `font-size: 22px; font-weight: 700; color: #1D1D1D;`
- Badge de status + Nome da live: `font-size: 16px; font-weight: 500; color: #3E4956;`
- Meta info: `font-size: 13px; color: #79899D;` separada por `|`

### 5.6 Secao QR Code (Card esquerdo)

Card branco, centralizado.

```css
.qr-section {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  padding: 32px;
  text-align: center;
}
.qr-code-large {
  width: 240px; height: 240px;
  margin: 0 auto 16px;
  background: #f5f5f5;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

- QR Code grande: `240x240px` (placeholder com icone `qr_code_2` tamanho 80px em cinza)
- Label do link abaixo: `font-size: 14px; color: #3E4956; font-weight: 500;`
- Link: `font-size: 13px; color: #79899D; word-break: break-all;`
- Botoes: "Copiar Link" (`.btn-outline`) + "Baixar QR Code" (`.btn-primary` com icone `download`)
  - Lado a lado, centralizados
  - Gap: `12px`

### 5.7 Secao Compartilhamento Social (Card direito)

Card branco com multiplas sub-secoes.

#### Botoes de Redes Sociais:

```
+------------------------------------------+
| COMPARTILHAR NAS REDES                   |
|                                          |
| +------+ +------+ +------+ +------+     |
| | [wa]  | | [fb]  | | [ig]  | | [tg]  |  |
| | Whats | | Face  | | Insta | | Tele  |  |
| | App   | | book  | | gram  | | gram  |  |
| +------+ +------+ +------+ +------+     |
+------------------------------------------+
```

```css
.social-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
  margin-bottom: 24px;
}
.social-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 16px 12px;
  border: 1px solid #E2E8F0;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
  text-decoration: none;
}
.social-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}
.social-btn-icon {
  width: 44px; height: 44px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: #fff;
}
.social-btn-icon.whatsapp { background: #25D366; }
.social-btn-icon.facebook { background: #1877F2; }
.social-btn-icon.instagram { background: linear-gradient(135deg, #F58529, #DD2A7B, #8134AF); }
.social-btn-icon.telegram { background: #0088cc; }

.social-btn-label {
  font-size: 12px;
  font-weight: 500;
  color: #3E4956;
}
```

**Template de mensagem WhatsApp:**
```
Oi! Quero te convidar para minha LIVE de produtos O Boticario!

Kit Lily Blanche - Especial
Data: 28/02/2026 as 19:00
Cupom exclusivo: LIVE10 (15% OFF!)

Acesse: flip.net/live/daicalmon
```

#### Link da Live:

```
+------------------------------------------+
| LINK DA LIVE                             |
| +--------------------------------------+ |
| | https://flip.net/live/daicalmon  [C] | |
| +--------------------------------------+ |
+------------------------------------------+
```

- Input read-only com botao copiar integrado a direita
- Ao copiar: Tooltip "Copiado!" por 2s, icone muda para `check`
- `font-family: monospace; font-size: 13px;`

#### Cupom:

```
+------------------------------------------+
| CUPOM DE DESCONTO                        |
| +--------------------------------------+ |
| | [tag] LIVE10  15% OFF   Frete Gratis | |
| +--------------------------------------+ |
| [Copiar Cupom]                           |
+------------------------------------------+
```

- Display do cupom: card com fundo `rgba(255,86,161,0.05)`, borda `1px solid rgba(255,86,161,0.2)`, padding `12px 16px`
- Codigo: `font-size: 18px; font-weight: 700; color: #ff56a1; letter-spacing: 1px;`
- Info do desconto: `font-size: 13px; color: #3E4956;`
- Botao "Copiar Cupom": `.btn-outline` pequeno

### 5.8 Card de Divulgacao

Secao full-width abaixo do layout 2 colunas. Card visual que o parceiro pode salvar como imagem para postar.

```css
.promo-card-section {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  padding: 24px;
}
.promo-card-section-header {
  font-size: 11px;
  font-weight: 700;
  color: #79899D;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 16px;
}
```

#### Design do Card de Divulgacao:

```
+-------------------------------------------------------+
|  CARD DE DIVULGACAO                [Baixar Imagem]     |
|                                                       |
| +---------------------------------------------------+ |
| | [gradient: pink -> dark pink]                     | |
| |                                                   | |
| |  [camera icon] LIVE COMMERCE                      | |
| |                                                   | |
| |  Kit Lily Blanche - Especial                      | |
| |  font-size: 24px, branco, bold                    | |
| |                                                   | |
| |  28/02/2026 as 19:00                              | |
| |  font-size: 14px, branco 80%                      | |
| |                                                   | |
| |  +-------------+  Cupom: LIVE10                   | |
| |  |   QR CODE   |  15% OFF + Frete Gratis          | |
| |  |   100x100   |                                  | |
| |  +-------------+  flip.net/live/daicalmon         | |
| |                                                   | |
| |  [logo OBOTICARIO]                                | |
| +---------------------------------------------------+ |
+-------------------------------------------------------+
```

```css
.promo-card-visual {
  background: linear-gradient(135deg, #ff56a1 0%, #d6006e 50%, #8a0044 100%);
  border-radius: 12px;
  padding: 40px 32px;
  color: #fff;
  position: relative;
  overflow: hidden;
  max-width: 600px;
  margin: 0 auto;
}
/* Elementos decorativos */
.promo-card-visual::before {
  content: '';
  position: absolute;
  top: -40px; right: -40px;
  width: 160px; height: 160px;
  background: rgba(255,255,255,0.08);
  border-radius: 50%;
}
.promo-card-visual::after {
  content: '';
  position: absolute;
  bottom: -20px; left: -20px;
  width: 100px; height: 100px;
  background: rgba(255,255,255,0.05);
  border-radius: 50%;
}

.promo-card-badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
  opacity: 0.9;
  margin-bottom: 16px;
}
.promo-card-title {
  font-size: 24px;
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: 8px;
}
.promo-card-date {
  font-size: 14px;
  opacity: 0.8;
  margin-bottom: 20px;
}
.promo-card-footer {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  margin-top: 16px;
}
.promo-card-qr {
  width: 100px; height: 100px;
  background: #fff;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.promo-card-info {
  text-align: right;
}
.promo-card-coupon {
  font-size: 16px;
  font-weight: 700;
  margin-bottom: 4px;
}
.promo-card-discount {
  font-size: 13px;
  opacity: 0.85;
  margin-bottom: 4px;
}
.promo-card-link {
  font-size: 12px;
  opacity: 0.7;
}
```

Botao "Baixar Imagem": `.btn-primary` com icone `download`, alinhado a direita do header da secao.

### 5.9 Codigo Embed

Secao full-width.

```
+-------------------------------------------------------+
| CODIGO EMBED                          [Copiar Codigo]  |
|                                                       |
| +---------------------------------------------------+ |
| | <iframe                                           | |
| |   src="https://flip.net/live/daicalmon"           | |
| |   width="100%"                                    | |
| |   height="600"                                    | |
| |   frameborder="0"                                 | |
| |   allowfullscreen>                                | |
| | </iframe>                                         | |
| +---------------------------------------------------+ |
|                                                       |
| Copie e cole este codigo no seu site ou blog.         |
+-------------------------------------------------------+
```

```css
.embed-section {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  padding: 24px;
  margin-top: 20px;
}
.embed-code {
  background: #1a1a1a;
  color: #21BA45;
  border-radius: 8px;
  padding: 16px 20px;
  font-family: 'Roboto Mono', monospace;
  font-size: 13px;
  line-height: 1.6;
  overflow-x: auto;
  white-space: pre;
}
.embed-hint {
  font-size: 12px;
  color: #79899D;
  margin-top: 8px;
}
```

### 5.10 Analytics de Compartilhamento

Secao full-width, read-only stats.

```
+-------------------------------------------------------+
| ANALYTICS DE COMPARTILHAMENTO                         |
|                                                       |
| +-----------+ +-----------+ +-----------+             |
| | Views     | | Cliques   | | Conversoes|             |
| |   234     | |    45     | |    12     |             |
| | no link   | | no link   | | compras   |             |
| +-----------+ +-----------+ +-----------+             |
+-------------------------------------------------------+
```

- Grid 3 colunas de mini stat cards
- Estilo identico ao `.stat-card` do dashboard, mas menor
- Valores mockados: 234 views, 45 cliques, 12 conversoes (vendas)
- Sub-label explicativo em cada

```css
.analytics-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  margin-top: 20px;
}
.analytics-card {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  padding: 20px;
  text-align: center;
}
.analytics-value {
  font-size: 28px;
  font-weight: 300;
  color: #1D1D1D;
  margin-bottom: 4px;
}
.analytics-label {
  font-size: 11px;
  font-weight: 600;
  color: #79899D;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}
```

### 5.11 Responsivo

**Tablet (max-width: 1024px):**
- Layout QR + Social: mantém 2 colunas
- Social grid: 2x2

**Mobile (max-width: 768px):**
- Layout QR + Social: 1 coluna empilhada
- Social grid: 2x2
- Card de divulgacao: padding reduzido, font-sizes menores
- Analytics grid: 3 colunas comprimidas (valores menores)
- Embed code: scroll horizontal

**Small mobile (max-width: 480px):**
- Social grid: 2x2 compacto
- QR Code: 180x180px
- Analytics: empilha 1 coluna

---

## 6. PADROES GLOBAIS

### 6.1 Sidebar - Todas as Paginas

Todas as 4 paginas DEVEM incluir a sidebar conforme definido em `design-system.md`:
- Width: 57px (expandida 220px no hover)
- Background: #222222
- Item ativo: borda branca esquerda 3px
- Items de Live Commerce com badge "NOVO" em pink
- A pagina atual marca o item correspondente como `.active`

Mapeamento de `active`:
- `minhas-lives.html`: item `videocam` (Minhas Lives)
- `criar-live.html`: item `add_circle` (Criar Live)
- `sala-live.html`: item `videocam` (Minhas Lives) - pois e sub-pagina
- `compartilhar-live.html`: item `share` (Compartilhar)

### 6.2 Header - Todas as Paginas

Header fixo conforme `design-system.md`:
- Height: 61px
- Left: 57px (respeitar sidebar)
- Breadcrumb com `OBOTICARIO > [nome da pagina]`
- Botoes: "Meu Espaco" dropdown, Notificacoes (badge 10), Avatar "DAI"

### 6.3 Feedback de Acoes

| Acao | Feedback |
|------|----------|
| Copiar link/cupom | Tooltip "Copiado!" por 2s, icone muda para check verde |
| Salvar/criar com sucesso | Toast verde topo direito "Live criada com sucesso!" 3s |
| Erro de validacao | Input fica vermelho + mensagem abaixo |
| Deletar live | Modal de confirmacao antes |
| Navegar entre steps | Animacao slide horizontal sutil (0.3s) |

### 6.4 Loading States

Para a versao mockup estatica, loading nao sera implementado, mas documentado:
- Skeleton shimmer em cards antes do conteudo carregar
- Spinner circular pink nos botoes durante acoes
- Overlay com spinner central para mudancas de pagina

### 6.5 Consistencia de Dados

Todas as paginas devem referenciar a **mesma live mockada** para consistencia:
- **Live ativa:** "Kit Lily Blanche - Especial"
- **Status:** Ao Vivo (para sala-live.html) ou Agendada (para compartilhar-live.html)
- **Data:** 28/02/2026 as 19:00 (se agendada)
- **Produtos:** Usar os 4 primeiros da tabela do design-system (Combo L'eau de Lily, Combo Lily Blanche, Body Spray, L'eau de Lily Blanche)
- **Cupom:** LIVE10, 15% OFF, Frete Gratis
- **Link:** flip.net/live/daicalmon
- **Parceiro:** DAI / LOJADAICALMON / OBOTICARIO

### 6.6 Navegacao entre Paginas

Links devem usar caminhos relativos simples (todos os arquivos no mesmo diretorio):
- `minhas-lives.html`
- `criar-live.html`
- `sala-live.html`
- `compartilhar-live.html`
- `dashboard.html`

### 6.7 Z-index Stack

| Elemento | Z-index |
|----------|---------|
| Sidebar | 200 |
| Header | 100 |
| Modal overlay | 1000 |
| Modal card | 1001 |
| Tooltips | 500 |
| Dropdowns | 300 |

---

## FIM DO DOCUMENTO

Este documento serve como referencia completa para os 4 desenvolvedores construirem as paginas. Cada desenvolvedor deve:
1. Ler esta spec inteira para contexto geral
2. Focar na secao da sua pagina especifica
3. Seguir os padroes globais (Secao 6) para sidebar, header, feedback
4. Usar os dados mockados definidos na Secao 6.5
5. Referenciar o `design-system.md` para CSS base (botoes, inputs, badges, cores)
