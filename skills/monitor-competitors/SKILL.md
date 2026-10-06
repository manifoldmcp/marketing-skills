---
name: monitor-competitors
description: When the user wants to keep watching what competitors do. Runs every week on the host's schedule and reports only what changed at two or three competitors since the last run, new and stopped ads in the Meta, LinkedIn, TikTok and Google libraries, new posts on LinkedIn, X, YouTube, TikTok, Instagram and Facebook, moves in organic traffic and new top pages, bidding on the user's brand, and new headlines on their home and pricing pages. Also use when the user mentions watch our competitors, competitor alerts, alert me when a competitor launches new ads, weekly competitor updates, track what a rival posts, or competitive intel every Monday. A one-off look at one competitor goes to tear-down-competitor, their ads once to research-meta-ads, research-tiktok-ads, research-linkedin-ads or research-google-ads, one weekly digest of everything to write-weekly-report.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Competitor watch

What changed at two or three competitors since the last run: ads they started or stopped, what they posted, how their search footprint moved, and whether their key pages changed. It runs weekly and reports only the changes. The full picture of one competitor is a one-off: [tear-down-competitor](../tear-down-competitor/SKILL.md), and for ads [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md). This skill watches what those found.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `ads_get_advertiser_ads` and `seo_get_domain_overview` (hosts often add a prefix, for example `mcp__manifold__ads_get_advertiser_ads`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- The steps cross tool groups: `ads_*` for ads, the platform groups for posts, `seo_*` for search footprint and pages. If some are there and others are not, the missing ones are switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say which, run what the rest allow, and mark those sections "not watched" in every report rather than reporting zero.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the competitors with their domains and handles, the user's brand name) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Competitors**: two or three, each with its domain and its handles: the Facebook page, the LinkedIn company slug, the X, YouTube and TikTok handles where it is active. Take them from a teardown if one ran; otherwise ask. The host stores them.
- **What to watch**: ads (Meta, LinkedIn, TikTok, Google), posts (LinkedIn, X, YouTube, TikTok, and Instagram or Facebook where they matter), search footprint, and the home and pricing pages. Default: all four, on the platforms where each competitor is active.
- **Cadence**: weekly by default. The ad libraries are cached 24 hours and domain overviews 7 days, so a daily run repeats the same numbers.
- **Budget**: per competitor, about 4 ad listings + 4 post listings + 5 for the overview = 13 credits, plus 2 for the brand page in step 4: about 41 a run for three competitors and about 176 a month weekly. The first run may add `ads_search_advertisers` for Google (1 credit per competitor). `seo_get_page` is free. Say both numbers before setting up the schedule; pass `max_credits` on every call so no run overspends.

## Steps

1. **Set up (first run only).** Settle the inputs, store them with the host (see [Schedule and state](#schedule-and-state)), and set the schedule. On Google, resolve each advertiser as [the libraries](../create-paid-ads-plan/references/ad-libraries.md#the-libraries) set out, and the host stores the id for the user's region. The first run is the baseline: store every ad id with its `first_shown`, `last_shown` and `active`, every post id, the overview numbers and the page headings, and report them as the starting picture.
2. **Ads.** `ads_get_advertiser_ads` for each competitor on each library it uses (1 credit a page; leave `details` off, since on Google it costs 25). Pass `active_only: true` on Facebook, the only library that applies it; the others return stopped ads too and leave `active` null, so read `last_shown` there. New ids are new ads: read `headline`, `body`, `cta`, `destination_url` and `first_shown`. An ad has stopped when an id stored as active no longer appears on Facebook, once every page is read (follow `meta.cursor` before calling an ad stopped, 1 credit a page), or when `last_shown` falls before the last run on the other libraries. A `destination_url` not seen before is a new landing page or offer. When a new push needs its angles read, run that library's skill ([research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md)) for that competitor.
3. **Posts.** One listing per channel per competitor (1 credit a page): `linkedin_get_company_posts`, `twitter_get_tweets`, `youtube_get_videos`, `tiktok_get_videos`, and `instagram_get_posts` or `facebook_get_posts` where they matter. Posts with ids not stored are new. For each: topic, format, and engagement against the account's stored median. LinkedIn company posts carry no engagement; `linkedin_get_post` (1 credit) for the one or two that look like announcements. Flag launches, pricing changes, funding, hires and customer wins.
4. **Search footprint.** `seo_get_domain_overview` on each competitor's domain (5 credits): `organic_traffic`, `organic_keywords`, `positions` and `top_pages[]` against the stored numbers. A URL new to `top_pages[]` is a page that started earning traffic: a comparison page, a free tool, a new product page. For the keywords behind it, `seo_get_ranked_keywords` on that URL (10 credits), only when it matters. Then `seo_get_serp` on the user's brand name with `device: "desktop"` and with `device: "mobile"` (1 credit each): a competitor new in the `type: "paid"` rows has started bidding on the user's name, which [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md) reads in full.
5. **Pages.** `seo_get_page` on each competitor's home and pricing pages (free, rate limited). Compare `title`, `meta_description`, `h1` and `h2` with the stored ones: a new headline is new positioning, and new plan names in the headings mean the pricing changed. The tool does not return prices or body text; if the headings changed, the host can open the page if it has a browser, or the user can.
6. **Deliver and store.** A change table: competitor, channel, what changed (new ad, stopped ad, new landing page, new post, traffic move, new top page, new headline), the detail (headline, offer, topic, the numbers before and after), evidence URL, first seen, and why it matters. Then the host stores the new state: ad ids with `active` and `last_shown`, post ids, overview numbers, page headings, the run's date and credits. A quiet week is one line per competitor.

## Judgment

- Flag any new ad id, and a competitor post at twice that account's median engagement. Lead with what changed; levels come second, as context.
- One new ad is a test; five new ads on one angle are a push. Report the pattern, not every creative.
- An ad that stays active week after week is one that works for them. An ad that stopped within a week or two was most likely a test that lost. Keep the stored `first_shown` so each run can say how long an ad has run.
- Impressions and spend are ranges, and only some libraries publish them. Never add ranges up or turn them into a budget estimate.
- The overview numbers are estimates that move slowly. A weekly change under about 10 percent is noise; report traffic month over month and flag weekly only a jump or a new top page.
- `twitter_get_tweets` returns one page, so a competitor posting more than a page between runs leaves a gap. LinkedIn `created_at` is approximate; dedupe on `id`.
- Do not answer every move. The report names at most three changes worth a response, each with the skill that acts on it: [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md) or [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md) for ads, [plan-comparison-pages](../plan-comparison-pages/SKILL.md) or [create-seo-content-plan](../create-seo-content-plan/SKILL.md) for new pages, [find-positioning](../find-positioning/SKILL.md) for positioning. Answering is the user's call.

## Schedule and state

- The host runs the schedule. If it has a scheduler (a scheduled task, a routine, a cron job), set the skill up there with the cadence the user chose. If it has none, say so: the user asks again each week, and the host reruns the skill with the stored state.
- The host stores the settings (competitors, domains, handles, libraries, Google advertiser ids) with the state from step 6 where the next run can read it: a file, a doc, a sheet, its memory. Without the stored state every run is a first run. A competitor or channel added later starts its own baseline; report it apart until it has history.
- A month is about 4.3 weekly runs. Ad lists and profiles are cached 24 hours, platform listings 6 hours, single ads and domain overviews 7 days: a rerun inside the window returns the same numbers for nothing and adds nothing new. `dry_run: true` prices any call for free.
- The deliverable is the change table. The server never sends alerts; if the host has an email, chat or notification tool, offer to send the table there.

## Related skills

- The full picture of one competitor, once: [tear-down-competitor](../tear-down-competitor/SKILL.md). Their ads read in depth: [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md).
- A competitor bidding on the user's brand: [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md).
- How to win against them: [create-competitor-plan](../create-competitor-plan/SKILL.md).
- Competitors, mentions, ranks and AI visibility in one weekly report: [write-weekly-report](../write-weekly-report/SKILL.md).
