# Plano-mestre — Realinhamento da home com `index.html`

> Documento estratégico desta rodada. Cada section vira brief próprio `home-realinhamento-NN-<nome>.md` (numerados 01 a 13). Dev executa em sequência ou em paralelo onde fizer sentido.

## Por quê esta rodada existe

A Fase 3 (home) ficou desalinhada com o mock `index.html`. Eu (Diretor) escrevi briefs sem ler o mock direito e mandei o dev criar 4 sections inventadas (`objetivo`, `manifesto`, `linhas` cardzinho, `depoimentos` quote longo). O dev executou corretamente — o erro foi meu, de direção.

Esta rodada **substitui** a home atual pela tradução fiel do `index.html`, com 10 sections nativas Shopify + duas que já estão certas (Newsletter, Footer).

## Regra absoluta — nada hardcoded

**Na Shopify, conteúdo NUNCA fica preso em Liquid.** Toda informação visível precisa vir de uma das três fontes:

1. **`settings` no schema da section** (admin edita no theme editor)
2. **`product.*` / `collection.*` / `customer.*` / `shop.*`** (admin gerencia no admin Shopify, via produto/coleção/cliente)
3. **`metafields.*`** (admin gerencia via Settings → Custom data)

Exemplos:
- Título "Mais Vendidos" → `setting.text`, default "Mais Vendidos"
- Produto da listagem → `setting.collection` picker, loop em `collection.products`
- Foto do hero → `setting.image_picker`, com fallback pra placeholder do tema
- Preço "R$ 89,90" → `product.price | money`
- Avaliação "★★★★★ 4.9 (287)" → metafield numérico (placeholder visual quando não houver)

Se algo está hardcoded e o admin não pode editar, **é bug arquitetural**. Re-fazer.

## Convenções desta rodada (Shopify Theme Standards + Marca)

### Nomenclatura
- Section: `sections/nanovit-home-<nome>.liquid` (prefixo `nanovit-home-` evita conflito com upgrades do Horizon e marca claramente que é da home).
- Snippets reutilizados (ex: product card): `snippets/nanovit-product-card.liquid`.
- Schema `name`: pt-BR descritivo, até 25 chars (ex: "Hero · Slideshow").
- Schema `class`: kebab-case com prefixo (ex: `nanovit-home-hero-section`).
- Block `type`: kebab-case (ex: `slide`, `beneficio`, `linha`, `kit`, `quote-medico`, `ugc-item`, `marketplace`).

### Schema Shopify — boas práticas obrigatórias
- `settings_schema.json` defaults preenchidos pra "sair da caixa pronto".
- **`presets`** com `name`, `category` ("Home Nanovit"), e blocks default instanciados.
- **`enabled_on.templates: ["index"]`** quando a section só faz sentido na home.
- **`max_blocks`** definido em toda section com blocks repetíveis.
- **`limit`** em settings de imagem/coleção quando aplicável.
- **`info`** em settings não-óbvios pra orientar admin.
- **`{%- liquid -%}`** pra blocos longos + `{%-` `-%}` pra remover whitespace.
- **`{{ x | t }}`** pra strings de UI (locales). Defaults hardcoded em pt-BR aceitáveis quando admin pode editar via setting.

### Acessibilidade
- Todo carrossel: setas com `aria-label`, navegação via teclado, `aria-live` em região anunciada.
- Imagens: `alt` vindo de `setting.text` ou `image.alt`.
- Cores: contraste mínimo WCAG AA (4.5:1 texto normal, 3:1 texto grande).
- Foco visível em todo interativo.

### Performance
- Imagens via `{{ image | image_url: width: X | image_tag: ... }}` (Shopify gera srcset).
- `loading="lazy"` em tudo abaixo da dobra.
- JS embutido só quando inevitável; preferir `<details>`, scroll-snap CSS, etc.
- `{% stylesheet %}` da section escopo limitado.

### Marca (brand-guide v1.1)
- Fonte: `var(--font-heading--family)` Montserrat Alternates (já carregada).
- Tokens: `var(--nanovit-radius-lg)` 14px, `var(--nanovit-radius-xl)` 22px, `var(--nanovit-radius-pill)` 999px.
- Cores: `var(--color-primary)` Nano Red, `var(--color-foreground)` Cocoa #2A1C1A.
- **Sem travessão (—)** em copy renderizada. Usar ponto, vírgula, dois pontos, parêntese.
- **Sem promessa terapêutica** ("cura", "trata"). Suplemento não cura.
- **Sem urgência forçada** ("últimas unidades", "garanta já").

