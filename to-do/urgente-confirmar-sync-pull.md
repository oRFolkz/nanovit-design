# Urgente — Confirmar status do sync-pull

> Dev fechou `seo-metatags-fallbacks.md` ANTES de fechar `sync-pull-mudancas-admin.md`. Preciso saber se houve sobrescrita.

## Pergunta direta

Você rodou `shopify theme pull --theme=152714772659 --nodelete` ANTES de fazer push do seo-metatags?

### Caso A — Sim, rodei o pull antes do push

Perfeito. Renomeia `sync-pull-mudancas-admin.md` → `[CONCLUIDO] sync-pull-mudancas-admin.md` e me avisa via Notas do Dev se houve algum conflito notável (settings_data, templates) e o que foi commitado. Sem ação adicional.

### Caso B — Não, esqueci do pull e fui direto pro push

Risco confirmado: o push do seo-metatags pode ter sobrescrito alterações que o cliente fez no admin (theme editor, settings, ou uploads).

**O que fazer agora:**

1. Rodar o pull AGORA mesmo com `--nodelete`:
   ```bash
   cd /e/Shopify/nanovit-theme
   shopify theme pull --theme=152714772659 --store=nexgeniusstore.myshopify.com --nodelete
   ```
2. Conferir o git diff:
   ```bash
   git diff
   ```
3. Reportar via `[REVISÃO] urgente-confirmar-sync-pull.md`:
   - Se a diff trouxe mudanças "perdidas" do admin → confirmar/commitar.
   - Se não trouxe nada → o admin pode não ter chegado a salvar nada, ou o push sobrescreveu mesmo.

**Em qualquer cenário**, listar nos Notas exatamente o que o pull trouxe.

## ❌ Não faça
- Não rodar `push` de novo antes de confirmar este pull.

---

**Status:** [ ] Pendente · [ ] Concluído · [ ] Revisão

**Notas do Dev:** > _(preencher: A ou B + detalhes)_
