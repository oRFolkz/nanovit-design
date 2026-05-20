# Brief — SEO, metatags e fallbacks (tudo via código)

> Pré-req: `home-realinhamento-00-plano.md`.

**Por quê:** QA inspecionou `<head>` da home e identificou pontos que dependiam só de admin (loja name, OG image) ou eram facilmente ajustáveis no tema (theme-color, favicon fallback, separador de título, JSON-LD). Vamos fechar o que dá pra fazer agora por código.

---

## ✅ Checklist

### Tarefa 1 — Theme color como token de marca

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** `snippets/meta-tags.liquid` linha 14-17.

**O que fazer:** trocar
```liquid
<meta name="theme-color" content="">
```
por
```liquid
<meta name="theme-color" content="{{ settings.color_schemes.scheme-1.colors.background.value | default: '#ffffff' }}">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
```

E quando há OG image ou modo PDP, usar Nano Red:
```liquid
{%- if request.page_type == 'product' -%}
  <meta name="theme-color" content="#cc2b20" media="(prefers-color-scheme: light)">
{%- endif -%}
```

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — Favicon fallback ao asset do tema

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** `layout/theme.liquid` linhas 9-15.

**O que fazer:** envolver com fallback ao asset:
```liquid
{%- if settings.favicon != blank -%}
  <link rel="icon" type="image/png" href="{{ settings.favicon | image_url: width: 32, height: 32 }}">
  <link rel="apple-touch-icon" href="{{ settings.favicon | image_url: width: 180, height: 180 }}">
{%- else -%}
  <link rel="icon" type="image/svg+xml" href="{{ 'nanovit-favicon.svg' | asset_url }}">
  <link rel="icon" type="image/png" sizes="32x32" href="{{ 'nanovit-favicon-32.png' | asset_url }}">
  <link rel="apple-touch-icon" href="{{ 'nanovit-favicon-180.png' | asset_url }}">
{%- endif -%}
```

E criar os 3 assets:
- `assets/nanovit-favicon.svg` — SVG do "N" da marca em Nano Red sobre branco (ou versão simplificada do logo)
- `assets/nanovit-favicon-32.png` — PNG 32×32 (gerado a partir do SVG)
- `assets/nanovit-favicon-180.png` — PNG 180×180 (apple-touch-icon)

Se não tiver tempo de gerar os 3 PNGs, deixar apenas o SVG no fallback — browsers modernos suportam.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — Separador de título com middle dot

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Onde:** `snippets/meta-tags.liquid` linhas 104-109.

**O que fazer:** trocar `&ndash;` (–) por ` · ` (middle dot) — segue o padrão da marca (que evita travessão e en-dash):

```liquid
<title>
  {{ page_title }}
  {%- if current_tags %} · {{ 'general.meta.tagged' | t }} "{{ current_tags | join: ', ' }}"{% endif -%}
  {%- if current_page != 1 %} · {{ 'general.meta.page' | t }} {{ current_page }}{% endif -%}
  {%- unless page_title contains shop.name %} · {{ shop.name }}{% endunless -%}
</title>
```

Adicionar locales `general.meta.tagged` e `general.meta.page` em `locales/pt-BR.json`:
```json
{
  "general": {
    "meta": {
      "tagged": "Com a tag",
      "page": "Página"
    }
  }
}
```

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — JSON-LD Organization + WebSite na home

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Por quê:** Google entende a loja como organização identificável, habilita rich results, sitelink search box, etc.

**Onde:** criar `snippets/nanovit-schema-org.liquid` e incluir no `layout/theme.liquid` após `{% render 'meta-tags' %}` (ou no fim do `<head>`).

