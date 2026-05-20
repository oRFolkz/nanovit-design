# Brief 13 — Orquestração do `templates/index.json`

> Pré-req: TODOS os briefs 02-12 fechados. Este é o último passo.
**Arquivo:** `templates/index.json`

**O que é:** Reescreve o `templates/index.json` da home pra usar as 10 sections novas + Newsletter existente, na ordem certa, com defaults instanciados nos blocks. Sem isso, o trabalho dos briefs anteriores não aparece na home.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

### Ordem final das sections (de cima pra baixo na home)

| # | Section type | ID sugerido | Settings principais |
|---|---|---|---|
| 1 | `nanovit-home-hero` | `hero` | Preset com 3 blocks slide (admin sobe imagens depois). |
| 2 | `nanovit-home-beneficios` | `beneficios` | Preset com 5 blocks de benefício (defaults já preenchidos no schema). |
| 3 | `nanovit-home-linha` | `linha` | Preset com 7 blocks de categoria (admin liga coleções). |
| 4 | `nanovit-home-produtos` | `bestsellers` | `title="Mais Vendidos"`, `subtitle="Os produtos mais amados pela nossa comunidade"`, `collection="bestsellers"`, `limit=8`, `see_all_label="Ver todos os produtos"`. |
| 5 | `nanovit-home-cta-banner` | `cta_marca` | Defaults do schema (admin sobe foto). |
| 6 | `nanovit-home-produtos` | `kits` | `title="Kits Nanovit"`, `subtitle="Combinações pensadas para resultados consistentes."`, `collection="kits"`, `limit=6`, `see_all_label="Ver todos os kits"`. |
| 7 | `nanovit-home-promo-strip` | `promo_nanocomprimidos` | Defaults (admin liga URL pra `/pages/tecnologia`). |
| 8 | `nanovit-home-medicos` | `medicos` | Preset com 5 blocks (admin sobe fotos + texto). |
| 9 | `nanovit-home-ugc` | `ugc` | Preset com 5 blocks (admin liga vídeos + produtos). |
| 10 | `nanovit-home-institucional` | `institucional` | Defaults (admin sobe foto). |
| 11 | `nanovit-home-marketplaces` | `marketplaces` | Preset com 5 logos + 3 trust (admin sobe logos). |
| 12 | `nanovit-newsletter` | `newsletter` | Existente, manter como está. |

### Esqueleto do JSON

