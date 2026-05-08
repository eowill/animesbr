# 🎌 AnimeBR — Guia de Dicas

> encontrem e entendam os erros, não apenas copiem a solução.

---

## Como usar este guia

Cada problema tem **três níveis de dica**

Tente resolver com a dica 1 antes de ler a dica 2.



---

## ⭐ Problemas Fáceis

---

### Problema 1 — O texto do banner está quase invisível

O subtítulo logo acima do título principal na página inicial mal aparece.
Abra o DevTools (F12) e passe o mouse sobre o elemento para ver qual cor está
sendo aplicada.

**Dica 1 →** Inspecione o elemento `.hero-subtitle` no DevTools e observe o
valor de `color`. Pergunte-se: essa cor tem contraste suficiente sobre o fundo
escuro?

**Dica 2 →** O arquivo responsável é `css/home.css`. Procure pela classe
`.hero-subtitle` e verifique o valor hexadecimal da cor. Compare com as cores
vibrantes já definidas nas variáveis CSS em `css/global.css`.

**Dica 3 →** Há uma variável chamada `--neon-green` disponível no projeto.
Trocar a cor atual por ela (ou por um tom mais claro como `#c0cadf`) já
resolve. Depois da troca, use a aba **Accessibility** do DevTools para
confirmar que o índice de contraste ficou acima de 4.5:1.

---

### Problema 2 — Os campos do formulário estão estranhos de digitar

Abra a página de contato e tente clicar em um campo de texto. Algo no espaçamento
interno parece desproporcional — muito "gordo" em uma direção, muito "apertado" na outra.

**Dica 1 →** Inspecione um `<input>` da página no DevTools e procure pela
propriedade `padding` no painel de estilos.

**Dica 2 →** O CSS desta página está declarado como `<style>` inline dentro do
próprio `contato.html`. Procure a regra que afeta `.form-input`.

**Dica 3 →** A propriedade `padding` aceita dois valores: `vertical horizontal`.
O valor atual tem os dois lados trocados. Um bom ponto de partida é algo
equilibrado como `12px 16px`.

---

### Problema 3 — Os botões do hero estão empilhados

Na seção principal da página inicial, os dois botões aparecem um embaixo do
outro, quando o layout pede que fiquem lado a lado.

**Dica 1 →** Os botões estão dentro de um elemento com a classe `.hero-actions`.
Inspecione-o no DevTools e verifique quais propriedades Flexbox estão ativas.

**Dica 2 →** O arquivo `css/home.css` contém a regra de `.hero-actions`.
Uma única propriedade está com o valor errado.

**Dica 3 →** `flex-direction` controla se os filhos de um flex container ficam
em linha (`row`) ou em coluna (`column`). Qual dos dois você precisa aqui?

---

### Problema 4 — Uma seção usa uma fonte diferente do resto do site

Na seção de notícias da página inicial, os parágrafos de descrição parecem
"de código", com letras espaçadas de forma diferente do restante da página.

**Dica 1 →** Selecione o texto de um card de notícia no DevTools e veja qual
`font-family` está sendo aplicada.

**Dica 2 →** Abra `css/home.css` e procure pela classe `.noticia-desc`.

**Dica 3 →** A solução é **remover** a declaração de `font-family` dali. Quando
uma propriedade não é declarada, o elemento herda o valor do pai — e o `body`
já define a fonte correta para todo o site em `css/global.css`.

---

### Problema 5 — Algo está errado com os avatares dos cards

Os avatares dos cards de personagens na página inicial parecem distorcidos ou
com um comportamento visual incorreto.

**Dica 1 →** Inspecione o elemento `.card-avatar` no DevTools e observe se há
alguma propriedade CSS com um valor incomum ou que não existe na especificação.

**Dica 2 →** Abra `css/home.css` e procure pela regra `.card-avatar`. Leia cada
propriedade com atenção.

**Dica 3 →** A propriedade `object-fit` existe para controlar o ajuste de
**imagens** (`<img>`). Os valores válidos são `cover`, `contain`, `fill`,
`none` e `scale-down`. O valor `stretch` não existe — e como o avatar é uma
`<div>` com emoji, essa propriedade não faz sentido nenhum aqui. Remova-a.

---

## 🔶 Problemas Médios

---

### Problema 6 — O layout de destaques está invertido

Na seção "Destaques da Semana", o card principal (com a descrição longa) está
menor que os cards laterais menores. Parece que as proporções estão ao
contrário do esperado.

**Dica 1 →** Inspecione o elemento `.destaques-grid` no DevTools e observe a
propriedade `grid-template-columns`. As frações estão na ordem certa?

**Dica 2 →** O arquivo `css/home.css` define `.destaques-grid`. O card
principal usa `grid-column: 1` e os laterais usam `grid-column: 2`. Qual
coluna deveria ser maior?

