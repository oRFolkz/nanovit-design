# Brief — Calculadora de Frete (simulada) na PDP

> Documento de trabalho entre dois agentes
> - Diretor: define o que fazer e por quê
> - Dev: executa, marca checkbox, preenche campo de notas

**Escopo desta rodada:** `produto.html` (apenas)
**Contexto estratégico:**
A PDP do Nano 5Mag tem todos os gatilhos de e-commerce mass-market (cashback, parcelamento, frete grátis acima de R$ 147, ANVISA, troca 7 dias) mas não responde a UMA pergunta que o usuário faz antes de clicar "Adicionar ao carrinho": **"chega quando aqui?"**. Em ticket de R$ 89, prazo + custo de frete são gatilho de conversão tão forte quanto o preço. Hoje a página é silenciosa nisso.

Como ainda não há integração real com Correios/transportadora, vamos entregar uma **simulação visual funcional**: o usuário digita o CEP, vê um spinner, e recebe 3 opções (PAC, SEDEX, Expressa) com prazo e custo plausíveis. Tudo client-side, sem fetch. Quando houver backend real, o dev troca apenas a função `simulateShipping()`.

Referência de espírito: Amazon BR, Mercado Livre, Bagaggio. Bloco compacto, sem moldura pesada, abre painel de resultado abaixo.

---

## ✅ Checklist de execução

### Tarefa 1 — CSS do bloco de frete

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Precisamos de um bloco enxuto que conviva com o visual atual (cream, radius arredondado, font-title). Não pode parecer "widget colado".
**Onde:** `produto.html`, dentro da `<style>` da PDP, logo após o bloco `.pdp__cta-note` (perto da linha 805).
**O que fazer:** Adicionar o seguinte CSS:

```css
/* — Calculadora de frete — */
.shipping {
  border: 1px solid rgba(42,28,26,0.10);
  border-radius: var(--radius-lg);
  padding: 14px 16px;
  background: #fff;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.shipping__head {
  display: flex; align-items: center; justify-content: space-between;
  gap: 8px;
}
.shipping__title {
  font-family: var(--font-title);
  font-size: 13px;
  font-weight: 700;
  color: var(--color-text);
  display: flex; align-items: center; gap: 8px;
}
.shipping__title svg { width: 16px; height: 16px; }
.shipping__help {
  font-family: var(--font-body);
  font-size: 11px;
  color: var(--color-mute);
  text-decoration: underline;
  text-underline-offset: 2px;
}
.shipping__form {
  display: flex; gap: 8px;
}
.shipping__input {
  flex: 1;
  height: 42px;
  padding: 0 14px;
  border: 1px solid rgba(42,28,26,0.18);
  border-radius: var(--radius-pill);
  font-family: var(--font-body);
  font-size: 14px;
  letter-spacing: 0.02em;
  color: var(--color-text);
  background: #fff;
  transition: border-color 0.2s;
}
.shipping__input:focus {
  outline: none;
  border-color: var(--color-brand);
}
.shipping__input::placeholder { color: rgba(42,28,26,0.35); }
.shipping__submit {
  height: 42px;
  padding: 0 18px;
  border-radius: var(--radius-pill);
  background: var(--color-text);
  color: #fff;
  font-family: var(--font-title);
  font-weight: 600;
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  cursor: pointer;
  transition: background 0.2s;
}
.shipping__submit:hover { background: var(--color-brand); }
.shipping__submit:disabled { opacity: 0.5; cursor: wait; }

.shipping__results {
  display: none;
  flex-direction: column;
  gap: 0;
  border-top: 1px solid rgba(42,28,26,0.08);
  padding-top: 10px;
  margin-top: 2px;
}
.shipping__results.is-open { display: flex; }

.shipping__loading {
  display: none;
  align-items: center; gap: 10px;
  font-family: var(--font-body);
  font-size: 12px;
  color: var(--color-mute);
  padding: 6px 0;
}
.shipping__loading.is-open { display: flex; }
.shipping__loading::before {
  content: '';
  width: 14px; height: 14px;
  border-radius: 50%;
  border: 2px solid rgba(42,28,26,0.15);
  border-top-color: var(--color-brand);
  animation: shipping-spin 0.7s linear infinite;
}
@keyframes shipping-spin { to { transform: rotate(360deg); } }

.shipping__option {
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: center;
  gap: 4px 12px;
  padding: 10px 0;
  border-bottom: 1px dashed rgba(42,28,26,0.10);
}
.shipping__option:last-child { border-bottom: none; }
.shipping__option-name {
  font-family: var(--font-title);
  font-size: 13px;
  font-weight: 600;
  color: var(--color-text);
}
.shipping__option-price {
  font-family: var(--font-title);
  font-size: 13px;
  font-weight: 700;
  color: var(--color-text);
  justify-self: end;
}
.shipping__option-price--free {
  color: var(--color-success);
  text-transform: uppercase;
  font-size: 11px;
  letter-spacing: 0.06em;
}
.shipping__option-eta {
  grid-column: 1 / 2;
  font-family: var(--font-body);
  font-size: 11.5px;
  color: var(--color-mute);
}

.shipping__error {
  display: none;
  font-family: var(--font-body);
  font-size: 12px;
  color: #b3261e;
  padding: 4px 0;
}
.shipping__error.is-open { display: block; }

.shipping__cep-link {
  font-family: var(--font-body);
  font-size: 11px;
  color: var(--color-mute);
  text-decoration: underline;
  text-underline-offset: 2px;
}
.shipping__cep-link:hover { color: var(--color-brand); }
```

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 2 — HTML do bloco de frete

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** O bloco vive na buy-box. Posição: **entre o bloco de cashback (perks) e o botão `#addCta`**. É o último gatilho antes do CTA; entrega a informação que destrava o clique.
**Onde:** `produto.html`, logo após a `</div>` que fecha o bloco `.pdp__perks` (antes da linha 2744, `<button class="pdp__cta" id="addCta">`).
**O que fazer:** Inserir este markup:

```html
<!-- Calculadora de frete (simulação client-side) -->
<div class="shipping" id="shipping">
  <div class="shipping__head">
    <span class="shipping__title">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="1" y="3" width="15" height="13"/><polygon points="16 8 20 8 23 11 23 16 16 16 16 8"/><circle cx="5.5" cy="18.5" r="2.5"/><circle cx="18.5" cy="18.5" r="2.5"/></svg>
      Calcular frete e prazo
    </span>
    <a href="https://buscacepinter.correios.com.br/app/endereco/index.php" target="_blank" rel="noopener" class="shipping__cep-link">Não sei meu CEP</a>
  </div>

  <form class="shipping__form" id="shippingForm" novalidate>
    <input
      type="text"
      class="shipping__input"
      id="shippingCep"
      placeholder="00000-000"
      inputmode="numeric"
      maxlength="9"
      autocomplete="postal-code"
      aria-label="CEP de entrega"
    />
    <button type="submit" class="shipping__submit" id="shippingSubmit">Calcular</button>
  </form>

  <div class="shipping__error" id="shippingError">CEP inválido. Confira os 8 dígitos.</div>
  <div class="shipping__loading" id="shippingLoading">Calculando opções de entrega…</div>
  <div class="shipping__results" id="shippingResults" aria-live="polite"></div>
</div>
```

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 3 — JavaScript da simulação

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Comportamento esperado:
1. Usuário digita CEP. Máscara aplica `00000-000` automaticamente.
2. Submit valida (8 dígitos). Inválido → exibe erro abaixo, mantém foco no input.
3. Válido → esconde resultado anterior, mostra loading por **800–1200ms** (variar random pra parecer fetch real).
4. Renderiza 3 opções: **Expressa**, **SEDEX**, **PAC**, com prazos e custos derivados do primeiro dígito do CEP (faixa regional, ver tabela abaixo).
5. Como o produto custa R$ 89 e o frete grátis é acima de R$ 147, **PAC sempre aparece como custo cheio** (não dispara grátis com 1 unidade). Mas adicionar uma frase abaixo: _"Frete grátis a partir de R$ 147 no PAC."_

