# Dev → Diretor · Header custom executado e em staging

> Sinalização proativa: brief `[CONCLUIDO] header-fiel-mock.md` aplicado integralmente. Aguardando QA visual.

## O que mudou

| Item | Antes | Agora |
|---|---|---|
| Section ativa no `header-group.json` | `header` (Horizon, ~700 linhas) | `nanovit-header` (custom, fiel ao mock) |
| Logo | "Início" textual (sem asset) | Logo do settings ou fallback `nanovit-logo.svg` |
| Search field | Não existia central | Pill 520px central com placeholder "O que você procura?" |
| Login / Ajuda | Ícones soltos | Ícone + label, padrão da marca |
| Cart | Ícone genérico | Círculo Nano Red 42px com badge dinâmico `cart.item_count` |
| Nav-strip | Não existia | 7 pills com mega-menu (objetivos, feature, collection, none) |
| Mobile | Hamburger sem drawer custom | Offcanvas com logo + search + lista das pills + footer |
| CSS override | Bloco em `nanovit-tokens.css` | Removido; styling vive no `{% stylesheet %}` da section |

## Mega-menus

- **Compre por objetivo** (active, ícone): grid 4 colunas com 8 objetivos hard-coded (Emagrecimento, Memória & Foco, Bem-estar, Energia, Sono, Imunidade, Beleza, Performance). URLs `/collections/<handle>`. Para virar editável pelo admin no futuro, criar metaobject `nanovit_objectives`.
- **Quiz Personalizado**: card richtext + CTA "Começar agora" → `/pages/quiz`.
- **Kits / Mais vendidos / Lançamentos / Promoções**: dropdown_type=collection. Cada um lista até 6 produtos da coleção atribuída pelo admin (pendência: criar coleções `kits`, `bestsellers`, `lancamentos`, `promocoes`). Sem coleção atribuída, dropdown mostra mensagem instrucional cream.
- **Todos os produtos**: link simples `/collections/all`, sem dropdown.

## Tema em staging

https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659

Push limpo (após corrigir um `tag: null` no schema → tem que ser string).

## Pendências cliente que persistem

- Subir **logo** em Settings → Brand → Logo (fallback SVG entra enquanto isso).
- Criar **coleções**: `kits`, `bestsellers`, `lancamentos`, `promocoes` (Smart com tag ou Manual). Sem isso os 4 mega-menus collection ficam vazios.
- Atribuir cada coleção no theme editor → Home/Header → Pílula correspondente → "Coleção fonte".

## Próximo

Vou agora executar `seo-metatags-fallbacks.md` (7 tarefas de meta/JSON-LD/preload, sem dependência do cliente). Em seguida volto a aguardar.

—Dev
