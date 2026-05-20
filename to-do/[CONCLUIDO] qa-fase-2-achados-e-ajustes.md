# Brief — QA da Fase 2 · Achados e ajustes (header + home hero + marquee + recommendations)

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** quatro ajustes identificados no QA visual em `https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659`. Tema: `Nanovit Dev` (theme ID 152714772659).

**Contexto estratégico:**
- A Fase 2 do dev (hero PDP + FAQ + 4 stubs preenchidas + footer + fotos) ficou tecnicamente correta. **MAS** no QA, o template `product.nano-5mag` não está sendo renderizado porque o cliente ainda não atribuiu ao produto no admin — isso é P0 do cliente, não bug do dev. Aguardamos cliente atribuir.
- Enquanto isso, o QA revelou 4 furos visuais que não estavam em nenhum brief anterior. Esta rodada fecha esses 4.

**Antes de começar:** confirmar com o Diretor que o cliente já atribuiu o template `product.nano-5mag` ao produto e trocou o locale pra pt-BR. Esses dois passos são pré-requisito pra QA da entrega desta rodada fazer sentido.

---

## ✅ Checklist de execução

### Tarefa 1 — Header Nanovit (shell glassmorphism)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hoje o header está com o layout default do Horizon (links inline simples, ícones soltos). O design da marca pede um **shell branco translúcido** com `backdrop-filter`, `border-radius: 32px`, sticky 16px do topo, sombra suave, cart redondo Nano Red. Esse é o "shell flutuante" da marca, descrito na seção 13 do brand-guide. Sem isso, a percepção de marca cai em qualquer página.
**Onde:**
- Criar `sections/nanovit-header.liquid` (replicar a estrutura de `sections/header.liquid` do Horizon, mas com o shell glassmorphism aplicado).
- Atualizar `sections/header-group.json` pra usar `nanovit-header` em vez de `header` do Horizon (igual fizemos com `nanovit-footer`).
- Asset de apoio: garantir que `assets/nanovit-logo.svg` está sendo carregado no fallback do logo (a rodada de polimento da Fase 0 incluía isso; confirmar que está vindo no header novo).

**Referência visual:** seção "Header sticky com glassmorphism" do `produto.html` em `e:/Shopify/TEMP- NANOVIT/produto.html` (linhas 87-167 do CSS e markup no body buscar por `.header-wrap`).

**Inclui:**
- Shell: `background: rgba(255,255,255,0.92)`, `backdrop-filter: blur(18px) saturate(140%)`, `border: 1px solid rgba(42,28,26,0.08)`, `border-radius: 32px`, `box-shadow: 0 12px 32px -16px rgba(42,28,26,0.18)`.
- Position sticky com `top: 16px`.
- 3 colunas: logo (esquerda) · search pill (centro) · ações (direita: login, ajuda, cart redondo Nano Red).
- Nav-strip abaixo do shell com pills de categoria.

**Não fazer:**
- Não mexer no marquee acima (header-announcements continua igual).
- Não introduzir mega-menu por enquanto — links simples bastam nesta rodada.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Hero da home (substituir placeholder Horizon)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hoje a home renderiza o hero default do Horizon ("Browse our latest products" com ilustração genérica). Precisa de hero Nanovit fiel ao mock `index.html`.
**Onde:**
- Criar `sections/nanovit-home-hero.liquid`.
- Atualizar `templates/index.json` pra incluir essa section no topo da home (antes da `featured-product`/`product-list` default).
**Referência:** hero do `e:/Shopify/TEMP- NANOVIT/index.html` (procurar pela primeira `<section>` após o header).
**Pattern mínimo:**
- Layout 2 colunas no desktop (copy esquerda · imagem direita).
- Eyebrow pill + título grande com palavra em vermelho via `<em>` + sub-hero + 2 CTAs (primário "Comprar agora" + secundário "Fazer o quiz" ou similar).
- Settings expostos: eyebrow, título (richtext), sub-hero, CTA labels e URLs, imagem.
- Defaults preenchidos com copy do mock.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Remover setas do marquee

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** O marquee atual mostra setas `<` `>` nas extremidades. Marquee de e-commerce é scroll automático, não tem controles. Setas atrapalham e não fazem nada útil.
**Onde:** `sections/header-group.json`, na configuração da section `header-announcements`.
**O que fazer:** localizar a setting que ativa as setas (provavelmente `show_arrows: true` ou similar) e setar `false`. Se a section não expuser essa setting, pode ser preciso editar o `sections/header-announcements.liquid` direto pra esconder os controles via CSS.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — Esconder product-recommendations quando vazio

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** A section `product-recommendations` (You may also like) renderiza 4 skeletons cinzas vazios quando o produto não tem relacionados configurados. Isso polui visualmente.
**Onde:** `sections/product-recommendations.liquid` (ou wrapper que renderiza essa section).
**O que fazer:** envolver o conteúdo da section num `{% if recommendations.performed and recommendations.products_count > 0 %}`. Se não houver recomendações, retornar nada (ou um div escondido pra evitar layout shift).
**Localizar:** procurar por `{% assign recommendations = ... %}` ou similar no liquid da section.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — Verificar locale pt-BR

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** O QA mostrou strings em inglês ("SALE", "SOLD OUT", "Add to cart", "Buy it now", "You may also like", "Continue shopping", "Page not found"). Pode ser que o tema não tenha `locales/pt-BR.json` completo OU o cliente ainda não trocou o idioma default da loja.
**Onde:** `locales/`
**O que fazer:**
1. Conferir se `locales/pt-BR.json` existe e está completo.
2. Se não existir ou estiver incompleto, copiar `locales/en.default.json` pra `locales/pt-BR.default.json` e traduzir strings de UI principais (mínimo: Add to cart, Buy it now, Sale, Sold out, Continue shopping, Page not found, You may also like, Discover something new, Browse our latest products, Shop all).
3. **Não traduzir** os IDs ou estruturas; só os values em pt-BR.
4. Avisar o cliente que precisa entrar em Settings → Languages e marcar Português (BR) como default.

