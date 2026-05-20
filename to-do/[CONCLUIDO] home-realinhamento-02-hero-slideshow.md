# Brief 02 — Hero slideshow

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-hero.liquid` (substitui o `nanovit-hero.liquid` atual).
**Referência mock:** `index.html` linhas 2495-2511.

**O que é:** Banner full-bleed dentro do container, com 1 a 5 slides em loop. Cada slide é uma imagem grande (16:7 aspect ratio aprox). Tem dots de paginação centralizados na base e setas anterior/próximo nos cantos.

**Por quê fiel ao mock:** O hero atual (2 colunas com foto do frasco) é meu erro de direção. O mock usa banner editorial slideshow — formato padrão de e-commerce mass-market BR (Granado, Risqué, suplementos em geral).

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado

### Estrutura visual
- Wrapper `.hero` dentro de `container` (1320px).
- `.hero__inner` com `position: relative`, `border-radius: var(--nanovit-radius-xl)` (22px), `overflow: hidden`.
- Cada slide: `<img>` `aspect-ratio: 16/7`, `object-fit: cover`, `width: 100%`.
- Dots: posicionados `bottom: 24px`, centralizados; 8px círculos com gap 8px; ativo em Nano Red.
- Setas: posicionadas nos cantos esquerdo/direito (vertical center), circulares 48px, fundo `rgba(255,255,255,0.92)`, ícone cocoa.
- Link opcional no slide inteiro (URL via setting).

### Schema (zero hardcode)

```json
{
  "name": "Hero · Slideshow",
  "class": "nanovit-home-hero-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "max_blocks": 5,
  "settings": [
    {
      "type": "header",
      "content": "Comportamento"
    },
    {
      "type": "checkbox",
      "id": "autoplay",
      "label": "Reproduzir automaticamente",
      "default": true
    },
    {
      "type": "range",
      "id": "autoplay_speed",
      "label": "Tempo por slide",
      "min": 3, "max": 10, "step": 1, "unit": "s",
      "default": 6
    },
    {
      "type": "checkbox",
      "id": "show_arrows",
      "label": "Mostrar setas",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_dots",
      "label": "Mostrar paginação (dots)",
      "default": true
    }
  ],
  "blocks": [
    {
      "type": "slide",
      "name": "Slide",
      "settings": [
        {
          "type": "image_picker",
          "id": "image",
          "label": "Imagem (desktop)",
          "info": "Recomendado 2600×1140px. Será cortada em 16:7 no desktop."
        },
        {
          "type": "image_picker",
          "id": "image_mobile",
          "label": "Imagem (mobile, opcional)",
          "info": "Recomendado 750×900px. Se vazio, usa a imagem desktop."
        },
        {
          "type": "text",
          "id": "alt",
          "label": "Texto alternativo (acessibilidade)",
          "info": "Descreva a imagem brevemente."
        },
        {
          "type": "url",
          "id": "link",
          "label": "Link do slide",
          "info": "Toda a imagem fica clicável se preenchido."
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Hero Nanovit",
      "category": "Home Nanovit",
      "blocks": [
        { "type": "slide" },
        { "type": "slide" },
        { "type": "slide" }
      ]
    }
  ]
}
```

### Liquid (esqueleto referencial)

```liquid
{%- liquid
  assign autoplay = section.settings.autoplay
  assign speed = section.settings.autoplay_speed | times: 1000
  assign show_arrows = section.settings.show_arrows
  assign show_dots = section.settings.show_dots
-%}

<section class="nanovit-home-hero container" aria-label="Destaques">
  <div
    class="nanovit-home-hero__inner"
    data-autoplay="{{ autoplay }}"
    data-speed="{{ speed }}"
  >
    {%- for block in section.blocks -%}
      {%- assign img = block.settings.image -%}
      {%- assign img_mobile = block.settings.image_mobile -%}
      {%- assign link = block.settings.link -%}
      {%- assign alt_text = block.settings.alt | default: img.alt | default: shop.name -%}
      <div
        class="nanovit-home-hero__slide{% if forloop.first %} is-active{% endif %}"
        {{ block.shopify_attributes }}
        role="group"
        aria-roledescription="slide"
        aria-label="Slide {{ forloop.index }} de {{ section.blocks.size }}"
      >
        {%- if link != blank -%}<a href="{{ link }}" class="nanovit-home-hero__link" aria-label="{{ alt_text }}">{%- endif -%}
          {%- if img != blank -%}
            <picture>
              {%- if img_mobile != blank -%}
                <source media="(max-width: 749px)" srcset="{{ img_mobile | image_url: width: 750 }} 750w, {{ img_mobile | image_url: width: 1000 }} 1000w">
              {%- endif -%}
              <img
                src="{{ img | image_url: width: 2600 }}"
                srcset="{{ img | image_url: width: 1200 }} 1200w, {{ img | image_url: width: 2000 }} 2000w, {{ img | image_url: width: 2600 }} 2600w"
                sizes="(max-width: 1320px) 100vw, 1320px"
                alt="{{ alt_text | escape }}"
                width="{{ img.width }}"
                height="{{ img.height }}"
                loading="{% if forloop.first %}eager{% else %}lazy{% endif %}"
                fetchpriority="{% if forloop.first %}high{% else %}auto{% endif %}"
              >
            </picture>
          {%- else -%}
            <div class="nanovit-home-hero__placeholder">{{ 'home.hero.placeholder' | t }}</div>
          {%- endif -%}
        {%- if link != blank -%}</a>{%- endif -%}
      </div>
    {%- endfor -%}

    {%- if show_dots and section.blocks.size > 1 -%}
      <div class="nanovit-home-hero__dots" role="tablist" aria-label="Selecionar slide">
        {%- for block in section.blocks -%}
          <button
            type="button"
            class="nanovit-home-hero__dot{% if forloop.first %} is-active{% endif %}"
            role="tab"
            aria-selected="{% if forloop.first %}true{% else %}false{% endif %}"
            aria-controls="slide-{{ forloop.index0 }}"
            aria-label="Ir para slide {{ forloop.index }}"
            data-index="{{ forloop.index0 }}"
          ></button>
        {%- endfor -%}
      </div>
    {%- endif -%}

    {%- if show_arrows and section.blocks.size > 1 -%}
      <button type="button" class="nanovit-home-hero__arrow nanovit-home-hero__arrow--prev" aria-label="Slide anterior">
        {% render 'icon', icon: 'chevron-left' %}
      </button>
      <button type="button" class="nanovit-home-hero__arrow nanovit-home-hero__arrow--next" aria-label="Próximo slide">
        {% render 'icon', icon: 'chevron-right' %}
      </button>
    {%- endif -%}
  </div>
</section>

{% stylesheet %}
  .nanovit-home-hero { margin-top: 16px; }
  .nanovit-home-hero__inner {
    position: relative;
    border-radius: var(--nanovit-radius-xl);
    overflow: hidden;
    aspect-ratio: 16 / 7;
  }
  .nanovit-home-hero__slide { position: absolute; inset: 0; opacity: 0; transition: opacity 0.5s ease; }
  .nanovit-home-hero__slide.is-active { opacity: 1; z-index: 1; }
  .nanovit-home-hero__slide img,
  .nanovit-home-hero__link { width: 100%; height: 100%; object-fit: cover; display: block; }
  .nanovit-home-hero__dots {
    position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%);
    display: flex; gap: 8px; z-index: 2;
  }
  .nanovit-home-hero__dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: rgba(255,255,255,0.5); border: 0; padding: 0; cursor: pointer;
    transition: background 0.2s, transform 0.2s;
  }
  .nanovit-home-hero__dot.is-active { background: var(--color-primary); transform: scale(1.25); }
  .nanovit-home-hero__arrow {
    position: absolute; top: 50%; transform: translateY(-50%);
    width: 48px; height: 48px; border-radius: 50%;
    background: rgba(255,255,255,0.92); border: 0; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    color: var(--color-foreground); z-index: 2;
    backdrop-filter: blur(8px);
  }
  .nanovit-home-hero__arrow--prev { left: 24px; }
  .nanovit-home-hero__arrow--next { right: 24px; }
  .nanovit-home-hero__placeholder {
    width: 100%; height: 100%;
    display: flex; align-items: center; justify-content: center;
    background: var(--nanovit-color-cream);
    color: var(--nanovit-color-mute);
    font-family: var(--font-heading--family);
  }
  @media (max-width: 749px) {
    .nanovit-home-hero__inner { aspect-ratio: 4 / 5; }
    .nanovit-home-hero__arrow { display: none; }
  }
{% endstylesheet %}

