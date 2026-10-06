---
name: find-best-of-lists
description: When the user wants to get into the best-of lists and listicles buyers and AI engines read. Finds the best X, top 10 and alternatives articles that rank on Google's first page or that ChatGPT, Perplexity, Gemini, Claude and Google AI Overviews cite for category prompts, shows which competitors each names and whether the user is there, and finds the editor with a verified address. Also use when the user mentions get us into best X lists, listicles for our category, roundup posts that rank, top 10 tools articles, alternatives pages we should be on, which best-of lists ChatGPT uses, or roundups Perplexity quotes. Other sources AI engines cite go to build-ai-citations, the user's own comparison or alternatives pages to plan-comparison-pages, and other link prospects to find-backlink-targets.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Best-of lists

A "best X tools" article sends buyers and a link at the same time, and when a buyer asks an AI engine for "the best X", the answer is often built from a handful of these listicles, which the engines cite. Getting onto those lists changes both. This skill picks the lists either by where they rank on Google or by what the AI engines cite, sees which competitors each one names and whether the user is there, and finds the person who edits each one. It hands back a table with the angle for each list.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_serp` and `leads_get_domain_emails`, and `aeo_run_ai_answers` for lists AI engines cite (hosts often add a prefix, for example `mcp__manifold__seo_get_serp`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `seo_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the list table without contacts.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand, the site, the category words, the competitors, the AI prompts in the tracking set, the markets) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Case**: lists that rank on Google, lists AI engines cite, or both. Default: the one the user's words name; Google when they name neither.
- **Category**: the words a buyer searches with ("crm for startups", "email warmup tool"), two or three; for AI engines, three to five "best X", "top X for Y" and "X alternatives" prompts. Default for AI engines: the category prompts of an [check-ai-visibility](../check-ai-visibility/SKILL.md) run, if there is one.
- **Brand, site and competitors**: the user's name and domain, to check whether a list names them already, and the products the lists should already name. Default competitors: the ones the lists name most often.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: lists that rank cost about 10 + 10 + 10 + 4 + 20 x 8 + 20 = 214 credits for 20 lists. Lists AI engines cite cost about 5 x 18 + 5 x 2 + 10 x 8 + 10 = 190 for five prompts, five AI overviews and contacts at ten lists, or about 100 with an check-ai-visibility run already in hand. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pick the case** and open its reference: [lists that rank](references/ranking-lists.md) or [lists AI engines cite](references/ai-cited-lists.md). For both, run both and merge the rows by URL.
2. **Find the lists**, as the reference's "Find the lists" says. Keep pages whose title reads as a list (best, top, alternatives, compared, vs) on a publisher, not on a vendor. A vendor's own "best X" page never names a rival, so note it for [plan-comparison-pages](../plan-comparison-pages/SKILL.md) and move on.
3. **See who each list names.** `seo_get_page` on each list URL (free, rate limited). Listicles put each product in a heading, so read `h1` and `h2[]` for the competitors and the user's brand. Mark each list: names the user, names competitors but not the user, or unclear (headings without product names; the host can open the page if it has a browser).
4. **Rank the lists**, as the reference's "Rank" says: by position and domain rank for lists that rank, by engines and prompts citing for lists AI engines cite.
5. **Find the editor.** Run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on the top list domains (20 for lists that rank, 10 for lists AI engines cite), with `limit: 10`.
6. **Deliver** a table: list URL, the reference's own columns, competitors named, user named (yes, no, unclear), contact name, role, email, verification status, and the angle: what the list lacks that the user adds (a use case, a price point, a feature, what the user does best).

## Judgment

- Many lists are affiliate pages. The editor may ask for an affiliate deal or a fee. Flag the lists where that is likely (the site reviews many paid tools, the title says "tested" or "we earn a commission"), and let the user decide; never agree to pay.
- The two cases overlap but are ranked differently: a list on Google's page one proves itself by its position, a list the engines cite by their citations. Do not apply the domain rank floor to a list the engines cite.
- The [outreach](../create-link-building-plan/references/outreach.md) credits and handoff apply: contacts last, only for the shortlist; never send.

## Related skills

- Every other source AI engines cite (articles, Reddit threads, YouTube videos, review sites): [build-ai-citations](../build-ai-citations/SKILL.md). Where the brand stands in AI answers: [check-ai-visibility](../check-ai-visibility/SKILL.md).
- The user's own comparison and alternatives pages: [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- Other link prospects and the 90-day plan: [find-backlink-targets](../find-backlink-targets/SKILL.md), [create-link-building-plan](../create-link-building-plan/SKILL.md). Publishers that would promote the user for a commission: [find-affiliate-partners](../find-affiliate-partners/SKILL.md).
