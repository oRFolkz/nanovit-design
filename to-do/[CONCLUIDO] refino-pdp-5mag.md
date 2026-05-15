# Brief — Refino da PDP do Nano 5MAG (pré-aprovação do cliente)

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** `produto.html` apenas. `index.html` já foi aprovada pelo cliente; não tocar.
**Contexto estratégico:** esta PDP vai ser enviada ao cliente final (Nanovit) para aprovação de LAYOUT. O catálogo real do produto Nano 5MAG é **Bisglicinato + Dimalato + Cloreto + Taurato + Óxido** (briefing oficial em `5MAG_Briefing.docx.md`). O HTML hoje usa formas erradas (Treonato, L-Treonato, Citrato) — isso contradiz o produto e bloqueia a aprovação. Também faltam alguns ajustes de copy e uma seção visual central que está no briefing oficial mas não foi executada (mapa "5 formas → 5 sistemas").
**Escopo é apresentação, não produção:** dados estáticos, sem integração real necessária. Foco em impacto visual e clareza narrativa.

---

## ✅ Checklist de execução

### 🔴 BLOQUEADORES (precisam ir antes da apresentação)

### Tarefa 1 — Corrigir as 5 formas de magnésio em todos os locais

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Briefing oficial do produto define **Bisglicinato + Dimalato + Cloreto + Taurato + Óxido**. O HTML hoje cita Treonato/L-Treonato/Citrato. Esse erro é factual e o cliente final detecta na hora.
**Onde:** `produto.html`, múltiplos locais (linhas aproximadas).
**O que fazer:** substituir conforme tabela. Linhas são referência da leitura inicial — confirmar conteúdo antes de editar.

| Linha aprox. | Onde | Trocar de | Para |
|---|---|---|---|
| 2578 | Hero — descrição | "(Bisglicinato, Dimalato, Treonato, L-Treonato e Citrato)" | "(Bisglicinato, Dimalato, Cloreto, Taurato e Óxido)" |
| 2782 | Fórmula transparente — Possui | "L-Treonato de Magnésio" | "Cloreto de Magnésio" |
| 2788 | Fórmula transparente — Possui | "Citrato de Magnésio" | "Óxido de Magnésio" |
| 2823 | Fórmula transparente — **Não possui** | "Óxido de magnésio" (o produto TEM óxido) | substituir item por **"Soja"** (segura para fórmula) |
| 3180 | FAQ — resposta da pergunta 1 | "Bisglicinato age no sistema nervoso central; Dimalato nos músculos; Treonato no cérebro; Citrato no intestino; Taurato no coração" | "Bisglicinato age no sistema nervoso e sono; Dimalato nos músculos e mitocôndrias; Cloreto no sistema imune e absorção; Taurato no coração; Óxido no trato gastrointestinal." |
| 2930 | Modo de uso — texto | "ganhos cognitivos do Treonato em até 4 semanas" | "ganhos cardiovasculares do Taurato em até 4 semanas" |
| 3250 | Tabela nutricional — descrição da linha do Magnésio | "Bisglicinato · Dimalato · Treonato · Citrato · Taurato" | "Bisglicinato · Dimalato · Cloreto · Taurato · Óxido" |

**Notas do Dev:** Todas as 7 substituições aplicadas conforme a tabela. A descrição do hero (linha 2578) recebeu a copy nova da Tarefa 7 ao mesmo tempo — a frase mencionando "Treonato, L-Treonato e Citrato" foi substituída integralmente pelo novo texto, então as 5 formas erradas saíram naturalmente da copy do hero (não há mais menção de formas no texto do hero — elas aparecem agora nos itens da fórmula transparente e na nova seção da T5).

---

### Tarefa 2 — Corrigir dose na tabela nutricional

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Briefing oficial declara 260 mg / 62% VD. HTML está com 300 mg / 71% VD.
**Onde:** `produto.html` linhas ~3251-3252 (linha do Magnésio na tabela nutricional).
**O que fazer:**
- Trocar `300 mg` por `260 mg`
- Trocar `71%` por `62%`

**Notas do Dev:** Aplicado. Valores conferidos nas células correspondentes da linha "Magnésio (blend de 5 formas)".

---

