# Vet a creator

Before the user pays a creator, check that the audience is real, still watching, reacting in its own words, and not tired of ads. This playbook reads one creator on every platform they use, judges each number against the creator's own baseline, and ends in a scorecard with a verdict and a fair price.

## Inputs to settle first

- **Creator**: a handle or profile URL, and the platforms they post on. If the user gives one platform, check the bio and `website` for the others.
- **Deal**: the fee, product or commission on the table, and the deliverables. The fee sets the depth: product-only gifting needs steps 1, 3 and 4, with one page of `tiktok_get_videos` in place of the full audience check; a flat fee needs every step.
- **Market**: the countries the user sells in, for the audience check.
- **Paid social cost**: what the user pays per 1,000 impressions on Meta or TikTok ads, if they know it. Step 7 prices the creator against it.
- **Budget**: a default run on a creator with TikTok and Instagram costs about 32 + 1 + 2 + 3 + 1 + 2 = 41 credits: the TikTok audience check, the Instagram profile, one page each of reels and posts, comments on three Instagram posts and one sponsored TikTok, and two more pages of TikTok followers. Add about 1 + 2 + 3 + 3 = 9 for YouTube. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Profiles.** `instagram_get_profile` and `youtube_get_channel` (1 credit each; the TikTok profile comes in step 2). Read `followers`, `following`, `posts_count`, `bio`, `website` and `created_at`. Confirm the accounts are one person (the bios link each other, or name the same site). A `following` close to `followers` is follow-for-follow growth.
2. **TikTok.** Run the [audience check](../../tiktok/references/audience-check.md) on the handle (about 32 credits). Take its median views, view rate, engagement rate, comment quality, follower sample (`tiktok_get_followers`) and the share of the audience in the user's market. Its follower sample is one page; when it is mixed, page `tiktok_get_followers` twice more (1 credit a page) and count the accounts with no bio, a name made of digits and zero followers of their own. Over about half such accounts means an inflated following.
3. **Instagram and YouTube baseline.** `instagram_get_reels` and `instagram_get_posts` (1 credit a page), `youtube_get_videos` (1 credit a page, two pages), and on YouTube `youtube_get_video` on three recent videos (1 credit each) for likes and comments. Per platform, work out median views, view rate and engagement rate by that platform's rules (see the [router](../SKILL.md#engagement)), the posting cadence and the days since the last post. Flag a single viral post that carries the numbers, and a drop in views over the last month against the months before.
4. **Sponsored share.** Across every platform's recent posts, count those with `is_ad: true` or a sponsored caption (see the [floors](../SKILL.md#floors)). Compare the sponsored posts' views with the creator's own median. List the brands they worked with, and flag the user's direct competitors.
5. **Comment quality.** `instagram_get_comments` (without `include_replies`, which costs 15) and `youtube_get_comments` on three recent posts each, and `tiktok_get_comments` on one sponsored TikTok if the audience check read only organic ones (1 credit a page). Read the share of comments that respond to the content in the commenter's own words against generic ones ("love this", emoji only), the same text from different accounts (pods or bots), questions about the product under the sponsored post ("where is this from", "does it work for"), and the language commenters write in. More than half generic is a failed check.
6. **Audience country.** TikTok's share in the user's market comes from step 2. Instagram and YouTube have no split and no follower tool here: report the signals from the router's [floors](../SKILL.md#floors), mark them unproven, and lean on the view rate.
7. **Price.** Effective CPM is the fee divided by the median views of one post, times 1,000. Compare it with the user's paid social CPM: at several times that, the post has to bring something ads cannot (trust, content the user can reuse) or the price comes down.
8. **Deliver** a scorecard per platform: followers, median views, view rate, engagement rate, cadence, last post, sponsored share, sponsored views against the median, comment quality (share specific, two quoted examples, pod signs), follower sample (share suspicious, TikTok only), audience in the user's market (TikTok share, or the signals), brands worked with, effective CPM. Then a verdict: go, go at a lower fee (name the fee that meets the user's CPM), or skip, with the two reasons that decided it.

## Judgment

- The creator's own baseline is the fair comparison. A 3% engagement rate is poor for one creator and normal for another; the drop or the gap against their own median is the signal.
- No tool shows follower history, so a following bought years ago shows only as a low view rate and a hollow follower sample. Name that as the likely cause when both are there.
- Engagement pods inflate comments with short praise from the same few accounts. Reading twenty comments beats any rate.
- A creator with 20K followers and a 30% view rate is worth more than one with 500K and 2%. Size is what they charge for; median views is what the user gets.
- The TikTok audience split comes from a sample of a few hundred followers. For a creator strong on Instagram or YouTube, their TikTok split is only a proxy for the audience there.
- A verdict is a judgment on public numbers. It cannot see the creator's reliability, contract terms or past sales; ask the user to request a media kit with results from past deals.
