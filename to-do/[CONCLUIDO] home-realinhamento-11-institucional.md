# Brief 11 — Banner Institucional (full-width com overlay)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-institucional.liquid`
**Referência mock:** `index.html` linhas 3173-3188.

**O que é:** Banner editorial grande dentro do container, `aspect-ratio: 16/7`, `min-height: 420px`, com imagem de fundo + overlay gradient escuro à esquerda + copy branca sobreposta (pill eyebrow vermelho + título + parágrafo + CTA pill branco).

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Estrutura
- Wrapper `position: relative; border-radius: var(--nanovit-radius-xl); overflow: hidden`.
- Imagem absolute inset-0, `object-fit: cover`.
- Overlay absolute inset-0 com `background: linear-gradient(180deg, rgba(0,0,0,0.55) 0%, rgba(0,0,0,0.15) 35%, rgba(0,0,0,0) 60%)` (vinheta superior). `pointer-events: none`.
- Copy: `position: absolute; top: 50%; left: 0; transform: translateY(-50%); padding: 0 clamp(32px, 5vw, 72px)`, `max-width: 720px`, `color: #fff`.

### Schema

```json
{
  "name": "Banner institucional",
  "class": "nanovit-home-institucional-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "settings": [
    {
      "type": "image_picker",
      "id": "image",
      "label": "Imagem de fundo",
      "info": "Recomendado 2400×1050px (16:7). Foco do assunto à direita pra não competir com a copy esquerda."
    },
    {
      "type": "text",
      "id": "image_alt",
      "label": "Descrição da imagem (alt)",
      "default": "Nanovit"
    },
    {
      "type": "text",
      "id": "eyebrow",
      "label": "Eyebrow (pill vermelho)",
      "default": "Nossa essência"
    },
    {
      "type": "richtext",
      "id": "title",
      "label": "Título",
      "default": "<p>Ciência funcional que cabe na sua rotina.</p>"
    },
    {
      "type": "richtext",
      "id": "body",
      "label": "Texto",
      "default": "<p>A Nanovit nasceu da inquietação de quem entende que cuidar do corpo é um ato presente. Fórmulas desenvolvidas com especialistas, ingredientes premium e total transparência nos rótulos, para entregar resultado real, sem promessas vazias.</p>"
    },
    {
      "type": "text",
      "id": "cta_label",
      "label": "Texto do CTA",
      "default": "Conheça a Nanovit"
    },
    {
      "type": "url",
      "id": "cta_url",
      "label": "Link do CTA"
    },
    {
      "type": "select",
      "id": "overlay_strength",
      "label": "Intensidade do overlay (gradient escuro)",
      "options": [
        { "value": "soft", "label": "Suave" },
        { "value": "medium", "label": "Médio" },
        { "value": "strong", "label": "Forte" }
      ],
      "default": "medium",
      "info": "Aumente se a imagem competir com a legibilidade do texto."
    }
  ],
  "presets": [
    {
      "name": "Banner institucional",
      "category": "Home Nanovit"
    }
  ]
}
```

### Liquid

```liquid
{%- liquid
  assign img = section.settings.image
  assign overlay = section.settings.overlay_strength | default: 'medium'
-%}

<section class="nanovit-home-institucional container" aria-label="{{ section.settings.eyebrow }}">
  <div class="nanovit-home-institucional__inner">
    {%- if img -%}
      <img
        class="nanovit-home-institucional__bg"
        src="{{ img | image_url: width: 2400 }}"
        srcset="{{ img | image_url: width: 1200 }} 1200w, {{ img | image_url: width: 1800 }} 1800w, {{ img | image_url: width: 2400 }} 2400w"
        sizes="(max-width: 1320px) 100vw, 1320px"
        alt="{{ section.settings.image_alt | default: shop.name | escape }}"
        loading="lazy"
      >
    {%- endif -%}

    <div class="nanovit-home-institucional__overlay nanovit-home-institucional__overlay--{{ overlay }}" aria-hidden="true"></div>

    <div class="nanovit-home-institucional__copy">
      {%- if section.settings.eyebrow != blank -%}
        <span class="nanovit-home-institucional__eyebrow">{{ section.settings.eyebrow }}</span>
      {%- endif -%}
      <h3 class="nanovit-home-institucional__title">{{ section.settings.title }}</h3>
      <div class="nanovit-home-institucional__body">{{ section.settings.body }}</div>
      {%- if section.settings.cta_url and section.settings.cta_label != blank -%}
        <a href="{{ section.settings.cta_url }}" class="nanovit-home-institucional__cta">
          <span>{{ section.settings.cta_label }}</span>
          {% render 'icon', icon: 'arrow-right' %}
        </a>
      {%- endif -%}
    </div>
  </div>
</section>
```

### CSS

```css
.nanovit-home-institucional { margin-top: 64px; }
.nanovit-home-institucional__inner {
  position: relative;
  border-radius: var(--nanovit-radius-xl);
  overflow: hidden;
  aspect-ratio: 16 / 7;
  min-height: 420px;
}
.nanovit-home-institucional__bg {
  position: absolute; inset: 0;
  width: 100%; height: 100%;
  object-fit: cover; display: block;
}
.nanovit-home-institucional__overlay { position: absolute; inset: 0; pointer-events: none; }
.nanovit-home-institucional__overlay--soft { background: linear-gradient(180deg, rgba(0,0,0,0.35) 0%, rgba(0,0,0,0.05) 40%, rgba(0,0,0,0) 70%); }
.nanovit-home-institucional__overlay--medium { background: linear-gradient(180deg, rgba(0,0,0,0.55) 0%, rgba(0,0,0,0.15) 35%, rgba(0,0,0,0) 60%); }
.nanovit-home-institucional__overlay--strong { background: linear-gradient(180deg, rgba(0,0,0,0.7) 0%, rgba(0,0,0,0.3) 35%, rgba(0,0,0,0.05) 70%); }
.nanovit-home-institucional__copy {
  position: absolute; top: 50%; left: 0;
  transform: translateY(-50%);
  padding: 0 clamp(32px, 5vw, 72px);
  color: #fff; max-width: 720px;
}
.nanovit-home-institucional__eyebrow {
  display: inline-block;
  font-family: var(--font-heading--family);
  font-weight: 600; font-size: 12px;
  letter-spacing: 0.18em; text-transform: uppercase;
  color: #fff;
  background: rgba(204,43,32,0.95);
  padding: 6px 14px;
  border-radius: var(--nanovit-radius-pill);
  margin-bottom: 18px;
}
.nanovit-home-institucional__title {
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 24px; line-height: 1.1;
  letter-spacing: -0.02em; color: #fff;
  margin-bottom: 8px; text-wrap: balance;
}
.nanovit-home-institucional__body {
  font-size: 14px; line-height: 1.55;
  color: rgba(255,255,255,0.92);
  max-width: 520px; margin-bottom: 24px;
}
.nanovit-home-institucional__cta {
  display: inline-flex; align-items: center; gap: 8px;
  background: #fff; color: var(--color-foreground);
  padding: 14px 26px; border-radius: var(--nanovit-radius-pill);
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 13px;
  text-decoration: none;
}
@media (max-width: 749px) {
  .nanovit-home-institucional__inner { aspect-ratio: 4 / 5; min-height: 360px; }
  .nanovit-home-institucional__title { font-size: 20px; }
  .nanovit-home-institucional__body { font-size: 13px; }
}
```

**Notas do Dev:** > _(preencher após executar)_

---

## ❌ Não fazer
- Não hardcoded foto, eyebrow, título, body, CTA. Tudo via `settings`.
- Não usar travessão em copy.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
