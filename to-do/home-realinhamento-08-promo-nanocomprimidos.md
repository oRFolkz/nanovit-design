# Brief 08 — Promo Strip Nanocomprimidos (full-bleed vermelho)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-promo-strip.liquid`
**Referência mock:** `index.html` linhas 3023-3047.

**O que é:** Faixa horizontal **full-bleed** (estoura o container, sangra até as bordas da viewport) em Nano Red sólido com glow circular como decoração. Conteúdo: 3 "comprimidos" decorativos à esquerda + eyebrow técnico + título + CTA pill branco.

**Por quê full-bleed:** Promove a tecnologia "Nanocomprimidos" como diferencial. Full-bleed em vermelho rompe o ritmo cream/branco da home e marca atenção.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Estrutura
- Section `<section>` com `margin: 64px calc(-1 * clamp(16px, 3vw, 32px)) 0;` pra estourar o padding do `<body>`.
- Background `var(--color-primary)`, padding `32px clamp(20px, 4vw, 56px)`, color `#fff`.
- 4 glows decorativos (`<span aria-hidden>`) com `position: absolute; filter: blur(60px); border-radius: 50%; background: rgba(255,255,255,0.18)` em opacidades e posições diferentes (replicar do mock).
- Inner: `max-width: var(--nanovit-container-max); margin: 0 auto; display: flex; gap: 28px; align-items: center; position: relative; z-index: 1`.
- Visual à esquerda: 3 círculos 64px sobrepostos como "comprimidos" (gradients claros).
- Copy meio: eyebrow uppercase letter-spacing alto + título 26px peso 700.
- CTA direita: pill branco com texto Nano Red.

### Schema

```json
{
  "name": "Promo · Nanocomprimidos",
  "class": "nanovit-home-promo-strip-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "settings": [
    {
      "type": "text",
      "id": "eyebrow",
      "label": "Eyebrow (uppercase)",
      "default": "Tecnologia exclusiva Nanovit"
    },
    {
      "type": "text",
      "id": "title",
      "label": "Título",
      "default": "Nanocomprimidos com até 5× mais absorção"
    },
    {
      "type": "text",
      "id": "cta_label",
      "label": "Texto do CTA",
      "default": "Conheça a tecnologia"
    },
    {
      "type": "url",
      "id": "cta_url",
      "label": "Link do CTA",
      "info": "Aponte para a página /pages/tecnologia."
    },
    {
      "type": "checkbox",
      "id": "show_decorative_pills",
      "label": "Mostrar comprimidos decorativos",
      "default": true
    }
  ],
  "presets": [
    {
      "name": "Promo Nanocomprimidos",
      "category": "Home Nanovit"
    }
  ]
}
```

### Liquid

```liquid
<section class="nanovit-home-promo-strip" aria-label="{{ section.settings.eyebrow }}">
  <span class="nanovit-home-promo-strip__glow nanovit-home-promo-strip__glow--1" aria-hidden="true"></span>
  <span class="nanovit-home-promo-strip__glow nanovit-home-promo-strip__glow--2" aria-hidden="true"></span>
  <span class="nanovit-home-promo-strip__glow nanovit-home-promo-strip__glow--3" aria-hidden="true"></span>
  <span class="nanovit-home-promo-strip__glow nanovit-home-promo-strip__glow--4" aria-hidden="true"></span>

  <div class="nanovit-home-promo-strip__inner">
    {%- if section.settings.show_decorative_pills -%}
      <div class="nanovit-home-promo-strip__pills" aria-hidden="true">
        <span class="nanovit-home-promo-strip__pill nanovit-home-promo-strip__pill--1"></span>
        <span class="nanovit-home-promo-strip__pill nanovit-home-promo-strip__pill--2"></span>
        <span class="nanovit-home-promo-strip__pill nanovit-home-promo-strip__pill--3"></span>
      </div>
    {%- endif -%}

    <div class="nanovit-home-promo-strip__copy">
      <p class="nanovit-home-promo-strip__eyebrow">{{ section.settings.eyebrow }}</p>
      <h3 class="nanovit-home-promo-strip__title">{{ section.settings.title }}</h3>
    </div>

    {%- if section.settings.cta_url and section.settings.cta_label != blank -%}
      <a href="{{ section.settings.cta_url }}" class="nanovit-home-promo-strip__cta">
        <span>{{ section.settings.cta_label }}</span>
        {% render 'icon', icon: 'arrow-right' %}
      </a>
    {%- endif -%}
  </div>
</section>
```

