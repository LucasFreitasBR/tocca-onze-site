# Costa Coral Imóveis (protótipo)

Site de imobiliária de praia (marca fictícia) para imóveis no litoral sul de Pernambuco e norte de Alagoas: Porto de Galinhas, Praia dos Carneiros/Tamandaré e Maragogi/Japaratinga. Foco em férias e rentabilidade para quem compra.

## Arquivos
- `index.html`: site inteiro (HTML + CSS + JS) em um arquivo. Rode com `npx serve .` (há `.claude/launch.json`, porta 4173) ou abra direto no navegador. **Mantenha o `<!doctype html>` na primeira linha**: sem ele o navegador entra em modo quirks, o Lenis não consegue rolar a página pela roda do mouse e as âncoras da navbar param de funcionar.
- `assets/`: 10 fotos reais dos destinos, do Wikimedia Commons (CC BY 2.0, CC0 ou domínio público). **Hoje nenhuma é usada no site**: a pedido do cliente, as fotos saíram por enquanto. Ficaram guardadas para voltar depois: `hero-porto.jpg`, `faixa-*.jpg` (Porto, Carneiros, Maragogi, Japaratinga), `destino-*.jpg` (Porto, Carneiros, Maragogi, visão geral) e `processo-praia.jpg`. Título, autor, licença e página de cada uma estão em `assets/credits.json`. Se alguma voltar ao site, o crédito volta ao rodapé.
- Bibliotecas por CDN: GSAP 3.12.5 e Lenis 1.1.13 (scroll suave). Fonte: Gabarito (Google Fonts, variável 400–900), da Naipe Foundry, do Rio de Janeiro, numa família só: 800/900 nos títulos e 400/500 no texto. Foi escolhida por ser moderna e de origem brasileira.
- `.claude/skills/design-taste-frontend`: skill de direção visual (tasteskill v2). `.taste-skill/before` e `.taste-skill/after` guardam as capturas do inspetor antes e depois do redesenho da hero, da navbar e da tipografia.

