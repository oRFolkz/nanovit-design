# Brief 03 — Faixa de Benefícios

> Pré-req: `home-realinhamento-00-plano.md`.
**Section:** `sections/nanovit-home-beneficios.liquid`
**Referência mock:** `index.html` linhas 2513-2553 (grid `repeat(5, 1fr)` com items cream).

**O que é:** Faixa horizontal logo abaixo do hero com 3 a 6 benefícios curtos. Cada item é um card cream com ícone redondo + frase com palavra-chave destacada.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Layout
- `display: grid; grid-template-columns: repeat({{ blocks.size }}, 1fr); gap: 14px;` no desktop.
- Mobile: scroll horizontal com snap (`grid-auto-flow: column; grid-auto-columns: 80%; overflow-x: auto`).
- Cada item: fundo `var(--nanovit-color-cream-2)`, radius `var(--nanovit-radius-lg)` (14px), padding 16px 18px, layout `display: flex; align-items: center; gap: 14px`.
- Ícone: círculo 44px branco com borda 1px `rgba(42,28,26,0.06)`, ícone SVG cocoa 20px (Feather/Lucide style).
- Texto: 13px line-height 1.35, com `<strong>` opcional destacando palavra-chave.

### Schema

```json
{
  "name": "Faixa de benefícios",
  "class": "nanovit-home-beneficios-section",
  "tag": "section",
  "enabled_on": { "templates": ["index"] },
  "max_blocks": 6,
  "settings": [
    {
      "type": "paragraph",
      "content": "Cada benefício é um block. Mínimo 3, máximo 6. Mobile vira scroll horizontal."
    }
  ],
  "blocks": [
    {
      "type": "beneficio",
      "name": "Benefício",
      "settings": [
        {
          "type": "select",
          "id": "icon",
          "label": "Ícone",
          "options": [
            { "value": "truck", "label": "Caminhão (entrega)" },
            { "value": "star", "label": "Estrela (Nano Points)" },
            { "value": "shield", "label": "Escudo (proteção)" },
            { "value": "leaf", "label": "Folha (natural)" },
            { "value": "check", "label": "Check (qualidade)" },
            { "value": "phone", "label": "Telefone (atendimento)" },
            { "value": "zap", "label": "Raio (rápido)" },
            { "value": "package", "label": "Pacote (envio)" }
          ],
          "default": "truck"
        },
        {
          "type": "richtext",
          "id": "text",
          "label": "Texto",
          "info": "Use <strong> para destacar a palavra-chave (vira vermelho).",
          "default": "<p>Entrega expressa em <strong>até 48h</strong>.</p>"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Benefícios Nanovit",
      "category": "Home Nanovit",
      "blocks": [
        { "type": "beneficio", "settings": { "icon": "truck", "text": "<p>Entrega expressa em <strong>até 48h</strong> em todo Brasil.</p>" } },
        { "type": "beneficio", "settings": { "icon": "star", "text": "<p>Acumule <strong>Nano Points</strong> em cada compra.</p>" } },
        { "type": "beneficio", "settings": { "icon": "package", "text": "<p>Frete <strong>grátis</strong> acima de R$ 199,90.</p>" } },
        { "type": "beneficio", "settings": { "icon": "check", "text": "<p>Fórmulas testadas com selo <strong>Nanotech</strong>.</p>" } },
        { "type": "beneficio", "settings": { "icon": "phone", "text": "<p>Consultoria <strong>grátis</strong> com nutricionistas.</p>" } }
      ]
    }
  ]
}
```

### Detalhes Liquid

- Renderizar ícone via snippet `{% render 'icon', icon: block.settings.icon, size: 20 %}`. Se o snippet `icon` do Horizon não suporta esses nomes, criar `snippets/nanovit-icon.liquid` com `case`/`when` retornando os SVGs.
- `<strong>` dentro do `richtext` deve receber `color: var(--color-primary)` via CSS (`.nanovit-home-beneficios__text strong { color: var(--color-primary); font-weight: 700; }`).

### CSS

```css
.nanovit-home-beneficios { margin-top: 48px; }
.nanovit-home-beneficios__grid {
  display: grid;
  gap: 14px;
  grid-template-columns: repeat(var(--cols, 5), 1fr);
}
.nanovit-home-beneficios__item {
  display: flex; align-items: center; gap: 14px;
  background: var(--nanovit-color-cream-2);
  border-radius: var(--nanovit-radius-lg);
  padding: 16px 18px;
}
.nanovit-home-beneficios__icon {
  width: 44px; height: 44px; border-radius: 50%;
  background: #fff;
  border: 1px solid rgba(42,28,26,0.06);
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.nanovit-home-beneficios__icon svg { width: 20px; height: 20px; color: var(--color-foreground); }
.nanovit-home-beneficios__text { font-size: 13px; line-height: 1.35; color: var(--color-foreground); }
.nanovit-home-beneficios__text strong { color: var(--color-primary); font-weight: 700; }
@media (max-width: 749px) {
  .nanovit-home-beneficios__grid {
    grid-template-columns: none;
    grid-auto-flow: column;
    grid-auto-columns: 80%;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    scrollbar-width: none;
  }
  .nanovit-home-beneficios__item { scroll-snap-align: start; }
}
```

No Liquid, passar contagem como CSS var: `style="--cols: {{ section.blocks.size }};"` no `.nanovit-home-beneficios__grid`.

**Notas do Dev:** > _(preencher após executar)_

---

## ❌ Não fazer

- Não fixar quantidade de items (deve aceitar 3-6 dinamicamente).
- Não hardcoded texto. Tudo via `richtext`.
- Não usar travessão.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
