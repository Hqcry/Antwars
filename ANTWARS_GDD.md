# ANTWARS — GAME DESIGN DOCUMENT (PRE-ALPHA v0.1)

**Working title:** ANTWARS: BELOW THE SURFACE  
**Genre:** Multiplayer underground mining + unpredictable PvP + light extraction + persistent meta-progression  
**Target:** Roblox on mobile and desktop first.  
**Status:** A design hypothesis to validate with actual player tests, **not** a researched/verified retention forecast.  
**One-liner:** Mine blindly through a shared underground world. Every strike may bring treasure — or break through into another player's tunnel. Take your haul to extraction before you lose it.

**FOUNDER CONSTRAINT: ONE ROUND, ONE CHAMPION.** Extraction is an optional early cash-out, not an automatic win. If only one active, living, non-extracted player remains, that player becomes champion (unless a debug solo match). At time limit / no active player, use deterministic server ranking: PvP defeats, then total banked+carried ore, then time survived, then unique player ID. Exactly one champion is shown, and ties cannot create two. All thresholds and scoring are testable, configurable **design hypotheses**; a simpler pure last-man-standing mode is a valid post-test alternative.


## 1. Pillars, promise, boundaries

### 1.1 Three pillars
1. **Mystery behind every block:** Mining changes physical navigable space. Hidden ore, vaults, and players create tension and surprise. Never show all information or instantly point players to combat.
2. **Greed versus safety:** A player decides when to leave with loot. Risk is meaningful but not punishing enough to erase the first day's progress.
3. **Player-made stories:** Shared tunnels, audible digging and short PvP encounters create spontaneous situations that can be described in one exciting sentence.

### 1.2 The emotional journey
- **Curiosity:** “What is behind this wall?”
- **Progress:** “My tools are better; I see a new vein.”
- **Unease:** “Wait, I can hear someone digging.”
- **Panic or bravery:** “The wall broke open and there is a real player!”
- **Relief:** “I barely got out with my crystals.”
- **Desire to retry:** “Next run, I will take a better route.”

### 1.3 What this game is not
Not a passive auto-clicker; not a mining tycoon; not an open-world sandbox with no win condition; not a random loot-gambling casino; not a game whose main progression requires paying Robux; not a pure competitive arena shooter; not real insects/anthill management. The term AntWars refers metaphorically to underground tunnel competition.

## 2. Gameplay loop at three time scales

**Moment-to-moment, ~2–15 s:** look at block → mine it (hit feedback) → reveal adjacent block/small pocket → gain resource or information → move forward / change direction. Listen for clues.

**One run, target ~8–12 min:** spawn with safe starter kit → uncover low-tier ore → choose riskier paths → get optional cache / conflict with player → decide whether to extract → bank resources or salvage on death → result/quick requeue.

**Across sessions:** bank Salvage currency → upgrade basic mining efficiency/carry capacity with diminishing returns → unlock cosmetic helmets/lantern colors/tunnel banners → discover varied biome conditions (later) → pursue account milestones/season challenges (later).

### 2.1 Basic round states
### 2.0A Champion and voluntary escape rules

- **Championship is the competition:** One server-announced champion per genuine multiplayer round. Being the last living **active** contestant earns the win; players who extracted earlier keep their loot but exit the championship. Players who die are eliminated.
- If all players leave active status by death or evacuation, or if timer ends with multiple survivors, winner determined by **PvP defeated count**, then **total ore banked+carried**, then **survival time**, then **unique ID**. This is a fallback scoring rule, not claimed to reflect player demand. Always present reason `LAST MINER STANDING` or `POINTS TIEBREAK` on results.
- To avoid an early auto-win while everyone extracts, consider minimum round active time and appropriate extraction gate locations, but do not turn winning into random chance.
- In solo debug only, show `SIMULATION COMPLETE`, not a false competitive victory.
- Store champion decision as single immutable per-run result to avoid two players winning due to race conditions.


