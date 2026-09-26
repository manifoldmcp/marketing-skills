# Content ideas

Good ideas answer questions people already ask. This playbook collects the questions and the demand around a topic from Google, AI engines, Reddit, YouTube and TikTok, merges the same question across sources, and gives each idea the channel and format it fits and the evidence behind it. It ends in an idea table, not drafts.

## Inputs to settle first

- **Topic**: one to three seeds in the buyer's words: the problem and the category ("cash flow for freelancers", "invoice software"). Ask; the brand name alone finds nothing for a new product.
- **Channels**: the ones the user posts on, or the ranked table from [channels](channels.md). Default: let the evidence pick, and say which channels it pointed to.
- **Audience**: who reads, and how much they know: new to the problem, comparing tools, or already a customer. It sets the angle.
- **Count**: default 20 ideas.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 10 + 36 + 2 + 3 + 6 + 1 = 68 credits for two seeds. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Questions on Google.** `seo_search_keywords` with each seed (10 credits each). Keep the keywords above the Google floor in the [router](../SKILL.md#evidence) that read as a question or a choice: how, what, why, can, best, vs, alternatives, template, examples. Note `intent` (informational wants an answer, commercial wants a comparison) and a rising `trend[12]`.
2. **Questions to AI engines.** `aeo_search_prompts` with `keyword` set to the main seed and `limit: 20` (36 credits). Prompts are longer and more specific than keywords ("best invoicing app for a freelancer with clients in Europe"); each is a candidate idea as asked. `answer_preview` shows what the engines say now, which is the answer to beat.
3. **Questions on Reddit.** `reddit_search_posts` with the main seed, `sort: "relevance"` and `time_range: "year"` (1 credit a page, two pages), then order the rows by `comments` yourself. Check the rows are on topic, as the search rules in the [reddit router](../../reddit/SKILL.md) describe; other sorts can drop the query. Keep threads whose title is a question, a complaint or a choice, with 10 or more comments. For the three busiest, `reddit_get_comments` (1 credit each): where the replies disagree, there is an opinion post; where they repeat one answer, there is a how-to. Keep the thread's wording: it is the audience's own.
4. **What already works on video.** `youtube_search_videos` and `tiktok_search_videos` with the three strongest questions from steps 1 to 3, `since: "year"` and `sort: "popular"` (1 credit a page each). Read titles, views and authors. A question with strong Google or Reddit demand and no good video is an open slot; a question where the top videos have large views proves the format and shows the angle to beat.
5. **LinkedIn, for a business audience.** `linkedin_search_posts` with the main seed and `since: "month"` (1 credit a page). Read which posts earn comments: a practitioner's story, a framework, a hot take. Skip this step for a consumer product.
6. **Merge into ideas.** One idea per question, with every source that shows it. For each, choose:
   - **Channel and format**, from where the evidence is: Google volume above the floor means an article (hand it to the `seo` group's brief); strong videos mean a YouTube tutorial or a short video; a busy Reddit thread means a reply there and an opinion post elsewhere; LinkedIn posts with comments mean a text post or a carousel; an AI prompt means a page that answers it directly. Where the format is unclear, `seo_get_serp` on the question (1 credit) shows what Google rewards: a `video` entry in `features[]`, Reddit threads, or articles only.
   - **Angle**: an answer, a comparison, an opinion, a story, or data.
   Rank by the number of sources first, then by the strongest demand number.
7. **Deliver** a table of the top ideas (default 20): idea (a working title), the question as people ask it, sources with their numbers (Google volume, AI prompt, Reddit comments, top video views), channel, format, angle, one evidence link, and who covers it now (the top result's author or domain). The host writes drafts from a row only when the user asks.

## Judgment

- An idea found in two sources beats a bigger number in one. A question people search on Google, ask ChatGPT and argue about on Reddit is the safest idea in the table.
- The format follows the evidence, not habit. If Google and the video searches both answer a question with videos, a blog post will not win it.
- Do not copy the top video or post. Take the question and answer it better: newer, more specific, with the user's own data or experience.
- Commercial questions ("best X", "X vs Y", "alternatives to X") are the ones closest to a sale. They belong in the table, but comparison pages on the user's site are the `seo` group's comparison pages.
- A rising `trend[12]` is worth an early slot; a falling one needs a reason to stay.
- The tools see demand, not conversion. Say which ideas the evidence supports; the user's analytics later say which ones sold.
- Ideas for one platform only (TikTok hooks, YouTube video ideas, LinkedIn post formats) are those groups' playbooks; this one mixes channels.
