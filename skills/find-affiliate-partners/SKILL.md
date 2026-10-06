---
name: find-affiliate-partners
description: When the user wants to recruit affiliates or partners who already promote competitors. Finds publishers paid by competitors from the affiliate parameters in competitors' backlinks, review sites that rank for competitor reviews and alternatives, and YouTube reviewers, sizes their organic reach and finds the partnerships contact with a verified address. Also use when the user mentions affiliate partners, who promotes our competitor, review sites to partner with, recruit affiliates, partner program prospects, or bloggers who review a competitor. Creators to sponsor or vet go to find-creators and vet-creator, sites to ask for an editorial link to find-backlink-targets, and best X lists to get onto to find-best-of-lists.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Affiliate partners

The best affiliate recruits are publishers who already earn from a competitor: they have the audience, the content and the habit. This skill finds them from the evidence in competitors' backlinks and the review searches, sizes their reach and finds the person who handles partnerships. It hands back a table of publishers with a verified contact each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_backlinks` and `leads_get_domain_emails` (hosts often add a prefix, for example `mcp__manifold__seo_get_backlinks`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the publisher list without contacts.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitors and their domains, the category, the markets) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Competitors**: two or three that run an affiliate or partner program. Ask the user; if they do not know, the affiliate parameters in step 1 show which ones do.
- **Offer**: whether the user has commission terms. The pitch needs them; never promise terms the user has not set.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 25 + 10 + 3 + 2 + 65 + 4 + 20 x 8 + 20 = 314 credits for 20 partners. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find publishers already paid by competitors.** `seo_get_backlinks` with `limit: 1000` on each competitor domain (25 credits each). Keep the rows whose `url_to` carries an affiliate parameter: `ref=`, `via=`, `aff=`, `affiliate`, `fpr=`, `partner=`, `irclickid`, `utm_medium=affiliate`. Each is proof of a deal. Links routed through an affiliate network's own domain do not show here; step 2 finds those sites.
2. **Find the review publishers.** `seo_search_keywords` with `seed: "<competitor> review"` (10 credits) shows which review and comparison searches have volume. Then `seo_get_serp` for "<competitor> review", "<competitor> alternatives" and "best <category>" (1 credit each). Keep the publishers, not the vendors: a site ranking for a competitor's review usually earns from it.
3. **Find video reviewers.** `youtube_search_videos` with "<competitor> review" and `since: "year"` (1 credit a page, two pages). Note the channels; [vet-creator](../vet-creator/SKILL.md) vets creators, so hand them over rather than vetting here.
4. **Size the publishers.** `seo_get_traffic_estimates` on the publisher domains (50 credits plus 50 per 100 domains; 65 for 30) for reach, and `seo_get_domain_ratings` (4 credits) for authority. Affiliates are about reach: rank by `organic_traffic` first. Drop sites under about 1,000 organic visits a month unless they cover the niche exactly.
5. **Find the partnerships contact.** Run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on the top 20 with `limit: 10` and `department: ["marketing", "executive", "sales"]`: on a publisher, partnerships sit with marketing, the owner or business development.
6. **Deliver** a table: publisher, type (review site, blog, newsletter, YouTube channel), organic traffic, domain rank, competitors promoted, the evidence (the affiliate URL or the ranking review page), contact, role, email, verification status.

## Judgment

- An affiliate parameter in `url_to` is proof; a review page that ranks for a competitor's name is likely but unproven. Say which each row has.
- Skip coupon, cashback and deal sites unless the user wants them: they claim credit for sales that would have happened anyway.
- A publisher promoting three competitors will compare the user's terms with theirs. Ask the user what they offer before outreach starts.
- One row per publisher, even when several of its pages link to competitors. List the strongest page as evidence.
- The server does not read the competitor's own affiliate program page. If the host can open it, the commission rate there is useful context for the user's offer.
- The [outreach](../create-link-building-plan/references/outreach.md) handoff applies: never send or sign anyone up; the deliverable is the table.

## Related skills

- YouTube reviewers to vet or sponsor: [vet-creator](../vet-creator/SKILL.md) and [find-creators](../find-creators/SKILL.md).
- Where links should come from, and a 90-day plan: [create-link-building-plan](../create-link-building-plan/SKILL.md). Lists to get onto: [find-best-of-lists](../find-best-of-lists/SKILL.md).
- Who the competitors are: [find-competitors](../find-competitors/SKILL.md).