`WAITING_FOR_PLAYERS` → `COUNTDOWN` → `GENERATING` → `SPAWNING` → `ACTIVE` → `ENDING` → `RESULTS` → `RESET`.
- Real PvP requires at least two human clients. For solo Editor tests, allow a **DEV_DEBUG_SOLO** flag that launches round without inflating the player count or pretending PvP happened.
- Players joining mid-round: join spectator-safe lobby and can queue for next round. MVP does not insert new players inside an active map.
- Match is local to a server for MVP; do not build cross-server matchmaking or many places yet.
- When the only surviving player leaves early, allow safe conclusion rather than waiting out timer.
- Create transparent status handling when no players or generation fails.

## 3. Underground world

### 3.1 Prototype spatial model
- Work with a logical grid, initially **48×48×6 cells × 6 studs/cell**, reducing dimensions if performance requires. The logical grid is a **server-side occupancy/resource model**, not a requirement to create ~14k physical Parts.
- Map has an outer indestructible shell. 2–12 isolated initial pockets, spaced to avoid near-instant combat. Main navigable caverns should feel interconnected only after mining.
- Later add biome bands, randomized chambers, dangerous depths and distinct minerals.
- **First playable restriction:** support clear walkable horizontal digging, plus curated vertical connections (shafts with stairs/ramps). Do not expose free vertical mining unless movement, falling, stuck detection and server validation are solved.
- Random seed per round; record seed in console/telemetry for reproducibility.

### 3.2 Traversability and map generation
- Spawn rooms are at least 2 cells high and have a diggable exit; safe spawn areas cannot be destroyed from outside immediately.
- At least two extraction points are reachable by some route from every spawn (pathfinding on expected diggable grid, not only initial walkable space). This can mean a player digs a reasonable distance; no “permanently trapped spawn”.
- Ensure enough baseline stone corridors/valuable ore near early spawns, but don't reveal player positions through the map.
- Indestructible boundary blocks are visually distinct (metal reinforcement/dark obsidian) with clear “UNBREAKABLE” feedback.
- Block removal creates immediate tunnel traversal; neighboring terrain visuals and collisions update efficiently.
- Spawn protected no-PvP zone lasts 60 seconds; do not encourage players to camp entrance after it ends.

### 3.3 Biomes (visual now, full gameplay later)
- **Dust Layer:** orange-brown dirt, copper; safest and brightest.
- **Slate Layer:** blue-gray stone, gold; more mysterious.
- **Crystal Layer:** navy black with luminous cyan/purple veins; highest loot/risk.
For MVP, use these as color/material treatments and limited variations, not 3 separate sprawling fully simulated maps.

## 4. Mining and discovery

### 4.1 Player inputs
Desktop: aim with mouse crosshair, left-click/hold mines; `1` pickaxe, `2` melee, `E` interact/extract, `Shift` sprint if appropriate. Mobile: right-hand `MINE` hold button or tap-on-target (choose and test), `ATTACK`, `INTERACT`, visual reticle; Roblox thumbstick remains unobstructed. No key is required to mine the very first block if a sensible default tool is equipped.

### 4.2 Block definitions (prototype values)

| Block type | Health/hits | Reward | Visual cue | Purpose |
|---|---:|---|---|---|
| Dirt | 1 | 0–1 dust | Brown smooth face | Fast progress / open tunnels |
| Stone | 2 | 1 stone | Blue-grey cracks | Baseline mine action |
| Copper Ore | 3 | 2–3 copper | Copper specks | First meaningful discovery |
| Gold Ore | 4 | 3–5 gold | Angular yellow flecks | Scarcity / mid-run reward |
| Crystal Ore | 5 | 1–2 crystals | Cyan luminescence | High stakes / highlight moment |
| Reinforced Border | ∞ | 0 | Dark steel + warning symbol | World boundary, not mineable |

Numbers are tuning defaults. Rewards derive from server-side configuration and seed; the client may render effects but may not choose ore, rarity or final yield.

### 4.3 Ore reveal
Hidden ore distribution must be deterministic by round seed on the server. Avoid instancing and replicating every hidden ore block in advance — clients should receive **visible** surface material only, not future loot contents. A struck visible block reveals what was behind it based on deterministic server data. Hide contents until reveal. Use local particles/glow for already exposed rare veins.

### 4.4 Tool upgrades and pace
A starter pickaxe is permanent. Some persistent Salvage buys 3 early grades of mining speed or carry capacity; price grows while advantage stays bounded. Combat skill, tactical choices and awareness should outweigh account progression in PvP. For MVP, tool progression may be showcased in lobby with only one purchase and one obvious improvement.

