# Flip Live Commerce - Design System & Specifications

> Documento compartilhado entre todos os agentes do time.
> Data: 2026-02-25

---

## 1. STACK & CDN Links

```html
<!-- Google Fonts - Roboto -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700&display=swap" rel="stylesheet">

<!-- Material Icons -->
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

<!-- Font Awesome 6 -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
```

Font stack: `Roboto, -apple-system, "Helvetica Neue", Helvetica, Arial, sans-serif`

---

## 2. Color Palette

### Brand
| Token | Hex | Usage |
|-------|-----|-------|
| `--flip-pink` | `#ff56a1` | Primary brand, CTAs, highlights |
| `--flip-pink-dark` | `#d6006e` | Gradients, hover states |
| `--flip-pink-gradient` | `linear-gradient(135deg, #ff56a1, #d6006e)` | Badges, featured cards |

### Text
| Token | Hex | Usage |
|-------|-----|-------|
| `--text-primary` | `#3E4956` | Body text, titles |
| `--text-secondary` | `#79899D` | Subtitles, labels |
| `--text-dark` | `#1D1D1D` | Strong emphasis |
| `--text-light` | `#8C8C8C` | Muted, sidebar default |

### Backgrounds
| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-page` | `#f5f5f5` | Page background |
| `--bg-card` | `#FFFFFF` | Cards, panels |
| `--bg-sidebar` | `#222222` | Dark sidebar |
| `--bg-sidebar-accent` | `#2F3235` | Sidebar sections |

### UI
| Token | Hex | Usage |
|-------|-----|-------|
| `--border-color` | `#E2E8F0` | Dividers, borders |
| `--shadow-card` | `#DCD6D6 0px 0px 2px 0px` | Card shadow |
| `--border-subtle` | `rgba(0,0,0,0.12)` | Header/sidebar borders |
| `--badge-blue` | `#027BE3` | Info badges, notifications |
| `--success` | `#21BA45` | Success states |
| `--warning` | `#F2C037` | Warning states |
| `--error` | `#C10015` | Error states, "ao vivo" |

---

## 3. Layout Structure

```
┌─────────────────────────────────────────────────────┐
│ HEADER (fixed, h:61px, bg:white, left:57px)         │
│ [≡] [◀] brand   breadcrumb    [loja] [🔔10] [DAI]  │
├──────┬──────────────────────────────────────────────┤
│SIDE  │                                              │
│BAR   │  PAGE CONTENT                                │
│57px  │  (padding: 16px-24px)                        │
│      │  max-width: 1400px                           │
│ bg:  │                                              │
│#222  │                                              │
│      │                                              │
│icons │                                              │
│only  │                                              │
│      │                                              │
└──────┴──────────────────────────────────────────────┘
```

### Sidebar CSS
```css
.sidebar {
  position: fixed;
  left: 0; top: 0; bottom: 0;
  width: 57px;
  background: #222222;
  z-index: 200;
  transition: width 0.3s ease;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  padding-top: 12px;
}
.sidebar:hover { width: 220px; }
.sidebar .nav-item {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 10px 16px;
  color: #8C8C8C;
  text-decoration: none;
  font-size: 13px;
  font-weight: 500;
  white-space: nowrap;
  transition: all 0.2s;
  position: relative;
}
.sidebar .nav-item:hover { color: #F7F7F7; background: rgba(255,255,255,0.05); }
.sidebar .nav-item.active { color: #F7F7F7; }
.sidebar .nav-item.active::before {
  content: '';
  position: absolute; left: 0; top: 50%;
  transform: translateY(-50%);
  width: 3px; height: 24px;
  background: #F7F7F7;
  border-radius: 0 2px 2px 0;
}
.sidebar .nav-item .material-icons { font-size: 22px; min-width: 22px; color: inherit; }
.sidebar .nav-item span { opacity: 0; transition: opacity 0.2s; }
.sidebar:hover .nav-item span { opacity: 1; }
```

### Header CSS
```css
.header {
  position: fixed;
  top: 0; left: 57px; right: 0;
  height: 61px;
  background: #fff;
  border-bottom: 1px solid rgba(0,0,0,0.12);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 24px;
  z-index: 100;
}
```

### Content CSS
```css
.page-container {
  padding-top: 61px;
  padding-left: 57px;
  min-height: 100vh;
  background: #f5f5f5;
}
.page-content {
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px 24px;
}
```

---

## 4. Sidebar Menu Items (ALL PAGES MUST USE)

