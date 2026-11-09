# tocca onze · Investimentos imobiliários (protótipo)

Site da **tocca onze, investimentos imobiliários**, para imóveis no litoral sul de Pernambuco e norte de Alagoas: Porto de Galinhas, Praia dos Carneiros/Tamandaré e Maragogi/Japaratinga. Foco em férias e rentabilidade para quem compra.

## Arquivos
- `index.html`: site inteiro (HTML + CSS + JS) em um arquivo. Rode com `npx serve .` (há `.claude/launch.json`, porta 4173) ou abra direto no navegador. **Mantenha o `<!doctype html>` na primeira linha**: sem ele o navegador entra em modo quirks, o Lenis não consegue rolar a página pela roda do mouse e as âncoras da navbar param de funcionar.
- `assets/`: 10 fotos reais dos destinos, do Wikimedia Commons (CC BY 2.0, CC0 ou domínio público). **Hoje nenhuma é usada no site**: a pedido do cliente, as fotos saíram por enquanto. Ficaram guardadas para voltar depois: `hero-porto.jpg`, `faixa-*.jpg` (Porto, Carneiros, Maragogi, Japaratinga), `destino-*.jpg` (Porto, Carneiros, Maragogi, visão geral) e `processo-praia.jpg`. Título, autor, licença e página de cada uma estão em `assets/credits.json`. Se alguma voltar ao site, o crédito volta ao rodapé.
- Bibliotecas por CDN: GSAP 3.12.5 e Lenis 1.1.13 (scroll suave). Fonte: Gabarito (Google Fonts, variável 400–900), da Naipe Foundry, do Rio de Janeiro, numa família só: 800/900 nos títulos e 400/500 no texto. Foi escolhida por ser moderna e de origem brasileira.
- `assets/brand/`: logo da marca em PNG branco com fundo transparente. `tocca-onze-nome.png` é o nome com "INVESTIMENTOS IMOBILIÁRIOS" e `tocca-onze-marca.png` é o símbolo: um T num círculo, com a haste formada por duas barras (o "11", o onze do nome). No CSS os dois viram máscara (`mask: url(...)`) e ganham a cor do token, então funcionam nos dois temas.
- `versoes/v1-costa-coral/` (marca Costa Coral), `versoes/v2-tocca-onze-teal-laranja/` (tocca onze em teal, laranja e branco) e `versoes/v3-color-hunt/` (paleta 91c6bc · 4b9da9 · f6f3c2 · e37434): cópias das versões anteriores, guardadas como base. Não editar.
- `.claude/skills/design-taste-frontend`: skill de direção visual (tasteskill v2). `.taste-skill/before` e `.taste-skill/after` guardam as capturas do inspetor antes e depois do redesenho da hero, da navbar e da tipografia.

## Repositório e deploy
- Git na branch `main`, repositório privado no GitHub da conta LucasFreitasBR (`tocca-onze-site`). O `.gitignore` deixa de fora `.taste-skill/`, a cópia local da skill (`.agents/` e `.claude/skills/`) e `dist/`. O `skills-lock.json` registra a origem da skill para reinstalar.
- Netlify: o `netlify.toml` copia só `index.html` e `assets/` para `dist/` e publica essa pasta. Assim `CLAUDE.md`, `versoes/` e os arquivos de trabalho não ficam acessíveis no endereço público. Cada push na `main` atualiza o site.

## O que existe
**Paleta "Maré baixa"** (água rasa `#a8e0d8`, teal `#2a9d9f`, areia `#f4e6cc`, coral `#f07a4a`, em cerca de 41 / 26 / 18 / 15%). As cores fixas ficam em `--p-agua`, `--p-teal`, `--p-areia` e `--p-coral`, mais `--p-deep` (`#0a2629`) só para texto. Ele é um pouco mais escuro que o `#0e2f33` sugerido, para o texto sobre o teal passar de 4,5:1.
- **Água rasa (~41%)**: fundo da página (`--bg`), "Por que", passagens e fundo dos campos.
- **Teal (~26%)**: céu da hero, Destinos (`.fly` em `--band`), simulador e telas de verbo.
- **Areia (~18%)**: cards, HUD, modal, cartão do contato, areia e sol.
- **Coral (~15%)**: botões, painel do "Por que", tela "Escolher", vela e guarda-sol.

