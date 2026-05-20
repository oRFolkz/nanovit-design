# Brief 05 — Carrossel de produtos (reutilizável: bestsellers, kits, lançamentos, etc.)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-produtos.liquid`
**Snippet:** `snippets/nanovit-product-card.liquid` (reusável em vários contextos)
**Referência mock:** `index.html` linhas 2612-2792 (Bestsellers) e 2814-3020 (Kits — mesma estrutura).

**O que é:** Carrossel horizontal de cards de produto, populado por uma coleção. Mesma section reutilizada com `collection` diferente vira "Mais Vendidos" ou "Kits Nanovit" ou "Lançamentos".

**Por quê uma section única:** Bestsellers e Kits do mock têm estrutura idêntica de card. Mudar nome/coleção/copy é responsabilidade do admin via theme editor. Section única = menos código, menos manutenção.

---

## ✅ Checklist

### Tarefa 1 — Snippet `nanovit-product-card.liquid`

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** `snippets/nanovit-product-card.liquid`

**Argumentos do snippet:**
- `product` (objeto product, obrigatório)
- `show_tags` (boolean, default true)
- `show_rating` (boolean, default true)

**Estrutura do card** (mock linhas 2640-2666):
- Wrapper `<article>` branco, `border-radius: var(--nanovit-radius-lg)` (14px), padding 16px, border 1px `rgba(42,28,26,0.08)`, `position: relative`, flex column.
- Botão favoritar circular 34px no canto sup. direito (link pra `/account` se logged out, ou wishlist se app instalado — por ora apenas visual).
- Imagem: `aspect-ratio: 1/1`, padding interno 14px, `object-fit: contain` (frasco com respiro).
- **Tags (chips):** vêm do metafield `product.metafields.nanovit.tags_display` (lista de strings). Exemplo: ["Vegano", "Sem glúten", "Nanotech"]. Sem metafield → não renderiza.
- Cor da chip: alternar por palavra-chave (ex: "Vegano" → verde, "Nanotech" → vermelho). Manter mapping em snippet ou usar cor neutra cocoa-soft default.
- Título: `product.title`, font-title 600 15px, 2 linhas máx, line-height 1.3.
- Rating: estrelas + nota + nº reviews. Vem dos metafields `nanovit.rating` (number) e `nanovit.review_count` (integer). Sem metafield → omitir bloco de rating (não mostrar 0).
- Preço: `product.price | money` em destaque (700 20px cocoa). Se `product.compare_at_price > product.price`, mostrar `compare_at_price` riscado pequeno antes do preço atual.
- Parcelamento: calculado em Liquid (`product.price | divided_by: 3 | money`), prefixo "ou 3x de" e sufixo "sem juros". Texto micro mute.
- CTA: botão Nano Red "Adicionar" com ícone carrinho, formato `border-radius: var(--nanovit-radius-md)` (10px). Submit do form `/cart/add` com `id=product.first_available_variant.id`.

### Liquid (esqueleto)

```liquid
{%- comment -%}
  Snippet: nanovit-product-card
  Args:
    - product (obrigatório)
    - show_tags (boolean, default true)
    - show_rating (boolean, default true)
{%- endcomment -%}
{%- assign show_tags = show_tags | default: true -%}
{%- assign show_rating = show_rating | default: true -%}
{%- assign variant = product.selected_or_first_available_variant -%}
{%- assign tags_display = product.metafields.nanovit.tags_display.value -%}
{%- assign rating = product.metafields.nanovit.rating.value -%}
{%- assign review_count = product.metafields.nanovit.review_count.value -%}

<article class="nanovit-product-card">
  <button type="button" class="nanovit-product-card__fav" aria-label="Favoritar {{ product.title | escape }}">
    {% render 'icon', icon: 'heart' %}
  </button>

  <a href="{{ product.url }}" class="nanovit-product-card__media" aria-label="{{ product.title | escape }}">
    {%- if product.featured_image -%}
      {{ product.featured_image | image_url: width: 600 | image_tag: loading: 'lazy', widths: '300,600', alt: product.featured_image.alt | default: product.title }}
    {%- endif -%}
  </a>

  {%- if show_tags and tags_display != blank -%}
    <div class="nanovit-product-card__tags">
      {%- for tag in tags_display -%}
        <span class="nanovit-product-card__tag nanovit-product-card__tag--{{ tag | handle }}">{{ tag }}</span>
      {%- endfor -%}
    </div>
  {%- endif -%}

  <h3 class="nanovit-product-card__title">
    <a href="{{ product.url }}">{{ product.title }}</a>
  </h3>

  {%- if show_rating and rating != blank -%}
    <div class="nanovit-product-card__rating" aria-label="Avaliação {{ rating }} de 5">
      <span class="nanovit-product-card__stars" aria-hidden="true">★★★★★</span>
      <span><strong>{{ rating }}</strong>{% if review_count %} ({{ review_count }}){% endif %}</span>
    </div>
  {%- endif -%}

  <div class="nanovit-product-card__price">
    {%- if product.compare_at_price > product.price -%}
      <span class="nanovit-product-card__price-was">{{ product.compare_at_price | money }}</span>
    {%- endif -%}
    <span class="nanovit-product-card__price-now">{{ product.price | money }}</span>
  </div>
  {%- assign installment = product.price | divided_by: 3 -%}
  <p class="nanovit-product-card__installment">{{ 'products.installments' | t: count: 3, price: installment | money }}</p>

  {%- form 'product', product, class: 'nanovit-product-card__form' -%}
    <input type="hidden" name="id" value="{{ variant.id }}">
    <button type="submit" class="nanovit-product-card__add" {% if variant.available == false %}disabled{% endif %}>
      {% render 'icon', icon: 'cart' %}
      {%- if variant.available -%}
        {{ 'products.add_to_cart' | t }}
      {%- else -%}
        {{ 'products.sold_out' | t }}
      {%- endif -%}
    </button>
  {%- endform -%}
</article>
```

