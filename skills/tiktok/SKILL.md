---
name: tiktok
description: When the user wants organic growth on TikTok or asks what works there now. Holds dated, sourced findings a model would not know from training (what the 2026 US joint venture changed, what TikTok documents about For You and search, length, watch time and posting frequency from studies of millions of posts, engagement benchmarks by follower tier, the music limits that get brands sued) and how to read Manifold's TikTok data without fooling yourself. Also use when the user mentions a TikTok strategy or 90-day TikTok plan, starting a TikTok account for a brand, how often to post on TikTok, a TikTok audit, what a competitor does on TikTok, TikTok engagement rate, best performing TikToks, TikTok hooks or opening lines, what is trending on TikTok, trending formats or sounds, mining TikTok comments, objections or customer language from TikTok, why a TikTok went viral, TikTok SEO or search, or the TikTok algorithm. TikTok creators go to creators, TikTok ads to paid-ads, Reels to instagram, Shorts to youtube.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# TikTok

What wins organic reach on TikTok as the evidence stood in October 2026, and how to read TikTok numbers without being misled by paid posts, one-off hits and mismatched benchmarks. Much of what a model believes about TikTok is folklore with no source (a 70% completion threshold, "the first 3 seconds decide everything", a best hour to post) or predates 2025, when average views for small accounts fell by more than half while big accounts gained. The US business changed owners in January 2026; what that changed for marketers is less than the headlines suggest. Prefer what is written here to what you remember, and re-check any figure older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the brand, its TikTok handle, ICP, competitors and their handles, the markets it sells in and the goal from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Reading live accounts, niches, transcripts and comments needs the Manifold connector: look for `tiktok_search_videos`, `tiktok_get_videos` and `tiktok_get_profile` (hosts often add a prefix such as `mcp__manifold__`). If other Manifold tools are there but not these, the TikTok tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). If none are there, still answer from this skill and use the by-hand method in [reading the numbers](references/reading-the-numbers.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Reach is decided per video, and small accounts got squeezed in 2025.** TikTok says follower count and past hits are not direct ranking factors and that finishing a longer video is a strong signal (TikTok, June 2020, still its fullest public account). But across 2M videos from 214,507 profiles, average views per post fell 59% for 1K to 5K accounts from 2024 to 2025 and rose 39% for 100K to 1M accounts, while brands posted far more often (Socialinsider, 2026). A new account can still break out (in our sample a 6,131-follower account had a 7.1M-view video), but plan on the account's own median, not on virality ([how it ranks](references/how-it-ranks.md)).
2. **Posting more buys lottery tickets, not a better typical post.** Across 11.4M posts from 150,000 accounts, median views stayed near 500 at every frequency, while the top tenth of posts rose from 3,722 views at one post a week to 14,401 at eleven or more (Buffer, October 2025). The biggest step is from one post a week to two to five (+17% per post). Set cadence from what the team can hold ([content](references/content.md#posting-frequency)).
3. **Long videos win views, not attention.** Brand videos of 2 to 3 minutes had about 11 times the median views of 15 to 30 second videos, which had the highest engagement rate (Socialinsider, 6M brand videos, H1 2026). But on videos of 1 to 10 minutes the median watch time was 11.3 seconds (Buffer, 1.1M videos, March 2025). Length lets a video that holds people earn more; it does not make people stay. TikTok's Creator Rewards pays only on videos over a minute (TikTok, March 2024), which skews what creators post ([content](references/content.md#length-and-watch-time)).
4. **Engagement benchmarks disagree because the denominators do.** By views, rates sit in a narrow 3.75% to 4.4% band at every account size (Socialinsider, 2025 data). By followers, they fall from 7.84% for 1K to 10K accounts to 1.84% above 500K (SociaVault, vendor, 150,000 accounts, April 2026). Never compare a number from one method with a benchmark from the other ([reading the numbers](references/reading-the-numbers.md#engagement-rate)).
5. **The US joint venture changed owners, not (yet, visibly) the feed.** TikTok USDS Joint Venture closed on 22 January 2026 (Oracle, Silver Lake and MGX 15% each, ByteDance 19.9%). The algorithm is licensed from ByteDance and is to be retrained on US data inside Oracle's cloud; ByteDance still runs US ads and TikTok Shop. As of October 2026 no public statement says the retraining is done, and the reach drops creators report rest on anecdotes and coincided with outages ([how it ranks](references/how-it-ranks.md#the-us-joint-venture)).
6. **TikTok search is used for how-to and reviews, not instead of Google.** 49% of US consumers had used TikTok as a search engine (41% in 2024), but only 4% of Gen Z would rely on it over Google, down from 8% (Adobe, 807 consumers, January 2026). Tutorials (61%) and product reviews (45%) are what people look for there ([how it ranks](references/how-it-ranks.md#tiktok-search)).
7. **Brands cannot ride trending songs.** Business accounts get the Commercial Music Library, licensed for TikTok only. Labels keep suing brands over songs in organic posts: Warner against Crumbl (2025, settled in principle May 2026) and PacSun (December 2025), Universal and Concord against Quince (April 2026, up to $150,000 a work). Paying a creator to post does not move the risk ([content](references/content.md#music-and-sounds)).
8. **Paid posts swamp niche samples.** In our "budget app" search the 8 of the top 30 videos marked as ads or paid partnerships held 87% of the views, some at a like rate of 0.13%, a sign the views were bought. Strip `is_ad` rows before any median ([field notes](references/field-notes.md)).
9. **Audiences notice AI.** Since November 2025 users can turn down AI-generated content in For You, and TikTok labels it through C2PA credentials and invisible watermarks. In our sample, the top comments under an AI-made product video with 7.1M views called it out as AI. Make product claims on real footage.

**Does not hold up:** "70% completion is the threshold" (no TikTok source; the figure circulates without an origin); "followers decide reach"; "post three to five times a day" (the median post does not improve); "longer is always better" (median watch time on long videos is seconds); "TikTok replaced Google for Gen Z"; "the joint venture cut reach by 20% to 40%" (no primary data); "a song in TikTok's library is cleared for a brand".

**Unknown:** whether the retrained US algorithm is live and what it ranks differently; whether a TikTok Shop product link raises or lowers organic reach (no independent study either way); how much TikTok search weights speech and on-screen text against the caption (TikTok documents captions, sounds and hashtags only).

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| How does For You rank? What changed with the US owners? How does TikTok search work? Does TikTok Shop help reach? | [How it ranks](references/how-it-ranks.md) |
| How long should videos be? How often to post? Carousels or video? Which sounds can a brand use? | [Content](references/content.md) |
| Is this engagement rate good? Is this video an outlier? Which benchmark applies? What do the fields mean? | [Reading the numbers](references/reading-the-numbers.md) |
| Audit our account or a competitor's; benchmark us against rivals | [Account audit](references/audit.md) |
| How do the best TikToks in our niche open? What is trending this week? | [Hooks and trends](references/hooks-and-trends.md) |
| Why did this one TikTok go viral? Can we repeat it? | [Viral video](references/viral-video.md) |
| What do people ask and object to in TikTok comments? | [Comments](references/comments.md) |
| We want a TikTok strategy or a 90-day plan | [Strategy](references/strategy.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **A niche:** `tiktok_search_videos` with a `query`, `since` and `sort`, 30 rows a page, 1 credit. It is TikTok's ranked search, never complete, with no country filter (read `language`). `sort: "popular"` ranks by likes, so it favours older videos; rank by views per day yourself. `since: "year"` reaches back about six months.
- **An account:** `tiktok_get_profile` (1 credit) for followers, `posts_count` and the bio link in `website`; `tiktok_get_videos` (1 credit a page of 10) with `sort: "latest"` for the recent run, pinned videos first whatever their age, so order by `created_at`. Its `sort: "popular"` ranks by views and reaches back years.
- **What was said:** `tiktok_get_transcript`, 1 credit, cached 30 days. The `transcript` field on search and listing rows came back empty in our runs: call the tool. `NoData` (still 1 credit) means no speech TikTok kept; `ai_fallback: true` adds 10 credits, videos under 2 minutes only.
- **Comments:** `tiktok_get_comments`, about 20 top-level comments a page, 1 credit; replies themselves do not come back.
- **Where the audience is:** `tiktok_get_audience`, 26 credits, from a sample of a few hundred followers: read `share`, never `count`. Use it on the user's own account when they sell in a few countries, on finalists only.
- **Skip `tiktok_get_video`** (up to 10 credits) when a search or listing row already has the video. Pass `max_credits` on every call when the user names a budget; `dry_run: true` prices any call for free. Cached results are free: 6 hours for searches, listings and comments, 24 for profiles.
- **Not in the tools:** watch time, completion, traffic sources, profile visits and link clicks live in the account's own TikTok Studio, as do LIVE and TikTok Shop sales. Ask the user to read them; never estimate them.
- **Never post, comment, like, follow, message or schedule.** Deliver tables with a link to every video and account cited. Write hooks or scripts only when asked, from a pattern the evidence shows, never passing off a creator's words as the user's.

## Related skills

- Creators to partner with, their real audience and fees: [creators](../creators/SKILL.md). TikTok ads, Spark Ads and a rival's ad library: [paid-ads](../paid-ads/SKILL.md).
- The same short video on other platforms: [instagram](../instagram/SKILL.md) for Reels, [youtube](../youtube/SKILL.md) for Shorts and long videos. Compare views only within one platform.
- What buyers say across Reddit, reviews and every platform's comments: [voice-of-customer](../voice-of-customer/SKILL.md). A rival beyond TikTok: [competitor-intel](../competitor-intel/SKILL.md). Whether TikTok is the right channel at all: [growth-strategy](../growth-strategy/SKILL.md). A weekly watch of mentions: [brand-monitoring](../brand-monitoring/SKILL.md).
