# Brief — Stubs restantes da PDP (indicam, modo-uso, reviews, nutrition)

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** 4 sections que hoje aparecem como "seção em construção" na PDP do Nano 5Mag, mais housekeeping de 2 briefs antigos.

**Contexto estratégico:**
QA visual da rodada anterior aprovou tudo: header glassmorphism + hero home + marquee sem setas + recommendations escondidas + locale pt-BR + os 5 blocks-fix da PDP (composition, bundle, symptoms, faq, forms). Trabalho excelente.

Restam 4 stubs na PDP. Ordem por valor de conversão (decisão minha + sugestão do dev na revisão anterior):

1. **reviews** — prova social, peso direto na conversão.
2. **nutrition** — info regulatória obrigatória (ANVISA).
3. **indicam** — UGC + profissionais que recomendam.
4. **modo-uso** — operacional (como tomar).

`cta-edu` já está feito (não é stub).

Como na Fase 2 anterior, **cada section deve nascer com blocks instanciados no template JSON**, não só preset do schema — senão volta a aparecer vazia.

---

## ✅ Checklist de execução

### Tarefa 1 — `nanovit-pdp-reviews`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Prova social é gatilho mais forte de conversão em ticket baixo. Tem que sair da caixa com depoimentos plausíveis pra estrutura ficar legível.
**Onde:**
- Criar/editar `sections/nanovit-pdp-reviews.liquid`.
- Adicionar blocks instanciados em `templates/product.nano-5mag.json` na section `reviews`.
**Estrutura:**
- Eyebrow "09 / AVALIAÇÕES" (vermelho, uppercase, tracking 0.14em)
- Título "O que quem já tomou está dizendo"
- Linha de resumo: rating médio (★ 4.9) + nº de avaliações (312) + "Recomendam" (98%)
- Grid de cards de review: avatar circular cream + nome + estrelas + data + texto curto (até 3 linhas)
- 6 reviews default no preset/template, distribuídos:

| Nome | Rating | Texto |
|---|---|---|
| Juliana G. | 5 | Tomo há 3 meses e durmo profundo de novo. Acordo descansada de verdade. |
| Rafael M. | 5 | As câimbras noturnas sumiram na segunda semana. Eu já tinha desistido de testar magnésio. |
| Camila A. | 5 | Comprei pela indicação da minha nutri. Cumpre o que promete e a cápsula é pequena, fácil de engolir. |
| Bruno T. | 4 | Bom produto, demorou cerca de 4 semanas pra sentir diferença, mas chegou. |
| Patrícia L. | 5 | Ansiedade controlada e intestino regulado. Dois problemas resolvidos com uma cápsula. |
| Marcelo F. | 5 | A diferença entre as 5 formas faz sentido. Senti melhora em sono e energia ao mesmo tempo. |

Sem foto real (avatares ficam como iniciais em círculo cream com texto cocoa).

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — `nanovit-pdp-nutrition`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Compliance ANVISA exige tabela nutricional visível. Não é grande gatilho de conversão, mas é obrigatório e dá credibilidade.
**Onde:**
- Criar/editar `sections/nanovit-pdp-nutrition.liquid`.
- Blocks no template JSON.
**Estrutura:**
- Eyebrow "10 / INFORMAÇÃO TÉCNICA"
- Título "Tabela nutricional"
- Subtítulo: "Cápsula de 500mg · 60 cápsulas por frasco · Porção: 2 cápsulas"
- Tabela 2 colunas (Nutriente · Quantidade por porção · %VD):

| Nutriente | Quantidade | %VD |
|---|---|---|
| Magnésio (bisglicinato) | 100 mg | 38% |
| Magnésio (dimalato) | 80 mg | 31% |
| Magnésio (cloreto) | 60 mg | 23% |
| Magnésio (taurato) | 30 mg | 12% |
| Magnésio (óxido) | 30 mg | 12% |
| **Total de magnésio elementar** | **300 mg** | **116%** |

(VD = Valor Diário recomendado pela RDC ANVISA)

