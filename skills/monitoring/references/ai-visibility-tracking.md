# AI visibility tracking

How often AI engines mention and cite the brand for a fixed set of prompts, measured on a schedule and compared over runs. The measurement itself is the `ai-search` group's [visibility check](../../ai-search/references/visibility-check.md): how to build the prompt set, run the answers and score them. This playbook fixes the set, runs it every one or two weeks and reports the change. Answers are live and non-deterministic, so one answer moving means little; the share across the set, over several runs, is the number.

## Inputs to settle first

- **Prompts, brands, engines and market**: as in [visibility check](../../ai-search/references/visibility-check.md), fixed for every run. Default: ten prompts (one `aeo_run_ai_answers` call takes at most ten), the user plus up to nine competitors in `brands`, the five default engines, United States and English.
- **Cadence**: every two weeks by default; weekly during a campaign to get cited. A shorter gap adds cost and noise, not information.
- **Budget**: ten prompts on the five default engines cost 10 x 18 = 180 credits a run: about 390 a month every two weeks, about 775 weekly. Answers are never cached, so every run is charged in full; a cell that fails is not charged. The first run adds about 90 for the prompt searches if no prompt set exists. Chosen at the start, a smaller engine set cuts the cost: chatgpt, gemini and ai_overview alone are 6 credits a prompt, 60 a run. Say both numbers before setting up the schedule; pass `max_credits` if the user gave a budget.

## Steps

1. **Set up (first run only).** Build the prompt set and run the baseline as visibility check does. Store with the host, as in the [router](../SKILL.md#schedule-and-state): each prompt's exact text, the `brands`, the `engines`, `location` and `language`, and set the schedule. From the baseline, store per cell (one prompt on one engine) whether each brand was mentioned, its position, whether it was cited, and the cited domains.
2. **Run.** `aeo_run_ai_answers` with the stored `prompts`, `brands` and `engines` (18 credits a prompt on the default five). It returns a `task_id`; call `get_task` (free) after `poll_after_s`, and again until the result is there. Results stay on the server for 30 days; the host keeps its own copy.
3. **Score.** As visibility check scores it: for each brand, mention share, average position when mentioned and citation share, overall and per engine, leaving out cells whose `answer` is null. Keep brand prompts ("is Acme any good") apart from category and problem prompts. Note the answer rate per engine (cells with an answer over cells asked) apart from the shares.
4. **Compare over runs.** Each brand's shares against the last run and the average of the last three. Flag only what clears the router's [thresholds](../SKILL.md#change-not-level). List the cells that flipped for the user (mentioned before, absent now, or the reverse) only when the flip held for two runs. List the cited domains new this run, and above all those citing competitors and not the user: those go to the `ai-search` group's [citation building](../../ai-search/references/citation-building.md).
5. **Deliver and store.** Tables: brands (mention share, citation share and average position, each this run, last run and three-run average, overall and per engine), prompts that changed for two runs (prompt, engine, before, now, who took the place), and new cited domains (domain, prompts citing it, whether it cites the user). The host appends this run's cells to the history with the date and the credits.

## Judgment

- The same prompt asked twice can name different brands. With ten prompts on five engines, one cell is two points of share: read the three-run average for the trend and treat single-run moves under about 10 points as noise.
- Keep the set fixed. A prompt added later starts its own baseline; report it apart until it has three runs. Changing the engines changes every share.
- `ai_overview` often shows no answer. Report its answer rate on its own line rather than letting nulls read as losses.
- Engines differ: chatgpt and gemini are what their consumer apps show a person; claude and perplexity come from their model APIs with web search on. Report per engine; an overall share can hide one engine dropping the brand entirely.
- A change in shares says what moved, not why. The why is the `ai-search` group's work: citation building for sources, [incorrect answers](../../ai-search/references/incorrect-answers.md) for wrong claims, [site readiness](../../ai-search/references/site-readiness.md) when the site is never cited.
- The cadence is the cost lever. If the budget is tight, run every two weeks before cutting prompts or engines: a smaller set makes every share noisier.
