# 🍉 Magali Melancia

Jogo HTML simples feito para uma criança de 6 anos: arraste a Magali (Turma da Mônica) pela tela para comer melancias e ganhar pontos. Sem fases, sem game over — só diversão infinita.

**🎮 Jogar agora:** https://leoborja.github.io/magali-melancia/

*feito por Marina Vaz 💚*

---

## Como jogar

| Onde | Controle |
|------|----------|
| 📱 Celular | Arraste o dedo pela tela — a Magali segue o toque |
| 💻 PC | Setas do teclado ou WASD (ou arraste com o mouse) |

Cada melancia comida vale 1 ponto no placar 🍉. A cada 10 pontos aparece uma celebração especial ⭐.

## O que tem no jogo

- **Magali oficial** (imagem clássica com fundo transparente) que balança ao andar, vira para o lado que se move e dá pulinhos de alegria ao comer
- **3 melancias** sempre na tela, em posições aleatórias, quicando suavemente
- **Ao comer:** pedacinhos e sementes voam, aparece "NHAM!"/"DELÍCIA!"/"QUE DOCE!" e toca um bipe alegre (WebAudio)
- **Placar** grande no topo que pulsa a cada ponto
- **Cenário:** céu com gradiente, sol, nuvens passando, gramado
- Dica inicial ("Arraste a Magali até a melancia! 👆") que some no primeiro toque

## Técnico

- **Um único `index.html`** (canvas 2D, JavaScript puro, zero dependências) + `magali.png` (sprite)
- Pointer Events (funciona com dedo e mouse), `touch-action: none` para não rolar a página
- Movimento por interpolação (lerp) — a Magali persegue o alvo com suavidade
- Teclado: setas/WASD movem o alvo ~55px à frente da posição atual (≈440 px/s)
- Som gerado por osciladores WebAudio (contexto criado no primeiro toque/tecla, exigência dos navegadores)
- Canvas com devicePixelRatio (máx. 2x) para ficar nítido em telas retina
- Hook `window.__debug` expõe `{magali, melancias, getScore()}` para testes automatizados

## Rodar local

```bash
cd ~/amagalimelancia
python3 -m http.server 8080
# abrir http://localhost:8080
```

(Também funciona abrindo o `index.html` direto no navegador.)

## Deploy

GitHub Pages servindo a branch `main` deste repositório. Para publicar mudanças:

```bash
git add . && git commit -m "mensagem" && git push
# o Pages reconstrói sozinho em ~30-60s
```

## 💡 Ideias de melhorias futuras

Priorizadas pelo impacto para uma criança de 6 anos:

**Combo recomendado (pouco código, muito impacto):**
1. 🥇 **Melancia dourada rara** — vale 5 pontos, brilha, confete e som especial
2. 🎉 **Festa a cada 10 melancias** — confete na tela toda, Magali dá um pulão, "PARABÉNS!" gigante
3. 🔊 **Sons mais divertidos** — "nhac nhac" mastigando de verdade; arrotinho após 5 seguidas rápidas
4. 🏆 **Recorde salvo** — "Seu recorde: 47 🍉" via localStorage

**Backlog:**
5. 🏃 **Melancias fugitivas** — depois de 15 pontos, algumas fogem devagarinho (perseguição engraçada)
6. 🤰 **Barriguinha** — Magali cresce um tiquinho por alguns segundos a cada melancia
7. 🍰 **Petiscos bônus** — bolo ou sorvete ocasional valendo 2 pontos
8. 🐱 **Mingau** — o gato dela passeia e "rouba" melancias se chegar primeiro (sem tirar ponto)
9. 🌅 **Fundo dinâmico** — dia → pôr do sol → noite com estrelas conforme a pontuação
10. 📲 **PWA** — instalável com ícone na tela inicial, tela cheia e offline

## Créditos

- Personagem Magali © Mauricio de Sousa Produções — projeto de fã, pessoal e sem fins comerciais
- Feito com [Claude Code](https://claude.com/claude-code)
