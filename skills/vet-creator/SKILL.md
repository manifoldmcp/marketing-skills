---
name: vet-creator
description: When the user wants to check a creator before paying them. Vets one creator on TikTok, Instagram and YouTube against their own baseline (median views, view rate, engagement, sponsored share, comment quality, a follower sample and TikTok's audience split by country) and ends in a scorecard with a verdict and a fair price, or checks one to five TikTok creators' audiences by country with a pass, check or fail. Also use when the user mentions vet this influencer, are her followers fake, fake followers, is this creator worth the fee, check a creator before we pay, how many sponsored posts does he do, engagement rate, or where a TikToker's audience is. Finding creators goes to find-creators, the brief to write-creator-brief, and a creator program plan to create-influencer-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Vet a creator

Before the user pays a creator, check that the audience is real, still watching, reacting in its own words, not tired of ads, and in the countries the user sells to. This skill reads one creator on every platform they use, judges each number against the creator's own baseline, and ends in a scorecard with a verdict and a fair price. For TikTok creators whose audience country is the only question, it runs the audience check alone.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_get_audience`, `instagram_get_profile` and `youtube_get_channel` (hosts often add a prefix, for example `mcp__manifold__tiktok_get_audience`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If one platform's tools are missing (all `tiktok_*`, `instagram_*` or `youtube_*`), that group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, vet the creator on the platforms that are on, and mark the others "not checked".

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the countries the user sells in, the competitors whose deals to flag, and the paid social cost if it is there) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Creator**: a handle or profile URL, and the platforms they post on. If the user gives one platform, check the bio and `website` for the others.
- **Deal**: the fee, product or commission on the table, and the deliverables. The fee sets the depth: product-only gifting needs steps 2, 4 and 5, with one page of `tiktok_get_videos` in place of the full audience check; a flat fee needs every step.
- **Market**: the countries the user sells in, for the audience check.
- **Paid social cost**: what the user pays per 1,000 impressions on Meta or TikTok ads, if they know it. Step 8 prices the creator against it.
- **Budget**: say the case's estimate before starting; pass `max_credits` if the user gave a budget.
  - Full vetting on a creator with TikTok and Instagram: about 32 + 1 + 2 + 3 + 1 + 2 = 41 credits: the TikTok audience check, the Instagram profile, one page each of reels and posts, comments on three Instagram posts and one sponsored TikTok, and two more pages of TikTok followers. Add about 1 + 2 + 3 + 3 + 2 = 11 for YouTube.
  - TikTok audience check only: about 32 credits per creator, 96 for three.

## Steps

1. **Pick the case.** One to five TikTok handles where the question is only whether the audience is real and in the right country: follow the [TikTok audience check](references/tiktok-audience.md) alone and deliver its table. One creator before a deal, on any platform: steps 2 to 9.
2. **Profiles.** `instagram_get_profile` and `youtube_get_channel` (1 credit each; the TikTok profile comes in step 3). Read `followers`, `following`, `posts_count`, `bio`, `website` and `created_at`. Confirm the accounts are one person (the bios link each other, or name the same site). A `following` close to `followers` is follow-for-follow growth.
3. **TikTok.** Run the [TikTok audience check](references/tiktok-audience.md) on the handle (about 32 credits). Take its median views, view rate, engagement rate, comment quality, follower sample (`tiktok_get_followers`) and the share of the audience in the user's market. Its follower sample is one page; when it is mixed, page `tiktok_get_followers` twice more (1 credit a page) and count the accounts with no bio, a name made of digits and zero followers of their own. Many real viewers look like that, so read the count with the view rate: a mostly hollow sample and a view rate under about 5% together mean an inflated following; a hollow sample alone does not.
4. **Instagram and YouTube baseline.** `instagram_get_reels` and `instagram_get_posts` (1 credit a page), `youtube_get_videos` (1 credit a page, two pages), and on YouTube `youtube_get_video` on three recent videos (1 credit each) for likes and comments. Per platform, work out median views, view rate and engagement rate by that platform's rules (see [engagement and floors](#engagement-and-floors)), the posting cadence and the days since the last post. Flag a single viral post that carries the numbers, and a drop in views over the last month against the months before.
5. **Sponsored share.** Across every platform's recent posts, count those with `is_ad: true` or a sponsored caption (see [engagement and floors](#engagement-and-floors)). On YouTube the sponsor read is spoken and often missing from the description: `youtube_get_transcript` on two recent videos (1 credit each) finds "sponsored by" and "use code", and where in the video the read sits. Compare the sponsored posts' views with the creator's own median. List the brands they worked with, and flag the user's direct competitors.
6. **Comment quality.** `instagram_get_comments` (without `include_replies`, which costs 15) and `youtube_get_comments` on three recent posts each, and `tiktok_get_comments` on one sponsored TikTok if the audience check read only organic ones (1 credit a page). Read the share of comments that respond to the content in the commenter's own words against generic ones ("love this", emoji only), the same text from different accounts (pods or bots), questions about the product under the sponsored post ("where is this from", "does it work for"), and the language commenters write in. More than half generic is a failed check.
7. **Audience country.** TikTok's share in the user's market comes from step 3. Instagram and YouTube have no split and no follower tool here: report the signals from [engagement and floors](#engagement-and-floors), mark them unproven, and lean on the view rate.
8. **Price.** Effective CPM is the fee divided by the median views of one post, times 1,000. Compare it with the user's paid social CPM: at several times that, the post has to bring something ads cannot (trust, content the user can reuse) or the price comes down. As a rule of thumb for the US in 2026, flat fees land around $15 to $35 per 1,000 median views for a TikTok or a Reel and $20 to $50 for a YouTube integration (about double for a dedicated video); paid usage of the post (Spark Ads, partnership ads) adds 20 to 50% of the fee per 30 days, and category exclusivity 10 to 30%.
9. **Deliver** a scorecard per platform: followers, median views, view rate, engagement rate, cadence, last post, sponsored share, sponsored views against the median, comment quality (share specific, two quoted examples, pod signs), follower sample (share suspicious, TikTok only), audience in the user's market (TikTok share, or the signals), brands worked with, effective CPM. Then a verdict: go, go at a lower fee (name the fee that meets the user's CPM), or skip, with the two reasons that decided it.

