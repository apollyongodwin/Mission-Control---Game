# Making mods for Mission Control

A mod is one `.json` file that adds new things to the game, or changes existing ones, without touching the code.
Start from `modding/example_mod.json`: copy it, rename it, change it.

## Where mod files go
- **Easiest to share:** a folder called `mods` next to `MissionControl.exe`. Zip the game folder with it.
- **Or your user folder:** Settings > **Mods** opens it (on Windows: `%APPDATA%\Godot\app_userdata\Mission Control\mods`).

Restart the game after adding or changing a mod. Settings > Mods shows what loaded and any problems.

**Every laptop in one game must have the same mods.** The version check compares a fingerprint of all mod
files, and Mission Control refuses a laptop with different mods (the message says so). To play without mods,
start the game with `-- --no-mods`.

## What a mod can contain
```json
{
  "mod": {"name": "My mod", "author": "Me", "version": 1},
  "instruments": [ ... ],
  "parts": {"bus": [ ... ], "panel": [ ... ], "battery": [ ... ], "comm": [ ... ],
            "adcs": [ ... ], "obc": [ ... ], "shield": [ ... ], "prop": [ ... ]},
  "rockets": [ ... ],
  "stations": [ ... ],
  "events": [ ... ],
  "actions": [ ... ],
  "twists": [ ... ],
  "orbits": [ ... ],
  "scenario_orbits": {"bagyo_watch": {"my_orbit_id": {"coverage": {...}, "min_life_days": 1095, "target_life_days": 2555, "budget_mult": 1.0}}}
}
```

## The rules
1. **Copy a real item first.** Open `data/countdown_data.json`, copy an item of the same kind, then change the
   `id` and the numbers. A new item must have every field the original has, or it is refused with a message
   like `'my_camera' is missing mass, power_w`.
2. **New `id` = a new item. An `id` that already exists = you replace that item** (handy for rebalancing).
3. **Instruments** need `"scenario"` (`bagyo_watch`, `moon_relay` or `deep_probe`) and `values` that use that
   mission's goal tags (Bagyo Watch: `typhoon_track`, `flood_map`, `rain_wind`).
4. **Crises** (`"kind": "crisis"`) need `threat` tags from: `radiation, power, data, comms, thermal, debris,
   software, budget`. The tags decide which crew gauges go on ALARM, so pick a mix no other crisis uses
   (the handbook's crisis finder matches crises by their alarms). Check that two crew cards can block it fully:
   the Commander's handbook names the smallest set of cards that blocks 100%.
5. **Twists** look like `{"id", "name", "blurb", "effect"}`. Effects the game understands:
   `budget_mult` (0.9 = 10% less money), `extra_crises` (1), `crisis_time_mult` (0.8 = shorter clocks),
   `bonus_tokens` (1), `bonus_lives` (1), `task_chance` (0.75), `force_event` (a crisis id).
6. **A new orbit** needs an entry in `orbits` AND a place in a mission (`scenario_orbits`).
7. Keep text short and kind: `blurb` is one plain sentence, `learn` is why it matters.

## Checking your mod
- Settings > Mods lists every loaded mod and every problem in plain words.
- The game runs the same maths on modded items as on the real ones (SimEngine is never changed by mods).
- Developers: `tests/round8_test.tscn` checks that `modding/example_mod.json` still loads cleanly.
