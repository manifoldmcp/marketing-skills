# Lists AI engines cite

When a buyer asks an engine for "the best X", the answer is often built from a handful of listicles, and the engines cite them. Here the lists are chosen by what the engines cite.

## Find the lists

1. **Get the answers.** Reuse an [check-ai-visibility](../../check-ai-visibility/SKILL.md) run with `get_task` (free), or run `aeo_run_ai_answers` with the category prompts and the `brands` (18 credits per prompt on the default five engines), then `get_task` after `poll_after_s` until the result is there. Leave out rows whose `answer` is null.
2. **Add Google's AI overviews.** `seo_get_serp` with `ai_overview: true` for each category prompt written as a keyword (2 credits each). `ai_overview.references` are the pages Google's answer cites, and the organic results show which of them also rank.
3. **Pick the lists.** From the citations in step 1 and the references in step 2, keep the pages whose title reads as a list on a publisher. For each, count the engines and prompts citing it. If no cited page reads as a list, stop and say so: the engines answer these prompts from other sources, which [build-ai-citations](../../build-ai-citations/SKILL.md) works on.

## Rank

- Order by engines citing, then prompts. A list three engines cite for the category prompt outranks any list with more traffic.
- Skip the domain rank floor: here the citations are the proof a list matters.

## Columns

Engines citing, prompts citing, cited in Google's AI overview (yes or no), organic position if it ranks.

## Judgment

- Answers are live and non-deterministic. A list cited on two or more engines or for two or more prompts is a target; one cited once may be gone next run.
- A list that already names the user but low down is still worth an email: engines read order, and a line on what the user does best can move it up.
- Lists refresh slowly and engines refresh their sources at their own pace. Re-run the same prompts after four weeks to see whether an inclusion changed the answers.
- For lists that rank on Google but that no engine cites, use [lists that rank](ranking-lists.md).
