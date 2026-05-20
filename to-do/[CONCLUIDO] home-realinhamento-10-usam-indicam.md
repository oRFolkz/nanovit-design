# Brief 10 — Usam e Indicam (UGC vídeo + mini product card)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-ugc.liquid`
**Referência mock:** `index.html` linhas 3072-3170.

**O que é:** Carrossel horizontal de vídeos UGC (aspect-ratio 9:14, tipo Stories) com card pequeno de produto abaixo de cada vídeo (avatar do produto + nome + preço + botão "+" pra adicionar ao carrinho). Cada item liga uma criadora/cliente a um produto da marca.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Estrutura
- Section head (título + sub + ações).
- Track: scroll horizontal com 4.5 items visíveis desktop, 1.5 mobile.
- Cada item:
  - Vídeo `aspect-ratio: 9/14`, `border-radius: var(--nanovit-radius-xl)` (22px), `object-fit: cover`. Autoplay + muted + loop + playsinline.
  - Abaixo: linha branca radius lg com avatar produto + nome + preço + botão circular Nano Red "+".

### Dados via Shopify
- Vídeo: `block.settings.video` (Shopify aceita upload de vídeo nativo via `type: video`) OU URL externa.
- Produto referenciado: `block.settings.product` (product picker). Tema renderiza:
  - Avatar: `product.featured_image | image_url: width: 76` (38×38 retina)
  - Nome: `product.title`
  - Preço: `product.price | money`
  - URL: `product.url`
  - Add-to-cart: `{% form 'product' %} ... {% endform %}` com variant.id

### Schema

```json
{
  "name": "UGC · Usam e indicam",
  "class": "nanovit-home-ugc-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "max_blocks": 12,
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Título",
      "default": "Quem usa, indica"
    },
    {
      "type": "text",
      "id": "subtitle",
      "label": "Subtítulo",
      "default": "Histórias reais de quem transformou a rotina com Nanovit"
    },
    {
      "type": "url",
      "id": "see_all_url",
      "label": "Link 'Ver todos'"
    },
    {
      "type": "text",
      "id": "see_all_label",
      "label": "Texto 'Ver todos'",
      "default": "Ver todos os depoimentos"
    }
  ],
  "blocks": [
    {
      "type": "ugc-item",
      "name": "Depoimento UGC",
      "settings": [
        {
          "type": "video",
          "id": "video",
          "label": "Vídeo (upload nativo Shopify)",
          "info": "Recomendado vertical 9:16 ou 9:14, MP4 H.264. Máx ~20MB."
        },
        {
          "type": "url",
          "id": "video_url",
          "label": "OU URL externa de vídeo",
          "info": "Use apenas se não fizer upload. Aceita HLS .m3u8 ou MP4 hospedado."
        },
        {
          "type": "image_picker",
          "id": "poster",
          "label": "Poster (imagem de capa do vídeo)",
          "info": "Recomendado 360×560px."
        },
        {
          "type": "text",
          "id": "alt",
          "label": "Descrição acessível do vídeo",
          "default": "Cliente mostrando produto Nanovit"
        },
        {
          "type": "product",
          "id": "product",
          "label": "Produto relacionado"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "UGC Nanovit",
      "category": "Home Nanovit",
      "blocks": [
        { "type": "ugc-item" },
        { "type": "ugc-item" },
        { "type": "ugc-item" },
        { "type": "ugc-item" },
        { "type": "ugc-item" }
      ]
    }
  ]
}
```

### Liquid