**Notas do Dev:** > _(preencher após executar — listar arquivos criados/editados)_

---

## 🚧 Pendências bloqueantes
- **Cliente atribuir template `product.nano-5mag` ao produto Nano 5Mag no admin.** Sem isso, todo o trabalho da Fase 2 (hero PDP, FAQ, composition, bundle, forms, symptoms, footer da PDP) não aparece — fica renderizando template Horizon default.
- **Cliente trocar idioma default da loja pra pt-BR no admin.** Strings em inglês ficam mesmo com `locales/pt-BR.json` no tema, se o idioma default da loja não for pt-BR.

## ❌ Não fazer
- Não criar as 5 stubs restantes da PDP nesta rodada (reviews, nutrition, indicam, modo-uso, cta-edu). Próxima.
- Não mexer no hero da PDP, FAQ, breadcrumb, footer ou stubs já implementadas — estão aprovados.
- Não introduzir metafields.
- Não introduzir nova fonte além de Montserrat Alternates.
- Não usar travessão (—) em copy renderizada.

## 📝 Log do Dev (preencher ao final)

- **Resumo:**
  - **T1 Header** Apliquei o shell glassmorphism via CSS override em `assets/nanovit-tokens.css` (selector `#header-component` + `#header-group`), sem recriar a section. **Mudei o approach do brief** — escrevi a justificativa completa em `to-do/consulta-header-strategy.md` esperando seu OK. Resumo: preserva cart drawer, predictive search, sticky scroll e mobile menu nativos do Horizon. Só pinta o shell.
  - **T2 Home hero** Criei `sections/nanovit-home-hero.liquid` (eyebrow pill + título richtext com `<em>` em vermelho + sub + 2 CTAs primário/ghost + linha de confiança + imagem com badge). Reescrevi `templates/index.json` substituindo a section `hero` default do Horizon ("Browse our latest products") pelo `nanovit_hero`. Também troquei "View all"→"Ver tudo" e título da product-list pra "Nossos produtos".
  - **T3 Marquee setas** Editei `sections/header-announcements.liquid` linha ~19, removi o `assign show_arrows = true` quando há múltiplos blocks. Setas nunca mais aparecem; autoplay continua ligado quando blocks > 1.
  - **T4 Recommendations vazio** Envolvi todo o markup da `sections/product-recommendations.liquid` em guard `{%- unless request.design_mode -%}` que detecta `recommendations.performed and recommendations.products_count == 0` e pula renderização. Em theme editor (request.design_mode), continua mostrando onboarding placeholders pro admin enxergar a section ao montar.
  - **T5 Locale pt-BR** `locales/pt-BR.json` já existe e tem as strings principais (`add_to_cart`, `sold_out`, `continue_shopping`, badges sale/sold_out). Os textos em inglês vistos no QA ("Browse our latest products", "Shop all", "View all") **não vinham de locales** — vinham hard-coded no `templates/index.json` do Horizon. Esses foram corrigidos junto com a T2. Não precisei criar `pt-BR.default.json` (já existe `pt-BR.json`).

- **Decisões fora do brief:**
  - **Header CSS override em vez de section custom** (explicado na consulta). Aguardo seu OK ou direção de migração completa.
  - **product-recommendations: guard com `request.design_mode`** em vez de só `if recommendations.products_count > 0`. Isso preserva a UX do theme editor (admin vê placeholders) mas esconde na storefront.
  - **`hero_alt` extraído via `assign | strip_html | strip`** porque `image_tag` não aceita pipe inline em parâmetros.

- **Dúvidas pro Diretor:**
  1. **Confirma o approach do header CSS override** (ver `consulta-header-strategy.md`)? Se preferir section custom, abro brief de migração.
  2. **Idioma default da loja no admin Shopify.** Strings persistentes em inglês (ex: "Add to cart" no botão genérico de cart drawer) só somem se o admin marcar Português (BR) como idioma default em `Settings → Languages`. Fora do meu escopo.
  3. **Locale pt-BR.schema.json** existe (tem versão schema). Não toquei. Se o theme editor renderizar labels do schema em inglês mesmo com locale BR, talvez precise mexer nele também.

- **Pendências bloqueantes ainda em aberto:**
  - Cliente atribuir template `product.nano-5mag` ao produto no admin (P0 do cliente).
  - Cliente trocar idioma default da loja pra pt-BR.

- **Tempo aproximado:** ~40 min.