## O que existe
1. **Menu lateral de capítulos** (`#rail`, a partir de 1280px): fica fixo no canto inferior esquerdo, como o índice do Shopify Editions. Lista os 7 capítulos; o atual fica em negrito e a barra dele enche conforme a seção é rolada (`railUpdate()`). Usa `mix-blend-mode: difference` para ficar legível sobre qualquer fundo. Nessa largura, `.wrap` e o carrossel ganham 210px de margem dos dois lados (`--rail`), e o cabeçalho volta para 32px. Abaixo de 1280px o menu some e fica só o da navbar.
1. **Navbar fixa**: fica presa no topo e compacta com o scroll (variável `--hk` de 0 a 1 escrita em `headUpdate()`: altura de 84 para 62px, some o subtítulo da marca, ganha fundo). O link da seção visível recebe `aria-current` e um ponto coral. Os links (`data-scroll`) chamam `goTo()`, que rola com o Lenis e desconta os 62px da barra; só "Destinos" (`#regioes`) para no topo exato, porque o voo começa ali.
2. **Hero**: cena em parallax com 5 camadas em canvas (colinas, mar animado, jangada, areia, coqueiro) e sol que desce com o scroll. A hero fica presa (`position: sticky`, dentro de `.hero-stage`) e a seção "Por que" sobe por cima dela como uma cortina: a borda são duas ondas (`.why-wave`) que andam com o scroll, e a hero escurece (`#heroDim`) enquanto é coberta. A cena fica parada e só aproxima (zoom de até 8% em `.scene`). Não use translate para cima nem scale menor que 1 nas camadas, porque isso abre brechas na base ou nas bordas. O `body` precisa de `margin:0`, ou aparece uma moldura de 8px em volta da hero. Por cima, um letreiro gigante ("Renda de praia · Porto de Galinhas · Carneiros · Maragogi") corre para a esquerda (`mqUpdate()`, 40 px/s mais um empurrão do scroll). Abaixo dele vêm a frase de apoio, o dado de mercado de Porto de Galinhas, o botão "Calcular minha renda" e o link "Ver empreendimentos".
**Passagens entre seções** (ideia das transições por cena do Shopify Editions Winter '26): entre as seções há palcos presos (`.pass`, 190svh, montados no bloco "Passagens entre seções" do script, a partir de `PASSES` e `PASS_KINDS`; o progresso vem de `passUpdate()`). Em cada um, um elemento de praia atravessa a "câmera", cobre a tela, o título do capítulo e uma linha sobre o que vem (`sub` em `PASSES`) ficam no centro da tela enquanto a cena a cobre, mais ou menos de p = .3 a .8, e um portal circular (`portal()`) se abre na cor da seção seguinte, para a emenda não aparecer. São quatro:
   - **Por que → Destinos:** nuvens e gaivotas sobre o céu (`clouds`).
   - **Destinos → Empreendimentos:** folhas de coqueiro fecham a tela e reabrem (`fronds`).
   - **Empreendimentos → Rentabilidade:** três ondas sobem e terminam na cor `--band` do simulador (`wave`).
   - **Como funciona → Contato:** a vela listrada da jangada atravessa a tela (`sail`).

   Tudo é desenhado em SVG com os tokens de cor e funciona nos modos claro e escuro. As passagens não aparecem sem JS nem com `prefers-reduced-motion`. A ordem do scroll é: hero → Por que → nuvens → Destinos → coqueiros → Empreendimentos → onda → Rentabilidade → Processo → vela → Contato.
3. **Por que o litoral sul** (`#porque`): título e frase grande lado a lado; depois, três argumentos só em texto.
4. **Voo 3D pelo litoral** (`#regioes`): seção alta com `position: sticky`. Um mapa do litoral em canvas inclinado com CSS 3D; a câmera percorre de Maragogi até Porto de Galinhas e depois sobe para a visão geral. Os cards de cada destino "levantam" do mapa. O HUD é um card sólido com o destino, o acesso e o perfil de investimento. Lógica em `flyUpdate()`.
5. **Carrossel de empreendimentos** (`#empreendimentos`): arrastável, com inércia, filtro por destino e inclinação por velocidade. Os cards são chapados (sem sombra) e destacam os números num bloco de tom diferente. O clique abre um modal com preço, área, diária, ocupação, receita anual, rentabilidade e gráfico mensal por tipologia.
6. **Simulador de rentabilidade** (`#simulador`): receita mês a mês com sazonalidade (`SEASON`), custos, retorno do capital e comparação com um rendimento de referência. Começa com diária de R$ 410 e ocupação de 38% (média de 1 quarto em Porto de Galinhas).
7. **Processo** (`#processo`): título fixo à esquerda; à direita, uma lista ordenada sem rótulos "Etapa N", com uma trilha de 4px que se preenche com o scroll.
8. Formulário de contato (demonstração, não envia nada) e modo claro/escuro por `prefers-color-scheme`.

## Dados
Tudo fica em `REGIONS`, `DEVS` e `SEASON`, no início do script.
- **Diária (`d`) e ocupação (`o`)** de cada tipologia são **médias reais de mercado do Airbnb** na cidade, para o mesmo número de quartos, segundo a AirROI (out/2025 a set/2026): Ipojuca para Porto de Galinhas, Tamandaré para Carneiros e Maragogi para Maragogi e Japaratinga. Loft conta como studio e loft duplex como 1 quarto. Maragogi não tem amostra de studio, então o studio do Galés usa a média de 1 quarto. São médias de todos os anúncios, não do imóvel em si.
- **`SEASON`** = RevPAR mensal de Ipojuca dividido pela média do ano (AirROI, set/2025 a ago/2026). Janeiro é o pico (1,90) e maio o mês mais fraco (0,64).
- **Ainda fictícios**: marca, empreendimentos, nomes, preços (`p`), áreas (`a`), datas de entrega, CRECI, WhatsApp e endereço.
- Fórmula: receita anual = soma mensal de `diária × dias × ocupação × peso sazonal`; rentabilidade bruta = receita / preço.

Médias da AirROI por cidade (12 meses até set/2026): Ipojuca R$ 647 de diária e 37,3% de ocupação; Tamandaré R$ 753 e 29,9%; Maragogi R$ 670 e 32,0%.

## Pendências para produção
- Trocar marca, logo, CRECI, WhatsApp, e-mail/endereço e o rodapé ("marca fictícia") pelos dados reais da imobiliária.
- Trocar `DEVS` pelos empreendimentos reais (nome, local, tipo, entrega, preço e área por tipologia). Se houver histórico próprio de diária e ocupação, ele substitui as médias da AirROI. Sem histórico, manter as médias com o rótulo "média da região".
- Substituir as ilustrações SVG dos empreendimentos (função `art()`, nos cards, no mapa e no modal) por fotos reais de cada empreendimento. As fotos de destino atuais não servem para isso.
- O inspetor da skill ainda acusa três pontos. O gradiente do céu e o brilho do sol fazem parte da ilustração da hero e foram mantidos de propósito. Um card do mapa ("Burgalhau · Maragogi") encosta na borda direita em 1440px. Sem as fotos, a página não tem nenhuma imagem real nem vídeo (o inspetor marca `few-pictures`); isso é intencional por enquanto.
- Conectar o formulário (WhatsApp API, CRM ou e-mail).
- Confirmar se a imobiliária oferece gestão de temporada (o site promete isso).
- Revisar os custos do simulador (35% por padrão) com dados reais.
- Avaliar versão em inglês, SEO e meta tags, e hospedagem/domínio.
- Testar desempenho em celulares modestos (canvas do hero e do mapa).

## Estilo
- Respeitar `prefers-reduced-motion` (o letreiro para).
- Cores, fontes e os cinco degraus de tamanho (`--step-small`, `--step-ui`, `--step-body`, `--step-lead`, `--step-sub`, `--step-head`, mais `--step-line` para o letreiro) são tokens CSS no `:root`.
- Títulos em uma só fonte e cor, sem palavra de destaque em itálico ou colorida. Sem rótulos em caixa alta espaçada e sem monoespaçada.
- Botões chapados, em pílula, sem sombra.
- Animações com GSAP e `requestAnimationFrame`, sem CSS transitions para o que depende do scroll.
