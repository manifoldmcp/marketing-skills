# Demand check

Whether people want X, from five public signals: Google search volume and its trend, the prompts people ask AI engines, how much Reddit discusses the problem, and how much TikTok and YouTube carry on it. Each signal proves something different and misses something different, so the deliverable states both, then gives a verdict with its confidence and the test that would settle it.

## Inputs to settle first

- **The idea**: the product, feature or category, and the problem it solves. Ask for three kinds of phrase: the solution words ("ai grant writer"), the problem words ("grant applications take forever"), and the incumbent or workaround ("grant consultant", "<incumbent> alternative").
- **Keywords the user already has**: if any, step 1 prices them directly instead of searching.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 20 + 45 + 4 + 2 + 2 = 73 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search demand.** `seo_search_keywords` with the solution phrase as `seed` (10 credits for 100 rows), and again with the problem phrase (10 credits). If the user already has keywords, call `seo_get_keyword_metrics` on them instead (5 plus 5 per 100 keywords, 10 for up to 100); never both on the same keywords. Keep the relevant rows and read: the summed `volume`, `trend[12]` (the last three months against the first three), the share with commercial or transactional `intent`, and `cpc`. Add `ai_volume: true` on `seo_get_keyword_metrics` (4 plus 4 per 100 keywords more) when the question is also whether AI engines see the term.
2. **AI prompts.** `aeo_search_prompts` with the category as `keyword` (45 credits for the default 50 rows, `engine: "chatgpt"`). Read the prompts people ask, their `ai_search_volume` and `cited_domains[]`. Prompts asking for a recommendation ("best tool for...", "how do I...") are the ones with buying intent; `cited_domains[]` shows who answers them today.
3. **Reddit.** `reddit_search_subreddits` with the problem phrase (1 credit): which communities discuss it and how many posts in the sample. `reddit_search_posts` for the problem phrase and for the solution phrase (1 credit each), plus a second page of the problem phrase with `meta.cursor` (1 credit), all with the relevance sort and no time limit per the [router](../SKILL.md#reddit-search). Count the on-topic rows, how many have a `created_at` in the past month and in the past year, their `comments`, and above all the posts asking for a solution ("is there a tool", "looking for", "would pay for").
4. **Video.** `tiktok_search_videos` and `youtube_search_videos` with the solution phrase and with the problem phrase, `since: "year"` (1 credit a page each). Read how many results on the page are on topic, their `views`, how recent they are, and what kind they are: creators reviewing products means a market exists; creators explaining the problem means awareness without a product yet.
5. **Weigh.** For each signal, write what it measured and what that proves. Then give one verdict: **strong** (volume with a flat or rising trend, recommendation prompts, and Reddit posts asking for a tool), **emerging** (little volume but a rising trend, active Reddit or video talk, few products), **crowded** (strong demand, high `cpc`, many products reviewed on video), or **weak** (none of these). Give the confidence (high, medium, low) from how many signals agree.
6. **Deliver** a table: signal, tool, the number, what it proves, what it cannot prove. Then the verdict, its confidence, and the one test that would settle what the tools cannot: a landing page with a waitlist, a small ad test, or ten sales calls. The user runs the test; this playbook does not.

## Judgment

- Search volume proves people type the words into Google. It cannot prove they would pay, and it misses a category too new to have a name: zero volume with lively Reddit talk is an early market, not an empty one. `volume` is rounded; under about 50 a month it is noise.
- `trend[12]` shows direction and seasonality. Compare the same months where the product is seasonal, and treat a one-month spike as news.
- A high `cpc` means advertisers earn money on the click, which is evidence of a market and of competition for it at the same time.
- `ai_search_volume` is a People Also Ask proxy, not query logs. Use it to compare prompts, not to size demand.
- Reddit shows the problem in people's words and who asks for help. Its search is ranked and never complete, and one heated thread is not a market.
- TikTok and YouTube views measure attention, not intent ([router](../SKILL.md#signals-and-their-limits)). They follow trends and entertainment: the weakest proof of intent to buy, and the best early read on awareness.
- No signal here proves willingness to pay ([router](../SKILL.md#signals-and-their-limits)). Say so in every verdict, and make the settling test the first action.
- To size the market in companies rather than searches, use [leads market size](../../leads/references/market-size.md); to see who already serves it, [market map](market-map.md).