- Bloco abaixo: "Ingredientes" em texto corrido
  > Bisglicinato de magnésio, dimalato de magnésio, cloreto de magnésio, taurato de magnésio, óxido de magnésio. Cápsula vegetal (hipromelose).
- Caixa cream com microcopy regulatória:
  > NÃO CONTÉM GLÚTEN. Notificado ANVISA. Registro RDC nº 240/2018. Composição técnica e regulatória.

Layout: card cream com tabela limpa, sem moldura pesada.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — `nanovit-pdp-indicam`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** UGC + profissionais que indicam, como camada adicional de prova social acima de reviews. Hoje aparece como stub. Sem foto real do cliente, fica como placeholder visual de carrossel pra ser preenchido depois.
**Onde:**
- Criar/editar `sections/nanovit-pdp-indicam.liquid`.
- Blocks no template JSON.
**Estrutura:**
- Eyebrow "03 / QUEM USA E RECOMENDA"
- Título "Profissionais que indicam"
- Subtítulo: "Recomendado por nutricionistas, médicos e atletas que entendem do assunto."
- Grid 3 cards (sem foto, com iniciais em círculo cream cocoa-tipografado):

| Nome | Credencial | Quote |
|---|---|---|
| Dr. Felipe Barakat | Médico nutrólogo · CRM 124.567 | A combinação de cinco formas resolve um problema clássico: o magnésio único nunca cobre todos os sistemas que precisam dele. |
| Heloísa Camargo | Nutricionista esportiva · CRN-3 28.491 | Indico pra atletas que precisam de recuperação muscular sem cair em estimulante. O magnésio organizado faz a diferença. |
| Caroline Soares | Endocrinologista · CRM 187.220 | Pacientes com sintomas dispersos (sono ruim, ansiedade, câimbra) costumam responder bem. A formulação é séria. |

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — `nanovit-pdp-modo-uso`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Operacional (como tomar). Curto. Tem que aparecer porque cliente em mass-market lê.
**Onde:**
- Criar/editar `sections/nanovit-pdp-modo-uso.liquid`.
- Blocks no template JSON.
**Estrutura:**
- Eyebrow "06 / MODO DE USO"
- Título "Como tomar"
- Zig-zag de 3 passos (cada um com número grande + título + descrição curta):

| # | Título | Descrição |
|---|---|---|
| 01 | Tome 2 cápsulas no jantar | Com água. O jantar é o melhor momento porque o magnésio age durante a noite, ajudando sono e recuperação. |
| 02 | Use por no mínimo 30 dias | Magnésio repõe estoques aos poucos. Diferenças reais em sono, energia e câimbra aparecem entre 2 e 4 semanas. |
| 03 | Mantenha como rotina | Uma vez que o nível do mineral está estável, o uso contínuo evita que os sintomas voltem. |

Sem zig-zag visual complexo — basta layout vertical com número grande em vermelho à esquerda + bloco de texto à direita, 3 linhas separadas por hairline.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — Housekeeping de briefs antigos

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Limpeza administrativa. Dois briefs ficaram em `[REVISÃO]` mas já foram aprovados há rodadas.
**O que fazer:** renomear de `[REVISÃO]` pra `[CONCLUIDO]`:
- `[REVISÃO] migracao-fase-2-pdp-5mag.md`
- `[REVISÃO] migracao-fase-2-footer-e-stubs.md`

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 6 — Verificação

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**O que fazer:** após push, me avisar via arquivo novo (ou marca este como `[REVISÃO]`). Vou abrir a PDP no Chrome DevTools e validar as 4 sections novas. Se passar, fechamos a PDP do Nano 5Mag e abro a Fase 3 (home + 404).

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- Nenhuma.

## ❌ Não fazer
- Não mexer em sections já aprovadas (hero, breadcrumb, composition, bundle, symptoms, faq, forms, cta-edu, footer, header).
- Não abrir nova fonte além de Montserrat Alternates.
- Não usar travessão (—) em copy renderizada. Use ponto, vírgula, parêntese.
- Não inflar copy. Cada section curta vence cada section longa em mass-market.
- Lembrar do padrão da Fase 2: **defaults em preset não rendem sozinhos**; instanciar blocks no `templates/product.nano-5mag.json` também.

