# ANTWARS — WORK ITEMS, ORDER AND DEFINITION OF DONE

## Execution rules
- **Do not spend entire session producing a specification.** A playable test is the goal.
- Do not add multiple redundant feature systems before fixing mining and synchronizing world state.
- If Studio MCP exists, use it; if not, diagnose once, then continue only work actually available without pretending to control Studio.
- Every completed task receives evidence: Studio instance path, source file, console/test result, or screenshot as appropriate. Mark missing tests explicitly.

## Milestone M0 — Safe initialization
- [ ] Inspect open Studio session (name/place id/state) and current data model.
- [ ] If content exists, preserve it and isolate AntWars content.
- [ ] Establish `ReplicatedStorage/AntWarsShared`, `AntWarsRemotes`, `ServerScriptService/AntWarsServer`, `StarterGui/AntWarsHUD`, `Workspace/AntWarsWorld` (or equivalent).
- [ ] Central `Config`, remote contracts and bootstrap in place.
- [ ] Visual reference reviewed, screenshot of initial scene taken if possible.
**Exit:** no overwrites, game boots, Studio Output has no blocking errors.

## Milestone M1 — Real mineable cave
- [ ] Atmospheric underground room in 3D, all required materials visibly distinct.
- [ ] Server-side grid exists; bounds and spawn room; seed reproduction.
- [ ] Player can mine stone and copper by mouse/touch.
- [ ] Server validates cell/range/tool/cooldown; correct HP and ore awards.
- [ ] Destruction yields **physical hole** the player can walk through.
- [ ] Player's HUD resource counter, hit feedback and inventory cap.
- [ ] No hidden resource data sent prematurely; no one-Part-per-filled-cell approach.
- [ ] Play mode run, output errors fixed.
**Exit:** from fresh boot to 3 mined blocks and collected copper, no errors; inspect screenshot before and after hole.

## Milestone M2 — Multiplayer and PvP
- [ ] Two separated spawn areas.
- [ ] Both clients see same mining state and can enter shared tunnels.
- [ ] Other miner audible when genuinely near.
- [ ] Melee hit, death and loot bag from server, no one-tap unfair damage.
- [ ] 60-sec protection functional both directions.
- [ ] Damage through rock blocked.
- [ ] Concurrent dig same cell doesn't duplicate loot.
- [ ] 2-client test documented.
**Exit:** two actual test clients mine toward each other, meet, fight and one dies in an observed test. If missing, mark unverified.

## Milestone M3 — Extraction and persistent progression
- [ ] Waiting/countdown/active/ending/results phases and 12-min match timer.
- [ ] Two portals become available at 4 minutes (debug quick mode for tests).
- [ ] 6-second extraction hold with interrupts.
- [ ] Run inventory settled once; Salvage bank/round result correct.
- [ ] Death 40/60 split and world bag settlement once; timeout fallback.
- [ ] Lobby upgrade 1->2 applies mining improvement, profile service with mock Studio mode.
- [ ] `PLAY AGAIN` starts safe new run and wipes only temporary inventory.
- [ ] Exactly one `CHAMPION` displayed and announced after genuine multiplayer round; extractors are safe but not champion; deterministic timeout tie-break and solo debug label validated.

- [ ] Mid-extraction disconnect and spam tested.
**Exit:** one full extraction and one full death flow, and repeated settlement creates no extra currency.

## Milestone M4 — Playability polish
- [ ] Mobile UI allows mining/combat/extraction.
- [ ] Text readable at small resolutions.
- [ ] Measured approximate FPS / instance count / server costs in at least one representative test.
- [ ] No initial camera collision and tunnel clipping bugs.
- [ ] Event counters for tutorial, mine, conflict, extract and requeue.
- [ ] `README` documents how to start tests and tune settings.
**Exit:** private pre-alpha ready for 2–8 testers; do not call it commercially ready.

## Deferred backlog (do not let it block M1–M3)
- [ ] Free vertical digging and intelligent dynamic navigation.
- [ ] Ranked ladders, cross-server queues, seasonal cosmetics.
- [ ] Teams, factions, clans, trading, marketplaces.
- [ ] Procedural boss battles, raid events, pets, skill trees.
- [ ] Paid game passes and product monetization.
- [ ] Advanced spectator camera, game clips and social sharing.

## Required handoff report template
```
Studio: <verified place name / id>
MCP tools used: <actual, not hypothetical>
Completed: <specific assets and behaviors>
Changed: <scripts and instance paths>
Tested: <mode, human client count, exact interactions>
Errors: <relevant Studio Output>
Visual captures: <real screenshot evidence or NOT AVAILABLE>
Performance: <measurements or NOT MEASURED>
Known issues: <practical blockers>
Next step: <one milestone-sized action>
```
