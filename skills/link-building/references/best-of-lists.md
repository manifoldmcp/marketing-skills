# Best-of lists that rank

A "best X tools" article on Google's first page sends buyers and a link at the same time. This playbook finds the lists that rank for the category, sees which competitors they name and whether the user is there, and finds the person who edits each one.

## Inputs to settle first

- **Category**: the words a buyer searches with ("crm for startups", "email warmup tool"). Ask for two or three.
- **Competitors**: the products the lists should already name. Default: the ones the lists name most often in step 3.
- **Site**: the user's domain, to check whether a list names them already.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 10 + 10 + 10 + 4 + 20 x 8 + 20 = 214 credits for 20 lists. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the list keywords.** `seo_search_keywords` with `seed: "best <category>"` (10 credits for 100 rows), and once more with `seed: "<top competitor> alternatives"` (10 credits). Keep keywords that read as a list: best, top, alternatives, vs, tools, software, apps. Sort by volume and keep the top 10.
2. **Pull the first page.** `seo_get_serp` for each of the 10 keywords (1 credit each at the default depth of 10). Keep the results whose title reads as a list and whose domain is a publisher, not a vendor. A vendor's own "best X" page never names a rival, so note it for the [seo](../../seo/references/comparison-pages.md) group's comparison pages and move on. Reddit threads and YouTube videos on the page belong to the [reddit](../../reddit/SKILL.md) and [youtube](../../youtube/SKILL.md) groups; note them.
3. **See who each list names.** `seo_get_page` on each list URL (free, rate limited). Listicles put each product in a heading, so read `h1` and `h2[]` for the competitors and the user's brand. Mark each list: names the user, names competitors but not the user, or unclear (headings without product names; the host can open the page if it has a browser).
4. **Rate.** `seo_get_domain_ratings` with every list domain in one call (4 credits). Apply the domain rank floor from the [router](../SKILL.md#floors). Rank what is left: first the lists that name two or more competitors and not the user, then by the keyword's volume and the list's position on the page.
5. **Find the editor.** Run the [contact steps](../SKILL.md#contact-steps) on the top 20 list domains, with `limit: 10`.
6. **Deliver** a table: list URL, keyword, monthly volume, position, domain rank, competitors named, user named (yes, no, unclear), contact name, role, email, verification status, and the angle: what the list lacks that the user adds (a use case, a price point, a feature).

## Judgment

- The position on the first page is the proof a list is worth the effort. A list that dropped off page one is not.
- Many ranking lists are affiliate pages. The editor may ask for an affiliate deal or a fee. Flag the lists where that is likely (the site reviews many paid tools, the title says "tested" or "we earn a commission"), and let the user decide; never agree to pay.
- A list the user is already on is not a target, but its position is worth reporting: if it ranks, ask the editor to update the entry rather than add it.
- In a new category with no "best X" lists, use "alternatives to <incumbent>" keywords: those lists exist earlier.
- For lists that AI engines cite rather than lists that rank on Google, use the [ai-search](../../ai-search/references/best-of-lists.md) group's best-of lists playbook; the two overlap but are ranked differently.
