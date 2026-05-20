# Brief — Header fiel ao mock (substituir Horizon header)

> Pré-req: `home-realinhamento-00-plano.md` (regras gerais).
**Referência mock:** `index.html` linhas 2299-2456 (header + mobile menu).

## Por quê esta rodada existe

QA visual revelou que o header atual ainda é o `sections/header.liquid` do Horizon (com CSS override pintando glassmorphism). Estruturalmente está **muito diferente** do mock:

**Mock pede:**
- 3 colunas: **logo esquerda** + **search field LARGO central** (input + ícone lupa, placeholder "O que você procura?") + **actions direita** (Login com label + Ajuda com label + cart redondo Nano Red 42px com badge).
- Logo visível no desktop e mobile (no mobile vai pro centro do shell, com hamburger esquerda).
- **Nav-strip** abaixo do shell: pills horizontais com "Compre por objetivo (active)" + "Quiz Personalizado" + "Kits" + "Mais vendidos" + "Lançamentos" + "Promoções" + "Todos os produtos". Cada pill abre **mega-menu dropdown** com produtos/objetivos.
- Mobile: hamburger esquerda + logo centro + cart direita; nav-strip fica oculto e vira mobile menu offcanvas.

**Loja hoje (Horizon default):**
- Sem logo (renderiza "Início" como texto onde o logo deveria estar).
- Sem search field central.
- Login/Ajuda só como ícones soltos sem label.
- Cart como ícone, sem fundo redondo Nano Red, sem badge dinâmico visível.
- Nav simples "Início | Mais vendidos | Coleções" — não é nav-strip de mega-menu.

Decisão anterior ("CSS override em vez de section custom") era razoável quando ainda tínhamos pouco do mock. Agora a divergência estrutural é grande demais. **Refazer.**

---

## ✅ Checklist de execução

### Tarefa 1 — Section custom `nanovit-header`

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Arquivo novo:** `sections/nanovit-header.liquid`

**O que substitui:** o uso do `header.liquid` original do Horizon no `header-group.json`. Não deletar o `header.liquid` original (deixar como código morto de referência), apenas trocar a referência no group.

### Estrutura do header (fiel ao mock)

```
header-wrap (sticky, top: 16px, z-index: 100)
  header-shell (background glassmorphism, border-radius: 32px)
    .header (grid auto 1fr auto, padding 16px 14px 14px 24px, gap 16px)
      logo (esquerda)
      search (centro, max-width 520px, pill com ícone lupa interno)
      actions (direita)
        action--login (svg + label "Login" + URL /account)
        action--ajuda (svg + label "Ajuda" + URL /pages/central-de-atendimento)
        cart (círculo 42px Nano Red, ícone branco, badge dinâmico cart.item_count)
    nav-strip (sob o header, padding 10px 14px 14px, gap 4px, flex wrap)
      nav-item N (pill + dropdown)
```

### Implementação Liquid (essencial — adaptar nomenclatura/estilo)

