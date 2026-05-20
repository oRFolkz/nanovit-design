# Dev → Diretor · Fix de regressão no header (resolvido)

> Site reportado como "todo quebrado". Investiguei via Chrome DevTools MCP. Bug isolado em uma linha de CSS faltando.

## Diagnóstico

Inspeção via JS:
- Section `nanovit_header` estava ocupando **12306px** de altura (deveria ser ~150px).
- Empurrava todas as sections da home (hero, benefícios, bestsellers, etc.) pra fora da viewport visível.
- Body total: 17685px (deveria ~5500px).

Causa raiz: o snippet `nanovit-mobile-menu.liquid` (drawer offcanvas) não tinha CSS de `visibility: hidden` por default. Em desktop o drawer renderizava aberto + os SVGs de ícone, sem width/height explícitos, expandiam pra preencher o container pai. Resultado: cada `<svg>` tomava ~1400px de altura, multiplicado por todos os ícones dos pills + logo + close + search → 12000px.

## Fix

`sections/nanovit-header.liquid`:
- **Stylesheet:** adicionado bloco completo `.nanovit-mobile-menu*` com `position: fixed`, `visibility: hidden`, `opacity: 0`, `transform: translateX(-100%)` por default; `.is-open` ativa visibility/opacity/transform.
- **JS:** o toggle agora alterna `.is-open` na div do drawer, além de `aria-hidden` e scroll-lock.

## Resultado pós-fix

| Métrica | Antes | Depois |
|---|---|---|
| Section header | 12306px | 163px |
| Body total | 17685px | 5542px |
| Mobile menu visibility | visible (bug) | hidden (correto) |

Home agora renderiza todas as 14 sections normalmente. Hero mostra placeholder cream "Suba a imagem deste slide pelo theme editor" porque o admin atual tem 3 slides vazios (cliente ainda não subiu as imagens).

## Tema em staging

https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659

Push limpo. Commit do fix: `b9981ae`. GitHub sincronizado.

## Lição

Snippet de drawer offcanvas sempre precisa de CSS de "fechado por default" inline ou no stylesheet da section pai. Confiar só em `aria-hidden=true` não esconde visualmente — é apenas semântica de acessibilidade. Atualizado memory com a regra.

—Dev
