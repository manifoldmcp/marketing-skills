# Channels

Which channels to use is a question about where buyers already are and what they do there: search Google, ask ChatGPT, argue on Reddit, watch YouTube or TikTok, read LinkedIn. This playbook measures each channel for the user's topic with one or two cheap calls, checks where competitors already show up, and ends in a ranked channel table with the evidence and the effort each channel takes.

## Inputs to settle first

- **Topic**: two or three phrases buyers use for the problem or the category ("expense tracking for freelancers", "invoice app"). Not the brand name: a new brand has no demand of its own yet.
- **Audience**: business or consumer, and the role or the person. It breaks ties between channels the evidence rates alike.
- **Competitors**: two or three, with their domains and, if the user knows them, their handles. Handles usually sit in the footer of the competitor's site (the host can open it if it has a browser), or show up as `author` in the search rows of steps 4 and 5. Default: the brands that recur in steps 2, 4 and 5.
- **Team**: who makes content and what they can make: writing, talking on camera, design. Default: one person, writing only, 4 hours a week. It sets the effort column.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: a default run costs about 2 x 10 + 2 + 36 + 2 + 4 + 2 + 3 x (5 + 4 + 4) = 105 credits for two topic phrases and three competitors. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Google.** `seo_search_keywords` with each topic phrase as `seed` (10 credits each). Sum the `volume` of the keywords that fit, apply the Google floor in the [router](../SKILL.md#evidence), and note the question and comparison keywords (how, what, best, vs, alternatives). Then `seo_get_serp` for the two biggest keywords (1 credit each): a `video` entry in `features[]` means Google shows videos for the query, so YouTube content can rank there too; rows with `type: "discussions_and_forums_element"` are Reddit and forum threads Google puts on page one.
2. **AI engines.** `aeo_search_prompts` with `keyword` set to the main topic and `limit: 20` (36 credits). Count the prompts and read `ai_search_volume`. `cited_domains[]` shows which sources the engines lean on: youtube.com and reddit.com there point to those channels, and a competitor's domain there means it is already in the answers.
3. **Reddit.** `reddit_search_subreddits` with each topic phrase and `time_range: "year"` (1 credit each). Keep communities with 3 or more posts in the sample and read `subscribers`. Several such communities mean Reddit is a channel for answers and replies (the `reddit` group runs it); none means the conversation happens elsewhere.
4. **Video.** `youtube_search_videos` and `tiktok_search_videos` with each topic phrase, `since: "year"` and `sort: "popular"` (1 credit a page each). Read the views of the top 10 and who made them: brands, independent creators, or nobody relevant. Many views and few brands is an opening; when every top video is many months old, nobody serves the topic now.
5. **LinkedIn.** `linkedin_search_posts` with each topic phrase and `since: "month"` (1 credit a page). Count the posts, read likes and comments where published, and note who posts: vendors, practitioners or creators. For a consumer product, add `instagram_search_posts` with the category hashtag (1 credit a page).
6. **Competitor presence.** For each competitor:
   - `seo_get_domain_overview` on its domain (5 credits): `organic_traffic` and `organic_keywords` say whether search is one of its channels.
   - The profile on each platform where it has a handle (1 credit each): `youtube_get_channel`, `tiktok_get_profile`, `linkedin_get_company`, `twitter_get_profile`, or `instagram_get_profile` for a consumer brand. Read `followers` and `posts_count`.
   - A listing on the channels where the account is real (1 credit a page each): `youtube_get_videos`, `tiktok_get_videos`, `linkedin_get_company_posts`, `twitter_get_tweets`. The newest `created_at` says whether it still posts; views say whether anyone watches.

   Also mark where competitors turned up for free in steps 2, 4 and 5 (in `cited_domains[]`, or as `author` in search rows).
7. **Score each channel.** For Google, AI answers, Reddit, YouTube, TikTok, LinkedIn, and Instagram or X where they apply: demand (does it clear the router's floor), competition (absent, present, strong), and effort for this team. A channel with demand and weak competition that the team can make content for ranks first. Recommend one primary and one secondary channel; list the rest as later.
8. **Deliver** a table: rank, channel, demand (the number and its source), best evidence (a keyword, a prompt, a thread or a video URL), competitors present (who, followers or traffic, still active or not), gap (open, contested, crowded), effort for this team (hours a week and the skill it needs), first move (the playbook that starts it, such as [content ideas](content-ideas.md) or the platform group's strategy).

## Judgment

- Do not add the units up. Searches, prompts, comments and views measure different things; each channel clears its own floor or does not, and the ranking comes from demand, competition and effort together.
- A crowded channel is not closed. Take it when demand is large and the team has an angle the incumbents lack; take the open channel first when the team is small.
- Two channels done every week beat five done badly. A team of one gets one primary channel and, at most, one secondary.
- A new category often has no Google volume yet (null or under the floor). Demand shows up first in Reddit threads and AI prompts; weigh those more for a new category.
- X cannot be measured here: there is no search, so only a competitor's X account can be sized. Say so rather than rating X low.
- Facebook groups can be measured only from group URLs the user supplies; the `facebook` group's group mining reads them.
- `seo_get_serp_competitors` returns sites that share search results, not business competitors. Do not use it to name competitors here.
- Paid channels (Meta, Google, LinkedIn, TikTok ads) are the `paid-ads` group; a growth plan by goal across paid and organic is `growth-plan`.