```liquid
{%- liquid
  assign logo_image = settings.logo
  assign menu = section.settings.main_menu
  assign cart_count = cart.item_count
-%}

<div class="nanovit-header-wrap">
  <div class="nanovit-header-shell">
    <header class="nanovit-header" role="banner">
      <button
        type="button"
        class="nanovit-header__menu-toggle"
        aria-label="Abrir menu"
        aria-controls="nanovit-mobile-menu"
        aria-expanded="false"
        data-nv-mobile-open
      >
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <line x1="3" y1="6" x2="21" y2="6"/>
          <line x1="3" y1="12" x2="21" y2="12"/>
          <line x1="3" y1="18" x2="21" y2="18"/>
        </svg>
      </button>

      <a href="{{ routes.root_url }}" class="nanovit-header__logo" aria-label="{{ shop.name }}">
        {%- if logo_image -%}
          {{ logo_image | image_url: width: 480 | image_tag: alt: shop.name, height: 32, widths: '120,240,480', class: 'nanovit-header__logo-img' }}
        {%- else -%}
          <img src="{{ 'nanovit-logo.svg' | asset_url }}" alt="{{ shop.name }}" height="32" class="nanovit-header__logo-img nanovit-header__logo-img--fallback">
        {%- endif -%}
      </a>

      <form action="{{ routes.search_url }}" method="get" role="search" class="nanovit-header__search">
        <span class="nanovit-header__search-icon" aria-hidden="true">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/>
          </svg>
        </span>
        <input
          type="search"
          name="q"
          placeholder="{{ section.settings.search_placeholder | default: 'O que você procura?' }}"
          aria-label="{{ 'general.search.placeholder' | t }}"
          autocomplete="off"
        >
        <input type="hidden" name="type" value="product">
      </form>

      <div class="nanovit-header__actions">
        <a href="{% if customer %}{{ routes.account_url }}{% else %}{{ routes.account_login_url }}{% endif %}" class="nanovit-header__action">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/>
          </svg>
          <span class="nanovit-header__action-label">
            {%- if customer -%}{{ customer.first_name | default: 'Conta' }}{%- else -%}Login{%- endif -%}
          </span>
        </a>
        <a href="{{ section.settings.help_url | default: '/pages/central-de-atendimento' }}" class="nanovit-header__action">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><line x1="12" y1="17" x2="12.01" y2="17"/>
          </svg>
          <span class="nanovit-header__action-label">Ajuda</span>
        </a>
        <a href="{{ routes.cart_url }}" class="nanovit-header__cart" aria-label="Carrinho com {{ cart_count }} {{ cart_count | pluralize: 'item','itens' }}">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <circle cx="9" cy="21" r="1"/><circle cx="20" cy="21" r="1"/>
            <path d="M1 1h4l2.7 13.4a2 2 0 0 0 2 1.6h9.7a2 2 0 0 0 2-1.6L23 6H6"/>
          </svg>
          {%- if cart_count > 0 -%}
            <span class="nanovit-header__cart-badge" data-cart-count>{{ cart_count }}</span>
          {%- endif -%}
        </a>
      </div>
    </header>

    {%- if section.blocks.size > 0 -%}
      <nav class="nanovit-nav-strip" aria-label="Categorias rápidas">
        {%- for block in section.blocks -%}
          {%- if block.type == 'pill' -%}
            {%- assign label = block.settings.label -%}
            {%- assign link = block.settings.link -%}
            {%- assign dropdown_type = block.settings.dropdown_type -%}
            <div class="nanovit-nav-item{% if block.settings.is_active %} is-active{% endif %}" {{ block.shopify_attributes }}>
              <a href="{{ link | default: '#' }}" class="nanovit-nav-pill{% if block.settings.is_active %} nanovit-nav-pill--active{% endif %}">
                {%- if block.settings.show_menu_icon -%}
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
                    <line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/>
                  </svg>
                {%- endif -%}
                {{ label }}
              </a>

              {%- case dropdown_type -%}
                {%- when 'objectives' -%}
                  {% render 'nanovit-megamenu-objectives', block: block %}
                {%- when 'feature' -%}
                  {% render 'nanovit-megamenu-feature', block: block %}
                {%- when 'collection' -%}
                  {% render 'nanovit-megamenu-collection', block: block %}
                {%- when 'none' -%}
                  {%- comment -%} sem dropdown {%- endcomment -%}
              {%- endcase -%}
            </div>
          {%- endif -%}
        {%- endfor -%}
      </nav>
    {%- endif -%}
  </div>
</div>

{% render 'nanovit-mobile-menu', section: section %}
```

### Schema (zero hardcode)

