# Brief — Demo isolada: Tecnologia Nanocomprimido

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** criar arquivo **novo** `demo-tecnologia-nano.html` no root do projeto.
**NÃO tocar em `index.html` (home aprovada) nem em `produto.html`.**
**Contexto estratégico:** o nanocomprimido mastigável é diferencial visual real da Nanovit (hoje aplicado na Melatonina 210mcg e D3+K2). O cliente quer ver uma proposta visual desse conceito **isoladamente**, apenas para apresentação — não para integrar à home (que já foi aprovada). A peça é uma demo standalone que será mostrada lado a lado com o site aprovado.
**Escopo é apresentação:** fotos podem ser placeholder; slider pode ser mock estático.

---

## ✅ Checklist de execução

### Tarefa 1 — Criar arquivo `demo-tecnologia-nano.html`

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Apresentar o conceito da seção "Tecnologia Nanocomprimido" ao cliente em peça isolada — slider antes/depois mostrando comprimido tradicional vs nanocomprimido Nanovit, com texto de apoio.
**Onde:** raiz do projeto (`c:\Users\conta\Downloads\TEMP- NANOVIT\demo-tecnologia-nano.html`).

**O que fazer:**

Criar HTML standalone reusando paleta, fontes e variáveis CSS da home (`index.html`) para manter consistência visual quando o cliente comparar lado a lado.

#### Estrutura geral

```
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <!-- mesma <meta>, <link> de fonts e <style> com :root vars copiado da home -->
</head>
<body>

  <!-- Mini header de contexto (apenas para a apresentação saber o que é) -->
  <header class="demo-header">
    <span class="demo-header__tag">Demo · Apresentação</span>
    <h1 class="demo-header__title">Proposta de seção · Tecnologia Nanocomprimido</h1>
    <p class="demo-header__sub">Peça isolada para validação do cliente. Não integrada à home aprovada.</p>
  </header>

  <!-- A seção em si -->
  <section class="tech-nano container">
    <div class="tech-nano__inner">

      <!-- COLUNA ESQUERDA: slider antes/depois -->
      <div class="tech-nano__slider">
        <div class="ba-slider">
          <img class="ba-slider__after"  src="[placeholder]" alt="Nanocomprimido Nanovit na palma da mão">
          <img class="ba-slider__before" src="[placeholder]" alt="Comprimido tradicional na palma da mão">
          <div class="ba-slider__handle" aria-hidden="true">
            <span class="ba-slider__handle-line"></span>
            <span class="ba-slider__handle-knob">
              <!-- SVG de duas setas opostas horizontais -->
            </span>
          </div>
          <span class="ba-slider__label ba-slider__label--before">Comprimido tradicional</span>
          <span class="ba-slider__label ba-slider__label--after">Nanocomprimido Nanovit</span>
          <span class="ba-slider__disclaimer">Imagem ilustrativa · foto de produção pendente</span>
        </div>
      </div>

      <!-- COLUNA DIREITA: texto -->
      <div class="tech-nano__copy">
        <span class="section__eyebrow">Tecnologia Nanovit</span>
        <h2>Menor. Mastigável. <em>Pra absorver já na boca.</em></h2>
        <p>O nanocomprimido Nanovit é menor que um comprimido tradicional e foi feito pra ser mastigado. A absorção começa pela mucosa oral, antes de chegar ao estômago.</p>
        <ul class="tech-nano__bullets">
          <li>Mastigável, com sabor agradável</li>
          <li>Sem precisar de água</li>
          <li>Absorção começa pela boca</li>
        </ul>
        <p class="tech-nano__note">Hoje na linha <strong>Melatonina</strong> e <strong>D3 + K2</strong>.</p>
      </div>

    </div>
  </section>

</body>
</html>
```

#### Variáveis CSS — copiar da home

Copiar para o `<style>` desta página o bloco `:root { ... }` que está em `index.html` (linhas ~10-30, contendo `--font-title`, `--color-brand`, `--color-cream`, `--color-text`, `--color-mute`, `--container`, `--radius-pill`, etc). Também copiar os imports de Google Fonts. Sem isso a peça vai parecer destacada da home quando mostrada lado a lado.

#### Mini-header de contexto (`.demo-header`)

