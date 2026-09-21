---
name: trip-edit
description: Add or edit data in the Japan trip vault — hotel details, booking/cost status, food/locations/events/shopping suggestions, daily plan, transport legs. Use whenever the user asks to update, change, book, add, fix, or fill in any trip info, even if they don't name a file. Knows which file is the real source of truth per data type, so edits land there instead of in a generated view or a legacy duplicate, and keeps the downstream views that depend on it in sync.
---

# Trip Edit

Editing the Japan trip vault. Each data type has one source-of-truth file — edit that one, then sync anything downstream that copies its value.

## Where to edit, by data type

| Editing... | Edit this file | Then sync |
|---|---|---|
| Hotel name / dates / nights / cost / booked / confirmation | `<NN-City>/<NN-City>.md` frontmatter | `Hotels.md` row for that city; `Daily-Plan.md` Sleep column for its nights; if `Japan Trip.md`'s checklist names this city as a "missing hotel", drop it from that list once filled |
| Day's plan text / travel note | `Daily-Plan.md` Plan/Travel columns | — |
| Budget category estimate | `Budget.md` table | — |
| A transport leg (mode, station, cost, reserved, confirmation) | `Legs/<NN>-<From>-to-<To>.md` frontmatter | `Transport.md` overview line for that leg, if it states a value that changed |
| Food / restaurant suggestion | `<NN-City>/Food.md` `## Suggestions` list | — |
| Sight / location suggestion | `<NN-City>/Locations.md` `## Suggestions` list | — |
| Festival / event suggestion | `<NN-City>/Events.md` `## Suggestions` list | — |
| Shopping suggestion | `<NN-City>/Shopping.md` `## Suggestions` list | — |
| Orientation / transit / emergency note | `<NN-City>/General-Info.md` `## Suggestions` list | — |
| Pre-trip checklist item | `Japan Trip.md` checklist | — |

Never edit `Hotels.md`'s hotel/date/cost columns directly — its own header note says it's generated from stay frontmatter, so a direct edit gets overwritten or drifts from the real source next time someone regenerates it. Same logic for `Daily-Plan.md`'s Sleep column: it should mirror the stay frontmatter, not diverge from it.

Never edit `Hotels.backup.md` or `Hotels  (nights).md` — they're superseded duplicates kept for reference only.

## City folders

`01-Tokyo-Arrival, 02-Nikko, 03-Fujikawaguchiko, 04-Matsumoto, 05-Takayama, 06-Kanazawa, 07-Kyoto, 08-Hiroshima, 09-Itsukushima, 10-Osaka, 11-Hakone, 12-Tokyo-Departure`

## Match existing formatting exactly

These files are Obsidian notes with YAML frontmatter — a formatting slip (wrong quoting, wrong date shape) breaks Obsidian's rendering and any `.base` view built on it. Before editing, read a neighboring stay note or leg note as a template and mirror its style:

- Dates: `YYYY-MM-DD`, unquoted (e.g. `checkin: 2026-12-05`)
- Strings with spaces or special chars: double-quoted (e.g. `hotel: "Good Nature Hotel Kyoto"`); short plain slugs can stay unquoted (e.g. `city: Hakone`)
- Booleans: bare `true`/`false`, never quoted
- Empty/unknown values: leave the key with nothing after the colon (`cost_yen:`) — don't write `null` or `"TBD"` in frontmatter unless the existing file already used `TBD` for that field (some `hotel:` fields do)
- Checklist items in `## Suggestions` sections: `- [ ] Text` (open) / `- [x] Text` (done), matching the terse, no-fluff style of existing bullets — one line, place/name first, then a short qualifier

## Making the edit

1. Read the target file first — never guess frontmatter keys or table columns.
2. Change only the field(s) asked for. Don't reformat surrounding content, reorder table rows/columns, or touch unrelated fields.
3. If the edit fills in a value that was `TBD`, check the sync list above and update dependent files in the same pass — don't leave `Hotels.md` or `Daily-Plan.md` showing `TBD` for something now known.
4. After editing, report what changed in 1-2 lines (file + field + old → new). No recap of the whole file.

## Examples

**"Book the Hakone hotel — it's Gora Kadan, ¥180,000"**
→ edit `11-Hakone/11-Hakone.md` frontmatter: `hotel: "Gora Kadan"`, `booked: true`, `cost_yen: 180000`
→ sync `Hotels.md` row 11 (Hotel, Cost, drop it from the "Missing" totals note)
→ sync `Daily-Plan.md` Sleep column for 2026-12-05 through 12-07 (last night before checkout)
→ report: "11-Hakone.md: hotel TBD → Gora Kadan, booked false → true, cost — → ¥180,000. Synced Hotels.md row 11 and Daily-Plan.md Sleep (Dec 5-7)."

**"add a ramen place to Kyoto food list"**
→ append one `- [ ]` line to `07-Kyoto/Food.md` under `## Suggestions`, matching existing bullet style.

**"mark Kanazawa as booked"**
→ edit `06-Kanazawa/06-Kanazawa.md` frontmatter: `booked: true`. No cascading value changed, so no downstream sync needed beyond noting `Hotels.md`/checklist text if it explicitly says "not booked".
