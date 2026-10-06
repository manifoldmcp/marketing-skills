---
name: find-brand-fans
description: When the user wants to find creators already posting about their brand without being paid. Searches TikTok, Instagram and YouTube for organic posts that name or tag the brand, keeps the authors who are real creators, checks how their brand posts did against their own median and whether viewers asked where to buy, and ends in a table of fans with the deal that fits each, a gift, an affiliate code, a paid post or running their post as an ad. Also use when the user mentions brand fans, brand advocates, creators who already post about us, organic creator mentions, gifting or seeding to existing fans, affiliate codes for fans, or Spark Ads from creator posts. Creators in a niche who never posted about the brand go to find-creators, UGC makers for ads to find-ugc-creators, and ongoing mention alerts to monitor-brand-mentions.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find brand fans

Creators who already post about the brand without being paid are the cheapest creators to work with and the most believable: their audience has watched them use the product. This skill finds the organic posts that name or tag the brand on TikTok, Instagram and YouTube and keeps the authors who are real creators. It ends in a table of fans with the deal that fits each: a gift, an affiliate code, a paid post, or the right to run their post as an ad.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos`, `instagram_search_posts` and `youtube_search_videos` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If one platform's tools are missing (all `tiktok_*`, `instagram_*` or `youtube_*`), that group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and run on the platforms that are on.
- Contacts at a creator's own website need the `leads_*` tools. If that group is missing, it is switched off: deliver the table with the bio contact only, and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand and product names, the brand's own accounts and hashtag, the countries that matter) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Brand terms**: the brand name, product names, the brand's hashtag and common misspellings. A brand name that is also a common word ("Notion", "Ring") needs a product word next to it, or the search returns everything else.
- **Platforms**: default TikTok, Instagram and YouTube.
- **Size**: no lower floor by default: a 2,000-follower customer who posts unprompted is worth a gift. Set a ceiling only if the user has no budget for larger creators.
- **Exclusions**: the brand's own accounts and the creators the user already pays or gifts. The host holds them.
- **Budget**: a default run costs about 6 + 4 + 4 + 30 + 20 + 10 = 74 credits: three TikTok terms and two Instagram hashtags and two YouTube searches at two pages each, 30 profiles, 20 listings and comments on 10 posts. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the brand.** `tiktok_search_videos` with each brand term and `since: "year"`, two pages each (1 credit a page). `instagram_search_posts` with the brand hashtag and the main product hashtag, `since: "year"`, two pages each. `youtube_search_videos` with the brand name and "<brand> review", `since: "year"`, two pages each. Group the rows by `author`.
2. **Keep the organic fans.** Drop the exclusions, resellers and pages that repost other people's videos. Drop rows with `is_ad: true` or a sponsored caption ("ad", "#partner", a discount code for the brand): those are already paid. Read each caption for its stance: praise, a how-to and a comparison the brand wins are fans; a complaint goes on a separate list for the user's support team.
3. **Profiles.** `tiktok_get_profile`, `instagram_get_profile` or `youtube_get_channel` on up to 30 fans (1 credit each): `followers`, `bio`, `website`, and signs they already take deals (a business email, "collabs", a management agency).
4. **Their recent work.** `tiktok_get_videos`, `instagram_get_reels` or `youtube_get_videos` on the top 20 (1 credit a page): median views, how many recent posts mention the brand (more than one marks a real fan), the share on the niche, the sponsored share, and the views of the brand posts against their own median. Read the numbers by the platform notes ([TikTok](../create-tiktok-plan/references/platforms/tiktok.md), [Instagram](../create-instagram-plan/references/platforms/instagram.md), [YouTube](../create-youtube-plan/references/platforms/youtube.md)): medians, never the mean, and a null left out, not read as zero. Drop fans with no post in the last 30 days, and those whose recent posts are more than about a third sponsored; the size floor does not apply here.
5. **What their audience asked.** `tiktok_get_comments`, `instagram_get_comments` or `youtube_get_comments` on the best brand post of the top 10 (1 credit a page). Questions such as "where do I get it" or "link?" show the post moved people to buy.
6. **Pick the deal** for each fan:
   - Gift or early access: nano creators and anyone who posted more than once unpaid.
   - Affiliate code: fans whose brand posts drew buying questions.
   - Paid post: fans whose brand posts reached their own median or beat it, in the size band the budget allows.
   - Run their post as an ad: a brand post that beat their median. That needs their permission and a TikTok Spark Ads code or Meta partnership-ads access.
7. **Deliver** a table: creator, platform and link, followers, median views, brand posts (count, best URL, views against their median), stance, buying questions seen, already takes deals, the suggested deal, and contact. Add the complaints list below it.

## Judgment

- A fan who posted twice unprompted is worth more than a bigger creator who never has. Their audience believes them, and they will say yes faster.
- Their post is theirs. Reposting it needs their consent and credit; running it as an ad needs their authorization and usually a fee.
- Once the brand gives anything, a gift included, every later post about it must be disclosed: the platform's paid partnership label and a plain "ad" the viewer cannot miss. [write-creator-brief](../write-creator-brief/SKILL.md) states how.
- Search finds only the fans who name or tag the brand, and ranks them. Misspellings and product names find more; a thin result for a young brand is normal, not a verdict. Instagram search is by hashtag only, so fans who do not tag the brand are missed there.
- A complaint with reach needs an answer this week. Keep it off the fan list but put it in front of the user.
- Contacts: the bio first (a business email or a management agency), then the profile's `website`. When the website is the creator's own domain, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on it with `department: ["executive", "marketing"]` and `limit: 10`. Never guess an address.
- Credits: profiles, listings, searches and comments cost 1 credit a call or a page; `tiktok_get_video` and `instagram_get_post` cost up to 10 when the vendor fetches media, so read numbers from the listing rows, and `instagram_get_comments` with `include_replies: true` costs 15. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free.
- Never follow, message, comment on, email, contract or pay a creator, and never ship product. The deliverable is a table.
- Watching for new posts about the brand every week is [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md); this search is the one-off list of fans to work with. The server keeps no state, so the host stores the table between runs.

## Related skills

- Creators in the niche who have not posted about the brand: [find-creators](../find-creators/SKILL.md), or one platform with [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-instagram-creators](../find-instagram-creators/SKILL.md) and [find-youtube-creators](../find-youtube-creators/SKILL.md).
- Creators who film reviews and unboxings for ads, paid per video: [find-ugc-creators](../find-ugc-creators/SKILL.md).
- One fan checked before a paid deal: [vet-creator](../vet-creator/SKILL.md). The brief and the disclosure rules: [write-creator-brief](../write-creator-brief/SKILL.md).
- New brand mentions every week: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
