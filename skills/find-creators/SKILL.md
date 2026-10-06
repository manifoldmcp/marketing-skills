---
name: find-creators
description: When the user wants to find influencers or creators to work with across TikTok, Instagram and YouTube. Runs each platform's creator search for a niche, merges the results into one table deduped by person, and puts engagement on one scale so creators on different platforms can be compared, with the evidence and a contact for each. Also use when the user mentions influencers, creators, KOLs, micro or nano influencers, brand ambassadors, a creator list, or who should we sponsor, with several platforms or none named. Creators on one platform go to find-tiktok-creators, find-instagram-creators or find-youtube-creators, UGC makers for ads to find-ugc-creators, fans already posting about the brand to find-brand-fans, vetting one creator to vet-creator, and LinkedIn voices to find-linkedin-topic-leaders.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find creators

Most creators worth sponsoring post on more than one platform, and a list built per platform counts them two or three times with numbers that cannot be compared. This skill runs each platform's own creator search, merges the results into one row per person, and puts their engagement on one scale. It ends in a table of creators with the evidence and a contact for each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos`, `instagram_search_posts` and `youtube_search_videos` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If one platform's tools are missing (all `tiktok_*`, `instagram_*` or `youtube_*`), that group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and run on the platforms that are on.
- Contacts at a creator's own website need the `leads_*` tools. If that group is missing, it is switched off: deliver the table with the bio contact only, and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and category, the niche, the ICP and the platforms it uses, the countries that matter, the competitors) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Scope**: across platforms is the default for "find influencers". If the user names one platform, wants UGC makers for ads, or wants fans already posting about the brand, send the job to the skill in [Related skills](#related-skills) that owns it.
- **Niche**: three to five keywords and hashtags the niche uses ("home espresso", "latte art", "espressotips"). Ask for two.
- **Platforms**: default TikTok, Instagram and YouTube. Drop a platform the user's buyers do not use.
- **Size**: default 10K to 250K followers, per the [floors](#engagement-and-floors). Pass the same band to each platform search; YouTube's is set in median views.
- **Market**: the country and language the user sells in.
- **Count**: default 20 people in the final table.
- **Budget**: the three platform searches cost about 80 + 75 + 102 = 257 credits at their defaults (take the current figures from each platform skill), plus 20 x 2 = 40 for the cross-platform lookups in step 2: about 300 credits. Dropping a platform saves its share. Say the estimate before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Run each platform's search.** Follow [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-instagram-creators](../find-instagram-creators/SKILL.md) and [find-youtube-creators](../find-youtube-creators/SKILL.md) with the same niche, size and market, up to their shortlist. Take from each: handle, profile URL, followers, median views, engagement, bio, `website`, last post date and sponsored posts. Do not repeat their steps here.
2. **Match people across platforms.** For the top 20 by engagement, look for the same person on the other platforms: `tiktok_get_profile`, `instagram_get_profile` or `youtube_get_channel` with the same handle (1 credit each, about 40 in all). Count it a match only with a second signal: the same `website` (on YouTube a site shows in the `bio`, since `website` is null there), a bio that names the other handle, or the same name with the same niche. A shared handle alone is not proof; handles are claimed by strangers. When a match turns up a platform the creator is active on, pull its listing (`tiktok_get_videos`, `instagram_get_reels` or `youtube_get_videos`, 1 credit a page) and work out median views there too.
3. **Put engagement on one scale.** For each person and platform, take the view rate and engagement rate as the [engagement rules](#engagement-and-floors) set out, then express each as a multiple of the median of the candidates on that platform. A creator at 2.0 on TikTok and 0.6 on YouTube is strong on TikTok and weak on YouTube, whatever the raw numbers say.
4. **Cut to the list.** Apply the [floors](#engagement-and-floors): active, niche fit, sponsored share. Rank by the best platform's engagement multiple first and the combined followers second, and keep the count the user asked for.
5. **Merge contacts.** Keep one contact per person: the platform searches already took a business email from the bio or ran the contact steps on the creator's site. Prefer a management or business address over a personal one. Only when step 2 turned up a site none of them checked, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on it with `limit: 10`.
6. **Deliver** one table, one row per person: name, platforms with handle and link, followers per platform, combined followers, best platform, median views there, view rate and engagement multiple per platform, sponsored share, last post, one post URL that shows the niche fit, country signal, contact and where it came from. Mark the platform-only rows (no match found) so the user knows they were checked.

## Judgment

- Pay for the platform where a creator's median views are, not where their follower count is. Most creators are strong on one platform and carry an old following on the others.
- The engagement multiple is relative to this candidate set. In a narrow niche with fewer than ten candidates on a platform, the median is noisy; say so and show the raw rates.
- Search on every platform is ranked and never complete. A creator the user expected and did not find is a reason for a second pass with more keywords, not proof they are absent; searches are cached for 6 hours, so repeats are cheap.
- Instagram search is by hashtag only, so Instagram creators who do not tag their posts are under-found. Say so when Instagram is the user's main platform.
- A list is not a vetting. Before any fee, run [vet-creator](../vet-creator/SKILL.md) on the shortlist.
- Credits: search wide and cheap, and spend on profiles and listings only for authors worth it. Profiles, listings, searches, comments and transcripts cost 1 credit a call or a page; `tiktok_get_video` and `instagram_get_post` cost up to 10 when the vendor fetches media, so read numbers from the listing rows, which carry them. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Profiles are cached 24 hours, listings and searches 6 hours.
- Never follow, message, comment on, email, contract or pay a creator, and never ship product. The deliverable is a table. If the host has an email tool, a CRM or a sequencer, offer to pass the table to it; do not send.
- Contacts: the bio first (creators often list a business email or a management agency there), then the profile's `website`. When the website is the creator's own domain, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on it with `department: ["executive", "marketing"]`, since the creator owns the site. Never guess an address.

## Engagement and floors

- Read each platform's numbers by its own rules and floors, in the platform notes: [TikTok](../create-tiktok-plan/references/platforms/tiktok.md), [Instagram](../create-instagram-plan/references/platforms/instagram.md) and [YouTube](../create-youtube-plan/references/platforms/youtube.md). They agree on the basics: medians over a creator's recent posts, never the mean; engagement is likes plus comments over views (over followers for Instagram image posts); paid posts stay out of the organic median; a null is left out, not read as zero; a view rate (median views over followers) under about 5% means most of the following no longer watches.
- YouTube listings carry views but not likes or comments: `youtube_get_video` on three to five recent videos (1 credit each) fills them in.
- Never compare raw rates across platforms: each platform has its own engagement floor. In a table across platforms, give each creator's view rate and engagement rate as a multiple of the median of the candidates on that platform (1.0 is typical for this niche there).
- Judge a creator's sponsored posts against their own median. A sponsored post near the median means the audience trusts the creator's recommendations; one under half of it means the audience skips their ads.
- **Size.** Nano under 10K followers, micro 10K to 100K, mid 100K to 500K, macro 500K to 1M, mega above. Default: 10K to 250K (on YouTube, a median of 2,000 to 100,000 views a video). Engagement per follower falls as accounts grow while fees rise with followers, so this band usually buys the most engaged views per dollar.
- **Active.** Posted in the last 30 days. A dormant creator does not deliver on time.
- **Niche fit.** At least half of the recent posts are on the niche. One post about the topic is not a niche.
- **Sponsored share.** `is_ad: true` marks paid posts and marked partnerships where the platform says; it is null where it does not. Also count captions with "ad", "sponsored", a partner tag, a discount code or an affiliate link, because undisclosed deals look organic. Over about a third of recent posts sponsored, the feed is an ad feed and its audience skips ads.
- **Audience country.** Only TikTok publishes a split: `tiktok_get_audience` (26 credits, shortlist only, cached 7 days), run through [vet-creator](../vet-creator/SKILL.md). On Instagram and YouTube the tools cannot prove where an audience is; report the signals instead (caption and comment language, the profile's `location`, places named in comments) and mark them unproven.

## Related skills

- Creators on one platform: [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-instagram-creators](../find-instagram-creators/SKILL.md) or [find-youtube-creators](../find-youtube-creators/SKILL.md).
- Creators who film reviews and unboxings for the user's ads: [find-ugc-creators](../find-ugc-creators/SKILL.md). Creators already posting about the brand unpaid: [find-brand-fans](../find-brand-fans/SKILL.md).
- One creator checked before a deal, and the TikTok audience by country: [vet-creator](../vet-creator/SKILL.md).
- The brief the creators get: [write-creator-brief](../write-creator-brief/SKILL.md). Which creators, platforms and deal types, as a plan: [create-influencer-plan](../create-influencer-plan/SKILL.md).
- Creators for a launch day, timed with the rest of the launch: [create-launch-plan](../create-launch-plan/SKILL.md).
- Podcasts to appear on: [find-podcasts](../find-podcasts/SKILL.md). Review sites, blogs and newsletters as affiliates: [find-affiliate-partners](../find-affiliate-partners/SKILL.md).
- Leaders of a topic on LinkedIn: [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md).
