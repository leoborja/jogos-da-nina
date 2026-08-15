# Jogos da Nina — notas de desenvolvimento

Coleção de joguinhos HTML para a irmã de 6 anos do Leo (a Nina). Ver README.md para descrição de cada jogo e backlog.

## Estrutura

```
index.html          → home (escolhe o jogo) — cartões grandes, sem JS
melancia/index.html → jogo 1: Magali Melancia (canvas 2D)
melancia/magali.png → sprite oficial da Magali (180x270 RGBA)
memoria/index.html  → jogo 2: Memória da Turma (DOM + CSS 3D flip)
memoria/img/*.png   → 8 personagens em 260px de altura, fundo transparente
```

Cada jogo é autocontido num `index.html` (CSS + JS inline, zero dependências) e tem botão 🏠 para `../`.

## Regras do projeto

- **Público-alvo = criança de 6 anos:** nunca adicionar game over, punição ou dificuldade frustrante. Só reforço positivo.
- **Tudo em português** (textos do jogo, commits podem ser pt).
- Cada jogo fica **num único index.html** — facilita compartilhar e abrir offline.
- Crédito **"feito por Marina Vaz 💚"** deve permanecer visível em toda página.
- Jogo novo = pasta nova + cartão novo na home (`index.html`) + seção no README.
- Testar local em **http://localhost:8080** antes de publicar (`python3 -m http.server 8080`).

## Imagens dos personagens

Vêm da wiki (fandom), com fundo transparente. Para achar a imagem principal de qualquer personagem:

```bash
curl -s -G "https://monica.fandom.com/pt-br/api.php" \
  --data-urlencode "titles=Mônica|Cebolinha|Cascão|Magali|Chico Bento|Franjinha|Bidu|Titi" \
  -d "action=query&prop=pageimages&piprop=original&format=json"
```

Baixar com `curl -L -A "Mozilla/5.0"` e reduzir com `sips -Z 260 *.png` (mantém ~50KB cada).

## Deploy

- Repo: `leoborja/jogos-da-nina` (GitHub, público) — `gh` já autenticado como `leoborja`
- URL: https://leoborja.github.io/jogos-da-nina/ (GitHub Pages, branch `main`, path `/`)
- O repo antigo era `magali-melancia`. O GitHub redireciona a URL do *repositório*, mas **não**
  a do Pages: `leoborja.github.io/magali-melancia/` dá 404. Se alguém tiver o link antigo salvo,
  a solução é criar um repo `magali-melancia` só com um `index.html` de redirect.
- Pasta local ainda se chama `~/amagalimelancia` (não renomeada para não quebrar nada).
- Publicar = commit + push; rebuild leva ~30-60s. Verificar com:
  `curl -s https://leoborja.github.io/jogos-da-nina/ | grep <string-nova>`

## Como testar sem abrir o navegador

**Importante:** o Chrome headless tem largura mínima de ~500px de viewport. Screenshot direto em
`--window-size=390,844` renderiza o layout a 500px e **corta** a imagem — parece overflow mas não é.
Para testar largura real de celular, use um **iframe** de 360px dentro de uma janela grande:

```bash
cat > _teste.html <<'EOF'
<body style="margin:0;background:#333;display:flex;gap:10px;padding:10px">
<iframe src="/memoria/" style="width:360px;height:760px;border:0;background:#fff"></iframe>
</body>
EOF
("/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu \
  --no-sandbox --user-data-dir=/tmp/cp-$$ --no-first-run --window-size=1300,820 \
  --virtual-time-budget=6000 --screenshot=/tmp/shot.png "http://localhost:8080/_teste.html" &)
sleep 12   # o Chrome trava se rodar em foreground neste ambiente — rodar em background e dormir
rm _teste.html
```

O harness de iframe também dá acesso a `iframe.contentWindow.__debug` para dirigir o jogo por script.

Hooks de debug expostos por cada jogo:
- melancia: `window.__debug = {magali, melancias, getScore()}` — mexer em `magali.tx/ty`
- memoria: `window.__debug = {cartas, getPares(), getNivel(), clicar(i), resolver()}` — `resolver()` ganha o jogo sozinho

## Decisões de design já tomadas

**Melancia:**
- Movimento por lerp (`dt*8`) — a Magali "persegue" o dedo em vez de teleportar
- Teclado (setas/WASD) move o alvo 55px à frente ⇒ ~440 px/s, independente de FPS
- DPR limitado a 2 por performance; melancia nasce a 200px+ da Magali

**Memória:**
- Grade escolhida por JS: testa só divisores de N (grade sempre cheia) e pega a coluna que
  deixa a carta maior; recalcula no resize/orientationchange. Carta com aspect-ratio 1/1.3, máx 170px
- Espiadinha de 1,2s no início; erro só vira de volta em 900ms (som suave, sem punição)
- Nível: `min(2 + nivel*2, 8)` pares — 4 → 6 → 8 → 8…
- Troféus acumulados em `localStorage['nina_memoria_trofeus']`
- Nome do personagem aparece na carta virada (pratica leitura)

**Geral:**
- AudioContext criado apenas em gesto do usuário (pointerdown/keydown), senão o navegador bloqueia
- CSS: cuidado com especificidade — `.nome span` sobrescreve `.botao`; usar `.nome .botao`
