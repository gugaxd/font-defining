# font defining

Calcula o tamanho de tipo de cada nível — h1, h2, h3, corpo e legenda — a partir do formato
da peça, da distância de leitura e da quantidade de texto que cada nível carrega. Exporta
CSS, JSON, SVG e PNG. Tudo roda no navegador.

Faz parte da família do [gri.d.maker](https://github.com/gugaxd/gri.d.maker) e abre a partir
do hub `design tools`. O botão de grade no cabeçalho volta para o menu (`HUB_URL` no topo de
`src/FontSizer.jsx`).

## Como o tamanho sai

Nada é tabela fixa: cada número vem de uma restrição real, e a ferramenta mostra qual delas
mandou.

**Corpo.** Dois caminhos, escolhidos em _corpo puxado pela_:

- _medida_ — o corpo cresce até a coluna entregar os caracteres por linha pedidos
  (`corpo = coluna ÷ (cpl × largura média do caractere)`);
- _leitura_ — o corpo fica no mínimo legível da distância informada e a linha estica
  até 95 caracteres.

O mínimo legível vem do ângulo visual: a altura de caixa alta precisa ocupar cerca de 0,36°
no campo de visão. A 40 cm isso dá ~10 pt; a 1,5 m, ~38 pt. Abaixo disso a ferramenta não
deixa o corpo descer, e avisa quando a coluna fica estreita demais para a distância.

**Títulos.** O que sobra da área de texto depois do corpo e da legenda é o orçamento dos
títulos. Cada um é encaixado na caixa por busca binária, com quebra de linha real medida na
fonte escolhida — por isso a **quantidade de palavras muda o tamanho**: um h1 de três
palavras cabe muito maior que um de doze no mesmo cartaz. Em _linhas do h1: auto_, testa de
1 a 4 linhas e fica com a que permite o maior corpo.

**Escala.** A razão automática é a que liga o h1 ao menor nível ativo em passos iguais
(`razão = (h1 ÷ menor)^(1/degraus)`). Dá para fixar numa razão clássica — 1,2 terça menor
até 1,618 áurea — e aí cada nível encolhe se não couber na própria caixa. O arredondamento
(livre, 0,5, 1 ou 2) acontece na unidade de saída, então o número da tela é o mesmo do export.

**Tela.** Pixel não tem tamanho físico. Para peças de tela, _largura física_ diz em que
tamanho a peça é vista — celular 70 mm, monitor 340 mm, projeção 2400 mm — e é isso que liga
o pixel à distância de leitura. Um slide projetado num auditório pede corpo muito maior que
o mesmo arquivo visto no notebook.

## Presets

Formatos impressos de A6 a A0, cartaz, banner, flyer DL, carta e cartão; formatos de tela
para Instagram, story, celular, slide 16:9 e 4:3, web, thumbnail e painel. Cada um já traz
a distância de leitura típica e, na tela, a largura física presumida — tudo editável depois.

## Rodando localmente

```bash
npm install
npm run dev
```

## Publicando

Vercel, sem configuração (Vite, build `npm run build`, saída `dist`). Depois do deploy,
atualize a `url` do font defining em `hub/src/ferramentas.js`.
