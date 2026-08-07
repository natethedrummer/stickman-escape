# Changelog

## [Unreleased]

### Added
- Challenge of the Day — one level a day, under **EXTRA MODES**, generated from today's date rather
  than from a random number. That one decision is what makes the mode work with no server and no
  account: two people on the same day get the same level because they computed it, not because
  something told them. The day of the week picks the world, so the week ramps from Forest on Monday
  to Berlin on Sunday and the player can learn the rule; the level inside that world comes from the
  date hash, so which one stays a surprise. Finishing under 2:30 pays a time bonus of 12 points a
  second, because everybody is playing the same level and completion alone would tie every player on
  the same number — the clock is the only thing that separates two finished runs. 40 coins for
  finishing, 60 more every fifth day in a row, and streak, best streak and best score are kept
  ([#63](https://github.com/natethedrummer/stickman-escape/issues/63))
- **The attempt is spent when the run starts, not when it ends.** One try is the whole rule of the
  mode, and if quitting or reloading handed the day back there would be no rule — just a level you
  can grind until it goes well. Because the cost is that real, the row asks for a second press before
  it begins, and GIVE UP ON TODAY says plainly what it does

### Changed
- **EXTRA MODES** rows are keyed by id instead of position. They were a chain of index comparisons,
  and inserting the daily at the top would have silently rewired every action below it — the same
  bug the pause menu was rebuilt to avoid
- The daily's pause menu has no RESTART LEVEL and no WORLD MAP. Restarting would be a second attempt
  at the one level you get today, and the map would launch a campaign level with the attempt live

### Fixed
- Secret keys and doors no longer appear in runs that are not the campaign. Leaving through a secret
  door calls `leaveSecret()` → `nextLevel()`, which advances the campaign — so a door found in an
  **Endless** run (which has been possible since Endless shipped: all six secret levels are in its
  rotation) could move the player's saved campaign position from a mode that is supposed to leave it
  alone. Generation now gates the key and door on `seedOverride==null`, which already meant "not a
  campaign level"
- A death schedules Phil's respawn 1.8 seconds out. Giving up on the daily inside that window ended
  the attempt and wrote the result, and then the timer dropped Phil back into the level — where
  reaching the flag would overwrite a loss with a win. `respawnPhil` now refuses to resurrect a run
  that has already ended

### Added
- Endless mode — levels that keep coming until you run out of lives, under the new **EXTRA MODES**
  row. Depth climbs through the campaign's own world/level pairs rather than pinning one and turning
  a dial, because enemy variety, hazards and moving platforms are all gated on literal world/level
  checks: climbing the pairs is the only thing that actually opens the roster up, and the scenery
  changes for free. Boss levels are skipped entirely. Every depth gets a fresh seed and every run
  gets a fresh one on top, so the same eight layouts never come round again. Past depth 12 the
  enemies also speed up, capped at 1.35x. You get 3 lives for the whole run, health carries between
  depths with one heart back per depth cleared, and best depth and best score are kept
  ([#8](https://github.com/natethedrummer/stickman-escape/issues/8))

### Changed
- **BOSS RUSH** on the title screen became **EXTRA MODES**, holding Boss Rush and Endless together
  with each one's best score. The title menu was at nine rows and out of room, and grouping them
  means the next mode does not need a row of its own either
- Endless pays coins at every fifth depth rather than from kills. An infinite level that drops coins
  is an infinite coin printer, and the shop is meant to be earned in the campaign

### Fixed
- Phil is not a machine. Three lines added with the dialogue and the museum implied Factory 999 had
  built him on an assembly line and that "Unit 47" was a model number — which contradicts the game's
  own premise, where he is a soldier tired of taking orders who walked away, and the intro cutscene
  is about everybody standing in the same line and getting the same orders. The factory now builds
  war machines, Phil served a year guarding its floor, and Foreman-9 calls him **Recruit 47**.
  Caught by Teddy, who noticed it in the Foreman's museum entry

### Added
- Museum — a **MUSEUM** row on the title screen opens a gallery of all 5 enemies and all 12 bosses.
  An entry unlocks the first time you meet that character in the game; until then it is a silhouette
  and a question mark, and a locked boss does not leak its own tagline. Each page shows the
  character drawn with the **game's own art**, idling, alongside its name, what it does, and how many
  of them Phil has beaten — counted across every run, so the soldier tally just keeps climbing.
  Boss pages call the real `drawBoss()` on an object from `spawnBoss`, so an exhibit can never drift
  from what you actually fight ([#20](https://github.com/natethedrummer/stickman-escape/issues/20))

### Added
- Shields — a third slot, sold from a new **SHIELDS** tab. The stock wood shield's rule (it only
  stops what hits Phil's front) is the game's oldest lesson, so each upgrade bends exactly one thing
  about it and pays for it somewhere else. The **Tower shield** (250) blocks arrows from *any* side
  but slows his walk to 66 while it is up. The **Spiked shield** (350) blocks the same as wood and
  deals 1 back to anything that runs into it — once per raise of the guard, so backing off and
  charging again costs another. The **Magic shield** (500) carries 3 charges a life that swallow any
  one hit whole, refilled on every respawn and shown as pips beside the hearts
  ([#21](https://github.com/natethedrummer/stickman-escape/issues/21))

### Changed
- The tutorial's shield lesson and its summary now read differently if you are wearing a tower
  shield, because "it only stops what hits his front" is not true of that one, and a tutorial that
  teaches something false about your own kit is worse than no tutorial
- The shield drill always uses the basic wood shield. A tower shield would let you stand still and
  pass without turning once, which is the only thing that drill measures

### Added
- Cloaks — six outfits Phil can wear, sold from a new **CLOAKS** tab in the shop. The Ninja wrap
  with its trailing scarf, the Army kit he defected from, a Tuxedo, and a Glow suit that glows and
  does nothing else are pure decoration. The **Kung Fu Gi** is not: it takes the weapon slot over
  entirely, replacing whatever you bought with punches and kicks — short reach, a fast 0.26s swing,
  and a **flying kick that hits for 2** if you land it in mid-air. That is the trade for 600 coins,
  and it is why swapping weapons is refused while the gi is on. A cloak is a layer over the Boss
  Rush skins rather than a replacement, so Golden Phil can wear a tuxedo
  ([#5](https://github.com/natethedrummer/stickman-escape/issues/5),
  [#10](https://github.com/natethedrummer/stickman-escape/issues/10))

### Changed
- The weapon shop is now just the **SHOP**, with WEAPONS and CLOAKS tabs. Navigation runs in three
  bands — tabs, cards, back — so a tap and the keyboard reach exactly the same things
- Shop saves made before cloaks existed load unchanged, with no cloak owned and none worn

### Added
- Training drills — four timed arenas, each about one skill, on a new **PRACTICE** screen that also
  hosts the tutorial. **Sword Timing**: one target lights at a time, hit it and the next lights.
  **Shield Drill**: two archers, and your score is blocks minus arrows that got through — standing
  still facing one way scores exactly zero. **Parkour Run**: a floating course with no floor, timed,
  and falling costs you the climb back with the clock running. **Survival**: waves that tighten from
  every 2.4s to every 1.0s and harden from soldiers to archers, bombers and heavies. Each drill
  keeps your best score and pays 50 coins the first time you pass it — once ever, so a drill can't
  be farmed. Campaign progress, score, lives and the save are parked while a drill runs, and no
  coins drop from drill kills ([#11](https://github.com/natethedrummer/stickman-escape/issues/11))

### Changed
- The title screen's HOW TO PLAY row became **PRACTICE**, which opens the tutorial and the four
  drills together. The menu was already at eight rows and out of vertical space, and it groups them
  honestly: the tutorial teaches, the drills score

### Added
- Dialogue — characters now speak over the live level in a box along the bottom, with a portrait of
  whoever is talking, their name, and the line typed out a character at a time. Press or tap to go
  on, **Esc** to skip. It fills the moments the comic cutscenes never covered: **Phil reacting to
  each new world** as he arrives in it, and **Phil's line after every boss goes down**. The five
  level-5 mini bosses — the only bosses with no story of their own — now taunt him as the fight
  opens, and he answers back. The level stays on screen behind the box, which is the whole point:
  the comic panels are for the big story beats, dialogue is for a character saying one thing
  without leaving the place you are standing in. Portraits are drawn for Phil and each speaking
  boss, with a tinted fallback so a new speaker never renders blank. Lines play once per run and
  come back on a new game, and never interrupt Boss Rush or the tutorial
  ([#25](https://github.com/natethedrummer/stickman-escape/issues/25))

### Fixed
- Quitting to the title while a defeated boss was still exploding fired the level-clear screen three
  seconds later anyway, dropping you into a cleared level you had already left

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
