# Journalists

A media list for one story: the reporters who already cover the topic, where they wrote about it, and an address for each. A journalist wants a story, not a link request, so the list is built around a beat and an angle.

## Inputs to settle first

- **Story**: what the user will pitch (a launch, a data study, expert comment on a trend). Without one, the list has nothing to lead with; ask.
- **Topic and beat**: the words the coverage uses ("payroll software", "remote work"), and the kind of outlet: national, trade press or niche blogs.
- **Competitors**: two or three whose coverage shows who writes about the space.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 18 + 5 x 2 + 4 + 15 x 20 + 5 + 10 = 365 credits for 15 publications and 10 verified reporters. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find coverage of competitors.** `seo_get_backlinks` with `limit: 500` on each competitor domain (18 credits each). Keep rows whose `domain_from` is a publication and whose `page_title` reads as an article, not a directory or a list of tools. Each is a story that already mentioned a rival: note its title and `url_from`.
2. **Find coverage of the topic.** `seo_get_serp` with `depth: 20` for three to five news-style queries ("<topic> study", "<topic> statistics", "<topic> trends", "<topic> survey") at 2 credits each. Keep the publication results with their titles and URLs.
3. **Rate the publications.** `seo_get_domain_ratings` with every publication domain in one call (4 credits). Apply the domain rank floor from the [router](../SKILL.md#floors), but keep a smaller trade publication that covers the exact beat: relevance beats rank here.
4. **Find the reporters.** For the top 15 publications, `leads_get_domain_emails` with `department: ["communication", "marketing"]`, `type: "personal"` and the default 30 addresses per domain (about 20 credits each). Keep the rows whose `position` says reporter, journalist, editor, writer, correspondent or columnist, and whose beat matches the topic where the position names one. At a small blog, take the editor or the owner. For each coverage row from steps 1 and 2, if the host can open the article, read the byline and match it; otherwise match on position.
5. **Fill the gaps from LinkedIn.** Where a publication has no reporter on the beat, `linkedin_search_posts` with the topic (1 credit a page) finds people posting about it; `linkedin_get_profile` on an author (1 credit) shows the headline ("Reporter at ..."). For a reporter found this way, run contact step 2 in the [router](../SKILL.md#contact-steps): `leads_get_email` with the name and the publication's domain (6 credits on a hit).
6. **Verify.** Contact step 3 in the [router](../SKILL.md#contact-steps): `leads_get_email_status` on the one address per reporter you would send to (1 credit each).
7. **Deliver** a table: reporter, publication, role, the article that shows the beat (title and URL), domain rank, email, verification status, and the angle: one line tying the user's story to what this reporter wrote.

## Judgment

- A reporter's last relevant article is the whole pitch. Rows with no article evidence go to the bottom.
- Reporters move. If the evidence article is more than a year old, check the current role with `linkedin_get_profile` before the row stays.
- A generic `press@` at a national outlet goes nowhere; at a trade blog it is often the editor. Keep generic addresses only for small publications.
- Keep the list short and matched. Fifteen reporters on the beat beat two hundred on the masthead.
- Syndicated copies of one article (the same title on many domains) are one story. Keep the original publisher.
- For press around a launch day, the [launch](../../launch/references/press.md) group's press playbook plans the timing and uses this list.