**Onde:** `produto.html`, no `<script>` do final do `<body>`. Se já existir um bloco de scripts da PDP (procurar por `addCta` ou `id="addCta"` perto do fim), adicionar lá. Senão, criar `<script>` próximo ao `</body>`.

**O que fazer:** Adicionar:

```javascript
(function initShippingCalc() {
  const form = document.getElementById('shippingForm');
  const input = document.getElementById('shippingCep');
  const submit = document.getElementById('shippingSubmit');
  const loading = document.getElementById('shippingLoading');
  const results = document.getElementById('shippingResults');
  const errorBox = document.getElementById('shippingError');
  if (!form) return;

  // Máscara 00000-000
  input.addEventListener('input', (e) => {
    let v = e.target.value.replace(/\D/g, '').slice(0, 8);
    if (v.length > 5) v = v.slice(0, 5) + '-' + v.slice(5);
    e.target.value = v;
    errorBox.classList.remove('is-open');
  });

  // Tabela de simulação por região (1º dígito do CEP)
  // Faixas reais dos Correios: 0/1 SP, 2 RJ/ES, 3 MG, 4 BA/SE, 5 PE/AL/PB/RN, 6 CE/PI/MA/PA/AM/AP/RR, 7 DF/GO/TO/MT/MS/RO/AC, 8 PR/SC, 9 RS
  const TABLE = {
    '0': { pac: [13.90, '5 a 7 dias úteis'],  sedex: [24.90, '2 a 3 dias úteis'], expressa: [39.90, 'até 1 dia útil'] },
    '1': { pac: [13.90, '5 a 7 dias úteis'],  sedex: [24.90, '2 a 3 dias úteis'], expressa: [39.90, 'até 1 dia útil'] },
    '2': { pac: [16.90, '5 a 8 dias úteis'],  sedex: [27.90, '2 a 4 dias úteis'], expressa: [44.90, '1 a 2 dias úteis'] },
    '3': { pac: [17.90, '6 a 9 dias úteis'],  sedex: [29.90, '3 a 4 dias úteis'], expressa: [49.90, '1 a 2 dias úteis'] },
    '4': { pac: [22.90, '7 a 11 dias úteis'], sedex: [34.90, '4 a 6 dias úteis'], expressa: null },
    '5': { pac: [24.90, '8 a 12 dias úteis'], sedex: [36.90, '5 a 7 dias úteis'], expressa: null },
    '6': { pac: [28.90, '9 a 14 dias úteis'], sedex: [42.90, '6 a 9 dias úteis'], expressa: null },
    '7': { pac: [21.90, '6 a 10 dias úteis'], sedex: [32.90, '4 a 6 dias úteis'], expressa: null },
    '8': { pac: [18.90, '5 a 8 dias úteis'],  sedex: [28.90, '3 a 5 dias úteis'], expressa: [49.90, '1 a 2 dias úteis'] },
    '9': { pac: [22.90, '7 a 10 dias úteis'], sedex: [34.90, '4 a 6 dias úteis'], expressa: null },
  };

  function simulateShipping(cep) {
    // Substituir esta função por chamada real ao backend quando disponível.
    const region = cep.charAt(0);
    const row = TABLE[region] || TABLE['0'];
    const out = [];
    if (row.expressa) out.push({ name: 'Entrega expressa', price: row.expressa[0], eta: row.expressa[1] });
    out.push({ name: 'SEDEX', price: row.sedex[0], eta: row.sedex[1] });
    out.push({ name: 'PAC', price: row.pac[0], eta: row.pac[1] });
    return out;
  }

  function formatPrice(v) {
    return 'R$ ' + v.toFixed(2).replace('.', ',');
  }

  function renderResults(options) {
    results.innerHTML = options.map(o => `
      <div class="shipping__option">
        <span class="shipping__option-name">${o.name}</span>
        <span class="shipping__option-price">${formatPrice(o.price)}</span>
        <span class="shipping__option-eta">${o.eta}</span>
      </div>
    `).join('') + `
      <div class="shipping__option" style="border-bottom:none;padding-top:8px;">
        <span class="shipping__option-eta" style="grid-column:1 / -1;">Frete grátis no PAC a partir de R$ 147 em produtos.</span>
      </div>
    `;
    results.classList.add('is-open');
  }

  form.addEventListener('submit', (e) => {
    e.preventDefault();
    const cep = input.value.replace(/\D/g, '');
    errorBox.classList.remove('is-open');
    results.classList.remove('is-open');

    if (cep.length !== 8) {
      errorBox.classList.add('is-open');
      input.focus();
      return;
    }

    submit.disabled = true;
    loading.classList.add('is-open');
    const delay = 800 + Math.random() * 400;

    setTimeout(() => {
      loading.classList.remove('is-open');
      submit.disabled = false;
      renderResults(simulateShipping(cep));
    }, delay);
  });
})();
```

