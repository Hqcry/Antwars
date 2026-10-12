# ANTWARS — AI Agent Starter Pack

Use with **Codex CLI** or **Claude Code**, connected to **Roblox Studio** using Roblox's official Studio MCP server.

## Start in four actions
1. Back up your Studio place, or create a **new private place** named AntWars Prototype.
2. Follow `docs/BUILD_AND_TEST.md` to install/connect the official agent plugin and enable **Studio as MCP server**.
3. Open this folder as the agent's working directory. Codex reads `AGENTS.md`; Claude Code reads `CLAUDE.md`.
4. Send: **"Прочитай START_HERE_PROMPT.md и все docs/. Посмотри assets/reference/. Подключись к открытому Roblox Studio, создай и протестируй playable AntWars MVP, начиная с M0–M1. Не публикуй игру и не трать Robux."**

## Files
- `КОНЦЕПЦИЯ_ANTWARS_НА_РУССКОМ.md` — краткое объяснение идеи по-русски для владельца проекта.
- `START_HERE_PROMPT.md` — direct executable task for AI agent.
- `ANTWARS_MASTER_PROMPT.md` — standalone complete giant prompt (all design and technical directions collected together).
- `AGENTS.md` / `CLAUDE.md` — agent project instructions.
- `docs/ANTWARS_GDD.md` — product game design and tuning hypotheses.
- `docs/TECHNICAL_ARCHITECTURE.md` — modules, security, networking, grid, performance, persistence.
- `docs/VISUAL_DIRECTION.md` — style, palette, world props and UI layouts.
- `docs/BUILD_AND_TEST.md` — official connection and verification instructions.
- `docs/TASKS.md` — implementation milestones and exit criteria.
- `docs/POST_MVP_PRODUCT_STRATEGY.md` — later retention, cosmetics and growth vision; out of scope for first build.
- `assets/reference/ANTWARS_CONCEPT_ART.png` — original moodboard.
- `assets/reference/ANTWARS_HUD_WIREFRAME.png` — original HUD wireframe.
- `assets/reference/ANTWARS_MAP_SCHEMATIC.png` — original developer-only map schematic.

## Notes
Concept art is an illustration, **not evidence of working gameplay**. The values in GDD (12 players, 12-minute round, damage, loot loss etc.) are configurable design hypotheses; tests should drive changes. The founder asked for **one winner per competitive round**; voluntary extraction banks loot but forfeits the champion title. MVP excludes monetization, real-money assets, paid randomness and player-to-player trade.

The package doesn't include a functioning `.rbxl` game; the connected agent is explicitly tasked with creating and testing that inside your Roblox Studio environment. Its ability to complete milestones depends on working local access, tools, permissions and development/debugging quality. Never trust unsupported “done” claims.
