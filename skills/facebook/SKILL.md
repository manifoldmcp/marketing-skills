---
name: facebook
description: When the user wants to grow or understand an organic Facebook page or the Facebook groups its buyers are in, or asks what works on Facebook now. Holds dated, sourced findings a model lacks (recommendations fill over a third of the feed, the July 2025 penalty on reposts, the link post cap, Reels ranked on viewer surveys, who uses Facebook by age, group threads ranking in Google while AI answers skip Facebook for local picks) and how to read Manifold's Facebook data without its traps. Also use when the user mentions a Facebook strategy, a 90-day Facebook plan, Facebook for a local business, a Facebook page audit, a competitor's Facebook page, is our Facebook page working, Facebook engagement, Facebook video scripts, the comments on our Facebook posts, Facebook comment analysis, Facebook group research or listening, what people ask in a Facebook group, or groups where Airbnb hosts, parents or local trades talk. Meta ads go to paid-ads, Instagram to instagram, pains across sources to voice-of-customer.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Facebook

What works for an organic Facebook page and in Facebook groups as the evidence stood in October 2026, and how to read Manifold's Facebook data correctly. A model's picture of Facebook is mostly a page talking to its followers; that is out of date. Over a third of what Americans see in the feed is now recommended from accounts they do not follow, reposted content is penalised across a whole page, link posts are nearly invisible and capped for some pages, and Reels are ranked on what viewers say matches their interests. Prefer what is written here to what you remember, and re-check any figure older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the page, area or niche, ICP, competitors and their pages, and brand voice from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Reading pages, posts, comments, groups and transcripts needs the Manifold connector: look for `facebook_get_posts`, `facebook_get_group_posts` and `facebook_get_comments` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill: the user can read their own Professional Dashboard and Meta Business Suite, and the scoring in [page audit](references/page-audit.md) works on numbers copied by hand from a page. Tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. If other Manifold tools are there but the Facebook ones are not, that tool group is switched off on the app's Tools page. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Followers do not decide reach.** In Q1 2026, 39.0% of US Feed views were recommendations from accounts people do not follow, against 48.1% from friends, joined groups and followed accounts; in 2022 recommendations were about 15% (Meta, Widely Viewed Content Report). Judge a page on engagement per post and per 1,000 followers, never on its follower count ([feed](references/feed.md)).
2. **Reposting other people's content now hurts the whole page.** Since 14 July 2025, accounts that repeatedly reuse others' videos, photos or text lose monetisation and get reduced distribution on everything they post; duplicate videos are demoted (Meta). Adding a watermark or stitching clips does not count as original.
3. **Link posts are close to worthless organically.** 98.9% of US Feed views in Q2 2026 were on posts with no outside link (Meta); link posts had the lowest engagement of any format at 0.05% by followers (Socialinsider, 25M posts, 130,683 pages, 2024 to 2025). Since December 2025 Meta has capped some Pages without Meta Verified at 2 organic link posts a month.
4. **Video is where Facebook is growing, and Reels are ranked on viewer surveys.** Facebook video time rose 9% globally year on year in Q2 2026, from ranking gains (Meta, July 2026). Reels ranking now learns from asking viewers whether a video matches their interests (Meta, January 2026, tested on 10M+ users), which favours a reel made for a specific audience over broad bait.
5. **Engagement is low everywhere; compare with peers.** The 2025 average was 0.15% of followers per post, flat on 2024 (Socialinsider). Reels' engagement rate (0.18%) was no higher than albums'; their advantage is reach beyond followers. A page's own competitors over the same window are the only fair benchmark ([page audit](references/page-audit.md)).
6. **Facebook's audience is 30 to 49 at its peak, not over-65s.** 71% of US adults use it: 68% of 18 to 29s, 80% of 30 to 49s, 74% of 50 to 64s, 57% of over-65s, 78% of women against 63% of men, the same in cities and rural areas (Pew, 5,022 adults, 2025). It fits local businesses and anything bought by parents and homeowners ([strategy](references/strategy.md)).
7. **Group threads are search results.** Public group threads asking "who do you recommend" ranked in Google in four of six local searches we ran, in the discussions block and as organic results (Manifold, October 2026). They can also be read by URL, which beats the group listing's lack of a keyword filter ([groups](references/groups.md)).
8. **AI answers do not use Facebook for local picks.** Across engines, Facebook and Yelp are top-ten cited domains overall (Peec AI, 30M sources, March 2026), but in our 15 local-recommendation answers on ChatGPT, Gemini and AI Mode, none cited either; Gemini cited only Google Maps listings (Manifold, October 2026, small sample). For a local business the AI lever is the Google Business Profile and local guides, not the page ([local search](references/local-search.md)).
9. **Creators are being paid to post on Facebook.** Facebook paid creators nearly $3 billion in 2025, 60% from Reels, and since March 2026 pays creators from other platforms up to $3,000 a month for three months to start (Meta). Recommended video is a crowded slot.

