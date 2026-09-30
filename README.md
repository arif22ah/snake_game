# Dynamic Snake 3

A feature-rich take on the classic Snake game, written in Python with Pygame. Everything is generated in code: graphics, sound effects and music. There are no external asset files.

## Highlights

- **Realistic snake**: smooth spline body, scale patterns, swallowed-food bulges that travel down the body, a flicking tongue, blinking eyes that follow food, and a smoothly turning head.
- **Living world**: day/night cycle with real lighting (the snake carries a light), themed weather, fireflies at night, soft shadows, textured ground, rocks and brick walls.
- **Creatures and hazards**: mice that run away (big points), patrolling spiders, and ticking bombs that clear rocks and kill spiders.
- **Portals** that teleport you across the board.
- **Quests**: three live missions at any time, each paying a bonus when completed.
- **Six game modes**, three difficulties, four themes and eight snake skins.
- **Power-ups**: Slow, Shield, Ghost, Double points, Shrink, Magnet, Freeze and the risky Dizzy mushroom.
- **Procedural music** that gets more intense as you level up, plus synthesized sound effects.
- **Polish**: 3-2-1 countdown, slow-motion death sequence, screen shake, combo system, boost mechanic and "close call" bonuses.
- **Persistence**: best scores, top-5 lists, settings, lifetime stats and 16 achievements.

## Requirements

- Python 3.8+
- [Pygame](https://www.pygame.org/) (`pip install pygame`)

## Installation and Running

```bash
pip install pygame
python dynamic_snake_v3.py
```

If there is no audio device, the game still runs; sound is disabled automatically.

## Menu Controls

| Key | Action |
|---|---|
| Up / Down | Select a menu item |
| Left / Right | Change the selected option |
| Enter | Confirm |

### Menu options

| Option | Choices |
|---|---|
| Mode | Classic, Wrap, Rival, Time Attack, Maze, Chaos |
| Difficulty | Easy, Normal, Hard |
| Theme | Neon, Forest, Sunset, Mono |
| Skin | Viper, Coral, Python, Cyber, Rainbow, Ice, Lava, Theme |
| Audio | Full, SFX only, Music only, Off |

The menu also has **Achievements**, **Records & Stats** and **How to Play** screens.

## In-Game Controls

| Key | Action |
|---|---|
| Arrow keys / WASD | Steer |
| SPACE / SHIFT | Hold to boost (drains energy, recharges when released) |
| P | Pause / resume |
| R | Restart (while paused) |
| Q | Show / hide quests |
| M | Cycle audio mode |
| F11 | Toggle fullscreen |
| ESC | Back to menu |

## Game Modes

| Mode | Description |
|---|---|
| **Classic** | Walls are deadly. Rocks and spiders appear as you level up. |
| **Wrap** | The edges wrap around. Just don't bite yourself. |
| **Rival** | An AI snake steals your food. Defeat it for +100 points. |
| **Time Attack** | 90 seconds on the clock; every apple adds 1.5 seconds. |
| **Maze** | A new maze layout every level, plus portals. |
| **Chaos** | Random events: earthquakes, apple rain, night, speed surges, dizzy winds, bomb drops and spider swarms. |

## Gameplay Guide

### Scoring
- Eat apples to grow. Chain them within 3 seconds for a combo of up to **x5**.
- Every **5 apples** you level up: the game speeds up and new obstacles appear.
- Points are multiplied by your level, combo and the x2 power-up.

### Pickups

| Item | Effect |
|---|---|
| Apple | Points and growth |
| Gold star | Big bonus, fades quickly |
| Slow potion | Slows the game |
| Shield | Absorbs a hit (stacks up to 2) |
| Ghost | Pass through obstacles and yourself |
| x2 | Double points |
| Scissors | Cuts up to 3 segments |
| Magnet | Pulls nearby food toward you |
| Freeze | Freezes the rival, mice and spiders; frozen spiders can be eaten |
| Dizzy mushroom | Reverses your controls, so avoid it |

### Creatures and hazards
- **Mice** flee from you. Catch them for +40 (times your multipliers).
- **Spiders** patrol the board and strike when you are adjacent. Freeze them to eat them, or blow them up.
- **Bombs** tick down and explode, destroying rocks and spiders nearby. Watch your own distance.
- **Portals** teleport you between two points on the board.

### Boosting and close calls
Hold SPACE or SHIFT to move faster. Brushing past rocks, spiders or bombs while boosting awards **close call** bonus points.

### Quests
Three quests are active at any time (eat apples, reach a length, catch mice, collect power-ups, make close calls, and more). Completing one pays a bonus scaled by your level, and a new quest replaces it.

### Day and night
A day/night cycle changes the lighting. At night your snake carries a light, and special items, bombs and portals glow.

## Achievements

There are 16 to unlock, including *First Bite*, *Long Boy*, *Speed Demon*, *Combo King*, *Bounce Back*, *Rival Slayer*, *Beat the Clock*, *Mouse Hunter*, *Spider Bane*, *Wormhole*, *Quest Master*, *Night Owl* and *Marathon*. View them from the main menu.

## Save Data

Progress is stored in `snake_save.json`, created next to the script. It contains:

- Best score per mode/difficulty and top-5 lists
- Lifetime stats (games, apples, mice, spiders, quests, portals, longest snake, play time)
- Unlocked achievements
- Your last-used settings (mode, difficulty, theme, skin, audio)

Delete this file to reset all progress.

## Project Structure

The whole game lives in a single file, `dynamic_snake_v3.py`:

| Section | Contents |
|---|---|
| Constants and data tables | Grid size, modes, difficulty tuning, themes, skins, achievements, quest templates |
| Maze layouts | Hand-designed wall layouts for Maze mode |
| Helpers | Colour math, glow and light sprites, spline smoothing (`chaikin`) and resampling |
| Audio | `Synth` (tone/sweep/noise generator), `Sound` (effects), `Music` (background-threaded procedural loops with crossfade) |
| `Storage` | JSON persistence |
| Game objects | `Snake`, `Food`, `Mouse`, `Spider`, `Bomb` |
| Sprite drawing | Pickup, mouse, spider and bomb renderers |
| `Game` | State machine, spawning, quests, rival AI (BFS pathfinding), chaos events, rendering, input and main loop |

## Configuration

Gameplay is tuned through constants at the top of the file:

- `CELL`, `COLS`, `ROWS`: board size and cell pixel size
- `BASE_INTERVAL`, `MIN_INTERVAL`: snake speed per difficulty
- `BOOST_FACTOR`: speed multiplier when boosting
- `TIME_ATTACK_MS`, `APPLE_BONUS_MS`: Time Attack timing
- `DAY_LENGTH_MS`: length of one full day/night cycle
- `SPIDER_CAP`, `SPIDER_INTERVAL`, `MOUSE_INTERVAL`: creature behaviour

Themes (`THEMES`), skins (`SKINS`), achievements (`ACHIEVEMENTS`) and quest types (`QUEST_TYPES`) are plain dictionaries/lists and easy to extend.

## Troubleshooting

- **No sound**: the game needs a working audio device; otherwise it runs silently. Try cycling audio with `M`.
- **Menu music delayed**: music loops are synthesized in a background thread on first launch and start as soon as they are ready.
- **Window too large or small**: the window is resizable and scales automatically; press `F11` for fullscreen.
- **Performance**: the night lighting and smooth body rendering are the most demanding parts. Playing on a lower-power machine may benefit from a smaller `COLS`/`ROWS`.
