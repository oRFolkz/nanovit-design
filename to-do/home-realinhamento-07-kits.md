# Brief 07 — Kits (reuso da section `nanovit-home-produtos`)

> Pré-req: brief 05 (`home-realinhamento-05-bestsellers.md`) precisa estar concluído.
**Sem section nova.** Apenas configuração do template.

**O que é:** A mesma `nanovit-home-produtos.liquid` do brief 05 é instanciada uma segunda vez no `templates/index.json`, agora com `collection` apontando pra "kits" e título "Kits Nanovit". Zero código novo.

---

## ✅ Tarefa única

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Quando fazer:** dentro do brief 13 (orquestração do `templates/index.json`), instanciar a section `nanovit-home-produtos` duas vezes:

```json
{
  "produtos_bestsellers": {
    "type": "nanovit-home-produtos",
    "settings": {
      "title": "Mais Vendidos",
      "subtitle": "Os produtos mais amados pela nossa comunidade",
      "collection": "bestsellers",
      "limit": 8,
      "see_all_label": "Ver todos os produtos",
      "see_all_url": "/collections/all"
    }
  },
  "produtos_kits": {
    "type": "nanovit-home-produtos",
    "settings": {
      "title": "Kits Nanovit",
      "subtitle": "Combinações pensadas para resultados consistentes.",
      "collection": "kits",
      "limit": 6,
      "see_all_label": "Ver todos os kits",
      "see_all_url": "/collections/kits"
    }
  }
}
```

**Notas do Dev:** > _(este brief pode ser fechado como Concluído imediatamente se o brief 05 estiver pronto e o brief 13 instanciar 2x conforme acima — é só lembrete administrativo.)_

---

## 🚧 Pendência cliente
- Criar coleção `kits` no admin Shopify.
- Idealmente cada "kit" é um produto Shopify (bundle) com sua própria página de produto e variantes (quantidades, sabores).

## ❌ Não fazer
- Não criar `nanovit-home-kits.liquid` separada — duplica código.

## 📝 Log do Dev
- Resumo:
- Decisões:
- Dúvidas:
- Tempo:
