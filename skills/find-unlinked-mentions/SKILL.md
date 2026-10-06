---
name: find-unlinked-mentions
description: When the user wants links from pages that already mention the brand. Finds pages on Google that name the brand, the product or the founder, drops the sites that already link, checks and rates the rest, and finds who can add the link with a verified address. Also use when the user mentions unlinked mentions, articles that mention us but do not link, turning brand mentions into links, who wrote about us without a link, or blogs that reviewed our product without linking. New mentions watched over time go to monitor-brand-mentions, links that were removed to reclaim-lost-links, other link prospects to find-backlink-targets, and wrong facts in AI answers to fix-wrong-ai-answers.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Unlinked mentions

A page that already names the brand is the easiest link to ask for: the writer chose to mention it, and adding a link is a one-line edit. This skill finds pages that mention the brand on Google, drops the sites that already link, rates the rest and finds who can add the link. It hands back a table with the ask for each page.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_serp` and `leads_get_domain_emails` (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the mention list without contacts.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand and product names, the founder, the site and the pages worth linking to) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Brand**: the name as people write it, the product names, and the founder's name if they are quoted in press. If the name is a common word, add a category word to every query ("Acme" crm).
- **Site**: the user's domain, and the page each mention should link to when it is not the homepage (a stats page, a product page, a free tool).
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 10 + 25 + 6 + 20 x 8 + 20 = 231 credits for two names and 20 contacts. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the mentions.** `seo_get_serp` with the keyword `"<brand>" -site:<domain>` and `depth: 100` (10 credits), and the same for each other name. Google's operators pass through: the quotes keep exact matches, and `-site:` drops the user's own pages. For recent coverage only, run it again with `after:<a date, YYYY-MM-DD>` added. Keep the organic `results` with their `url`, `domain`, `title` and `snippet`. Drop social platforms, forums and video (reddit.com, youtube.com, linkedin.com, x.com, facebook.com), the user's own profiles, and review sites and directories (G2, Capterra, Trustpilot, Product Hunt, Crunchbase): a profile is claimed, not pitched. If no result is left, say so and stop: the brand has no indexed mentions to reclaim yet.
2. **Drop the sites that already link.** `seo_get_referring_domains` on the user's domain with `limit: 1000` (25 credits). Drop every mention whose `domain` is in it. The backlink index lags the live web by weeks, so a page from the last month may link already: if the host can open the page, check before outreach; otherwise mark the row "no link in the index".
3. **Check the pages.** `seo_get_page` on each kept URL (free, rate limited). Drop pages whose `status` is not 200 or whose `final_url` moved elsewhere. Note what the mention is from the `title`, `h1` and the SERP `snippet`: a review, a list, a news story, an interview, a passing reference.
4. **Rate.** `seo_get_domain_ratings` with every kept domain in one call (6 credits for 200). Apply the [floors](../create-link-building-plan/references/outreach.md#floors), with relevance over rank: a trade blog that reviewed the product beats an unrelated site with a higher rank.
5. **Find the contact.** Run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on the top 20 domains with `limit: 10`. When the page names its author and the host can read it, use contact step 2 for that person: the writer adds links faster than a generic inbox.
6. **Deliver** a table: page URL, title, domain, domain rank, kind of mention, how the brand is named (from the snippet), the user's page to link to, contact name, role, email, verification status, and the ask in one line.

## Judgment

- The ask is a thank-you plus one URL: "you mentioned us in <title>; here is the page if you want to link it." No pitch, no request for anything else.
- Point the link at the page that helps the reader: the stat's source page for a stat, the product page for a review. The homepage is the default only when the mention is the brand itself.
- Recent mentions get edited; pages over a year old rarely do. Work the newest first.
- Many news sites do not link to companies they cover, or link with `nofollow`. A mention without a link still counts for brand demand and AI answers; do not push past one polite email.
- A mention that gets a fact wrong (an old price, a dropped feature) is a correction first, a link second. Use [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md) when the same error shows in AI answers.
- Google returns at most about 100 results per query, so this is a sample of the best-ranking mentions, not every one. Run it again each quarter; the server keeps no history.
- The [outreach](../create-link-building-plan/references/outreach.md) handoff applies: never send; the deliverable is the table.

## Related skills

- New mentions on Reddit and social platforms, watched over time: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
- Links that sites removed, and broken pages that still have links: [reclaim-lost-links](../reclaim-lost-links/SKILL.md).
- Where links should come from, and a 90-day plan: [create-link-building-plan](../create-link-building-plan/SKILL.md). Press that earns new mentions: [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md).
