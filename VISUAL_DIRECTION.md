# ANTWARS — VISUAL DESIGN, ENVIRONMENT & UI ART BRIEF

## 1. Core identity

**Look:** readable Roblox blocky adventure, hand-crafted underground labyrinth, tactile stone, ore sparks, lantern pools, small glowing crystal veins, dark cinematic edges but playful/approachable characters. Think **mystery + excitement**, not horror, gore, realistic war or lifeless default Roblox terrain.

**Reference poster:** `../assets/reference/ANTWARS_CONCEPT_ART.png`. Generated concept art shows world atmosphere, a mining encounter, example HUD, ore and equipment. Treat it as **direction only**; reproduce composition/color/feedback with Roblox-native primitives and legitimate assets, not as a pixel-exact demand. **No upload/purchase/publication without permission.**

**World schematic:** `../assets/reference/ANTWARS_MAP_SCHEMATIC.png` — developer-only schematic for spawn separation, hidden ore, shared tunnels and escape routes. This schematic must NOT be sent to players as a fully revealed minimap.

**Reference HUD wireframe:** `../assets/reference/ANTWARS_HUD_WIREFRAME.png`. This is an original schematic for UI hierarchy, not a finished game screenshot.

### Color palette

| Token | Hex | Meaning |
|---|---|---|
| Night | `#0D1522` | Underground atmosphere/backplates |
| Rock | `#293445` | Stone and panels |
| Earth | `#694330` | Dirt and support beams |
| Copper | `#D97837` | Basic resource |
| Amber | `#FFBD50` | Gold/rare resource |
| Crystal | `#11CDEB` | Cyan ore / mining accent |
| Epic | `#904DE0` | Very rare crystal |
| Escape | `#2AD78F` | Extraction and success |
| Hazard | `#EB5959` | Enemy damage and interruption |
| Soft white | `#E5EDF8` | Main text |
| Muted text | `#97A9C1` | Secondary hint |

Accessibility: **do not rely only on color**: copper = angular flecks, gold = broad bright triangles, crystal = sharp translucent spikes, escape = triple upward arrows and green glow, enemy hit = sharp impact cue and red flash.

## 2. World hero-shot art direction

Visual of first run:
- Camera at shoulder-height behind a Roblox-stylized adventurer wearing mining backpack.
- A cyan pickaxe hits a dark rock wall; fragments, crisp cracks and blue sparks appear.
- Uncovered pocket in wall reveals small gold crystals to the right.
- Another tunnel behind wall shows a **brief distant silhouette** of another player, not a HUD wallhack marker.
- Warm lanterns, rough stone arches, minimal abandoned wooden props.
- Far-off extraction beacon is bright green, distinct from ore glow; if not physically visible, HUD shows heading to portal when unlocked.
- Mood is **“something lives beyond this wall”** while remaining bright enough to distinguish interactable cells on mobile.

## 3. Environment kit for first pass

Use Roblox primitives and materials to create a coherent kit:
- `Dirt_01`: warm brown cubic/disrupted surface, irregular small dots.
- `Stone_01`: dark slate rock, cracks/decal visible when mining.
- `Copper_01`: warm burnt-orange angular ore inclusions.
- `Gold_01`: faceted yellow inclusions that contrast with copper.
- `Crystal_01`: cyan clusters with optional restrained PointLight.
- `Border_01`: dark reinforced metal, chevrons or warning stripes, visibly unmineable.
- `Lantern_01`: small copper frame + glowing warm cube; duplicate at authored pockets/landmarks, not on every cell.
- `SupportBeam_01`: beams that visually frame tunnels and make scale legible.
- `ExtractionGate_01`: industrial octagonal portal with green chevrons and animated edge, collision-free valid trigger region, optional sign `EXTRACT`.
- `LootBag_01`: stylized gold-brown pouch/crate with unmistakable interactive icon.
- `StarterPickaxe_01`: charcoal handle + cyan head; simple enough to prototype as 3–5 Parts before full mesh.

Avoid unique high-poly mesh for every rock. Keep clear silhouettes and repeated modules with occasional variation. Restrict lights and shadows to performance budget.

## 4. HUD wireframe and states

