---
name: instagram
description: When the user wants to grow on Instagram, audit an Instagram account, or know what Instagram rewards now. Holds dated, sourced findings a model would not know from training (what Mosseri says Instagram ranks on, the April 2026 rule that drops reposting accounts from recommendations, Trial Reels, the 3-minute limit for recommended Reels, the five-hashtag cap, Your Algorithm, teen account limits, 2025-2026 engagement benchmarks by format) and how to read Manifold's Instagram data without its traps. Also use when the user mentions an Instagram or IG strategy, content plan or 90-day plan, how to grow on Instagram, reels or carousels, how often to post reels, an Instagram audit, a competitor's Instagram, Instagram engagement rate, best performing posts, Reels hooks or caption hooks, reels skipped early, Reels trends or what to film next, mining Instagram comments, objections under reels, or why a reel went viral. Creators and influencers go to creators, Meta ads to paid-ads, TikTok to tiktok, Facebook to facebook.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Instagram

What gets organic reach on Instagram as the evidence stood in October 2026, and how to read Manifold's Instagram data without fooling yourself. Much of what a model believes about Instagram dates from before 2025 and is now wrong: hashtags do not add reach and are capped at five, an account that mostly reposts others' work is cut out of recommendations, sends to non-followers matter more than likes, and follower count says little about how many people a reel reaches. Prefer what is written here to what you remember, and re-check anything older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the brand, the Instagram handle, the ICP, the competitors and their accounts, the brand voice and the goal from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Measuring accounts, reels, hashtags and comments needs the Manifold connector: look for `instagram_get_profile`, `instagram_get_reels` and `instagram_search_posts` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand method in [the data notes](references/data.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. If other Manifold tools are there but not the `instagram_*` ones, the Instagram tool group is switched off on the app's Tools page. Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Watch time, likes and sends are the signals Instagram names**, measured per reach. Likes weigh more with followers, sends (shares by DM) more with non-followers (Mosseri, January 2025 and May 2026). Reaching new people means making something people send ([ranking](references/ranking.md)).
2. **Mostly reposting gets an account cut from recommendations.** Since 30 April 2026, an account whose posts over a rolling 30 days are mostly unoriginal, photos and carousels included, is not shown to non-followers; a credit, a crop or a watermark does not make a repost original. Collabs, Remix and Reposts are the allowed routes ([ranking](references/ranking.md#originality)).
3. **Reels are recommended up to 3 minutes**, though the camera records 20. Trial Reels test a reel on non-followers before followers see it, and can be scheduled since April 2026.
4. **Hashtags do not add reach** (Mosseri, February 2025) and are capped at five per post since December 2025. Captions now matter for search instead: Instagram reads them, Google indexes public posts, and the first line is the caption hook.
5. **The base decides which format wins.** Over followers, carousels (0.55%) and reels (0.52%) tie and single images trail (0.37%; Socialinsider, 35M posts, 2025). Over reach, carousels earn about twice the engagement of reels (6.9% against 3.3%; Buffer, 2025), while reels reach about 1.4 times as many people. Reels for reach, carousels for saves and depth ([benchmarks](references/benchmarks.md)).
6. **Engagement fell 16% to 26% in 2025 on every large dataset**, and follower growth roughly halved in every size tier. A falling rate is partly the platform.
7. **Followers predict reach poorly.** In Manifold's pull of 15 accounts, brands' median reel reached about 6% of their followers and creators' about 33%, with no pattern by size. Compare on recent median views, and compare a brand only with its direct rivals ([field notes](references/field-notes.md)).
8. **Posting more does not cannibalise.** Weeks with more posts grew followers faster and reach per post did not fall, with diminishing returns (Buffer, 2.1M posts, 102K accounts, 2025). Instagram gives no number beyond "as much as you can without burning out".
9. **The opening decides recommendation.** Instagram's Insights show skip rate (gone in the first 3 seconds) since August 2025. Speech in the first three seconds lifted 10-second retention by about 25% over music alone on Facebook Reels (Emplifi, 10,110 reels, 2026).
10. **Viewers now steer their own feed.** Your Algorithm lets people edit the topics Instagram assigned them, on Reels, Explore and since June 2026 the main feed; under-18s sit in a 13+ setting worldwide that shows less on topics such as diet and weightlifting.

**Does not hold up:** "use 20 to 30 hashtags"; "hashtags grow reach"; "link in bio kills reach"; "share your post to your Story for a boost"; "engage with similar accounts before posting"; "reposting with credit is fine"; "Edits gives a permanent boost" (small and temporary, Mosseri said); "a good engagement rate for a 10K account is X%" (no tier benchmark with a method exists); "sends are worth 3 to 5 likes" (no source).

**Unknown:** the weights Instagram gives sends, likes, saves and watch time; whether a successful Trial Reel performs like a normal post once shared to followers; engagement by follower tier from any study with a published method.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| What does Instagram reward now? Originality, Trial Reels, length, hashtags, captions, Your Algorithm, teen accounts? | [Ranking](references/ranking.md) |
| Is our engagement good? Reels or carousels? How often should we post? | [Benchmarks](references/benchmarks.md) |
| What do these Instagram numbers mean? Which call answers what, and what does it cost? | [The data notes](references/data.md) |
| Audit our account or a competitor's; benchmark us against rivals; our best posts | [Audit](references/audit.md) |
| How do the top reels and captions open? Our reels get skipped | [Hooks](references/hooks.md) |
| What is trending on Reels? What should we film next? | [Trends](references/trends.md) |
| Why did this reel blow up? Can we repeat it? | [Viral reel](references/viral-reel.md) |
| What do people ask or object to in Instagram comments? | [Comments](references/comments.md) |
| We want an Instagram strategy or a 90-day plan | [Strategy](references/strategy.md) |
| What has Manifold seen in its own pulls? | [Field notes](references/field-notes.md) |

## Using Manifold

- **An account:** `instagram_get_profile` (1 credit) for size and bio, `instagram_get_reels` (1 credit a page of 12) for reel views, `instagram_get_posts` (1 a page) for images, carousels and captions. Listings run newest first with pinned posts on top and collabs by other authors mixed in; there is no popular sort.
- **A niche:** `instagram_search_posts` takes one hashtag and `since`, in Instagram's own order, and sees only tagged posts. Its rows carry `music` and `top_comments`, which listings lack.
- **Words:** `instagram_get_transcript` (1 credit, reels up to two minutes) and `instagram_get_comments` (1 credit for 15, newest first; 15 credits with replies).
- **Traps to know before any number:** `posts_count` is null and `kind` reads person for most brands; `likes` is null for hidden likes, except in hashtag search, where it reads 3; reel views exist only in the reels listing and search; `shares` and `saves` are always null; sends, reach and watch time are not in the tools at all. Read [the data notes](references/data.md) before any audit.
- **Costs:** an audit is about 10 credits an account, a plan's research about 45, a hooks or trends read about 30. Cached results are free (6 hours for listings, 24 for profiles, 30 days for transcripts); `dry_run: true` prices a call. Pass `max_credits` when the user names a budget.
- **Never post, comment, like, follow, DM or schedule.** Deliver tables with a link to every post cited; write hooks or scripts only when asked, and never pass off a creator's words as the user's.

## Related skills

- The same work on other platforms: [tiktok](../tiktok/SKILL.md), [facebook](../facebook/SKILL.md) (the same Meta Reels system and often the same buyers), [youtube](../youtube/SKILL.md).
- Finding, sizing and vetting Instagram creators and UGC partners: [creators](../creators/SKILL.md). Meta ads on Instagram and rivals' ad libraries: [paid-ads](../paid-ads/SKILL.md).
- A rival beyond Instagram: [competitor-intel](../competitor-intel/SKILL.md). Pains and objections across every source: [voice-of-customer](../voice-of-customer/SKILL.md). Which channels to be on, content calendars and repurposing: [growth-strategy](../growth-strategy/SKILL.md). Brand mentions over time: [brand-monitoring](../brand-monitoring/SKILL.md).
