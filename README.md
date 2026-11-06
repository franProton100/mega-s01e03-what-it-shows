# S01E03 · what it shows

Ticket #4471 merged 412 date reports into one issue. The reports hold two problems.

| Issue | Problem | Reports |
|---|---|---|
| [#1](../../issues/1) | Month and day swapped (shared parser) | 379 |
| [#2](../../issues/2) | booking-v3 form hint makes people type month first | 33 |

379 + 33 = 412. Counts are from the kb export of 2026-10-04.

Issue #3 (one day off, 4 reports) was folded into #1 and closed as not planned. The five desk-flagged one-day reports are counted under #1 and footnoted.

## The page
- `src/routes/kb/incidents/4471-problems/+page.svx`: the page, in the kb's format.
- `src/lib/kb/ProblemCounts.svelte`: computes the counts from the data, so no number is typed by hand.
- `rendered/4471-problems.html`: the page as the desk sees it, as a standalone file.

## How reports were sorted
By tenant (locale and booking form), not by subject line or intake tag.
