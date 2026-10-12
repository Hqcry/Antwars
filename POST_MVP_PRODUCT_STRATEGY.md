# ANTWARS — POST-MVP PRODUCT STRATEGY (DO NOT IMPLEMENT YET)

This document prevents feature drift by recording the larger commercial vision. **Do not build these systems before the first multiplayer mining → combat → extract loop is stable.** All market and retention claims below are **hypotheses** until measured; none is a verified Roblox-specific benchmark.

## 1. Why the concept might work

**Novel core hook:** dynamic player-carved terrain creates unpredictable, replayable social encounters without requiring complex scripted missions. The dramatic moment is **someone breaking through the same wall from the other side**. Exploration, loot, fear, PvP and escape combine into one story. The main risk is that the actual digging is tedious or encounters are too rare/too lethal. Prove those before adding more assets.

## 2. Proposed audience

- Roblox players who like mining progression but seek higher-stakes gameplay.
- Light survival/PvP players who want approachable short rounds, not a demanding hardcore extraction shooter.
- Friends who share surprising ambushes, lucky crystal discoveries and escape clips.
- Mobile players requiring visible objectives and forgiving punishment.

Don't assume age/geography/genre overlap from these hypotheses; instrument onboarding/retention and conduct small interviews.

## 3. Long-term mode roadmap

**v0.1 private pre-alpha (now):** single 12-minute mode, seeded caves, reliable digging, basic ore, a single weapon, extraction, simple progression.

**v0.2 alpha:** 2–3 cave biomes, 3 tuned weapons, more mining tools, one environmental threat, 6 additional ore and cosmetic types, social profile summary. Improve time-to-encounter using actual telemetry.

**v0.3 social alpha:** opt-in 2-player duos, team spawn chambers, non-lethal communication wheel, rematch party. Solo and duo progression separated for fairness.

**v0.4 beta:** rotating cave modifiers (unstable walls, crystal surge, tremors), seasonal leaderboards and cosmetic rewards, content update cadence; add replay-ready highlights only if evidence suggests users share clips.

**v1.0 public launch:** only after real mobile/performance/exploit tests and favorable cohort outcomes; stable persistence, moderation and updates.

## 4. Future monetization hypotheses — no implementation in MVP

Prefer **cosmetics, expression and identity** over buying combat advantage:
- pickaxe skins, ore impact VFX, helmets, backpacks, lantern colors, profile banners;
- extraction animations / victory poses / nameplate themes;
- basecamp room decorations, personal showcase shelves;
- optional seasonal cosmetic track only after enough organic play;
- limited inventory quality-of-life only if no meaningful PvP or economic advantage (evaluate carefully).

**Avoid:** buying lives, purchasing superior PvP stats, premium extraction immunity, pay-to-keep looted currency, paid random boxes connected to extraction risk, Robux wagers, cash-out markets. Don't implement `MarketplaceService` until the user approves legal/policy review and monetization design.

## 5. Candidate retention hooks (not artificial streak addiction)

- **Run variation:** procedural room layouts and 1–2 randomized cave modifiers.
- **Mastery:** real skill at aiming routes, deciding when to leave, reading footsteps and fighting.
- **Collection:** rare non-combat cosmetic ore artifacts; display in lobby.
- **Social:** duo escape and shared tunnel building; eventual clan wall of fame.
- **Goals:** short optional challenges (“extract with three gold ores”) not repetitive login gates.
- **Long-term:** mining prestige as cosmetics/efficiency with capped advantage, not infinite stat inflation.

## 6. Measurement / decision gates

Before public launch, collect the following with consent and platform-compatible analytics: onboarding completion, first successful mine rate, time to first ore, attempted fights and actual encounter rate, channel starts and completions, deaths, successful extractions, second-run starts, session length, D1 and D7 cohorts if available. Do not fabricate published Roblox genre thresholds. Define test-specific thresholds and compare cohorts/versions.

**Kill/pivot criteria to consider:** player testing shows digging is monotonous even after visual/reward tuning; most sessions end without meaningful discovery, encounter or extraction; deaths feel arbitrary; server cannot support the map under normal mobile device/network constraints; monetization would need unfair pay-to-win to cover content cost. Concrete thresholds require observed player data and budget context.

## 7. Pre-mortem

| Failure | Mechanism | Cheap check | Mitigation |
|---|---|---|---|
| Boring mining | Repeated interactions lack decisions | 5–10 first-time player observation | Reveal choices, meaningful ore/pacing |
| No player contact | World too large / split | Multi-client logs, encounter time | Tune spawn geometry and tunnel permeability |
| Unfair spawn fights | Collisions in first 30 seconds | Staged 2–8 player tests | 60s protection, staggered spawns |
| Technical lag | Too many instantiated voxel parts | Instance/CPU profiling | Sparse chunk renderer or Terrain benchmark |
| X-ray exploiting | Secret ore and player data leaked | Inspect client model / remote payloads | Server-only secret grid; limited reveals |
| Extraction camping | One exit, perfect chokepoint | Multiple route playtests | Two gates, cover, unlock timing, anti-camping layout |
| Death too punishing | Players leave after first loss | Interviews, repeated-run metric | 40% salvage by default, first-run protection |
| Pay-to-win perception | Sales alter player combat | Cosmetic-only launch | Separate fair PvP progression |

## 8. What we need from a human creator after MVP

Once the agent proves it can actually build a reliable cave + PvP + extract:
- Choose `Arcade and accessible` versus `Hardcore and high-risk` for loss/TTK.
- Choose `Solo only` or `Solo + Duos` for first release.
- Choose `Stylized neon cave` vs `Brighter earth adventure` from Studio screenshots on phone.
- Choose target development budget, team, quality bar and content cadence.
- Review pricing/policy separately, not as a substitute for retention.