```liquid
<section class="nanovit-home-ugc container" aria-label="{{ section.settings.title }}">
  {% render 'nanovit-section-head',
    title: section.settings.title,
    subtitle: section.settings.subtitle,
    link_url: section.settings.see_all_url,
    link_label: section.settings.see_all_label,
    show_arrows: true %}

  <div class="nanovit-home-ugc__rail">
    {%- for block in section.blocks -%}
      {%- assign product = block.settings.product -%}
      {%- assign variant = product.selected_or_first_available_variant -%}
      {%- assign video = block.settings.video -%}
      {%- assign video_url = block.settings.video_url -%}
      {%- assign poster = block.settings.poster -%}
      <article class="nanovit-home-ugc__item" {{ block.shopify_attributes }}>
        <div class="nanovit-home-ugc__media">
          {%- if video != blank -%}
            {{ video | video_tag: image_size: '720x', loop: true, controls: false, autoplay: true, muted: true, playsinline: true, class: 'nanovit-home-ugc__video', poster: poster }}
          {%- elsif video_url != blank -%}
            <video class="nanovit-home-ugc__video" src="{{ video_url }}" muted autoplay loop playsinline {% if poster %}poster="{{ poster | image_url: width: 720 }}"{% endif %} aria-label="{{ block.settings.alt | escape }}"></video>
          {%- elsif poster -%}
            {{ poster | image_url: width: 720 | image_tag: class: 'nanovit-home-ugc__video', alt: block.settings.alt }}
          {%- endif -%}
        </div>

        {%- if product != blank -%}
          <div class="nanovit-home-ugc__product">
            <a href="{{ product.url }}" class="nanovit-home-ugc__product-link">
              {%- if product.featured_image -%}
                {{ product.featured_image | image_url: width: 76 | image_tag: loading: 'lazy', alt: product.title, class: 'nanovit-home-ugc__thumb' }}
              {%- endif -%}
              <div class="nanovit-home-ugc__info">
                <p class="nanovit-home-ugc__name">{{ product.title | truncatewords: 4 }}</p>
                <p class="nanovit-home-ugc__price">{{ product.price | money }}</p>
              </div>
            </a>
            {%- form 'product', product, class: 'nanovit-home-ugc__add-form' -%}
              <input type="hidden" name="id" value="{{ variant.id }}">
              <button type="submit" class="nanovit-home-ugc__add" aria-label="Adicionar {{ product.title | escape }} ao carrinho" {% if variant.available == false %}disabled{% endif %}>
                {% render 'icon', icon: 'cart' %}
              </button>
            {%- endform -%}
          </div>
        {%- endif -%}
      </article>
    {%- endfor -%}
  </div>
</section>
```

### CSS

```css
.nanovit-home-ugc { margin-top: 64px; }
.nanovit-home-ugc__rail {
  display: flex; gap: 16px;
  overflow-x: auto; overflow-y: visible;
  scroll-snap-type: x mandatory; scrollbar-width: none;
  padding-bottom: 8px;
}
.nanovit-home-ugc__item {
  flex: 0 0 calc((100% - 64px) / 4.5);
  max-width: calc((100% - 64px) / 4.5);
  min-width: 0;
  scroll-snap-align: start;
  display: flex; flex-direction: column; gap: 12px;
}
.nanovit-home-ugc__media {
  aspect-ratio: 9 / 14;
  border-radius: var(--nanovit-radius-xl);
  overflow: hidden;
  background: var(--color-foreground);
}
.nanovit-home-ugc__video {
  width: 100%; height: 100%;
  object-fit: cover; display: block;
}
.nanovit-home-ugc__product {
  display: flex; align-items: center; gap: 10px;
  background: #fff;
  border: 1px solid rgba(42,28,26,0.08);
  border-radius: var(--nanovit-radius-lg);
  padding: 8px 8px 8px 10px;
}
.nanovit-home-ugc__product-link {
  display: flex; align-items: center; gap: 10px;
  flex: 1; min-width: 0;
  color: inherit; text-decoration: none;
}
.nanovit-home-ugc__thumb {
  width: 38px; height: 38px; object-fit: contain; flex-shrink: 0;
}
.nanovit-home-ugc__info { flex: 1; min-width: 0; }
.nanovit-home-ugc__name {
  font-family: var(--font-heading--family);
  font-weight: 600; font-size: 13px;
  color: var(--color-foreground);
  white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
  margin-bottom: 2px;
}
.nanovit-home-ugc__price {
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 13px;
  color: var(--color-primary);
}
.nanovit-home-ugc__add {
  width: 36px; height: 36px; border-radius: 50%;
  background: var(--color-primary); color: #fff; border: 0;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer; flex-shrink: 0;
}
.nanovit-home-ugc__add:disabled { opacity: 0.4; cursor: not-allowed; }
@media (max-width: 749px) {
  .nanovit-home-ugc__item {
    flex: 0 0 70%; max-width: 70%;
  }
}
```

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendência cliente
- Subir 5+ vídeos UGC verticais no theme editor + ligar cada um a um produto da loja.

## ❌ Não fazer
- Não autoplay vídeo com som. Sempre `muted` + `playsinline`.
- Não hardcoded vídeo/produto. Tudo via blocks.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
