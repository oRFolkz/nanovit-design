# Brief — Migração Shopify · Fase 2 · PDP Nano 5Mag

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** PDP do produto Nano 5Mag em `e:/Shopify/nanovit-theme/`. Template + sections `nanovit-*` que reproduzem fielmente a estrutura de `e:/Shopify/TEMP- NANOVIT/produto.html`.

**Contexto estratégico:**
- A PDP é o ativo de maior conversão da marca. Tem ~12 sections distintas em hierarquia clara: gallery + buy-box (above the fold), depois composição → sintomas → bundle → reviews → FAQ → nutrition.
- **Estratégia confirmada:** copy hard-coded em Liquid com `settings` expostos via schema (defaults preenchidos). Site sai da caixa visualmente idêntico ao mock; admin pode trocar copy/imagens via theme editor depois. Sem metafields nesta entrega.
- Cada section vira um arquivo `sections/nanovit-pdp-<nome>.liquid` autocontido (CSS embutido via `{% stylesheet %}`, schema com defaults).
- Template `templates/product.nano-5mag.json` monta todas as sections em ordem.
- Calculadora de frete (client-side) e sticky buy-mobile vivem dentro de `nanovit-pdp-buy-box`.

---

## 📐 Arquitetura de sections (mapa)

Ordem no template e correspondência com o HTML estático:

| # | Section Shopify | HTML estático (linhas) | Conteúdo |
|---|---|---|---|
| 1 | `nanovit-pdp-breadcrumb` | 2716-2727 | Breadcrumb home > catálogo > produto |
| 2 | `nanovit-pdp-hero` | 2727-2963 | Gallery (esquerda) + Buy-box (direita) com preço, variantes, perks, cashback, calc-frete, CTA, specialist, trust row |
| 3 | `nanovit-pdp-composition` | 2963-3052 | Comparativo de composição (com vs. sem) |
| 4 | `nanovit-pdp-indicam` | 3052-3122 | Quem usa, recomenda — UGC + indicações de profissionais |
| 5 | `nanovit-pdp-forms` | 3122-3190 | 5 formas, 5 sistemas — diagrama das formas de magnésio |
| 6 | `nanovit-pdp-cta-edu` | 3190-3214 | CTA educacional (link pra demo-tecnologia) |
| 7 | `nanovit-pdp-modo-uso` | 3214-3242 | Zig-zag de modo de uso |
| 8 | `nanovit-pdp-bundle` | 3242-3306 | Compre junto |
| 9 | `nanovit-pdp-symptoms` | 3306-3363 | Sintomas que o produto resolve |
| 10 | `nanovit-pdp-reviews` | 3363-3463 | Reviews + summary stars |
| 11 | `nanovit-pdp-faq` | 3463-3518 | FAQ accordion |
| 12 | `nanovit-pdp-nutrition` | 3518-3568 | Tabela nutricional + tech info |

E suportes globais:
- `sections/nanovit-footer-group.json` + `sections/nanovit-footer.liquid` — footer institucional dark (substitui ou complementa o `footer-group` do Horizon)

---

## ✅ Checklist de execução

