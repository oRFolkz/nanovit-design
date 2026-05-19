# Brief — Migração Shopify · Fase 0 · Polimento

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** ajustes pontuais em `e:/Shopify/nanovit-theme/` decorrentes da revisão da Fase 0.
**Contexto estratégico:** Fase 0 foi aprovada. 3 ajustes técnicos identificados na revisão precisam ser feitos antes de declararmos a fundação fechada. Nada novo, só polimento.

---

## ✅ Checklist de execução

### Tarefa 1 — Logo: fallback para asset do tema

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** A sintaxe `shopify://shop_images/nanovit-logo.svg` em `settings_data.json` aponta para a área Files do admin Shopify, **não** para `assets/`. Se o cliente subir o tema sem fazer upload manual do SVG nos Files, o logo aparece quebrado. Solução: no partial do header, fallback para o asset físico do tema quando `settings.logo` vier vazio.
**Onde:**
1. `config/settings_data.json` — remover a chave `"logo": "shopify://shop_images/..."` do `current` (deixar `null` ou ausente).
2. `blocks/_header-logo.liquid` (ou snippet equivalente que renderiza o logo no header).

**O que fazer:**

No partial do logo, identificar o ponto onde `settings.logo` é renderizado e adicionar fallback:

```liquid
{%- if settings.logo != blank -%}
  {{ settings.logo | image_url: width: 480 | image_tag: ... }}
{%- else -%}
  <img
    src="{{ 'nanovit-logo.svg' | asset_url }}"
    alt="{{ shop.name }}"
    width="240"
    height="32"
    loading="eager"
  >
{%- endif -%}
```

(Adaptar atributos pro padrão do Horizon — manter `loading`, `width`, `height`, `class` que o Horizon já usa nos outros casos.)

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Reverter pill em swatches de opção

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Setar `border-radius: 999px` em swatches de variante deforma opções não-circulares (ex: "Pílula", "Pó", "Gomas") em óvalos finos. Pill funciona em cor (círculo perfeito), não em texto. Voltar pra radius-md (10px) nos campos de variant button/swatch.
**Onde:** `config/settings_data.json` (ou `config/settings_schema.json` se os defaults tiverem sido alterados também).

**O que fazer:** Trocar os valores dos campos abaixo para **10**:

- `variant_button_radius` → `10`
- `variant_swatch_radius` → `10`

Manter `badge_corner_radius: 999` (pill em badge está correto). Manter pill em buttons/inputs (tarefa 2 da Fase 0).

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Letter-spacing escalonado em headings

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Brand-guide define -0.02em em h1/h2 (títulos grandes), -0.01em em h3 (médio), neutro em h4+ (pequeno). Aplicar -0.02 uniforme aperta demais headings menores.
**Onde:** `assets/nanovit-tokens.css`.

**O que fazer:** Substituir o bloco atual de headings:

```css
h1, h2, h3, h4, h5, h6,
.h1, .h2, .h3, .h4, .h5, .h6 {
  font-family: var(--font-heading--family);
  letter-spacing: -0.02em;
}

h1, .h1 { font-weight: 700; line-height: 1; }
h2, .h2 { font-weight: 700; line-height: 1.1; }
h3, .h3 { font-weight: 700; line-height: 1.15; }
h4, .h4 { font-weight: 700; line-height: 1.2; }
```

Por:

```css
h1, h2, h3, h4, h5, h6,
.h1, .h2, .h3, .h4, .h5, .h6 {
  font-family: var(--font-heading--family);
  font-weight: 700;
}

h1, .h1 { line-height: 1;    letter-spacing: -0.02em; }
h2, .h2 { line-height: 1.1;  letter-spacing: -0.02em; }
h3, .h3 { line-height: 1.15; letter-spacing: -0.01em; }
h4, .h4 { line-height: 1.2;  letter-spacing: normal; }
h5, .h5 { line-height: 1.25; letter-spacing: normal; }
h6, .h6 { line-height: 1.3;  letter-spacing: normal; }
```

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- **T8 da Fase 0 (subir em staging)** continua bloqueada até o cliente prover loja Shopify dev.

## ❌ Não fazer
- Não introduzir alterações fora dos 3 escopos acima.
- Não mexer em `settings_schema.json` exceto se a Tarefa 2 exigir (provavelmente só `settings_data.json` basta).

## 📝 Log do Dev (preencher ao final)

- **Resumo:**
  - **T1** Re-baixei `nanovit-logo.svg` em `assets/` (tinha sido removido numa rodada intermediária quando a estratégia era "tudo placeholder"). Em `blocks/_header-logo.liquid`, envolvi o `{% render 'image' %}` original num `if settings.logo != blank` e adicionei o ramo `else` com `<img src="{{ 'nanovit-logo.svg' | asset_url }}" ...>`. Atributos `width`/`height` derivados de `settings.logo_height` (× 7 para largura aproximada), `loading="eager"`, `decoding="async"`. Resultado: tema "sai da caixa" com a logo Nanovit; quando o admin fizer upload no theme editor, o ramo `if` assume.
  - **T2** Em `settings_data.json`, troquei `variant_button_radius` e `variant_swatch_radius` de 100 → 10. Mantive `badge_corner_radius: 100` (pill em badge continua certo), `button_border_radius_primary/secondary: 100` e `inputs_border_radius: 32` (pill em CTA e inputs continua certo).
  - **T3** Em `assets/nanovit-tokens.css`, substituí o bloco uniforme `letter-spacing: -0.02em` em todas as headings pela escala do brief: h1/h2 −0.02em, h3 −0.01em, h4/h5/h6 neutro. `font-weight: 700` movido para o grupo (estava duplicado por nível).

- **Decisões fora do brief:** Nenhuma.

- **Dúvidas pro Diretor:** Nenhuma. Pendência conhecida: **T8 da Fase 0** (verificação em staging) continua bloqueada até loja Shopify dev/staging do cliente.

- **Tempo aproximado:** ~10 min.
