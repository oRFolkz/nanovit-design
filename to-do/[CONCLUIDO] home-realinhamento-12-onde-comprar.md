# Brief 12 — Onde Comprar (logos marketplaces + trust badges)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-marketplaces.liquid`
**Referência mock:** `index.html` linhas 3245-3275.

**O que é:** Bloco cream centralizado com eyebrow vermelho + título + parágrafo + grid horizontal de logos de marketplaces (Droga Raia, Drogasil, Panvel, Amazon, Mercado Livre, etc.) + linha inferior com trust badges (Compra segura, Entrega rápida, Atendimento BR, etc.).

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Estrutura
- Wrapper card cream radius xl, padding `72px 56px` desktop, `48px 24px` mobile, `text-align: center`.
- Eyebrow vermelho uppercase.
- Título h2.
- Parágrafo mute `max-width: 520px; margin: 0 auto 48px`.
- Linha 1: row flex `justify-content: space-around` com 4-8 logos `max-height: 44px`.
- Hairline divisor `border-top: 1px solid rgba(42,28,26,0.08)`.
- Linha 2: row flex com 3 trust badges (ícone + texto micro uppercase).

### Schema

```json
{
  "name": "Onde Comprar",
  "class": "nanovit-home-marketplaces-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "max_blocks": 12,
  "settings": [
    {
      "type": "text",
      "id": "eyebrow",
      "label": "Eyebrow",
      "default": "Disponível em todo o Brasil"
    },
    {
      "type": "text",
      "id": "title",
      "label": "Título",
      "default": "Onde você já compra."
    },
    {
      "type": "richtext",
      "id": "body",
      "label": "Texto",
      "default": "<p>A Nanovit está presente nas principais redes de farmácias e marketplaces do país, para que você encontre seus produtos onde for mais conveniente.</p>"
    },
    {
      "type": "header",
      "content": "Trust badges (linha inferior)"
    },
    {
      "type": "checkbox",
      "id": "show_trust",
      "label": "Mostrar trust badges",
      "default": true
    }
  ],
  "blocks": [
    {
      "type": "marketplace",
      "name": "Logo de marketplace",
      "settings": [
        {
          "type": "image_picker",
          "id": "logo",
          "label": "Logo (PNG/SVG)",
          "info": "Altura máxima renderizada: 44px. Use logo monocromático ou preservando cor original."
        },
        {
          "type": "text",
          "id": "name",
          "label": "Nome (alt text)",
          "default": "Marketplace"
        },
        {
          "type": "url",
          "id": "link",
          "label": "Link externo (opcional)"
        }
      ]
    },
    {
      "type": "trust",
      "name": "Trust badge",
      "limit": 3,
      "settings": [
        {
          "type": "select",
          "id": "icon",
          "label": "Ícone",
          "options": [
            { "value": "shield-check", "label": "Escudo check (segurança)" },
            { "value": "truck", "label": "Caminhão (entrega)" },
            { "value": "headphones", "label": "Headphones (atendimento)" },
            { "value": "leaf", "label": "Folha (natural)" },
            { "value": "award", "label": "Selo (certificação)" }
          ],
          "default": "shield-check"
        },
        {
          "type": "text",
          "id": "label",
          "label": "Texto",
          "default": "Compra segura"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Onde Comprar",
      "category": "Home Nanovit",
      "blocks": [
        { "type": "marketplace" },
        { "type": "marketplace" },
        { "type": "marketplace" },
        { "type": "marketplace" },
        { "type": "marketplace" },
        { "type": "trust", "settings": { "icon": "shield-check", "label": "Compra segura" } },
        { "type": "trust", "settings": { "icon": "truck", "label": "Entrega rápida" } },
        { "type": "trust", "settings": { "icon": "headphones", "label": "Atendimento BR" } }
      ]
    }
  ]
}
```

### Liquid