### 4.5 Mining feedback
Impact screen shake (subtle), sparks, dust burst, crack decal/progress bar, tiny loot popup, mineral sound, short celebratory accent on crystals. Effects must be toggleable/reduced on mobile. Immediate client visual prediction allowed, but state only changes on confirmed server result; reconcile promptly after denial.

## 5. Encounters and PvP

### 5.1 Unique selling point
Two mining players are separated by a thin wall. Their pickaxe sounds travel imperfectly. Neither sees the other. One more swing removes the block. One player can flee, talk through approved Roblox chat, or fight. This must be a genuinely possible situation in the real multiplayer world, not a pre-recorded animation.

### 5.2 Noise, information, counterplay
- Nearby mining emits directional/muffled audio within a limited radius; audio “clues” rather than perfect position and distance.
- Later: dust falling off ceilings, cracks, footprints, scanners with cooldown. No wallhack minimap or always-on arrows pointing at enemies in MVP.
- In PvP use local hit indication and damage amount optional, health displayed only when encounter visible (if enabled).

### 5.3 Combat prototype
- All players 100 HP. One sword/short pick; server checks equipped state, hit range, line of sight when appropriate, angle, cooldown, target live state, phase not protected, friendly/self cases.
- Proposed default: 20–25 damage per validated swing, ~0.8 s cooldown; test responsiveness and time-to-kill. Values configurable, not backed by platform data.
- No automatic aim-lock through walls. Avoid stun-lock; give ~0.3–0.5 s damage feedback without disabling control.
- Cosmetic hit sparks only: platform-appropriate fantasy fighting; no gore, realistic wounds or gambling/death-for-Robux.
- Basic anti-teaming considerations: solo mode first; if teaming causes issues later consider squad queue, but don't add complex anti-team detection at MVP.

### 5.4 Death penalty
Starter equipment and durable profile remain. Suggested default: **40% of unbanked ore salvaged** as fallback, **60% dropped as a single interactable bag**; explicit integer rounding and anti-duplication. Alternative configurable `RunLootLossRate` if UX testing shows frustration. Dead player goes to lobby/result and can join future round. Don't destroy all rewards for new players without evidence.

## 6. Extraction and bank

### 6.1 Mechanics
- 2 green escape points on map, distinguishable by shape, beacon, color and audio; location revealed as reachable **directional objective**, not wallhacks or enemy coordinates.
- Extraction unlocks after **4 min**; HUD shows countdown to unlock. Channel lasts **6 s**, requires presence in trigger region, valid living player, and no incoming damage or movement out of range during channel.
- When successful: transfer current **run ore** to durable **Salvage** (conversion table/config), increment `ExtractedRuns`, give result recap and teleport character to safe lobby. No duplicated rewards on repeated request/reconnect. Extracted player no longer vulnerable in match.
- At round time expiry, apply **25% fallback** on unbanked resources for surviving unextracted player (separate from death salvage; ensure exactly one settlement event).
- If loot bag collected by other player, update inventory with capacity limits and mark bag consumed atomically.

### 6.2 Result UX
Result shows `Ore Found`, `Salvage Banked`, `Players Defeated`, `Time Underground`, `Biggest Find`; choice `UPGRADE PICKAXE` or `PLAY AGAIN`. If no persistence in Studio tests, tell developer via debug note, not player via ugly stack trace.

## 7. Meta-progression & economy

### 7.1 Two inventories
**Run inventory:** ephemeral; ore, picked-up cache rewards and salvageable items, cleared after death/extraction/timeout.  
**Profile:** durable; `schemaVersion`, Salvage, PickaxeTier, CosmeticUnlocks, LifetimeStats, TutorialState, optionally Settings. Never treat a client's in-memory GUI count as the source of truth.

### 7.2 Progression examples (test values)
- Grade 1 starter pick: 1.0× mine speed.
- Grade 2: 1.15×, cost 30 Salvage.
- Grade 3: 1.3×, cost 100 Salvage.
- Initial backpack 40 units; bigger capacity may be a later upgrade.
No rebirth/prestige, idle generators, pets, loot boxes or massive currency inflation in MVP.

