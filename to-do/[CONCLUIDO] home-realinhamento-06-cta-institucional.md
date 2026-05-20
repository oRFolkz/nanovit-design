# Brief 06 — CTA Institucional (banner 2 colunas)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-cta-banner.liquid`
**Referência mock:** `index.html` linhas 2794-2812.

**O que é:** Banner editorial 2 colunas dentro do container. Esquerda: copy + 2 CTAs (primary + secondary). Direita: imagem lifestyle. Fundo `var(--nanovit-color-cream-2)`, radius xl (22px), `min-height: 420px`.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Layout
- Grid `1fr 1fr` desktop, empilha mobile.
- Esquerda: padding 56px (mobile 32px), `display: flex; flex-direction: column; justify-content: center`.
- Direita: imagem `object-fit: cover; width: 100%; height: 100%`.

### Schema

```json
{
  "name": "CTA Banner",
  "class": "nanovit-home-cta-banner-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "settings": [
    {
      "type": "header",
      "content": "Conteúdo"
    },
    {
      "type": "richtext",
      "id": "title",
      "label": "Título",
      "info": "Use <em> para destacar palavras em vermelho.",
      "default": "<p>Nanovit: <em>suplementos de alta qualidade</em> para saúde e performance</p>"
    },
    {
      "type": "richtext",
      "id": "subtitle",
      "label": "Subtítulo",
      "default": "<p>Para quem entende que cuidar do corpo é priorizar o presente. A Nanovit nasceu com a missão de unir ciência funcional, ingredientes premium e clareza nos rótulos. Cada fórmula é desenvolvida para entregar resultado real, sem promessas vazias.</p>"
    },
    {
      "type": "header",
      "content": "CTA primário"
    },
    {
      "type": "text",
      "id": "cta_primary_label",
      "label": "Texto",
      "default": "Ver produtos"
    },
    {
      "type": "url",
      "id": "cta_primary_url",
      "label": "Link"
    },
    {
      "type": "header",
      "content": "CTA secundário"
    },
    {
      "type": "text",
      "id": "cta_secondary_label",
      "label": "Texto",
      "default": "Sobre a marca"
    },
    {
      "type": "url",
      "id": "cta_secondary_url",
      "label": "Link"
    },
    {
      "type": "header",
      "content": "Visual"
    },
    {
      "type": "image_picker",
      "id": "image",
      "label": "Imagem",
      "info": "Recomendado 800×800px ou maior, formato quadrado/retrato."
    },
    {
      "type": "select",
      "id": "image_position",
      "label": "Posição da imagem",
      "options": [
        { "value": "right", "label": "Direita" },
        { "value": "left", "label": "Esquerda" }
      ],
      "default": "right"
    }
  ],
  "presets": [
    {
      "name": "CTA Institucional",
      "category": "Home Nanovit"
    }
  ]
}
```

### Liquid

```liquid
{%- liquid
  assign img = section.settings.image
  assign pos = section.settings.image_position
-%}

<section class="nanovit-home-cta-banner container" aria-label="{{ 'sections.cta_banner' | t }}">
  <div class="nanovit-home-cta-banner__inner nanovit-home-cta-banner__inner--{{ pos }}">
    <div class="nanovit-home-cta-banner__copy">
      <h3 class="nanovit-home-cta-banner__title">{{ section.settings.title }}</h3>
      <div class="nanovit-home-cta-banner__sub">{{ section.settings.subtitle }}</div>
      <div class="nanovit-home-cta-banner__actions">
        {%- if section.settings.cta_primary_url and section.settings.cta_primary_label != blank -%}
          <a href="{{ section.settings.cta_primary_url }}" class="button button--primary">
            <span>{{ section.settings.cta_primary_label }}</span>
            {% render 'icon', icon: 'arrow-right' %}
          </a>
        {%- endif -%}
        {%- if section.settings.cta_secondary_url and section.settings.cta_secondary_label != blank -%}
          <a href="{{ section.settings.cta_secondary_url }}" class="button button--secondary">
            {{ section.settings.cta_secondary_label }}
          </a>
        {%- endif -%}
      </div>
    </div>
    <div class="nanovit-home-cta-banner__visual">
      {%- if img -%}
        {{ img | image_url: width: 1200 | image_tag: loading: 'lazy', alt: img.alt | default: shop.name, widths: '600,900,1200' }}
      {%- else -%}
        {{ 'lifestyle-1' | placeholder_svg_tag: 'placeholder-svg' }}
      {%- endif -%}
    </div>
  </div>
</section>
```

### CSS

```css
.nanovit-home-cta-banner { margin-top: 64px; }
.nanovit-home-cta-banner__inner {
  background: var(--nanovit-color-cream-2);
  border-radius: var(--nanovit-radius-xl);
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: stretch;
  overflow: hidden;
  min-height: 420px;
}
.nanovit-home-cta-banner__inner--left { direction: rtl; }
.nanovit-home-cta-banner__inner--left > * { direction: ltr; }
.nanovit-home-cta-banner__copy {
  padding: 56px;
  display: flex; flex-direction: column; justify-content: center;
}
.nanovit-home-cta-banner__title {
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 24px; line-height: 1.1;
  letter-spacing: -0.02em; color: var(--color-foreground);
  margin-bottom: 8px; text-wrap: balance;
}
.nanovit-home-cta-banner__title em {
  font-style: normal; color: var(--color-primary);
}
.nanovit-home-cta-banner__sub {
  font-size: 14px; color: var(--nanovit-color-mute);
  line-height: 1.55; max-width: 520px; margin-bottom: 22px;
}
.nanovit-home-cta-banner__actions { display: flex; gap: 10px; flex-wrap: wrap; }
.nanovit-home-cta-banner__visual {
  position: relative; overflow: hidden;
}
.nanovit-home-cta-banner__visual img {
  width: 100%; height: 100%; object-fit: cover; display: block;
}
@media (max-width: 749px) {
  .nanovit-home-cta-banner__inner { grid-template-columns: none; }
  .nanovit-home-cta-banner__copy { padding: 32px; }
  .nanovit-home-cta-banner__visual { aspect-ratio: 4 / 3; }
}
```

**Notas do Dev:** > _(preencher após executar)_

---

## ❌ Não fazer
- Não hardcoded foto, texto ou URL. Tudo via `settings`.
- Não usar travessão.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