### Tarefa 3 — Substituir "Aprovado ANVISA" por linguagem correta

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Suplemento alimentar é **notificado** na ANVISA, não "aprovado". "Aprovado pela ANVISA" pode ser questionado em fiscalização e enfraquece a credibilidade técnica da marca.
**Onde:** `produto.html`, 2 locais.
**O que fazer:**
- Linha ~2736 (trust row no hero): "Aprovado ANVISA" → "Notificado ANVISA"
- Linha ~3279 (footer): "...aprovadas pela ANVISA." → "...notificadas na ANVISA."

**Notas do Dev:** Ambos os trechos atualizados. Trust row exibe "Notificado ANVISA"; copy do footer agora termina em "...notificadas na ANVISA."

---

### Tarefa 4 — Reescrever as 4 reviews com erros de português

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** As reviews atuais têm frases quebradas ou que entregam que são fictícias ("Levanto melhor", "Notei o como melhorou", "1h de hora extra"). Em apresentação para cliente, isso queima a credibilidade da seção inteira.
**Onde:** `produto.html` linhas ~3100-3155 (seção Reviews).
**O que fazer:**

| Review | Linha aprox. | Substituir por |
|---|---|---|
| Ana Paula M. — título | 3109 | "Doze semanas e as **câimbras** noturnas passaram" (magnésio = câimbra, não cólica) |
| Ana Paula M. — corpo | 3110 | "Tomo todas as noites antes de dormir. Tirou as câimbras e melhorou muito o sono. 7 dias depois já notei a diferença. Recomendo sem medo." |
| Roberto G. — título | 3123 | "Relaxa rápido. Já notei diferença na primeira semana." |
| Roberto G. — corpo | 3124 | "Pesquisei bastante antes de escolher e o Nano 5Mag entregou tudo o que prometeu. Acordo melhor, durmo bem e até a ansiedade caiu bastante. Vou continuar." |
| Fernanda L. — corpo | 3138 | "Tomava só bisglicinato simples e mudei pro blend. A diferença em energia foi grande. Aquela última hora do dia que travava acabou." |
| Carlos G. — título | 3151 | "Escolhi pelo preço. Fiquei pelo resultado." |
| Carlos G. — corpo | 3152 | "Já tinha tentado magnésio simples e não funcionou. O 5MAG cumpriu o que prometeu. Sigo tomando." |

**Notas do Dev:** 4 reviews atualizadas conforme a tabela (Ana Paula — título+corpo, Roberto G. — título+corpo, Fernanda L. — corpo, Carlos G. — título+corpo).

---

### 🟡 RECOMENDADOS (impacto alto, recomendo aplicar antes da apresentação)

### Tarefa 5 — Adicionar nova seção "5 formas → 5 sistemas" (mapa visual central)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Este é o **argumento competitivo central** do produto segundo o briefing oficial ("o mapa de 5 órgãos é o argumento que mais converte"). Hoje aparece diluído em 3 lugares mas não tem seção dedicada. Sem essa seção, o gancho de venda principal fica enterrado.
**Onde:** `produto.html`, **inserir nova `<section>` entre o fechamento do hero (linha ~2752) e a abertura da seção Fórmula transparente (linha ~2755)**.
**O que fazer:**

Criar seção no padrão `.section.container` já existente, com:

- **Eyebrow:** "Cobertura completa"
- **Título (h2.section__title):** "5 formas. 5 sistemas. <em>1 cápsula.</em>"
- **Subtítulo (section__sub):** "Cada forma de magnésio tem afinidade com um sistema do corpo. Tomar uma forma só cobre uma fração. O 5MAG cobre tudo em uma dose."
- **Grid horizontal de 5 colunas** (em mobile, vira carousel ou empilha 2+2+1):

| Ícone | Forma | Sistema | Frase |
|---|---|---|---|
| 💤 (lua/sono) | **BISGLICINATO** | Sistema Nervoso | "Para o sono e a ansiedade." |
| ⚡ (raio/energia) | **DIMALATO** | Energia Celular | "Para músculos e fadiga." |
| 🛡️ (escudo) | **CLORETO** | Sistema Imune | "Para imunidade e absorção." |
| ❤️ (coração) | **TAURATO** | Coração | "Para coração e pressão." |
| 💚 (folha/digestão) | **ÓXIDO** | Digestão | "Para intestino e base de dose." |

Pode usar SVGs no estilo dos outros benefícios do hero (linhas 2589-2611) em vez de emojis — fica mais consistente com o resto da página. Cada coluna: ícone grande em círculo, nome da forma em uppercase pequeno, nome do sistema em h4, frase em parágrafo pequeno.