```
PROFILE SECTION:
  - Avatar circle "DAI" (bg: #ff56a1, color: white, 36px)

MAIN MENU:
  home         → "Homepage"       → dashboard.html
  store        → "Minha Loja"     → #
  thumb_up     → "Meus Favoritos" → #
  wallet       → "Minha Carteira" → #
  shopping_cart → "Minhas Vendas" → #

─── divider (rgba(255,255,255,0.08)) ───

LIVE COMMERCE (with "NOVO" pink badge):
  videocam     → "Minhas Lives"   → minhas-lives.html
  add_circle   → "Criar Live"     → criar-live.html
  share        → "Compartilhar"   → compartilhar-live.html

─── divider ───

FOOTER:
  help    → "FAQ"  → #
  logout  → "Sair" → #
```

### "NOVO" Badge on Live Commerce items:
```css
.novo-badge {
  background: #ff56a1;
  color: #fff;
  font-size: 8px;
  font-weight: 700;
  padding: 2px 5px;
  border-radius: 3px;
  letter-spacing: 0.5px;
  margin-left: 4px;
}
```

---

## 5. Header Structure (ALL PAGES MUST USE)

```html
<header class="header">
  <div class="header-left">
    <span class="header-brand">OBOTICARIO</span>
    <span class="breadcrumb-sep">›</span>
    <span class="breadcrumb-current">[PAGE NAME]</span>
  </div>
  <div class="header-right">
    <button class="header-btn">
      <i class="material-icons">shopping_bag</i>
      <span>Meu Espaco</span>
      <i class="material-icons" style="font-size:16px;">expand_more</i>
    </button>
    <div class="header-notification-wrap">
      <button class="header-btn">
        <i class="material-icons">notifications</i>
      </button>
      <span class="notification-badge">10</span>
    </div>
    <div class="header-avatar">DAI</div>
  </div>
</header>
```

---

## 6. Common Components

### Cards
```css
.card {
  background: #fff;
  border-radius: 8px;
  box-shadow: #DCD6D6 0px 0px 2px 0px;
  padding: 20px;
}
```

### Buttons
```css
/* Primary (pink) */
.btn-primary {
  background: #ff56a1;
  color: #fff;
  border: none;
  border-radius: 6px;
  padding: 10px 20px;
  font-family: inherit;
  font-size: 13px;
  font-weight: 700;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 6px;
}
.btn-primary:hover { background: #e0408a; }

/* Outline */
.btn-outline {
  background: #fff;
  color: #3E4956;
  border: 1px solid #E2E8F0;
  border-radius: 6px;
  padding: 10px 20px;
  font-family: inherit;
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 6px;
}
.btn-outline:hover { border-color: #ff56a1; color: #ff56a1; }

/* Ghost/Text */
.btn-ghost {
  background: transparent;
  color: #79899D;
  border: none;
  padding: 8px 12px;
  font-family: inherit;
  font-size: 13px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 4px;
}
.btn-ghost:hover { color: #3E4956; }
```

### Status Badges
```css
.badge { padding: 4px 10px; border-radius: 4px; font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.3px; display: inline-flex; align-items: center; gap: 4px; }
.badge-live { background: #C10015; color: #fff; } /* AO VIVO */
.badge-scheduled { background: #027BE3; color: #fff; } /* AGENDADA */
.badge-ended { background: #E0E0E0; color: #79899D; } /* ENCERRADA */
.badge-draft { background: #F2C037; color: #3E4956; } /* RASCUNHO */
```

### Form Inputs
```css
.form-group { margin-bottom: 20px; }
.form-label {
  display: block;
  font-size: 12px;
  font-weight: 600;
  color: #79899D;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 6px;
}
.form-input {
  width: 100%;
  padding: 10px 14px;
  border: 1px solid #E2E8F0;
  border-radius: 6px;
  font-family: inherit;
  font-size: 14px;
  color: #3E4956;
  background: #fff;
  transition: border-color 0.2s;
}
.form-input:focus {
  outline: none;
  border-color: #ff56a1;
  box-shadow: 0 0 0 3px rgba(255,86,161,0.1);
}
```

---

## 7. Product Data (use in all pages)

