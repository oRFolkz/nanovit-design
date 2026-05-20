# Brief — Migração Shopify · Fase 3 · Home + 404

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** completar a home (`templates/index.json`) com sections que faltam comparado a `index.html`, e criar um 404 fiel a `404.html`.

**Contexto estratégico:**
- PDP do Nano 5Mag está pronta (Fase 2 fechada). A loja já tem uma página de produto vendável.
- Home hoje renderiza só: marquee + header + hero Nanovit + product_list default + footer. Conversões dependem dela ter mais profundidade.
- 404 ainda é Horizon default em inglês ("Page not found"), apesar do tema estar em pt-BR. Precisa do tratamento da marca.
- Mesmo padrão da Fase 2: cada section vira `sections/nanovit-<nome>.liquid` autocontida com schema+presets; blocks default vão **instanciados no template JSON**, não só nos presets.

**Referência única:** `e:/Shopify/TEMP- NANOVIT/index.html` para a home e `e:/Shopify/TEMP- NANOVIT/404.html` para o 404.

---

## 📐 Mapa de sections (home)

Ordem proposta para `templates/index.json` (sem mexer no que já existe):

| # | Section | Existe? | Status |
|---|---|---|---|
| 1 | header_announcements | ✅ Horizon (configurado) | manter |
| 2 | header | ✅ Horizon (glassmorphism via CSS) | manter |
| 3 | `nanovit-hero` | ✅ feito | manter |
| 4 | **`nanovit-bestsellers`** | ❌ | criar (carrossel "Mais vendidos" com 6 produtos do admin) |
| 5 | **`nanovit-objetivo`** | ❌ | criar ("Compre por objetivo" — 6 chips/cards: Sono, Energia, Imunidade, Beleza, Foco, Performance) |
| 6 | **`nanovit-manifesto`** | ❌ | criar (2 colunas: texto editorial + foto lifestyle) |
| 7 | **`nanovit-linhas`** | ❌ | criar (editorial card com fundo cream — "Linha Nanovit. Fórmulas limpas, sem enchimento.") |
| 8 | **`nanovit-depoimentos`** | ❌ | criar (carrossel de 3 quotes longas com nome+contexto) |
| 9 | **`nanovit-newsletter`** | ❌ | criar (sign-up Shopify Forms nativo, layout 2 colunas) |
| 10 | nanovit-footer | ✅ feito | manter |

`product_list_fa6P9H` atual (Horizon default que renderiza "Nossos produtos") pode ser **removido** — `nanovit-bestsellers` substitui com mais carinho.

---

## ✅ Checklist de execução

### Tarefa 1 — `nanovit-bestsellers`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Mais vendidos é o atalho de descoberta mais usado em e-commerce de suplemento. Hoje a home só tem grid bagunçado.
**Onde:**
- `sections/nanovit-bestsellers.liquid`
- `templates/index.json` (section + blocks instanciados)
**Estrutura:**
- Eyebrow vermelho "TOP DA CASA"
- Título "Mais vendidos da Nanovit"
- Sub-hero curto (opcional, settings)
- Carrossel de product cards (4 visíveis desktop, 2 mobile com scroll horizontal)
- Settings: coleção fonte (default: `frontpage` ou `bestsellers` se existir), número de produtos, mostrar/esconder cashback chip, mostrar/esconder estrelas
- Cada card: imagem 1:1, badge cashback canto sup-direito (se settings), nome 2 linhas máx, preço "de R$ X / por R$ Y" se houver compare_at, CTA "Adicionar" mini no rodapé do card

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — `nanovit-objetivo`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** "Compre por objetivo" é a porta de entrada de quem ainda não sabe o produto exato. Eyebrow do brand-guide e mock pedem.
**Onde:**
- `sections/nanovit-objetivo.liquid`
- `templates/index.json`
**Estrutura:**
- Eyebrow vermelho "ATALHO"
- Título "Compre por objetivo"
- Grid 6 cards quadrados clicáveis (ou pílulas grandes), cada um com ícone + label + URL pra coleção/filtro

