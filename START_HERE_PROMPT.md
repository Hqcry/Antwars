# ANTWARS — MASTER EXECUTION PROMPT FOR CODEX / CLAUDE CODE

> Project: **ANTWARS: BELOW THE SURFACE** (working title). Language for developer communication: **Russian**. In-game copy: concise **English**, stored in one localization/config module for later translation. Engine: **Roblox Studio + Luau**. Date of brief: **12 October 2026**. Stage: **playable pre-alpha vertical slice**, not a finished commercial game.

## YOUR ROLE AND AUTHORITY

You are the hands-on **lead Roblox gameplay engineer + technical game designer + environment/UI prototyper + QA engineer**. **BUILD AND TEST** the game inside the currently open Roblox Studio place using the official Roblox Studio MCP integration, not merely write advice or theoretical scripts. You have access only to the tools actually provided by the connected client; verify which are available before use. You may create well-organized scripts, objects, scenes, UI and tests in a new/safely isolated workspace. Do not publish, purchase assets, spend Robux, enable public access, destroy existing user content, modify unrelated places or install untrusted plugins without explicit approval.

This is a **new product**; where requirements conflict, prioritize: (1) a runnable, secure multiplayer gameplay loop; (2) clear player experience; (3) performance; (4) coherent visual quality; (5) extra mechanics. Do not overengineer phase 1.

**Start by executing these steps, in order:**
1. Confirm MCP connection using available Studio discovery/state tools, list Studio instances if applicable, identify correct open place and current Edit/Play state. If MCP is unavailable, state exactly what is blocked, create/update local project source files when tools allow, and provide a short, specific connection fix; NEVER say you modified Studio unless you actually did.
2. Inspect DataModel / scripts / Workspace and take a baseline screenshot (if tools available). If existing assets are present, do not overwrite them blindly; put new content in clearly named `AntWars_*` containers or make a backup/duplicate for significant edits.
3. Read `docs/ANTWARS_GDD.md`, `docs/TECHNICAL_ARCHITECTURE.md`, `docs/VISUAL_DIRECTION.md`, `docs/BUILD_AND_TEST.md`, `docs/TASKS.md`, and `docs/POST_MVP_PRODUCT_STRATEGY.md` in this package. Use `assets/reference/ANTWARS_CONCEPT_ART.png` and `assets/reference/ANTWARS_HUD_WIREFRAME.png` and `assets/reference/ANTWARS_MAP_SCHEMATIC.png` as VISUAL REFERENCES, not as guaranteed-accurate final UI assets. If only this prompt is present, use the full brief appended in `ANTWARS_MASTER_PROMPT.md`.
4. State a **concise implementation plan** (max 10 bullets) and immediately start building; do not spend the whole answer writing plans. Make reasonable defaults and document assumptions. Ask only for an essential permission/irreversible decision.
5. Deliver a complete **first playable loop** in the open Studio place: spawn in an underground chamber -> mine destructible rock -> gain virtual ore in run backpack -> find crystals/gear -> reach another player via tunnel -> fight -> extract to bank resources -> return to lobby -> start another round. If no second real player is present, simulate the gameplay state in tests, but do not claim real PvP is validated until a 2-client test passes.
6. **Playtest repeatedly**, capture output/errors, fix blocking issues, visually inspect with screenshot(s), and report: created objects/files, working features, tests passed/failed, known limitations, next task. Finish with a playable phase, not a pile of broken scripts.

## PRODUCT VISION / NON-NEGOTIABLE CORE

**Pitch:** 2–12 players enter the same giant mysterious underground labyrinth. Everyone starts in separate sealed chambers and carves tunnels through destructible soil and stone. Hidden ore yields loot; distant pickaxe strikes reveal that somebody else might be on the other side of the wall. The player weighs greed against survival: keep mining for a rare crystal, ambush another player, or escape through an extraction portal and permanently bank the run's discoveries. Short, readable PvP clashes produce surprising emergent stories. The experience is tense and rewarding but **not gruesome, unfair, or unplayable for beginners**.

The game name **ANTWARS** describes ant-like tunnel networks and territorial encounters. **Players are Roblox-style adventurers, not literal ants**. Do not replace concept with ant colonies, insect warfare, generic tycoon, Minecraft clone, tower defense or an endless simulator. The distinctive fantasy is: **“You never know who is behind the next wall.”**

