# ANTWARS — GETTING ROBLOX STUDIO AND THE AGENT READY

## 1. Quick launch for a developer using Codex CLI or Claude Code

### Prerequisites
- Windows or macOS, current Roblox Studio installed and logged in.
- One **new/duplicated private** Roblox place dedicated to AntWars.
- Claude Code or Codex CLI installed locally with access to this project folder.
- The official Roblox Studio MCP support enabled.

### Preferred official integration
Roblox publishes **its own plugin for coding agents**:
https://github.com/Roblox/roblox-studio-agent-plugin

**Claude Code:** type `/plugin`, search for **Roblox Studio**, install the official plugin. Use `/mcp` to check the server state.

**Codex CLI:** in terminal run:
```bash
codex plugin marketplace add Roblox/roblox-studio-agent-plugin
```
Then inside Codex open `/plugins`, install **Roblox Studio**, and start a fresh thread with `/new`. `codex mcp list` should show `Roblox_Studio` when configured.

**In Roblox Studio:** open **Assistant** → `...` → `Settings / Manage MCP Servers` → turn on **Enable Studio as MCP server**. In Quick Connect enable your agent (on some versions Codex is labelled `ChatGPT (codex)`), or use the agent/plugin's connection instructions. Restart client if required.

**Connection smoke test:** ask the agent `List my open Roblox Studio windows, tell me the active place name, and inspect its Workspace without changing anything.` The agent must report the actual place, not guess.

Official Studio docs: https://create.roblox.com/docs/studio/mcp

### Alternate direct MCP setup
If the official plugin is unavailable, Roblox documents a built-in `stdio` server with manual configurations. On Windows the default launcher is `%LOCALAPPDATA%\Roblox\mcp.bat`, on macOS `/Applications/RobloxStudio.app/Contents/MacOS/StudioMCP`. Client-specific JSON layouts differ; follow **official docs and the current client setup** rather than copying unsupported fields. Studio must still have MCP enabled.

## 2. Giving the agent this project

**Option A — best:** unzip `ANTWARS_AGENT_STARTER_PACK.zip` into a new project directory, open that directory in Claude Code or Codex, open the separate Roblox Studio place, connect MCP, and send:

> Read `START_HERE_PROMPT.md`, `docs/ANTWARS_GDD.md`, `docs/TECHNICAL_ARCHITECTURE.md`, `docs/VISUAL_DIRECTION.md`, `docs/BUILD_AND_TEST.md`, and `docs/TASKS.md`. Review the two images in `assets/reference/`. You are allowed to build and test AntWars in the current private Roblox Studio place via official MCP. Start with milestone M0, then immediately implement M1 (real mining) and test it. Continue toward M2/M3 if available. Do not publish or spend Robux. Report only verified work.

**Option B — single prompt:** attach/paste the standalone `ANTWARS_MASTER_PROMPT.md` and the 3 PNG images. The master file contains the full instructions and documents in one place.

## 3. First proof-of-life checks

Immediately after connection agent should verify:
1. Studio open and not in Play mode (or deliberately stop active test before edits).
2. Expected place, and no user-authored objects will be destroyed.
3. Current game tree and scripts inspected, not hallucinated.
4. A tiny test module/script/object is created in a harmless AntWars-prefixed namespace.
5. Play mode starts and output is readable.
6. A screenshot of the scene is retrievable (if MCP tool available).

If the agent cannot see the place, it must troubleshoot connection, not invent world changes.

## 4. Safety, backup and IP

Create a **backup or version** of any existing Roblox place before agent changes. Agent must never publish publicly, spend Robux, enable live DataStore API, install random marketplace packages or alter unrelated projects without consent. Prefer self-authored Parts, scripts and simple UI. If using Creator Store assets, check creator/rights and avoid suspicious scripted models. Disable/inspect scripts inside third-party assets; don't copy copyrighted titles or character designs. Generate original geometry.

## 5. Studio testing basics

Roblox Studio normally provides solo Play, client/server tests and device emulation. Solo Play does **not** prove PvP. A full claim of M2 requires **two simulated/real clients**; this can be initiated with Studio test modes or supported `StudioTestService` flows. The built-in MCP `start_stop_play` often launches a single Play Client, so use additional supported tooling for multiplayer and report precisely which scenario ran.

Developer checklist:
- On PC: mouse/keyboard mining, combat, extraction and UI.
- On mobile emulator: touch buttons, camera, overlays and legibility.
- After starting: retrieve Roblox Output; fix errors before claiming a milestone.
- For mining: test two quick requests for same block and spam/out-of-range coordinates.
- For extraction: trigger repeated start/complete and exit zone during countdown.
- For dying: reward math, loot bag and repeated claim attempts.
- For persistence: test with **mock store** or separate private test universe, not production stores.

## 6. Game acceptance playtest script

```
A. Launch from clean place (new match/debug solo), capture scene.
B. Spawn underground facing mineable stone.
C. Mine a dirt and copper block with desktop input.
D. Verify one visible physical tunnel and +resource in server-authoritative HUD.
E. Attempt invalid MineRequest (far away/noninteger/spam): server denies and does not award loot.
F. Spawn 2nd test client; spawn chambers differ.
G. Client A mines toward Client B. Both see same hole.
H. Both reach visible mutual encounter; verify footsteps/mining audio.
I. Client A hits B at valid range. Attack through intact wall denied.
J. B dies; loot bag appears; duplicate bag claim rejected.
K. A navigates to unlocked extraction, channels, banks ore.
L. Attempt duplicate extraction: no second currency credit.
M. See results, buy tier 2 pickaxe, requeue and compare hits needed.
N. Open mobile device emulator, repeat C and K with touch.
O. Inspect output and screenshot; document failures and real measurements.
```

## 7. Common setup blockers

- **Agent can't see Studio:** enable MCP in Studio, check Quick Connect, restart both app and agent. Don't confuse web ChatGPT with desktop/client MCP connectivity.
- **MCP connected but no edit tools:** verify plugin permissions, agent connection state and tool list.
- **Play starts but errors:** ask agent for actual console traces and targeted fixes, not a complete rewrite without diagnostics.
- **No multiplayer automated tool:** execute Studio multi-client testing manually or with `StudioTestService` if supported; report M2 as unverified until real observed sync test.
- **Asset tools inaccessible:** use primitive Parts and native UI; production-grade AI meshes are not a blocker for M1.

## 8. References and documentation dates

Prepared 12 October 2026. Tools, UI names and defaults may change. Refer to official, current Roblox docs instead of assuming static third-party MCP packages:
- https://create.roblox.com/docs/studio/mcp
- https://github.com/Roblox/roblox-studio-agent-plugin
- https://create.roblox.com/docs/studio/testing-modes
- https://create.roblox.com/docs/reference/engine/classes/StudioTestService
- https://create.roblox.com/docs/scripting/security/client-server-boundary