## Judgment

- The creator's own baseline is the fair comparison. A 3% engagement rate is poor for one creator and normal for another; the drop or the gap against their own median is the signal.
- No tool shows follower history, so a following bought years ago shows only as a low view rate and a hollow follower sample. Name that as the likely cause when both are there.
- Engagement pods inflate comments with short praise from the same few accounts. Reading twenty comments beats any rate.
- A creator with 20K followers and a 30% view rate is worth more than one with 500K and 2%. Size is what they charge for; median views is what the user gets.
- The TikTok audience split comes from a sample of a few hundred followers. For a creator strong on Instagram or YouTube, their TikTok split is only a proxy for the audience there.
- A verdict is a judgment on public numbers. It cannot see the creator's reliability, contract terms or past sales; ask the user to request a media kit with results from past deals.
- Credits: run the audience split last, on the shortlist only. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Profiles are cached 24 hours, listings 6 hours, audience splits 7 days.
- Never follow, message, comment on, email, contract or pay a creator. The deliverable is the scorecard.

## Engagement and floors

- Read each platform's numbers by its own rules and floors, in the platform notes: [TikTok](../create-tiktok-plan/references/platforms/tiktok.md), [Instagram](../create-instagram-plan/references/platforms/instagram.md) and [YouTube](../create-youtube-plan/references/platforms/youtube.md). Medians over recent posts, never the mean; engagement is likes plus comments over views (over followers for Instagram image posts); paid posts stay out of the organic median; a null is left out, not read as zero; a view rate under about 5% means most of the following no longer watches.
- Judge sponsored posts against the creator's own median. Near it, the audience trusts their recommendations; under half of it, the audience skips their ads.
- **Sponsored share.** `is_ad: true` marks paid posts and marked partnerships where the platform says; it is null where it does not. Also count captions with "ad", "sponsored", a partner tag, a discount code or an affiliate link, because undisclosed deals look organic. Over about a third of recent posts sponsored, the feed is an ad feed and its audience skips ads.
- **Audience country.** Only TikTok publishes a split (`tiktok_get_audience`, 26 credits). On Instagram and YouTube the tools cannot prove where an audience is; report the signals instead (caption and comment language, the profile's `location`, places named in comments) and mark them unproven.

## Related skills

- Creators to vet in the first place: [find-creators](../find-creators/SKILL.md).
- The brief the creator gets once the deal is on: [write-creator-brief](../write-creator-brief/SKILL.md).
- Which creators, platforms and deal types, as a plan: [create-influencer-plan](../create-influencer-plan/SKILL.md).
- Creators for a launch day: [create-launch-plan](../create-launch-plan/SKILL.md).
