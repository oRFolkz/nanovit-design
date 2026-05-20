# Brief — Fix: blocks default no template `product.nano-5mag.json`

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** SÓ `templates/product.nano-5mag.json`. Nada de Liquid, nada de schema novo.
**Prioridade:** ALTA. Antes da rodada de stubs restantes (reviews, nutrition, indicam, modo-uso).

**Contexto estratégico:**
QA visual da PDP mostrou problema sistêmico: as sections `composition`, `bundle`, `symptoms`, `faq` e `forms` aparecem vazias na loja, mesmo o dev tendo declarado presets com defaults preenchidos no schema de cada section.

**Causa raiz:** o **preset** do schema só dita o que aparece quando o admin adiciona a section manualmente pelo theme editor ("Adicionar section"). Como nessas sections o conteúdo vem de blocks (não de settings da section), o template JSON precisa instanciar esses blocks explicitamente. Hoje o `templates/product.nano-5mag.json` lista as sections mas não preenche os blocks delas.

**O que NÃO é o problema:** as Liquid sections estão certas. Hero + breadcrumb + cta-edu funcionam porque não dependem de blocks (settings inline da section bastam).

---

## ✅ Checklist de execução

### Tarefa 1 — Preencher blocks em `composition`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Colunas "POSSUI" e "NÃO POSSUI" aparecem vazias.
**Onde:** `templates/product.nano-5mag.json`, dentro da section `composition`.
**O que fazer:** Adicionar 5 blocks `has` + 5 blocks `missing` (mesmo conteúdo do preset). Conteúdo sugerido:

**Possui (5 itens):**
1. 5 formas de magnésio biodisponíveis
2. Cápsula vegetal (vegana)
3. Sem aditivos artificiais
4. Notificado ANVISA
5. Fórmula testada em laboratório

**Não possui (5 itens):**
1. Açúcar ou adoçante
2. Corante artificial
3. Glúten ou lactose
4. Estearato de magnésio
5. Magnésio óxido (baixa absorção)

Cada block segue o schema definido em `sections/nanovit-pdp-composition.liquid`. Se o schema do block tiver settings adicionais (ícone, cor), usar defaults razoáveis.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Preencher blocks em `bundle`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Bundle mostra "Total R$ 0,00" e "Adicionar combo de 0 itens".
**Onde:** `templates/product.nano-5mag.json`, section `bundle`.
**O que fazer:** Adicionar 3 blocks de item do bundle:

1. **Nano 5Mag** (este produto, sempre marcado e não-desmarcável)
   - Imagem: `nanovit-5mag-01.webp` (asset)
   - Preço: R$ 89,90
   - URL: `/products/nano-5mag-dimalato-bisglicinato-cloreto-taurato-e-oxido-de-magnesio-60-caps-nanovit`

2. **Nano Calm — Melatonina**
   - Imagem: usar asset existente ou placeholder
   - Preço: R$ 69,90 (preço atual no admin)
   - Preço antes: R$ 89,90
   - URL: `/products/nano-calm-melatonina`

3. **Vitamina D3 2000**
   - Imagem: placeholder
   - Preço: R$ 59,90
   - URL: `/products/vitamina-d3-2000`

Adaptar nomes de field ao schema real do block (provavelmente `title`, `image`, `price`, `price_was`, `url`, `checked`).

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Preencher blocks em `symptoms`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Lista numerada de sintomas não aparece (só a foto à esquerda).
**Onde:** `templates/product.nano-5mag.json`, section `symptoms`.
**O que fazer:** Adicionar 5 blocks `symptom` + 3 blocks `cause`:

**Sintomas (5):**
1. Acorda cansado mesmo dormindo 8 horas
2. Câimbras frequentes nas pernas
3. Ansiedade e tensão sem razão clara
4. Dor de cabeça recorrente
5. Constipação intestinal

**Causas (3):**
1. Dieta moderna pobre em magnésio
2. Estresse crônico que esgota o mineral
3. Suplementos com formas mal absorvidas

(adaptar pros nomes de field reais do schema)

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — Preencher blocks em `faq`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** "Perguntas frequentes" renderiza só o título; nenhum `<details>`/accordion presente. JS de QA confirmou: 0 details na section.
**Onde:** `templates/product.nano-5mag.json`, section `faq`.
**O que fazer:** Adicionar 5 blocks `qa`. Conteúdo:

1. **Quanto tempo para sentir efeito?**
   Em geral, entre 15 e 30 dias de uso contínuo. Magnésio age na regulação celular: o corpo precisa repor estoques antes de mostrar diferença em sono, energia e câimbra.

2. **Posso tomar todos os dias?**
   Sim. A dose diária recomendada (2 cápsulas) está dentro do limite seguro estabelecido pela ANVISA. Uso contínuo é o que entrega o resultado.

3. **Tem contraindicação?**
   Gestantes, lactantes e pessoas com doença renal devem consultar médico antes do uso. Para o resto da população adulta saudável, o produto é seguro.

4. **Por que 5 formas de magnésio numa cápsula só?**
   Cada forma é melhor absorvida em um sistema do corpo. Bisglicinato para sono, dimalato para energia muscular, cloreto para coração, taurato para sistema nervoso, óxido para intestino. Uma cápsula cobre os cinco.