```json
{
  "name": "Cabeçalho Nanovit",
  "class": "nanovit-header-section",
  "tag": "header",
  "max_blocks": 12,
  "settings": [
    { "type": "text", "id": "search_placeholder", "label": "Placeholder da busca", "default": "O que você procura?" },
    { "type": "url", "id": "help_url", "label": "Link de Ajuda", "info": "Default /pages/central-de-atendimento", "default": "/pages/central-de-atendimento" },
    { "type": "header", "content": "Logo" },
    { "type": "paragraph", "content": "Logo principal vem de Settings → Logo. Fallback automático ao SVG do tema quando não configurado." },
    { "type": "range", "id": "logo_height_desktop", "label": "Altura do logo (desktop)", "min": 24, "max": 56, "step": 2, "unit": "px", "default": 32 },
    { "type": "range", "id": "logo_height_mobile", "label": "Altura do logo (mobile)", "min": 20, "max": 40, "step": 2, "unit": "px", "default": 28 }
  ],
  "blocks": [
    {
      "type": "pill",
      "name": "Pílula de navegação",
      "settings": [
        { "type": "text", "id": "label", "label": "Texto da pílula", "default": "Compre por objetivo" },
        { "type": "url", "id": "link", "label": "Link" },
        { "type": "checkbox", "id": "is_active", "label": "Marcar como ativa (destaque vermelho)", "default": false },
        { "type": "checkbox", "id": "show_menu_icon", "label": "Mostrar ícone de menu antes do texto", "default": false },
        { "type": "select", "id": "dropdown_type", "label": "Tipo de mega-menu",
          "options": [
            { "value": "none", "label": "Sem dropdown" },
            { "value": "objectives", "label": "Grid de objetivos (4 colunas, com ícones)" },
            { "value": "feature", "label": "Card destacado (texto + CTA)" },
            { "value": "collection", "label": "Grid de produtos por coleção" }
          ],
          "default": "none"
        },
        { "type": "header", "content": "Conteúdo do dropdown (quando aplicável)" },
        { "type": "text", "id": "dropdown_title", "label": "Título do dropdown" },
        { "type": "text", "id": "dropdown_more_label", "label": "Texto do link 'Ver tudo'" },
        { "type": "url", "id": "dropdown_more_url", "label": "URL do link 'Ver tudo'" },
        { "type": "collection", "id": "dropdown_collection", "label": "Coleção fonte (para dropdown_type=collection)", "info": "Os primeiros 6 produtos da coleção aparecem como cards no mega-menu." },
        { "type": "richtext", "id": "feature_text", "label": "Texto do feature (para dropdown_type=feature)" },
        { "type": "text", "id": "feature_cta_label", "label": "CTA do feature" },
        { "type": "url", "id": "feature_cta_url", "label": "Link do CTA do feature" }
      ]
    }
  ],
  "presets": [
    {
      "name": "Cabeçalho Nanovit",
      "category": "Nanovit",
      "blocks": [
        { "type": "pill", "settings": { "label": "Compre por objetivo", "is_active": true, "show_menu_icon": true, "dropdown_type": "objectives", "dropdown_title": "Encontre o que você precisa", "dropdown_more_label": "Ver todos os objetivos" } },
        { "type": "pill", "settings": { "label": "Quiz Personalizado", "dropdown_type": "feature", "feature_text": "<p>Responda 8 perguntas e receba uma seleção feita sob medida para seus objetivos.</p>", "feature_cta_label": "Começar agora", "feature_cta_url": "/pages/quiz" } },
        { "type": "pill", "settings": { "label": "Kits", "dropdown_type": "collection", "dropdown_title": "Combos prontos para presentear", "dropdown_more_label": "Ver todos os kits", "dropdown_more_url": "/collections/kits" } },
        { "type": "pill", "settings": { "label": "Mais vendidos", "dropdown_type": "collection", "dropdown_title": "Os queridinhos da casa", "dropdown_more_label": "Ver ranking completo", "dropdown_more_url": "/collections/bestsellers" } },
        { "type": "pill", "settings": { "label": "Lançamentos", "dropdown_type": "collection", "dropdown_title": "Chegou agora", "dropdown_more_label": "Ver tudo novo" } },
        { "type": "pill", "settings": { "label": "Promoções", "dropdown_type": "collection", "dropdown_title": "Ofertas da semana", "dropdown_more_label": "Ver todas as ofertas" } },
        { "type": "pill", "settings": { "label": "Todos os produtos", "link": "/collections/all", "dropdown_type": "none" } }
      ]
    }
  ]
}
```

### CSS principal (referência)

Mover/replicar tudo do bloco `.nanovit-header*` que já existe em `assets/nanovit-tokens.css` (override CSS atual) pra dentro de `{% stylesheet %}` da nova section. **Limpar** o bloco de override em `nanovit-tokens.css` depois (substituído por esse arquivo). Adicionar:

