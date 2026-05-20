# Brief — Migração Shopify · Fase 4 · Páginas institucionais + coleções

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** criar todos os templates de páginas institucionais que hoje têm link mas dão 404, mais um template de coleção genérico que sirva pras 6 coleções por objetivo.

**Contexto estratégico:**
- PDP, Home e 404 estão fechados. A loja já vende e tem entrada estética coerente.
- Falta a camada institucional: páginas que aparecem em footer, header, manifesto, objetivo. Sem elas, todo link secundário cai em 404.
- **Estratégia para páginas estáticas (políticas, termos):** template único `page.legal.json` reutilizável. Cada página real no admin vai usar esse template e ter o conteúdo no campo `page.content`.
- **Estratégia para páginas editoriais (sobre, seja afiliado, seja prescritor, tecnologia):** template próprio com sections customizadas pra cada uma. São páginas de venda/conversão, não de leitura.
- **Coleções:** template único `collection.objetivo.json` com hero + grid de produtos. Cliente filtra produtos por tag/coleção no admin.

**Referência única:** HTMLs em `e:/Shopify/TEMP- NANOVIT/` (`politica-de-frete.html`, `politica-de-reembolso.html`, `termos-de-servico.html`, `central-de-atendimento.html`, `seja-um-afiliado.html`, `seja-um-prescritor.html`, `demo-tecnologia-nano.html`).

---

## 📐 Mapa de templates

| Template | Páginas que usam | Tipo |
|---|---|---|
| `page.legal.json` | politica-de-frete, politica-de-reembolso, termos-de-servico | Layout de leitura, conteúdo via `page.content` |
| `page.sobre.json` | sobre | Editorial: hero + pilares + foto + manifesto |
| `page.afiliado.json` | seja-afiliado | Conversão: hero + benefícios + comissão + FAQ + CTA |
| `page.prescritor.json` | seja-prescritor | Conversão similar a afiliado, copy diferente |
| `page.central.json` | central-de-atendimento | Hub: busca + categorias + FAQ + contato |
| `page.tecnologia.json` | tecnologia | Editorial técnico: hero + processo + diferencial + CTA |
| `collection.objetivo.json` | sono, energia, imunidade, beleza, foco, performance | Coleção: hero por objetivo + grid de produtos |

---

## ✅ Checklist de execução

### Tarefa 1 — Template `page.legal.json`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Políticas e termos são leitura, não venda. Layout calmo, max-width pra conforto, header simples.
**Onde:**
- `sections/nanovit-legal-content.liquid`
- `templates/page.legal.json`
**Estrutura:**
- Header padrão (header_announcements + header_section)
- Section `nanovit-legal-content`:
  - Container `max-width: 760px`, padding generoso
  - Eyebrow vermelho "DOCUMENTO LEGAL" (settings)
  - H1 = `{{ page.title }}`
  - Microcopy "Atualizado em [data]" (settings)
  - Render `{{ page.content }}` com tipografia limpa (h2 cocoa peso 600, parágrafos line-height 1.7, listas com bullets pequenos)
  - Hairline separador no topo e fim
- Footer nanovit
**Cliente cria 3 páginas no admin com este template:**
- `politica-de-frete` (handle), conteúdo do mock
- `politica-de-reembolso`, conteúdo do mock
- `termos-de-servico`, conteúdo do mock

**Notas do Dev:** > _(preencher após executar — listar arquivos criados e instrução pra cliente)_

---

### Tarefa 2 — Template `page.sobre.json` + section `nanovit-sobre`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Manifesto da marca, sustenta posicionamento.
**Onde:**
- `sections/nanovit-sobre.liquid` (autocontida, hero + pilares + foto + manifesto numa section só)
- `templates/page.sobre.json`
**Estrutura mínima (defaults preenchidos):**
- Hero: eyebrow "SOBRE A NANOVIT" + título grande "Suplementação séria, **sem o teatro de farmácia**." + 1 parágrafo
- Pilares (3 blocks): título + descrição
  1. Ciência sem academicismo — Falamos a partir de evidência: dose, forma química, absorção. Em português, sem jargão.
  2. Beleza funcional — Cada elemento da página tem razão de existir. Decoração só quando vira utilidade.
  3. Próximo, nunca infantil — Tratamos o cliente como adulto curioso. Sem mística, sem promessa mágica.
