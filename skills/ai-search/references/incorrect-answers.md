# Incorrect AI answers

AI engines sometimes state the wrong price, a feature the product dropped, the wrong founder, or mix the brand up with another company. This playbook finds which answers are wrong, on which engines, traces each error to the sources those answers cite, and gives a fix per error.

## Inputs to settle first

- **Brand**: the name and the domain, and any names it is confused with.
- **Fact sheet**: the facts that must be right: what the product does, who it is for, pricing and plans, key features and integrations, founding and location, and anything recently changed. Ask the user for it; every judgment in step 3 is against it.
- **Known errors**: anything the user has already seen an engine get wrong.
- **Competitors**: one or two for comparison prompts.
- **Budget**: about 8 x 20 = 160 credits for eight brand prompts on the five default engines plus `ai_mode`. `seo_get_page` and `aeo_get_site_readiness` are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Write the brand prompts.** Six to ten prompts that make an engine state facts: "What is <brand>?", "How much does <brand> cost?", "Is <brand> good for <use case>?", "Does <brand> integrate with <tool>?", "<brand> vs <competitor>", "<brand> alternatives", plus one prompt per known error.
2. **Run them.** `aeo_run_ai_answers` with the prompts, `brands` set to the user and the competitors, and `engines` set to the default five plus `ai_mode` (20 credits per prompt). Call `get_task` after `poll_after_s` until the result is there.
3. **Check every answer against the fact sheet.** For each row, list the claims about the user and mark each: correct, wrong, outdated, missing (a key fact left out) or confused (a fact about another company). Keep the row's `citations[]` beside each wrong claim: they are the likely source.
4. **Trace the sources.** For the citations behind wrong claims:
   - The user's own pages: `seo_get_page` (free) shows the title, description and headings; an old price or plan name there is often the cause.
   - The user's site as a whole: `aeo_get_site_readiness` (free). If engines cannot fetch or read the site (answer bots blocked, content only in JavaScript), they fall back on what others wrote; [site readiness](site-readiness.md) fixes that first.
   - Third-party pages (reviews, directories, old articles, comparison pages): note the page and what it says, as far as its title and headings show.
   - No citation at all: the error likely comes from the model's training data, which the user cannot change quickly.
5. **Plan the fix per error.**
   - Own page: update it, and put the fact in plain text on a crawlable page (pricing, about, a facts or FAQ page); the [seo](../../seo/references/optimize-page.md) group's optimize page helps with the page itself.
   - Third-party page: ask for a correction through the [contact steps](../../link-building/SKILL.md#contact-steps), with the right fact and a source.
   - Review site or directory: update the listing.
   - Training data only: publish the correct fact where engines search, and re-check the engines that search live (claude and perplexity here run with web search on) before the ones that may not.
6. **Deliver** a table: prompt, engine, wrong claim, correct fact, kind (wrong, outdated, missing, confused), severity (would it lose a sale), cited sources, likely cause, fix, owner. Then the order to fix them in, most severe and most repeated first.

## Judgment

- Confirm an error before chasing it. A wrong claim in one row may be noise; one that shows on two engines or twice on one engine is real.
- Wrong pricing and wrong "does it do X" answers lose sales; a wrong founding year does not. Rank by what a buyer would decide on.
- Confusion with a similar name is fixed by distinctness: the full product name, the category and the domain together on the user's pages and profiles.
- The server cannot report an error to an engine. The engines' own feedback buttons exist, but fixing the sources is what changes answers that search the web.
- Re-run the same prompts two to four weeks after the fixes. The [monitoring](../../monitoring/references/ai-visibility-tracking.md) group can keep that on a schedule.