### CSS

```css
.nanovit-home-promo-strip {
  position: relative;
  margin: 64px calc(-1 * var(--nanovit-container-padding)) 0;
  padding: 32px clamp(20px, 4vw, 56px);
  background: var(--color-primary);
  color: #fff;
  overflow: hidden;
}
.nanovit-home-promo-strip__glow {
  position: absolute; border-radius: 50%;
  filter: blur(60px); pointer-events: none;
  background: rgba(255,255,255,0.18);
}
.nanovit-home-promo-strip__glow--1 { top: -160px; left: 8%;  width: 340px; height: 340px; }
.nanovit-home-promo-strip__glow--2 { bottom: -180px; right: 12%; width: 400px; height: 400px; background: rgba(255,200,190,0.28); filter: blur(70px); }
.nanovit-home-promo-strip__glow--3 { top: -120px; left: 38%; width: 280px; height: 280px; background: rgba(255,255,255,0.14); filter: blur(55px); }
.nanovit-home-promo-strip__glow--4 { bottom: -140px; left: 4%;  width: 260px; height: 260px; background: rgba(255,180,170,0.22); filter: blur(55px); }
.nanovit-home-promo-strip__inner {
  position: relative; z-index: 1;
  max-width: var(--nanovit-container-max);
  margin: 0 auto;
  display: flex; align-items: center; gap: 28px;
}
.nanovit-home-promo-strip__pills {
  position: relative; width: 150px; height: 64px; flex-shrink: 0;
}
.nanovit-home-promo-strip__pill {
  position: absolute; width: 64px; height: 64px; border-radius: 50%;
  box-shadow: inset 0 -6px 14px rgba(0,0,0,0.18), 0 4px 12px rgba(0,0,0,0.18);
}
.nanovit-home-promo-strip__pill--1 { left: 0;  background: linear-gradient(160deg,#fff,#ffd6d0); }
.nanovit-home-promo-strip__pill--2 { left: 38px; background: linear-gradient(160deg,#ffe9e5,#ff9e93); }
.nanovit-home-promo-strip__pill--3 { left: 76px; background: linear-gradient(160deg,#fff,#ffb8b0); }
.nanovit-home-promo-strip__copy { flex: 1; min-width: 0; }
.nanovit-home-promo-strip__eyebrow {
  font-family: var(--font-heading--family);
  font-weight: 600; font-size: 12px;
  letter-spacing: 0.18em; text-transform: uppercase;
  opacity: 0.85; margin-bottom: 6px;
}
.nanovit-home-promo-strip__title {
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 26px;
  letter-spacing: -0.01em; line-height: 1.2;
}
.nanovit-home-promo-strip__cta {
  background: #fff; color: var(--color-primary);
  font-family: var(--font-heading--family);
  font-weight: 700; font-size: 14px;
  padding: 14px 24px; border-radius: var(--nanovit-radius-pill);
  display: inline-flex; align-items: center; gap: 8px;
  text-decoration: none; flex-shrink: 0;
}
@media (max-width: 749px) {
  .nanovit-home-promo-strip__inner { flex-direction: column; align-items: flex-start; gap: 18px; }
  .nanovit-home-promo-strip__title { font-size: 20px; }
}
```

**Notas do Dev:** > _(preencher após executar)_

---

## ❌ Não fazer
- Não hardcoded título/eyebrow. Tudo via `setting.text`.
- Não usar travessão.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