Bloco sóbrio no topo da página, fundo creme claro ou branco, max-width container, padding 48px vertical:
- `.demo-header__tag`: pílula pequena cinza com texto "Demo · Apresentação" em uppercase 11px (sinaliza ao cliente que é peça de proposta).
- `.demo-header__title`: h1 tamanho médio (~28px) com o título da proposta.
- `.demo-header__sub`: subtítulo cinza explicando contexto.

Esse header é puramente contextual — fica fora da "seção real" que está sendo proposta. Deixa claro pro cliente que a seção começa abaixo.

#### Seção `.tech-nano` — layout

- **Desktop (>768px):** grid 2 colunas 50/50, gap 48px. Padding vertical generoso (64-96px).
- **Mobile (<768px):** stack vertical — slider em cima, copy embaixo.
- **Max-width:** mesmo container da home (`var(--container)`).

#### Slider — mock estático

- **Handle fixo no meio** (`left: 50%`, sem JS de drag).
- **Visual do handle:** linha vertical branca fina (2-3px) com knob circular branco (~36px) no centro. Dentro do knob, SVG de duas setas opostas horizontais (ícone padrão). Sombra suave (`0 2px 12px rgba(0,0,0,0.18)`) para destacar do fundo.
- **Imagem `after`:** ocupa o container inteiro, `object-fit: cover`.
- **Imagem `before`:** posicionada absoluta sobre a `after`, com `clip-path: inset(0 50% 0 0)` — mostra só os 50% da esquerda.
- **Labels (`--before` à esquerda, `--after` à direita):** pílulas em fundo branco translúcido (`rgba(255,255,255,0.9)`) com texto em font-title peso 600 size 11px, posicionadas no canto inferior de cada metade.
- **Disclaimer:** pílula pequena no canto superior do container do slider — texto "Imagem ilustrativa · foto de produção pendente" em 10px cinza sobre branco translúcido. Honestidade pra apresentação.
- **Aspect ratio do container:** 1:1 quadrado (ou 4:5 se ficar melhor com altura da coluna de texto).

#### Fotos — placeholder

Como ainda não temos as fotos reais de produção, criar SVG inline minimalista para cada lado:
- **Lado `before` (comprimido tradicional):** silhueta simples de mão (vista de cima/palma aberta) em tom rosa-bege neutro, segurando um **círculo grande** (~80px visual, branco/off-white) representando o comprimido tradicional.
- **Lado `after` (nanocomprimido):** mesma silhueta de mão, mesma posição/escala, segurando um **círculo pequeno** (~40px visual) representando o nanocomprimido.

Pode embutir o SVG diretamente como `<svg>` inline em vez de usar `<img src="data:...">`. O importante é que **a diferença de tamanho entre os dois círculos seja imediatamente visível**, pra apresentação comunicar a ideia mesmo com slider parado.

Estilo do SVG: minimalista, traço único stroke 1.5-2px sobre fill plano, sem detalhe excessivo. Paleta neutra (off-white/bege para a mão, branco para os comprimidos com borda cinza muito leve). Mais frame editorial do que ilustração técnica.

Se o dev preferir e tiver acesso a imagens stock genéricas adequadas (Unsplash gratuito tipo "hand holding pill"), pode usar — mas o SVG minimalista é mais coerente com a estética da Nanovit e funciona offline sem broken links.

#### Coluna direita — estilo

- **Eyebrow `.section__eyebrow`:** font-title weight 600, size 11px, uppercase, letter-spacing 0.14em, cor `--color-brand`. Margin-bottom 14px.
- **H2:** font-title weight 700, size ~36-42px desktop / ~26-30px mobile, letter-spacing -0.02em, line-height 1.1. `<em>` com cor `--color-brand`, font-style normal.
- **Parágrafo principal:** font-body size 15-16px, line-height 1.6, color `--color-text` ou levemente desbotado.
- **Bullets `.tech-nano__bullets`:** lista vertical com gap 12px. Cada `<li>` com ícone de check verde-suave/vermelho-marca à esquerda (mesmo SVG da PDP `.composition__icon`) + texto font-body size 14-15px. Sem marker padrão.
- **Nota final `.tech-nano__note`:** fundo creme `var(--color-cream)` em pílula sutil, padding 12px 18px, border-radius `var(--radius-pill)` ou retangular, texto size 13px, margin-top 24-32px. Comunica honestamente quais produtos têm o formato sem virar disclaimer pesado.