```json
{
  "sections": {
    "hero": {
      "type": "nanovit-home-hero",
      "blocks": {
        "slide_1": { "type": "slide", "settings": {} },
        "slide_2": { "type": "slide", "settings": {} },
        "slide_3": { "type": "slide", "settings": {} }
      },
      "block_order": ["slide_1", "slide_2", "slide_3"],
      "settings": {
        "autoplay": true,
        "autoplay_speed": 6,
        "show_arrows": true,
        "show_dots": true
      }
    },

    "beneficios": {
      "type": "nanovit-home-beneficios",
      "blocks": {
        "b1": { "type": "beneficio", "settings": { "icon": "truck", "text": "<p>Entrega expressa em <strong>até 48h</strong> em todo Brasil.</p>" } },
        "b2": { "type": "beneficio", "settings": { "icon": "star",  "text": "<p>Acumule <strong>Nano Points</strong> em cada compra.</p>" } },
        "b3": { "type": "beneficio", "settings": { "icon": "package", "text": "<p>Frete <strong>grátis</strong> acima de R$ 199,90.</p>" } },
        "b4": { "type": "beneficio", "settings": { "icon": "check", "text": "<p>Fórmulas testadas com selo <strong>Nanotech</strong>.</p>" } },
        "b5": { "type": "beneficio", "settings": { "icon": "phone", "text": "<p>Consultoria <strong>grátis</strong> com nutricionistas.</p>" } }
      },
      "block_order": ["b1","b2","b3","b4","b5"],
      "settings": {}
    },

    "linha": {
      "type": "nanovit-home-linha",
      "blocks": {
        "l1": { "type": "linha", "settings": { "bg_color": "#ffe4d6" } },
        "l2": { "type": "linha", "settings": { "bg_color": "#e0e4f5" } },
        "l3": { "type": "linha", "settings": { "bg_color": "#fff1cc" } },
        "l4": { "type": "linha", "settings": { "bg_color": "#dcebe0" } },
        "l5": { "type": "linha", "settings": { "bg_color": "#fbe6e3" } },
        "l6": { "type": "linha", "settings": { "bg_color": "#f5dde8" } },
        "l7": { "type": "linha", "settings": { "bg_color": "#d9c0a3" } }
      },
      "block_order": ["l1","l2","l3","l4","l5","l6","l7"],
      "settings": {
        "title": "Linha Nanovit",
        "subtitle": "Fórmulas limpas e de alta absorção para você alcançar sua melhor versão.",
        "see_all_label": "Ver todas as coleções"
      }
    },

    "bestsellers": {
      "type": "nanovit-home-produtos",
      "blocks": {},
      "settings": {
        "title": "Mais Vendidos",
        "subtitle": "Os produtos mais amados pela nossa comunidade",
        "limit": 8,
        "see_all_label": "Ver todos os produtos",
        "show_tags": true,
        "show_rating": true,
        "show_arrows": true
      }
    },

    "cta_marca": {
      "type": "nanovit-home-cta-banner",
      "blocks": {},
      "settings": {
        "title": "<p>Nanovit: <em>suplementos de alta qualidade</em> para saúde e performance</p>",
        "subtitle": "<p>Para quem entende que cuidar do corpo é priorizar o presente. A Nanovit nasceu com a missão de unir ciência funcional, ingredientes premium e clareza nos rótulos. Cada fórmula é desenvolvida para entregar resultado real, sem promessas vazias.</p>",
        "cta_primary_label": "Ver produtos",
        "cta_secondary_label": "Sobre a marca",
        "image_position": "right"
      }
    },

    "kits": {
      "type": "nanovit-home-produtos",
      "blocks": {},
      "settings": {
        "title": "Kits Nanovit",
        "subtitle": "Combinações pensadas para resultados consistentes.",
        "limit": 6,
        "see_all_label": "Ver todos os kits",
        "show_tags": true,
        "show_rating": true,
        "show_arrows": true
      }
    },

    "promo_nanocomprimidos": {
      "type": "nanovit-home-promo-strip",
      "blocks": {},
      "settings": {
        "eyebrow": "Tecnologia exclusiva Nanovit",
        "title": "Nanocomprimidos com até 5× mais absorção",
        "cta_label": "Conheça a tecnologia",
        "show_decorative_pills": true
      }
    },

    "medicos": {
      "type": "nanovit-home-medicos",
      "blocks": {
        "m1": { "type": "medico", "settings": { "name": "Dr. Barakat", "quote": "<p>\"A Nanovit é minha parceira há muitos anos, trazendo tecnologia e inovação no cuidado dos meus pacientes.\"</p>", "author": "Dr. Victor Barakat, Médico Funcional Integrativo" } },
        "m2": { "type": "medico", "settings": {} },
        "m3": { "type": "medico", "settings": {} },
        "m4": { "type": "medico", "settings": {} },
        "m5": { "type": "medico", "settings": {} }
      },
      "block_order": ["m1","m2","m3","m4","m5"],
      "settings": {
        "eyebrow": "Indicado por especialistas",
        "autoplay": true,
        "autoplay_speed": 7
      }
    },

    "ugc": {
      "type": "nanovit-home-ugc",
      "blocks": {
        "u1": { "type": "ugc-item", "settings": {} },
        "u2": { "type": "ugc-item", "settings": {} },
        "u3": { "type": "ugc-item", "settings": {} },
        "u4": { "type": "ugc-item", "settings": {} },
        "u5": { "type": "ugc-item", "settings": {} }
      },
      "block_order": ["u1","u2","u3","u4","u5"],
      "settings": {
        "title": "Quem usa, indica",
        "subtitle": "Histórias reais de quem transformou a rotina com Nanovit",
        "see_all_label": "Ver todos os depoimentos"
      }
    },

    "institucional": {
      "type": "nanovit-home-institucional",
      "blocks": {},
      "settings": {
        "eyebrow": "Nossa essência",
        "title": "<p>Ciência funcional que cabe na sua rotina.</p>",
        "body": "<p>A Nanovit nasceu da inquietação de quem entende que cuidar do corpo é um ato presente. Fórmulas desenvolvidas com especialistas, ingredientes premium e total transparência nos rótulos, para entregar resultado real, sem promessas vazias.</p>",
        "cta_label": "Conheça a Nanovit",
        "overlay_strength": "medium"
      }
    },

    "marketplaces": {
      "type": "nanovit-home-marketplaces",
      "blocks": {
        "mk1": { "type": "marketplace", "settings": { "name": "Marketplace 1" } },
        "mk2": { "type": "marketplace", "settings": { "name": "Marketplace 2" } },
        "mk3": { "type": "marketplace", "settings": { "name": "Marketplace 3" } },
        "mk4": { "type": "marketplace", "settings": { "name": "Marketplace 4" } },
        "mk5": { "type": "marketplace", "settings": { "name": "Marketplace 5" } },
        "t1": { "type": "trust", "settings": { "icon": "shield-check", "label": "Compra segura" } },
        "t2": { "type": "trust", "settings": { "icon": "truck", "label": "Entrega rápida" } },
        "t3": { "type": "trust", "settings": { "icon": "headphones", "label": "Atendimento BR" } }
      },
      "block_order": ["mk1","mk2","mk3","mk4","mk5","t1","t2","t3"],
      "settings": {
        "eyebrow": "Disponível em todo o Brasil",
        "title": "Onde você já compra.",
        "body": "<p>A Nanovit está presente nas principais redes de farmácias e marketplaces do país, para que você encontre seus produtos onde for mais conveniente.</p>",
        "show_trust": true
      }
    },

    "newsletter": {
      "type": "nanovit-newsletter",
      "blocks": {},
      "settings": {}
    }
  },
  "order": [
    "hero",
    "beneficios",
    "linha",
    "bestsellers",
    "cta_marca",
    "kits",
    "promo_nanocomprimidos",
    "medicos",
    "ugc",
    "institucional",
    "marketplaces",
    "newsletter"
  ]
}
```

### Validações antes de push

1. JSON válido (sem trailing comma, sem comentário, aspas duplas).
2. Todo `type` referenciado existe em `sections/`.
3. Todo `block.type` referenciado existe no schema da section pai.
4. `block_order` lista exatamente os IDs dos `blocks` (sem extras, sem faltantes).
5. Coleções `bestsellers` e `kits` apontadas — quando admin criar, ligar via theme editor.

### Pós-push

Quando o dev finalizar, **avisar via arquivo novo** `[REVISÃO] home-realinhamento-13-template-index.md` (ou direto `[CONCLUIDO]`). Vou validar via Read no JSON + Grep nas sections (sem Chrome DevTools desta rodada — só confirmar que o arquivo está válido e referenciando tudo certo).

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendência cliente (admin Shopify)
- Subir imagens do hero
- Ligar coleções na Linha (7), Bestsellers (1), Kits (1)
- Subir fotos dos médicos + texto
- Subir vídeos UGC + ligar produtos
- Subir foto do banner institucional
- Subir logos de marketplaces

## ❌ Não fazer
- Não deletar `templates/index.json` sem ter o novo pronto pra push junto.
- Não referenciar sections inexistentes.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