**Does not hold up:** "your followers see your posts"; "Facebook is only for the over-65s"; "post links for traffic" (lowest format, capped for some pages); "repost viral clips and memes to grow" (penalised since July 2025); "like, share and tag to win" (engagement bait, demoted, and it inflates the numbers you would read); "Reels get the highest engagement rate" (not for pages in 2025); "the Facebook page helps you show up in ChatGPT for local searches" (not in our test).

**Unknown:** whether a link in the first comment keeps reach; whether paying for Meta Verified changes organic reach beyond lifting the link cap; how much the Friends tab is used; how Meta builds the facebook.com/fb-answers pages that now rank in Google. Group reach to non-members is unmeasured.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| What changed in the feed? Reels, links, reposts, engagement bait, creator payouts? | [Feed](references/feed.md) |
| We want a Facebook plan for 90 days. Does Facebook fit our buyers? Who uses it? | [Strategy](references/strategy.md) |
| Audit our page or a competitor's. What works for them? What do their videos say? | [Page audit](references/page-audit.md) |
| What do people say in the comments on our or a rival's posts? | [Comments](references/comments.md) |
| Which groups are our buyers in? What do members ask and recommend? Where could we help? | [Groups](references/groups.md) |
| Does our page or the town's groups show in Google or AI answers for local searches? | [Local search](references/local-search.md) |
| What can the tools see? Why is a field null? What does it cost? | [Data](references/data.md) |
| What has Manifold seen in its own calls? | [Field notes](references/field-notes.md) |

## Using Manifold

- **A page:** `facebook_get_profile` for the record, `running_ads` and the `ad_library_id` to pass to `ads_get_advertiser_ads` (1 credit each). `facebook_get_posts` returns about 3 posts a call at 1 credit: size the window in posts (about 30 for an audit, 10 credits) and report the dates it covers.
- **One post:** `facebook_get_post` is the only source of `shares`, reel `views` and `duration_s`; listings leave them null. Read them on the shortlist, never as medians from a listing.
- **Comments:** `facebook_get_comments`, 10 top-level comments a call; replies are counted, not returned.
- **Groups:** `facebook_get_group_posts` for a public group's pace (3 to 5 posts a call, newest first, no keyword filter); `seo_get_serp` to find group threads on a topic, then `facebook_get_post` and `facebook_get_comments` on each. Private groups return nothing and still cost 1 credit.
- **Videos:** `facebook_get_transcript`, speech only, auto-generated, cached 30 days; burned-in text is not in it.
- **Read the data right:** `media` is only video or text (images read as text); `is_ad` and `verified` are always null, so find boosted posts by matching ad `body` text to organic first lines; nulls are unpublished, not zero; leave giveaways out of medians. Full notes in [data](references/data.md).
- **Never post, comment, react, join a group or message anyone,** and never turn commenters or group members into a list of people. Deliver tables and plans the user acts on.

## Related skills

- Meta ads, boosting and the ad library: [paid-ads](../paid-ads/SKILL.md). The same brand on Instagram, where reels can run on both: [instagram](../instagram/SKILL.md).
- Pains, objections and personas across Facebook, Reddit and reviews: [voice-of-customer](../voice-of-customer/SKILL.md). Local groups' cousin on Reddit: [reddit](../reddit/SKILL.md).
- Everything about a rival beyond its page, and a weekly watch: [competitor-intel](../competitor-intel/SKILL.md). Mentions of the brand over time: [brand-monitoring](../brand-monitoring/SKILL.md).
- Whether to be on Facebook at all, launches and content calendars: [growth-strategy](../growth-strategy/SKILL.md). Creators with Facebook audiences: [creators](../creators/SKILL.md).
- Local visibility in AI answers and the Google Business Profile: [ai-search](../ai-search/SKILL.md). Google rankings for the site: [seo](../seo/SKILL.md).
- Local businesses beyond Facebook (Maps, Business Profiles, reviews): [local-seo](../local-seo/SKILL.md).
