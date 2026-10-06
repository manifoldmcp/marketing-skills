---
name: write-battlecard
description: When the user wants a sales battlecard against one competitor. Builds a one-page card from the competitor's homepage and active ads, what ChatGPT and Gemini tell prospects about it, dated Reddit complaints, the comparison searches buyers run and its company facts, with claims and counters, where they win, landmine questions, planted objections and quick facts. Also use when the user mentions a battlecard, a sales battlecard against X, how do we beat X in deals, what reps should say when a prospect is also looking at X, a competitive one-pager for the sales team, or trap questions to ask. A marketing teardown of the competitor goes to tear-down-competitor, a plan to take share from it to create-competitor-plan, and side-by-side messaging and prices to compare-messaging.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Battlecard

One page sales can use in a deal against one competitor: what the competitor claims and the user's counter to each, where the competitor wins and how to handle it, the questions that bring its weak spots up, and the quick facts. [tear-down-competitor](../tear-down-competitor/SKILL.md) measures a competitor's marketing; the battlecard turns what buyers and engines say about it into talk tracks. It ends in the card, with a source on every line.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_get_page`, `aeo_run_ai_answers` and `reddit_search_posts` (hosts often add a prefix, for example `mcp__manifold__seo_get_page`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `seo_*`, `ads_*`, `aeo_*`, `reddit_*`, `leads_*` and `linkedin_*`. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run without them, and mark their lines "not checked".

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitor's domain, the user's differentiation and proof points, and the objections sales hears) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Competitor**: its domain and brand name.
- **The user's product**: what it does better and worse than this competitor, and the proof for each (a number, a customer, a demo). Every counter on the card comes from this; ask for it.
- **Deals**: why the user's team wins and loses against this competitor, if anyone knows. A lost-deal reason the public evidence also shows goes to the top of the card.
- **Existing research**: the [compare-messaging](../compare-messaging/SKILL.md) table and the [find-competitor-complaints](../find-competitor-complaints/SKILL.md) output for this competitor, if they ran. Steps 1 and 3 reuse them instead of calling again.
- **Budget**: a default run costs about 2 + 8 + 9 + 11 + 2 = 32 credits; the pages are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Their pitch.** `seo_get_page` (free) on the competitor's homepage and pricing page: `h1[]`, `h2[]` and `meta_description` are the claims its reps repeat. `ads_get_advertiser_ads` on `facebook` with `active_only: true` and on `linkedin` (1 credit each): the offers it pays to push (a trial, a discount, a migration service). Prices come only from the page, read by the host or pasted by the user, as in [compare-messaging](../compare-messaging/SKILL.md); otherwise "not read".
2. **What the prospect hears from AI.** `aeo_run_ai_answers` with "What are the downsides of <competitor>?" and "<competitor> vs <user's brand>", `engines: ["chatgpt", "gemini"]` and `brands` holding both (4 credits per prompt, 8), then `get_task` (free) after `poll_after_s`. Prospects ask this before the call. Note what the answers say each product is better at, and any claim about the user that is wrong.
3. **Where buyers say it fails.** `reddit_search_posts` with "<competitor> alternative" and "switched from <competitor>" (1 credit each), `reddit_search_comments` with "<competitor> support" and "<competitor> price increase" and `time_range: "all"` (1 credit each), keeping the past two years by `created_at`, per the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching). `reddit_get_comments` on the five busiest threads (1 credit each). Keep quotes that name a failure and a date; note where people defend it too.
4. **What buyers compare.** `seo_search_keywords` with "<competitor> vs" as `seed` (10 credits): the products it is weighed against, and how often. `seo_get_serp` for "<competitor> vs <user's brand>" (1 credit): who owns the page a prospect reads before deciding. If it is the competitor's own comparison page, read its `h2[]` (`seo_get_page`, free): those are the arguments the prospect arrives with.
5. **Quick facts.** `leads_get_company` (1 credit) for `employees` and `founded_year`, and `linkedin_get_company` (1 credit) for LinkedIn's own `employees`. The two headcounts often disagree; put both on the card. With step 1: pricing model and entry price where read, and the offer in its ads.
6. **Write the card**, one page:
   - **Their claims, our counters**: each claim from step 1 or step 4, the user's answer, and the proof. A claim the user cannot answer stays on the card with "concede".
   - **Where they win**: what step 2, step 3 and the user's deal notes say they do better, and how to handle it (narrow the fit, reframe the cost, or walk away).
   - **Landmine questions**: three to five questions for the prospect that bring a weak spot up without naming the competitor ("How do you handle X when Y?"), each tied to a dated quote from step 3.
   - **Objections they plant**: what their pitch and the AI answers tell the prospect about the user, with the reply.
   - **Quick facts**: headcount (both sources), pricing model with its source and date, active offers, who owns the "vs" search.
   Every line carries its source link.

## Judgment

- A card with no "where they win" section is not trusted by sales. Honesty about the competitor's strengths is what makes the counters credible.
- Landmines are questions, not attacks. A rep who runs down a competitor loses the buyer's trust; a question that makes the buyer discover the gap keeps it.
- Date every weakness. A complaint from before the competitor's last release or price change may be fixed; leave out anything older than two years.
- A counter needs proof the user can show in the call. A counter without one is marked "claim only".
- Never put a price, a feature or a customer count on the card from memory. No manifold tool reads a page's body text; use what the page, the user or a tool returned, or write "not read".
- Quote the competitor's copy only as short evidence (a headline, a tagline), with the URL.
- Refresh the card when the competitor changes its pricing or homepage claims; [monitor-competitors](../monitor-competitors/SKILL.md) flags both.
- Credits: say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Never send the card or contact anyone; hand it to the user. After delivering, offer to write the objections and counters into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Related skills

- The competitor's marketing across every channel: [tear-down-competitor](../tear-down-competitor/SKILL.md).
- Complaints about the competitor in depth: [find-competitor-complaints](../find-competitor-complaints/SKILL.md). Buyers' objections to the user: [find-objections](../find-objections/SKILL.md).
- A "<user> vs <competitor>" page for the search in step 4: [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
- A wrong AI answer about the user from step 2: [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md).
- A plan to take share from this competitor: [create-competitor-plan](../create-competitor-plan/SKILL.md).
