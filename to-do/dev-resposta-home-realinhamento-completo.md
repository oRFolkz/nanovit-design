# Dev → Diretor · Home Realinhamento completo (briefs 01–13)

> Arquivo de sinalização proativa. Toda a rodada de realinhamento da home (13 briefs) foi executada. Tema atualizado em staging, sem erros de schema.

## Estado final

| # | Brief | Section/arquivo entregue |
|---|---|---|
| 01 | cleanup | 5 sections obsoletas deletadas |
| 02 | hero-slideshow | `sections/nanovit-home-hero.liquid` (custom element `<nanovit-hero>`) |
| 03 | beneficios | `sections/nanovit-home-beneficios.liquid` (3-6 blocks, mobile scroll-snap) |
| 04 | linha-nanovit | `sections/nanovit-home-linha.liquid` (7 cards de coleção, scroll horizontal) |
| 05 | bestsellers | `sections/nanovit-home-produtos.liquid` + 2 snippets reutilizáveis |
| 06 | cta-institucional | `sections/nanovit-home-cta-banner.liquid` (2 colunas) |
| 07 | kits | Reuso de 05 no template (sem section nova) |
| 08 | promo-strip | `sections/nanovit-home-promo-strip.liquid` (full-bleed Nano Red com glows) |
| 09 | medicos | `sections/nanovit-home-medicos.liquid` (custom element `<nanovit-medicos>`) |
| 10 | ugc | `sections/nanovit-home-ugc.liquid` (vídeo 9:14 + mini product card) |
| 11 | institucional | `sections/nanovit-home-institucional.liquid` (banner com overlay) |
| 12 | onde-comprar | `sections/nanovit-home-marketplaces.liquid` (logos + trust badges) |
| 13 | template-index | `templates/index.json` reescrito com 12 sections em ordem |

**Snippets reusáveis criados:**
- `snippets/nanovit-section-head.liquid` — cabeçalho de section (título + sub + link + setas com data-nv-head-prev/next)
- `snippets/nanovit-product-card.liquid` — card de produto com tags/rating/preço/installments via metafields + `{% form 'product' %}`

**Locale:** `locales/pt-BR.json` agora tem `products.installments_html`, `products.add_to_cart`, `products.sold_out` no nível raiz (além do já existente `products.product.add_to_cart`).

## Tema em staging

- **Preview:** https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659
- **Editor:** https://nexgeniusstore.myshopify.com/admin/themes/152714772659/editor
- **Theme ID:** `152714772659` (Nanovit Dev)

Push limpo, sem erros. Home renderiza as 12 sections na ordem do brief 13.

## Decisões fora do brief (resumo)

- **Custom elements** em hero e médicos (autoplay com pausa em hover/focus, cleanup no disconnectedCallback). Briefs deixavam a escolha pro dev.
- **SVGs inline** em todos os ícones (Feather-style) em vez de `{% render 'icon' %}` — não dependo de snippet do Horizon que pode não suportar os nomes.
- **`{%- assign alt_var -%}` antes do `image_tag`** porque Liquid não aceita pipe inline em parâmetros (`alt: settings.foo | escape` quebra; assign primeiro funciona).
- **Fallbacks visuais em todas as sections** quando admin ainda não configurou conteúdo (placeholders cream com instrução). Tema "sai da caixa" sem ficar quebrado.
- **Snippet `nanovit-section-head`** centraliza o pattern título+sub+link+setas. Reusado em produtos e UGC (e qualquer futura section que precise).

## Pendências do cliente (admin Shopify)

Pra home "ficar pronta" visualmente:

- Hero: subir 3 a 5 imagens (2600×1140px aproximado)
- Linha Nanovit: criar 7 coleções com `collection.image` definida e ligar cada bloco do template ao handle correto
- Bestsellers: criar coleção com handle `bestsellers` ou usar `frontpage`
- Kits: criar coleção com handle `kits`
- Médicos: subir 5 fotos circulares + nomes + credenciais + quotes
- UGC: subir 5 vídeos verticais (9:14) + ligar produto correspondente em cada um
- Institucional: subir foto 2400×1050 (16:7)
- Marketplaces: subir 5 logos de redes (Droga Raia, Drogasil, Panvel, Amazon, MELI)
- Metafields opcionais: `product.metafields.nanovit.rating`, `.review_count`, `.tags_display`

## Sem dúvidas em aberto

Implementação fiel aos 13 briefs. Aguardando sua validação.

—Dev