```css
.nanovit-header__search {
  flex: 1; max-width: 520px;
  display: flex; align-items: center;
  position: relative;
  background: rgba(255,255,255,0.6);
  border: 1px solid rgba(42,28,26,0.1);
  border-radius: var(--nanovit-radius-pill, 999px);
  height: 42px;
  padding: 0 18px 0 44px;
}
.nanovit-header__search:focus-within { border-color: var(--color-foreground); background: #fff; }
.nanovit-header__search-icon {
  position: absolute; left: 16px; top: 50%; transform: translateY(-50%);
  color: var(--nanovit-color-mute, #8a7a76); pointer-events: none;
}
.nanovit-header__search-icon svg { width: 16px; height: 16px; }
.nanovit-header__search input {
  flex: 1; background: transparent; border: 0; outline: none;
  font-family: inherit; font-size: 13px; color: var(--color-foreground); height: 100%;
}
.nanovit-header__search input::placeholder { color: var(--nanovit-color-mute, #8a7a76); }

.nanovit-header__actions { display: flex; align-items: center; gap: 6px; }
.nanovit-header__action {
  display: flex; align-items: center; gap: 8px;
  padding: 8px 14px;
  font-family: var(--font-heading--family); font-weight: 600; font-size: 12px;
  border-radius: var(--nanovit-radius-pill, 999px);
  color: var(--color-foreground); text-decoration: none;
  transition: background 0.2s;
}
.nanovit-header__action:hover { background: rgba(42,28,26,0.05); }
.nanovit-header__action svg { width: 16px; height: 16px; }
.nanovit-header__action-label { white-space: nowrap; }

.nanovit-header__cart {
  position: relative;
  width: 42px; height: 42px; border-radius: 50%;
  background: var(--color-primary); color: #fff;
  display: flex; align-items: center; justify-content: center;
  margin-left: 4px; text-decoration: none;
}
.nanovit-header__cart svg { width: 18px; height: 18px; }
.nanovit-header__cart-badge {
  position: absolute; top: -2px; right: -2px;
  background: var(--color-primary); color: #fff;
  width: 18px; height: 18px; border-radius: 50%;
  font-size: 10px; font-weight: 700;
  display: flex; align-items: center; justify-content: center;
  border: 2px solid #fff;
}

.nanovit-header__menu-toggle { display: none; }
@media (max-width: 989px) {
  .nanovit-header { grid-template-columns: auto 1fr auto; }
  .nanovit-header__menu-toggle { display: inline-flex; }
  .nanovit-header__logo { justify-self: center; }
  .nanovit-header__search { display: none; }
  .nanovit-header__action-label { display: none; }
  .nanovit-header__action svg { width: 18px; height: 18px; }
  .nanovit-nav-strip { display: none; }
}
```

(Resto do CSS — nav-strip, pills, mega-menu, etc. — copiar do `produto.html`/`index.html` originais ou recriar fiel.)

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Snippets de mega-menu

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

3 snippets pequenos pra organizar:
- `snippets/nanovit-megamenu-objectives.liquid` — grid 4 colunas de objetivos com ícones + label (admin define lista via metaobject `nanovit_objectives` ou via blocks-de-block: aceitável usar uma lista fixa via setting de linha de texto pra v1).
- `snippets/nanovit-megamenu-feature.liquid` — card grande com richtext + CTA (usa `block.settings.feature_text`, `feature_cta_label`, `feature_cta_url`).
- `snippets/nanovit-megamenu-collection.liquid` — grid de até 6 produtos da `block.settings.dropdown_collection` (cada item: thumb + nome + preço) + link "Ver tudo".

Pra `objectives`, sugestão simplificada: hard-coded em pt-BR no Liquid uma lista de 8 objetivos (Emagrecimento, Memória & Foco, Bem-estar, Energia, Sono, Imunidade, Beleza, Performance) com ícones SVG e URLs `/collections/<handle>`. Não é hardcode de dado de produto — é estrutura semântica de nav. Se quiser dinâmico depois, vira metaobject. Por ora aceitável.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Mobile menu offcanvas

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

