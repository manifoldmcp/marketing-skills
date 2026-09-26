# Citation building

AI engines answer from sources they cite. When a competitor is named and the user is not, the sources in that answer are the reason. This playbook finds those sources, ranks them by how many engines and prompts cite them, routes each one by type, and finds a person to contact where a person can change the page.

It shares the contact mechanics with link building and nothing else: targets here are chosen by citations, not by domain rank, so the link-building floors do not apply. A small site that ChatGPT cites often beats a large site it never cites.

## Inputs to settle first

- **Brand and competitors**: as in the [visibility check](visibility-check.md).
- **Answers**: a visibility check run from this conversation or an earlier one (its `task_id`; results are kept 30 days). If there is none, run one first: open [visibility-check.md](visibility-check.md) and do its steps 1 and 2.
- **What the user can offer a source**: data, a quote, a product trial, an updated fact. The ask depends on it.
- **Budget**: with an existing run, about 15 x 8 + 15 = 135 credits for contacts at 15 sources. Without one, add the visibility check (about 270). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the answers.** `get_task` with the visibility check's `task_id` (free). Keep the rows where at least one competitor is mentioned or cited and the user is neither.
2. **Build the source table.** From those rows, list every citation by URL. For each, count the engines that cite it and the prompts it is cited for, across all rows, and note which competitors those rows mention. Drop the user's own domain and any source already citing the user in other rows (it knows the user; a nudge may do, but it is not a gap).
3. **Rank.** Order the sources by engines citing, then prompts citing. A source cited by two or more engines, or for two or more prompts, is a target. A source cited once is noise unless it is the only source for a prompt that matters.
4. **Route each source by type.** Read the URL, the domain and the citation `title`; when the type is unclear, `seo_get_page` on the URL (free) gives the title and headings. Then:
   - **Editorial page** (an article or guide on a publisher or blog): a contact target, step 5.
   - **Best-of list** (a title with best, top, alternatives or compared, on a publisher): open [best-of-lists.md](best-of-lists.md) for inclusion.
   - **Reddit thread**: the [reddit](../../reddit/references/ai-cited-threads.md) group's threads AI engines cite; replies there follow the subreddit's rules, never outreach.
   - **YouTube video**: the [youtube](../../youtube/references/ai-cited-videos.md) group's videos AI engines cite, and the creator through [influencers](../../influencers/references/vet-creator.md).
   - **Review site or directory** (G2, Capterra, Trustpilot, Product Hunt, AlternativeTo, Clutch and the like): no manifold tool covers these. Put them on a checklist: claim the profile, complete it, ask customers for reviews.
   - **A competitor's own page** (their comparison or alternatives page): the answer is the user's own page on the same question, through the [seo](../../seo/references/comparison-pages.md) group's comparison pages.
   - **Wikipedia or a reference site**: checklist only. Do not suggest editing a page about the user's own company.
5. **Find the contacts.** Run the [contact steps](../../link-building/SKILL.md#contact-steps) on the editorial domains, with `limit: 10`, skipping the link-building floors. When the citation names an author you know, use its step 2 for that person.
6. **Deliver** a table: source URL, type, engines citing, prompts citing, competitors in those answers, route (the playbook or the checklist), contact, role, email, verification status, and the ask: what the page is missing that the user can supply (a mention in a list of tools, an updated fact, a data point).

## Judgment

- Rank by citations, never by domain rank. The engines have already said which pages they trust for these prompts.
- Citation sets move between runs. A source cited by several engines is stable; a source one engine cited once may be gone next week.
- The ask is to make the page more complete or more correct for its readers, with evidence. Do not offer payment for a mention; a paid placement that the page does not disclose is a risk to the user.
- `ai_overview` and `ai_mode` citations follow Google's rankings closely, so ranking work in the [seo](../../seo/SKILL.md) group helps there too.
- Some prompts have no gap to close: every engine names the user. Report them as held, and spend nothing on them.
- After the sources change, re-run the same prompt set to measure it. Changes reach engines at different speeds; re-measure after two to four weeks, not days.