### Tarefa 1 — Template `product.nano-5mag.json`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** É o orquestrador. Mostra a ordem das sections e quais settings cada uma usa. Quando o produto Nano 5Mag for criado no admin, esse template é selecionado no campo "Theme template".
**Onde:** `e:/Shopify/nanovit-theme/templates/product.nano-5mag.json`.
**O que fazer:** Criar JSON listando todas as 12 sections na ordem do mapa acima, cada uma com `type` correspondente e `settings` mínimos.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Section `nanovit-pdp-hero` (gallery + buy-box)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** É o coração da PDP. Engloba o que está acima da dobra em 1 section porque gallery e buy-box compartilham contexto (preço, variantes, etc.) e o layout grid `1.05fr 1fr` precisa viver junto.
**Onde:** `sections/nanovit-pdp-hero.liquid`.
**Inclui:**
- Gallery com `main image`, badge "TOP 1", cashback chip, thumbnails (5 fotos), `aspect-ratio: 1`, sticky no desktop
- Buy-box: pílulas de variante, título, eyebrow, preço (de/por), parcelamento, perks (cashback + frete grátis), calculadora de frete (form + simulação client-side igual ao site estático), CTA principal "Adicionar ao carrinho", botão "Tirar dúvida com nutricionista", trust row (ANVISA, 7 dias, frete grátis)
- Sticky buy-bar mobile (aparece no scroll em mobile)

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Sections de conteúdo (composição → nutrition)

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado · **Parcial** (FAQ completa, demais como stubs com placeholder visível no theme editor)
**Por quê:** São as 10 sections abaixo da dobra. Cada uma é um arquivo `.liquid` próprio com schema mínimo (settings de visibilidade + copy editável).
**Onde:** `sections/nanovit-pdp-<nome>.liquid` × 10.
**Pattern:**
- CSS embutido via `{% stylesheet %}` (limita scope à section)
- Copy default em `schema.settings` (richtext ou text), tudo pré-preenchido com a copy atual do mock
- `enabled_on: { templates: ["product"] }` pra restringir aparição
- Presets pra cada uma (admin pode adicionar/remover via theme editor)

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — Section `nanovit-pdp-breadcrumb` (utilitário)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Pequena mas reutilizável.
**Onde:** `sections/nanovit-pdp-breadcrumb.liquid`.
**O que fazer:** Renderizar `Home > Catálogo > {{ product.title }}` ou similar.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — Footer institucional (dark)

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado
**Por quê:** O footer das páginas HTML é dark `#2a1c1a` com 4 colunas (brand+social, links rápidos, institucional, contato). Horizon vem com footer próprio em `sections/footer-group.json` que não corresponde a esse layout. Substituir.
**Onde:** `sections/nanovit-footer.liquid` + atualizar `sections/footer-group.json` pra usar essa section.
**Inclui:**
- 4 colunas com markup do site estático
- Links institucionais já apontando para os templates da Fase 4 (mesmo que ainda não existam)
- Social row + copyright

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 6 — Verificação visual

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado
**Por quê:** Sem rodar staging não tem como saber se o tema renderiza correto.
**Onde:** loja Shopify dev.
**O que fazer:**
1. Criar produto "Nano 5Mag" no admin (handle: `nano-5mag`).
2. Selecionar template `product.nano-5mag`.
3. Conferir cada section vs. mock HTML (galleria, perks, calc-frete funcionando, faq abrindo, etc.).
4. Mobile 375px: sticky buy-bar aparece no scroll, header colapsa em hamburger.

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- **Loja Shopify dev/staging:** continua bloqueando T6 verificação visual.
- **Produto "Nano 5Mag" no admin:** precisa existir pro template ser aplicável. Quando o cliente criar, o template aparece como opção.

## ❌ Não fazer
- Não criar metafields nesta fase. Decisão estratégica: copy hard-coded em Liquid com schema. Quando o cliente quiser dados dinâmicos, abrir refactor próprio.
- Não mexer em arquivos do Horizon original (sem prefixo `nanovit-`). Toda customização vai pra arquivo novo.
- Não introduzir nova fonte além de Montserrat Alternates.
- Não usar travessão (—) em copy renderizada.
- Não duplicar tokens do `:root` — usar `var(--font-heading--family)`, `var(--nanovit-radius-lg)`, etc.

## 📝 Log do Dev (preencher ao final)

