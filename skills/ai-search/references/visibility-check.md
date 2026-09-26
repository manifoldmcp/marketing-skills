# Visibility check

Where the brand stands in AI answers today: for a fixed set of buyer prompts, which engines mention it, in what position, and whether they cite its site, against the competitors. It is the baseline every other playbook in this group starts from, and the measurement to repeat.

## Inputs to settle first

- **Brand**: the name and the domain, since engines mention names and cite domains.
- **Competitors**: up to nine, names or domains. Default: the brands the engines name most in the first run; the [competitors](../../competitors/references/find-competitors.md) group finds them if the user has none.
- **Prompts**: five to ten, built per the [router](../SKILL.md#prompts-and-brands). Default: built in step 1.
- **Engines**: the default five (chatgpt, claude, gemini, perplexity, ai_overview); add `ai_mode` if the user cares about Google's AI Mode (2 credits a prompt more).
- **Market**: `location` and `language` if not the United States and English. Answers differ by country.
- **Budget**: a default run costs about 45 + 45 + 10 x 18 = 270 credits: two prompt searches and ten prompts on five engines. With a prompt set already chosen, 180. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Build the prompt set.** `aeo_search_prompts` with `domain` set to the user's site (45 credits for 50 rows): the prompts where the index already sees the site cited (`mentions_brand`), and the domains cited beside it (`cited_domains`). Then `aeo_search_prompts` with `keyword` set to the category (45 credits): the prompts buyers ask in the category, with `ai_search_volume`. Choose five to ten that mix category, problem, comparison and brand prompts, weighted to category and problem. Show the set to the user before the paid run.
2. **Run the answers.** `aeo_run_ai_answers` with the `prompts`, `brands` (the user first, then competitors) and `engines` (18 credits per prompt on the default five). It returns a `task_id`; call `get_task` after `poll_after_s`, and again until the result is there. A live answer can take two minutes per engine.
3. **Score it.** For each row (one prompt on one engine), read `mentions[]` (mentioned, position, cited) and `citations[]` (domain, url). Leave out rows whose `answer` is null. For each brand compute:
   - **Mention share**: rows where it is mentioned, over rows answered.
   - **Average position** when mentioned (1 is named first).
   - **Citation share**: rows where its domain is cited, over rows answered.

   Compute each per engine, then overall. Count the domains cited most across all rows.
4. **Name the gaps.** The prompts where a competitor is mentioned and the user is not; the domains cited in those rows; engines where the user is absent everywhere; any answer that states something wrong about the user.
5. **Deliver** three tables and a short list:
   - Brands: brand, mention share, average position, citation share, each overall and per engine.
   - Prompts: prompt, engine, user mentioned (position), user cited, the first competitor named, the top three cited domains.
   - Cited domains: domain, rows citing it, prompts citing it, whether it ever cites the user.
   - The three biggest gaps, each with the playbook that fixes it: [citation building](citation-building.md) for sources that cite competitors, [best-of lists](best-of-lists.md) for lists in the citations, [incorrect answers](incorrect-answers.md) for wrong claims, [site readiness](site-readiness.md) when the user's site is never cited at all.

   Give the user the `task_id` and the prompt set, so a later check can reuse them.

## Judgment

- Answers are non-deterministic. With ten prompts on five engines, one row moves a share by two points; do not report a change smaller than that as a trend.
- A brand prompt ("is Acme good") almost always mentions Acme. Report brand prompts apart from category and problem prompts, or they inflate the share.
- An engine that mentions the user but never cites the site is reading about the user elsewhere. Those third-party sources are what [citation building](citation-building.md) works on.
- `ai_overview` often shows no answer at all. A null there is Google's choice for that query, not a miss for the brand.
- The index behind `aeo_search_prompts` covers ChatGPT and AI Overview only, and its volumes are a proxy. The live run in step 2 is the evidence; the search only helps choose prompts.
