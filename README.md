# S01E03 · what it shows

Ticket #4471 merged 412 date reports into one issue. The reports hold three problems.

| Issue | Problem | Reports |
|---|---|---|
| [#1](../../issues/1) | Month and day swapped (shared parser) | 375 |
| [#2](../../issues/2) | booking-v3 form hint makes people type month first | 33 |
| [#3](../../issues/3) | Date one day off, cause unconfirmed | 4 |

375 + 33 + 4 = 412. Counts are from the kb export of 2026-10-04.

## The page
- `src/routes/kb/incidents/4471-problems/+page.svx`: the page, in the kb's format.
- `src/lib/kb/ProblemCounts.svelte`: computes the counts from the data, so no number is typed by hand.
- `rendered/4471-problems.html`: the page as the desk sees it, as a standalone file.

## How reports were sorted
By tenant (locale and booking form), not by subject line or intake tag.
