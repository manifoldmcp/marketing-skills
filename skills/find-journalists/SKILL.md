---
name: find-journalists
description: When the user wants a media list of journalists to pitch one story. Finds the reporters who already cover the beat from the coverage competitors earned and recent news on the topic, rates each publication, and gives every reporter the article that proves the beat, a verified address and a one-line angle. Also use when the user mentions a media list, journalists or reporters to pitch, reporters who write about a topic, press contacts, a HARO alternative, or who covered our competitor. A whole digital PR strategy goes to create-digital-pr-plan, press timed to a launch day to create-launch-plan, podcasts to guest on to find-podcasts, and coverage that names the brand without a link to find-unlinked-mentions.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find journalists

A media list for one story: the reporters who already cover the topic, where they wrote about it, and an address for each. This skill starts from the coverage competitors already earned and the recent news on the topic, so every reporter on the list comes with the article that proves the beat. A journalist wants a story, not a link request, so the list is built around a beat and an angle.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_backlinks` and `leads_get_domain_emails` (hosts often add a prefix, for example `mcp__manifold__seo_get_backlinks`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so; the list goes out without addresses.
- If the `linkedin_*` tools are missing, that group is switched off: skip step 5 and say which publications have no reporter on the beat.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the competitors and their domains, the ICP, the story for a pitch, the proof points and data the user holds, the markets) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Story**: what the user will pitch (a launch, a data study, expert comment on a trend). Without one, the list has nothing to lead with; ask, or build one with [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md).
- **Topic and beat**: the words the coverage uses ("payroll software", "remote work"), and the kind of outlet: national, trade press or niche blogs.
- **Competitors**: two or three whose coverage shows who writes about the space.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 18 + 5 x 2 + 4 + 15 x 20 + 5 + 10 = 365 credits for 15 publications and 10 verified reporters. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Coverage of competitors.** `seo_get_backlinks` with `limit: 500` on each competitor domain (18 credits each). Keep rows whose `domain_from` is a publication and whose `page_title` reads as an article, not a directory or a list of tools. Each is a story that already mentioned a rival: note its title and `url_from`, and name what earned it (a data study, a funding round, a founder quote, a free tool).
2. **Find coverage of the topic.** `seo_get_serp` with `depth: 20` for three to five news-style queries ("<topic> study", "<topic> statistics", "<topic> trends", "<topic> survey") at 2 credits each. Add `after:<the date a year ago, YYYY-MM-DD>` to each query: Google's operators pass through, and without it page one is evergreen stats pages, not recent coverage. Keep the publication results with their titles and URLs.
3. **Rate the publications.** `seo_get_domain_ratings` with every publication domain in one call (4 credits). Apply the domain rank [floor](../create-link-building-plan/references/outreach.md#floors), but keep a smaller trade publication that covers the exact beat: relevance beats rank here.
4. **Find the reporters.** For the top 15 publications, `leads_get_domain_emails` with `department: ["communication", "marketing"]`, `type: "personal"` and the default 30 addresses per domain (about 20 credits each). Keep the rows whose `position` says reporter, journalist, editor, writer, correspondent or columnist, and whose beat matches the topic where the position names one. At a small blog, take the editor or the owner. For each coverage row from steps 1 and 2, if the host can open the article, read the byline and match it; otherwise match on position.
5. **Fill the gaps from LinkedIn.** Where a publication has no reporter on the beat, `linkedin_search_posts` with the topic (1 credit a page) finds people posting about it; `linkedin_get_profile` on an author (1 credit) shows the headline ("Reporter at ..."). For a reporter found this way, run step 2 of the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps): `leads_get_email` with the name and the publication's domain (6 credits on a hit).
6. **Verify.** Step 3 of the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps): `leads_get_email_status` on the one address per reporter you would send to (1 credit each).
7. **Deliver** a table: reporter, publication, role, the article that shows the beat (title and URL), domain rank, email, verification status, and the angle: one line tying the user's story to what this reporter wrote.

## Judgment

- A reporter's last relevant article is the whole pitch. Rows with no article evidence go to the bottom.
- Reporters move. If the evidence article is more than a year old, check the current role with `linkedin_get_profile` before the row stays.
- A generic `press@` at a national outlet goes nowhere; at a trade blog it is often the editor. Keep generic addresses only for small publications.
- Syndicated copies of one article (the same title on many domains) are one story. Keep the original publisher.
- Fifteen reporters on the beat beat two hundred on the masthead.
- Data from these tools is public, so say where every number in the angle came from. A journalist checks.
- The [outreach](../create-link-building-plan/references/outreach.md) floors, credits and handoff apply: never send a pitch; the deliverable is a table.

## Related skills

- A digital PR plan with the stories to pitch and a 30-60-90 day plan: [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md).
- Press around a launch day, which plans the timing and uses this list: [create-launch-plan](../create-launch-plan/SKILL.md).
- Coverage that names the user without a link: [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md). Press links the site earned and broke: [reclaim-lost-links](../reclaim-lost-links/SKILL.md).
- Podcasts to guest on: [find-podcasts](../find-podcasts/SKILL.md). A whole link plan: [create-link-building-plan](../create-link-building-plan/SKILL.md).
