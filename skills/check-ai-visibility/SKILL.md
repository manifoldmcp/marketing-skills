---
name: check-ai-visibility
description: When the user wants to know whether AI engines mention and recommend their brand, once or tracked over time. Runs a fixed set of buyer prompts live on ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews and AI Mode and scores mention share, position and citation share against competitors, as a one-off check or on the host's schedule with the change since the last run. Also use when the user mentions AI visibility, an AI visibility audit, AI share of voice, does ChatGPT recommend us, are we in Perplexity's answers, which AI engines mention us, track our AI visibility, monitor ChatGPT mentions over time, or alert me if Gemini stops mentioning us. A plan for AI search goes to create-ai-search-plan, sources to win to build-ai-citations, wrong facts in answers to fix-wrong-ai-answers, and the site's Google keywords with an AI overview to check-ai-overviews.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# AI visibility

Where the brand stands in AI answers: for a fixed set of buyer prompts, which engines mention it, in what position, and whether they cite its site, against the competitors. It hands back a brands table, a prompts table, the cited domains and the biggest gaps, once or run over run on the host's schedule. It is the baseline every other AI search skill starts from, and the measurement to repeat.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `aeo_run_ai_answers` and `aeo_search_prompts` (hosts often add a prefix, for example `mcp__manifold__aeo_run_ai_answers`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand, the site, the competitors, the buyer questions and AI prompts in the tracking set, the markets) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Brand**: the name and the domain, since engines mention names and cite domains.
- **Competitors**: up to nine, names or domains. Default: the brands the engines name most in the first run; [find-competitors](../find-competitors/SKILL.md) finds them if the user has none.
- **Prompts**: five to ten. Default: built in step 1.
- **Engines**: the default five (chatgpt, claude, gemini, perplexity, ai_overview); add `ai_mode` if the user cares about Google's AI Mode (2 credits a prompt more).
- **Market**: `location` and `language` if not the United States and English. Answers differ by country.
- **Once or tracked**: a one-off check, or the same set on a schedule (see [On a schedule](#on-a-schedule)).
- **Budget**: a default check costs about 45 + 45 + 10 x 18 = 270 credits: two prompt searches and ten prompts on five engines. With a prompt set already chosen, 180. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Build the prompt set.** A prompt is a question a buyer asks an engine, not a keyword. A good set has five to ten prompts and mixes four kinds: category ("best CRM for a two-person agency"), problem ("how do I stop leads going cold"), comparison ("HubSpot vs Pipedrive for startups") and brand ("is Acme any good"), weighted to category and problem: brand prompts alone flatter the user, and category and problem prompts are where buyers choose. Build it from the user's own words, question keywords from `seo_search_keywords`, and `aeo_search_prompts` (prompts observed in the provider's index, with `ai_search_volume`, a People Also Ask proxy and not query logs): once with `domain` set to the user's site (45 credits for 50 rows) for the prompts where the index already sees the site cited (`mentions_brand`) and the domains cited beside it (`cited_domains`), and once with `keyword` set to the category (45 credits) for the prompts buyers ask in the category. Show the set to the user before the paid run, and keep it fixed once chosen, so runs compare.
2. **Run the answers.** `aeo_run_ai_answers` with the `prompts`, `brands` (the user first, then up to nine competitors, names or domains; ten at most) and `engines` (18 credits per prompt on the default five). It is a task: it returns a `task_id`; call `get_task` (free) after `poll_after_s`, and again until the result is there. A live answer can take two minutes per engine.
3. **Score it.** For each row (one prompt on one engine), read `mentions[]` (mentioned, position, cited) and `citations[]` (domain, url). A mention is the brand in the answer text; a citation is a link to its domain. Leave out rows whose `answer` is null. For each brand compute:
   - **Mention share**: rows where it is mentioned, over rows answered.
   - **Average position** when mentioned (1 is named first).
   - **Citation share**: rows where its domain is cited, over rows answered.

   Compute each per engine, then overall. Count the domains cited most across all rows.
4. **Name the gaps.** The prompts where a competitor is mentioned and the user is not; the domains cited in those rows; engines where the user is absent everywhere; any answer that states something wrong about the user.
5. **Deliver** three tables and a short list:
   - Brands: brand, mention share, average position, citation share, each overall and per engine.
   - Prompts: prompt, engine, user mentioned (position), user cited, the first competitor named, the top three cited domains.
   - Cited domains: domain, rows citing it, prompts citing it, whether it ever cites the user.
   - The three biggest gaps, each with the skill that fixes it: [build-ai-citations](../build-ai-citations/SKILL.md) for sources that cite competitors, [find-best-of-lists](../find-best-of-lists/SKILL.md) for lists in the citations, [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md) for wrong claims. When the user's site is never cited: [check-ai-crawler-access](../check-ai-crawler-access/SKILL.md) if it fails a `blocks_citations` check; if it passes and the user is named, build-ai-citations for the sources the engines read instead, plus an answer page from [create-seo-content-plan](../create-seo-content-plan/SKILL.md) so there is a page of the user's to cite.

   Give the user the `task_id` and the prompt set, so a later check can reuse them.
6. **Track it, if asked.** For the same check on a schedule, set it up as [On a schedule](#on-a-schedule) says; this run is the baseline.

## Judgment

- Answers are live and non-deterministic. One cell (one prompt on one engine) is a sample, not a verdict. Report shares across cells, and act on a gap only when it shows in at least two cells. With ten prompts on five engines, one row moves a share by two points; do not report a change smaller than that as a trend.
- A brand prompt ("is Acme good") almost always mentions Acme. Report brand prompts apart from category and problem prompts, or they inflate the share.
- An engine that mentions the user but never cites the site is reading about the user elsewhere. Those third-party sources are what [build-ai-citations](../build-ai-citations/SKILL.md) works on.
- `ai_overview` often shows no answer at all. A null there is Google's choice for that query, not a miss for the brand.
- chatgpt and gemini are what their consumer apps show a person; claude and perplexity are their model APIs with web search on; ai_overview and ai_mode are Google's. Report them separately before any total: an overall share can hide one engine dropping the brand entirely.
- The index behind `aeo_search_prompts` covers ChatGPT and AI Overview only, and its volumes are a proxy. The live run in step 2 is the evidence; the search only helps choose prompts.
- Never post, send or edit third-party pages. The deliverable is a table the user acts on.

## Credits

- `aeo_run_ai_answers` costs, per prompt, 6 credits each for claude and perplexity and 2 each for chatgpt, gemini, ai_overview and ai_mode: 18 credits per prompt on the default five engines, so 180 for ten prompts. One call takes at most ten prompts.
- `aeo_search_prompts` costs 30 plus 30 per 100 rows (45 at the default 50).
- Answers are kept 30 days and never cached, so every new run costs; reuse a run's `task_id` instead of running it again. A failed cell is not charged. Do not re-run a whole set to fill one failed cell; run that prompt alone.

## On a schedule

The server has no scheduler and keeps no history. To track the share over time, settle with the host when it runs and what it stores, then run the same set every one or two weeks and report the change.

- **Cadence**: every two weeks by default; weekly during a campaign to get cited. A shorter gap adds cost and noise, not information. The host runs the schedule: if it has a scheduler (a scheduled task, a routine, a cron job), set the check up there; if it has none, say so: the user asks again each week or fortnight, and the host reruns the check with the stored state.
- **Cost**: ten prompts on the default five engines cost 180 credits a run: about 390 a month every two weeks (about 2.2 runs), about 775 weekly (about 4.3 runs). Every run is charged in full. Chosen at the start, a smaller engine set cuts the cost: chatgpt, gemini and ai_overview alone are 6 credits a prompt, 60 a run. Say both numbers before setting up the schedule, and pass `max_credits` on every call so no run overspends. If the budget is tight, run every two weeks before cutting prompts or engines: a smaller set makes every share noisier.
- **State**: the host stores, where the next run can read it (a file, a doc, a sheet, its memory), each prompt's exact text, the `brands`, the `engines`, `location` and `language`, and per cell whether each brand was mentioned, its position, whether it was cited, and the cited domains, with each run's date and its credits (`meta.credits_charged`). Without the stored state every run is a first run.
- **Baseline**: the first run sets what later runs compare against; nothing in it is new, and the first report says so. A prompt added later starts its own baseline; report it apart until it has three runs. Changing the engines changes every share.

Each run:

1. **Run** `aeo_run_ai_answers` with the stored `prompts`, `brands` and `engines`, and read it with `get_task`.
2. **Score** as step 3 does, and note the answer rate per engine (cells with an answer over cells asked) apart from the shares; report `ai_overview`'s answer rate on its own line rather than letting nulls read as losses.
3. **Compare** each brand's shares with the last run and the average of the last three. Lead with what changed; levels come second. Flag a mention share that moves 10 or more points, or smaller moves that hold for three runs; read the three-run average for the trend. List the cells that flipped for the user (mentioned before, absent now, or the reverse) only when the flip held for two runs. List the cited domains new this run, above all those citing competitors and not the user: those go to [build-ai-citations](../build-ai-citations/SKILL.md).
4. **Deliver and store.** Tables: brands (mention share, citation share and average position, each this run, last run and three-run average, overall and per engine), prompts that changed for two runs (prompt, engine, before, now, who took the place), and new cited domains (domain, prompts citing it, whether it cites the user). A quiet run is one line ("no change since the last run"). The host appends this run's cells to the history with the date and the credits.

A change in shares says what moved, not why; the why is [build-ai-citations](../build-ai-citations/SKILL.md), [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md) or [check-ai-crawler-access](../check-ai-crawler-access/SKILL.md). The server never sends alerts: if the host has an email, chat or notification tool, offer to send the table there.

## Related skills

- A plan to raise the share: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
- Acting on the gaps: [build-ai-citations](../build-ai-citations/SKILL.md), [find-best-of-lists](../find-best-of-lists/SKILL.md), [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md), [check-ai-crawler-access](../check-ai-crawler-access/SKILL.md).
- Which of the site's own Google keywords show an AI overview: [check-ai-overviews](../check-ai-overviews/SKILL.md).
- AI visibility beside ranks, mentions and competitors in one digest: [write-weekly-report](../write-weekly-report/SKILL.md).
- Who the competitors are: [find-competitors](../find-competitors/SKILL.md).
