---
name: trip-fetch
description: Look up data from the Japan trip vault — hotels, daily plan, budget, transport/legs, or per-city food/locations/events/shopping/general-info. Use whenever the user asks what/where/when/how-much about the trip, or to find/show/list/check any trip info, even if they don't name a file. Answers must be terse — a short table or bullet list, no restating the question, no filler sentences.
---

# Trip Fetch

Read-only lookups over the Japan trip vault. Find the right file(s) below, read only those, answer short.

## File map

| Ask about | Authoritative source | Also check |
|---|---|---|
| A hotel: name, dates, nights, cost, booked?, confirmation | `<NN-City>/<NN-City>.md` frontmatter | — |
| Whole-trip hotel table / totals / what's missing | `Hotels.md` | cross-check against stay frontmatter if it looks stale (see Staleness below) |
| Route, trip dates, pre-trip checklist | `Japan Trip.md` | — |
| Day-by-day plan / where sleeping a given night | `Daily-Plan.md` | stay frontmatter `hotel` field if the Sleep cell looks wrong |
| Budget / costs overview | `Budget.md` | `cost_yen` in each stay frontmatter; `Transport.base` for train totals |
| Transport overview (mode per leg) | `Transport.md` | `Legs/<NN>-*.md` for one leg's detail (station, time, cost, reserved) |
| One specific leg (e.g. "Osaka to Hakone") | `Legs/<NN>-<From>-to-<To>.md` | — |
| Food / restaurants for a city | `<NN-City>/Food.md` | — |
| Sights / neighborhoods for a city | `<NN-City>/Locations.md` | — |
| Festivals / seasonal events for a city | `<NN-City>/Events.md` | — |
| Shopping for a city | `<NN-City>/Shopping.md` | — |
| Orientation, transit, emergency for a city | `<NN-City>/General-Info.md` | — |

Never read `Hotels.backup.md` or `Hotels  (nights).md` unless the user explicitly asks for the old/original/legacy hotel list — they're superseded duplicates and will give stale answers.

## City folders

`01-Tokyo-Arrival, 02-Nikko, 03-Fujikawaguchiko, 04-Matsumoto, 05-Takayama, 06-Kanazawa, 07-Kyoto, 08-Hiroshima, 09-Itsukushima, 10-Osaka, 11-Hakone, 12-Tokyo-Departure`

Match loosely — "Miyajima" → `09-Itsukushima`, "Kyoto" → `07-Kyoto`, etc.

## Staleness

`Hotels.md` and `Daily-Plan.md` are hand-maintained views over the per-city stay frontmatter, not live queries — they can drift out of sync. If a value you're about to report from either one looks like it might be outdated (a hotel field shows `TBD` there but the question is about a specific city), verify it against that city's `<NN-City>.md` frontmatter before answering. Don't do this extra check for whole-trip questions where the table itself is the answer (e.g. "show me the hotel table").

## Answering

- Whole-trip / multi-row question → a compact markdown table, only the columns asked for (or the obvious default: hotel name, dates, nights, cost).
- Single-item question → 1-3 bullets, or one line.
- Missing data → say `TBD` or `not booked`, don't pad with a sentence explaining why.
- No preamble ("Based on the files..."), no restating the question, no closing summary.
- Cite the source file only if the user asked "where is this from" or if you had to resolve a staleness conflict.

## Examples

**Q: "what hotel is Kyoto"**
A: Good Nature Hotel Kyoto (Nov 23–29, 6 nights, ¥325,208, booked)

**Q: "show me all the hotels"**
A: table with columns # | City | Hotel | Check-in→out | Nights | Cost | Booked, one row per stay, pulled from `Hotels.md` (flag any `TBD` rows).

**Q: "where are we sleeping Dec 6"**
A: Hakone — Daily-Plan.md says TBD, and 11-Hakone.md frontmatter also has `hotel: TBD` → answer: `TBD (not booked yet)`.

**Q: "how do we get from Osaka to Hakone"**
A: read `Legs/07-Osaka-to-Hakone.md`, answer with mode, service, duration, stations, cost, reserved? — bullets, no prose.
