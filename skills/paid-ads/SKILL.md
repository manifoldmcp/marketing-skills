---
name: paid-ads
description: Paid ads research with the manifold tools. Reads the public ad libraries of Meta (Facebook and Instagram), TikTok, LinkedIn and Google to show what competitors run, since when and with what offer, builds a swipe file of a category's ads grouped by angle, offer, format and CTA, turns it into a creative brief for the user's own ads, finds search ads keywords with CPC, volume, intent and negatives, and plans a paid ads strategy. Use when the user asks about Meta ads, Facebook ads, Instagram ads, Google ads, TikTok ads, LinkedIn ads, PPC, SEM, paid search, paid social, the ad library, competitor ads, what ads a brand is running, ad spy, ad inspiration, a swipe file, winning ads, ad angles, hooks and offers, an ad creative brief, new ad concepts, keywords to bid on, CPC, negative keywords, or where to spend an ad budget. The libraries show creative and dates, not performance. The result is a table or a brief; buying, launching, editing or pausing ads is out of scope.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Paid ads

Every job here reads the same evidence: the ads a platform publishes in its public library, with the dates each one ran. The libraries show what advertisers pay to keep running, never how it performs, so the playbooks read survival as the signal and say so.

## Connector check

These playbooks run on the manifold MCP server. Before the first step, confirm that its tools are available: look for `ads_search_ads` and `ads_get_advertiser_ads` (hosts often add a prefix, for example `mcp__manifold__ads_search_ads`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `ads_*` tools are not, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. PPC keywords still runs on the `seo_*` tools without the competitor ads step; the other playbooks need the ads group.
- PPC keywords and the strategy also use `seo_*` tools, and video ads use the platform transcript tools (`tiktok_get_transcript`, `facebook_get_transcript`, `instagram_get_transcript`). If one of those groups is off, skip its step, say which, and work from the ad text.

## Menu

Pick one job from the request, open its playbook and follow it. If the request fits two jobs, ask one question. If it only names a competitor and asks about its ads, open competitor ads.

| Job | The user says | Open |
|---|---|---|
| Paid ads strategy: which platforms, what to test first, and a 90-day plan | "paid ads strategy", "should we run Meta or Google ads", "plan our first $5k of ad spend", "paid social plan for next quarter", "how do we start with PPC" | [references/strategy.md](references/strategy.md) |
| Competitor ads: what one or more competitors run across the libraries, since when, with which offer | "what ads is Notion running", "spy on competitor ads", "check the Meta ad library for Huel", "is ClickUp advertising on LinkedIn", "their longest-running ads" | [references/competitor-ads.md](references/competitor-ads.md) |
| Swipe file: a category's ads by keyword, grouped by angle, offer, format and CTA | "ad swipe file", "ad inspiration for meal kits", "what ads run in our category", "winning ads in skincare", "examples of good ads for X" | [references/swipe-file.md](references/swipe-file.md) |
| Creative brief: a brief for the user's own ads from proven ads, customer language and hooks | "write an ad creative brief", "new ad angles to test", "what should our Facebook ads say", "ad concepts for our video editor", "our ads are fatigued" | [references/creative-brief.md](references/creative-brief.md) |
| PPC keywords: search ads keywords with CPC, volume, intent and negatives, and competitors' Google ads | "PPC keywords", "Google Ads keyword list", "what keywords should we bid on", "negative keywords", "how much does a click cost for X" | [references/ppc-keywords.md](references/ppc-keywords.md) |

## Shared rules

### The libraries

- **Meta** (`platform: "facebook"`): one library for Facebook, Instagram, Messenger and Audience Network. `placements` says where each ad ran; the library names surfaces, not countries, so `countries` is empty. Search by keyword with `ads_search_ads`, or one page's ads with `ads_get_advertiser_ads` (a page name, or the numeric page id from a row's `advertiser_id` when a name matches the wrong page). Only this library marks an ad as running: `active`, which `active_only: true` filters on; the other libraries ignore `active_only`, so read `last_shown` there. It keeps stopped ads mainly for political ads and ads shown in the EU, so a Meta list is mostly what runs now.
- **TikTok** (`platform: "tiktok"`): keyword search or an advertiser name. `country` has no effect there. List rows carry dates, format and a cover image but no ad text; `ads_get_ad` with the row's `id` adds the title, landing page, countries and the published ranges.
- **LinkedIn** (`platform: "linkedin"`): keyword search, or a company name or id. List rows already carry headline, body, CTA and destination.
- **Google** (`platform: "google"`): keyed by advertiser, so `ads_search_ads` does not take it. With the competitor's domain, pass it straight to `ads_get_advertiser_ads` as `advertiser` (1 credit a page): one call, the cheapest path. When the user has only a brand name, when the domain finds nothing, or when one market's account matters, call `ads_search_advertisers` first (1 credit) with the brand name and `region`: one brand has one entry per region and similar names belong to other companies, so keep the id whose `website` is the competitor's domain and pass that id. List rows carry format (text, image, video), dates and an image, no text. `details: true` costs 25 credits a page; read the few ads that matter with `ads_get_ad` instead, passing the row's `url` as `id` (1 credit each): it returns headline, body, destination and an impressions range.

