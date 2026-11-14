# Architecture

How Mission Control fits together. Godot 4.7, typed GDScript.

## The big picture

```
            crew laptops (clients)                          host laptop (authority)
  ┌──────────────────────────────────┐         ┌───────────────────────────────────────────┐
  │ CrewScreen                       │ request │ Net ──> PhaseMachine (game flow, timers)   │
  │   role panel (FlightPanel, ...)  │ ──────> │          │                                 │
  │   crisis panel, mini-tasks, chat │         │          ├─> SimEngine.evaluate (design)   │
  │                                  │ <────── │          ├─> FlightDirector (crises)       │
  │ MissionState = FILTERED copy     │  state  │          └─> BotCrew (empty seats)         │
  └──────────────────────────────────┘         │ MissionState = the TRUE copy               │
                                               │ HostScreen: Commander console + broadcast  │
                                               └───────────────────────────────────────────┘
```

- **Host-authoritative.** The host (peer 1) runs every rule. Clients only send requests; the host checks each one
  (the sender owns that choice, the value exists in the data, lists are a sane size) and sends the state back.
- **Privacy is enforced on the host.** Each client gets only `SimEngine.role_view(...)` for its own seat. Crew screens
  never receive the full `evaluate()` result.
- **Everything is data-driven.** Parts, orbits, crises, cards, twists and text live in `data/countdown_data.json`.
  Mods merge into the same data at start-up.

## Autoloads (the only four)

| Autoload | Job |
|---|---|
| `Data` | loads `data/countdown_data.json` (+ kid text + mods) into `Data.db`; lookup helpers |
| `MissionState` | the shared picture: design, phase, seats, tokens, lives, twists, the evaluate result (host only). Emits signals for the UI |
| `Net` | ENet host/join on port 7777, the RPCs, the version handshake, discovery |
| `Audio` | sound effects, music per phase, optional announcer voice; also draws the version/AI tag on every screen |

## Game flow (`net/PhaseMachine.gd`, host only)

`LOBBY -> BRIEFING -> DESIGN -> REVIEW -> LAUNCH -> FLIGHT -> DEBRIEF` (any NO-GO in REVIEW goes back to DESIGN).

- **BRIEFING:** bots take empty seats, a mission twist is rolled, the build clock starts.
- **DESIGN:** every accepted choice re-runs `SimEngine.evaluate`; MissionState then applies budget changes (orbit,
  twist, investors) after the formula. Bots pick their parts; the Commander can flag parts SWAP.
- **REVIEW:** the launch poll (four crew + the Commander).
- **LAUNCH:** a seeded reliability roll (never fails on Cadet or in Demo Mode).
- **FLIGHT:** `FlightDirector` (through `FlightDirectorAdapter`) deals a seeded deck of crises, opportunities and a
  decision. PhaseMachine owns the timers, lives, mini-tasks and token hand-overs.
- **DEBRIEF:** `final_report()` -> `ReportFormat.normalize()` -> every screen.

## The simulation (`sim/`)

| File | Job |
|---|---|
| `SimEngine.gd` | pure static design maths: mass, power, data, lifetime, science, cost, schedule, score, tokens, lights. A line-for-line port of `reference_sim.py`, checked by the golden tests |
| `FlightDirector.gd` | crises and scoring in flight: deck, card play, mitigation, final report, medals |
| `FlightDirectorAdapter.gd` | fits FlightDirector to PhaseMachine's contract; twists, auto-play for bots |
| `BotCrew.gd` | AutoCrew bots for empty seats (design picks and flight plays) |
| `ShipDoctor.gd` | plain-language advice: what to fix and why, science leaks, ground-station reach, the crew's goal meter and team box (built from public info only) |
| `ReportFormat.gd` | one report shape for every screen |

Crisis maths: `blocked = sum over threat tags of threat[t] * min(1, sum of the played cards' mitigates[t])`.
The handbook's advice (`RoleGuide.calls`) searches for the smallest set of cards that blocks 100%.

## Networking (`autoload/Net.gd`, `net/`)

Client -> host: `request_seat`, `submit_choice`, `set_seat_ready`, `call_review`, `poll_answer`, `play_action`,
`give_token`, `ping`, `event_choice`, `send_chat`, `hello`.
Host -> client: `sync_state` (public state + that seat's private view), `phase_changed`, `crisis_started`,
`crisis_resolved`, `debrief`, `server_message`, `chat_message`.

- **Version handshake:** on connect the client sends `MCV|<GAME_VERSION>|<Godot version>|mods:<fingerprint>`; a
  mismatch is refused with a clear message. Bump `GAME_VERSION` whenever an RPC or the data changes.
- **Small messages ride existing RPCs:** mini-tasks use `server_message("task_start", ...)` and `ping("@task:ok")`;
  token requests use `ping("@ask:<token>")`; HURRY uses `ping("@commander")`.
- **Discovery:** the host broadcasts `MISSIONCONTROL|<name>|<ip>|7777` on UDP 7778 every second.
- **Dropped players:** their seat is held for 60 seconds and they can rejoin.

## Screens (`ui/`)

| Folder | Screens |
|---|---|
| `menu/` | MainMenu, Lobby (host and crew views), Settings (incl. System check and Mods), PastMissions |
| `main/` | HostScreen (Commander console, Tab = broadcast view), CrewScreen (briefing, role panel, flight view, debrief), CommanderConsole, CommanderHandbook, GaugeFinder |
| `roles/` | `RolePanel` base + Flight/Systems/Payload/Ground panels: one decision per page, cards in a row, goals and team boxes |
| `broadcast/` | BroadcastMain (16:9 show), ShipSchematic, OrbitView (3D Kepler orbits, spinning tilted Earth), Ticker, Announcer |
| `common/` | PartCard, CrisisPanel, MiniTask, LivesBar, ChatDrawer, TokenTray, RoleGuide, GameTheme, Credits, ... |
| `debrief/` | Debrief, MissionPatch, MissionStore (saved missions) |

All UI is built in code in one house style (`GameTheme.gd`) and must fit a **1280 x 720** canvas
(`canvas_items` stretch, so bigger screens scale up).

## Data (`data/`)

| File | Contents |
|---|---|
| `countdown_data.json` | `meta` (constants, difficulty, roles, tokens, build clock, mini-tasks, twists), `orbits`, `rockets`, `stations`, `rtg`, `parts`, `instruments`, `scenarios`, `actions`, `events`, `bot_presets`, `golden_tests` |
| `kid_text.json` | a 10-word kid line for every item |
| `credits.json` | who made what and what was AI-assisted (shown in Credits & AI use) |

## Tools (`tools/`)
`export_handbook.tscn` + `make_handbook_pdf.py` (the handbook), `make_sfx.py` (every sound, generated from maths),
`make_medals.py` (medal pictures).