**ORIGINAL USER REQUIREMENT — EXACTLY ONE WINNER:** The founder originally specified that ONE PLAYER wins each round. Preserve that in game rules: extraction is an optional cash-out that forfeits the round championship; the last active non-extracted survivor becomes **CHAMPION** if exactly one remains. If time runs out or everyone extracts/dies, select one champion using a deterministic server-side ranking (PvP defeats → banked+carried ore → survival duration → unique player ID tie-break), clearly explaining that this is a fallback rule. The MVP must display ONE CHAMPION. These are prototype design decisions; keep configurable for future user review.


Core loop:
`LOBBY → PREPARE → SPAWN SEPARATELY → DIG → DISCOVER ORE/GEAR → HEAR OTHER PLAYER → BREAK THROUGH → FIGHT OR EVADE → EXTRACT → BANK LOOT / UPGRADE → REQUEUE`.

MVP principles:
- **Mining is physical and multiplayer:** a block disappears for all players after a server-validated mine action; it reveals a traversable cell/tunnel, not just changes a number on the screen.
- **Discovery is uncertain:** no enemy outlines through solid walls, no full-map enemy radar, no client-replicated secret loot tables.
- **First reward fast:** player understands mining and receives first ore within ~20–40 seconds (design target; verify in tests).
- **First potential encounter:** world design should allow an encounter within ~3–6 minutes with multiple active players (initial design target, not measured market data).
- **Meaningful extraction:** accumulated **run loot** is at risk, but basic equipment and durable account progress are safe.
- **Mobile first:** large buttons, readable HUD, high contrast, usable at small screens.
- **No pay-to-win, paid randomness, wagering, Robux-linked loot risk or real-money rewards in the MVP.**

## INITIAL GAME RULES — CONFIGURABLE HYPOTHESES, NOT VERIFIED METRICS

- Match target: **12** players; **minimum 2** for true PvP matches; solo development/debug mode allowed. Provide config `MinPlayers`, `MaxPlayers`. Match length: **12 minutes**; configurable and visible in HUD.
- Intro/spawn safety: **60 seconds** of PvP protection, clear icon, end-of-protection warning. No ability to exploit protection to deal damage.
- Layout: seed-based underground 3D environment, approximately **48×48×6 logical cells**, **6 studs** each. This is a starting prototype target; reduce if real tests show memory/fps/network problems. Do NOT create one permanently replicated Part per filled voxel. Use sparse/chunked visible geometry (or a measured Terrain solution) with authoritative logical occupancy.
- First version may restrict player-modifiable mining to X/Z and curated vertical access routes while retaining 3D atmosphere; mark additional freeform vertical digging as Phase 2. Never fake it as implemented.
- Stone: ~2 hits; dirt ~1; copper ~3; gold ~4; crystal ~5. Values are illustrative and should live in config, not hard-coded throughout scripts. Use three ore tiers minimum.
- Backpack: **40** run-resource units by default. Mining yields resources only if validated and capacity allows; prompt to extract or discard when full.
- Loot: ore has guaranteed small rewards, scarce crystal clusters, **optional** weapon pickup from physically exposed caches. Do not expose hidden ore in client state before reveal.
- PvP MVP: one reliable melee weapon, health 100, damage with server-side hit/distance/cooldown validation, respawn in lobby on death. No gore, realistic blood, scary body horror. Avoid kill farming at spawn.
- Extraction: place **two distinguishable extraction portals**, unlocked after minute **4**; player must reach one and channel for **6 seconds**. Channel is interrupted by damage, leaving zone, or death. Extracted player banks all carried run resources and returns to lobby/result screen. Portals need obvious green beacon and directional cues, not exact coordinates of enemies.
- Death: keep persistent progression and starter equipment; **40% of unbanked run ore is salvaged** into a small fallback reward, remaining **60% drops as one world loot bag** collectable by others (if this presents fairness/performance problems, make the loss rate configurable). A dead player exits the round; no repeated instant PvP deaths in same match. Loot drop values are whole numbers with explicit rounding rules.
- Match timeout: surviving players who have not extracted get **25% fallback banked ore** and clear explanation; no full rewards. Check this does not encourage AFK farming.
- Persistent currency: **Salvage**, earned from extracted ore conversion and allowed partial fallback. Upgrade pickaxe mining speed/range by small increments but prevent direct combat stat purchases/upgrades in MVP. No trading or user-to-user paid items.
- End-of-round leaderboard: ore extracted, opponents defeated, survival time. Status-based praise, not only richest-player dominance.
- **Single champion condition:** on only one living, active, non-extracted contestant, conclude with that person as sole champion; extraction cashes out resources but forfeits win. On time-up or zero active contestants, apply deterministic scoreboard winner tie-break described above. Do not award multiple champions. If only a solo debug player exists, do not pretend competitive champion victory was earned.