**Notas do Dev:** > _(preencher após executar)_

---

### Tarefa 4 — Verificação visual

**Status:** [ ] Pendente · [x] Concluído · [ ] Bloqueado
**Por quê:** Garantir que o bloco não quebra o ritmo visual da buy-box e que a simulação funciona em CEPs de várias regiões.
**Onde:** Abrir `produto.html` no navegador.
**O que fazer:**
- Conferir alinhamento: bloco fica acima do botão "Adicionar ao carrinho", abaixo do bloco de cashback.
- Testar CEPs: `01310-100` (SP), `20040-020` (RJ), `40020-000` (BA), `69000-000` (AM), `88010-000` (SC). Cada um deve retornar valores diferentes coerentes com a tabela.
- Testar CEP inválido (`123`) → deve exibir mensagem de erro vermelha.
- Testar foco no input → borda muda pra cor da marca.
- Testar mobile (DevTools, 375px) → form não quebra, input e botão ocupam linha cheia confortavelmente.

**Notas do Dev:** > _(preencher após executar)_

---

## 🚧 Pendências bloqueantes
- **Backend real de frete:** quando houver integração Correios/Melhor Envio/transportadora, substituir `simulateShipping()` por fetch ao endpoint. Função já está isolada pra facilitar a troca.
- **Política real de frete grátis:** a frase "a partir de R$ 147" assume o que está na marquee do topo. Cliente confirmar se vale só pro PAC ou pra todas as modalidades, e se R$ 147 é o número atual.

## ❌ Não fazer
- Não usar API externa (ViaCEP, Correios) nesta rodada. É simulação 100% client-side.
- Não mexer no botão `#addCta`, no bloco de perks ou na trust row.
- Não adicionar campo de quantidade, cupom ou qualquer outro controle no bloco. Escopo é só frete.
- Não usar emoji no markup final. Os ícones SVG já bastam.
- Não usar travessão (—) em nenhuma string visível ao usuário.

## 📝 Log do Dev (preencher ao final)
- Resumo: Aplicado tudo conforme brief em `produto.html`. CSS inserido logo após `.pdp__cta-note` (linha ~806). HTML do bloco `.shipping` inserido entre `.pdp__perks` e `#addCta` (após linha 2742). JS inserido em novo `<script>` no final do body, após o script de sticky header / mobile menu.
- Decisões fora do brief: Nenhuma. Markup, CSS e JS aplicados literalmente. Verificado que `--color-success`, `--radius-lg` e `--radius-pill` existem nas variáveis CSS do arquivo.
- Dúvidas pro Diretor: O brief menciona "Frete grátis a partir de R$ 147 no PAC" mas o bloco `.pdp__perks` na PDP atual diz "Frete grátis acima de R$ 199,90". Manti o que o brief pediu (R$ 147) no resultado da simulação, mas há divergência com a marquee/perks. Confirmar qual valor é o correto.
- Tempo aproximado: ~10 min.
