# MISSION CONTROL

**Every real spacecraft is a negotiation. Here is one you can play.**

Mission Control (formerly COUNTDOWN) is a cooperative 4-player LAN game about designing a space mission. Four players sit at four laptops
(FLIGHT, SYSTEMS, PAYLOAD, GROUND). Each sees only part of the numbers, so they have to talk to make one spacecraft fit
one rocket and one budget. Then they launch it and survive crises using the spare margins they built in.
A fifth screen, **Mission Control**, shows the whole thing like a live broadcast.

Team **Pawstronauts**, NASA Space Apps Challenge 2026.

## New in MC-9.0
- Every crew screen shows the **mission goals** (how full each one is, in our orbit) and the **whole team's picks**.
  Orbit cards say what each orbit sees; instrument cards say how much they help in the chosen orbit.
- Science is fairer: a good design now reaches about 75-80.
- Crew screens fit: one decision per page, cards in one row with arrows, nothing cut off.
- AI use is noted everywhere: see **Credits & AI use** and `AI_DISCLOSURE.md`.

## New in MC-8.0
- **Lives:** Cadet 3, Engineer 2, Flight Director 1. A crisis mistake (a card that doesn't help, or blocking less than
  half) costs one. No lives left = the mission is called off.
- **Mini-tasks:** surprise tasks for everyone, and a task for whoever taps CHIP IN on a lucky chance (asteroids, wiring,
  binary, antenna tracing). Each finished task earns a token.
- **Mission twists:** one surprise rule per game (lobby switch). **Build time** is chosen in the lobby.
- **The budget depends on the orbit.** Overtime costs money AND FLIGHT's time tokens.
- **The Commander's screen says exactly what to swap and why**, plus a science forecast. The handbook has new
  Ground stations, Science and Tips pages.
- **Real orbits** on the broadcast: Kepler timing, a spinning tilted Earth, geostationary parked over the Philippines.
- **Mods:** add your own parts, instruments, crises and twists. See `MODDING.md` and `modding/example_mod.json`.
- The game version is shown at the bottom of every screen.

## Mods
Put mod `.json` files in a folder called `mods` next to `MissionControl.exe` (or Settings > Mods), then restart.
Every laptop must have the same mods, or Mission Control refuses to connect. How to write one: `MODDING.md`.

## Run it

1. Install **Godot 4.7 stable** (the whole team must use the same version).
2. Open `project.godot` in Godot and press **F5**.
3. One laptop presses **Host Mission Control**. The others press **Join as Crew**, pick the game from the list
   (or type the address shown on Mission Control), choose a seat and press READY.
4. Mission Control presses **INITIATE LAUNCH**. Empty seats are flown by AutoCrew bots, so 1 to 4 players all work.

Start with **Bagyo Watch** on **Cadet**.

### Testing on one laptop
Debug > Customize Run Instances: enable multiple instances (5) and give them these arguments:
`--host` for one, and `--auto-join=127.0.0.1 --role=flight` (or `systems`, `payload`, `ground`) for the others.
Host extras: `--scenario=moon_relay --difficulty=engineer --length=full --demo --autostart=5`.

### LAN tips
Port **7777** (game) and **7778** (finding games). Venue wifi often blocks laptop-to-laptop traffic, so bring a travel
router or phone hotspot. Allow Godot through the Windows firewall the first time.

## How a mission goes
Lobby > Briefing (your secret goal) > Design (pick parts; the coach bubble tells you the one next thing to fix) >
Design Review (everyone answers GO or NO-GO) > Launch > Flight (crises: play cards, hand tokens to teammates) >
Debrief (grade, medals, who used your data, mission patch). Then press **Again!**

## Chat
Press **T** (or the CHAT tab) on a crew laptop. Quick phrases always work; Mission Control can switch to
"quick phrases only" in the lobby. Chat is muted during Radio Silence crises: use the ping wheel.

## Tests
See `MERGE_NOTES.md` for the full list. Quick checks:
`godot --headless -s res://tests/golden_test.gd` and `godot --headless -s res://tests/load_all_test.gd`.

## Data and credits
All numbers are game-scale and only loosely inspired by real missions (see `real_analog` in `data/countdown_data.json`).
They are **not** official NASA data.

Ideas and structure from: NASA *Design and Develop Science Missions* webinar; NASA *CubeSat 101*;
NASA Goddard *Mission Design Lab*; NASA *Learning Game About Telescopes*; JPL *Concept Maturity Levels* paper;
CSA *RADARSAT Constellation* game.

Art: menu and lobby artwork by the team (Dev D). Crew portraits: the four cat astronauts by the team's artist
(`assets/images/roles/`). Sound effects and music: generated from code by the team (`tools/make_sfx.py`), no third-party audio.
The "Quindar tones" chat beep imitates NASA's Apollo-era radio signalling (2525 Hz / 2475 Hz).
Fonts: Godot's default font (add any SIL OFL fonts here with their licence).

**AI assistance:** the team used Claude (Anthropic) to help write code, merge modules, check the maths, balance numbers,
draft text and build the handbook. Every script has an "AI-assisted" header, every screen says "Made with AI
assistance", and the main menu has **Credits & AI use**. Full details: `AI_DISCLOSURE.md` (edit `data/credits.json`
so it is true for your team before you submit).


## Commander mode (default)

5 people: 4 crew on their laptops plus the **Commander** on the host laptop. Keep the Commander's screen private!
The crew can't see whether their picks are right; the Commander can, and taps a pick to flag it SWAP. In a crisis the
crew says which gauges are on ALARM; the Commander taps them into the Gauge Finder and says which cards to play. The crew has
the clock and can press **HURRY, COMMANDER!**. Print `Commander_Handbook.pdf` for a paper copy.

## Playing on other laptops (read this before sending the game around)

**Send the built game (`MissionControl.exe`), not the project folder.** If someone opens the project in their own Godot, their Godot version, editor settings and the editor's "Game" tab all change how it behaves. Full screen, for example, can't work inside the editor's Game tab. Every laptop should run **the same build**. A different copy gets refused with a "different version" message.

1. **Same Wi-Fi network.** School or guest Wi-Fi often blocks laptops from talking to each other ("client isolation"). If it does, turn on a phone hotspot and put every laptop on it.
2. **First launch on Windows:** if SmartScreen says "Windows protected your PC", click *More info* then *Run anyway* (the game isn't code-signed). When the **firewall** box appears, tick **Private** networks and click *Allow*.
3. Set the Wi-Fi to **Private**: Windows Settings > Network & internet > Wi-Fi > (your network) > *Private network*. On a "Public" network the firewall blocks the game.
4. The host reads out the address shown in the lobby. If the crew can't see the host automatically, they type that address. If it still fails, try the "or try" addresses.
5. Still stuck? Open **Settings > System check** on both laptops and send the reports to the Lead.
