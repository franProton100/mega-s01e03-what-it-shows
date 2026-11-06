<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { groupByCause, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  // `leftIn` = desk-flagged reports whose own text shows no one-day shift. They stay under the
  // cause the tenant data gives them.
  let { ticket, leftIn = [] }: { ticket: number; leftIn?: string[] } = $props();

  const merged = $derived(uniqueById(reports as Report[]).filter((r) => r.mergedInto === ticket));
  const oneDayIds = $derived(notes.oneDayMove.filter((id) => !leftIn.includes(id)));
  const oneDay = $derived(merged.filter((r) => oneDayIds.includes(r.id)));
  const rest = $derived(merged.filter((r) => !oneDayIds.includes(r.id)));
  const groups = $derived(groupByCause(rest, tenantOf));
  const count = (cause: string) => groups.find((g) => g.cause === cause)?.reports.length ?? 0;

  const rows = $derived([
    { problem: 'month and day swapped (shared parser)', cause: 'parser', n: count('parser') },
    { problem: 'form hint makes people type month first', cause: 'hint', n: count('hint') },
    ...(oneDay.length ? [{ problem: 'date one day off, cause unconfirmed', cause: 'one-day', n: oneDay.length }] : []),
    ...(count('unplaced') ? [{ problem: 'not a day-first tenant', cause: 'unplaced', n: count('unplaced') }] : []),
  ]);
  const total = $derived(rows.reduce((s, r) => s + r.n, 0));
  const dataThrough = $derived(merged.map((r) => r.receivedAt.slice(0, 10)).sort().at(-1));
</script>

<table>
  <thead><tr><th>problem</th><th>cause</th><th>reports</th></tr></thead>
  <tbody>
    {#each rows as r (r.cause)}
      <tr><td>{r.problem}</td><td><code>{r.cause}</code></td><td>{r.n}</td></tr>
    {/each}
  </tbody>
  <tfoot>
    <tr><td>all problems</td><td></td><td>{total}</td></tr>
  </tfoot>
</table>

<p class="note">
  {total} of {merged.length} unique reports merged into #{ticket} · data through {dataThrough}
  {total === merged.length ? '· every report is in exactly one problem' : '· COUNTS DO NOT ADD UP'}
</p>

<style>
  .note { color: var(--muted); font-size: 0.85em; }
  tfoot td { font-weight: 600; }
</style>
