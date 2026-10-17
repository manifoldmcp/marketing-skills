# Measuring AI visibility

How to measure whether engines name and cite a brand, without fooling yourself with noise. Read it before any baseline, tracking schedule or "did our fix work" check.

## The prompt set

A prompt is a question a buyer types into an engine, not a keyword. The set decides every number after it.

- **Five to ten prompts for a check, twenty to forty for a programme.** Fewer than five and one answer swings the share; more than ten per call needs a second `aeo_run_ai_answers` call.
- **Mix four kinds, weighted to the first two:** category ("best CRM for a two-person agency"), problem ("how do I stop leads going cold"), comparison ("HubSpot vs Pipedrive for startups"), brand ("is Acme any good"). Brand prompts almost always name the brand, so they flatter the share; report them apart.
- **Write them the way buyers talk:** from sales calls, support tickets and Reddit threads, not from the keyword list. `aeo_search_prompts` shows prompts its index has seen on ChatGPT and Google's AI Overview, with `ai_search_volume`, a People Also Ask proxy, not query logs. Use it to choose prompts, never as demand data.
- **Freeze it.** Changing a prompt's wording changes the answer. A prompt added later starts its own baseline and is reported apart until it has three runs.

## Four numbers

Per engine first, then overall. An overall share can hide one engine dropping the brand entirely.

- **Mention share:** answers that name the brand, over answers given.
- **Average position:** where the brand sits when named (1 is first).
- **Citation share:** answers that link the brand's own domain, over answers given.
- **Share of voice:** the brand's mentions over all tracked brands' mentions in the same answers.

Leave out cells whose `answer` is null. A null on `ai_overview` or `ai_mode` is Google choosing not to show an AI answer for that query, not a loss for the brand; report the answer rate on its own line.

## Noise

Answers are sampled, not looked up. The same prompt on the same engine a minute apart names a different set of brands and cites different pages (see [field notes](field-notes.md) for what we measured).

- **Lists almost never repeat.** Across 2,961 runs of 12 prompts on ChatGPT, Claude and Google's AI answers, the chance of the same brand list twice was under 1 in 100, and of the same order about 1 in 1,000; yet a stable core of brands appeared in 55% to 77% of runs (SparkToro and Gumshoe, November to December 2025). So **never report a rank position as a fact**: report how often the brand appears, and position only as an average.
- **Real people word the same need very differently** (average semantic similarity 0.08 in the same study), but the answers still converge on that core. A fixed prompt set is a consistent instrument, not a picture of every buyer; spread it across wordings rather than repeating one.
- **The error bar is wide.** Appearance is a proportion, so its 95% margin is about ±1.96 × √(p(1−p)/n): at a 50% share that is ±14 points on 50 answers, ±10 on 100, ±7 on 200. Ten prompts on five engines is 50 answers. Say the margin beside the share.
- **One cell is an anecdote.** Act on a gap only when it shows in at least two cells: two engines, two prompts, or two runs.
- **Small sets move in big steps.** On ten prompts and five engines one cell is two points of share. Never report a move smaller than the step as a change.
- **Read trends on a three-run average.** Flag a move of ten points or more at once, or a smaller move that holds for three runs.
- **Engines move at different speeds.** Engines that search live (Perplexity, ChatGPT and Claude with search, Google's AI answers) can shift within days of a source changing. An answer with no citations came from the model's training data and moves on the model's release cycle, months not weeks.

## Cadence

- **Every two weeks by default**, weekly during a push to get cited, monthly for a mature brand. Shorter gaps add cost and noise, not information.
- **Re-measure a fix after two to four weeks**, not days.
- The server keeps no history and has no scheduler. The host stores, for each run: the exact prompts, `brands`, `engines`, `location` and `language`, and per cell whether each brand was named, its position, whether it was cited and the cited domains, with the date and `meta.credits_charged`. Without that every run is a first run. If the host has a scheduler (a scheduled task, a routine, cron), set the run up there; if not, say so.

## Cost

`aeo_run_ai_answers` costs, per prompt, 15 credits for claude, 6 for perplexity and 2 each for chatgpt, gemini, ai_overview and ai_mode: 27 per prompt on the default five engines, 270 for ten prompts. A tracked set of ten prompts every two weeks is about 585 credits a month. Cut cost by dropping engines the buyers do not use (chatgpt, gemini and ai_overview alone are 6 a prompt) before cutting prompts: fewer prompts make every share noisier. Results are kept 30 days and never cached, so reuse a `task_id` rather than re-asking, and re-run only the prompt whose cell failed.

For Google's AI Overview alone, `seo_get_serp` with `ai_overview: true` is cheaper and also shows the organic results the overview drew on.

## Without the connector

The method works by hand, slowly: ask each engine every prompt in a fresh, logged-out or private window (personalisation and memory change answers), note the date, and log per answer whether the brand was named, its position, the competitors named, whether the description was right, and the cited URLs. Say plainly that a hand-run sample of a few answers cannot separate a real gap from noise.