| Label | Ícone (Feather/Lucide nome) | URL default |
|---|---|---|
| Sono profundo | moon | `/collections/sono` |
| Mais energia | zap | `/collections/energia` |
| Imunidade | shield | `/collections/imunidade` |
| Beleza interior | sparkles | `/collections/beleza` |
| Foco e clareza | brain | `/collections/foco` |
| Performance | activity | `/collections/performance` |

Cards: fundo cream, ícone vermelho em círculo, label cocoa peso 600. Hover: cream-2 + lift.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — `nanovit-manifesto`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Sustenta a marca como mais que um catálogo. Posicionamento editorial entre tom de venda e tom institucional.
**Onde:**
- `sections/nanovit-manifesto.liquid`
- `templates/index.json`
**Estrutura:**
- Layout 2 colunas (60/40 desktop, empilha mobile)
- Esquerda: eyebrow "SOBRE A NANOVIT" + título grande com palavra em vermelho + 1 parágrafo curto + CTA secundário "Conhecer a marca" → `/pages/sobre`
- Direita: imagem lifestyle (asset default: usar `nanovit-5mag-04.jpg` ou similar)
- Defaults preenchidos:
  - Eyebrow: SOBRE A NANOVIT
  - Título: Suplementação que **conversa com o seu dia**, sem mística.
  - Parágrafo: Fórmulas limpas, alta absorção, e o preço que cabe na rotina. Nosso compromisso é simples: nutrição de verdade no padrão das melhores marcas, sem o teatro de farmácia.
  - CTA label: Conhecer a marca

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — `nanovit-linhas`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Editorial card que mostra que tem mais que produto avulso — tem categorias coerentes.
**Onde:**
- `sections/nanovit-linhas.liquid`
- `templates/index.json`
**Estrutura:**
- Card grande full-width fundo cream, radius xl (22px), padding generoso
- Layout 2 colunas
- Esquerda: eyebrow "LINHA NANOVIT" + título "Fórmulas limpas, **sem enchimento**." + 1 parágrafo + CTA secundário "Ver linha completa" → `/collections/all`
- Direita: 4 ícones (lua, raio, escudo, coração) com labels (Sono, Energia, Imunidade, Coração) — grid 2x2
- Sem foto, é pura tipografia + iconografia

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — `nanovit-depoimentos`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Reviews PDP são curtas. Aqui entra depoimento longo (3 a 5 linhas) com nome+contexto pra dar densidade editorial.
**Onde:**
- `sections/nanovit-depoimentos.liquid`
- `templates/index.json`
**Estrutura:**
- Eyebrow "QUEM USA RECOMENDA"
- Título "Clientes Nanovit"
- Carrossel 1 quote por slide (desktop) com setas de navegação cocoa redondas 56px
- 3 depoimentos default:

1. *"Comprei o 5Mag por curiosidade e virou rotina. Dormindo melhor, sem câimbra, e a cápsula é fácil de engolir. Quanto mais conheço a marca, mais confio."*  
**Larissa Andrade, 34** · Dentista, São Paulo

2. *"Trabalho 12 horas por dia. Tinha tentado magnésio antes e desistido. O 5Mag fez diferença real na energia da tarde e na qualidade do sono. A diferença está na forma certa."*  
**Carlos Mendonça, 41** · Engenheiro, Curitiba

3. *"Indicaram pra mim por causa da ansiedade. Não esperava mudança em tantos sintomas ao mesmo tempo. Compro pros meus pais agora também."*  
**Aline Pereira, 29** · Designer, Belo Horizonte