Estilo: fundo claro `--color-cream` ou similar, cada coluna com card sutil. Espaçamento generoso entre colunas.

**Notas do Dev:** Seção inserida entre o fechamento do hero e a abertura de Composition. Grid de 5 colunas com cards em `var(--color-cream)`, ícone em círculo branco com cor da marca (`var(--color-brand)`), eyebrow uppercase com o nome da forma, h4 com o nome do sistema, parágrafo curto com a frase. SVGs reaproveitados da página (lua/raio/escudo/coração) + um novo para Óxido/Digestão. Adicionado breakpoint `@media (max-width: 900px)` que vira o grid em 2 colunas com o card do Óxido ocupando a linha inteira (padrão 2+2+1).

---

### Tarefa 6 — Adicionar legenda sob cada vídeo UGC ("Quem usa, indica")

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hoje os 5 vídeos rodam sem nenhum contexto (nome, idade, frase). Em apresentação, parece placeholder. Adicionar legenda preenche o tom narrativo sem mexer no layout.
**Onde:** `produto.html` linhas ~2860-2890 (cada `<article class="indicam-ugc">`).
**O que fazer:**

Dentro de cada `<article class="indicam-ugc">`, adicionar após o `<a>` do vídeo um bloco de legenda:

```html
<div class="indicam-ugc__caption">
  <span class="indicam-ugc__name">Nome · idade</span>
  <p class="indicam-ugc__quote">"Frase curta."</p>
</div>
```

Sugestões de copy para os 5 vídeos (na ordem em que aparecem):

| # | Nome | Frase |
|---|---|---|
| 1 | Aline · 34 anos | "Em 3 semanas dormindo a noite inteira." |
| 2 | Marina · 41 anos | "Acabou a câimbra de madrugada." |
| 3 | Júlia · 38 anos | "Mais energia para acabar o dia." |
| 4 | Camila · 45 anos | "Trocou meu bisglicinato simples." |
| 5 | Sofia · 36 anos | "Ansiedade baixou junto com o sono melhor." |

Estilo: name em font-title weight 600 pequeno, quote em itálico tamanho ~13px. Caption pode ficar dentro do card do vídeo (overlay no canto inferior) ou abaixo do vídeo — escolher o que ficar melhor no grid.

**Notas do Dev:** Caption implementada como overlay no rodapé do vídeo (com gradiente preto na base para legibilidade sobre vídeos claros). Optei pelo overlay porque mantém o aspect ratio 9:14 do card limpo e não estica o grid. Adicionadas 3 classes novas no CSS: `.indicam-ugc__caption`, `.indicam-ugc__caption-name`, `.indicam-ugc__caption-quote` (evitei reaproveitar `.indicam-ugc__name` para não colidir com a definição existente). Aplicado nas 5 articles.

---

### 🟢 AJUSTES DE COPY (pequenos, recomendados)

### Tarefa 7 — Ajustes pontuais de copy

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Pequenas melhorias de tom (alinhamento com voz da marca: especificidade, frase curta, sem "acolher" tipo poético) que somam na percepção de cuidado editorial.
**Onde:** `produto.html`, locais abaixo.
**O que fazer:**

| Linha aprox. | Está | Trocar para |
|---|---|---|
| 2578 (hero desc) | "Combinação inteligente de cinco quelatos de magnésio (...) para **acolher** sono, energia, intestino e clareza mental em uma única cápsula de alta absorção." | "5 formas de magnésio numa cápsula só. Cada uma atua num sistema diferente: sono, energia, coração, imunidade e digestão. A cobertura completa que magnésio simples não dá." |
| 2598 (benefit 2) | "Mais energia & disposição" | "Mais energia para o dia" |
| 2610 (benefit 4) | "Coração & saúde cerebral" | "Coração equilibrado" |
| 2719 (CTA secundário) | "Falar com especialista" | "Tirar dúvida com nutricionista" |
| 3049 (causa 2 — título) | "Não absorvíveis" | "Pouco absorvíveis" |
| 3050 (causa 2 — texto) | "Óxido de magnésio tem apenas 4% de biodisponibilidade. Você toma, mas o corpo não usa." | "Magnésio comum, em forma isolada, tem absorção limitada. O corpo aproveita só uma parte. No 5MAG, cada forma entra junto e é absorvida onde precisa." |
| 3054 (causa 3 — título) | "5 sistemas afetados" | "5 sistemas, 5 formas" |
| 3055 (causa 3 — texto) | "Cada forma atua em um sistema: sono, músculo, intestino, coração e cérebro." | "Cada forma atua num sistema: sono, energia, imunidade, coração e digestão." |

