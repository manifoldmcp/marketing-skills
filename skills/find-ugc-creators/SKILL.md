---
name: find-ugc-creators
description: When the user wants UGC creators to make videos for their ads or product pages. Finds smaller creators on TikTok and Instagram, and YouTube reviewers when asked, who already film reviews, unboxings, demos and made-me-buy-it videos in or near the category, adds the makers behind talking-head ads in the TikTok and Meta ad libraries, judges each one's craft from a transcript, and ends in a table with an example of their work. Also use when the user mentions UGC creators, user generated content for ads, people who film unboxings or product reviews, UGC for Meta or TikTok ads, or a UGC portfolio. Creators to sponsor for reach go to find-creators, fans already posting about the brand to find-brand-fans, the ad brief to write-ad-brief, and usage rights and the brief to write-creator-brief.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find UGC creators

UGC creators make videos for the brand to run as ads or put on its product pages: reviews, unboxings, demos, "TikTok made me buy it" stories. The user pays per video and posts it on their own accounts, so the creator's audience matters little and the craft matters most. This skill finds smaller creators already making product-style videos in or near the category and ends in a table with an example of each one's work.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos`, `instagram_search_posts` and `ads_search_ads` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If one platform's tools are missing (all `tiktok_*`, `instagram_*` or `youtube_*`), that group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and run on the platforms that are on.
- The ad library step needs the `ads_*` tools, and contacts at a creator's own website need the `leads_*` tools. If one of those groups is missing, it is switched off: skip the ad library step, or deliver the table with the bio contact only, and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and category, the ICP, the countries that matter, the competitors) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Product and category**: what the videos will show, and two or three competitor or adjacent products whose reviews count as proof of fit.
- **Styles**: which kinds of video the user wants: review, unboxing, demo, problem and solution, before and after, "TikTok made me buy it", get ready with me. Default: review and unboxing.
- **Platforms**: default TikTok and Instagram, where the style lives. Add YouTube when the category has long-form reviewers; its search returns regular videos, not Shorts.
- **Size**: default 1K to 50K followers. The user pays per video, and small creators charge less, answer faster and look like customers, which is the point of UGC.
- **On camera**: a face, a voice-over over hands, or the product alone. Ask before cutting the list.
- **Count**: default 20 creators in the final table.
- **Budget**: a default run costs about 10 + 6 + 2 + 30 + 20 + 10 = 78 credits: five TikTok queries at two pages, three Instagram hashtags at two pages, two ad library searches, 30 profiles, 20 listings and 10 transcripts. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search for product-style videos.** `tiktok_search_videos` with queries that pair a style with the category: "<category> review", "<product type> unboxing", "<competitor> honest review", "tiktok made me buy it <category>", "<category> haul"; two pages each (1 credit a page), `since: "year"` in a small category. `instagram_search_posts` with hashtags such as ugccreator, ugc plus the category, and the category plus review (1 credit a page). On YouTube, `youtube_search_videos` with "<category> review" and `since: "year"` when the user added it (1 credit a page).
2. **Read the ad libraries.** `ads_search_ads` with the category as `query` on `platform: "tiktok"` and `platform: "facebook"` (1 credit a page). An ad that is a person talking to camera is UGC a brand bought; when it runs from a creator's handle or names one, add that creator as a proven maker.
3. **Keep the makers.** Group the rows by `author`. Keep authors with two or more product-style videos in the results, or one strong one (a clear demo with a verdict). Drop brand accounts, resellers and aggregator pages that repost other people's videos.
4. **Profiles.** `tiktok_get_profile` or `instagram_get_profile` on up to 30 authors (1 credit each), `youtube_get_channel` for YouTube reviewers. Read `followers` against the size band, and the `bio` and `website` for the signs of a working UGC creator: "UGC creator", "content for brands", a rate card, a portfolio link, a business email.
5. **Their recent work.** `tiktok_get_videos` or `instagram_get_reels` on the top 20 (1 credit a page). Count the product-style videos, the `is_ad: true` posts (paid brand work before), and read `duration_s`: 15 to 60 seconds is ad length. Note whether they show their face, talk over hands or only film the product. Keep creators who posted in the last 30 days.
6. **Hear them.** `tiktok_get_transcript` or `instagram_get_transcript` on one product video each for the top 10 (1 credit each). A creator who opens with a hook, names the problem, shows the product working and ends with a verdict can make an ad. One who rambles for twenty seconds before the product appears cannot.
7. **Add contacts.** From the bio, then `website`. For a creator's own domain, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) with `department: ["executive", "marketing"]` and `limit: 10`. Never guess an address.
8. **Deliver** a table: creator, platform and link, followers, styles they make, best example (URL and views), its hook from the transcript, on camera (face, voice-over, hands only), brand work signs (`is_ad` count, brands named), portfolio or contact, and a one-line note on fit. Sort by craft, not by followers.

## Judgment

- For UGC, views on the creator's own posts are a weak signal. A 3K-follower creator with tight 30-second reviews beats a 200K creator who rambles; judge the transcript and the structure.
- A creator whose own "made me buy it" video drew comments asking where to buy has already shown they can sell. Mark those rows.
- Paid brand work in the feed is a plus here, not a warning: it shows the creator delivers to brands. `is_ad` is null where the platform does not say, so also read captions for "ad", a partner tag or a brand code.
- UGC is bought with usage rights: where the video may run (organic, paid ads, product page), for how long, and whether it runs from the creator's handle (whitelisting, Spark Ads). The tools cannot see rights; [write-creator-brief](../write-creator-brief/SKILL.md) states them.
- As a rule of thumb for the US in 2026, a UGC video without posting costs about $150 to $500; paid usage beyond 30 days, raw footage and extra hooks cost more. Agree them in [write-creator-brief](../write-creator-brief/SKILL.md).
- Search is ranked, leans recent and is never complete. Widen `since` and add competitor names when the category is small. Instagram search is by hashtag only, so makers who do not tag are under-found.
- Faceless formats (hands, voice-over) suit products that do the talking. A face on camera suits trust-heavy categories (skincare, supplements, finance).
- Credits: profiles, listings, searches, ad library pages and transcripts cost 1 credit a call or a page; `tiktok_get_video` and `instagram_get_post` cost up to 10 when the vendor fetches media, so read numbers from the listing rows, and `tiktok_get_transcript` with `ai_fallback: true` costs 11. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Never message, email, contract or pay a creator, and never ship product. The deliverable is a table.

## Related skills

- The ad itself (angles, hooks, what the category's ads already say), which the UGC creator works from: [write-ad-brief](../write-ad-brief/SKILL.md). Usage rights, deliverables and the brief: [write-creator-brief](../write-creator-brief/SKILL.md).
- Creators to sponsor for reach: [find-creators](../find-creators/SKILL.md) across platforms, or [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-instagram-creators](../find-instagram-creators/SKILL.md) and [find-youtube-creators](../find-youtube-creators/SKILL.md) on one.
- Fans already posting about the brand, whose posts can run as ads: [find-brand-fans](../find-brand-fans/SKILL.md).
- The category's ads on Meta and TikTok in depth: [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md).
