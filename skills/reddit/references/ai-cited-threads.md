# AI-cited threads

The Reddit threads that AI engines cite and Google shows for the user's category questions. These threads keep being read, by people and by the next AI answer, long after they are posted. The playbook finds them, ranks them by how many engines and prompts surface them, reads who each one recommends, and checks whether it still takes replies. It ends in a table of threads and how to take part in each.

## Inputs to settle first

- **Answers**: a visibility check from the `ai-search` group, if one exists: its `task_id` (results are kept 30 days, and reading them with `get_task` is free). If there is none, this playbook runs the prompts in step 1.
- **Prompts**: five to ten questions a buyer asks an AI engine about the category. Choosing them is the `ai-search` group's job: build the set as its [visibility check](../../ai-search/references/visibility-check.md) does, or use the user's own.
- **Keywords**: the Google searches behind the same questions. Default: the prompts shortened to keywords ("best crm for agencies").
- **Brands**: the user's brand and up to nine competitors, for `brands` (ten at most).
- **Budget**: a default run costs about 10 x 18 + 10 x 2 + 15 x 2 + 5 = 235 credits (ten prompts on the five default engines, ten SERPs with the AI overview, fifteen threads read with their comments, the rules of five communities). With an existing `task_id`, about 55. A cheaper first pass is five prompts on `engines: ["chatgpt", "perplexity"]` at 8 credits a prompt. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Get the AI answers.** With a `task_id`, `get_task` (free). Otherwise `aeo_run_ai_answers` with the `prompts`, `brands` and the default engines (18 credits per prompt, 180 for ten); it returns a `task_id`, and `get_task` reads the result after `poll_after_s`. Leave out rows whose `answer` is null. From each row's `citations[]`, keep those whose `domain` is reddit.com or a subdomain of it (www, old), with the prompt, the engine and the citation `position`. Note whether the user is `mentioned` in that answer.
2. **Get what Google shows.** `seo_get_serp` for each keyword with `ai_overview: true` (2 credits each at the default depth). Keep the rows with `type: "discussions_and_forums_element"` (the threads in Google's discussions and forums block, mostly Reddit), organic rows on reddit.com, and Reddit URLs among the `ai_overview` references. Note each one's `rank`.
3. **Merge and rank.** One row per thread: strip query strings, and treat www, old and bare reddit.com as the same thread. Count the surfaces that show it (each AI engine, Google's results, Google's AI overview) and the prompts or keywords it appears for. Rank by surfaces, then by prompts. A thread cited by three engines for four prompts is the core of the list; one cited once is noise unless it is the only Reddit source for a prompt that matters.
4. **Read each thread.** For the top 15, `reddit_get_post` (1 credit): `created_at`, `score`, `comments`, `locked` and `archived`. Then `reddit_get_comments` (1 credit, one page): which products the top comments recommend, whether the user is named, and whether one competitor dominates.
5. **Check the rules.** For the communities on the list, the verdict from [Subreddit rules](subreddit-rules.md) (`reddit_get_subreddit`, 1 credit each).
6. **Deliver** a table: thread title, URL, subreddit, shown by (engines and Google surfaces), prompts and keywords, age, open (yes, locked, archived), products the thread recommends, user named (yes, no), rules verdict, and how to take part. An open thread goes through the judgment in [Threads to reply](threads-to-reply.md): a reply that adds what the thread lacks. A locked or archived thread cannot take a reply: the move is a new, better thread on the same question in the same community, where its rules allow, or a reply in a newer thread on that question.

## Judgment

- AI answers are live and change between runs. Trust threads that several engines or prompts surface; a single citation may be gone next week.
- A thread engines already cite that names competitors and not the user is the gap. A thread that already names the user is held: report it and spend nothing on it.
- A new reply in a years-old thread starts at the bottom, under comments with hundreds of votes. It is worth writing only when it adds something the thread lacks (a newer option, a correction, a number). Whether the engines read that far down is not something the tools can see.
- Never ask anyone to upvote a reply or use a second account to agree with it. Vote manipulation breaks Reddit's rules and gets the account banned.
- How long an engine takes to pick up a change is not visible. Re-run the same prompts after two to four weeks, not days; on a schedule, that is the `monitoring` group's [AI visibility tracking](../../monitoring/references/ai-visibility-tracking.md).
- Sources other than Reddit (articles, lists, videos, review sites) belong to the `ai-search` group's [citation building](../../ai-search/references/citation-building.md).