**Notas do Dev:** Os 8 ajustes aplicados conforme tabela. O ajuste da linha 2578 (hero desc) foi feito junto com a Tarefa 1, já que substituía a frase inteira (que continha as formas erradas). Os outros 7 trocados linha a linha.

---

### Tarefa 8 — Hierarquia visual das duas CTAs em "Entenda o magnésio"

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Hoje há dois botões com peso visual quase igual ("Ler o artigo completo" vermelho + "Baixar guia em PDF" outlined). Cria competição de atenção. CTA primário deve ganhar.
**Onde:** `produto.html` linhas ~2900-2909.
**O que fazer:** manter "Ler o artigo completo" como botão sólido vermelho. Transformar "Baixar guia em PDF" em **link inline** discreto, sem caixa de botão — algo como `<a>` simples cinza com seta de download e underline-on-hover, alinhado horizontalmente ao botão principal.

**Notas do Dev:** Botão "Baixar guia em PDF" virou link inline com classe `.pdf-link`: cor `var(--color-mute)`, sem caixa/borda, ícone menor (13px), peso 500. No hover, ganha `text-decoration: underline` com offset de 3px e escurece para `var(--color-text)`. Gap do container aumentado de 10px para 20px para dar respiro entre o botão e o link. Hierarquia visual: CTA primário ganha claramente.

---

## 🚧 Pendências bloqueantes
- Nenhuma. Todas as decisões desta rodada já estão tomadas. Briefing oficial do 5MAG (`5MAG_Briefing.docx.md`) cobre as 5 formas, doses e claims permitidos.

## ❌ Não fazer
- **Não mexer no `index.html`** — já foi aprovado pelo cliente.
- **Não trocar a paleta, tipografia ou variáveis CSS** — layout aprovado pela home; PDP herda.
- **Não criar lógica nova de integração** (carrinho real, checkout, Shopify) — apresentação usa dados estáticos.
- **Não mexer no header, marquee, footer estrutural** — só copy textual conforme indicado.
- **Não tocar nos vídeos UGC originais** (URLs do Shopify CDN) — só adicionar legendas em cima.
- **Não inventar claims novos** (ex: "trata cãibra", "cura insônia", "reduz pressão") — manter linguagem ANVISA-compliant que já existe na FAQ.
- **Não mover a ordem das seções** — está aprovada estruturalmente, só inserir a nova Tarefa 5 no ponto indicado.

## 📝 Log do Dev (preencher ao final)
- **Resumo:** Todas as 8 tarefas executadas em `produto.html`. Bloqueadores (5 formas, dose, ANVISA, reviews) resolvidos. Recomendados (nova seção 5×5 e legendas UGC) implementados. Os 8 ajustes de copy + hierarquia das CTAs aplicados. `index.html`, header/marquee/footer e variáveis CSS não foram tocados.
- **Decisões fora do brief:**
  - Na Tarefa 1, a substituição da hero desc (linha 2578) foi feita de uma vez com a nova copy da Tarefa 7, porque a frase inteira saiu — não ficou uma versão intermediária só com as formas trocadas. Resultado prático é o mesmo, mas vale registrar que essas duas tarefas se fundiram nessa linha.
  - Na Tarefa 6, optei por colocar a legenda como overlay no rodapé do vídeo (com gradiente preto sutil) em vez de bloco abaixo. Razão: aspect-ratio 9:14 dos cards é o que dá ritmo à fileira; adicionar bloco abaixo esticaria os cards e quebraria o equilíbrio visual com os outros 4 vídeos.
  - Na Tarefa 5, o SVG do Óxido (Digestão) não tinha equivalente pronto na página, então criei um ícone simples de leaf/curva — tom consistente com os outros 4 SVGs reusados (lua, raio, escudo, coração).
  - Adicionei um breakpoint `@media (max-width: 900px)` novo (só para `.forms-map`) para garantir o 2+2+1 em mobile, conforme orientação do brief. Não toquei nas media queries existentes.
- **Dúvidas pro Diretor:** Nenhuma. Todas as substituições estavam claras no brief.
- **Tempo aproximado:** ~25 minutos de execução pura (sem contar leituras de contexto inicial).
