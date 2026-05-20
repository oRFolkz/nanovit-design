# Dev → Diretor · Resposta sobre briefs 01 e 02

> Arquivo de sinalização proativa. Briefs 01 (cleanup) e 02 (hero slideshow) foram executados e marcados [CONCLUIDO]. Tema atualizado em staging.

## Estado atual

- **`[CONCLUIDO] home-realinhamento-01-cleanup.md`** — 5 sections deletadas (`bestsellers`, `objetivo`, `manifesto`, `linhas`, `depoimentos`). Nenhum block órfão encontrado.
- **`[CONCLUIDO] home-realinhamento-02-hero-slideshow.md`** — `sections/nanovit-home-hero.liquid` reescrita do zero como slideshow custom element `<nanovit-hero>`. Specs do schema fielmente seguidas (max_blocks 5, settings autoplay/speed/arrows/dots, picture com source mobile + srcset desktop, navegação por teclado, autoplay com pausa em hover/focus).

## Tema em staging

Push limpo, sem erros de schema. Theme ID **`152714772659`** (Nanovit Dev) atualizado.

- **Preview:** https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659
- **Editor:** https://nexgeniusstore.myshopify.com/admin/themes/152714772659/editor

## ⚠️ Quebra esperada durante a transição

`templates/index.json` ainda referencia sections deletadas (`nanovit-bestsellers`, `nanovit-objetivo`, `nanovit-manifesto`, `nanovit-linhas`, `nanovit-depoimentos`). A home vai dar warning de "section type não existe" no preview até o **brief 13** reorganizar o template. Conforme combinado no brief 01.

## Decisões fora do brief (resumo)

- **Custom element `<nanovit-hero>`** em vez de `<div>` + JS (brief deixou a escolha pro dev).
- **Chevron SVG inline** em vez de `{% render 'icon' %}` (não dependo de snippet do Horizon).
- **Guard contra HMR** (`if (!customElements.get(...))`) pra theme editor não estourar ao re-renderizar.

## Aguardando

Polling ativo. Quando você criar os briefs 03-13 (ou qualquer outro), o dev pega e executa em sequência. Sem dúvidas em aberto desta rodada.

—Dev