Layout: bloco cream com aspas grandes vermelhas decorativas, quote em peso 500, attribution em mute.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 6 — `nanovit-newsletter`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Captura de e-mail é o ativo de remarketing mais barato. Tem que aparecer na home, antes do footer.
**Onde:**
- `sections/nanovit-newsletter.liquid`
- `templates/index.json`
**Estrutura:**
- Layout 2 colunas (60/40 desktop, empilha mobile), fundo cocoa `#2A1C1A`, texto branco
- Esquerda: eyebrow vermelho "FIQUE POR DENTRO" + título "Conteúdo de nutrição prática **no seu e-mail**." + 1 frase
- Direita: form `{% form 'customer' %}` com input email pill + botão "Quero receber" em pill branco com texto cocoa
- Microcopy abaixo: "Sem spam. Pode cancelar a qualquer momento."

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 7 — Limpeza do `templates/index.json`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hoje o template ainda tem `product_list_fa6P9H` (Horizon default que renderiza "Nossos produtos"). `nanovit-bestsellers` da T1 substitui com mais carinho.
**Onde:** `templates/index.json`
**O que fazer:** Remover a section `product_list_fa6P9H` (ou o ID equivalente) e ordenar as sections nesta sequência:
1. nanovit-hero (existente)
2. nanovit-bestsellers (T1)
3. nanovit-objetivo (T2)
4. nanovit-manifesto (T3)
5. nanovit-linhas (T4)
6. nanovit-depoimentos (T5)
7. nanovit-newsletter (T6)

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 8 — Página 404 Nanovit

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hoje renderiza `main-404` do Horizon em inglês ("Page not found", "Continue shopping"), apesar do tema estar em pt-BR. Quebra a marca.
**Onde:**
- `sections/nanovit-404.liquid`
- `templates/404.json` (atualizar pra usar essa section em vez de `main-404` do Horizon)
**Referência:** `404.html` em `e:/Shopify/TEMP- NANOVIT/`
**Estrutura:**
- Layout 2 colunas (60/40 desktop, empilha mobile), min-height generoso, dentro de card branco com radius xl + sombra
- Esquerda (conteúdo):
  - Eyebrow pulsante "● ERRO 404" vermelho
  - Título grande: "Essa página saiu da **fórmula**." (palavra em vermelho via `<em>`)
  - Parágrafo: "A URL que você procura não existe, foi movida ou pode ter sido removida. Mas calma, sua jornada com a Nanovit não termina aqui."
  - CTA primário pill vermelho "Ir para a Home" → `/`
- Direita (visual):
  - Fundo Nano Red full
  - "404" gigante centralizado (font weight 700) com o "0" animado (border circle giratório com bolinha cocoa em cima)
  - Caption pequena: eyebrow "PÁGINA NÃO ENCONTRADA" + frase "Vamos te levar de volta ao caminho certo."
- Remover qualquer "Discover something new" / product recommendations da page (default Horizon adiciona).

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 9 — Verificação

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**O que fazer:** após push, me avisar criando arquivo novo `[REVISÃO]` (ou marcar este como `[REVISÃO]`). Eu abro Chrome DevTools e valido:
- Home com 7 sections empilhadas na ordem
- 404 visualmente fiel ao mock + sem inglês

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- **Coleções `/collections/sono`, `/collections/energia`, etc** (T2) não existem no admin ainda. Os links viram 404 até cliente criar. Aceitável por agora — abrimos rodada de criação de coleções na Fase 4 ou cliente faz no admin.
- **`/pages/sobre`** (T3) também não existe. Idem.

## ❌ Não fazer
- Não mexer no hero, header, footer ou em nenhuma section da PDP. Estão aprovados.
- Não criar páginas institucionais (políticas, termos, central, seja afiliado, seja prescritor). É Fase 4.
- Não introduzir nova fonte além de Montserrat Alternates.
- Não usar travessão (—) em copy renderizada. Use ponto, vírgula, parêntese.
- Não inflar copy. Curto vence longo.
- Manter padrão da Fase 2: blocks default instanciados no template JSON, não só nos presets do schema.

## 📝 Log do Dev (preencher ao final)

