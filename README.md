# 🎮 Jogos da Nina

Coleção de joguinhos HTML feitos para a Nina, uma criança de 6 anos. Tudo em português, sem game over, sem punição, sem anúncio — só reforço positivo. Abre no celular, no tablet ou no PC, sem instalar nada.

**🕹 Jogar agora:** https://leoborja.github.io/jogos-da-nina/

*feito por Marina Vaz 💚*

---

## Os jogos

### 🍉 Magali Melancia — [`/melancia/`](melancia/)

Arraste a Magali pela tela para comer melancias e ganhar pontos. Sem fases, sem game over — diversão infinita.

| Onde | Controle |
|------|----------|
| 📱 Celular | Arraste o dedo pela tela — a Magali segue o toque |
| 💻 PC | Setas do teclado ou WASD (ou arraste com o mouse) |

- Magali oficial que balança ao andar, vira para o lado que se move e dá pulinhos ao comer
- 3 melancias sempre na tela, quicando suavemente
- Ao comer: pedacinhos e sementes voam, aparece "NHAM!"/"DELÍCIA!" e toca um bipe alegre
- Placar que pulsa a cada ponto; celebração especial ⭐ a cada 10 melancias
- Cenário com céu, sol, nuvens passando e gramado

### 🧠 Memória da Turma — [`/memoria/`](memoria/)

Jogo da memória com a Turma da Mônica: Mônica, Magali, Cebolinha, Cascão, Chico Bento, Franjinha, Bidu e Titi.

- **Espiadinha:** todas as cartas aparecem por 1,2s no começo de cada rodada
- **Erro não pune:** as cartas viram de volta com calma, sem som ruim nem perda de ponto
- **Nível sobe sozinho:** 4 pares → 6 pares → 8 pares, sempre depois de vencer
- **Ao acertar:** a carta fica verde, solta ⭐✨💚 e toca um som alegre; o nome do personagem aparece embaixo (ajuda a ler!)
- **Vitória:** confete, "PARABÉNS!" e um troféu salvo no navegador (🏆 N jogos completos)
- Grade se reorganiza sozinha para caber em qualquer tela, em pé ou deitada

## Estrutura

```
index.html          → home: escolhe o jogo
melancia/           → jogo 1 (canvas 2D)
  index.html
  magali.png
memoria/            → jogo 2 (DOM + CSS 3D)
  index.html
  img/*.png         → 8 personagens (fundo transparente)
```

Cada jogo é **um único `index.html`** com CSS e JS inline, zero dependências, e tem um botão 🏠 que volta para a home.

## Rodar local

```bash
cd ~/amagalimelancia
python3 -m http.server 8080
# abrir http://localhost:8080
```

## Deploy

GitHub Pages servindo a branch `main`. Para publicar:

```bash
git add . && git commit -m "mensagem" && git push
# o Pages reconstrói sozinho em ~30-60s
```

## 💡 Ideias de melhorias futuras

**Magali Melancia:**
1. 🥇 Melancia dourada rara — vale 5 pontos, brilha, confete e som especial
2. 🎉 Festa a cada 10 melancias — confete na tela toda e "PARABÉNS!" gigante
3. 🔊 Sons mais divertidos — "nhac nhac" de verdade; arrotinho após 5 seguidas
4. 🏆 Recorde salvo no navegador
5. 🏃 Melancias fugitivas depois de 15 pontos
6. 🐱 Mingau passeando e "roubando" melancias (sem tirar ponto)

**Memória da Turma:**
7. 🔉 Falar o nome do personagem ao achar o par (speechSynthesis)
8. 🎴 Mais personagens (Titi, Marina, Sansão, Floquinho, Franjinha…) para níveis maiores
9. 👯 Modo dois jogadores (ela contra alguém, sem placar competitivo)

**Novos jogos para a coleção:**
10. 🎨 Pintar e Colorir — desenhos de contorno pintados com o dedo
11. 👗 Ateliê da Magali — vestir a Magali com roupas e cenários
12. 🫧 Estourar Bolhas — bolhas sobem e estouram com nota musical
13. 🔢 Contar bichinhos — quantos tem na tela?

**Geral:**
14. 📲 PWA — instalável com ícone na tela inicial, tela cheia e offline

## Créditos

- Personagens © Mauricio de Sousa Produções — projeto de fã, pessoal e sem fins comerciais
- Imagens dos personagens: [Wiki Turma da Mônica](https://turmadamonica.fandom.com/pt-br)
- Feito com [Claude Code](https://claude.com/claude-code)
