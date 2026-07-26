# Changelog

## [Unreleased]

### Added
- Collectible power-ups — 2–3 per level on floating platforms, seeded so a level always rolls the
  same set. Invincibility star (5s, blocks even saws and lava, with a gold aura distinct from the
  spawn-invincibility flicker), speed boost (8s, faster run and higher jump with motion streaks) and
  score ×2 (10s). Each shows a draining timer bar in the HUD, and picking one up again refreshes its
  duration instead of stacking. Enemy heart drops now stay on the ground when Phil is at full health
  ([#32](https://github.com/natethedrummer/stickman-escape/issues/32))

### Added
- Saved campaign progress — auto-saves world, level, score, lives and seen cutscenes at the start of
  every level, with a CONTINUE option on the title screen. Versioned save format; malformed or
  older-version saves are rejected rather than partially loaded. NEW GAME requires a second
  confirmation before wiping an existing save, and finishing the campaign clears it
  ([#28](https://github.com/natethedrummer/stickman-escape/issues/28))

### Changed
- Running out of lives now restarts the level you died on with 3 fresh lives and your score intact,
  instead of throwing you back to World 1 Level 1

### Added
- Boss Rush mode — a shuffled run through all 11 world and mini bosses with the Giant Stickman Mech
  always last. Health carries between fights (1.5 hearts healed per boss), bosses scale up in health and
  speed as the run goes on, and clearing 4 / 8 / 12 bosses unlocks the Mario, Sonica and Minion skins
  for Phil. Selectable from a new title screen menu; unlocks and best score persist to localStorage
- Title screen menu (Start Game / Boss Rush / Skin) with keyboard and tap support

### Fixed
- Bone Revenant went permanently invisible and invincible when a split half was hit — the reformed
  boss spawned 4px inside the floor and was ejected out of the arena ([#47](https://github.com/natethedrummer/stickman-escape/issues/47))
- `moveCollide` no longer ejects an already-embedded entity sideways out of the level; pre-existing
  platform overlaps are left to the vertical pass, which lifts the entity out instead

### Added (previously)
- Story cutscenes drawn as canvas comic panels — an intro, one before each world boss, and an ending
  sequence after the Giant Stickman Mech. Speech, thought and shout bubbles with typewriter text;
  advance by click, tap or key; skippable with Escape or the on-screen SKIP button
- Heavy Soldier enemy with a frontal shield block, 4-hit health pool, and a 50% health-drop chance
- Bomb Thrower enemy that lobs destroyable arcing bombs with a small 1-heart blast radius
- Stickman Glider enemy with patrol/chase/dive behavior and jump-attack-only sword vulnerability
- Procedural unlock rules and score values for the expanded standard enemy roster

## [v1.0.0] — 2026-03-14

Initial release.

### Features
- Phil the stickman warrior with sword and shield combat
- 5-heart health system (half-heart damage per hit)
- Shield blocks frontal arrows from archers
- Two worlds: Forest (World 1) and Desert (World 2), 10 levels each
- Procedurally generated levels via seeded RNG
- Blue checkpoint flags (mid-level) and red finish flags (level end)
- Enemy types: Stickman Soldier (melee) and Stickman Archer (ranged)
- 4 boss fights: Stickman Giant (mini), Green Anaconda, Giant Robot Lizard, Giant Stickman Mech (final)
- Multi-phase boss AI with unique attack patterns per boss
- Parallax scrolling backgrounds themed per world
- Web Audio API synthesized sound effects
- Particle system (hits, dust, score popups, death bursts)
- Mobile touch controls
- High score saved to localStorage
