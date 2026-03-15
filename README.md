# Phil's Escape

A browser-based platformer game built with vanilla HTML5 Canvas and JavaScript — no dependencies, no build step, just open `index.html` and play.

Phil is a warrior from the Stickman Army who's trying to break free. Fight through two worlds packed with enemies, mini-bosses, and world bosses to earn your freedom.

---

## Play

Open `index.html` in any modern browser (Chrome, Firefox, Safari, Edge).

---

## Controls

| Action | Keys |
|--------|------|
| Move | `A` / `D` or Arrow Keys |
| Jump | `Space`, `W`, or `↑` |
| Sword Attack | `Z` or `J` |
| Shield (hold) | `X`, `K`, or `Shift` |
| Restart | `R` or `Enter` |

Mobile touch controls appear automatically on touch devices.

> **Shield tip:** The shield only blocks arrows coming from the front — face the archer before raising it.

---

## Game Structure

### Worlds

| World | Theme | Levels | Mini Boss (Lvl 5) | World Boss (Lvl 10) |
|-------|-------|--------|-------------------|----------------------|
| 1 | Forest | 10 | Stickman Giant | Green Anaconda |
| 2 | Desert | 10 | Stickman Giant | Giant Robot Lizard |
| — | Final | — | — | Giant Stickman Mech |

### Flags
- 🔵 **Blue flag** — Checkpoint (saves your respawn point, awards +100 score)
- 🔴 **Red flag** — Level finish line

### Enemies
- **Stickman Soldier** — Melee, patrols and chases Phil
- **Stickman Archer** — Ranged, fires arrows that can be blocked with the shield

### Scoring
| Event | Points |
|-------|--------|
| Kill soldier | +100 |
| Kill archer | +150 |
| Checkpoint | +100 |
| Level complete | +500 |
| Boss kills | +800–3000 |

---

## Features

- Procedurally generated levels (seeded per world + level number — consistent every run)
- 5-heart health system with half-heart damage
- Sword + shield dual combat mechanic
- 4 unique boss fights with multi-phase AI
- Parallax scrolling backgrounds
- Web Audio API synthesized sound effects (no audio files)
- Particle system for hits, dust, and deaths
- Mobile touch controls
- High score saved to localStorage

---

## Version History

See [CHANGELOG.md](CHANGELOG.md).

---

## Tech

- Pure HTML5 Canvas + vanilla JavaScript
- No frameworks, no dependencies, no build tools
- Single file (`index.html`) — everything is self-contained