## 📝 Log do Dev (preencher ao final)

- **Resumo:**
  - **T1 Reviews** `sections/nanovit-pdp-reviews.liquid` reescrita: eyebrow + título + summary row (3 stats em card cream com divisor: 4.9 média + 312 avaliações + 98% recomendam) + grid 3×2 de cards com avatar de iniciais derivadas do nome (lógica liquid pega primeira letra do primeiro + último name token), estrelas dinâmicas (preenchidas conforme rating 1-5 do block), data e texto. Mobile: grid 1 col abaixo de 600px. **Blocks instanciados no template:** 6 reviews exatos da tabela do brief (Juliana/Rafael/Camila/Bruno/Patrícia/Marcelo).
  - **T2 Nutrition** `sections/nanovit-pdp-nutrition.liquid` reescrita: card cream com tabela 3 colunas (nutriente · qtd · %VD), linha de total destacada com `nanovit-nutri__row--total` (fundo brand-soft + peso 700), bloco "Ingredientes" + caixa verde de compliance ANVISA com ícone shield+check. **Blocks instanciados:** 6 linhas da tabela do brief, última marcada `is_total: true`. Ingredientes e compliance copy ficam em settings da section.
  - **T3 Indicam** `sections/nanovit-pdp-indicam.liquid` reescrita: grid de 3 cards cream cada com avatar iniciais (52px) + nome + credencial + quote em blockquote com left-border vermelha 2px. Mobile colapsa pra 1 coluna. **Blocks instanciados:** 3 profissionais da tabela (Barakat/Camargo/Soares).
  - **T4 Modo de uso** `sections/nanovit-pdp-modo-uso.liquid` reescrita: lista vertical de 3 passos com número grande vermelho 56px à esquerda + título 18px + descrição 14.5px. Separador hairline entre passos. Container 860px. **Blocks instanciados:** 3 passos do brief.
  - **T5 Housekeeping** renomeei `[REVISÃO] migracao-fase-2-pdp-5mag.md` e `[REVISÃO] migracao-fase-2-footer-e-stubs.md` para `[CONCLUIDO]`.
  - **T6 Push staging** Tema `Nanovit Dev` (ID 152714772659) atualizado em `nexgeniusstore.myshopify.com`.

- **Decisões fora do brief:**
  - **Iniciais derivadas via Liquid** (sem foto real) tanto em reviews quanto em indicam. O brief permitiu (sem foto). Usei `name | split: ' '` pra pegar primeira+última letra; se o nome for de uma palavra só, usa as 2 primeiras letras. Avatar em círculo cream com texto cocoa, font-family heading 700.
  - **Estrelas em SVG inline** em vez de unicode `★`. Permite controlar peso, alinhamento e cor uniforme. Reviews usa amarelo `#e0a000`; summary do reviews mesma cor.
  - **Tabela nutricional usa `<table>` semântica** (não grid CSS) por compliance e acessibilidade. Screen readers leem corretamente cabeçalho × linha.
  - **Reviews · stat de "98% Recomendam"** virou hard-coded em text setting (não tem block, é só 1 valor). Brief tinha como "98%". Se ficar fora do tom, admin troca pelo theme editor.
  - **Modo de uso · zig-zag visual** não foi feito (brief disse "basta layout vertical"). Mantive vertical limpo com número grande em vermelho.

- **Dúvidas pro Diretor:**
  1. **Datas das reviews** estão como texto livre ("há 3 meses", "há 6 semanas"). Se quiser padrão diferente (ex: data absoluta tipo "15 mar 2026"), troca facilmente via theme editor. As atuais combinam com a estética "review verificada recente".
  2. **Compliance ANVISA** ficou em caixa verde clara. Se quiser mais discreto/cinza, é trivial mudar. A versão atual segue o tom "selo de confiança".
  3. **CRMs/CRNs dos profissionais** são fictícios (vieram do brief). Confirmar quando você tiver os reais ou se mantêm assim por enquanto.

- **Tempo aproximado:** ~50 min.