### Reading an ad

1. **The text.** `ads_get_ad` (1 credit) wherever a list row has no `body`: always on TikTok and Google. Pass the row's `id` on Meta and TikTok, its `url` on LinkedIn and Google. Read `headline`, `body`, `cta` and `destination_url`. The hook is the first line of the body.
2. **The words of a video ad.** `tiktok_get_transcript` for a TikTok ad or `facebook_get_transcript` for a Meta ad, on the ad's `url` (1 credit). The libraries do not always hand over the video: on NoData (1 credit, do not retry), look for the same video on the advertiser's own account (`tiktok_get_videos` with `sort: "popular"`, `instagram_get_reels`, `facebook_get_posts`, 1 credit a page), matched on the caption, and transcribe that post with the same platform's transcript tool (`instagram_get_transcript` for a reel, up to two minutes long). On TikTok, `ai_fallback: true` transcribes the audio when TikTok holds no transcript (11 credits). LinkedIn and Google video ads have no transcript route here; use the text. The first spoken sentence is the hook.
3. **The landing page.** `seo_get_page` on `destination_url` (free, rate limited): `title`, `h1` and `h2` show the offer the page leads with. No manifold tool reads a page's body text; the host can open it if it has a browser or fetch tool.
4. **Link it.** Every row keeps its library `url`, so the user can see the creative itself. The tools return text and a media link, not the video.

### Winners

- The libraries show creative, dates and placements. They never show clicks, CTR, conversions, CPA or ROAS. Say so in every deliverable.
- An advertiser stops paying for an ad that loses, so the best proxy for a winner is an ad that has run a long time and still runs. Days running is `last_shown` minus `first_shown`, or today minus `first_shown` while it runs. Still running means `active: true` on Meta, or `last_shown` within the last 7 days elsewhere. When a library gives neither, days running is unknown: say so rather than guess.
- Running 30 days or more and still running: a likely winner. 90 days or more: proven. Under 14 days: a test, which says what the advertiser is trying, not what works.
- Many near-identical ads launched on the same day are a test. The one still running a month later won it. The same body across many formats and placements is a winner being scaled.
- `impressions` and `spend` are the ranges the library publishes, as text ("1K-10K", "< 1k"), and often null. Quote them as given. Never turn them into numbers, add them up or rank on them.

### Credits

- Ad lists cost 1 credit a page, `ads_get_ad` and `ads_search_advertisers` 1 credit, Google `details: true` 25 a page. Transcripts cost 1 credit, 11 with `ai_fallback: true`. Lists are cached 24 hours and one ad or an advertiser search 7 days, so a second pass the same day is free.
- Say the estimate before the first paid call; each playbook gives its default. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Page with `meta.cursor` only while new rows keep coming. Three pages per library per advertiser covers what most advertisers run.

### Handoff

- Never buy, launch, edit, pause or boost ads, and never ask for access to an ad account. The server reads public libraries only. The deliverable is a table or a brief the user takes into Ads Manager, TikTok Ads Manager, Campaign Manager or Google Ads.
- The user's own results (spend, CPA, ROAS) live in their ad accounts, which the server does not read. Ask for them when a playbook needs them.
- Write full ad copy or scripts only when the user asks. A brief gives angles, hooks and the evidence behind each.
- The server keeps no state. To watch a competitor's ads over time, the host stores the table and the `monitoring` group runs it again.

## Other groups

- Everything about a competitor beyond its ads (SEO, pricing, messaging, positioning): the [competitors](../competitors/SKILL.md) group, starting with its [teardown](../competitors/references/teardown.md).
- Alerts when a competitor launches new ads, every week: [competitor watch](../monitoring/references/competitor-watch.md) in `monitoring`, which reruns competitor ads on a schedule.
- What customers say about the problem, across Reddit, reviews and comments: [pain points](../customers/references/pain-points.md) in `customers`.
- Organic keywords and content that ranks: the [seo](../seo/SKILL.md) group.
- Organic posts, trends and hooks on one platform: [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [youtube](../youtube/SKILL.md), [linkedin](../linkedin/SKILL.md), [facebook](../facebook/SKILL.md).
- Creators to make the ads or to post for the brand (UGC, sponsored posts, whitelisting): the [influencers](../influencers/SKILL.md) group.
- No channel chosen yet, or "more signups" with no channel: [growth-plan](../growth-plan/SKILL.md); which channels to use at all: [channels](../content/references/channels.md) in `content`.
- Ads research for an agency's client or prospect: the [agency](../agency/SKILL.md) group.