### 7.3 Economic integrity
- Mining reward never awarded twice for same cell.
- Multiple interactions on one bag/extraction must settle once, including fast repeat requests.
- Each unique run has ID; reward transactions idempotent by `runId` / event ID where appropriate.
- Pickaxe price checked on server; negative values/non-finite numbers rejected.
- Avoid account power creep destroying fair encounters. Distinguish mining quality-of-life from combat advantage.

## 8. UX and onboarding

### First 60 seconds (hypothesis)
0–5 s: appear in sheltered grotto, glowing ore immediately visible. One strong callout: **“DIG TO ESCAPE”**.
5–20 s: aim/punch/mine first blocks; sensory feedback; first ore popup.
20–40 s: first tool/item reward; visible `BACKPACK: 3/40`.
40–60 s: subtle distant mining SFX and copy: **“HEAR THAT? YOU'RE NOT ALONE.”** PvP safety timer ending indicator. No six-page tutorial.

### Minutes 1–4
Broaden route choices and resource quality. Player discovers early tunnels and recognizes a high-value ore vein. Several possible directions, not a single scripted hallway.

### Minutes 4–8
Extraction opens and creates strategic choice; riskier ore zones become attractive; a few PvP encounters possible. Never enforce that every player fights.

### Minutes 8–12
Extraction threat and time pressure intensify; tell players what will happen if time runs out. Successful bank should feel rewarding even on modest first run.

### UI rules
- Never flood UI with floating text. Use 2–3 clear live priorities.
- Large high-contrast interactions with legible labels and icons; no small text on mobile.
- All countdowns and inventory numbers consistent with server state.
- Use readable non-flashing cues as alternative to pure sound/color.
- Ensure HUD doesn't obscure avatar or aim area.

## 9. Future features — explicitly OUT OF MVP

1. Multiple named biomes with bosses/hazards.
2. Dynamic small caves, deep vertical mining, elevators.
3. Procedural PvE creatures.
4. Duos/squads, factions, clan burrows.
5. Trading and marketplace.
6. Ranked seasons/competitive ladders.
7. Rare weapon modifiers and advanced gear.
8. Cosmetic monetization, passes and non-competitive customization.
9. Cross-server matchmaking and reserved server flow.
10. Anti-teaming/wallhack protections beyond baseline visibility and validation.
11. Replay / clip-friendly spectator camera.

These are future hypotheses, not instructions to build immediately. After MVP tests, prioritize based on retention/fairness/technical results.

## 10. Instrumentation and tests

Essential events (no personally sensitive freeform payloads):
`game_join`, `tutorial_first_mine`, `mine_attempt`, `mine_confirmed`, `ore_collected`, `rare_ore_discovered`, `player_heard_nearby` [if reliably detected], `encounter_started` [precise definition required], `pvp_damage`, `player_died`, `extraction_started`, `extraction_interrupted`, `extraction_completed`, `run_ended`, `upgrade_purchased`, `requeue_clicked`, `client_error`.

Every event should include event version, session/run ID, phase timestamp, relevant ore/tool tier, but no sensitive user information or excessive high-frequency logs. Define `encounter_started` as **a server-observable condition**, e.g. two living players with line of sight and within 40 studs for at least 0.5 s; don't pretend it measures a player's psychology. Throttle mining telemetry aggregation.

MVP design test questions:
- Can a first-time player mine without explanation?
- Do most test players see another human in a typical multiplayer run?
- Do players understand why they lost part of their loot on death?
- Can players reach a valid extraction from every spawn with a finite amount of mining?
- Is the PvP fight decided by awareness/skill rather than mining account level?
- Does client FPS and memory remain acceptable on low/mid-tier mobile test conditions? Measure, don't invent.
- Does a player want to start another run **after** extracting or dying?

## 11. Prototype decision log

All numeric settings are placeholders until measured in Studio and playtested with actual people. Agent must maintain a `TUNING_NOTES.md` or config changelog listing the original value, change, reason, test date and observations. Highlight trade-offs:
- Time-to-first-ore vs discovery surprise.
- Dense tunnels vs meaningful isolation.
- High mining speed vs world running out of blocks.
- Loot on death vs beginner frustration.
- Player density vs unfair spawn ambush.
- Visual darkness vs mobile visibility.