**Notas do Dev:** Arquivo criado em `demo-tecnologia-nano.html` no root. CSS `:root` copiado da home (vars + Google Fonts Montserrat Alternates). Mini-header construído com .demo-header em tom sóbrio (tag cinza + h1 + subtítulo + borda inferior leve pra separar da seção real). Seção `.tech-nano` em grid 2 colunas que vira stack vertical em <=768px. Slider `.ba-slider` em aspect-ratio 1/1 com handle fixo no meio (`left: 50%`) — sem JS, conforme orientação (marquei a classe `.ba-slider--static` no markup pra deixar pronto pra upgrade futuro de drag). Labels "Comprimido tradicional" (à esquerda, cor mute) e "Nanocomprimido Nanovit" (à direita, cor brand). Disclaimer "Imagem ilustrativa · foto de produção pendente" no topo do slider em pílula translúcida. SVG placeholder das mãos: minimalista — 4 retângulos arredondados como dedos + polegar curvo + palma elíptica + duas creases sutis. Em tons bege/cream (`#e8d4bc` fill, `#d4ba94` stroke). A única diferença entre os 2 SVGs é o tamanho da pílula (raio 20×12 no AFTER vs 46×28 no BEFORE) — diferença é imediata e óbvia mesmo com slider parado. Bullets com SVG de check em círculo verde-suave (success token).

---

## 🚧 Pendências bloqueantes
- **Fotos reais** ainda não existem. Cliente vai produzir depois.

## ❌ Não fazer
- **Não tocar em `index.html`** — home aprovada. Esta demo é arquivo separado.
- **Não tocar em `produto.html`.**
- **Não criar nav, header completo, footer ou outras seções** na demo — só o mini-header de contexto + a seção proposta.
- **Não adicionar JS de drag** no slider — handle fixo no meio basta. Se quiser deixar pronto pra upgrade futuro, marcar a classe `.ba-slider--static`.
- **Não inventar claim numérico** ("3x mais absorção", "5x mais rápido", "absorve 80% mais", etc). Único claim defensável usado neste brief: "absorção começa pela mucosa oral, antes de chegar ao estômago" (fato biológico, não claim de eficácia).
- **Não generalizar para a linha toda.** A nota final "Hoje na linha Melatonina e D3 + K2" é obrigatória — evita confusão com produtos em cápsula (5MAG, Slim+, Calm, etc).
- **Não esquecer o disclaimer "Imagem ilustrativa"** sobre o slider enquanto fotos forem placeholder.

## 📝 Log do Dev (preencher ao final)
- **Resumo:** Arquivo standalone `demo-tecnologia-nano.html` criado no root. Reusa todas as variáveis CSS e fontes da home pra não destoar quando mostrado lado a lado. Mini-header sóbrio sinaliza que é peça de proposta. Seção `.tech-nano` em grid 2 colunas (desktop) / stack (mobile). Slider estático com handle fixo em 50%, clip-path mostrando os 2 SVGs minimalistas. Index.html e produto.html não foram tocados.
- **Decisões fora do brief:**
  - SVG da mão: optei por composição com primitivas simples (rects arredondados como dedos + path do polegar + elipse da palma) em vez de um path único complexo. Lê melhor e é mais editável depois caso queiramos refinar. Mantém estética minimalista pedida no brief.
  - Adicionei classe `.ba-slider--static` no markup conforme sugestão do "Não fazer" — fica óbvio que é mock estático e marca o ponto onde JS de drag entraria depois.
  - Disclaimer posicionado no **topo** do slider (não no rodapé) pra não brigar com as 2 labels já presentes na base. Tom translúcido sutil.
  - Os 2 SVGs (BEFORE e AFTER) têm o mesmo desenho de mão pixel-a-pixel — só a pílula muda. Garante que quando o handle do slider move (mesmo manualmente em apresentação ao vivo, arrastando o CSS), a transição é exclusivamente sobre o tamanho do comprimido, que é o que importa contar.
- **Dúvidas pro Diretor:** Nenhuma. A peça está executável e cumpre o escopo. Quando as fotos reais chegarem, basta trocar os 2 blocos `.ba-slider__media` por `<img>` e remover o disclaimer.
- **Tempo aproximado:** ~20 minutos.