- Tutorial: 3 short contextual prompts: `MINE [rock]`, `LISTEN [distant digging]`, `EXTRACT [green beacon]`.

## STUDIO OBJECTS AND CODE QUALITY

Use **strict-ish typed Luau** (`--!strict` when practical), clear names, small modules, clean task lifecycles, one source of truth per state. Suggested containers:

```
ReplicatedStorage/
  AntWarsShared/
    Config (ModuleScript)
    ItemDefinitions (ModuleScript)
    GridMath (ModuleScript)
    RemoteNames (ModuleScript)
    Types (ModuleScript)
  AntWarsRemotes/
    MineRequest (RemoteEvent)
    CombatRequest (RemoteEvent)
    ExtractionRequest (RemoteEvent)
    ClientState (RemoteEvent)
    WorldDelta (RemoteEvent)
    PlayerFeedback (RemoteEvent)
ServerScriptService/
  AntWarsServer/
    Bootstrap.server.lua
    Services/
      MatchService.lua
      WorldGridService.lua
      MiningService.lua
      InventoryService.lua
      CombatService.lua
      ExtractionService.lua
      PersistenceService.lua
      TelemetryService.lua
      ValidationService.lua
StarterPlayer/StarterPlayerScripts/
  AntWarsClient/
    Bootstrap.client.lua
    Controllers/
      InputController.lua
      MiningController.lua
      CombatController.lua
      CameraController.lua
      HudController.lua
      EffectsController.lua
StarterGui/
  AntWarsHUD (ScreenGui)
Workspace/
  AntWarsWorld/
    GeneratedChunks/
    Lobby/
    ExtractionPoints/
    SpawnChambers/
    AudioZones/
ServerStorage/
  AntWarsServerAssets/
```

The exact folder mapping may differ when connected through MCP and when using Rojo: **prefer working real Studio objects over imaginary file paths**. Never place authoritative combat, mining rewards, inventories, or save logic solely on the client. All RemoteEvent payloads type-checked, distance-checked, cooldown-limited and state-checked on server. Client sends **intent**, not `giveOre=1000`, `dealDamage=9999`, `destroyInstance=...` or arbitrary path. No arbitrary model/asset injection from client messages.

For persistence, use `DataStoreService` with versioned schema, `UpdateAsync` where appropriate, `pcall` error handling, save throttling, autosave/disconnect handling, and Studio mock storage in development to avoid accidentally overwriting live data. **Do not enable live DataStore access without asking.** Lobby/result must still function if persistence is temporarily unavailable.

For performance, profile mobile budgets. The game must not keep 10k+ independent physics/collidable blocks instantiated or replicate full underground terrain state every frame. Implement deterministic world generation, only exposed chunks/faces, pooling or prudent cell visualization, and delta replication. Use `StreamingEnabled` judiciously and test it; don't assume it fixes all voxel costs. Preserve correctness when two players mine the same block almost simultaneously.

## VISUAL ART DIRECTION (REQUIRED, NOT OPTIONAL)

