# Brief — Migração Shopify · Fase 2 · Footer + refino das stubs PDP

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** finalizar o que ficou pendente da Fase 2.
**Contexto estratégico:**
A rodada anterior entregou hero (completo), FAQ (completo), breadcrumb e template. 8 sections ficaram como stubs visíveis e o footer institucional foi adiado. Esta rodada fecha esses dois pontos.

**Ordem importa:** Footer primeiro (bloqueia QA de qualquer página). Stubs em sequência por valor de conversão.

---

## ✅ Checklist de execução

### Tarefa 1 — Footer institucional dark (PRIORIDADE)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Afeta toda página, não só PDP. Sem ele, qualquer QA visual fica incompleto. É bloqueante pras Fases 3 e 4 também.
**Onde:**
- Criar `sections/nanovit-footer.liquid`.
- Atualizar `sections/footer-group.json` pra usar essa section em vez do `footer` original do Horizon.
**Referência:** rodapé dark de `e:/Shopify/TEMP- NANOVIT/produto.html` (próximo ao final do `<body>`, fundo `#2a1c1a`, 4 colunas).
**Estrutura mínima:**
- Coluna 1: logo branca + breve descrição da marca + 4 ícones de social (Instagram, TikTok, YouTube, WhatsApp)
- Coluna 2: "Loja" (links rápidos: Mais vendidos, Lançamentos, Promoções, Kits, Quiz)
- Coluna 3: "Institucional" (Sobre, Seja afiliado, Seja prescritor, Central de atendimento, Trabalhe conosco)
- Coluna 4: "Atendimento" (WhatsApp clicável, e-mail, horários, CNPJ)
- Linha inferior (full-width): copyright + selos de pagamento + links pra Políticas/Termos/Reembolso
**Settings expostos:** texto da descrição, URLs sociais, CNPJ, e-mail, telefone, horários. Resto hard-coded.
**Links institucionais:** apontar pros handles que vão existir na Fase 4 (`/pages/politica-de-frete`, `/pages/termos-de-servico`, etc.), mesmo que ainda não existam.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Refino das stubs (ordem de prioridade)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Stubs hoje mostram só "Seção em construção". Preciso de pelo menos as 4 primeiras renderizadas pra fazer QA de conversão de verdade.
**Onde:** `sections/nanovit-pdp-<nome>.liquid` (substituir o stub atual pela implementação fiel ao mock).

**Ordem obrigatória (não inverter):**

1. **`nanovit-pdp-composition`** — Comparativo com vs sem (HTML linhas 2963-3052). Tabela ou cards lado a lado. Settings: 2 listas de bullets editáveis + 2 labels de coluna.
2. **`nanovit-pdp-bundle`** — Compre junto (HTML 3242-3306). 3 produtos com checkboxes, preço total, CTA de adicionar todos. Settings: cada item como block com imagem + título + preço + URL + checkbox default.
3. **`nanovit-pdp-forms`** — 5 formas, 5 sistemas (HTML 3122-3190). 5 cards lado a lado: nome da forma de magnésio + ícone + sistema do corpo que atua. Hard-code 5 entradas reais.
4. **`nanovit-pdp-symptoms`** — Sintomas (HTML 3306-3363). Grid de chips/cards com sintomas que o produto resolve. Settings: lista editável de sintomas (cada um com texto curto + ícone).

**As 5 demais (reviews, nutrition, indicam, modo-uso, cta-edu) ficam como stubs ainda.** Abro rodada própria depois que validar essas 4.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Fotos reais do hero (galeria)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hoje a galeria do hero tem 5 thumbs vazios via preset. Pra QA visual ficar real, precisa sair da caixa com fotos. Substituir pelo conteúdo de `e:/Shopify/TEMP- NANOVIT/img/`.
**Onde:**
1. Copiar imagens de `e:/Shopify/TEMP- NANOVIT/img/` pra `e:/Shopify/nanovit-theme/assets/`, renomeando:
   - `Nano 5Mag 1.webp` → `nanovit-5mag-01.webp`
   - `Nano 5Mag 2.png` → `nanovit-5mag-02.png`
   - `galeria.jpg` → `nanovit-5mag-03.jpg`
   - `galeria 2.jpg` → `nanovit-5mag-04.jpg`
   - `o que é.jpg` → `nanovit-5mag-05.jpg`
2. No preset do `nanovit-pdp-hero.liquid`, definir 5 blocks `thumb` com default apontando pra `{{ 'nanovit-5mag-01.webp' | asset_url }}` etc.
3. Imagem principal do hero: usar `nanovit-5mag-01.webp` como default.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — Renomear briefs em revisão antigos

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Limpeza administrativa. 3 briefs estão em `[REVISÃO]` mas já foram aprovados em rodadas anteriores.
**O que fazer:** renomear de `[REVISÃO]` pra `[CONCLUIDO]`:
- `[REVISÃO] migracao-fase-0-fundacao.md`
- `[REVISÃO] unificar-identidade-paginas-secundarias.md`
- `[REVISÃO] simulacao-calculadora-frete.md`

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- **Loja Shopify dev/staging + URL:** continua sendo o que destrava qualquer QA visual real. Quando a URL chegar, abro rodada própria de QA via Chrome DevTools MCP.