Snippet `snippets/nanovit-mobile-menu.liquid`. Drawer lateral esquerdo (slide-in), com:
- Header: logo + botão fechar (X).
- Search field full-width (mesmo `form action=routes.search_url`).
- Lista vertical das pílulas do nav-strip (cada uma renderiza como link grande com ícone caret-right).
- Footer: links "Login / Minha conta", "Ajuda", "Política de Frete" (vindos de settings).
- Backdrop translúcido com clique pra fechar.

JS mínimo:
- `data-nv-mobile-open` no botão hamburger abre, `[data-nv-mobile-close]` fecha.
- `aria-expanded` no toggle, `aria-hidden` no menu, focus trap quando aberto.
- Bloqueio de scroll do body quando aberto (`html[scroll-lock]` já existe no theme).

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — Trocar `header-group.json` para usar `nanovit-header`

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

Editar `sections/header-group.json`:
- Manter `header_announcements` (marquee) no topo.
- Substituir referência ao `header` (Horizon) por `nanovit-header`.
- Settings da seção: garantir que os 7 pills do mock apareçam por default (preset).

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — Cleanup do CSS override antigo

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

Em `assets/nanovit-tokens.css`, **remover** o bloco "Header Nanovit · Shell glassmorphism sobre Horizon" (decisão Fase 0 polimento). Agora o header é nosso, não precisa de override.

Manter no `nanovit-tokens.css` apenas o que afeta sections genéricas (fonts, radii, color tokens, button pill, input radius global).

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências cliente
- Subir logo no admin (Settings → Brand assets → Logo). Sem isso, o fallback `nanovit-logo.svg` (asset do tema) entra.
- Criar página `/pages/central-de-atendimento` (já foi criada na Fase 4 — verificar).
- Páginas de objetivo (`/collections/sono`, etc.) — criadas pelas pendências da home.

## ❌ Não fazer
- Não tocar nos templates `templates/index.json`, `templates/product.*.json`, etc. — header é section global, não muda esses.
- Não deletar `sections/header.liquid` original do Horizon (deixar como referência).
- Não usar travessão. Não inflar copy. Manter padrão da marca.

## 📝 Log do Dev

**Resumo:**
- T1 ✅ `sections/nanovit-header.liquid` criada (415 linhas, schema com 7 pills preset, stylesheet inline, `enabled_on: { groups: ["header"] }`, `tag: section`).
- T2 ✅ 3 snippets: `nanovit-megamenu-objectives.liquid` (grid 8 objetivos hard-coded), `nanovit-megamenu-feature.liquid` (richtext + CTA), `nanovit-megamenu-collection.liquid` (até 6 produtos da `block.settings.dropdown_collection`).
- T3 ✅ `snippets/nanovit-mobile-menu.liquid` — drawer offcanvas com logo, search, lista das pills, footer com login/ajuda/política. Toggle via data-attrs.
- T4 ✅ `sections/header-group.json` substituiu `header_section` por `nanovit_header` com os 7 pills do preset materializados em blocks (objetivo ativo, quiz feature, kits/bestsellers/lançamentos/promoções collection, todos none). `header_announcements` mantido no topo.
- T5 ✅ Bloco "Header Nanovit · Shell glassmorphism sobre Horizon" removido de `assets/nanovit-tokens.css` (substituído por stub de comentário apontando pra section custom).

**Push staging:** ✅ limpo (após corrigir `tag: null` → `tag: "section"` no schema; null não é aceito como string). https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659

**Decisões:**
- Mantive `sections/header.liquid` original do Horizon intacto (cobertura: cart drawer, predictive search, etc. ainda referenciados internamente). Só removi do group; arquivo fica de referência.
- Objetivos no megamenu vieram hard-coded em pt-BR (8 categorias). Quando o cliente quiser editar, vira metaobject `nanovit_objectives`.
- Pills do header agora aparecem por default mesmo sem o cliente abrir o theme editor (block_order materializado no group JSON).

**Dúvidas:** nenhuma.

**Tempo:** ~25min total (T1 já estava feito antes do compact, T2-T5 + push + fix do tag = ciclo único).
