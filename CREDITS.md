# Credits

**Mission Control** was made by **Team Pawstronauts** for the **NASA Space Apps Challenge 2026**.

## Made with
- **[Godot Engine 4.7](https://godotengine.org)** (MIT license).
- **Claude (Anthropic)** as an AI assistant. See [AI_DISCLOSURE.md](AI_DISCLOSURE.md).
- Python with **reportlab** (the handbook PDF), **Pillow** (medals) and **NumPy + SciPy** (sounds).

## Art
| What | Where | Made by |
|---|---|---|
| Cat astronaut portraits | `assets/images/roles/` | the team's artist |
| Menu backgrounds and title art | `assets/images/` | the team (AI-generated / AI-assisted) |
| Part pictures, ship drawing, orbit view, mission patch | drawn in code (`ui/`) | the team, AI-assisted |
| Medals | `assets/images/medals/`, made by `tools/make_medals.py` | the team, AI-assisted |
| App icon | `icon.png`, `icon.ico` | built from the cat portraits |

*Team: check this table against `data/credits.json` (the game's Credits screen reads that file) and fix anything that isn't right.*

## Sound
Every sound effect and the music are **generated from maths** by `tools/make_sfx.py`: no third-party audio.
The radio beep imitates NASA's Apollo-era **Quindar tones** (2525 Hz and 2475 Hz).

## Fonts
Godot's default font. To use others, drop open-licence fonts (for example Fredoka and Share Tech Mono, both SIL OFL)
into `assets/fonts/` and list them here with their licence.

## Ideas and science
All numbers are **game-scale** and only loosely inspired by real missions (see `real_analog` in
`data/countdown_data.json`). They are **not** official NASA data.

Ideas and structure came from public learning material:
- NASA, *Design and Develop Science Missions* webinar
- NASA, *CubeSat 101*
- NASA Goddard, *Mission Design Lab*
- NASA, *Learning Game About Telescopes*
- NASA JPL, *Concept Maturity Levels* paper
- Canadian Space Agency, *RADARSAT Constellation* game

Real missions that inspired parts and orbits include Landsat, Sentinel-1, TRMM and GPM, Himawari and GOES, LRO,
Lunar Pathfinder and Queqiao-2, Gateway, New Horizons, Galileo, Juno, and the Philippines' Diwata microsatellites.

## Game design inspiration
*Keep Talking and Nobody Explodes* (talk-to-survive asymmetric information), *Among Us* (quick mini-tasks),
*Regicide* (resources that wear down), *The Mind* (playing in order without talking).
