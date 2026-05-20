# Consulta — Estratégia do header (CSS override vs. section custom)

> Arquivo criado pelo Dev pra alinhar uma decisão de approach antes/durante a execução.

**Contexto:** A T1 do `qa-fase-2-achados-e-ajustes.md` pedia criar `sections/nanovit-header.liquid` (replicar a estrutura do `sections/header.liquid` do Horizon e aplicar o glassmorphism). Eu mudei o approach pra **CSS override**.

**Por que mudei:**
- O `sections/header.liquid` do Horizon tem ~700 linhas e é o "cérebro" do tema: orquestra cart drawer, predictive search modal, mega-menu, mobile menu, sticky scroll behavior, color scheme transparente, lógica de logo inverso, layout responsivo. Recriar do zero significa reimplementar tudo isso ou aceitar regressão.
- O efeito visual da marca (shell branco translúcido, border-radius 32px, sombra, cart redondo Nano Red) **é puramente CSS**. Pinta o shell sem mexer no comportamento.

**O que apliquei:**
- Adicionei bloco "Header Nanovit · Shell glassmorphism sobre Horizon" em `assets/nanovit-tokens.css`.
- Selector `#header-component` ganhou background rgba + backdrop-filter + border + border-radius 32px + box-shadow.
- `#header-group` ganhou padding top 16px pra ficar "flutuando" no scroll.
- Cart button (que vier de `.header-actions__cart-button` ou com SVG de carrinho) recebe `background: var(--color-primary)` + redondo 42px.

**Tradeoff honesto:**
- ✅ Mantém TODOS os comportamentos do Horizon (cart drawer abre, search funciona, sticky funciona, mobile menu funciona, traduções funcionam).
- ✅ Migração futura do Horizon (`git fetch upstream && git merge upstream/main`) não dá conflito no nosso arquivo de override (Horizon nunca toca em `nanovit-*.css`).
- ⚠️ Não consigo controlar 100% da hierarquia interna do header via CSS (ex: se quiser nav-strip de pills nanovit-style abaixo do shell, precisaria do Liquid). Pra essa rodada, os links/menu do Horizon já bastam pra mostrar como shell.
- ⚠️ Selector do cart pill pode quebrar se o Horizon mudar a estrutura interna do botão. Fallback: o redondo brand fica mesmo sem o filtro funcionar 100%.

**Decisão que peço:**
1. **OK** com o CSS override (continuamos assim e fechamos T1)?
2. Ou prefere mesmo a section custom completa (~500 linhas, perda de features)? Aí abro brief de migração.

Se o **1**, posso fechar a T1 como Concluído e seguir pras outras 4 tarefas. Se o **2**, marco T1 como Bloqueado e espero o brief de migração.

—Dev