Reference: `assets/reference/ANTWARS_CONCEPT_ART.png`. This is a **mood board** — visual hierarchy matters more than exact fidelity.
- Distinctive blocky/voxel-compatible Roblox stylization; underground stone, compact tunnels, mineral crystals, wooden props, lanterns; **not a generic empty gray baseplate**.
- Colors: almost black navy `#0D1522`, basalt `#293445`, earth `#694330`, copper `#D97837`, warm gold `#FFBD50`, cyan crystal `#11CDEB`, purple epic `#904DE0`, extraction green `#2AD78F`, hazard red `#EB5959`.
- Key screenshot: camera behind blocky miner, cyan pickaxe hits stone wall, blue glowing crystal exposed, distant **ambiguous silhouette** at a newly opened tunnel, warm lantern behind; green escape beacon glimpsed far away.
- Visual clarity: ore identifiable by shape + color, readable even without color vision. Cracks/sparks and progress indication during mining. Combat sparks and satisfying but restrained hit feedback. No gore.
- HUD on desktop: health upper-left, timer + extraction state top-center, compact exploration-only map upper-right (no wallhack), resource inventory/hotbar bottom, mining crosshair center, contextual prompt near crosshair, events ticker. Mobile: left thumb movement stick, right `MINE`, `ATTACK`, `INTERACT` large tap areas.
- Ambient sounds: digging through rock, low underground rumble, crystal shimmer, muffled player mining sound whose volume/spatialization can build suspense. Audio must not incorrectly reveal precise coordinates through solid walls; balance gameplay cues in playtests.
- Lighting: cool ambient in caves, warm orange practical lights, colored ore accents, green extraction. Must stay readable on a mobile device at default brightness. Use Roblox-native primitives/materials/placeholders if external assets are unavailable.

## IMPLEMENTATION MILESTONES — EXECUTE, VERIFY, THEN ADVANCE

**M0 — Connect & survey:** Verify Studio session, empty/new/safe target, prepare folders, config, remote contracts, reference screenshots; produce technical notes.

**M1 — First playable mining:** Build atmospheric sealed cave, character spawn, mouse/touch mine action, server-authoritative block removal, ore rewards, HUD changes, sounds/particles, camera collision; works in single-client Studio Play and zero errors.

**M2 — Multiplayer encounters:** Two real test clients, separated spawns, shared persistent tunnel destruction within match, player footsteps/digging audible through nearby rock, short melee combat with server validation, death/reward handling. Confirm both clients see identical mined holes and no double rewards.

**M3 — Actual extract loop:** Match timer, green extract portal and channel, banked resource conversion, results screen, requeue, safe fallback when dead or time expires. Save test progress behind environment-safe persistence abstraction.

**M4 — Polish/prove:** Mobile input, optimized chunk streaming, encounter pacing/telemetry, screenshot reviews, 2–4-player tests, critical exploit tests, documented performance. Only after M3/M4 propose ranked modes, equipment rarity, clans, advanced vertical digging and monetization.

**Acceptance criteria** (do NOT claim passed without evidence):
1. On launch the player sees a compelling underground scene, not baseplate and no red errors.
2. A click/tap against diggable rock gives immediate feedback; server checks range/tool/cooldown and creates a persistent visible path for all clients.
3. Ore increments exactly once, inventories cannot exceed capacity, invalid remote arguments cause no state change.
4. Two clients can meet via player-made tunnels and fight; server decides damage, target and cooldown.
5. Extracting banks correct amount and clears temporary carried loot; death applies configured loss and persists appropriate fallback.
6. Leaving and rejoining does not duplicate rewards and does not corrupt profile data.
7. Basic UX works on mobile emulator; controls don't obstruct gameplay.
8. AI agent has run actual Play mode, retrieved console output, fixed errors, and captured before/after screenshots when tools permit.

## COMMUNICATION AND DONE DEFINITION

As you work, give short checkpoint reports in **Russian** after milestones. Report actual measured/observed outcomes rather than fabricated test coverage or FPS. If a feature wasn't built, mark it `NOT IMPLEMENTED`. If a multiplayer test wasn't possible, mark it `NOT VERIFIED` and explain the exact workaround. If blocked by permissions, ask one specific question and otherwise continue on unblocked work.

At the end, provide:
- `WHAT WAS BUILT` (specific Studio paths / source files)
- `PLAYTEST RESULTS` (mode, clients, date, observed behavior, output/errors)
- `SECURITY / PERFORMANCE` (which checks exist, bottlenecks)
- `VISUAL QA` (screenshots or tool-produced observations)
- `KNOWN LIMITATIONS`
- `NEXT IMPLEMENTATION STEP` (one clear next vertical slice)

**Your first response should be brief and operational: identify the connected Studio place, state the first 3 actions, then immediately inspect/build the project. Do not merely reprint the design document.**
