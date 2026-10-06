---
name: fix-wrong-ai-answers
description: When the user wants to fix what AI engines get wrong about their brand. Runs brand prompts on ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews and AI Mode, checks every claim against the user's fact sheet, traces each wrong, outdated or confused claim to the sources the answer cites, and gives a fix and an owner per error. Also use when the user mentions ChatGPT says the wrong price, AI answers are outdated about us, Gemini confuses us with another company, fix what AI says about our brand, or check what AI engines say about us and flag anything wrong. Whether engines mention the brand at all goes to check-ai-visibility, sources that cite competitors instead to build-ai-citations, and crawlers blocked from the site to check-ai-crawler-access.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Incorrect AI answers

AI engines sometimes state the wrong price, a feature the product dropped, the wrong founder, or mix the brand up with another company. This skill finds which answers are wrong, on which engines, traces each error to the sources those answers cite, and gives a fix per error. It hands back an error table in the order to fix them.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `aeo_run_ai_answers` and `aeo_get_site_readiness` (hosts often add a prefix, for example `mcp__manifold__aeo_run_ai_answers`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Correction requests to third-party pages use the `leads_*` tools. If they are missing, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the fixes without contacts.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand, the site, the product overview, pricing and plans, features, founding and location, the competitors) from it as the start of the fact sheet; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Brand**: the name and the domain, and any names it is confused with.
- **Fact sheet**: the facts that must be right: what the product does, who it is for, pricing and plans, key features and integrations, founding and location, and anything recently changed. Ask the user for it; every judgment in step 3 is against it.
- **Known errors**: anything the user has already seen an engine get wrong.
- **Competitors**: one or two for comparison prompts.
- **Budget**: about 8 x 20 = 160 credits for eight brand prompts on the five default engines plus `ai_mode`. `seo_get_page` and `aeo_get_site_readiness` are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Write the brand prompts.** Six to ten prompts that make an engine state facts: "What is <brand>?", "How much does <brand> cost?", "Is <brand> good for <use case>?", "Does <brand> integrate with <tool>?", "<brand> vs <competitor>", "<brand> alternatives", plus one prompt per known error.
2. **Run them.** `aeo_run_ai_answers` with the prompts, `brands` set to the user and the competitors, and `engines` set to the default five plus `ai_mode` (20 credits per prompt). It returns a `task_id`; call `get_task` after `poll_after_s` until the result is there. Leave out rows whose `answer` is null.
3. **Check every answer against the fact sheet.** For each row, list the claims about the user and mark each: correct, wrong, outdated, missing (a key fact left out) or confused (a fact about another company). Keep the row's `citations[]` beside each wrong claim: they are the likely source.
4. **Trace the sources.** For the citations behind wrong claims:
   - The user's own pages: `seo_get_page` (free) shows the title, description and headings; an old price or plan name there is often the cause.
   - The user's site as a whole: `aeo_get_site_readiness` (free). If engines cannot fetch or read the site (answer bots blocked, content only in JavaScript), they fall back on what others wrote; [check-ai-crawler-access](../check-ai-crawler-access/SKILL.md) fixes that first.
   - Third-party pages (reviews, directories, old articles, comparison pages): note the page and what it says, as far as its title and headings show.
   - No citation at all: the error likely comes from the model's training data, which the user cannot change quickly.
5. **Plan the fix per error.**
   - Own page: update it, and put the fact in plain text on a crawlable page (pricing, about, a facts or FAQ page); [optimize-page](../optimize-page/SKILL.md) helps with the page itself.
   - Third-party page: ask for a correction through the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps), with the right fact and a source.
   - Review site or directory: update the listing.
   - Training data only: publish the correct fact where engines search. A row with `citations[]` searched the web and moves when its sources change; a row with no citations answered from the model's own knowledge and moves only on a new model release.
6. **Deliver** a table: prompt, engine, wrong claim, correct fact, kind (wrong, outdated, missing, confused), severity (would it lose a sale), cited sources, likely cause, fix, owner. Then the order to fix them in, most severe and most repeated first.

## Judgment

- Confirm an error before chasing it. A wrong claim in one row may be noise; one that shows on two engines or twice on one engine is real.
- Wrong pricing and wrong "does it do X" answers lose sales; a wrong founding year does not. Rank by what a buyer would decide on.
- Confusion with a similar name is fixed by distinctness: the full product name, the category and the domain together on the user's pages and profiles. `aeo_get_site_readiness` returns the homepage's `same_as`; if it is empty, add Organization schema whose `sameAs` lists the user's LinkedIn, Crunchbase, Wikidata and review-site profiles, which ties the name to one entity.
- The server cannot report an error to an engine. The engines' own feedback buttons exist, but fixing the sources is what changes answers that search the web.
- Never post, send or edit third-party pages. The deliverable is a table the user acts on.
- Re-run the same prompts two to four weeks after the fixes. [check-ai-visibility](../check-ai-visibility/SKILL.md) can keep that on the host's schedule.

## Related skills

- Whether the engines mention the brand at all, against competitors: [check-ai-visibility](../check-ai-visibility/SKILL.md).
- Sources that cite competitors and not the user: [build-ai-citations](../build-ai-citations/SKILL.md). Crawlers blocked from the site: [check-ai-crawler-access](../check-ai-crawler-access/SKILL.md).
- A page that mentions the brand with a wrong fact and no link: [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md).
