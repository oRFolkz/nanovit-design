# Brief — Ajustes finos pós-uploads do admin

> QA visual final do tema ativo (`Shopify-Nanovit/main`, 152710774963). Admin subiu quase tudo. Restam 2 ajustes de código.

---

## ✅ Tarefa 1 — JSON-LD Organization usar logo wordmark

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** `snippets/nanovit-schema-org.liquid` (ou onde o JSON-LD Organization está renderizado).

**Problema:** hoje o `logo` do schema Organization é o favicon SVG (`nanovit-favicon.svg`). Favicon é ícone, não logo. Google espera logo wordmark de até ~600px de largura.

**Fix:** trocar a ordem de fallback do `logo`:

```liquid
"logo": "{% if settings.logo %}https:{{ settings.logo | image_url: width: 600 }}{% else %}{{ 'nanovit-logo.svg' | asset_url }}{% endif %}",
```

(Era `nanovit-favicon.svg` no fallback; trocar pra `nanovit-logo.svg`.)

**Notas do Dev:** > _(preencher)_

---

## ✅ Tarefa 2 — Placeholder de imagem do CTA marca

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** `sections/nanovit-home-cta-banner.liquid`.

**Problema:** quando admin não tem imagem configurada, o placeholder atual é um SVG abstrato decorativo do Shopify (`placeholder_svg_tag`) — ilustração de "lifestyle" que parece deslocada na marca. Hoje o admin subiu foto real, mas se em outra loja/contexto for usado sem foto, fica feio.

**Fix:** substituir o fallback por um placeholder limpo padrão do tema (igual ao do hero):

```liquid
{%- if img -%}
  {{ img | image_url: width: 1200 | image_tag: ... }}
{%- else -%}
  <div class="nanovit-home-cta-banner__placeholder" role="img" aria-label="Imagem ainda não configurada">
    Suba a imagem pelo theme editor.
  </div>
{%- endif -%}
```

Com CSS:
```css
.nanovit-home-cta-banner__placeholder {
  width: 100%; height: 100%;
  display: flex; align-items: center; justify-content: center;
  background: var(--nanovit-color-cream, #faf6f1);
  color: var(--nanovit-color-mute, #8a7a76);
  font-family: var(--font-heading--family);
  font-size: 14px;
  text-align: center;
  padding: 24px;
}
```

**Notas do Dev:** > _(preencher)_

---

## Pendência admin (não dev)

- **Médicos:** preencher dados reais dos 5 médicos (fotos, nomes, credenciais, quotes). Hoje só Dr. Barakat tem foto, e texto + autor ainda são placeholders.
- **Settings → General → Store name:** trocar "NexgeniusStore" pra "Nanovit Nutrition" — afeta title, OG, JSON-LD.

## ❌ Não fazer
- Não tocar nas sections que estão com conteúdo real (CTA marca, Institucional, UGC, Linha, Marketplaces) — só nos fallbacks dela.

## 📝 Log do Dev

**Resumo:**
- T1 ✅ `snippets/nanovit-schema-org.liquid` linha 18 — fallback do `logo` trocou de `nanovit-favicon.svg` pra `nanovit-logo.svg` (asset que já existe no tema). Agora Google recebe logo wordmark, não ícone.
- T2 ✅ `sections/nanovit-home-cta-banner.liquid` linha 29 — substituí `placeholder_svg_tag` do Shopify ("lifestyle-1") por `<div class="nanovit-home-cta-banner__placeholder">` com texto "Suba a imagem pelo theme editor". CSS do placeholder cream + texto mute adicionado ao bloco `{% stylesheet %}` da section.

**Push staging:** ✅ limpo. https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659

**Decisões:** nenhuma fora do brief.

**Dúvidas:** nenhuma.

**Tempo:** ~5min.
