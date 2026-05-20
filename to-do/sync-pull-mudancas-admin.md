# Brief — Sync: puxar alterações do admin pra o repo local

> Pré-req: nenhum. Prioridade ALTA — antes de qualquer outro brief.

**Por quê:** O cliente acabou de alterar configurações da loja diretamente no admin Shopify (theme editor, settings.json, ou edição de seções). Sem `shopify theme pull`, qualquer push subsequente do dev vai **sobrescrever** essas mudanças.

---

## ✅ Checklist

### Tarefa 1 — `shopify theme pull` do Nanovit Dev

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** raiz do tema, `e:/Shopify/nanovit-theme/`.

**Comando:**
```bash
cd /e/Shopify/nanovit-theme
shopify theme pull --theme=152714772659 --store=nexgeniusstore.myshopify.com --nodelete
```

**Flags:**
- `--theme=152714772659`: ID do Nanovit Dev (não puxar do main publicado).
- `--store`: nome da loja (evita prompt).
- `--nodelete`: NÃO apaga arquivos locais que não estão no remote (proteção contra perder código novo que ainda não subiu).

### Tarefa 2 — Conferir o diff e commitar

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

```bash
git status
git diff
```

**O que esperar:**
- Mudanças prováveis em `config/settings_data.json` (admin mexeu em theme editor → settings).
- Possíveis mudanças em `templates/*.json` (admin reorganizou sections via theme editor).
- Possíveis novos `assets/*` (admin fez upload via Files).

Se a diff fizer sentido (não apagou nada importante), commitar:
```bash
git add -A
git commit -m "sync: puxar alterações do admin Shopify (theme 152714772659)"
```

Se houver **conflito conceitual** (admin desfez algo que o dev acabou de fazer), abrir arquivo `[REVISÃO] sync-pull-conflito.md` listando o que conflitou e pedindo decisão ao Diretor.

### Tarefa 3 — Continuar de onde parou

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

Após o pull, pode retomar `seo-metatags-fallbacks.md` (o brief que estava em execução).

**Notas do Dev:** > _(listar o que veio no pull e se há conflito)_

---

## ❌ Não fazer
- **Não rodar `shopify theme push`** antes do pull. Vai sobrescrever as mudanças do admin.
- Não puxar do tema `main` (152710774963) — esse é o publicado, não tem nosso trabalho atual em draft. Use SEMPRE o Nanovit Dev (152714772659).
- Não usar `--allow-live` (só serve pro tema publicado).