```liquid
{%- liquid
  assign marketplace_blocks = section.blocks | where: 'type', 'marketplace'
  assign trust_blocks = section.blocks | where: 'type', 'trust'
-%}

<section class="nanovit-home-marketplaces container" aria-label="{{ section.settings.title }}">
  <div class="nanovit-home-marketplaces__inner">
    <p class="nanovit-home-marketplaces__eyebrow">{{ section.settings.eyebrow }}</p>
    <h2 class="nanovit-home-marketplaces__title">{{ section.settings.title }}</h2>
    <div class="nanovit-home-marketplaces__body">{{ section.settings.body }}</div>

    {%- if marketplace_blocks.size > 0 -%}
      <div class="nanovit-home-marketplaces__logos">
        {%- for block in marketplace_blocks -%}
          {%- assign tag_open = 'span' -%}
          {%- if block.settings.link != blank -%}{%- assign tag_open = 'a' -%}{%- endif -%}
          <{{ tag_open }}
            {% if block.settings.link != blank %}href="{{ block.settings.link }}" target="_blank" rel="noopener"{% endif %}
            class="nanovit-home-marketplaces__logo-wrapper"
            {{ block.shopify_attributes }}>
            {%- if block.settings.logo -%}
              {{ block.settings.logo | image_url: width: 220 | image_tag: loading: 'lazy', alt: block.settings.name, class: 'nanovit-home-marketplaces__logo' }}
            {%- endif -%}
          </{{ tag_open }}>
        {%- endfor -%}
      </div>
    {%- endif -%}

    {%- if section.settings.show_trust and trust_blocks.size > 0 -%}
      <div class="nanovit-home-marketplaces__trust">
        {%- for block in trust_blocks -%}
          <div class="nanovit-home-marketplaces__trust-item" {{ block.shopify_attributes }}>
            {% render 'icon', icon: block.settings.icon %}
            <span>{{ block.settings.label }}</span>
          </div>
        {%- endfor -%}
      </div>
    {%- endif -%}
  </div>
</section>
```

### CSS

```css
.nanovit-home-marketplaces { margin-top: 64px; }
.nanovit-home-marketplaces__inner {
  background: var(--nanovit-color-cream-2);
  border-radius: var(--nanovit-radius-xl);
  padding: 72px 56px;
  text-align: center;
}
.nanovit-home-marketplaces__eyebrow {
  font-family: var(--font-heading--family);
  font-weight: 600; font-size: 12px;
  letter-spacing: 0.18em; text-transform: uppercase;
  color: var(--color-primary); margin-bottom: 14px;
}
.nanovit-home-marketplaces__title {
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 24px;
  line-height: 1.1; letter-spacing: -0.02em;
  color: var(--color-foreground);
  margin-bottom: 8px; text-wrap: balance;
}
.nanovit-home-marketplaces__body {
  font-size: 14px; line-height: 1.55;
  color: var(--nanovit-color-mute);
  max-width: 520px; margin: 0 auto 48px;
}
.nanovit-home-marketplaces__logos {
  display: flex; align-items: center; justify-content: space-around;
  gap: 32px; flex-wrap: wrap; margin-bottom: 48px;
}
.nanovit-home-marketplaces__logo {
  max-height: 44px; width: auto; object-fit: contain;
}
.nanovit-home-marketplaces__trust {
  display: flex; align-items: center; justify-content: center;
  gap: 48px; flex-wrap: wrap;
  padding-top: 32px;
  border-top: 1px solid rgba(42,28,26,0.08);
}
.nanovit-home-marketplaces__trust-item {
  display: flex; align-items: center; gap: 10px;
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 12px;
  letter-spacing: 0.1em; text-transform: uppercase;
  color: var(--color-foreground);
}
.nanovit-home-marketplaces__trust-item svg {
  width: 22px; height: 22px; color: var(--color-primary); flex-shrink: 0;
}
@media (max-width: 749px) {
  .nanovit-home-marketplaces__inner { padding: 48px 24px; }
  .nanovit-home-marketplaces__logos { gap: 20px; }
  .nanovit-home-marketplaces__trust { gap: 24px; }
}
```

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendência cliente
- Subir 5+ logos de marketplaces no theme editor (PNG/SVG; preferir PNG transparente; SVG monocromático também serve).

## ❌ Não fazer
- Não hardcoded logos.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