- Foto lifestyle full-width (asset existente)
- Manifesto fechamento: 1 parágrafo + assinatura "Nanovit Nutrition"

Cliente cria página no admin com handle `sobre`.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Templates `page.afiliado.json` e `page.prescritor.json`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Conversão pra programa de afiliação (cliente influencer) e prescritor (nutricionista, médico). Mesma estrutura, copy diferente.
**Onde:**
- `sections/nanovit-cadastro-programa.liquid` (reutilizável, copy 100% via settings)
- `templates/page.afiliado.json` + `templates/page.prescritor.json`
**Estrutura da section:**
- Hero 2 colunas: copy esquerda (eyebrow + título + sub + 2 CTAs) + foto direita
- Faixa "Como funciona" — 3 passos numerados (01/02/03 vermelho grande)
- Faixa "Benefícios" — 4 cards cream com ícone + título + descrição curta
- Faixa de comissão/contrapartida (cards grandes com %, valor ou condição)
- FAQ accordion (4 perguntas)
- CTA final full-width cocoa
**Defaults `afiliado`:**
- Eyebrow: PROGRAMA DE AFILIADOS
- Título: Indique Nanovit, **ganhe comissão recorrente**.
- Sub: Para criadores, influencers e clientes que recomendam suplementação séria. Comissão de 15% por venda, paga via PIX.
- Comissão: 15% sobre cada venda · pagamento mensal · sem mínimo de vendas
**Defaults `prescritor`:**
- Eyebrow: PROGRAMA PRESCRITOR
- Título: Prescreva Nanovit, **com cashback para o seu paciente**.
- Sub: Para nutricionistas, médicos e profissionais de saúde. Seu paciente compra com desconto e você ganha bonificação.
- Comissão: 10% de desconto pro paciente · 10% de cashback pra você · sem custo de adesão

Cliente cria 2 páginas no admin, handles `seja-afiliado` e `seja-prescritor`.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — Template `page.central.json` + section `nanovit-central`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hub de atendimento, reduz pressão em WhatsApp/e-mail.
**Onde:**
- `sections/nanovit-central.liquid`
- `templates/page.central.json`
**Estrutura:**
- Hero curto: eyebrow "CENTRAL DE ATENDIMENTO" + título "Como podemos **ajudar**?" + input de busca pill (não funcional, só visual nesta rodada — pode evoluir pra search real depois)
- Grid 6 cards de categoria (cream, radius xl, ícone vermelho no círculo + título + descrição + link):
  1. Pedidos e entregas
  2. Trocas e devoluções
  3. Pagamento
  4. Produtos e composição
  5. Programa de cashback
  6. Conta e cadastro
- FAQ accordion abaixo (6 perguntas mais comuns)
- Bloco final 2 colunas: WhatsApp grande + e-mail + horários de atendimento + CTA "Falar com a gente"

Cliente cria página no admin com handle `central-de-atendimento`.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — Template `page.tecnologia.json` + section `nanovit-tecnologia`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Educacional, sustenta diferencial técnico ("Nanocomprimido"). Linkada pela PDP cta-edu.
**Onde:**
- `sections/nanovit-tecnologia.liquid`
- `templates/page.tecnologia.json`
**Referência:** `demo-tecnologia-nano.html`
**Estrutura:**
- Hero cocoa full-width: eyebrow "TECNOLOGIA" + título "Como o **Nanocomprimido** funciona." + sub-hero técnica
- Processo em 4 passos (cards horizontais com número + título + descrição)
- Diferencial vs cápsula comum (comparativo 2 colunas)
- Bloco visual/animação (placeholder por enquanto — pode ser vídeo embed depois)
- CTA final "Conhecer o Nano 5Mag" → `/products/nano-5mag-dimalato-bisglicinato-cloreto-taurato-e-oxido-de-magnesio-60-caps-nanovit`

