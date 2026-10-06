---
name: find-instagram-creators
description: When the user wants to find Instagram creators or influencers to work with in a niche. Finds Instagram creators who already post on the niche from the authors of its hashtag results, sizes them with their profile, keeps only those who post often and whose following responds, and ends in a ranked table with median reel views, engagement, paid partnerships and a contact for each. Also use when the user mentions Instagram influencers, IG creators, Reels creators to sponsor, micro influencers on Instagram, or an Instagram creator list. Creators across several platforms go to find-creators, TikTokers to find-tiktok-creators, YouTubers to find-youtube-creators, UGC makers for ads to find-ugc-creators, and vetting one creator to vet-creator.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find Instagram creators

Instagram creators who already post in the user's niche, found through the authors of the niche's hashtag results, sized with their profile, and kept only when they post often and their following responds. The skill ranks them by niche fit and median reel views, not followers. It ends in a ranked table of about 20 creators with the evidence and a contact for each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `instagram_search_posts`, `instagram_get_profile` and `instagram_get_reels` (hosts often add a prefix, for example `mcp__manifold__instagram_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `instagram_*` tools are not, the Instagram tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- Contacts at a creator's own website need the `leads_*` tools. If that group is missing, it is switched off: deliver the table with the bio contact only, and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and category, the niche, the ICP, the countries that matter, the competitors and their brand hashtags) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Hashtags**: four or five tags the niche uses: the category, the problem the product solves, a use case, and a competitor's brand tag (creators who posted about a rival). Default: the category tag, then the tags that recur in its captions.
- **Size**: a follower band. Default: 10,000 to 250,000, where cost per view is usually best; say so if the user wants bigger names. Nano is under 10K followers, micro 10K to 100K, mid 100K to 500K, macro 500K to 1M, mega above.
- **Language**: hashtag search has no country filter, so keep creators whose captions and speech are in the user's language.
- **Count**: 20 creators by default.
- **Budget**: a default run costs about 5 x 2 + 40 + 25 = 75 credits for five hashtags. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the hashtags.** `instagram_search_posts` for each tag with `since: "month"`, two pages each (1 credit a page). Collect the authors, with how many matching posts each has and their best numbers. Drop brands: an author that is a company, or whose every row has `is_ad: true`. The profile's `kind` does not settle it, since many creators run business accounts too.
2. **Size them.** `instagram_get_profile` on the top 40 authors by matching posts, then engagement (1 credit each). Keep the ones inside the size band. Note the `bio` (the topic, often a business email) and `website`.
3. **Check activity and engagement.** For the best 25, `instagram_get_reels`, one page each (1 credit): the last post date, reels a week, median reel views, median views over followers, median reel engagement rate, how many recent reels are on the niche, and any paid posts (`is_ad: true`). For a creator who posts mostly images, use `instagram_get_posts` instead (1 credit) and the engagement rate over followers. Apply the [floors](../create-instagram-plan/references/platforms/instagram.md#floors): active in the last 30 days, at least half of recent posts on the niche, and no more than about a third sponsored.
4. **Rank.** Niche fit first (the share of recent posts on the topic), then median reel views, then engagement rate. Cut to the count.
5. **Find a contact.** Take a business email from the bio where there is one. Where a creator lists a website that is their own domain, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on it with `department: ["executive", "marketing"]` and `limit: 10`, since the creator owns the site. Otherwise the contact is an Instagram DM, which the user sends.
6. **Deliver** a table: creator (handle and link), followers, median reel views, views over followers, median engagement rate (and its base: views or followers), posts a week, last post, niche fit (the share of recent posts, and one example link), paid posts seen, contact (bio email, website address and its verification status, or DM), and one line on why they fit.

## Judgment

- Read every number by the [Instagram notes](../create-instagram-plan/references/platforms/instagram.md): medians over recent posts, never the mean; engagement is likes plus comments over views for reels and over followers for image posts; paid posts stay out of the organic median; a null is left out, not read as zero. A view rate under about 5% means most of the following no longer watches.
- A creator's median reel views, not their followers, predicts what a sponsored reel will get. For a creator who posts mostly images, the engagement rate over followers is the measure.
- Paid partnership posts in a creator's feed show they already take deals. Compare those posts' numbers with the creator's median: a sponsored post far below it means their following tunes out ads. `is_ad` is null where Instagram does not say, so also count captions with "ad", "sponsored", a partner tag, a discount code or an affiliate link.
- Hashtag search finds only creators who tag their posts, and many good ones do not; it is also ranked and never complete. When the list is thin, add tags, or start from the creators in competitors' paid partnerships ([audit-instagram-account](../audit-instagram-account/SKILL.md) on the competitor's account). Searches are cached for 6 hours, so repeats are cheap.
- Instagram publishes no audience split, so the tools cannot prove where an audience is. Report the signals (caption and comment language, the profile's `location`, places named in comments) and mark them unproven. For a creator also on TikTok, run the TikTok audience check in [vet-creator](../vet-creator/SKILL.md) on that handle as a proxy.
- A list is not a vetting. Before any fee, run [vet-creator](../vet-creator/SKILL.md) on the shortlist.
- Credits: profiles, listings, searches, comments and transcripts cost 1 credit a call or a page; `instagram_get_post` costs up to 10 when the vendor fetches media, so read numbers from the listing rows, which carry them, and `instagram_get_comments` with `include_replies: true` costs 15. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Profiles are cached 24 hours, listings and searches 6 hours.
- Never follow, send a DM, comment on, email, contract or pay a creator, and never ship product. Never guess an address. Outreach copy and the brief belong to [write-creator-brief](../write-creator-brief/SKILL.md), and only when the user asks.

## Related skills

- Creators across Instagram, TikTok and YouTube in one table: [find-creators](../find-creators/SKILL.md). The same search on one other platform: [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-youtube-creators](../find-youtube-creators/SKILL.md).
- Creators who film reviews and unboxings for ads: [find-ugc-creators](../find-ugc-creators/SKILL.md). Fans already posting about the brand: [find-brand-fans](../find-brand-fans/SKILL.md).
- One creator checked before a deal: [vet-creator](../vet-creator/SKILL.md). The brief: [write-creator-brief](../write-creator-brief/SKILL.md). A creator program plan: [create-influencer-plan](../create-influencer-plan/SKILL.md).
- A plan for the user's own Instagram: [create-instagram-plan](../create-instagram-plan/SKILL.md).
