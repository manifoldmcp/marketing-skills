---
name: influencers
description: Influencer and creator marketing across TikTok, Instagram and YouTube with the manifold tools. Finds creators in a niche across platforms in one table deduped by person with engagement normalized, vets one creator before a deal (engagement against their own baseline, sponsored share, comment quality, a follower sample, audience country), finds UGC creators who make product reviews and unboxings, writes the campaign brief sent to creators, and plans an influencer strategy. Use when the user asks for influencers, creators, KOLs, micro or nano influencers, brand ambassadors, creator partnerships, sponsorships, paid collabs, gifting or seeding, influencer vetting, fake followers, engagement rate, UGC creators, review or unboxing creators, a creator or influencer brief, or an influencer strategy, across platforms or naming none. Creators on one named platform belong to that platform's group. Contacting, paying and contracting creators are out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Influencers

Every job here reads creators the same way: their profile, their recent posts against their own baseline, what their audience says under them, and how much of their feed is already paid. The platform groups find creators on one platform; this group works across them and adds the judgment before money changes hands.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos`, `instagram_search_posts` and `youtube_search_videos` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If one platform's tools are missing (all `tiktok_*`, `instagram_*` or `youtube_*`), that group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and run the job on the platforms that are on.
- Contacts at a creator's own website need the `leads_*` tools. If they are missing, the leads group is switched off; deliver the table with the bio contact only.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only says "find influencers", open find creators. If it names one platform only ("TikTok creators"), hand it to that platform's group (see Other groups).

| Job | The user says | Open |
|---|---|---|
| Influencer strategy: which creators, platforms and deal types, and a 90-day plan | "influencer marketing strategy", "plan our creator program", "how should we work with influencers", "gifting or paid posts or affiliates", "influencer plan for next quarter" | [references/strategy.md](references/strategy.md) |
| Find creators: one deduped table of creators in a niche across TikTok, Instagram and YouTube | "find influencers in the home coffee niche", "micro influencers who talk about budgeting", "creator list across platforms", "who should we sponsor", "influencers for our brand" | [references/find-creators.md](references/find-creators.md) |
| Vet a creator: whether one creator's audience and engagement are real, before a deal | "vet this influencer", "are her followers fake", "is @maya worth $3,000", "check this creator before we pay", "how many sponsored posts does he do" | [references/vet-creator.md](references/vet-creator.md) |
| UGC creators: smaller creators who make product-style videos to hire for ads and product pages | "find UGC creators", "creators to make our ad videos", "people who film unboxings", "review-style videos for our Meta ads", "UGC for our product page" | [references/ugc-creators.md](references/ugc-creators.md) |
| Campaign brief: the brief the user sends creators, with deliverables, hooks, do and don't, disclosure | "write an influencer brief", "creator campaign brief", "what do we send creators", "influencer guidelines", "brief for our sponsored posts" | [references/campaign-brief.md](references/campaign-brief.md) |

## Shared rules

### Engagement

- Read each platform's numbers by its own group's rules and floors, in the shared rules of the [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md) and [youtube](../youtube/SKILL.md) routers. They agree on the basics: medians over a creator's recent posts, never the mean; engagement is likes plus comments over views (over followers for Instagram image posts); paid posts stay out of the organic median; a null is left out, not read as zero; a view rate (median views over followers) under about 5% means most of the following no longer watches.
- YouTube listings carry views but not likes or comments: `youtube_get_video` on three to five recent videos (1 credit each) fills them in.
- Never compare raw rates across platforms: each platform has its own engagement floor, and the same rate means different things on TikTok and YouTube. In a table across platforms, give each creator's view rate and engagement rate as a multiple of the median of the candidates on that platform (1.0 is typical for this niche there).
- Judge a creator's sponsored posts and recent posts against their own median. A sponsored post near the median means the audience trusts the creator's recommendations; one under half of it means the audience skips their ads.

### Floors

- **Size.** Nano under 10K followers, micro 10K to 100K, mid 100K to 500K, macro 500K to 1M, mega above. Default: 10K to 250K (on YouTube, a median of 2,000 to 100,000 views a video), the platform playbooks' own default. Engagement per follower falls as accounts grow while fees rise with followers, so this band usually buys the most engaged views per dollar.
- **Active.** Posted in the last 30 days. A dormant creator does not deliver on time.
- **Niche fit.** At least half of the recent posts are on the niche. One post about the topic is not a niche.
- **Sponsored share.** `is_ad: true` marks paid posts and marked partnerships where the platform says; it is null where it does not. Also count captions with "ad", "sponsored", a partner tag, a discount code or an affiliate link, because undisclosed deals look organic. Over about a third of recent posts sponsored, the feed is an ad feed and its audience skips ads.
- **Audience country.** Only TikTok publishes a split: `tiktok_get_audience` (26 credits), run through [audience check](../tiktok/references/audience-check.md). On Instagram and YouTube the tools cannot prove where an audience is; report the signals instead (caption and comment language, the profile's `location`, places named in comments) and mark them unproven.

### Credits

- Profiles, listings, searches, comments and transcripts cost 1 credit a call or a page. The expensive calls: `tiktok_get_audience` 26 (shortlist only), `tiktok_get_video` and `instagram_get_post` up to 10 when the vendor fetches media (read numbers from the listing rows, which carry them), `instagram_get_comments` with `include_replies: true` 15, and `tiktok_get_transcript` with `ai_fallback: true` 11.
- Search wide and cheap, spend on profiles and listings only for authors worth it, and run the audience split last.
- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Profiles are cached 24 hours, listings and searches 6 hours, audience splits 7 days.

### Handoff

- Never follow, message, comment on, email, contract or pay a creator, and never ship product. The deliverable is a table or a brief. If the host has an email tool, a CRM or a sequencer, offer to pass the table to it; do not send.
- Contacts: the bio first (creators often list a business email or a management agency there), then the profile's `website`. When the website is the creator's own domain, run the [contact steps](../link-building/SKILL.md#contact-steps) on it. Never guess an address.
- Paid and gifted posts must be disclosed: the platform's paid partnership label and a plain "ad" the viewer cannot miss. That duty sits with the user and the creator; the campaign brief spells it out.
- The server keeps no state. To follow campaign posts or brand mentions over time, the host stores the table and the `monitoring` group runs the checks again.

## Other groups

- Creators on one named platform: [TikTok creators](../tiktok/references/find-creators.md), [Instagram creators](../instagram/references/find-creators.md), [YouTube creators](../youtube/references/find-creators.md). Where one TikTok creator's audience lives: [audience check](../tiktok/references/audience-check.md).
- Creators for a launch day, timed with the rest of the launch: [launch creators](../launch/references/creators.md).
- Podcasts to appear on: [podcast guesting](../youtube/references/podcast-guesting.md) in `youtube`.
- Ads made from creator content, and the ad creative brief: the [paid-ads](../paid-ads/SKILL.md) group and its [creative brief](../paid-ads/references/creative-brief.md).
- Review sites, blogs and newsletters as affiliates: [affiliate partners](../link-building/references/affiliate-partners.md); reporters: [journalists](../link-building/references/journalists.md), both in `link-building`.
- Hooks and trends for the user's own posts: the [tiktok](../tiktok/SKILL.md) and [instagram](../instagram/SKILL.md) groups.
- Tracking a campaign's posts or brand mentions every week: [brand mentions](../monitoring/references/brand-mentions.md) in `monitoring`.
- Creator research for an agency's client: the [agency](../agency/SKILL.md) group.
