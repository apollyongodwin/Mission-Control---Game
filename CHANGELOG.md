# Changelog

Every laptop in a game must run the same version. The version shows in the bottom-right corner of every screen.
The full development log is in [MERGE_NOTES.md](MERGE_NOTES.md).

## MC-9.0
- **The crew can plan together.** Every crew screen shows the **mission goals** (how full each one is with our
  instruments, in our orbit), plain hints, and the **whole team's picks**. Orbit cards say what each orbit sees;
  instrument cards say how much they help in the chosen orbit. The Commander gets a mission card and a
  **Mission** handbook tab.
- **Fairer science:** every goal fills 25% sooner. A strong Bagyo Watch design now reaches about 79 (was 65).
- **Crew screens fit:** one decision per page on every difficulty, cards in one row with arrows, the briefing
  always fits, and the version moved to the corner.
- **AI use is noted everywhere:** script headers, a corner notice on every screen, a background notice, the
  **Credits & AI use** screen, the handbook footer and [AI_DISCLOSURE.md](AI_DISCLOSURE.md).

## MC-8.0
- **Fixed: a perfect crew could still get hurt.** The handbook named cards that blocked only 60-87% of four crises,
  and bots played too late when humans finished quickly.
- **Lives:** Cadet 3, Engineer 2, Flight Director 1. A crisis mistake costs one; none left ends the mission.
- **Mini-tasks:** asteroids (FLIGHT), wiring (SYSTEMS), binary (PAYLOAD), antenna tracing (GROUND). They appear by
  chance for everyone, or for whoever chips in on a lucky chance. Each finished task earns a token.
- **Build time** is chosen in the lobby. Overtime costs money and FLIGHT's time tokens.
- **The budget depends on the orbit.**
- **Mission twists** and **mods**.
- The Commander is told exactly what to swap and why, plus a science forecast. New handbook pages: Ground stations,
  Science, Tips, Lives and tasks.
- **Real orbits** on the broadcast: Kepler timing, a spinning tilted Earth, geostationary parked over the Philippines.

## MC-7.0
- Role balance: engines and pointing moved to FLIGHT.
- A build clock for everyone; investors pull money in overtime.
- The Commander can flag single instruments and stations.
- At most 3 tokens of each kind at launch.

## MC-6.0
- Tokens matter: some cards need a teammate's token (ASK and GIVE).
- A see-through crisis sheet that keeps the ship in view; medal pictures; the Helping Paws medal.

## MC-5.0
- A simpler, kid-friendly Commander: one big instruction, one-tap SWAP flags, a Gauge Finder and an illustrated handbook PDF.

## MC-4.0
- **Commander mode** (Keep Talking style): the host laptop is the Commander with the handbook; the crew can't see
  whether their picks are right. MY JOB cards for every seat.

## MC-3.0
- LAN fixes (the right network address, discovery, the version handshake), the rename to **Mission Control**,
  the app icon, part pictures, music and sound effects, System check, and Windows builds.

## Before MC-3.0
- Every module merged into one project and playable from lobby to debrief; crew chat, the cat astronauts, effects,
  the announcer and the broadcast screen.
