# Content ideas and the calendar

What to post across channels, and how to lay four weeks of it out so the team can keep it. Good ideas answer questions people already ask; a good calendar is one the team still keeps in week four. One platform in depth (hooks, formats, trends) belongs to that platform's skill; this page mixes channels. Checked October 2026.

## Cadence

What the large datasets say, all observational and all from scheduling-tool users, so they describe accounts that already post:

- **Consistency goes with more engagement per post.** Buffer grouped 100,000+ users by how many of 26 weeks they posted in: those posting in 20+ weeks got about 5.5 times the median engagement per post of those posting in 4 or fewer, and 26% more than those posting in 5 to 19 (January 2025). Engagement per post peaked around week 21, at a median of 4.47 engagements a post: most posts reach very few people, so a plan's numbers should start small.
- **More posts in a week did not cannibalise a LinkedIn account's reach.** Comparing each account's busier weeks with its quieter ones (2M+ posts, 94,000+ accounts), 2 to 5 posts a week added about 1,200 impressions per post over 1 a week, 6 to 10 added about 5,000; returns shrank above that (Buffer, August 2025). Instagram showed the same shape, with 3 to 5 posts a week the point where gains flatten (Buffer, 2M+ posts, 100,000+ accounts, 2025).
- **Formats differ by platform.** In Buffer's 2025 data, LinkedIn carousels had a median engagement rate of 21.8% against 7.4% for video and 6.5% for images; Instagram carousels beat reels on engagement while reels got more reach; on X, text beat images and video; on Threads and TikTok, video led (52M+ posts, rate per reach). Platform skills hold the detail.
- **Nobody can tell you the best time to post.** The tools show when accounts published, not when their audience was online; only the account's own analytics have that. Do not name a best time.

So: a cadence the team can hold for six months beats a burst. Two posts a week for four weeks beat ten in week one and none after.

## Finding ideas

Collect the questions around the topic from several sources, merge the same question across them, and give each idea the channel and format its evidence points to. About 68 credits for two seeds.

- **Google:** `seo_search_keywords` per seed (10). Keep question and choice keywords (how, what, why, best, vs, alternatives, template, examples) above about 50 searches a month; note `intent` and a rising `trend[12]`.
- **AI engines:** `aeo_search_prompts` with `keyword` set to the main seed and `limit: 20` (36). Prompts are longer and more specific than keywords; `answer_preview` is the answer to beat. Run it once per topic, never per idea; `ai_search_volume` is a proxy, never searches.
- **Reddit:** `reddit_search_posts` on the main seed at relevance with `time_range: "year"` (1 a page, two pages), then order rows by `comments` yourself and keep on-topic threads with 10 or more comments that ask, complain or choose. `reddit_get_comments` on the three busiest (1 each): disagreement means an opinion post, one repeated answer means a how-to. Keep the thread's wording; it is the audience's own.
- **Video:** `youtube_search_videos` and `tiktok_search_videos` with the three strongest questions, `since: "year"` and `sort: "popular"` (1 a page). Strong demand elsewhere with no good video is an open slot; big views prove the format.
- **LinkedIn**, for a business audience: `linkedin_search_posts` on the seed with `since: "month"`; read which posts earn comments.

**Merging.** One idea per question, with every source that shows it, ranked by the number of sources first and the strongest number second. A question found in two sources beats a bigger number in one. The format follows the evidence, not habit: Google volume above the floor means an article ([seo](../../seo/SKILL.md)); strong videos mean a video; a busy Reddit thread means a reply there and an opinion post elsewhere; an AI prompt means a page that answers it directly; where unclear, `seo_get_serp` on the question shows whether Google rewards videos, forum threads or articles. Commercial questions ("best X", "X vs Y", "alternatives") sit closest to a sale. Do not copy the top video or post; answer the question better, with the user's own data or experience.

**The idea table:** idea (working title), the question as people ask it, sources with their numbers, channel, format, angle (answer, comparison, opinion, story, data), one evidence link, and who covers it now. A row with no evidence is marked as the user's call.

## Laying out the calendar

1. **Hours into slots.** Divide weekly hours by the effort of each format ([channel selection](channel-selection.md#effort-per-format)) and plan to 80% of the hours; the rest absorbs slips and one reactive post.
2. **Pillars.** Two to four recurring themes (answers, opinions on the category, customer stories, product); each week touches each where slots allow.
3. **One anchor a week:** the highest-ranked idea that fits the main channel, with its cut-downs on the other channels in the days after ([repurposing](repurposing.md)). Ideas with a rising trend or a fixed date go first. When capacity is under one anchor a week, drop to one channel.
4. **Fill and balance.** Remaining slots by idea rank; keep product posts to about one in four (a rule of thumb, not a measured ratio); leave one open slot a week for news or a reply to something that takes off.
5. **Check the load.** No week over the planned hours: move a slot or swap to a cheaper format, never add hours the team does not have.

**Competitors' cadence (optional, about 8 credits):** one listing per competitor per channel (`linkedin_get_company_posts`, `twitter_get_tweets`, `youtube_get_videos`, `tiktok_get_videos` or `instagram_get_posts`, 1 a page); count posts per week from `created_at`. LinkedIn dates are approximate, so count per month there. It shows the floor for being noticed, not a target to match.

**The calendar table:** week, date, day, channel, format, working title, pillar, evidence (the question and its number), repurposed from, owner, effort in hours, status. Under each week, planned hours against capacity. The host keeps it; the server schedules nothing. If the host has a scheduling tool, offer to pass the rows; never post, schedule or publish. Drafts only when asked, written from the evidence column, with no number, quote or result the evidence does not hold.

To review the past four weeks, list the user's own posts with the same listings and compare each with the channel's median.
