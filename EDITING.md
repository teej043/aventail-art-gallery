# Editing the gallery

The site is static: `index.html` reads `roster.json` and draws everything. No build step.

## Structure
- `categories[]`: each has `id`, `name`, `slots` (slot order) and `units[]`. Start: `characters` (Characters & Creatures). Add icons, backgrounds or tiles as new categories with their own `slots` list.
- Unit: `id` (used in the URL `#/characters/<id>`), `name`, `kind` (`character` or `creature`), `slots`.
- Slots: `portrait`, `idle_se/nw`, `move_se/nw`, `hurt_se/nw`, `attack_se/nw`, `cast_se/nw`, `skill_se/nw`.
- Each slot: `status` (`done` | `partial` | `pending`) and `versions[]`, oldest first, newest last:
  `{"label":"v3","file":"files/<unit>/<name>.gif","date":"2026-11-04","note":"short note"}`

## Status meanings
- done: in the game codebase and polished
- partial: imported into the codebase, not polished or incomplete
- pending: not in the game yet

The unit status is computed: done if every slot is done, partial if any slot is done or partial, otherwise pending.

## Common edits
- New version: copy the file into `files/<unit>/`, append a version entry at the end of the slot's `versions`.
- Polished in game: change the slot `status` to `done`, ideally adding the polished file as the newest version.
- New unit: copy an existing unit block and change `id`, `name`, `kind`; empty slots are `{"status":"pending","versions":[]}`.

Check that the JSON is valid (e.g. `python3 -m json.tool roster.json`), then commit and push; Pages updates in about a minute.
