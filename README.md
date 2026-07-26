# GameCheater-cheats

Authored trainer definitions for **GameCheater**. The app pulls `index.json`, then the
`games/*.json` files it lists, caches them locally, and shows each game's cheats. Push a fix
here and every client picks it up on next launch (or via the in-app **Refresh** button) — no
app release needed.

## What belongs here

- **Only original authored definitions** — signatures / pointer chains / patch bytes we found
  by scanning the game ourselves. These are facts about the game, authored in our own format.
- **Never** third-party Cheat Engine `.CT` tables, and **never** trainer binaries. Those are
  bring-your-own and stay local on a user's machine.
- Single-player / offline targets only.

## Structure

```
index.json            # games + revisions + file paths
games/<game>.json     # one TrainerDefinition per game
schema.json           # the definition shape
```

## Adding / updating a game

1. Find the cheats (GameCheater's watcher / scanner) → it emits a `games/<game>.json`.
2. Add or update that file here, and bump its `revision` (and the entry in `index.json`).
3. Commit. Clients refresh and pick it up.

See `games/example.json` for a template and `schema.json` for the full shape.
