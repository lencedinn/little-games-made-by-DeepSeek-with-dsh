# Three Little Games · 三个小游戏

Three tiny browser games, each a **single self-contained HTML file** — no build step, no dependencies, no external assets. All art is drawn at runtime on `<canvas>` (or in the DOM), all music and sound effects are synthesised live with the Web Audio API. There is not a single image, font, or audio file in this repository.

Each game has a **中文 / EN** toggle in the top-right corner. It picks your browser language on first load and remembers your choice.

| Game | Genre | Session length |
|---|---|---|
| [NEON DODGE](neon-dodge/index.html) | Real-time arcade · dodge + graze combo | 40 s – 2 min |
| [LUMEN](lumen/index.html) | Turn-based puzzle · route the light beam | 30 s – 3 min per level |
| [DICE DELVE](dice-delve/index.html) | Turn-based roguelike · dice allocation | 4 – 6 min |

**Run it:** double-click any `index.html`, or serve the folder from any static server. Nothing to install.

---

## 1. NEON DODGE

Top-down neon arcade. Blocks rain down; you survive by *grazing* them — shaving past a block's edge builds a combo multiplier that breaks 3.4 s after your last graze.

- **Move** — drag with mouse or finger (1:1 horizontal, the ship lives in a narrow band at the bottom)
- **Dash** — `Space` or the on-screen DASH button. Invulnerable mid-dash; dash into a block to smash it
- **Pickups** — green/score, gold/big score, pink/shield, blue/slow-motion

3 ships × 3 difficulties, an elite every 3 waves (Guard / Hunter, each with two phases), 6 achievements, a local leaderboard and C/B/A/S rankings.

Keys: `P` pause · `M` mute · `S` shake · `F` fps · `L` perf mode · `Enter` fullscreen

---

## 2. LUMEN

A turn-based puzzle. Rotate mirrors to route the beam from the source into every core. No timers, no failure states — only move count.

Click a mirror to rotate it 45°; the beam redraws immediately. Clear the level by lighting **all** cores.

| Element | Effect | From level |
|---|---|---|
| Mirror | reflects 90° | 1 |
| Prism | passes straight through **and** splits off a beam at 90° | 6 |
| Portal | enters one end, leaves the other in the same direction | 9 |
| Locked mirror | cannot rotate — route around it | 12 |
| Second core | both cores must be lit | 6 |

- **3 hand-authored tutorial levels** (5 / 8 / 11), one move each, each teaching a new element
- **Endless procedural levels** — boards capped at 12×8, difficulty ladder from PAR 2 up to 4–6
- **Daily challenge** — seeded by the date, so everyone gets the same puzzle; one-tap copy of your result
- Every level's **PAR is the exact minimum**, found by exhaustively trying all 2ⁿ mirror orientations. The `H` hint solves from your current position in real time.

`Z` undo · `R` restart · `H` hint · `Esc` close panel

---

## 3. DICE DELVE

A turn-based dice-allocation roguelike. Each turn you roll 5 dice and assign them to three actions:

| Action | Effect |
|---|---|
| ⚔ Attack | sum of the dice = damage |
| 🛡 Block | sum of the dice = block against this turn's incoming hit |
| ✦ Charge | sum feeds the charge bar; at 15 it auto-bursts for 22 damage + 12 block |

**Enemy intents are public** — you always see what's coming, so the whole decision is how much to block versus how hard to hit. Five floors, nine fights, and a three-way reward after each win (max HP, an extra die, attack/defence/charge efficiency, or a build-defining relic like *Thorn Mail*, *Head Start* or *Combo*).

- Click a die, then click a zone to assign it; click an assigned die to take it back
- **Reroll** once per turn, and only on dice you haven't assigned yet
- `Space` / `Enter` end the turn
- Progress auto-saves after every turn; the title screen offers **Continue**

Balance isn't guesswork: a greedy-policy bot plays 300 simulated runs and the numbers are tuned to a win rate around 55–60 % (a human lands lower). Average run: ~23 turns.

---

## How these were built and checked

All three are plain JavaScript in one file each — no framework, no bundler, no minification.

Core logic was verified headlessly in Node with minimal DOM/canvas stubs:

- **NEON DODGE** — a ~5,000-frame smoke test covering start, dash, pause, death, results and restart
- **LUMEN** — a 100-level audit: independently re-enumerates PAR, checks that every mirror's drawn orientation matches the orientation used by the beam, replays the optimal solution to confirm it wins, and verifies that locked mirrors actually sit on the light path
- **DICE DELVE** — 300 simulated runs for balance, plus a save/restore test that reloads the page-equivalent state and a strict-DOM check that every element the code touches really exists

That last check earned its keep: an earlier build referenced a removed element id, and the render function threw silently before it ever reached the dice — the HUD drew, the dice didn't. The permissive stub hid it; the strict one caught it immediately.

## Licence

Personal project, no licence specified. Play it, fork it, learn from it.
