# Mission Control

**Every real spacecraft is a negotiation. Here is one you can play.**

Mission Control is a cooperative LAN game about designing and flying a space mission, made by **Team Pawstronauts**
for the **NASA Space Apps Challenge 2026**. Four crew members each build part of one spacecraft; a fifth player,
the **Commander**, holds the handbook. Nobody can win alone: the crew has the ship, the Commander has the answers,
and the only way through is to talk.

> Built with **Godot 4.7 (stable)**. Kid-friendly: start on **Cadet** with **Bagyo Watch**.

---

## Contents
- [What it is](#what-it-is)
- [Download and play](#download-and-play)
- [Playing over a LAN](#playing-over-a-lan)
- [Run from source](#run-from-source)
- [Project layout](#project-layout)
- [Tests](#tests)
- [Mods](#mods)
- [Documentation](#documentation)
- [Credits and AI use](#credits-and-ai-use)
- [License](#license)

---

## What it is

| Seat | Who | Picks |
|---|---|---|
| **FLIGHT** | Flight Director | orbit, engines, pointing |
| **SYSTEMS** | Spacecraft Engineer | body, solar wings, battery, computer, shielding |
| **PAYLOAD** | Science Lead | the instruments that do the mission's job |
| **GROUND** | Operations and Budget | rocket, antenna, ground stations |
| **COMMANDER** | the host laptop | sees the budget, the ship's health and the handbook |

**A mission:** read the briefing, then build the ship against the clock. Everyone votes GO or NO-GO, and you
launch. In flight, crises hit: each crew member reads their gauges to the Commander, the Commander finds the
crisis in the handbook and says which cards to play, and the crew spends **tokens** (the spare power, fuel, money
and time they built in) to save the ship. The debrief shows a score, a grade, medals, who used your data and
a mission patch.

**What makes it different**
- **Asymmetric information** (inspired by *Keep Talking and Nobody Explodes*): the crew can't see if their picks are
  right; the Commander can, but can't see the gauges. You have to talk.
- **Margins become tokens:** the spare room you design in is what you spend to survive crises.
- **Real stories:** *Bagyo Watch* is a typhoon and flood satellite for the Philippines; *Moon Relay* serves the lunar
  south pole; *Deep Probe* goes to Jupiter. Orbits are drawn as real Kepler orbits.
- **Every failure explains itself:** the debrief says what limited the mission and why.
- **Variety:** lives, surprise mini-tasks, a random mission twist each game, three difficulties, and mods.

The full player guide is in **[HOW_TO_PLAY.md](HOW_TO_PLAY.md)**.

## Download and play

1. Go to **[Releases](../../releases)** and download `MissionControl_Windows.zip` (Windows 10/11, 64-bit).
2. Unzip it on **every** laptop. Everyone must run the **same release**; a different copy is refused when it tries to connect.
3. Print `Commander_Handbook.pdf` for the Commander (optional: the handbook is also in the game).
4. On the host laptop: **Host a Mission**. On the crew laptops: **Join as Crew**, pick a seat, press **READY**.
5. The host presses **INITIATE LAUNCH**. Empty seats are flown by AutoCrew bots, so 1 to 4 crew all work.

**First launch on Windows:** if SmartScreen says *Windows protected your PC*, click **More info > Run anyway** (the
game isn't code-signed). When the firewall asks, allow **Private networks**.

## Playing over a LAN

- All laptops on the **same Wi-Fi**, set to **Private** (Settings > Network & internet > Wi-Fi > your network).
- The game uses **UDP ports 7777** (game) and **7778** (finding games on the network).
- School or venue Wi-Fi often blocks laptops from talking to each other. A **phone hotspot or travel router** fixes it.
- If a crew laptop can't see the host, type the address shown in the host's lobby. *Crew can't connect?* in the lobby
  lists more tips, and **Settings > System check** makes a report you can send to the Lead.
- No network at all? Run the host and crew windows on one laptop (see below).

## Run from source

1. Install **Godot 4.7 stable** (the exact version: other versions are refused by the version check).
2. Clone this repository and open `project.godot` in Godot (**Import**).
3. Press **F5**.

**Testing on one laptop:** *Debug > Customize Run Instances*, enable 5 instances and give them these arguments:

| Instance | Arguments |
|---|---|
| 1 (host) | `--host` |
| 2 to 5 (crew) | `--auto-join=127.0.0.1 --role=flight` (or `systems`, `payload`, `ground`) |

Host extras: `--scenario=moon_relay --difficulty=engineer --length=full --build=180 --demo --no-twists --autostart=10`.
Add `--no-mods` to ignore mods.

**Export a Windows build:** install the Godot 4.7 export templates, then
`godot --headless --export-release "Windows Desktop" build/windows/MissionControl.exe`.
Upload the build to a GitHub Release (the `build/` folder is not committed: the .exe is over GitHub's 100 MB limit).

## Project layout

```
autoload/      Data, MissionState, Net, Audio (the only four autoloads)
sim/           SimEngine (design maths), FlightDirector (crises), BotCrew, ShipDoctor (advice), ReportFormat
net/           PhaseMachine (host-side game flow), Protocol, Discovery (LAN beacon), NetInfo
ui/menu/       main menu, lobby, settings, past missions
ui/roles/      the four crew panels
ui/main/       host and crew screens, the Commander console and handbook
ui/broadcast/  the big Mission Control screen: ship, orbit view, ticker, announcer
ui/common/     shared pieces: cards, crisis panel, mini-tasks, lives, chat, theme
ui/debrief/    debrief, mission patch, saved missions
data/          countdown_data.json (every number and text), kid_text.json, credits.json
handbook/      the Commander's Handbook (PDF + its source data)
modding/       an example mod
tools/         handbook, sound and medal generators
tests/         automated tests and screenshot helpers
```

More detail: **[ARCHITECTURE.md](ARCHITECTURE.md)**.

## Tests

Run from the project folder (`godot` = your Godot 4.7 executable):

```
godot --headless --path . -s res://tests/golden_test.gd -- --no-mods        # design maths: 12 of 12
godot --headless --path . -s res://tests/load_all_test.gd                    # every script and scene loads
godot --headless --path . -s res://tests/flight_test.gd                      # crises and bots
godot --headless --path . res://tests/phase_test.tscn -- --no-mods           # game flow
godot --headless --path . res://tests/commander_test.tscn -- --no-mods       # Commander mode
godot --headless --path . res://tests/round8_test.tscn -- --no-mods          # lives, tasks, twists, mods, team info
godot --headless --path . res://tests/autocrew_mission_test.tscn -- --no-mods
```

Without a terminal: open a test scene in Godot and press **F6**. Every test prints `PASS` lines and `ALL GOOD`.
See **[CONTRIBUTING.md](CONTRIBUTING.md)** for the full list and the rules.

## Mods

Add your own instruments, parts, rockets, stations, crises and twists with a `.json` file, no code needed.
Start from `modding/example_mod.json` and read **[MODDING.md](MODDING.md)**.
Every laptop in a game must have the same mods.

## Documentation

| File | For |
|---|---|
| [HOW_TO_PLAY.md](HOW_TO_PLAY.md) | players and teachers |
| [MODDING.md](MODDING.md) | anyone making new content |
| [CONTRIBUTING.md](CONTRIBUTING.md) | developers: setup, rules, tests |
| [ARCHITECTURE.md](ARCHITECTURE.md) | developers: how the code fits together |
| [CHANGELOG.md](CHANGELOG.md) | what changed in each version |
| [CREDITS.md](CREDITS.md) | sources, art, sound, fonts |
| [AI_DISCLOSURE.md](AI_DISCLOSURE.md) | how AI was used |
| [MERGE_NOTES.md](MERGE_NOTES.md) | the Lead's detailed development log |

## Credits and AI use

Made by **Team Pawstronauts**. Numbers are game-scale and only loosely inspired by real missions; they are **not**
official NASA data. Sources and assets: **[CREDITS.md](CREDITS.md)**.

**AI assistance:** we used Claude (Anthropic) to help write code, check the maths, balance numbers, draft text and
build the handbook. Every script has an "AI-assisted" header, every screen says "Made with AI assistance", and the
main menu has **Credits & AI use**. Details: **[AI_DISCLOSURE.md](AI_DISCLOSURE.md)**.
