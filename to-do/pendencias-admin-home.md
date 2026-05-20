# Pendências admin Shopify — Home

> Cliente faz no admin pra home "sair da caixa pronta". Sem isso, sections renderizam placeholders cream com instrução. Não bloqueia, só fica vazio.

## 1. Hero · Slideshow

**Theme editor → Home → Hero · Slideshow → Slide N**

- Subir 3 a 5 imagens, recomendado 2600×1140px (16:7), formato JPG ou WebP.
- Mobile (opcional): subir variante 750×900px se quiser arte vertical pra mobile.
- Preencher `alt` de cada imagem (acessibilidade + SEO).
- Opcional: `link` clicável (PDP, coleção, etc.).

## 2. Linha Nanovit · 7 coleções

**Admin → Produtos → Coleções → Create collection**

Criar 7 coleções com handles abaixo + atribuir `collection.image` (foto do produto-líder da linha).

| Handle | Título | Sugestão de imagem |
|---|---|---|
| `emagrecimento` | Emagrecimento | foto Laranja Moro Slim+ |
| `sono-relaxamento` | Sono & Relaxamento | foto Nano Calm |
| `energia-disposicao` | Energia & Disposição | foto Nano 5Mag |
| `imunidade` | Imunidade | foto NAC |
| `beleza` | Beleza | foto colágeno (criar produto se não houver) |
| `saude-da-mulher` | Saúde da Mulher | foto multivit feminino |
| `performance` | Performance | foto creatina ou whey |

Depois, no theme editor → Home → Linha Nanovit → cada block → escolher a coleção correspondente. Cor de fundo do card já está pré-definida no JSON.

## 3. Bestsellers · coleção

**Admin → Produtos → Coleções → Create collection**

- Handle: `bestsellers`
- Tipo: Smart (condição: tag igual a `best`) ou Manual.
- Atribuir produtos top da loja.

No theme editor → Home → Mais Vendidos → settings → Coleção fonte = `bestsellers`.

## 4. Kits · coleção

**Admin → Produtos → Coleções → Create collection**

- Handle: `kits`
- Tipo: Smart (tag = `kit`) ou Manual.
- Idealmente cada "kit" é um produto Shopify (bundle) com variantes (sabores, quantidades).

No theme editor → Home → Kits Nanovit → settings → Coleção fonte = `kits`.

## 5. Médicos parceiros

**Theme editor → Home → Médicos · Banner → Médico N**

Pra cada slot (5 default):
- Foto: avatar 500×500px (preferir background neutro/cinza).
- Nome curto (label do avatar): "Dr. Barakat".
- Quote: depoimento curto em primeira pessoa.
- Autor: nome completo + credencial, **sem travessão** (use vírgula). Ex: "Dr. Victor Barakat, Médico Funcional Integrativo".

## 6. UGC · Usam e indicam

**Theme editor → Home → UGC · Usam e indicam → Depoimento UGC N**

Pra cada slot (5 default):
- Vídeo: upload nativo (MP4 H.264, vertical 9:14 ou 9:16, máx ~20MB) OU URL externa HLS/.m3u8.
- Poster: imagem de capa 360×560px (mostra antes do play).
- Alt: descrição curta do que a pessoa faz no vídeo.
- Produto relacionado: escolher um produto da loja (o mini card aparece abaixo do vídeo).

## 7. Banner Institucional

**Theme editor → Home → Banner institucional → settings**

- Imagem de fundo: 2400×1050px (16:7), foco do assunto à direita (a copy fica à esquerda).
- Eyebrow / título / body / CTA já estão com defaults bons, ajustar se quiser.
- `overlay_strength`: aumentar pra `strong` se a imagem competir com a legibilidade do texto.

## 8. Marketplaces (Onde Comprar)

**Theme editor → Home → Onde Comprar → Logo de marketplace N**

Subir 5 logos das redes que vendem Nanovit. PNG transparente ou SVG monocromático. Altura renderizada: máx 44px.

Sugestões:
- Droga Raia
- Drogasil
- Panvel
- Amazon
- Mercado Livre

## 9. Metafields opcionais

**Admin → Settings → Custom data → Products → Add definition**

Criar 3 metafield definitions (namespace `nanovit`):

| Key | Type | Conteúdo |
|---|---|---|
| `rating` | Decimal (single) | 0.0 a 5.0 (ex: 4.9) |
| `review_count` | Integer (single) | Número de reviews (ex: 287) |
| `tags_display` | Single line text (list) | Lista de tags visuais (ex: ["Vegano", "Sem glúten", "Nanotech"]) |

Depois, em cada produto, abrir Metafields e preencher os 3 campos. Sem isso, o card de produto omite estrelas/tags (não fica quebrado, só fica "limpo").

---

## Resumo de prioridade

**Alta (sem isso a home parece em construção):**
- Hero (1) — imagens
- Bestsellers (3) — criar coleção e atribuir produtos
- Marketplaces (8) — logos das redes

**Média (sem isso falta densidade editorial):**
- Linha Nanovit (2) — 7 coleções com imagem
- Médicos (5) — 5 fotos + textos
- Banner Institucional (7) — foto + ajustes

**Baixa (refinamento):**
- Kits (4)
- UGC (6) — vídeos
- Metafields (9)

Quando você sinalizar que terminou pelo menos a prioridade alta, abro Chrome DevTools e faço QA visual completo.