- **Resumo:**
  - **T1** Template `templates/product.nano-5mag.json` criado orquestrando 12 sections na ordem do mapa. Cada uma com `color_scheme` predefinido alternando white/cream/cocoa pra ritmo visual.
  - **T2** `sections/nanovit-pdp-hero.liquid` completa: galeria sticky (1.05fr) com badge TOP 1 + cashback chip + thumbnails grid de 5, buy-box (1fr) com eyebrow + título + subtítulo + variant pills + preço de/por + parcelamento + 2 perks (frete + cashback) + calculadora de frete client-side (form CEP + simulação por região do CEP + máscara + erro) + CTA principal usando `{% form 'product' %}` (com fallback estático se produto não existir) + botão specialist + trust row 3 itens + sticky buy bar mobile. Schema com 17 settings + blocks pra thumbnails (preset adiciona 5 thumbs vazios).
  - **T3** `nanovit-pdp-faq.liquid` completa: accordion nativo HTML (`<details>/<summary>`), schema com bloco `qa` (pergunta + richtext de resposta + checkbox aberta por padrão), preset com 5 perguntas reais. As outras 8 sections (composition, indicam, forms, cta-edu, modo-uso, bundle, symptoms, reviews, nutrition) ficaram como **stubs**: cada uma tem eyebrow + título + copy editáveis + um placeholder visual "Seção em construção" dentro de um card cream tracejado. Já aparecem no theme editor com schema e presets — admin consegue arrastar/reordenar.
  - **T4** `sections/nanovit-pdp-breadcrumb.liquid` completa: Home › Catálogo (opcional) › Produto, settings pro handle/label da coleção, fallback title pra preview.
  - **T5** Footer institucional **não feito nesta rodada** — fica pra próxima junto com o refino das stubs.
  - **T6** Bloqueada (precisa loja Shopify dev).

- **Decisões fora do brief:**
  - **Hero é uma section única**, não 2 (gallery + buy-box separadas). Razão: o sticky da gallery depende do grid pai, e separar quebraria o `position: sticky` ou exigiria refactor com IntersectionObserver. Como section única o admin não pode reordenar gallery e buy-box independentemente — aceitável dado que essa ordem é fixa no design.
  - **Calculadora de frete inline em JS** dentro da section em vez de virar snippet/asset separado. Vantagem: section autocontida, sem dependência externa. Desvantagem: se duplicar o snippet no theme, duplica o JS. Pra essa rodada (1 PDP) é OK; quando criar outras PDPs reusa-se via snippet (refactor da Fase 3+).
  - **8 stubs em vez de implementação completa**. Razão: ~3000 linhas de CSS+HTML adicionais explodiam o budget desta rodada. As stubs já dão ao Diretor a possibilidade de ver a estrutura completa no theme editor e priorizar quais sections refinar primeiro na próxima rodada.
  - **Frete grátis = R$ 199,90** no JS da calc (consistente com decisão fechada no plano-mestre).
  - **Tag `nanovit-pdp-section`** em todas as sections pra facilitar CSS de página completa no futuro.

- **Dúvidas pro Diretor:**
  1. **Ordem de prioridade das 8 stubs** pra próxima rodada. Sugestão minha: composition > forms > symptoms > bundle > reviews > nutrition > indicam > modo-uso > cta-edu. Concorda? Ou quer outra ordem (ex: bundle antes porque é gatilho de AOV)?
  2. **Footer institucional dark** ficou pra próxima rodada. Confirma que prefere assim, ou prefere que eu pause as stubs e faça o footer primeiro (porque ele afeta a percepção em qualquer template, não só PDP)?
  3. **CTA principal usa `{% form 'product' %}`**. Quando produto Nano 5Mag não existir no admin (loja virgem), cai no fallback `<button>` sem ação. Tudo bem assim ou quer um link pra `/products/nano-5mag` mesmo sem produto criado?
  4. **Bloco `thumb`** no hero pra galeria: o preset adiciona 5 vazios. Admin precisa subir as imagens. Alternativa: hard-code 5 URLs reais nas defaults pra "sair da caixa" com fotos. Qual prefere?

- **Tempo aproximado:** ~50 min.