**Dica 3 →** Se a coluna 1 deve ser maior, o valor correto seria algo como
`2fr 1fr` — não `1fr 2fr`. O mesmo raciocínio se aplica ao grid da página
`personagens.html`: o `.personagens-full-grid` usa colunas fixas que não
se adaptam a telas menores. Pesquise sobre `repeat(auto-fill, minmax(...))`.

---

### Problema 7 — Os filtros de personagens não funcionam

Na página de personagens, os botões "Heróis", "Vilões" etc. não fazem nada
ao ser clicados.

**Dica 1 →** Abra o Console do DevTools (aba Console) e adicione um
`console.log` dentro da função `setupFiltros` em `js/cards.js` para ver se
ela está sendo chamada.

**Dica 2 →** A função usa `document.querySelector(...)` para encontrar a barra
de filtros. Compare o seletor usado no JavaScript com o `id` real do elemento
no HTML da página `personagens.html`.

**Dica 3 →** O seletor e o `id` do elemento HTML precisam ser idênticos.
Se o HTML tem `id="filtros-bar"`, o seletor no JS deve ser `'#filtros-bar'`.
Encontre a divergência e alinhe os dois.

---

### Problema 8 — O hover dos botões primários tem comportamento estranho

Ao passar o mouse sobre os botões verdes (`.btn-primary`), a animação parece
incompleta — o `transform` funciona mas as cores não mudam como esperado.

**Dica 1 →** Abra `css/global.css` e procure por `.btn-primary:hover`. Leia com
atenção — há algo estranho na quantidade de vezes que essa regra aparece?

**Dica 2 →** Quando duas regras CSS têm o mesmo seletor, a segunda **sobrescreve**
as propriedades em comum da primeira, mas **não herda** as propriedades únicas.
O resultado é imprevisível e difícil de depurar.

**Dica 3 →** A solução é **unir as duas declarações em uma só**, incluindo
todas as propriedades: `background`, `color`, `border-color`, `box-shadow` e
`transform`. CSS bem escrito nunca repete o mesmo seletor desnecessariamente.

---

### Problema 9 — O espaçamento entre seções é inconsistente

Navegando entre as páginas, algumas seções parecem mais "apertadas" ou mais
"espaçadas" que outras, sem motivo aparente.

**Dica 1 →** Inspecione elementos com a classe `.section` em diferentes páginas
e compare o `padding` calculado pelo DevTools. Ele é igual em todas?

**Dica 2 →** O padding padrão está em `css/global.css` na regra `.section`. Mas
há outras seções no CSS que declaram `padding` diretamente, sobrescrevendo esse
valor. Use a busca do editor (`Ctrl+F`) para encontrar todos os `padding` nos
arquivos CSS.

**Dica 3 →** Uma boa prática é criar uma **variável CSS** para o espaçamento,
por exemplo `--section-padding: 80px 0`, e usar essa variável em qualquer lugar
que precise desse valor. Assim, alterar em um lugar reflete em todo o projeto.
Consulte a documentação de `custom properties` no MDN.

---

### Problema 10 — O modo claro está quebrado (dois sub-problemas)

Ao tentar ativar o tema claro clicando no botão da lua/sol no header, duas coisas
acontecem de forma errada: o botão não responde ao clique, e mesmo que respondesse,
o tema claro apresentaria textos com contraste ruim.

#### Sub-problema A — O botão não funciona

**Dica 1 →** Abra `js/theme.js` e leia a função `setupThemeBtn`. Ela usa
`getElementById(...)` para encontrar o botão. Agora abra qualquer página HTML
e procure o `id` real do botão de tema no `<header>`.

**Dica 2 →** O `id` usado no JavaScript e o `id` no HTML precisam ser
**exatamente iguais** — maiúsculas, hífens e tudo mais. Encontre a diferença.

**Dica 3 →** A mesma função `getElementById` aparece em dois lugares em
`theme.js`. Corrija os dois.

#### Sub-problema B — O tema claro não troca todas as cores

**Dica 1 →** Ative o modo claro (após corrigir o sub-problema A) e inspecione
textos secundários e textos "apagados" da página. Eles estão legíveis sobre o
fundo branco?

**Dica 2 →** Abra `css/global.css` e procure o seletor `body.light-mode`.
Compare a lista de variáveis redefinidas ali com a lista completa de variáveis
de texto em `:root`.

**Dica 3 →** As variáveis `--text-secondary` e `--text-muted` existem em
`:root` para o tema escuro, mas **não foram redefinidas** para o tema claro.
No modo claro, o fundo é quase branco — valores muito escuros ou muito claros
para esses textos vão criar contraste ruim. Adicione-os ao bloco
`body.light-mode` com valores adequados.
