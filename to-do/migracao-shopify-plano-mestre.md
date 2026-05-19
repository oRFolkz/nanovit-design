# Plano-mestre — Migração para Shopify (tema Horizon)

> Documento estratégico. Não é executável. As fases viram briefs próprios na pasta `to-do/`.

## Contexto

Hoje a marca vive em HTML estático em `e:/Shopify/TEMP- NANOVIT/`. Foi clonado o tema **Horizon** (Shopify, OS 2.0, block-based) em `e:/Shopify/nanovit-theme/` para servir de base do tema oficial da loja.

A migração **não é** "transformar cada HTML em Liquid". Horizon já é um tema completo, com sections + theme-blocks + JSON templates. A estratégia é:

1. Aproveitar a engenharia do Horizon (cart drawer, predictive search, mega-menu, accessibility, mobile, etc).
2. Aplicar nossa identidade visual via `settings_schema.json` + tokens CSS, sobrescrevendo os defaults do Horizon.
3. Customizar ou criar sections/blocks específicos da Nanovit que o Horizon não tem.
4. Definir metafields para o conteúdo hoje hard-coded (benefícios, FAQ, perks, depoimentos, etc).
5. Compor cada página via `templates/<tipo>.<handle>.json`.

## Arquitetura de referência

- **`config/settings_schema.json`** — onde definimos cores, fontes, espaçamentos da marca (defaults).
- **`config/settings_data.json`** — valores atuais aplicados (esse é o que o admin/theme editor mexe).
- **`layout/theme.liquid`** — wrapper master de toda página.
- **`sections/*.liquid`** — seções reutilizáveis. Convenção Horizon: `main-<tipo>` para a section principal de cada template; sections custom de Nanovit terão prefixo `nanovit-<nome>`.
- **`blocks/_*.liquid`** — theme blocks reutilizáveis (prefixo underscore = "private", só montáveis dentro de outras sections).
- **`snippets/*.liquid`** — utilitários renderizáveis com `{% render %}`.
- **`templates/<tipo>.<handle>.json`** — composições JSON que listam sections + blocks com settings preenchidos.
- **`assets/base.css`** — CSS global. Aqui ficam nossos tokens `:root`.

## Convenções da migração

- **Prefixo `nanovit-`** em toda section/block/snippet/asset que a gente criar do zero. Facilita upgrades futuros do Horizon (qualquer arquivo sem prefixo é Horizon original; com prefixo é nosso).
- **Templates customizados** ficam como `<tipo>.<handle>.json` (ex: `product.nano-5mag.json`, `page.tecnologia.json`). Nunca sobrescrever o template default sem necessidade.
- **Metafields:** namespace `nanovit`, key em kebab-case (ex: `nanovit.benefits`, `nanovit.faq`, `nanovit.tech-demo`).
- **Sem travessão (—)** em copy renderizada na loja. Regra absoluta da marca.
- **Locales:** strings de UI vão para `locales/pt-BR.json`. Não hard-code texto em portuguese inside Liquid.

## Fases

| Fase | Brief | Saída |
|---|---|---|
| **0** | [migracao-fase-0-fundacao.md](migracao-fase-0-fundacao.md) + [polimento](migracao-fase-0-polimento.md) | Tokens de marca aplicados. Horizon "vestido" de Nanovit em fontes, cores, radii, espaçamentos. **Status: concluído.** |
| ~~**1**~~ | ~~_(catálogo de metafields)_~~ | **Obsoleto.** Decidimos pivotar para copy/estrutura hard-coded em Liquid com `settings` expostos via schema (defaults preenchidos). Metafields entram só onde fizer sentido genuíno (ex: rating médio, contagem de estoque). Vai mais rápido, entrega site visualmente pronto sem dependência de admin. |
| **2** | [migracao-fase-2-pdp-5mag.md](migracao-fase-2-pdp-5mag.md) | PDP Nano 5Mag: `templates/product.nano-5mag.json` + sections `nanovit-*` (gallery, buy-box c/ calc-frete, composição, FAQ, reviews, bundle, symptoms, nutrition). Copy hard-coded fiel ao mock HTML. |
| **3** | _(a abrir após Fase 2)_ | Home (`templates/index.json`) + 404 (`templates/404.json`). |
| **4** | _(a abrir após Fase 3)_ | Páginas institucionais: Políticas, Termos, Central de Atendimento, Seja Afiliado, Seja Prescritor. Cada uma com seu template. |

Cada fase abre seu próprio brief executável só quando a anterior fechar — para não inflar contexto e manter o dev focado.

## Decisões já fechadas

- **Apps:** loja não usa app de reviews/upsell/email ainda. Reviews e prova social viram metafield manual + section própria. Newsletter usa Shopify Forms nativo.
- **Frete grátis acima de:** R$ 199,90 (valor unificado em todo o site).
- **CTA padrão PDP:** "Adicionar ao carrinho" (mass-market BR).
- **Identidade visual:** o `brand-guide` v1.1 em `e:/Shopify/TEMP- NANOVIT/brand-guide/` é a verdade única. Toda decisão de tokens nasce dele.

## Como o dev trabalha

1. Pega o brief da fase em `e:/Shopify/TEMP- NANOVIT/to-do/`.
2. Executa as alterações em `e:/Shopify/nanovit-theme/`.
3. Renomeia o brief com prefixo `[CONCLUIDO]` ou `[REVISÃO]`.
4. Avisa o usuário, que sobe em ambiente Shopify de teste.
5. Diretor (Claude) valida visualmente via Chrome DevTools MCP ou screenshots e abre próxima fase.

## Pendências bloqueantes (fora do escopo do dev)

- Cliente precisa ter loja Shopify de **dev/staging** disponível para subir e validar.
- Cliente precisa criar os metafields no admin assim que a Fase 1 entregar o catálogo.
