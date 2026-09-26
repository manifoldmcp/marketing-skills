---
name: ai-search
description: AI search visibility with the manifold tools. Checks what ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews and AI Mode answer for a buyer's questions, which brands they mention and which sources they cite, then finds the sources to win (citation building), the best-of lists the engines cite, the wrong things they say about the brand, and whether the site lets AI crawlers in, and plans an AI search strategy. Use when the user asks about GEO, AEO, LLM SEO, AI SEO, generative engine optimization, answer engine optimization, AI visibility, share of voice in AI answers, "does ChatGPT recommend us", getting cited or mentioned by ChatGPT, Perplexity or AI Overviews, LLM citations, AI answers that are wrong or outdated about the brand, GPTBot, OAI-SearchBot or PerplexityBot access, llms.txt, or AI crawler readiness. Recurring AI visibility tracking belongs to the monitoring group.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# AI search

Every job here starts from the same evidence: a set of prompts a buyer would ask, run live on the AI engines, with the brands they mention and the sources they cite. The playbooks differ in what they do with it.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `aeo_run_ai_answers`, `aeo_search_prompts` and `aeo_get_site_readiness` (hosts often add a prefix, for example `mcp__manifold__aeo_run_ai_answers`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Citation building also needs the `leads_*` tools for contacts. If they are missing, the leads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the source list without contacts.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only asks "how do we show up in ChatGPT", start with the visibility check.

| Job | The user says | Open |
|---|---|---|
| AI search strategy: where the brand stands in AI answers, and a 90-day plan | "GEO strategy", "AEO plan", "how do we get recommended by ChatGPT", "AI search strategy for the next quarter", "LLM SEO plan" | [references/strategy.md](references/strategy.md) |
| Visibility check: which engines mention and cite the brand for buyer prompts, against competitors | "does ChatGPT recommend us", "AI visibility audit", "share of voice in AI answers", "are we in Perplexity's answers", "which AI engines mention us" | [references/visibility-check.md](references/visibility-check.md) |
| Citation building: the sources engines cite for competitors and not for the user, with who to contact | "get cited by ChatGPT", "which sources do AI engines cite for our category", "earn AI citations", "why does Perplexity cite our competitor and not us" | [references/citation-building.md](references/citation-building.md) |
| Best-of lists AI engines cite: the listicles in AI answers, to get included | "which best-of lists does ChatGPT use", "lists AI engines cite for best X", "get into the roundups that Perplexity quotes" | [references/best-of-lists.md](references/best-of-lists.md) |
| Incorrect AI answers: what the engines get wrong about the brand, where it comes from, and the fix | "ChatGPT says the wrong price", "AI answers are outdated about us", "Gemini confuses us with another company", "fix what AI says about our brand" | [references/incorrect-answers.md](references/incorrect-answers.md) |
| Site AI readiness: whether AI crawlers can reach and read the site | "is our site ready for AI search", "do we block GPTBot", "are we blocking ChatGPT at Cloudflare", "do we need an llms.txt", "AI crawler audit" | [references/site-readiness.md](references/site-readiness.md) |

## Shared rules

### Prompts and brands

- A prompt is a question a buyer asks an engine, not a keyword. A good set has five to ten prompts and mixes four kinds: category ("best CRM for a two-person agency"), problem ("how do I stop leads going cold"), comparison ("HubSpot vs Pipedrive for startups") and brand ("is Acme any good"). Brand prompts alone flatter the user; category and problem prompts are where buyers choose.
- Build the set from the user's own words, `aeo_search_prompts` (prompts observed in the provider's index, with `ai_search_volume`, a People Also Ask proxy and not query logs), and question keywords from `seo_search_keywords`. Keep it fixed once chosen, so runs compare.
- Pass the user's brand and up to nine competitors in `brands` (names or domains; ten at most). A mention is the brand in the answer text; a citation is a link to its domain (`cited` on each entry in `mentions[]`).

### Reading answers

- `aeo_run_ai_answers` is a task: it returns a `task_id`; call `get_task` (free) after `poll_after_s` until the result is there. Results are kept 30 days and never cached, so every new run costs; reuse a run's `task_id` instead of running it again.
- Answers are live and non-deterministic. One cell (one prompt on one engine) is a sample, not a verdict. Report shares across cells, and act on a gap only when it shows in at least two cells.
- `answer` null means that engine showed no AI answer for the prompt (common for `ai_overview`). Leave those cells out of every share.
- chatgpt and gemini are what their consumer apps show a person; claude and perplexity are their model APIs with web search on; ai_overview and ai_mode are Google's. Report them separately before any total.

### Credits

- `aeo_run_ai_answers` costs, per prompt, 6 credits each for claude and perplexity and 2 each for chatgpt, gemini, ai_overview and ai_mode: 18 credits per prompt on the default five engines, so 180 for ten prompts. Say it before the run; if the user names a budget, pass `max_credits` on every call.
- `aeo_search_prompts` costs 30 plus 30 per 100 rows (45 at the default 50). `aeo_get_site_readiness` and `seo_get_page` are free but rate limited.
- A failed cell is not charged. Do not re-run a whole set to fill one failed cell; run that prompt alone.

### Handoff

- Never post, send or edit third-party pages. The deliverable is a table the user acts on.
- The server keeps no history. The host keeps the prompt set, the `task_id` and the shares, so a later check can compare; for a check on a schedule, use the [monitoring](../monitoring/references/ai-visibility-tracking.md) group's AI visibility tracking.

## Other groups

- The contact steps (find an address at a site, verify it) and best-of lists that rank on Google: the [link-building](../link-building/SKILL.md) group.
- Pages that answer the prompts, comparison pages and technical SEO: the [seo](../seo/SKILL.md) group.
- Reddit threads the engines cite: the [reddit](../reddit/references/ai-cited-threads.md) group. YouTube videos they cite: the [youtube](../youtube/references/ai-cited-videos.md) group.
- AI visibility on a schedule: the [monitoring](../monitoring/references/ai-visibility-tracking.md) group.
- Who the competitors are, when the user does not know: the [competitors](../competitors/references/find-competitors.md) group.
