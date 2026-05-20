# Brief 09 — Médicos parceiros (banner rotativo)

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-medicos.liquid`
**Referência mock:** `index.html` linhas 3049-3069.

**O que é:** Banner horizontal cream (em container) com avatar circular de médico à esquerda, quote grande no meio, paginação "1/5" + setas à direita. Cada slide é um médico/profissional diferente que indica a marca.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Estrutura
- Section container.
- Inner: `display: grid; grid-template-columns: auto 1fr auto; gap: 28px; align-items: center` em card cream radius xl, padding 36px.
- Avatar: foto circular 96-120px com label nome embaixo (compacto).
- Quote: eyebrow "INDICADO POR ESPECIALISTAS" + quote em peso 500 itálico tamanho 18-20px + autor (nome + credencial) em mute.
- Paginação: "1 / N" texto micro + 2 setas circulares 44px cocoa-outline.

### Schema

```json
{
  "name": "Médicos · Banner",
  "class": "nanovit-home-medicos-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "max_blocks": 10,
  "settings": [
    {
      "type": "text",
      "id": "eyebrow",
      "label": "Eyebrow",
      "default": "Indicado por especialistas"
    },
    {
      "type": "checkbox",
      "id": "autoplay",
      "label": "Rotação automática",
      "default": true
    },
    {
      "type": "range",
      "id": "autoplay_speed",
      "label": "Tempo por slide",
      "min": 4, "max": 12, "step": 1, "unit": "s",
      "default": 7
    }
  ],
  "blocks": [
    {
      "type": "medico",
      "name": "Médico",
      "settings": [
        {
          "type": "image_picker",
          "id": "avatar",
          "label": "Foto (avatar circular)",
          "info": "Recomendado 500×500px, quadrado."
        },
        {
          "type": "text",
          "id": "name",
          "label": "Nome curto (label do avatar)",
          "info": "Aparece embaixo do avatar. Ex: Dr. Barakat",
          "default": "Dr. Nome"
        },
        {
          "type": "richtext",
          "id": "quote",
          "label": "Quote",
          "default": "<p>\"Texto da indicação do profissional sobre a marca.\"</p>"
        },
        {
          "type": "text",
          "id": "author",
          "label": "Autor (nome completo + credencial)",
          "info": "Ex: Dr. Victor Barakat — Médico Funcional Integrativo. (NÃO usar travessão; usar vírgula ou ponto.)",
          "default": "Dr. Nome Completo, Especialidade"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Médicos parceiros",
      "category": "Home Nanovit",
      "blocks": [
        { "type": "medico" },
        { "type": "medico" },
        { "type": "medico" },
        { "type": "medico" },
        { "type": "medico" }
      ]
    }
  ]
}
```

### Liquid

```liquid
{%- assign total = section.blocks.size -%}

<section class="nanovit-home-medicos container" aria-label="Médicos parceiros">
  <div class="nanovit-home-medicos__inner"
       data-autoplay="{{ section.settings.autoplay }}"
       data-speed="{{ section.settings.autoplay_speed | times: 1000 }}">
    {%- for block in section.blocks -%}
      <article class="nanovit-home-medicos__slide{% if forloop.first %} is-active{% endif %}"
               {{ block.shopify_attributes }}
               role="group"
               aria-roledescription="slide"
               aria-label="Profissional {{ forloop.index }} de {{ total }}">
        <div class="nanovit-home-medicos__avatar">
          {%- if block.settings.avatar -%}
            {{ block.settings.avatar | image_url: width: 240 | image_tag: loading: 'lazy', alt: block.settings.name, widths: '120,240' }}
          {%- endif -%}
          <span class="nanovit-home-medicos__avatar-label">{{ block.settings.name }}</span>
        </div>
        <div class="nanovit-home-medicos__quote-block">
          <p class="nanovit-home-medicos__quote-eyebrow">{{ section.settings.eyebrow }}</p>
          <div class="nanovit-home-medicos__quote">{{ block.settings.quote }}</div>
          <p class="nanovit-home-medicos__author">{{ block.settings.author }}</p>
        </div>
      </article>
    {%- endfor -%}

    {%- if total > 1 -%}
      <div class="nanovit-home-medicos__nav">
        <span class="nanovit-home-medicos__pagination"><span data-current>1</span> / {{ total }}</span>
        <div class="nanovit-home-medicos__arrows">
          <button type="button" class="nanovit-home-medicos__arrow" data-dir="prev" aria-label="Anterior">{% render 'icon', icon: 'chevron-left' %}</button>
          <button type="button" class="nanovit-home-medicos__arrow" data-dir="next" aria-label="Próximo">{% render 'icon', icon: 'chevron-right' %}</button>
        </div>
      </div>
    {%- endif -%}
  </div>
</section>
```

### CSS (essencial)

```css
.nanovit-home-medicos { margin-top: 64px; }
.nanovit-home-medicos__inner {
  position: relative;
  background: var(--nanovit-color-cream-2);
  border-radius: var(--nanovit-radius-xl);
  padding: 36px;
  min-height: 200px;
}
.nanovit-home-medicos__slide {
  display: none;
  grid-template-columns: auto 1fr auto;
  gap: 28px; align-items: center;
}
.nanovit-home-medicos__slide.is-active { display: grid; }
.nanovit-home-medicos__avatar {
  display: flex; flex-direction: column; align-items: center; gap: 8px;
}
.nanovit-home-medicos__avatar img {
  width: 96px; height: 96px; border-radius: 50%; object-fit: cover;
}
.nanovit-home-medicos__avatar-label {
  font-family: var(--font-heading--family);
  font-weight: 600; font-size: 12px;
  color: var(--color-foreground);
}
.nanovit-home-medicos__quote-eyebrow {
  font-family: var(--font-heading--family);
  font-weight: 600; font-size: 11px;
  letter-spacing: 0.14em; text-transform: uppercase;
  color: var(--color-primary); margin-bottom: 8px;
}
.nanovit-home-medicos__quote {
  font-size: 18px; line-height: 1.4;
  color: var(--color-foreground); font-style: italic;
  margin-bottom: 12px;
}
.nanovit-home-medicos__author {
  font-size: 13px; color: var(--nanovit-color-mute);
}
.nanovit-home-medicos__nav {
  display: flex; align-items: center; gap: 16px;
}
.nanovit-home-medicos__pagination {
  font-family: var(--font-heading--family);
  font-weight: 600; font-size: 13px;
  color: var(--nanovit-color-mute);
}
.nanovit-home-medicos__arrow {
  width: 44px; height: 44px; border-radius: 50%;
  border: 1.5px solid rgba(42,28,26,0.18);
  background: transparent; color: var(--color-foreground);
  display: inline-flex; align-items: center; justify-content: center;
  cursor: pointer;
}
@media (max-width: 749px) {
  .nanovit-home-medicos__slide { grid-template-columns: none; gap: 16px; text-align: center; }
  .nanovit-home-medicos__nav { justify-content: center; }
}
```

JS: lógica simples de troca de slide (pode reaproveitar o padrão do `nanovit-home-hero.liquid` brief 02).

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendência cliente
- Subir 3-5 fotos circulares de médicos parceiros no theme editor, com nomes/credenciais reais.

## ❌ Não fazer
- Não usar travessão (—) em author. Usar vírgula.
- Não hardcoded nenhum dado de médico.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
