# Brief 04 — Linha Nanovit (carrossel de categorias)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-linha.liquid`
**Referência mock:** `index.html` linhas 2555-2610 (7 cards de categoria com imagem + título sobreposto).

**O que é:** Carrossel horizontal de cards de categoria/coleção. Cada card é uma "porta" para uma coleção (Emagrecimento, Sono & Relaxamento, Energia & Disposição, Imunidade, Beleza, Saúde da Mulher, Performance). Foto grande do produto + título sobreposto.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Layout
- Section head: título + sub + ações à direita (link "Ver todas as coleções" + setas circulares 56px).
- Track: `display: flex; gap: 14px; overflow-x: auto; scroll-snap-type: x mandatory`.
- Card: aspect-ratio aprox 3/4, fundo cream/pastel (cor por categoria), imagem do produto centralizada (~70%), título grande na base.
- Hover desktop: lift + sombra.

### Dados via Shopify

**Cada card vem de uma `collection` picker.** Admin escolhe qual coleção apontar; tema renderiza:
- Imagem: `collection.image` (fallback `collection.products.first.featured_image`)
- Título: `collection.title`
- Link: `collection.url`

Não hardcoded.

### Schema

```json
{
  "name": "Linha Nanovit",
  "class": "nanovit-home-linha-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "max_blocks": 12,
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Título",
      "default": "Linha Nanovit"
    },
    {
      "type": "text",
      "id": "subtitle",
      "label": "Subtítulo",
      "default": "Fórmulas limpas e de alta absorção para você alcançar sua melhor versão."
    },
    {
      "type": "url",
      "id": "see_all_url",
      "label": "Link 'Ver todas as coleções'"
    },
    {
      "type": "text",
      "id": "see_all_label",
      "label": "Texto do link",
      "default": "Ver todas as coleções"
    }
  ],
  "blocks": [
    {
      "type": "linha",
      "name": "Categoria",
      "settings": [
        {
          "type": "collection",
          "id": "collection",
          "label": "Coleção",
          "info": "A imagem da coleção (collection.image) é usada como visual do card. Defina no admin → Coleções → [coleção] → Imagem."
        },
        {
          "type": "color_background",
          "id": "bg_color",
          "label": "Cor de fundo do card",
          "default": "#fbe6e3"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Linha Nanovit",
      "category": "Home Nanovit",
      "blocks": [
        { "type": "linha" },
        { "type": "linha" },
        { "type": "linha" },
        { "type": "linha" },
        { "type": "linha" },
        { "type": "linha" },
        { "type": "linha" }
      ]
    }
  ]
}
```

### Liquid (essencial)

```liquid
{%- for block in section.blocks -%}
  {%- assign col = block.settings.collection -%}
  {%- if col != blank -%}
    <a href="{{ col.url }}" class="nanovit-home-linha__card" style="background: {{ block.settings.bg_color }};" {{ block.shopify_attributes }}>
      {%- if col.image != blank -%}
        {{ col.image | image_url: width: 600 | image_tag: loading: 'lazy', alt: col.title, widths: '300,600' }}
      {%- elsif col.products.first.featured_image != blank -%}
        {{ col.products.first.featured_image | image_url: width: 600 | image_tag: loading: 'lazy', alt: col.title, widths: '300,600' }}
      {%- endif -%}
      <h3 class="nanovit-home-linha__title">{{ col.title }}</h3>
    </a>
  {%- endif -%}
{%- endfor -%}
```

### Cores de fundo sugeridas (defaults dos blocks no preset)
- Emagrecimento: `#ffe4d6` (peach)
- Sono & Relaxamento: `#e0e4f5` (lavanda)
- Energia & Disposição: `#fff1cc` (amarelo soft)
- Imunidade: `#dcebe0` (mint)
- Beleza: `#fbe6e3` (rose)
- Saúde da Mulher: `#f5dde8` (pink)
- Performance: `#d9c0a3` (tan)

Aceitável defaults variados ou todos cream (`#faf6f1`) — admin ajusta no theme editor.

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendência cliente
- Criar 7 coleções no admin com handles `emagrecimento`, `sono-relaxamento`, `energia-disposicao`, `imunidade`, `beleza`, `saude-da-mulher`, `performance`. Cada uma com `collection.image` definida (foto do produto-líder daquela linha).

## ❌ Não fazer
- Não hardcoded URL ou título de categoria. Tudo vem da `collection`.
- Não usar travessão.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