| # | Name | Price | Image URL |
|---|------|-------|-----------|
| 1 | Combo L'eau de Lily: Desodorante Colonia 75ml + Creme 250g | R$ 329,80 | `https://flipnet-assets.s3.sa-east-1.amazonaws.com/admin/public/users/products/117455/5d76ce8a-55e4-4667-bc41-71ddf09a148a-bot-2024071018.jpg` |
| 2 | Combo Lily Blanche: Desodorante Colonia 75ml + Creme 250g | R$ 364,80 | `https://flipnet-assets.s3.sa-east-1.amazonaws.com/admin/public/users/products/117455/2e945662-0a1f-4b71-bbce-d56c17bf6401-bot-2025030502.jpg` |
| 3 | Body Spray L'eau de Lily Blanche 100ml | R$ 49,90 | `https://flipnet-assets.s3.sa-east-1.amazonaws.com/admin/public/users/products/117455/2f161d48-d3a6-4c49-9fbe-8eed0400d422-bot-57882-l-eau-de-lily-blanche-body-spray-01.jpg` |
| 4 | L'eau de Lily Blanche Desodorante Colonia 75ml | R$ 219,90 | `https://flipnet-assets.s3.sa-east-1.amazonaws.com/admin/public/users/products/117455/ae6187eb-2dbc-4580-b916-d22b2bc6cfcd-bot-86895-l-eau-de-lily-blanche-01.jpg` |
| 5 | Combo QDB Batons: Hidratante Nude + Mate Malva 3,8g | R$ 105,80 | `https://flipnet-assets.s3.sa-east-1.amazonaws.com/admin/public/users/products/117455/73463997-b06a-40b2-afe9-dfe37d4b2879-bot-q2025030516.jpg` |
| 6 | Kit Presente Siage (4 itens) | R$ 167,96 | `https://flipnet-assets.s3.sa-east-1.amazonaws.com/admin/public/users/products/117455/c10e4cb9-69cd-4da6-ae6d-e8050eeb97df-e79371-53435-53436-84628-84687-estojo-maes-siage-nutri-rose-1.jpg` |
| 7 | Kit Presente Nativa Spa Ameixa (3 itens) | R$ 136,90 | `https://flipnet-assets.s3.sa-east-1.amazonaws.com/admin/public/users/products/117455/aa908e83-82ba-4e7f-8f5d-227f8219f77f-bot-87421-kit-maes-nativa-spa-ameixa-01.jpg` |
| 8 | Kit Presente Liz Sublime (2 itens) | R$ 164,90 | (use placeholder) |

---

## 8. Responsive Breakpoints

```css
/* Tablet */
@media (max-width: 1024px) {
  .sidebar { display: none; } /* or hamburger */
  .header { left: 0; }
  .page-container { padding-left: 0; }
}

/* Mobile */
@media (max-width: 768px) {
  .page-content { padding: 12px; }
  .header { padding: 0 12px; }
  .header-btn span { display: none; }
}

/* Small mobile */
@media (max-width: 480px) {
  /* Stack everything, simplify */
}
```

---

## 9. Pages to Build

| File | Title | Description |
|------|-------|-------------|
| `minhas-lives.html` | Minhas Lives | Dashboard de lives - listagem com status, stats, ações |
| `criar-live.html` | Criar Live | Formulário wizard: info básica → selecionar produtos → promoção → revisão |
| `sala-live.html` | Sala da Live | Painel de controle durante live: vídeo, produtos, chat, stats, QR code |
| `compartilhar-live.html` | Compartilhar Live | Gerador de QR code, cupom, links, embed code, social share |

### Existing Pages (reference):
| File | Title |
|------|-------|
| `dashboard.html` | Dashboard principal do parceiro |
| `index.html` | Página de live para o público (viewer) |

---

## 10. Navigation Flow

```
Dashboard (dashboard.html)
  └── "Iniciar Live" / "Agendar Live" / "Minhas Lives"
       │
       ├── Minhas Lives (minhas-lives.html)
       │     ├── "Criar Nova Live" → criar-live.html
       │     ├── "Entrar na Sala" → sala-live.html
       │     ├── "Compartilhar" → compartilhar-live.html
       │     └── "Ver Relatório" → (stats within page)
       │
       ├── Criar Live (criar-live.html)
       │     └── Step 1: Info → Step 2: Produtos → Step 3: Promoção → Step 4: Revisão
       │           └── "Iniciar Agora" → sala-live.html
       │           └── "Agendar" → minhas-lives.html
       │
       ├── Sala da Live (sala-live.html)
       │     ├── Video preview + controls
       │     ├── Product showcase queue
       │     ├── QR Code / Share panel
       │     ├── Chat panel
       │     └── "Encerrar Live" → minhas-lives.html
       │
       └── Compartilhar (compartilhar-live.html)
             ├── QR Code generator
             ├── Share links (WhatsApp, Facebook, Instagram)
             ├── Cupom code
             └── Embed code
```
