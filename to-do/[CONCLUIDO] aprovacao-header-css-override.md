# Aprovação — Header CSS override (resposta à consulta)

> Resposta do Diretor à `consulta-header-strategy.md`.

## Decisão

**Aprovo a Opção 1: CSS override em `assets/nanovit-tokens.css`.** Pode fechar a T1 do `qa-fase-2-achados-e-ajustes.md` como Concluído.

## Por quê

- Refazer ~700 linhas do `sections/header.liquid` do Horizon pra ganhar efeito que é puramente CSS é mau trade-off. Cart drawer, predictive search, mega-menu, mobile menu, sticky behavior, color schemes — tudo isso o Horizon já resolveu bem. Não vamos reinventar.
- Atualizações futuras do Horizon (`git pull upstream`) continuam triviais. Section custom faria todo upgrade virar pesadelo de merge.
- A "limitação" de não controlar nav-strip de pills abaixo do shell é cosmética, não funcional. Se sentirmos falta, abro brief próprio depois.

## Aceito também os trade-offs declarados

- ⚠️ Selector do cart pode quebrar se Horizon mudar estrutura interna. **OK** — quando acontecer, ajusta o selector. Risco gerenciável.
- ⚠️ Sem nav-strip de pills nanovit-style. **OK** — links do Horizon bastam por agora.

## Próximo passo

Vou abrir Chrome DevTools e validar visualmente o resultado do header + as outras 3 tarefas que já foram pra `[CONCLUIDO]` (`hero da home`, `setas do marquee`, `recommendations vazias`) + o `fix-template-blocks-pdp.md`. Se algo ficar fora do padrão, abro brief novo. Se tudo passar, sigo pras 5 stubs restantes (reviews, nutrition, indicam, modo-uso, cta-edu) — também em brief novo.

—Diretor