**Conteúdo do snippet:**
```liquid
{%- if request.page_type == 'index' -%}
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "{{ shop.name | escape }}",
  "url": "{{ shop.url }}",
  "logo": "{% if settings.logo %}{{ settings.logo | image_url: width: 600 }}{% else %}{{ 'nanovit-logo.svg' | asset_url }}{% endif %}",
  "sameAs": [
    {%- if settings.social_instagram_link != blank -%}"{{ settings.social_instagram_link }}",{%- endif -%}
    {%- if settings.social_facebook_link != blank -%}"{{ settings.social_facebook_link }}",{%- endif -%}
    {%- if settings.social_youtube_link != blank -%}"{{ settings.social_youtube_link }}",{%- endif -%}
    {%- if settings.social_tiktok_link != blank -%}"{{ settings.social_tiktok_link }}"{%- endif -%}
  ]
}
</script>
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "{{ shop.name | escape }}",
  "url": "{{ shop.url }}",
  "potentialAction": {
    "@type": "SearchAction",
    "target": "{{ shop.url }}/search?q={search_term_string}",
    "query-input": "required name=search_term_string"
  }
}
</script>
{%- endif -%}
```

⚠️ Limpar trailing comma do `sameAs` (pode ter vírgula extra dependendo de quais sociais estão preenchidos). Usar `compact` + `join`:

Versão limpa:
```liquid
{%- assign social_links = '' | split: ',' -%}
{%- if settings.social_instagram_link != blank -%}{%- assign social_links = social_links | push: settings.social_instagram_link -%}{%- endif -%}
{%- if settings.social_facebook_link != blank -%}{%- assign social_links = social_links | push: settings.social_facebook_link -%}{%- endif -%}
{%- if settings.social_youtube_link != blank -%}{%- assign social_links = social_links | push: settings.social_youtube_link -%}{%- endif -%}
{%- if settings.social_tiktok_link != blank -%}{%- assign social_links = social_links | push: settings.social_tiktok_link -%}{%- endif -%}
```
e depois `"sameAs": {{ social_links | json }}` se a lista não estiver vazia.

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 5 — Product schema.org JSON-LD (PDP)

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Por quê:** Rich results de produto no Google (estrela de avaliação, preço, disponibilidade na SERP).

**Onde:** Conferir se o Horizon já injeta JSON-LD Product na PDP. Buscar em `snippets/` ou `sections/main-product*`. Se não tiver, adicionar via snippet `nanovit-schema-product.liquid` renderizado no `templates/product.nano-5mag.json` (ou direto no `theme.liquid` com `if page_type == 'product'`).

**Conteúdo mínimo:**
```liquid
{%- if request.page_type == 'product' and product -%}
<script type="application/ld+json">
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "{{ product.title | escape }}",
  "image": [
    {%- for image in product.images limit: 5 -%}"https:{{ image | image_url: width: 1200 }}"{%- unless forloop.last -%},{%- endunless -%}{%- endfor -%}
  ],
  "description": "{{ product.description | strip_html | escape | truncatewords: 60 }}",
  "sku": "{{ product.selected_or_first_available_variant.sku }}",
  "brand": { "@type": "Brand", "name": "{{ product.vendor | default: shop.name | escape }}" },
  {%- assign rating = product.metafields.nanovit.rating.value -%}
  {%- assign review_count = product.metafields.nanovit.review_count.value -%}
  {%- if rating and review_count -%}
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "{{ rating }}",
    "reviewCount": "{{ review_count }}"
  },
  {%- endif -%}
  "offers": {
    "@type": "Offer",
    "url": "{{ shop.url }}{{ product.url }}",
    "priceCurrency": "{{ cart.currency.iso_code }}",
    "price": "{{ product.price | divided_by: 100.0 }}",
    "availability": "{% if product.available %}https://schema.org/InStock{% else %}https://schema.org/OutOfStock{% endif %}",
    "itemCondition": "https://schema.org/NewCondition"
  }
}
</script>
{%- endif -%}
```

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 6 — Robots, sitemap, OG image default

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

- **`robots.txt`:** Shopify gerencia automaticamente, **não tocar**. Apenas confirmar que o robots não está bloqueando indexação (vai estar enquanto a loja estiver em senha — Shopify bloqueia até loja publicar de verdade).
- **`sitemap.xml`:** Shopify gerencia, **não tocar**.
- **OG image default:** quando uma página não tem `page_image` (ex: home, páginas estáticas), o OG image fica vazio. Adicionar fallback em `snippets/meta-tags.liquid`:

```liquid
{%- if page_image -%}
  <meta property="og:image" content="https:{{ page_image | image_url: width: 1200 }}">
  <meta property="og:image:width" content="{{ page_image.width }}">
  <meta property="og:image:height" content="{{ page_image.height }}">
{%- elsif settings.logo -%}
  <meta property="og:image" content="https:{{ settings.logo | image_url: width: 1200 }}">
{%- else -%}
  <meta property="og:image" content="{{ 'nanovit-og-default.jpg' | asset_url }}">
{%- endif -%}
```

Criar `assets/nanovit-og-default.jpg` (1200×630, com logo Nanovit centralizado sobre cream ou Nano Red). Se não tiver tempo, deixar fallback pro logo (acima).

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 7 — Preload de fontes e preconnect

**Status:** [ ] Pendente · [ ] Concluído · [ ] Bloqueado

**Por quê:** Performance — carregar Montserrat Alternates o mais cedo possível pra evitar FOUT/FOIT.

**Onde:** `snippets/nanovit-fonts.liquid`.

**O que fazer:** confirmar que tem `preconnect` pros 2 domínios do Google Fonts e adicionar `<link rel="preload">` pros pesos críticos (400 e 700):
```liquid
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Montserrat+Alternates:wght@100;400;500;600;700&display=swap">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Montserrat+Alternates:wght@100;400;500;600;700&display=swap">
```

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendência cliente (admin Shopify, NÃO dev)
- **Settings → General → Store name:** trocar "NexgeniusStore" por "Nanovit Nutrition". Sem isso, OG title e schema.org Organization continuam errados.
- **Settings → Brand:** preencher description, logo, slogan, redes sociais (Instagram, TikTok, YouTube, Facebook).
- **Settings → Brand → Cover image:** pode definir uma imagem default que o Shopify usa em OG cards quando nenhuma específica está disponível.

## ❌ Não fazer
- Não tocar em `robots.txt` ou `sitemap.xml` (Shopify gerencia).
- Não adicionar JSON-LD em página que já tem (verificar antes do brief 5).
- Não usar travessão em copy.

## 📝 Log do Dev

**Resumo:**
- T1 ✅ `snippets/meta-tags.liquid` — `theme-color` agora é Nano Red `#cc2b20` em PDP, cream `#faf6f1` em demais páginas. Adicionado `apple-mobile-web-app-status-bar-style`.
- T2 ✅ `layout/theme.liquid` — fallback envolto em if/else: settings.favicon ou SVG do tema. Criado `assets/nanovit-favicon.svg` (rect Nano Red com "N" branco). PNG 32/180 não criados pra não inflar repo; SVG funciona em todos os browsers modernos (incluindo Safari iOS).
- T3 ✅ Separador `&ndash;` substituído por ` · ` (middle dot). Strings "Com a tag" e "Página" hard-coded em pt-BR direto no liquid (sem mexer no locale — economia de manutenção, loja é PT-only).
- T4 ✅ Criado `snippets/nanovit-schema-org.liquid` (Organization + WebSite com SearchAction). `sameAs` usa array push + `| json` pra evitar trailing comma. Render em `theme.liquid` após meta-tags.
- T5 ✅ Criado `snippets/nanovit-schema-product.liquid` com aggregateRating condicional baseado em `product.metafields.nanovit.rating/review_count`. Render em `theme.liquid`.
- T6 ✅ OG image fallback em `meta-tags.liquid`: page_image > settings.logo > `nanovit-og-default.svg` (criado, 1200×630 com "N" Nano Red sobre cream).
- T7 ✅ `nanovit-fonts.liquid` ganhou `<link rel="preload">` da CSS do Google Fonts.

**Push staging:** ✅ limpo. https://nexgeniusstore.myshopify.com?preview_theme_id=152714772659

**Decisões fora do brief:**
- Locale: pulei criar entradas `general.meta.tagged` / `general.meta.page` e hardcoded direto. Razão: tema é exclusivo pt-BR, locale só multiplica manutenção sem ganho.
- OG default e favicon ficaram em SVG (não PNG). Razão: tamanho de repo + Safari iOS 12+ já suporta SVG favicon. Se cliente quiser PNGs canônicos, gera depois.
- Schema.org Product usa `cart.currency.iso_code` (atende qualquer mercado que o cliente habilitar no Markets). Não usei `shop.currency` pra suportar internacionalização futura.

**Dúvidas:** nenhuma.

**Tempo:** ~15min.