Cliente cria página no admin com handle `tecnologia`.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 6 — Template `collection.objetivo.json`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** As 6 coleções por objetivo apontadas em `nanovit-objetivo` na home precisam de template próprio.
**Onde:**
- `sections/nanovit-collection-hero.liquid`
- `templates/collection.objetivo.json`
**Estrutura:**
- Hero da coleção: eyebrow editorial (ex: "PARA QUEM QUER") + título "Sono profundo." (vem de `{{ collection.title }}`) + descrição (vem de `{{ collection.description }}`)
- Settings: cor de fundo do hero (default cream), imagem opcional à direita
- Grid de produtos (reaproveitar lógica do `nanovit-bestsellers` ou `_product-card`)
- CTA final "Não encontrou? Tente o quiz." → `/pages/quiz` (placeholder)

Cliente cria 6 coleções no admin com handles `sono`, `energia`, `imunidade`, `beleza`, `foco`, `performance`. Usa este template em cada uma. Atribui produtos por tag.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 7 — Documentação pro cliente

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Cliente precisa criar manualmente no admin: 7 páginas + 6 coleções, atribuindo cada uma ao template certo. Sem instruções, ele não sabe o que fazer.
**Onde:** criar `e:/Shopify/nanovit-theme/INSTRUCOES-ADMIN-FASE-4.md` (ou similar dentro do tema, fácil de achar).
**O que incluir:**
- Lista das páginas a criar com handle + template + conteúdo recomendado
- Lista das coleções a criar com handle + template
- Como atribuir template a uma página/coleção no admin
- Como atribuir produto a uma coleção via tag

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 8 — Verificação

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**O que fazer:** após push e o cliente criar as páginas/coleções no admin, me avisar via arquivo novo. Eu valido via Chrome DevTools cada rota.

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- **Cliente criar 7 páginas e 6 coleções no admin** após o tema subir com os templates. Sem isso, os links continuam dando 404.
- **`/pages/quiz`** continua placeholder. Se vamos ter quiz, é projeto próprio (app ou implementação custom) — fora de escopo desta fase.

## ❌ Não fazer
- Não mexer em hero/header/footer/sections já aprovadas.
- Não criar quiz/calculadora interativa (fora de escopo).
- Não introduzir nova fonte ou cor que não esteja no brand-guide.
- Não usar travessão (—) em copy renderizada. Use ponto, vírgula, dois pontos, parêntese.
- Não inflar copy. Páginas legais podem ser longas (é leitura); editoriais devem ser enxutas.
- Manter padrão das fases anteriores: blocks default instanciados no template JSON quando houver blocks repetíveis.

## 📝 Log do Dev (preencher ao final)

