# Dev → Diretor · SEO + metatags + fallbacks aplicados

> Brief `[CONCLUIDO] seo-metatags-fallbacks.md` executado integralmente (7 tarefas). Tema em staging atualizado.

## O que mudou (visível na inspeção de `<head>`)

| Item | Antes | Agora |
|---|---|---|
| `theme-color` | vazio (`content=""`) | Nano Red em PDP, cream nas demais |
| Favicon | só se admin subir | SVG do tema como fallback (`nanovit-favicon.svg`) |
| Apple touch icon | inexistente | adicionado (mesmo fallback) |
| `<title>` separator | `&ndash;` (en-dash, vira "–") | ` · ` (middle dot, padrão da marca) |
| Tag/página i18n | "tagged" / "Page" (en) | "Com a tag" / "Página" (pt-BR) |
| OG image default | quebrava se não houvesse `page_image` | fallback page_image > settings.logo > `nanovit-og-default.svg` |
| JSON-LD Organization | inexistente | renderizado na home, com sameAs dinâmico das redes |
| JSON-LD WebSite SearchAction | inexistente | renderizado na home (sitelink search box no Google) |
| JSON-LD Product | dependia do Horizon | snippet próprio, aggregateRating se metafields preenchidos |
| Fonts | só preconnect + stylesheet | + `rel="preload"` da CSS |

## Tema em staging

https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659

Inspeção `view-source` deve mostrar todos os deltas acima.

## Pendências cliente que persistem

- **Settings → General → Store name:** trocar "NexgeniusStore" → "Nanovit Nutrition". Afeta `<title>`, OG `site_name` e schema.org `Organization.name`. Sem isso, todo o resto fica certo mas com o nome errado.
- **Settings → Brand:** preencher logo, slogan, redes sociais (Instagram, TikTok, YouTube, Facebook). Sem redes preenchidas, o array `sameAs` no JSON-LD fica vazio e é omitido.
- **Metafields produto** (`nanovit.rating` + `nanovit.review_count`): se preenchidos, JSON-LD Product habilita rich result de estrelas na SERP. Sem eles, schema.org Product continua válido só sem aggregateRating.

## Status geral da migração

| Trabalho | Status |
|---|---|
| Fase 0 (fundação) | ✅ |
| Fase 2 (PDP 5Mag + sub-rodadas) | ✅ |
| Fase 4 (institucionais) | ✅ |
| Home realinhamento (briefs 01-13) | ✅ |
| **Header fiel ao mock** | ✅ (rodada anterior) |
| **SEO + metatags + fallbacks** | ✅ (esta rodada) |

Sem trabalho ativo. Polling segue ligado.

—Dev
