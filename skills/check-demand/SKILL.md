---
name: check-demand
description: When the user wants to know whether people want a product, feature or idea before building it. Weighs six public signals (Google search volume and its trend, the prompts people ask AI engines, Reddit talk, TikTok and YouTube videos, and advertisers paying for it), says what each can and cannot prove, and gives a verdict with its confidence and the test that would settle it. Also use when the user mentions idea validation, validate this idea, is there demand for X, is anyone searching for X, is this market growing or dying, should we build X, or a growing market. How many companies could buy goes to size-market, who already serves the market to map-market, the problems buyers name to find-pain-points, keyword research for content to create-seo-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Demand check

Whether people want X, from six public signals: Google search volume and its trend, the prompts people ask AI engines, how much Reddit discusses the problem, how much TikTok and YouTube carry on it, and who pays to advertise for it. Each signal proves something different and misses something different, so the deliverable states both, then gives a verdict with its confidence and the test that would settle it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `seo_search_keywords` and `reddit_search_posts` (hosts often add a prefix, for example `mcp__manifold__seo_search_keywords`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- This skill reads several tool groups: `seo_*`, `aeo_*`, `reddit_*`, `tiktok_*`, `youtube_*` and `ads_*`. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run on the signals that remain, and name the missing ones in the deliverable, since a signal that was not checked is not a signal that is absent.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product, the problem it solves, the competitors and the workaround buyers use, the markets, the keywords in the tracking set) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first. After delivering, offer to write the keywords and prompts found into the tracking set in `.agents/product-marketing.md` with [create-product-context](../create-product-context/SKILL.md).

## Inputs to settle first

- **The idea**: the product, feature or category, and the problem it solves. Ask for three kinds of phrase: the solution words ("ai grant writer"), the problem words ("grant applications take forever"), and the incumbent or workaround ("grant consultant", "<incumbent> alternative").
- **Keywords the user already has**: if any, step 1 prices them directly instead of searching.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 20 + 45 + 4 + 4 + 1 = 74 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search demand.** `seo_search_keywords` with the solution phrase as `seed` (10 credits for 100 rows), and again with the problem phrase and `mode: "related"` (10 credits): the default mode returns only keywords that contain the seed, which a long problem phrase rarely matches. If the user already has keywords, call `seo_get_keyword_metrics` on them instead (5 plus 5 per 100 keywords, 10 for up to 100); never both on the same keywords. Keep the relevant rows and read: the summed `volume`, `trend[12]` (the last three months against the first three), the share with commercial or transactional `intent`, and `cpc`. Add `ai_volume: true` on `seo_get_keyword_metrics` (4 plus 4 per 100 keywords more) when the question is also whether AI engines see the term.
2. **AI prompts.** `aeo_search_prompts` with the category as `keyword` (45 credits for the default 50 rows, `engine: "chatgpt"`). Read the prompts people ask, their `ai_search_volume` and `cited_domains[]`. Prompts asking for a recommendation ("best tool for...", "how do I...") are the ones with buying intent; `cited_domains[]` shows who answers them today.
3. **Reddit.** `reddit_search_subreddits` with the problem phrase (1 credit): which communities discuss it and how many posts in the sample. `reddit_search_posts` for the problem phrase and for the solution phrase (1 credit each), plus a second page of the problem phrase with `meta.cursor` (1 credit), all with the relevance sort and no time limit per the [Reddit notes](../create-reddit-plan/references/platforms/reddit.md#search-and-watching). Count the on-topic rows, how many have a `created_at` in the past month and in the past year, their `comments`, and above all the posts asking for a solution ("is there a tool", "looking for", "would pay for").
4. **Video and ads.** `tiktok_search_videos` and `youtube_search_videos` with the solution phrase and with the problem phrase, `since: "year"` (1 credit a page each). Read how many results on the page are on topic, their `views`, how recent they are, and what kind they are: creators reviewing products means a market exists; creators explaining the problem means awareness without a product yet. Then `ads_search_ads` on `facebook` with the solution phrase and `active_only: true` (1 credit): advertisers paying for it now, and ads whose `first_shown` is months old, show demand someone already makes money on.
5. **Weigh.** For each signal, write what it measured and what that proves. Then give one verdict: **strong** (volume with a flat or rising trend, recommendation prompts, and Reddit posts asking for a tool), **emerging** (little volume but a rising trend, active Reddit or video talk, few products), **crowded** (strong demand, high `cpc`, many products reviewed on video, many advertisers running), or **weak** (none of these). Give the confidence (high, medium, low) from how many signals agree.
6. **Deliver** a table: signal, tool, the number, what it proves, what it cannot prove. Then the verdict, its confidence, and the one test that would settle what the tools cannot: a landing page with a waitlist, a small ad test, or ten sales calls. The user runs the test; this skill does not.

## Judgment

- Search volume proves people type the words into Google. It cannot prove they would pay, and it misses a category too new to have a name: zero volume with lively Reddit talk is an early market, not an empty one. `volume` is rounded; under about 50 a month it is noise. `null` means the provider has no data, not zero: never add it in as 0.
- `trend[12]` shows direction and seasonality. Compare the same months where the product is seasonal, and treat a one-month spike as news.
- A high `cpc` means advertisers earn money on the click, which is evidence of a market and of competition for it at the same time.
- `ai_search_volume` is a People Also Ask proxy, not query logs. Use it to compare prompts, not to size demand.
- Reddit shows the problem in people's words and who asks for help. Its search is ranked and never complete, and one heated thread is not a market.
- TikTok and YouTube views measure attention, not intent to buy. They follow trends and entertainment: the weakest proof of intent to buy, and the best early read on awareness. A request for a recommendation ("is there a tool that...") is the strongest intent signal a public post carries.
- No signal here proves willingness to pay. Only a sale, a preorder or a paid pilot does. Say so in every verdict, and make the settling test the first action.
- Credits: `aeo_search_prompts` is the expensive call here. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back; `dry_run: true` prices any call for free. A result this account already paid for is free while cached, so a rerun on the same idea soon after costs little.

## Related skills

- The market in companies rather than searches: [size-market](../size-market/SKILL.md). Who already serves it: [map-market](../map-market/SKILL.md).
- The problems behind the demand, in buyers' words: [find-pain-points](../find-pain-points/SKILL.md).
- Keyword research for content once the idea is real: [create-seo-plan](../create-seo-plan/SKILL.md). AI prompts to win: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
- Where to start once demand is there: [create-gtm-plan](../create-gtm-plan/SKILL.md).
