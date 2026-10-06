---
name: find-tiktok-creators
description: When the user wants to find TikTok creators or influencers to work with in a niche. Finds TikTokers who already make videos on the niche from the authors of TikTok search results, sizes them with their profile, keeps only those who post often and whose audience responds, and ends in a ranked table with median views, engagement, sponsored posts and a contact for each. Also use when the user mentions TikTok influencers, TikTokers to sponsor, TikTok creators in a niche, micro influencers on TikTok, or a TikTok creator list. Creators across several platforms go to find-creators, Instagram creators to find-instagram-creators, YouTubers to find-youtube-creators, UGC makers for ads to find-ugc-creators, and vetting one creator and their audience country to vet-creator.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find TikTok creators

TikTok creators who already make videos in the user's niche, found through the authors of the niche's search results, sized with their profile, and kept only when they post often and their audience responds. The skill ranks them by niche fit and median views, not followers. It ends in a ranked table of about 20 creators with the evidence and a contact for each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos`, `tiktok_get_profile` and `tiktok_get_videos` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `tiktok_*` tools are not, the TikTok tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- Contacts at a creator's own website need the `leads_*` tools. If that group is missing, it is switched off: deliver the table with the bio contact only, and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and category, the niche, the ICP, the countries that matter, the competitors) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Niche**: three to five search terms: the category, the problem the product solves, a use case, and a competitor's name (creators who reviewed a rival). Default: the category and two problems in the user's own words.
- **Size**: a follower band. Default: 10,000 to 250,000, where views per dollar are usually best; say so if the user wants bigger names. Nano is under 10K followers, micro 10K to 100K, mid 100K to 500K, macro 500K to 1M, mega above.
- **Language**: search has no country filter, so keep creators whose captions and speech are in the user's language. The TikTok audience check in [vet-creator](../vet-creator/SKILL.md) confirms the country for the finalists.
- **Count**: 20 creators by default.
- **Budget**: a default run costs about 5 x 3 + 40 + 25 = 80 credits for five terms. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the niche.** `tiktok_search_videos` for each term with `since: "month"` and the default relevance sort, three pages each (1 credit a page). Niche fit matters more than one viral video here. Collect the authors, with how many matching videos each has and their best views. Drop brands: an author that is a company, or whose every row has `is_ad: true`.
2. **Size them.** `tiktok_get_profile` on the top 40 authors by matching videos, then views (1 credit each). Keep the ones inside the size band. Note the `bio` (the topic, often a business email), `website` and `verified`.
3. **Check activity and engagement.** `tiktok_get_videos` with `sort: "latest"`, one page each for the best 25 (1 credit each): the last post date, videos a week, median views, median views over followers, median engagement rate, how many recent videos are on the niche, and any paid posts (`is_ad: true`). Apply the [floors](../create-tiktok-plan/references/platforms/tiktok.md#floors): active in the last 30 days, engagement and views over followers above the floor, at least half of recent videos on the niche, and no more than about a third sponsored.
4. **Rank.** Niche fit first (the share of recent videos on the topic), then median views, then engagement rate. Cut to the count.
5. **Find a contact.** Take a business email from the bio where there is one. Where a creator lists a website that is their own domain, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on it with `department: ["executive", "marketing"]` and `limit: 10`, since the creator owns the site. Otherwise the contact is a TikTok message, which the user sends.
6. **Deliver** a table: creator (handle and link), followers, median views, views over followers, median engagement rate, videos a week, last post, niche fit (the share of recent videos, and one example link), paid posts seen, contact (bio email, website address and its verification status, or TikTok message), and one line on why they fit.

## Judgment

- Read every number by the [TikTok notes](../create-tiktok-plan/references/platforms/tiktok.md): medians over recent videos, never the mean; engagement is likes plus comments over views; paid posts stay out of the organic median; a null is left out, not read as zero. A view rate (median views over followers) under about 5% means most of the following no longer watches.
- A creator's median views, not their followers, predicts what a sponsored video will get. Rank and price on it.
- Paid partnership posts in a creator's feed show they already take deals. Compare those videos' views with the creator's median: near the median means the audience trusts their recommendations; under half of it means the audience tunes out ads. Count captions with "ad", "sponsored", a partner tag, a discount code or an affiliate link too, because undisclosed deals look organic.
- Search is ranked and never complete, so a fourth page of one term is mostly weaker matches. Add a term rather than a page. A creator the user expected and did not find is a reason for a second pass, not proof they are absent; searches are cached for 6 hours, so repeats are cheap.
- A list is not a vetting. Run the TikTok audience check in [vet-creator](../vet-creator/SKILL.md) on the finalists before any money moves (about 32 credits each; `tiktok_get_audience` alone is 26, cached 7 days): it shows whether the audience is real and in the user's market.
- Credits: profiles, listings, searches, comments and transcripts cost 1 credit a call or a page; `tiktok_get_video` costs up to 10 when the vendor fetches media, so read numbers from the listing rows, which carry them, and `tiktok_get_transcript` with `ai_fallback: true` costs 11. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Profiles are cached 24 hours, listings and searches 6 hours.
- Never follow, message, comment on, email, contract or pay a creator, and never ship product. Never guess an address. Outreach copy and the brief belong to [write-creator-brief](../write-creator-brief/SKILL.md), and only when the user asks.

## Related skills

- Creators across TikTok, Instagram and YouTube in one table: [find-creators](../find-creators/SKILL.md). The same search on one other platform: [find-instagram-creators](../find-instagram-creators/SKILL.md), [find-youtube-creators](../find-youtube-creators/SKILL.md).
- TikTok creators who film reviews and unboxings for ads: [find-ugc-creators](../find-ugc-creators/SKILL.md). Fans already posting about the brand: [find-brand-fans](../find-brand-fans/SKILL.md).
- One creator checked before a deal, and the TikTok audience by country: [vet-creator](../vet-creator/SKILL.md). The brief: [write-creator-brief](../write-creator-brief/SKILL.md). A creator program plan: [create-influencer-plan](../create-influencer-plan/SKILL.md).
- A plan for the user's own TikTok: [create-tiktok-plan](../create-tiktok-plan/SKILL.md).