- **Resumo:**
  - **T1 Legal** `sections/nanovit-legal-content.liquid` + `templates/page.legal.json`. Container 760px, eyebrow + h1 (= page.title) + meta de atualização + render de `{{ page.content }}` com tipografia rte (h2/h3 cocoa 600, p line-height 1.75, ul/ol com bullets pequenos, strong/links em vermelho). Cliente cria 3 páginas no admin (frete/reembolso/serviço) e cola o conteúdo. Hairlines no topo e fim.
  - **T2 Sobre** `sections/nanovit-sobre.liquid` + `templates/page.sobre.json`. Hero (eyebrow + título richtext + lead) → 3 pilares com borda vermelha topo + número 01/02/03 + título + copy → foto lifestyle full → manifesto fechamento em italic centralizado com assinatura "Nanovit Nutrition" em eyebrow. 3 blocks de pilar instanciados no template.
  - **T3 Programa** `sections/nanovit-cadastro-programa.liquid` reutilizável (mesma section pra afiliado e prescritor). 5 bandas: hero 2 col + steps 3 col + benefits 4 col + commission 3 cards cocoa + FAQ + final CTA full-width cocoa. Settings 100% pra copy diferenciar afiliado vs prescritor. **2 templates** (`page.afiliado.json` e `page.prescritor.json`) com blocks completos instanciados (3 steps, 4 benefits, 3 commission, 4 qa). Block `commission` name encurtado pra "Card de comissão" (limite 25 chars).
  - **T4 Central** `sections/nanovit-central.liquid` + `templates/page.central.json`. Hero centralizado com input pill de busca (visual, não funcional) + grid 6 cards de categoria com select de 6 ícones + FAQ accordion com 6 QAs + bloco cocoa de contato 2 col (WhatsApp/email/horários com ícones).
  - **T5 Tecnologia** `sections/nanovit-tecnologia.liquid` + `templates/page.tecnologia.json`. Hero cocoa full-bleed (não respeita container) + 4 passos do processo em cards cream + comparativo 2 col (Nanocomprimido vs cápsula comum) com bullets verdes e cinzas + placeholder de vídeo (iframe quando media_url settado, senão placeholder tracejado) + CTA final com URL hard-coded pro produto Nano 5Mag.
  - **T6 Coleção** `sections/nanovit-collection-hero.liquid` + `templates/collection.objetivo.json`. Hero cream com 2 col (copy + imagem do collection.image ou settings) → grid 4 col de produtos paginados (24/página, default_pagination Shopify) → empty state quando coleção vazia → CTA quiz cocoa abaixo. Lê `collection.title` e `collection.description` direto do admin.
  - **T7 Docs** `INSTRUCOES-ADMIN-FASE-4.md` criado dentro do tema. Lista as 7 páginas + 6 coleções com handles, templates, content esperado, instruções de como atribuir template no admin e como usar smart collection com tag pra mapear produto→coleção.

- **Decisões fora do brief:**
  - **Section `nanovit-cadastro-programa` única, 2 templates.** Brief sugeriu mesma estrutura. Em vez de duplicar a section, criei uma só com 100% dos campos em settings e blocks; os dois templates JSON diferem apenas nos defaults preenchidos.
  - **Hero da Tecnologia full-bleed cocoa** estoura o container do tema. Foi proposital pro contraste editorial — o conteúdo abaixo volta pro container normal. Se ficar estranho na live, ajusto.
  - **Placeholder de mídia da Tecnologia** com ícone de play + texto "Demonstração visual em produção". Quando admin colar URL do YouTube embed em `media_url`, vira iframe responsivo 16:9.
  - **Block name 25 chars cap:** `Card de comissão/contrapartida` (30) → `Card de comissão` (16). Já tinha errado antes com `Nanovit · Compre por objetivo`.
  - **Página de Política de Privacidade** não está no escopo do brief mas tem link no footer e nos templates. Adicionei nota no `INSTRUCOES-ADMIN-FASE-4.md` sugerindo criação futura com template legal.
  - **`templates/page.json`** padrão do Horizon não foi tocado. Páginas que não usem nenhum dos templates novos caem nele.

- **Dúvidas pro Diretor:**
  1. **Busca da Central é visual.** Não tem JS funcional ainda. Quando rolar, vira input que filtra a FAQ accordion ou redireciona pra `/search?q=`. Confirma estratégia.
  2. **Hero cocoa full-bleed da Tecnologia** estoura o container. Visualmente combina com tom editorial, mas se você quiser confinar ao container 1320px, eu retiro o full-bleed.
  3. **CRMs/handles dos programas:** brief não definiu URL exata dos handles (`seja-afiliado` vs `programa-afiliado`). Fui de `seja-afiliado` e `seja-prescritor` por consistência com os HTMLs originais.
  4. **CTA primário das páginas de programa** aponta pra `#cadastro` (âncora interna). Não tem form de cadastro de fato — só rola até o CTA final. Quando tiver app/form externo (Shopify Forms, Typeform, app), troca a URL.

- **Tempo aproximado:** ~85 min.
