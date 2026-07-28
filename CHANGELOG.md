# Changelog

## [Unreleased]

### Fixed
- iPad: on-screen controls now appear. Detection was `'ontouchstart' in window`, which iPadOS Safari
  does not expose in its default desktop-class browsing mode, so an iPad was treated as a desktop and
  shown no controls at all — unplayable without a keyboard. Detection now also accepts a coarse
  primary pointer, and the buttons and canvas fall back to pointer events where touch events are
  absent, so "tap to continue" screens can be dismissed by touch too
- Mobile: the move/jump/attack pad no longer covers menus. It is a fixed overlay pinned to the bottom
  of the window, so in landscape — how anyone plays a 16:9 platformer on a phone — it sat on top of
  the world map's info panel and swallowed the taps meant for it. It now only appears during play
- Mobile: world map nodes and title menu rows snap to the nearest target instead of needing an exact
  hit. A map node was a ~18px tap target on a phone, and the title rows sit too close together to pad
  without overlapping each other

### Changed
- The Giant Stickman Mech now fights back. It walks Phil down instead of decelerating to a standstill
  in every state, has 32 health instead of 24, deals a full heart with fists and laser instead of half,
  gained a floor-sweeping beam from half health, and its stomp now throws up a column of force that
  can't be jumped over (the travelling ground waves still can)
  ([#49](https://github.com/natethedrummer/stickman-escape/issues/49))

### Added
- Shrink Ray finale — emptying the Mech's health bar no longer ends the game. It collapses, sheds a
  shrink ray that Phil picks up automatically, then reboots in a third phase. Z fires the ray instead
  of swinging the sword; seven hits shrink the Mech from three times Phil's height down to his size,
  and it gets faster the smaller it gets (though its attacks weaken with it). Once it's tiny it flees
  harmlessly and the sword lands the finishing blow, and only then does the ending play. Boss Rush
  keeps the original one-phase fight ([#49](https://github.com/natethedrummer/stickman-escape/issues/49))
- World map and level select — a canvas-drawn overworld reached from the title screen and the pause
  menu. One world at a time, its levels laid out as stepping stones along a winding path, with Phil
  walking to whichever node you select. Cleared levels show a star, boss levels a skull, locked ones
  a padlock, and the final Mech its own gold node at the end of Berlin. A panel shows the level or
  boss name, whether it's cleared, your best score for it, and whether you've found its secret;
  worlds page with up/down and levels with left/right. Unlocked levels and per-level bests live in
  their own `pe_progress` store, so they survive both NEW GAME and finishing the campaign
  ([#14](https://github.com/natethedrummer/stickman-escape/issues/14))
- Secret bonus levels — one hidden stage per world. A key is stashed on the highest platform of a set
  level in each world; carrying it to the door beside that level's finish flag opens the secret instead
  of the normal exit. Three themes: **The Dark** (a lantern is all Phil can see by), **Low Gravity**
  (gaps too wide to clear under normal gravity), and **The Springs** (every landing throws Phil back
  up a zig-zag climb). Worth 2,500 points the first time and 500 on a replay; finding all six unlocks
  the Golden Phil skin. Progress persists to localStorage, and running out of lives inside a secret
  returns you to the campaign level that hides it
  ([#26](https://github.com/natethedrummer/stickman-escape/issues/26))
- Moving platforms — horizontal sliders, vertical elevators, and platforms that shake and drop away a
  moment after Phil lands on them (restored when he respawns). Anything standing on one is carried
  with it, enemies included, and each kind is marked so it reads as a mover before you jump: travel
  arrows and a rail for sliders and elevators, cracks for the ones that give way. Ramped in by world —
  World 1 stays static, World 2 gets sliders from level 6, World 3 adds elevators, Worlds 4-6 add
  falling platforms ([#31](https://github.com/natethedrummer/stickman-escape/issues/31))
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