{% javascript %}
  // Carrossel: troca de slide via dots + setas + autoplay opcional.
  // Implementação curta, sem libs. Pausa em hover.
  class NanovitHero extends HTMLElement {
    connectedCallback() {
      this.slides = this.querySelectorAll('.nanovit-home-hero__slide');
      this.dots = this.querySelectorAll('.nanovit-home-hero__dot');
      this.prev = this.querySelector('.nanovit-home-hero__arrow--prev');
      this.next = this.querySelector('.nanovit-home-hero__arrow--next');
      this.index = 0;
      this.autoplay = this.dataset.autoplay === 'true';
      this.speed = parseInt(this.dataset.speed, 10) || 6000;
      this.bind();
      if (this.autoplay) this.start();
    }
    bind() {
      this.dots.forEach((d, i) => d.addEventListener('click', () => this.goto(i)));
      if (this.prev) this.prev.addEventListener('click', () => this.goto(this.index - 1));
      if (this.next) this.next.addEventListener('click', () => this.goto(this.index + 1));
      this.addEventListener('mouseenter', () => this.stop());
      this.addEventListener('mouseleave', () => { if (this.autoplay) this.start(); });
    }
    goto(i) {
      const total = this.slides.length;
      this.index = ((i % total) + total) % total;
      this.slides.forEach((s, k) => s.classList.toggle('is-active', k === this.index));
      this.dots.forEach((d, k) => {
        d.classList.toggle('is-active', k === this.index);
        d.setAttribute('aria-selected', k === this.index);
      });
    }
    start() { this.stop(); this.timer = setInterval(() => this.goto(this.index + 1), this.speed); }
    stop() { if (this.timer) clearInterval(this.timer); }
  }
  customElements.define('nanovit-hero', NanovitHero);
{% endjavascript %}
```

(Ajustar wrapper pra `<nanovit-hero>` custom element ao invés de `<div>` com class, ou manter como `<div>` e inicializar via querySelector. Escolha do dev.)

**Notas do Dev:** Implementado conforme spec.

---

## 🚧 Pendências cliente
- Subir 3 a 5 imagens hero no admin via theme editor após o tema publicar. Sem imagens, o hero mostra placeholder cream.

## ❌ Não fazer
- Não colocar imagens hardcoded no Liquid. Toda imagem vem de `block.settings.image`.
- Não usar travessão em copy. Não usar texto hardcoded em qualquer string visível (tudo via `settings.text` ou via `locales/pt-BR.json` quando for UI fixa).

## 📝 Log do Dev

- **Resumo:** `sections/nanovit-home-hero.liquid` reescrita do zero como slideshow. Custom element `<nanovit-hero>` (não `<div>` com class) registrado via `customElements.define` com guard pra HMR. Estrutura: `.container` → `<nanovit-hero>` → slides (`<picture>` com `source` mobile + `<img>` desktop com srcset 1200/2000/2600), dots tablist com aria, setas 48px com chevron-left/right inline (em vez de `{% render 'icon' %}` pra não depender de snippet). JS: navegação via dots/setas/teclas ArrowLeft-ArrowRight, autoplay opcional com pausa em hover e focus-within, cleanup no `disconnectedCallback`. Schema fiel ao brief (max_blocks 5, settings autoplay/speed/arrows/dots, blocks `slide` com image desktop/mobile/alt/link, preset "Hero Nanovit" com 3 slides vazios em category "Home Nanovit"). Mobile: aspect-ratio muda pra 4/5, setas escondidas.

- **Decisões fora do brief:**
  - **Custom element em vez de `<div>`** (escolha permitida pelo brief). Mais limpo: instancia no `connectedCallback`, dá cleanup automático no `disconnectedCallback` quando o theme editor re-renderiza a section.
  - **Chevron SVG inline** em vez de `{% render 'icon' %}` porque o snippet de ícones do Horizon não está garantido. Inline funciona sempre.
  - **Guard `if (!customElements.get(...))`** pra suportar HMR/reload do theme editor sem erro de "already defined".
  - **Empty state separado** (quando blocks.size == 0) mostra placeholder cream com instrução pro admin. Quando há blocks mas sem imagem, cada slide mostra seu próprio placeholder.
  - **`fetchpriority="high"`** no primeiro slide, `loading="eager"` também só no primeiro (LCP do hero).

- **Dúvidas:** Nenhuma.

- **Tempo:** ~25 min (cleanup + hero).