**Locales (`locales/pt-BR.json`):**
```json
{
  "products": {
    "add_to_cart": "Adicionar",
    "sold_out": "Esgotado",
    "installments": "ou {{ count }}x de {{ price }} sem juros"
  }
}
```

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Section `nanovit-home-produtos.liquid`

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** `sections/nanovit-home-produtos.liquid`

### Estrutura
- Section head: título + sub + ações (link "Ver todos" + setas circulares 56px).
- Track: `display: flex; gap: 10px; overflow-x: auto; scroll-snap-type: x mandatory; scrollbar-width: none`.
- Cada card: flex-basis adaptativo (4 visíveis desktop, 2 mobile com scroll).

### Schema

```json
{
  "name": "Carrossel de produtos",
  "class": "nanovit-home-produtos-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Título",
      "default": "Mais Vendidos"
    },
    {
      "type": "text",
      "id": "subtitle",
      "label": "Subtítulo",
      "default": "Os produtos mais amados pela nossa comunidade"
    },
    {
      "type": "collection",
      "id": "collection",
      "label": "Coleção fonte",
      "info": "Os produtos desta coleção aparecem no carrossel."
    },
    {
      "type": "range",
      "id": "limit",
      "label": "Máximo de produtos",
      "min": 4, "max": 12, "step": 1,
      "default": 8
    },
    {
      "type": "url",
      "id": "see_all_url",
      "label": "Link 'Ver todos'"
    },
    {
      "type": "text",
      "id": "see_all_label",
      "label": "Texto do link 'Ver todos'",
      "default": "Ver todos os produtos"
    },
    {
      "type": "checkbox",
      "id": "show_tags",
      "label": "Mostrar tags do produto",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_rating",
      "label": "Mostrar avaliação",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_arrows",
      "label": "Mostrar setas de navegação",
      "default": true
    }
  ],
  "presets": [
    {
      "name": "Mais Vendidos",
      "category": "Home Nanovit",
      "settings": {
        "title": "Mais Vendidos",
        "subtitle": "Os produtos mais amados pela nossa comunidade",
        "see_all_label": "Ver todos os produtos"
      }
    },
    {
      "name": "Kits Nanovit",
      "category": "Home Nanovit",
      "settings": {
        "title": "Kits Nanovit",
        "subtitle": "Combinações pensadas para resultados consistentes.",
        "see_all_label": "Ver todos os kits"
      }
    }
  ]
}
```

### Liquid (loop)

```liquid
{%- assign col = section.settings.collection -%}
{%- assign limit = section.settings.limit -%}

<section class="nanovit-home-produtos container" aria-label="{{ section.settings.title }}">
  {% render 'nanovit-section-head',
    title: section.settings.title,
    subtitle: section.settings.subtitle,
    link_url: section.settings.see_all_url,
    link_label: section.settings.see_all_label,
    show_arrows: section.settings.show_arrows %}

  {%- if col == blank or col.products.size == 0 -%}
    <div class="nanovit-home-produtos__empty">
      {{ 'home.products.empty' | t }}
    </div>
  {%- else -%}
    <div class="nanovit-home-produtos__track">
      {%- for product in col.products limit: limit -%}
        <div class="nanovit-home-produtos__item">
          {% render 'nanovit-product-card',
            product: product,
            show_tags: section.settings.show_tags,
            show_rating: section.settings.show_rating %}
        </div>
      {%- endfor -%}
    </div>
  {%- endif -%}
</section>
```

**Sub-tarefa:** criar snippet `nanovit-section-head.liquid` (título + sub + link + setas) — reutilizado em várias sections.

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendência cliente
- Criar coleções `bestsellers` (ou marcar produtos top com tag "best") e `kits` no admin Shopify.
- Opcional: preencher metafields `nanovit.rating`, `nanovit.review_count`, `nanovit.tags_display` em cada produto.

## ❌ Não fazer
- Não hardcoded produto algum. Tudo via `section.settings.collection`.
- Não hardcoded rating ou tags. Só metafield.
- Não usar travessão.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
