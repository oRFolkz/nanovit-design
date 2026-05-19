# Brief — Unificar valor de frete grátis em R$ 199,90

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** `produto.html` (apenas — 3 strings curtas)
**Contexto estratégico:**
A rodada anterior (calculadora de frete) revelou que o `produto.html` tem **dois valores diferentes** de frete grátis na mesma página:
- Marquee: R$ 147,00
- Perks (buy-box): R$ 199,90
- Calculadora (nova): R$ 147

Calculadora e perks ficam coladas visualmente. Usuário lê os dois números, percebe a inconsistência, perde confiança. **Decisão minha:** unificar tudo em **R$ 199,90** (valor que está na perks, mais próximo do CTA, mais coerente com mass-market BR, melhor pro upsell de 3 unidades).

Decisão pendente de validação do cliente. Se ele responder "não, é R$ 147", invertemos — mas o problema da divergência precisa ser resolvido agora de qualquer jeito.

---

## ✅ Checklist de execução

### Tarefa 1 — Marquee (2 ocorrências)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Onde:** `produto.html` linhas ~2506 e ~2514 (ambas `marquee__item`).
**O que fazer:** Trocar `Frete grátis acima de R$ 147,00 para todo o Brasil` por `Frete grátis acima de R$ 199,90 para todo o Brasil` nas DUAS ocorrências (o marquee duplica o conteúdo pra rolagem infinita).
**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Calculadora (frase de rodapé do resultado)

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Onde:** `produto.html` linha ~3883, dentro de `renderResults()` no JS da calculadora.
**O que fazer:** Trocar `Frete grátis no PAC a partir de R$ 147 em produtos.` por `Frete grátis no PAC a partir de R$ 199,90 em produtos.`
**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Conferência final

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**O que fazer:** Buscar `147` em `produto.html` inteiro. Não deve sobrar nenhuma menção a frete grátis em R$ 147. (Outros usos de "147" sem relação com frete podem permanecer.)
**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- **Cliente confirmar:** R$ 199,90 é o valor correto da política de frete grátis? Se for R$ 147, reverter as 3 strings acima.

## ❌ Não fazer
- Não alterar o bloco `.pdp__perks` (já está em R$ 199,90, está certo).
- Não mexer na simulação por região da calculadora (valores e prazos por CEP).
- Não adicionar nada novo. Escopo é puramente correção de string.

## 📝 Log do Dev (preencher ao final)
- Resumo: 3 strings trocadas em `produto.html`. Marquee linhas 2506 e 2514 (R$ 147,00 → R$ 199,90); rodapé da calculadora linha 3883 (R$ 147 → R$ 199,90). Grep final por `147` no arquivo retorna zero ocorrências.
- Decisões fora do brief: Nenhuma.
- Dúvidas pro Diretor: Nenhuma. Pendência sobre valor final continua aberta com o cliente conforme brief.
- Tempo aproximado: ~2 min.
