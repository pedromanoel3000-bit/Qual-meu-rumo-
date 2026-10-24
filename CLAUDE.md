# Qual é o meu rumo?

Teste gratuito de orientação profissional, em português do Brasil, pensado para o celular. Foi criado pelo Pedro para ajudar a namorada a escolher um caminho profissional e depois virou um site público.

- Site no ar: https://pedromanoel3000-bit.github.io/Qual-meu-rumo-/
- Responda e escreva sempre em português do Brasil, de forma direta.
- O Pedro prefere aprovar cada parte antes de seguir para a próxima.
- Linguagem neutra: o texto não marca o gênero de quem faz o teste. Prefira substantivos e frases sem concordância (“Como podemos te chamar?”, “alguém da área”, “pessoas conhecidas”). Os nomes dos tipos RIASEC concordam com “perfil” (Investigativo, Artístico, Empreendedor).

## Como o site funciona

Tudo fica em `index.html`: HTML, CSS e JavaScript, sem framework e sem etapa de build. Não há servidor. As respostas ficam só no aparelho de quem faz o teste (`localStorage`, chave `rumo-v1`).

Fluxo: abertura → 4 etapas → resultado.

1. **Interesses** (`ITEMS`): 36 atividades, 6 de cada tipo RIASEC (modelo de Holland), escala de 0 a 3.
2. **Escolhas** (`PAIRS`): 12 dilemas "isso ou aquilo". Cada lado tem uma chave de preferência; os nomes ficam em `DIMS` e os opostos em `OPP`.
3. **Realidade** (`REAL`): trabalho, renda, horas, dinheiro para investir, estudo, faculdade e prazo.
4. **Reflexão** (`REFL`): 6 perguntas abertas e opcionais.

Resultado: código RIASEC (3 letras), barras por tipo, preferências escolhidas, caminhos ordenados por afinidade (os 6 primeiros em cartão completo; os outros numa lista compacta que abre ao tocar), plano de 30 dias, placar de decisão (`CRIT`, notas de 1 a 5), perguntas para conversar (`ASK`), imagem para compartilhar, botão de WhatsApp, botão para copiar o resumo e botão de PDF.

### Pontuação dos caminhos (`scorePaths`)

São 51 caminhos em `PATHS`. Cada um tem:
- `rx`: peso de cada tipo RIASEC (0 a 3);
- `pre` / `con`: preferências que combinam ou conflitam;
- `ent` (curto, medio, longo), `inv` (investimento de 0 a 3), `fac` (exige faculdade), `rem` (remoto de 0 a 2), `renda` (fixa, variavel, ambas);
- `teste` e `passo`: o experimento de 30 dias e o primeiro passo.

Ao criar um caminho: use só chaves que existem em `DIMS`, nunca coloque em `pre` duas preferências opostas (veja `OPP`) e use um `id` novo, porque os ids ficam salvos no aparelho (`picks`, `sc`, `done`). Adicionar caminhos não apaga as respostas salvas.

Total = 60% interesses + 30% preferências + 10% fixo − penalidades de realidade (faculdade indesejada, prazo curto, falta de dinheiro, renda variável). A afinidade exibida fica entre 18% e 97%.

Atenção: se mudar a quantidade de itens em `ITEMS`, `PAIRS` ou `REFL`, as respostas salvas no aparelho das pessoas são zeradas (veja `init`). Para mudanças grandes, troque a chave `rumo-v1`.

### Compartilhar e PDF

- **Imagem** (`drawShare`): 1080×1920, formato dos stories e do status do WhatsApp. É desenhada em canvas no próprio aparelho, com as cores do tema claro fixas em `SC`/`SC_T`. Mostra nome (se houver), código, barras e os 3 primeiros caminhos; nunca as reflexões nem a realidade. O endereço no rodapé vem da tag `canonical`. A imagem é criada assim que o resultado aparece (`prepShare`), para o toque em "Compartilhar" abrir o menu do celular na hora. Onde não há menu de compartilhar (computador), o botão baixa o PNG. O compartilhamento manda só o arquivo, sem texto: no iPhone, texto junto esconde o Instagram do menu.
- **PDF**: botão "Salvar em PDF ou imprimir" chama `window.print()`. O CSS de `@media print` força o tema claro, esconde botões e a lista compacta, e `openForPrint` abre o "Como testar" dos 6 primeiros caminhos.

## Visual

- Tema "caderneta de campo": curvas de nível desenhadas em canvas (`drawTopo`), trilha de 5 marcos no topo e bandeira no resultado.
- As cores ficam em variáveis no `:root`, com modo claro e escuro (`prefers-color-scheme` e `data-theme`).
- Fontes hospedadas no próprio site, em `fonts/`: Big Shoulders Display (títulos), Atkinson Hyperlegible (texto) e IBM Plex Mono (rótulos). Só o subconjunto latino.
- Precisa funcionar bem com 390px de largura, sem rolagem lateral.

## Outros arquivos

- `og.png` (1200×630): prévia de links no WhatsApp e nas redes. O código-fonte está em `tools/og.html`. Para gerar de novo, tire uma captura de tela de 1200×630 desse arquivo, por exemplo com o Playwright.
- `favicon.svg`, `apple-touch-icon.png`, `icon-192.png`, `icon-512.png` e `site.webmanifest`: ícones e instalação na tela inicial.
- `.nojekyll`: faz o GitHub Pages servir os arquivos como estão.

## Publicar

- GitHub Pages publica a partir do branch **`pascoa`** (o principal deste repositório), pasta raiz. Um push no `pascoa` atualiza o site em 1 a 2 minutos.
- O endereço completo aparece em `index.html` nas tags `canonical`, `og:url` e `og:image`. Se o repositório for renomeado ou ganhar domínio próprio, atualize as três. Para domínio próprio, crie também um arquivo `CNAME` com o domínio.

## Pendências conhecidas

- Não há contador de visitas nem coleta de resultados. Se forem adicionados, avise as pessoas sobre os dados (LGPD).
