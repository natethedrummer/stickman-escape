# Changelog

## [Unreleased]

### Fixed
- Phone and iPad players can pause. There was no pause button on the touch pad and the pause menu
  had no tap handling at all, so `Escape`/`P` was the only way in — meaning on a touch device the
  menu was simply unreachable, and with it the weapon shop, restart, the world map, and the
  tutorial's skip. There is now a **❚❚** button above the movement pad (smaller and dimmer than the
  action buttons, so it is never caught mid-jump), and every row of the menu can be tapped. The
  rows are centred and all six pad buttons sit hard against the left and right edges, so no button
  covers a row in any screen size or orientation. The pause menu's actions now live in one place
  shared by the keyboard and by taps, so the two can't drift apart

### Added
- Tutorial — an eight-step **HOW TO PLAY** stage that teaches the controls before World 1: walking,
  jumping, crossing a gap, attacking, the five hearts and what half a heart means, blocking arrows
  (and specifically that the shield only stops the ones hitting Phil's **front**), the blue
  checkpoint flag and the red finish flag. Each step states what to do, shows the keys — or the
  on-screen buttons on a phone — and waits until you have actually done it. It is a hand-built stage
  rather than an overlay on World 1 Level 1, because campaign levels are generated from a seed and
  cannot promise a gap here and one soldier there. Nothing in it can be failed: falling costs no
  lives and puts Phil back inside the step he was on, and the two steps that ask for a skill let go
  on their own after a while rather than trapping anyone. It plays automatically the first time
  anyone starts a new game, can be skipped from the pause menu, and is on the title screen to replay
  any time. Campaign progress, score, lives and the save file are all parked while it runs
  ([#13](https://github.com/natethedrummer/stickman-escape/issues/13))

### Fixed
- Finish flags and checkpoints floated a pole's length off the ground and could not be reached by
  walking into them — you had to jump. `genLevel` passed the flag's ground line as `GROUND_Y-80`,
  but the draw call and the hit box each subtract the 80px pole height themselves, so it came off
  twice. Every campaign level is affected; secret levels always passed `GROUND_Y` and were correct

### Added
- Weapon shop — beaten enemies now drop coins that fly to Phil and bank instantly, and levels,
  bosses and secrets pay out on top. Spend them on three new weapons: the **HAMMER** (200) swings
  slow but hits for 3 and smashes straight through a Heavy Soldier's raised shield, which nothing
  else in the game does; the **BOOMERANG** (350) is thrown, flies 230px through walls, and damages
  everything it touches on the way out *and* on the way back; the **BAZOOKA** (500) fires a rocket
  that explodes for 2 damage across a whole crowd, ignores shields, and knocks enemy arrows and
  bombs out of the air, with 6 rockets a life that refill every time Phil respawns. The sword is
  free, always owned, and unchanged. Swap weapons any time with **C** (or 1-4, or by tapping the
  weapon panel on touch) — the shop is on the title screen and in the pause menu, and what you
  bought survives NEW GAME, dying, and finishing the campaign
  ([#4](https://github.com/natethedrummer/stickman-escape/issues/4),
  [#12](https://github.com/natethedrummer/stickman-escape/issues/12))

### Fixed
- The pause menu's actions were a chain of hardcoded index comparisons, so inserting a row rewired
  every action below it. They are keyed by id now
- The Mech hands back a full load of rockets when it starts fleeing. It is harmless in that state,
  so a player who arrived with an empty bazooka could neither shoot it nor die to reload

### Added
- Secret level select cheat — type `odlaw` (or ▲ ⚔ ▲ ⚔ on touch) during play to open a menu listing
  all six secret levels, and jump straight into any of them without hunting for that world's key.
  The stage looks and sounds like the world it belongs to wherever you jumped in from, and finishing
  one returns you to the level you were on rather than advancing your progress. Cheated secrets score
  500 and deliberately **don't** count towards Golden Phil, which still has to be earned by finding
  all six keys ([#51](https://github.com/natethedrummer/stickman-escape/issues/51))

### Fixed
- The sword button had no cheat callback, so no touch code could ever use it
  ([#51](https://github.com/natethedrummer/stickman-escape/issues/51))

### Added
- Every boss has its own music — 12 themes, one per boss type, each sitting in or near its host
  world's key so entering the arena is a change of intensity rather than a change of place. Tempos run
  108-168bpm across all four waveforms. The Giant Stickman Mech gets its own theme plus a faster,
  higher variant for the shrink finale, since that phase is a different fight. Boss Rush now plays 12
  different themes instead of the same loop twelve times. Unrecognised boss types fall back to the
  original shared theme, so a new boss is playable before its music is written
  ([#50](https://github.com/natethedrummer/stickman-escape/issues/50))

### Fixed
- The pre-boss story cutscenes played the title-screen jingle under a boss taunting Phil. They now
  carry that boss's theme; cutscenes with no boss (the intro and ending) keep the menu track
  ([#50](https://github.com/natethedrummer/stickman-escape/issues/50))

### Added
- Background music — nine looping tracks synthesised with Web Audio oscillators, no asset files. One
  per world with its own key, tempo and voice (ambient Forest, tense Desert, sparse Snow Peaks,
  industrial Factory 999, slow dark Mines, driving Berlin), plus a boss battle theme, an eerie theme
  for the secret stages, and a menu jingle. Tracks switch with the game state — boss fights and secret
  areas take over, and the game-over screen falls silent. Music is on its own gain node so muting it
  leaves the sound effects alone, and the setting persists. Toggle with **M** anywhere, or from the
  new MUSIC row on the title screen and in the pause menu
  ([#15](https://github.com/natethedrummer/stickman-escape/issues/15))

### Fixed
- iPad: the Web Audio setup no longer risks taking input down with it. `AudioContext ||
  webkitAudioContext` throws a ReferenceError rather than falling back when the unprefixed name is
  missing, and because initAC runs first inside the keydown handler that throw would also skip the
  keypress — leaving an older iPad unable to play at all. The lookup goes through `window` now, and
  the game runs silently if Web Audio is unavailable
- The music drums share one pre-generated noise buffer instead of regenerating identical samples
  several times a second, with a random read offset so hits still vary
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
