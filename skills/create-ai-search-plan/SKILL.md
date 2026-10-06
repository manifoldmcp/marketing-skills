---
name: create-ai-search-plan
description: When the user wants a plan for getting their brand named and cited in AI answers. Measures where the brand stands on ChatGPT, Claude, Gemini, Perplexity and Google AI Overviews for a fixed set of buyer prompts, finds the gaps against competitors, in sources, on the site and in wrong facts, picks the tactics that close them fastest and ends in a 30-60-90 day plan with KPIs. Also use when the user mentions GEO, AEO, LLM SEO, AI SEO, generative engine optimization, answer engine optimization, a GEO strategy, an AEO plan, how do we get recommended by ChatGPT, an AI search strategy for the next quarter, or an LLM SEO roadmap. A one-off measurement goes to check-ai-visibility, a single tactic to its own skill such as build-ai-citations or check-ai-crawler-access, and a plan for Google rankings to create-seo-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# AI search strategy

An AI search (GEO, AEO) strategy answers three questions: for which buyer questions the brand should be named, where it stands on each engine today, and which tactics close the gap fastest. It hands back a 90-day plan measured on a fixed prompt set.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `aeo_run_ai_answers`, `aeo_search_prompts` and `aeo_get_site_readiness` (hosts often add a prefix, for example `mcp__manifold__aeo_run_ai_answers`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand, the site, the ICP, the competitors, the buyer questions and AI prompts in the tracking set, the team and the budget) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: the buyer questions the brand should win, and what winning means. Default: named in the answer for its category and problem prompts on ChatGPT and Google's AI Overview, the two engines most buyers see.
- **Stage**: a new brand the engines barely know, or an established one. Step 2 shows it.
- **ICP**: who buys, so the prompts are theirs.
- **Budget**: credits for the research (this skill costs about 270) and for re-measuring (180 a run for ten prompts). Default: 1,000 credits over 90 days.
- **Team**: who can edit the site, write pages and do outreach. Default: one marketer, a developer on request.
- **Competitors**: up to nine. Default: the brands the engines name most in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 270 credits. `aeo_get_site_readiness` on the site (free); `aeo_search_prompts` on the site's domain and on the category keyword (45 credits each) to choose ten prompts, built as [check-ai-visibility](../check-ai-visibility/SKILL.md) builds them in its step 1; then `aeo_run_ai_answers` with those prompts, the user and competitors as `brands`, on the default five engines (180 credits), read with `get_task` after `poll_after_s`. Score it as check-ai-visibility does in its step 3: mention share, average position and citation share per brand and engine.
3. **Gaps.**
   - Against competitors: the prompts and engines where a competitor is named and the user is not, and the leader on each engine.
   - Sources: the domains cited in those answers, by type (editorial, lists, Reddit, YouTube, review sites, competitor pages).
   - Site: any `blocks_citations` check that fails.
   - Accuracy: any answer that states something wrong about the user.
   - Market: prompts with `ai_search_volume` where no brand is named consistently; open ground.
4. **Tactics.** Choose, in this order, and say why each fits the numbers:
   - [check-ai-crawler-access](../check-ai-crawler-access/SKILL.md) when any `blocks_citations` check fails: nothing else works until engines can read the site.
   - [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md) when an answer gets a buying fact wrong.
   - [build-ai-citations](../build-ai-citations/SKILL.md) when competitors are named on sources the user is absent from, Reddit threads and YouTube videos included.
   - [find-best-of-lists](../find-best-of-lists/SKILL.md) when lists dominate the citations for category prompts.
   - [check-ai-visibility](../check-ai-visibility/SKILL.md) is the measurement: the same prompt set at day 30, 60 and 90.
   - [check-ai-overviews](../check-ai-overviews/SKILL.md) when Google's AI Overview is an engine that matters and the site already ranks on Google: the citations there follow the rankings the site has.
   - Pages on the user's site that answer the prompts directly are content work for [create-seo-content-plan](../create-seo-content-plan/SKILL.md), routed as check-ai-visibility does in its step 5 when the site is never cited.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - KPIs the tools re-measure on the same prompt set: mention share and citation share per engine (`aeo_run_ai_answers`), cited sources won (the rows of the build-ai-citations table that now cite the user), readiness checks passing (`aeo_get_site_readiness`).
   - A typical shape: by day 30, readiness fixed, errors corrected and the first 15 sources contacted; by day 60, list inclusions and new answer pages live; by day 90, a second round of sources and the re-measure. For a check on a schedule, check-ai-visibility keeps it running on the host's schedule.
   - **Deliver** one document: the inputs with defaults marked, the prompt set, the baseline table (brand by engine: mention share, position, citation share), the gaps, the chosen tactics with the linked skill and why, the 30-60-90 table, and the three first actions.

## Judgment

- Keep the prompt set fixed for the whole horizon. Changing prompts mid-plan makes every comparison meaningless.
- Answers are live and non-deterministic: leave out cells whose `answer` is null, report each engine apart before any total, and act on a gap only when it shows in at least two cells.
- Engines refresh sources at different speeds. Engines that search live move within weeks; answers that come from training data move on the model's release cycle. Say which is which when setting the 30-day KPI.
- A share on ten prompts moves in steps of two points per row. Set KPI targets that are larger than that noise.
- Google's AI Overview and AI Mode follow organic rankings closely. Where they are the engines that matter, SEO work is AI search work.
- Do not promise a position in an answer. Promise the inputs the engines read: a readable site, correct facts, and presence on the sources they cite.
- The server keeps no history. The host keeps the prompt set, the `task_id` and the shares, so the day 30, 60 and 90 checks can compare. Answers are never cached; reuse a `task_id` rather than running the same set twice.

## Related skills

- The measurement, once or on a schedule: [check-ai-visibility](../check-ai-visibility/SKILL.md).
- Google rankings, which AI Overviews follow: [create-seo-plan](../create-seo-plan/SKILL.md). Crawl errors and indexing: [audit-technical-seo](../audit-technical-seo/SKILL.md).
- Taking part in the Reddit threads the engines cite: [find-reddit-threads](../find-reddit-threads/SKILL.md). The YouTube creators behind the videos they cite: [find-creators](../find-creators/SKILL.md).
- Who the competitors are, when the user does not know: [find-competitors](../find-competitors/SKILL.md).
