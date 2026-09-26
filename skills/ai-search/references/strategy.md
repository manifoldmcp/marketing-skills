# AI search strategy

An AI search (GEO, AEO) strategy answers three questions: for which buyer questions the brand should be named, where it stands on each engine today, and which of this group's playbooks close the gap fastest. It ends in a 90-day plan measured on a fixed prompt set.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: the buyer questions the brand should win, and what winning means. Default: named in the answer for its category and problem prompts on ChatGPT and Google's AI Overview, the two engines most buyers see.
- **Stage**: a new brand the engines barely know, or an established one. Step 2 shows it.
- **ICP**: who buys, so the prompts are theirs.
- **Budget**: credits for the research (this playbook costs about 270) and for re-measuring (180 a run for ten prompts). Default: 1,000 credits over 90 days.
- **Team**: who can edit the site, write pages and do outreach. Default: one marketer, a developer on request.
- **Competitors**: up to nine. Default: the brands the engines name most in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 270 credits. `aeo_get_site_readiness` on the site (free); `aeo_search_prompts` on the site's domain and on the category keyword (45 credits each) to choose ten prompts per the [router](../SKILL.md#prompts-and-brands); then `aeo_run_ai_answers` with those prompts, the user and competitors as `brands`, on the default five engines (180 credits), read with `get_task`. Score it as the [visibility check](visibility-check.md) does in its step 3: mention share, average position and citation share per brand and engine.
3. **Gaps.**
   - Against competitors: the prompts and engines where a competitor is named and the user is not, and the leader on each engine.
   - Sources: the domains cited in those answers, by type (editorial, lists, Reddit, YouTube, review sites, competitor pages).
   - Site: any `blocks_citations` check that fails.
   - Accuracy: any answer that states something wrong about the user.
   - Market: prompts with `ai_search_volume` where no brand is named consistently; open ground.
4. **Tactics.** Choose from this group, in this order, and say why each fits the numbers:
   - [Site readiness](site-readiness.md) when any `blocks_citations` check fails: nothing else works until engines can read the site.
   - [Incorrect answers](incorrect-answers.md) when an answer gets a buying fact wrong.
   - [Citation building](citation-building.md) when competitors are named on sources the user is absent from.
   - [Best-of lists](best-of-lists.md) when lists dominate the citations for category prompts.
   - [Visibility check](visibility-check.md) is the measurement: the same prompt set at day 30, 60 and 90.
   - Pages on the user's site that answer the prompts directly are content work for the [seo](../../seo/references/content-plan.md) group; add them when the user's site is never cited even where it is named.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - KPIs the tools re-measure on the same prompt set: mention share and citation share per engine (`aeo_run_ai_answers`), cited sources won (the rows of the citation building table that now cite the user), readiness checks passing (`aeo_get_site_readiness`).
   - A typical shape: by day 30, readiness fixed, errors corrected and the first 15 sources contacted; by day 60, list inclusions and new answer pages live; by day 90, a second round of sources and the re-measure. For a check on a schedule, the [monitoring](../../monitoring/references/ai-visibility-tracking.md) group keeps it running.
   - **Deliver** one document: the inputs with defaults marked, the prompt set, the baseline table (brand by engine: mention share, position, citation share), the gaps, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- Keep the prompt set fixed for the whole horizon. Changing prompts mid-plan makes every comparison meaningless.
- Engines refresh sources at different speeds. Engines that search live move within weeks; answers that come from training data move on the model's release cycle. Say which is which when setting the 30-day KPI.
- A share on ten prompts moves in steps of two points per row. Set KPI targets that are larger than that noise.
- Google's AI Overview and AI Mode follow organic rankings closely. Where they are the engines that matter, SEO work is AI search work.
- Do not promise a position in an answer. Promise the inputs the engines read: a readable site, correct facts, and presence on the sources they cite.
