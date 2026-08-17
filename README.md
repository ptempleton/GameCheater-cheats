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

## Definition types

- `freeze`: write or hold a typed value. Set `resolveEachTick` for moving object chains.
- `patch`: replace code bytes; the client saves and restores the original bytes.
- `composite`: transactionally toggle concrete cheats named in `members`.

A composite may set `"hideMembers": true`. Supporting clients then show only the composite row
while retaining every member internally for enable, rollback, disable, and teardown. Member names
are case-insensitive and must resolve to concrete cheats in the same definition.

Composite/runtime/schema features must ship in the client before a definition depends on them.
Normal pointer, value, description, and member-list updates require only a definition revision.

## SnowRunner

`games/snowrunner.json` revision 6 contains:

- **Infinite Fuel**
- **No Vehicle Damage (except tires)**, a visible composite over engine, transmission, fuel-tank,
  and suspension accumulator freezes
- Four concrete damage cheats hidden by the composite

Tires are not included. Their authoritative per-tire storage is still unknown, so the master must
retain the **except tires** label.

When changing SnowRunner:

1. Update `games/snowrunner.json`.
2. Bump its top-level `revision`.
3. Bump the SnowRunner revision in `index.json` to the same number.
4. Validate both JSON files and `schema.json`.
5. Open a PR. Do not merge without owner approval.