### 4.1 Desktop HUD sketch (not to scale)

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ ♥ 100/100  [######----]     ⏳ 08:42   EXTRACTION OPEN       [Explored Map] │
│ ⛏ Tier 1  • PvP protection 0:24                                [░▓░▓░ ]  │
│                                                                              │
│                                                                              │
│                    (underground cave / player world)                         │
│                                                                              │
│                            +                                                 │
│                     [STONE • 2 HITS]                                         │
│                                                                              │
│                                                                              │
│                                   [HEAR THAT? YOU'RE NOT ALONE...]           │
│                                                                              │
│              [1 ⛏ PICK]   [2 ⚔ BLADE]   [3 🧪?]      [PACK 19 / 40]         │
│                           COPPER 12 | GOLD 4 | CRYSTAL 1                      │
└──────────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Mobile HUD state
- Movement thumbstick reserved lower-left (Roblox built-in thumbstick).
- Bottom-right: `MINE` big (~64–88 logical px depending on device, use scale constraints), `ATTACK` (~64–88), `INTERACT` contextual above.
- Top-left health and simple protection icon; top-center timer/extraction; top-right map toggle (explored cells only).
- Backpack collapsed to one count and expandable popover; 3D view remains dominant.
- All tap areas tested with mobile emulator and a tall aspect ratio. Avoid relying on mouse-hover tooltips.

### 4.3 States and messaging
- `MINE`: reticle pulses; `STONE 2/2`, fracture step animation, reward toast `+2 COPPER`.
- `NEARBY SOUND`: restrained orange ear icon and diegetic 3D audio; do not display enemy precise coordinates.
- `PROTECTED`: shield icon and `SAFE FOR 00:37`; PvP blocked in both directions.
- `EXTRACTION LOCKED`: amber countdown `OPENS IN 02:14`.
- `EXTRACTION OPEN`: green header `EXTRACT NOW` + directional compass marker if needed.
- `EXTRACTING`: green ring + `HOLD POSITION: 6.0s` with interrupt feedback.
- `DAMAGE`: red damage arc from approximate direction, quick fade, not huge vignette.
- `DEATH`: respectful result `RUN ENDED` + `SALVAGED 12` + `LOST 18` + `PLAY AGAIN`.
- `SUCCESS`: `ESCAPED WITH YOUR LOOT!` + currency conversion and one bright reward beat.
- `CHAMPION`: `LAST MINER STANDING — CHAMPION!` + golden winner banner for the sole genuine multiplayer winner; if tie-break, explain `CHAMPION BY SCORE`. Do not confuse successful extraction with victory.

- `FULL BAG`: `BACKPACK FULL — EXTRACT TO BANK LOOT`, a calm amber prompt.
- `UNBREAKABLE`: small white lock over dark reinforced wall, no false progress bar.

## 5. Menu sketches

### Lobby
Left: logo + `MINE • FIGHT • EXTRACT` / `PLAY` / `HOW TO PLAY`.  
Center: small 3D grotto showcase or player's avatar with equipped pickaxe.  
Right: `SALVAGE`, `PICKAXE UPGRADE`, basic loadout and cosmetic placeholder.  
Bottom: queue size and support text; no monetization CTA in prototype.

### Results
`RESULT: EXTRACTED / DEFEATED / TIME UP`  
`ORE FOUND` | `BANKED` | `PLAYERS DEFEATED` | `SURVIVED`  
One primary button `PLAY AGAIN`; secondary `UPGRADE`. Important explanation of death/timeout settlement.

## 6. Logo/icon/thumbnail later

Working logo: **ANTWARS** large geological letterforms, crack line through `A`, small cyan crystal, subtitle `BELOW THE SURFACE`. **Do not use copyrighted franchise logos**. Thumbnail concept: two blocky miners on either side of one thin stone wall, miner A sees a golden vein, miner B carries a glowing sword; headline `WHO'S BEHIND THE WALL?` or `DIG. FIGHT. ESCAPE.` Avoid text too small for mobile.

## 7. Scene validation screenshots

Take captures from the actual Roblox Studio playtest and compare with reference:
- A: spawn chamber is recognizable and glowing ore visible.
- B: before/after mining shows a genuine navigable hole.
- C: third-person miner + enemy reveal in adjacent tunnel.
- D: extraction scene clearly marked green with progress HUD.
- E: mobile emulator overlay legible.

A screenshot is evidence of **appearance** only. It is not proof that mining, combat, or extraction logic actually works. Pair with tests.