5. **Posso tomar junto com outros suplementos?**
   Sim. Magnésio combina bem com vitamina D3, K2, B6 e ômega-3. Evite tomar no mesmo horário de cálcio ou ferro, que competem pela absorção.

Primeira aberta por default, demais fechadas.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — Preencher blocks em `forms`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Section "5 formas. 5 sistemas. 1 cápsula." mostra título + subtítulo mas não os 5 cards.
**Onde:** `templates/product.nano-5mag.json`, section `forms`.
**O que fazer:** Adicionar 5 blocks `form` (ou nome equivalente no schema):

1. **Bisglicinato** — Sistema nervoso · Sono profundo e relaxamento. Ícone: lua.
2. **Dimalato** — Sistema muscular · Energia para o dia, sem cafeína. Ícone: raio.
3. **Cloreto** — Sistema cardiovascular · Coração em equilíbrio. Ícone: coração.
4. **Taurato** — Sistema imunológico · Imunidade reforçada. Ícone: escudo.
5. **Óxido** — Sistema digestivo · Intestino regulado. Ícone: folha.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 6 — Verificação

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**O que fazer:** após `shopify theme push` (ou auto-deploy via GitHub-Shopify), me avisar. Eu abro a PDP via Chrome DevTools e confirmo que:
- Composition mostra 5 + 5 itens
- Bundle mostra 3 produtos com checkbox e total > R$ 0
- Symptoms mostra 5 sintomas numerados + 3 causas
- FAQ mostra 5 accordions (primeiro aberto)
- Forms mostra 5 cards lado a lado

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- Nenhuma. Tarefa puramente de configuração do JSON do template.

## ❌ Não fazer
- Não mexer em nenhum Liquid de section (`nanovit-pdp-*.liquid`).
- Não mexer no schema dos blocks.
- Não implementar as 5 stubs restantes ainda (reviews, nutrition, indicam, modo-uso). Próxima rodada.
- Não esperar produto Nano Calm ou Vitamina D3 do bundle existirem com handles exatos — usar handle real do admin via inspeção visual, ou marcar como pendência se não bater.
- Não usar travessão (—) em copy renderizada. Onde fizer falta, usar ponto, vírgula, dois pontos ou parêntese.

## 📝 Log do Dev (preencher ao final)

- **Resumo:** `templates/product.nano-5mag.json` reescrito do zero. Cada section que tem blocks ganhou seção `blocks` + `block_order`:
  - **composition** (T1): 5 blocks `has` + 5 blocks `missing` com a copy exata do brief.
  - **forms** (T5): 5 blocks `form` (Bisglicinato/Dimalato/Cloreto/Taurato/Óxido) com sistema+descrição+ícone — adaptei ao schema real (form_name/system/description/icon), brief mencionava "Sistema cardiovascular/imunológico/digestivo" que casa com o select de ícones (heart/shield/leaf).
  - **bundle** (T2): 3 blocks `item` (Nano 5Mag, Nano Calm Melatonina R$ 69,90 com R$ 89,90 riscado, Vitamina D3 2000 R$ 59,90). Schema do bloco não tem `image` nem `url` per item (só `name/subtitle/price/price_was/is_current/checked_default`) — flagged abaixo.
  - **symptoms** (T3): 5 blocks `symptom` (richtext com `<strong>` na aspas) + 3 blocks `cause` (title+copy).
  - **faq** (T4): 5 blocks `qa` com a copy completa do brief. Primeira (`qa_1`) marcada `open_by_default: true`.

- **Decisões fora do brief:**
  - **Bundle items sem URL nem imagem por bloco**. O schema atual de `nanovit-pdp-bundle.liquid` block `item` só expõe nome/subtítulo/preço/preço-de/é-este-produto/checked-default. URLs e imagens dos itens não-current não estão no schema porque na rodada de design original o foco era o cálculo de total/savings, não navegação. Se quiser, posso abrir refactor da section pra adicionar `url` e `image` no item — peça brief próprio. Por ora, o admin não consegue trocar a URL de "Nano Calm" e "Vitamina D3" sem mexer no Liquid.
  - **IDs dos blocks usam prefixo curto** (`has_1`, `form_bisglicinato`, `qa_3`, etc) pra deixar o `templates/product.nano-5mag.json` legível. Shopify aceita qualquer string única.
  - **Symptoms causes adaptadas:** brief deu 3 causas mas o schema da section já tinha 3 cause blocks no preset com mensagens diferentes. Usei as causas do brief (dieta moderna / estresse crônico / suplementos mal absorvidos) substituindo as do preset (uma forma só / pouco absorvíveis / 5 sistemas 5 formas).
  - **Forms · "Imunológico" não tem ícone próprio** — o select expõe lua/raio/escudo/coração/folha. Taurato/Imunológico ganhou `shield`, que era o mais próximo.

- **Dúvidas pro Diretor:**
  1. Bundle precisa de URL + imagem por bloco? Se sim, abro refactor da section.
  2. Tem produto "Nano Calm" e "Vitamina D3 2000" reais no admin com esses handles? Se houver, o bundle pode evoluir pra usar product_picker em vez de campos manuais de texto — mais admin-friendly, mas exige mudar a section.

- **Tempo aproximado:** ~25 min.
