# Magali Melancia — notas de desenvolvimento

Jogo HTML para a irmã de 6 anos do Leo. Ver README.md para descrição completa e backlog de melhorias.

## Estrutura

- `index.html` — jogo inteiro (CSS + JS inline, canvas 2D, zero dependências)
- `magali.png` — sprite oficial da Magali (180x270, RGBA, veio de `~/Downloads/Magali.png` escolhida pelo Leo)
- Existe uma alternativa em alta resolução (792x885, Magali segurando melancia) no wiki:
  `https://static.wikia.nocookie.net/monica/images/4/4f/Magali_Lima.png/revision/latest?cb=20260420233335&path-prefix=pt-br&format=png`

## Regras do projeto

- **Público-alvo = criança de 6 anos:** nunca adicionar game over, punição ou dificuldade frustrante. Só reforço positivo.
- **Tudo em português** (textos do jogo, commits podem ser pt).
- Manter o jogo **num único index.html** — facilita compartilhar e abrir offline.
- Crédito **"feito por Marina Vaz 💚"** deve permanecer visível no rodapé.
- Testar local em **http://localhost:8080** antes de publicar (`python3 -m http.server 8080`).

## Deploy

- Repo: `leoborja/magali-melancia` (GitHub, público) — `gh` já autenticado como `leoborja`
- URL: https://leoborja.github.io/magali-melancia/ (GitHub Pages, branch `main`, path `/`, build "legacy")
- Publicar = commit + push; rebuild leva ~30-60s. Verificar com:
  `curl -s https://leoborja.github.io/magali-melancia/ | grep <string-nova>`

## Como testar sem abrir o navegador

Screenshot headless (tamanho iPhone):

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu \
  --user-data-dir=/tmp/chrome-prof-$RANDOM --no-first-run --window-size=390,844 \
  --screenshot=/tmp/shot.png --timeout=8000 "http://localhost:8080/index.html"
```

Teste de gameplay: o jogo expõe `window.__debug = {magali, melancias, getScore()}`.
Criar um HTML temporário com iframe apontando pro jogo e manipular `__debug.magali.tx/ty`
(ou despachar `KeyboardEvent` no `contentWindow`) até `getScore()` subir — depois screenshot
e apagar o HTML de teste. Funcionou bem com `--virtual-time-budget=15000`.

Cuidados observados:
- `--virtual-time-budget` e dois Chromes headless simultâneos podem conflitar ("allocator multiple times") — usar `--user-data-dir` único e `--timeout`.
- Texto fora do iframe pode renderizar defasado com virtual time; confiar no placar de dentro do jogo.

## Decisões de design já tomadas

- Movimento por lerp (`dt*8`) — suave, a Magali "persegue" o dedo em vez de teleportar
- Teclado (setas/WASD) move o alvo 55px à frente ⇒ ~440 px/s, independente de FPS
- AudioContext criado apenas em gesto do usuário (pointerdown/keydown), senão o navegador bloqueia
- DPR limitado a 2 por performance
- Melancia nasce longe da Magali (mín. 200px) para não dar ponto de graça
