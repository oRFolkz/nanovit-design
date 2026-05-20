# Brief 01 — Cleanup das sections erradas da home

> Documento de trabalho. Pré-requisito: ter lido `home-realinhamento-00-plano.md`.

**Por quê:** A Fase 3 anterior criou 4 sections que não correspondem ao `index.html` e estão sendo substituídas. Manter o cleanup separado evita arquivos órfãos no tema.

**Pode ser feito em paralelo:** sim, com 02-12.

---

## ✅ Checklist

### Tarefa 1 — Remover sections obsoletas

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado

Arquivos a deletar em `e:/Shopify/nanovit-theme/sections/`:
- `nanovit-bestsellers.liquid` (será reescrito pelo brief 05 com nome novo `nanovit-home-produtos.liquid`)
- `nanovit-objetivo.liquid` (não existe no mock)
- `nanovit-manifesto.liquid` (substituído pelo CTA institucional do brief 06)
- `nanovit-linhas.liquid` (cardzinho errado; substituído pelo carrossel do brief 04)
- `nanovit-depoimentos.liquid` (não existe no mock; UGC do brief 10 cobre)

Manter intocados:
- `nanovit-hero.liquid` (vai ser substituído pelo brief 02 — pode deixar até lá pra não quebrar a home no meio do caminho. Brief 02 trata da substituição.)
- `nanovit-newsletter.liquid` (correto)
- `nanovit-footer.liquid` (correto)
- `nanovit-404.liquid` (correto)
- Tudo da PDP (`nanovit-pdp-*.liquid`)

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Limpar referências de blocks órfãos

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado

Conferir se algum arquivo em `blocks/_*.liquid` foi criado especificamente pras sections deletadas (ex: `_objetivo-card.liquid`). Se sim, deletar também.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Avisar via campo Notas

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado

Após delete, o `templates/index.json` vai referenciar sections inexistentes. Tudo bem por ora — brief 13 reorganiza o template inteiro. Por enquanto a home ficaria quebrada se publicada, mas isso é esperado durante a transição.

**Notas do Dev:** > _(qualquer arquivo extra encontrado, listar aqui)_

---

## ❌ Não fazer

- Não tocar em `templates/index.json` ainda — é o brief 13.
- Não tocar em sections da PDP, 404, footer, header, marquee.

## 📝 Log do Dev

- **Resumo:** 5 sections deletadas de `sections/`: `nanovit-bestsellers.liquid`, `nanovit-objetivo.liquid`, `nanovit-manifesto.liquid`, `nanovit-linhas.liquid`, `nanovit-depoimentos.liquid`. Verifiquei `blocks/` por blocks órfãos com prefixo nanovit-/objetivo-/etc — nenhum encontrado. Index.json continua referenciando algumas dessas types e vai dar warning até o brief 13 reorganizar (esperado).
- **Decisões fora do brief:** Nenhuma.
- **Dúvidas:** Nenhuma.
- **Tempo:** ~2 min.
