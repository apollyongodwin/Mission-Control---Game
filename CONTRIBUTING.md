# Contributing to Mission Control

Thanks for helping! This page is for the team and anyone who wants to change the game.

## The Team
Mission Control is made by:
- Sam Ezra K. Capangpangan
- Marjah Sheham M. Sarip
- Yohann P. Domingo
- Klyne Xyril M. Africano
- Julliana Arlyn A. Virtudazo
- Noril Agustin O. Encabo

### Very Special Mentions
- **Rihanna Elisse C. Sani**: the character designs were commissioned from and created by her.

## Setup
1. Install **Godot 4.7 stable**. Everyone must use exactly this version: the network version check refuses mixed copies.
2. Clone the repo and open `project.godot` in Godot (**Import**). The first import takes a minute.
3. Run the golden test (below). If it isn't 12 of 12, stop and ask the Lead.

## Branches
- One branch per person: `name/feature` (for example `kim/crisis-panel`).
- **Only the Lead merges to `main`.**
- Never have two people editing the same `.tscn` scene at once: scene files merge badly.
- Pull before you start, push before you leave.

## Rules that keep the game working
1. **The maths lives in `sim/SimEngine.gd` and nowhere else.** Its formulas match `reference_sim.py` (the source of
   truth). Never change a formula without the Lead; run the golden test after anything near the sim or the data.
2. **Numbers live in `data/countdown_data.json`, never in code.** Never rename an id or a JSON key.
   Changed numbers that the golden tests depend on? Regenerate their `expect` values with `reference_sim.py` and say so in your pull request.
3. **The host decides, clients ask.** Clients send requests; the host checks them and sends the state back. Never
   trust a client, and never send a crew laptop numbers it isn't allowed to see (`SimEngine.role_view`).
4. **Changed, added or removed an `@rpc` function, or changed the data?** Bump `GAME_VERSION` in `net/Protocol.gd`.
   Otherwise different copies can connect and mix up their messages.
5. **Only four autoloads:** `Data`, `MissionState`, `Net`, `Audio`.
6. **Typed GDScript, snake_case, one class per file, signals for UI updates.**
7. **The UI must fit 1280 x 720** (the game's canvas; bigger screens are scaled up). Check new screens with
   `tests/screenshot_runner.tscn` or the `--shot-dir` launch option.
8. **Kid-friendly words:** short sentences, no jargon without a picture. Kid lines in `data/kid_text.json` stay at 10 words or fewer.
9. **Every new script starts with the AI header** if AI helped write it (it almost always did):
   `# AI-assisted: written with help from Claude (Anthropic). See AI_DISCLOSURE.md and data/credits.json.`
   If you add art, sound or text made with AI, add it to `data/credits.json`.

## Tests
Run from the project folder (`godot` = your Godot 4.7 executable). Each prints `PASS` lines and ends with
`ALL GOOD` (or a total such as `12 of 12`). Without a terminal, open the scene and press **F6**.

| Test | Command | Checks |
|---|---|---|
| Golden | `godot --headless --path . -s res://tests/golden_test.gd -- --no-mods` | the design maths (12 of 12) |
| Load all | `godot --headless --path . -s res://tests/load_all_test.gd` | every script and scene compiles |
| Flight | `godot --headless --path . -s res://tests/flight_test.gd` | crises, scoring, bots |
| Autofill / kid text | `-s res://tests/autofill_test.gd`, `-s res://tests/kid_test.gd` | helpers, kid lines |
| Phase | `godot --headless --path . res://tests/phase_test.tscn -- --no-mods` | the game flow |
| Commander | `res://tests/commander_test.tscn` | Commander mode, build clock |
| Round 8/9 | `res://tests/round8_test.tscn` | lives, mini-tasks, twists, budgets, mods, team info |
| AutoCrew | `res://tests/autocrew_mission_test.tscn` | whole missions with bots, all scenarios |
| Net (offline) | `res://tests/net_offline_test.tscn` | networking without a network |
| Net (two windows) | `res://tests/net_e2e.tscn -- --host`, then `-- --client` | a real host and client |
| Chat (two windows) | `res://tests/chat_e2e.tscn -- --host`, then `-- --client` | crew chat |

Visual helpers: `tests/screenshot_runner.tscn`, `tests/console_shot.tscn`, `tests/orbit_shot.tscn`,
`tests/minitask_shot.tscn` (run with a window, or under `xvfb-run` on Linux).

**A failing golden test stops everything** until it is fixed.

## The Commander's Handbook
The PDF is built from the live game, so it always matches the data:
```
godot --path . res://tools/export_handbook.tscn      # needs a window: writes handbook/handbook.json + pictures
python3 tools/make_handbook_pdf.py                   # needs reportlab: writes handbook/Commander_Handbook.pdf
```

## Releasing a build
1. Bump `GAME_VERSION` if anything network- or data-related changed, and add a `CHANGELOG.md` entry.
2. All tests green.
3. Export: `godot --headless --export-release "Windows Desktop" build/windows/MissionControl.exe` (needs the 4.7 export templates).
4. Test the build on a **different laptop** than the one that built it.
5. Zip `MissionControl.exe`, `Commander_Handbook.pdf`, `README.md`, `MODDING.md`, `AI_DISCLOSURE.md` and
   `modding/example_mod.json`, and attach the zip to a **GitHub Release** (the .exe is too big for the repo itself).

## Pull requests
Say what changed, why, and which tests you ran. Screenshots for UI changes. Keep it small: one feature per pull request.