Texto sobre teal e coral é sempre `--p-deep`. Texto em teal usa `--sea-text` (`#17646a`). O símbolo da marca fica em tinta. O modo escuro usa `--p-deep` como fundo e a areia como tinta. O inspetor marca o coral como "terracota" e a dupla areia e coral como "creme com terracota". É a paleta escolhida pelo cliente, e a água e o teal dominam, então rode com `--allow palette-terracotta,palette-claude`.

Direção da taste-skill: "O lugar carrega a página" (seed 161212063, `.taste-skill/direction.json`), adaptada para ilustração, porque o cliente pediu o site sem fotos reais por enquanto.

1. **Menu lateral de capítulos** (`#rail`, a partir de 1280px): fica fixo no canto inferior esquerdo, como o índice do Shopify Editions. Lista os 7 capítulos; o atual fica em negrito e a barra dele enche conforme a seção é rolada (`railUpdate()`). Usa `mix-blend-mode: difference` para ficar legível sobre qualquer fundo. Nessa largura, `.wrap` e o carrossel ganham 210px de margem dos dois lados (`--rail`), e o cabeçalho volta para 32px. Abaixo de 1280px o menu some e fica só o da navbar.
1. **Navbar fixa**: fica presa no topo e compacta com o scroll (variável `--hk` de 0 a 1 escrita em `headUpdate()`: altura de 84 para 62px, some o subtítulo da marca, ganha fundo). A marca no canto esquerdo é o símbolo em teal mais o nome em tinta. O link da seção visível recebe `aria-current` e um ponto coral. Os links (`data-scroll`) chamam `goTo()`, que rola com o Lenis e desconta os 62px da barra; só "Destinos" (`#regioes`) para no topo exato, porque o voo começa ali.
2. **Hero**: cena em parallax com 5 camadas em canvas (colinas, mar animado, jangada, areia, coqueiro) e sol que desce com o scroll. A hero fica presa (`position: sticky`, dentro de `.hero-stage`) e a seção "Por que" sobe por cima dela como uma cortina: a borda são duas ondas (`.why-wave`) que andam com o scroll, e a hero escurece (`#heroDim`) enquanto é coberta. A cena fica parada e só aproxima (zoom de até 8% em `.scene`). Não use translate para cima nem scale menor que 1 nas camadas, porque isso abre brechas na base ou nas bordas. O `body` precisa de `margin:0`, ou aparece uma moldura de 8px em volta da hero. O título `h1` vai de canto a canto: "Renda" no alto à esquerda e "de praia" embaixo à direita, sobre a areia. Por isso o horizonte fica em 60% da altura, a areia começa em 74% e o guarda-sol foi para a esquerda. No meio, a partir de 30vw, ficam a frase sobre a tocca onze, o dado de mercado de Porto de Galinhas, o botão "Calcular minha renda" e o link "Ver empreendimentos". O texto sobe junto com a página (`-s`) para a onda nunca passar por cima dele. Na entrada, cada linha do título sobe de trás da própria base. (O antigo letreiro corrido saiu; `mqUpdate()` ficou e não faz nada sem ele.)
**Passagens entre seções** (ideia das transições por cena do Shopify Editions Winter '26): entre as seções há palcos presos (`.pass`, 190svh, montados no bloco "Passagens entre seções" do script, a partir de `PASSES` e `PASS_KINDS`; o progresso vem de `passUpdate()`). Em cada um, um elemento de praia atravessa a "câmera", cobre a tela, o título do capítulo e uma linha sobre o que vem (`sub` em `PASSES`) ficam no centro da tela enquanto a cena a cobre, mais ou menos de p = .3 a .8, e um portal circular (`portal()`) se abre na cor da seção seguinte, para a emenda não aparecer. São quatro:
   - **Por que → Destinos:** nuvens e gaivotas sobre o céu (`clouds`).
   - **Destinos → Empreendimentos:** folhas de coqueiro fecham a tela e reabrem (`fronds`).
   - **Empreendimentos → Rentabilidade:** três ondas sobem e terminam na cor `--band` do simulador (`wave`).
   - **Como funciona → Contato:** a vela listrada da jangada atravessa a tela (`sail`).

   Tudo é desenhado em SVG com os tokens de cor e funciona nos modos claro e escuro. As passagens não aparecem sem JS nem com `prefers-reduced-motion`. A ordem do scroll é: hero → Por que → nuvens → Destinos → coqueiros → Empreendimentos → onda → Rentabilidade → Processo → vela → Contato.
3. **Por que o litoral sul** (`#porque`): o título grande fica à esquerda e a segunda linha entra num painel laranja (`.why-panel`), que encosta na borda direita a partir de 46vw e vai até o fim da seção. Dentro do painel, o símbolo da marca aparece grande e cortado, deslizando com o scroll (`whyUpdate()`). À esquerda vêm a frase e os três argumentos em linhas. No celular, o painel vira um bloco de 75% da largura no fim da seção.
4. **Voo 3D pelo litoral** (`#regioes`): seção alta com `position: sticky`. Um mapa do litoral em canvas inclinado com CSS 3D; a câmera percorre de Maragogi até Porto de Galinhas e depois sobe para a visão geral. Os cards de cada destino "levantam" do mapa. O HUD é um card sólido com o destino, o acesso e o perfil de investimento. Lógica em `flyUpdate()`.
5. **Carrossel de empreendimentos** (`#empreendimentos`): arrastável, com inércia, filtro por destino e inclinação por velocidade. Os cards são chapados (sem sombra) e destacam os números num bloco de tom diferente. O clique abre um modal com preço, área, diária, ocupação, receita anual, rentabilidade e gráfico mensal por tipologia.
6. **Simulador de rentabilidade** (`#simulador`): receita mês a mês com sazonalidade (`SEASON`), custos, retorno do capital e comparação com um rendimento de referência. Começa com diária de R$ 410 e ocupação de 38% (média de 1 quarto em Porto de Galinhas).
7. **Como funciona** (`#processo`): cinco telas de cor chapada, uma por verbo (Conversar, Escolher, Visitar, Assinar, Alugar), alternando teal, laranja, teal profundo, verde-água e branco. Cada tela tem o verbo gigante segurando o centro (`procUpdate()` move o verbo mais devagar que a tela), o símbolo da marca em tom sobre tom e a explicação no canto inferior direito.
8. **Contato** (`#contato`): uma cena de praia desenhada em SVG ocupa a tela, com um cartão branco sólido no canto superior direito. O cartão tem o formulário (nome, WhatsApp, destino, faixa, mensagem; é só demonstração e não envia nada) e o WhatsApp de exemplo. No celular, o cartão sobe 80px sobre a cena.
9. **Rodapé**: fundo teal profundo, com quatro colunas em cima (marca, navegação, destinos e dados de mercado) e o nome tocca onze ocupando a largura toda embaixo, com a linha legal.
10. **Modo claro e escuro**: sem escolha salva, o site segue o tema do aparelho (`prefers-color-scheme`). O botão de sol e lua na navbar (`#themeBtn`) grava `data-theme` no `<html>` e a escolha em `localStorage` (`tocca-theme`). Um script curto no `<head>` aplica essa escolha antes da pintura, para a página não abrir no tema errado e trocar. As cores do escuro respondem tanto à media query quanto a `:root[data-theme="dark"]`, e um MutationObserver redesenha as camadas em canvas quando o tema muda.

## Dados
Tudo fica em `REGIONS`, `DEVS` e `SEASON`, no início do script.
- **Diária (`d`) e ocupação (`o`)** de cada tipologia são **médias reais de mercado do Airbnb** na cidade, para o mesmo número de quartos, segundo a AirROI (out/2025 a set/2026): Ipojuca para Porto de Galinhas, Tamandaré para Carneiros e Maragogi para Maragogi e Japaratinga. Loft conta como studio e loft duplex como 1 quarto. Maragogi não tem amostra de studio, então o studio do Galés usa a média de 1 quarto. São médias de todos os anúncios, não do imóvel em si.
- **`SEASON`** = RevPAR mensal de Ipojuca dividido pela média do ano (AirROI, set/2025 a ago/2026). Janeiro é o pico (1,90) e maio o mês mais fraco (0,64).
- **Real**: a marca tocca onze e o logo.
- **Ainda fictícios**: empreendimentos, nomes, preços (`p`), áreas (`a`), datas de entrega, CRECI, WhatsApp e endereço.
- Fórmula: receita anual = soma mensal de `diária × dias × ocupação × peso sazonal`; rentabilidade bruta = receita / preço.

Médias da AirROI por cidade (12 meses até set/2026): Ipojuca R$ 647 de diária e 37,3% de ocupação; Tamandaré R$ 753 e 29,9%; Maragogi R$ 670 e 32,0%.

## Pendências para produção
- Trocar CRECI ("a informar" no rodapé), WhatsApp, e-mail e endereço pelos dados reais da tocca onze.
- Trocar `DEVS` pelos empreendimentos reais (nome, local, tipo, entrega, preço e área por tipologia). Se houver histórico próprio de diária e ocupação, ele substitui as médias da AirROI. Sem histórico, manter as médias com o rótulo "média da região".
- Substituir as ilustrações SVG dos empreendimentos (função `art()`, nos cards, no mapa e no modal) por fotos reais de cada empreendimento. As fotos de destino atuais não servem para isso.
- Sem as fotos, a página não tem nenhuma imagem real nem vídeo (o inspetor marca `few-pictures`); isso é intencional por enquanto.
- Conectar o formulário (WhatsApp API, CRM ou e-mail).
- Confirmar se a imobiliária oferece gestão de temporada (o site promete isso).
- Revisar os custos do simulador (35% por padrão) com dados reais.
- Avaliar versão em inglês, SEO e meta tags, e hospedagem/domínio.
- Testar desempenho em celulares modestos (canvas do hero e do mapa).

## Estilo
- Respeitar `prefers-reduced-motion`.
- Cores, fontes e os cinco degraus de tamanho (`--step-small`, `--step-ui`, `--step-body`, `--step-lead`, `--step-sub`, `--step-head`, mais `--step-line` para o letreiro) são tokens CSS no `:root`.
- Títulos em uma só fonte e cor, sem palavra de destaque em itálico ou colorida. Sem rótulos em caixa alta espaçada e sem monoespaçada.
- Botões chapados, em pílula, sem sombra.
- Ilustração em cor chapada, sem gradientes: o céu da hero são quatro faixas (`.sky i`), o sol é um círculo sólido com um halo sólido mais claro e as nuvens são formas sólidas, na mesma linguagem das passagens.
- Sem sombras em repouso. Ficam só no que flutua (menu aberto, diálogo, aviso) e nos estados (anel de foco, pulso do simulador), cada linha marcada com `taste-skill-ignore` e o motivo; o lint da skill aceita essas linhas.
- Sete tamanhos de letra a 1440px: 14, 18, 26, 36, 60, 104 (`--step-xl`) e 187 (`--step-line`, título da hero e verbos).
- Margem da página no token `--page-m` (16, 32 ou 210px, conforme a largura). O carrossel é recortado nessa margem, e o card totalmente fora do recorte fica com `visibility:hidden` (`carUpdate()`). No celular, cada card ocupa a largura útil.
- Gate da skill antes de publicar: `node .claude/skills/design-taste-frontend/scripts/lint.mjs index.html` (0 falhas) e `inspect.mjs index.html --out .taste-skill/after` com `--allow palette-terracotta,palette-claude` (a única falha aceita é `few-pictures`, enquanto o site estiver sem fotos).
- Animações com GSAP e `requestAnimationFrame`, sem CSS transitions para o que depende do scroll.