## Mapa de sections desta rodada

| # | Brief | Section | Tipo |
|---|---|---|---|
| 01 | [home-realinhamento-01-cleanup.md](home-realinhamento-01-cleanup.md) | (cleanup) | Deletar `nanovit-bestsellers`, `nanovit-objetivo`, `nanovit-manifesto`, `nanovit-linhas`, `nanovit-depoimentos` da Fase 3 errada |
| 02 | [home-realinhamento-02-hero-slideshow.md](home-realinhamento-02-hero-slideshow.md) | `nanovit-home-hero` | Slideshow banner com blocks `slide` |
| 03 | [home-realinhamento-03-beneficios.md](home-realinhamento-03-beneficios.md) | `nanovit-home-beneficios` | Faixa horizontal de blocks `beneficio` |
| 04 | [home-realinhamento-04-linha-nanovit.md](home-realinhamento-04-linha-nanovit.md) | `nanovit-home-linha` | Carrossel categorias via `collection` pickers |
| 05 | [home-realinhamento-05-bestsellers.md](home-realinhamento-05-bestsellers.md) | `nanovit-home-produtos` | Carrossel reutilizável (bestsellers, kits, etc.) via `collection` |
| 06 | [home-realinhamento-06-cta-institucional.md](home-realinhamento-06-cta-institucional.md) | `nanovit-home-cta-banner` | Banner 2 col cream com foto + CTA duplo |
| 07 | [home-realinhamento-07-kits.md](home-realinhamento-07-kits.md) | (REUSO de 05) | Apenas configurar `nanovit-home-produtos` 2x: bestsellers + kits |
| 08 | [home-realinhamento-08-promo-nanocomprimidos.md](home-realinhamento-08-promo-nanocomprimidos.md) | `nanovit-home-promo-strip` | Banner vermelho full-bleed |
| 09 | [home-realinhamento-09-medicos-banner.md](home-realinhamento-09-medicos-banner.md) | `nanovit-home-medicos` | Carrossel quote+avatar de médicos |
| 10 | [home-realinhamento-10-usam-indicam.md](home-realinhamento-10-usam-indicam.md) | `nanovit-home-ugc` | UGC vídeo + product card mini |
| 11 | [home-realinhamento-11-institucional.md](home-realinhamento-11-institucional.md) | `nanovit-home-institucional` | Banner full-width com overlay |
| 12 | [home-realinhamento-12-onde-comprar.md](home-realinhamento-12-onde-comprar.md) | `nanovit-home-marketplaces` | Logos marketplaces + trust badges |
| 13 | [home-realinhamento-13-template-index.md](home-realinhamento-13-template-index.md) | (orquestração) | Montar `templates/index.json` final |

## Ordem recomendada de execução

**Paralelizáveis (sem dependência):** 02, 03, 04, 05, 06, 08, 09, 10, 11, 12 — qualquer ordem.

**Dependências:**
- 01 (cleanup) pode ser antes ou simultâneo às criações novas (não conflita).
- 07 (kits) depende de 05 estar pronta (é configuração da mesma section).
- 13 (template) depende de TODAS as outras estarem prontas — é o último.

## Pendências do cliente (admin Shopify)

Vai precisar criar/configurar no admin:
- **Coleção `bestsellers`** com 5+ produtos (ou tag `best`).
- **Coleção `kits`** com 3+ produtos kit (ou criar produtos kit).
- **7 coleções de linha** (Emagrecimento, Sono, Energia, Imunidade, Beleza, Saúde da Mulher, Performance) — cada uma com `collection.image` definida (a imagem que aparece no card da Linha Nanovit).
- **Metafields opcionais:**
  - `product.metafields.nanovit.rating` (number, 0.0-5.0)
  - `product.metafields.nanovit.review_count` (integer)
  - `product.metafields.nanovit.tags_display` (list.single_line_text — "Vegano", "Sem glúten", "Nanotech")
  - `product.metafields.nanovit.video_ugc` (file picker — vídeo UGC do produto)

Sem esses metafields, o tema renderiza placeholders visuais. Não bloqueia.

## Como abrir os 12 briefs

Estão sendo criados nesta ordem na pasta `to-do/`. Cada um é autocontido (não precisa abrir os outros). Dev pode pegar 1 por vez e renomear `[REVISÃO]`/`[CONCLUIDO]` no fluxo padrão.