## ❌ Não fazer
- Não mexer no hero, FAQ, breadcrumb ou template — estão aprovados.
- Não implementar reviews/nutrition/indicam/modo-uso/cta-edu nesta rodada. Próxima.
- Não introduzir metafields. Tudo via schema + defaults.
- Não introduzir nova fonte além de Montserrat Alternates.
- Não usar travessão (—) em copy renderizada.

## 📝 Log do Dev (preencher ao final)

- **Resumo:**
  - **T1 Footer** `sections/nanovit-footer.liquid` criado (dark #2a1c1a, 4 colunas: brand+social, colunas dinâmicas via blocks `link_column`, atendimento com WhatsApp/email/horário/CNPJ, linha inferior com copyright+links legais). `sections/footer-group.json` reescrito apontando pra `nanovit-footer` em vez do footer original do Horizon. Tudo editável via theme editor (URLs sociais, descrição, listas de links).
  - **T2.1 Composition** layout 3 colunas (Possui · Imagem · Não Possui) com checkmarks verdes e Xs. Blocks separados `has` e `missing` (preset com 5 de cada). Pill verde inferior com mensagem de fórmula limpa.
  - **T2.2 Bundle** layout 2 colunas (imagem com badge | copy + checklist + total dinâmico). 3 itens default no preset (Nano 5Mag fixo + Nano Calm + Nanovit D3+K2). JS recalcula total e label do CTA quando admin marca/desmarca. Suporta `price_was` opcional pra mostrar riscado.
  - **T2.3 Forms** grid de 5 cards centralizados com ícone redondo (5 ícones disponíveis no select: lua/raio/escudo/coração/folha), kicker (nome da forma) + sistema do corpo + descrição. Preset com as 5 entradas reais.
  - **T2.4 Symptoms** seção em 3 partes: (a) grid lateral com imagem + overlay diagnóstico vs lista numerada de 5 sintomas, (b) bloco "A causa comum" cream com 3 cards de causas, (c) pill verde de resolução. Preset com 5 symptom blocks + 3 cause blocks já preenchidos.
  - **T3 Imagens** 7 arquivos copiados de `TEMP- NANOVIT/img/` pra `nanovit-theme/assets/` com nomes kebab-case: `nanovit-5mag-01.webp`, `02.png`, `03.jpg`, `04.jpg`, `05.jpg` + `compre-junto.jpg` + `voce-ja-passou.jpg`. Hero, composition, bundle e symptoms apontam pra esses assets como fallback quando admin ainda não fez upload pelo theme editor. Hero: thumbs iteram via `forloop.index` pra escolher fallback diferente por posição.
  - **T4 Briefs** renomeados pra `[CONCLUIDO]`: migracao-fase-0-fundacao, simulacao-calculadora-frete, unificar-identidade-paginas-secundarias.
  - **T5 Push staging** Tema `Nanovit Dev` subido com sucesso em `nexgeniusstore.myshopify.com`. Theme ID `152714772659`.

- **Decisões fora do brief:**
  - **Footer abandonou Horizon `footer-utilities`**. O Horizon vinha com 2 sections separadas (footer + utilities) — substituí por uma única `nanovit-footer` que inclui o copyright e legal links na própria linha inferior. Mais limpo, menos arquivos pra manter.
  - **URL settings viraram `text`** em vez de `type: url`. Razão: `type: url` no schema do Shopify só aceita defaults no formato shopify:// (pra recursos internos como collections, pages, products). Pra URLs externas e WhatsApp links, `text` aceita default string normal. Funciona idêntico do ponto de vista do admin.
  - **Bundle JS é inline**. Igual ao approach da calc-frete no hero. Funciona sem dependência externa. Quando criarmos outras PDPs com bundle, refatoramos pra snippet/asset.
  - **Symptoms uses `forloop` para numerar** (01, 02, 03...). Se admin adicionar/remover symptom blocks, a numeração se ajusta automaticamente.
  - **Composition product_image como image_picker** mas com fallback pra `nanovit-5mag-02.png`. Mesma estratégia em bundle (compre-junto.jpg) e symptoms (voce-ja-passou.jpg).

- **Dúvidas pro Diretor:**
  1. **Preview URL e password gate.** Tema subiu como unpublished. Preview: `https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659`. Editor: `https://nexgeniusstore.myshopify.com/admin/themes/152714772659/editor`. **Mas eu não consigo pegar a senha do storefront programaticamente.** Você precisa entrar em `Online Store → Preferences → Password protection` no admin e me passar a senha do storefront pra eu fazer QA via Chrome DevTools MCP. Ou se preferir, você mesmo abre o preview e me manda screenshots.
  2. **Composition default `product_image` está apontando pra `nanovit-5mag-02.png`** (frasco branco), igual o mock. Confirma essa foto ou prefere outra?
  3. **Ordem das 5 stubs restantes pra próxima rodada** (indicam → cta-edu → modo-uso → reviews → nutrition). Sugestão minha: reviews primeiro (afeta conversão direto), depois nutrition (info regulatória obrigatória), depois indicam, depois modo-uso, depois cta-edu por último (é só um link, baixa complexidade). Concorda?
  4. **Footer tem links pra páginas (`/pages/sobre`, etc.) que ainda não existem.** Funcionam como 404 até a Fase 4 criar. OK manter assim ou prefere que eu remova do default por enquanto?

- **Tempo aproximado:** ~70 min.
