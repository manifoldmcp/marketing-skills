# Weekly report

One report a week that puts the other monitoring jobs side by side: new mentions, competitor changes, rank moves and AI visibility, each with the change from last week. It runs the other playbooks rather than repeating them, and it leads with the few things worth acting on. It ends in one document the host keeps and, if it has the tools, delivers.

## Inputs to settle first

- **Sections**: which of the four the report carries: [brand mentions](brand-mentions.md), [competitor watch](competitor-watch.md), [rank tracking](rank-tracking.md) and [AI visibility tracking](ai-visibility-tracking.md). Default: all four. Each section's own inputs are settled in its playbook; a section never run before starts with its baseline this week.
- **Day and reader**: default Monday morning, for the user. The host schedules it and, if it has an email or chat tool, delivers it.
- **Which sections run weekly**: default mentions, competitors and ranks every week, AI visibility every other week (the report shows the last measured date in the off weeks). Mentions may run daily on their own schedule; the report then reads the week's stored mentions instead of searching again.
- **Budget**: at each playbook's defaults, a run costs about 26 (mentions, weekly) + 33 (competitors) + 120 (ranks) = 179 credits, or 359 in the weeks AI visibility runs (+180): about 1,160 credits a month (about $11.60). Daily mentions instead of weekly raise it to about 1,530. Say both numbers before setting up the schedule; pass `max_credits` on every call so no week overspends.

## Steps

1. **Set up (first run only).** Settle each section's inputs from its playbook, store them with the host as in the [router](../SKILL.md#schedule-and-state), and set one schedule for the report day. The first report is the baseline for every section: it says so and lists the starting numbers.
2. **Run the sections.** Follow each section's playbook from its run step, cheapest first: brand mentions, competitor watch, rank tracking, then AI visibility in the weeks it is due. If a tool group is switched off or a call fails, that section says "not measured this week", never zero.
3. **Compute the week-over-week change.** From each section's stored state:
   - Mentions: new mentions by platform and by kind, and the three with the most reach.
   - Competitors: new and stopped ads, new posts worth noting, traffic moves and new top pages.
   - Ranks: keywords up, down and unchanged, the count in the top 3 and top 10, the biggest moves.
   - AI visibility: mention and citation share per brand, against last run and the three-run average.
4. **Pick what to act on.** At most three items, each tied to a row and to the group that acts on it: a thread worth an answer (Reddit's [threads to reply](../../reddit/references/threads-to-reply.md)), a competitor push (the `paid-ads` or `competitors` group), a page off page one (the `seo` group), a prompt where a competitor took the user's place (the `ai-search` group's [citation building](../../ai-search/references/citation-building.md)). Only changes above the router's [thresholds](../SKILL.md#change-not-level) qualify.
5. **Deliver** one document: a headline table (metric, this week, last week, change, baseline), the three actions, each section's change table from its playbook (a quiet section is one line), and the week's credits by section. The host stores this week's numbers as next week's comparison. The server sends nothing; if the host has an email or chat tool, offer to send the document there.

## Judgment

- Changes first. A report that restates levels every week stops being read by the third one.
- The headline table carries the few numbers that answer "better or worse than last week": new mentions, rank count in the top 10, AI mention share, and the competitor moves count. Everything else sits in the sections.
- Each section keeps its own noise rules, from the [router](../SKILL.md#change-not-level). A report that flags everything flags nothing.
- Rank tracking and AI visibility are the largest lines in the cost. Run AI visibility every other week (its week-to-week moves are mostly noise anyway), and trim the rank set to the keywords that matter before cutting a section.
- SEO traffic estimates move slowly; report them month over month inside competitor watch, not as a weekly headline.
- A report for an agency's client, monthly and branded for them, is the `agency` group's [monthly report](../../agency/references/monthly-report.md), which can reuse these sections.
