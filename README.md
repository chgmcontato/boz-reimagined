# BOZ Reimagined

**Call of Duty: Black Ops Zombies** (Android 1.0.11) adaptado para **Android TV**: jogado inteiro com controle,
sem HUD de toque, com o layout de botões do Black Ops 1 de console e menus redesenhados no estilo quadrinhos do jogo.

<p align="center"><img src="screenshots/01-titulo.png" width="80%" alt="Tela de título Reimagined"></p>

> Projeto de fã, sem fins lucrativos e sem ligação com a Activision. **Este repositório não contém o jogo**:
> nenhum APK, OBB, binário, textura, fonte ou música originais. Para compilar, é preciso ter uma cópia
> própria do APK e do OBB.

## Capturas (TV real, 1280×720)

| Pause com dados da partida (Kino der Toten) | Pause com o cenário do mapa (Call of the Dead) |
|---|---|
| ![Pause no Kino](screenshots/02-pause-kino.png) | ![Pause no CoTD](screenshots/03-pause-cotd.png) |
| **Dead Ops Arcade com timer de partida** | **Player Profile com nome e TIME PLAYED** |
| ![Dead Ops](screenshots/04-deadops-hud.png) | ![Player Profile](screenshots/05-player-profile.png) |

## O que muda

**Controle (padrão Xbox)**
- Analógicos para mover e mirar, gatilhos para atirar e mirar (a mira de segurar virou alternar), X para recarregar.
- Granadas e táticos como no BO1: direcional direito para claymore e LB para o outro tático (Monkey Bomb, Gersh, Matryoshka).
- Todos os menus navegáveis por D-pad, com foco em cantoneiras e transições sem aparecer o SWF original.

**Menus e interface**
- Título "Reimagined", menu principal, seleção de personagem e mapa, Single Player, New Game, Quit, Save & Quit,
  Pause e Game Over redesenhados (camada Java por cima do jogo).
- Editor de nome pelo controle; o nome aparece no Player Profile, no pause e no lobby do co-op.
- Pause com jogador, mapa, round e estatísticas, e Start também pausa o Dead Ops.
- Timer de partida com a fonte do HUD (liga/desliga no pause) e TIME PLAYED no Player Profile.
- COD Points, More Apps e compras removidos.

**Jogabilidade (patches no binário)**
- Ondas e drops no ritmo do BO1: ciclo de drops sem repetição, Carpenter só com janelas quebradas,
  Death Machine só com energia ligada ou depois de alguém ter tido Quick Revive, DG-2 só ao matar o George.
- Dead Ops Arcade liberado de saída; morte vai direto ao Game Over.
- Limite de 30 FPS estável e contador de FPS.
- Correções: zumbi preso morre depois de 15 s parado e o jogo não fecha mais por um som já liberado.

## Estado

| Recurso | Situação |
|---|---|
| Controle, menus, transições, nome do jogador | ✅ testado na TV |
| Pause (dados, mapa, Dead Ops), timer, TIME PLAYED, Save & Quit → Continue | ✅ testado na TV |
| Death Machine, DG-2, táticos no LB e HUD | ✅ testado na TV |
| Co-op **Local Wi-Fi** (lobby, nome do jogador, seleção de mapa) | 🟡 implementado, lobby navegável, **partida com dois aparelhos ainda não testada** |
| Zumbi preso, correção do som, analógicos ao sair do pause | 🟡 implementado, falta confirmar na TV |
| Ondas/drops BO1, 30 FPS, tutorial, prompts "[A] to …" | 🟡 implementado, teste completo pendente |
| Play Online | ❌ servidores oficiais desligados |
| Mapa Moon | ⏸️ pausado (crash ao carregar a No Man's Land) |

Testado numa TV TCL com Android 11 (armeabi-v7a, 1280×720, 60 Hz) e um 8BitDo Ultimate no modo Xbox.

## Como compilar

Requisitos: Linux, JDK 17, Android build-tools (`d8`, `zipalign`, `apksigner`), apktool, Python 3 e ImageMagick.

1. Coloque o APK original em `build/base.apk` e extraia os assets dos menus com os scripts de `menus-edit/`
   e `build/character-menu/` (eles leem o seu OBB `blackops_etc.dz`).
2. Crie uma keystore própria em `build/boz-tv.jks` (ou ajuste `build/build.sh`).
3. `bash build/build.sh` gera `BOZ-TV.apk`.
4. Instale **sem modo incremental** (ele já apagou os saves numa TV):
   ```bash
   adb install --no-incremental -r -g BOZ-TV.apk
   ```
   O OBB vai em `/storage/emulated/0/Android/obb/com.activision.boz/`.

## Como funciona

- O jogo usa a engine Marmalade e é um binário fechado (`boz.s3e`, ARM/Thumb). As mudanças de jogabilidade são
  **patches binários** verificados byte a byte (`build/patch_s3e.py`, `patch_pickups.py` e outros), com código novo
  numa área livre da imagem.
- Controle, menus, pause e timer são uma **camada Java** (`build/java/com/carlos/boz/`) que reconhece cada tela
  pelos pixels e desenha por cima, sem mexer no SWF dos menus.
- Ferramentas de engenharia reversa (Ghidra, Frida, formato dos `.group.bin`) ficam em `build/re/`, `build/frida/` e `maps/`.
- O [`CLAUDE.md`](CLAUDE.md) é o diário técnico completo: offsets, armadilhas e decisões.

## Próximas atualizações

O foco é melhorar a experiência de quem joga na TV:

1. Testar o co-op Local Wi-Fi com dois aparelhos (host, cliente e nome do P2).
2. Confirmar na TV as correções recentes (zumbi preso, som, analógicos após o pause).
3. Redesenhar as telas que faltam: About, More Info, subtelas de Extras e Help.
4. Deixar a leitura de memória do HUD Java mais robusta.
5. Traduzir a interface (hoje em inglês).

Bugs e sugestões: abra uma *issue* dizendo o mapa, o round e o que aconteceu (um print ajuda muito).
