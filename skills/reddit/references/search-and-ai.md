# Reddit in Google and AI answers

Which engines still read and show Reddit, which threads they show for the user's category, and when taking part in those threads is worth it. Checked October 2026; these numbers moved twice in a year, so re-check before quoting.

## Where Reddit still counts

| Surface | Reddit as a source, autumn 2026 | Source |
|---|---|---|
| Google organic | 4th most visible domain in US Google; peaked October 2025, trend negative since | Sistrix, data of 28 July 2026 |
| Google AI Overviews | Roughly stable: -11% (Promptwatch) to +2% (Qwairy) through August 2026 | Promptwatch, Qwairy |
| Google AI Mode | Down 30% to 44% in August 2026, still a major source; 17% of AI Mode citations in our October runs | Promptwatch, Qwairy, Manifold |
| Perplexity | Up: Reddit citations roughly doubled from late July 2026 | Pierview, 44,488 citations |
| ChatGPT | Collapsed in August 2026: 3.83% to 0.52% of citations (Promptwatch), -95% in a fixed brand cohort (Qwairy), -94% (Pierview) | as listed |
| Gemini app | Down about 39% (Pierview); other trackers report it stable | Pierview, The Drum |
| Claude | Never cites Reddit (Qwairy); none in our runs | Qwairy, Manifold |

- **Why ChatGPT dropped it:** from 8 August 2026 its searches began restricting themselves to named sites with `site:` (0.37% to 16.8% of fan-out queries in a day, Promptwatch; to 23% by mid-August, Qwairy), mostly official brand, government and institutional sites. Reddit was not penalised; it stopped being among the sites ChatGPT chose to search. The timing matches GPT-5.6 becoming the default. OpenAI says it sets no fixed visibility for any site and still cites Reddit. Whether Reddit still shapes answers without being cited is unmeasured; where an engine reports `search_results[]` (pages retrieved, cited or not), count the reddit.com pages there against those in `citations[]`.
- **It happened before.** ChatGPT's Reddit citations fell from about 60% of responses to about 10% in September 2025 when Google removed the `num=100` results parameter (Semrush, 230K+ prompts), and came back only in part, unevenly by category.
- **Every tracker here is a vendor** with an undisclosed or partial sample, and the numbers disagree. Trust the direction and the dates, not the decimals.
- **What Reddit says:** the ChatGPT change has "no meaningful impact" on its business, and in June 2026 that there is no guarantee Reddit content makes its way into LLMs. Reddit licenses its data to Google (since February 2024, Google's announcement of 22 February) and OpenAI (since May 2024, announced 16 May), and sues scrapers that take it without a licence.

So for AI visibility, Reddit is a Perplexity and Google lever in late 2026, not a ChatGPT one. For the engines as a whole, see [ai-search](../../ai-search/SKILL.md).

## What kind of thread gets shown

- Of 1,000 AI chatbot prompts that mention or cite Reddit, 36% asked for a recommendation and 25% for a guide; comparisons were 5% and "alternatives" 1% (Sistrix, July 2026). The threads engines lean on answer "what should I use" and "how do I", in first person.
- Perplexity reads about ten pages per answer and cites three or four, and first-person experience threads keep fitting that (Pierview's reading; no direct measure).
- Google's discussions and forums block shows threads for queries with experience intent; `seo_get_serp` returns each as a row with `type: "discussions_and_forums_element"` at the block's rank.

## Finding the threads that matter

1. **AI answers:** an existing `aeo_run_ai_answers` `task_id` (kept 30 days; `get_task` is free), or a new run on five to ten buyer prompts (27 credits a prompt on the default five engines; a cheaper first pass is five prompts on `engines: ["perplexity", "ai_mode"]`, the engines that still cite Reddit, at 8 credits a prompt). From each row's `citations[]`, keep those on reddit.com (www, old and bare are the same thread), with the prompt, engine and whether the user was named. Leave out rows whose `answer` is null.
2. **Google:** `seo_get_serp` per keyword with `ai_overview: true` (2 credits): the discussions rows, organic rows on reddit.com, and Reddit URLs among the `ai_overview` references.
3. **Rank** one row per thread by the number of surfaces showing it, then prompts. A thread three surfaces show for four prompts is the core; one shown once is noise unless it is the only Reddit source for a prompt that matters.
4. **Read the top 15:** `reddit_get_post` for `created_at`, `locked`, `archived`; `reddit_get_comments` for which products the top comments recommend and whether the user is named. Then the rules verdict per community from [rules](rules.md#the-rules-check).
5. **Deliver** a table: title, URL, subreddit, shown by, prompts and keywords, age, open (yes, locked, archived), products recommended, user named, rules verdict, how to take part. Default cost about 325 credits with a new AI run, about 55 reusing one. If no surface shows Reddit for the category, say so and stop: Reddit is not where these answers come from.

## Taking part

- **Open thread:** a reply that adds what the thread lacks (a newer option, a correction, a number), written as [threads and replies](threads.md#writing-the-reply) says. It starts at the bottom under comments with hundreds of votes; whether engines read that far down is not visible.
- **Locked or archived:** no reply possible. The move is a reply in a newer thread on the same question (`reddit_search_posts` with the question, its `subreddit` and `time_range: "month"`), or a new, better thread where the rules allow it.
- **Already names the user:** held. Report it and spend nothing.
- **Never** start threads from second accounts, ask for upvotes, or pay people to post. This is exactly what moderators and Reddit are now hunting, and on ChatGPT it no longer pays even when it works.
- Re-run the same prompts after two to four weeks, not days; how long a change takes to show is unmeasured.
