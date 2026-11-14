# AI use in Mission Control

**Short version:** Team Pawstronauts built Mission Control with help from an AI assistant, **Claude (by Anthropic)**.
People on the team decided what the game is, how it plays and how it should feel, play-tested it, and chose which
AI suggestions to keep. The AI helped write and fix code, check the maths, balance numbers, draft text and build
the handbook.

## Where the game says so
- **Every script** (`.gd` and `.py`) starts with:
  `# AI-assisted: written with help from Claude (Anthropic). See AI_DISCLOSURE.md and data/credits.json.`
- **Every screen** shows "Made with AI assistance" next to the version, bottom-right.
- **Screens with the menu background** show "Background: AI-generated / AI-assisted", bottom-left.
- **Main menu > Credits & AI use** lists everything below. It reads `data/credits.json`, so the game, this page and
  the README always agree.
- The **handbook PDF** footer and `meta.ai_note` in `data/countdown_data.json` say so too.

## What was AI-assisted

| Part | AI use | How |
|---|---|---|
| Game code (GDScript), tests, build tools | AI-assisted | Each teammate built their module in chats with Claude using a shared prompt pack and spec; the Lead merged the modules with Claude's help. |
| Design maths (`SimEngine.gd`, `reference_sim.py`) | AI-assisted | Checked by 12 golden tests. |
| Numbers and item text (`data/`) | AI-assisted | Game-scale numbers loosely inspired by real missions; not official NASA data. |
| Commander's Handbook (PDF and in-game) | AI-assisted | Built from the game data by AI-assisted code. |
| Sound effects and music | AI-assisted | Generated from maths by `tools/make_sfx.py`; no third-party audio. |
| Medal pictures, part pictures, ship and orbit drawings | AI-assisted | Drawn by code. |
| Menu backgrounds and title art | AI-generated / AI-assisted | |
| Cat astronaut portraits | Drawn by the team's artist | |

## What people did
The game idea and the Commander (talk-to-survive) design, choosing the missions and roles, the role balance, the
art direction and the cat astronauts, play-testing with real players, deciding which AI suggestions to keep and
which to throw away, and checking the results in play.

## Keeping this page true
If you add or change anything made with AI (code, art, sound or text), update `data/credits.json` and this page in
the same pull request. New scripts start with the AI header line (see [CONTRIBUTING.md](CONTRIBUTING.md)).