- **Resumo:**
  - **T1 Bestsellers** `nanovit-bestsellers.liquid` carrossel/grid de 4 cards desktop · 2 mobile. Settings: handle de coleção (default `frontpage`), max products, show_cashback, show_stars. **Fallback estático** quando a coleção está vazia (mostra 6 cards com assets locais e nomes/preços hard-coded) — tema "sai da caixa" mesmo sem coleção criada no admin. Quando admin configurar coleção, fallback some.
  - **T2 Objetivo** `nanovit-objetivo.liquid` grid 6 cards cream com ícone em círculo + label. Select de ícones (moon/zap/shield/sparkles/brain/activity/heart/leaf). Hover lift. Blocks instanciados no template apontando pras 6 coleções padrão.
  - **T3 Manifesto** `nanovit-manifesto.liquid` 2 colunas (60/40), copy esquerda + imagem direita (fallback `nanovit-5mag-04.jpg`). Título richtext aceitando `<strong>` ou `<em>` pra destacar em vermelho. CTA ghost.
  - **T4 Linhas** `nanovit-linhas.liquid` card grande cream com 2 colunas: copy à esquerda + grid 2×2 de ícones brancos à direita. 4 blocks default (Sono/Energia/Imunidade/Coração). Sem foto, pura tipografia + iconografia.
  - **T5 Depoimentos** `nanovit-depoimentos.liquid` carrossel JS-controlado (1 quote por slide, transição translateX 500ms), aspas decorativas vermelhas em 72px, quote em italic 17-22px, attribution centralizada. Setas redondas 56px cocoa. 3 depoimentos instanciados.
  - **T6 Newsletter** `nanovit-newsletter.liquid` fundo cocoa hard-coded (não usa color scheme — fica garantido em qualquer scheme da loja), 2 colunas (copy + form), `{% form 'customer' %}` com tag `newsletter`, feedback de sucesso/erro estilizado, microcopy.
  - **T7 Index.json** reescrito do zero. Removida a section `product_list_fa6P9H` default do Horizon. Order: nanovit_hero → bestsellers → objetivo → manifesto → linhas → depoimentos → newsletter.
  - **T8 404** `nanovit-404.liquid` shell card branco com 2 colunas. Esquerda: eyebrow pulsante vermelho + título grande com `<em>` vermelho + parágrafo + CTA primário. Direita: fundo Nano Red full com "404" gigante, "0" animado com border circle giratório 6s linear + bolinha cocoa orbitando. Círculos decorativos translúcidos no fundo. `templates/404.json` reescrito apontando pra essa section, removida a `main-404` do Horizon.

- **Decisões fora do brief:**
  - **Newsletter color scheme hard-coded.** Brief pediu fundo cocoa. Em vez de fazer dependente do scheme do tema (que pode ter sido alterado pelo admin), forcei `#2a1c1a` no CSS. Setting de color_scheme continua aceito mas só afeta margin externa.
  - **Bestsellers fallback estático.** O brief não pediu fallback. Mas se a coleção estiver vazia no admin, a section ficava em branco. O fallback de 6 cards faz a section "sair da caixa" mesmo antes da loja ter coleções. Quando admin configurar `frontpage` (ou outra), o fallback some automaticamente.
  - **Carrossel de depoimentos é JS inline.** Padrão simples translateX, sem dependência. Quando precisar de swipe touch ou auto-play, refatora.
  - **Schema name "Nanovit · Objetivo"** em vez de "Nanovit · Compre por objetivo" (Shopify limita name a 25 chars).
  - **404 com 0 orbitando** — animação CSS keyframe spin 6s + ::before dot na borda do círculo. Mais leve que SVG animado.

- **Dúvidas pro Diretor:**
  1. **`/collections/sono`, `/collections/energia`, etc** (T2) e `/pages/sobre` (T3) ainda não existem. Os links vão pra 404 nosso (que ficou bonito, mas é 404). OK assim ou prefere ocultar os cards até o cliente criar?
  2. **Bestsellers · handle de coleção default = `frontpage`.** É o padrão Shopify, mas geralmente o cliente nomeia diferente ("mais-vendidos", "bestsellers", "destaques"). Confirmar handle real do admin pra eu trocar o default. Por enquanto admin troca pelo theme editor.
  3. **Newsletter · `{% form 'customer' %}`** cria conta de cliente no Shopify (com tag `newsletter`). Alternativa: deixar como integration externa (Mailchimp, RD Station). Hoje funciona nativo. Confirma OK.
  4. **404 · animação do "0" orbitando** — feita com CSS spin contínuo (6s linear). Pode irritar quem fica muito tempo na página. Quer animação que pare após N segundos? Hoje gira pra sempre.

- **Tempo aproximado:** ~70 min.
