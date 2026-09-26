# Find Instagram creators

Instagram creators who already post in the user's niche, found through the authors of the niche's hashtag results, sized with their profile, and kept only when they post often and their following responds. It ends in a ranked table of about 20 creators with the evidence for each.

## Inputs to settle first

- **Hashtags**: four or five tags the niche uses: the category, the problem the product solves, a use case, and a competitor's brand tag (creators who posted about a rival). Default: the category tag, then the tags that recur in its captions.
- **Size**: a follower band. Default: 10,000 to 250,000, where cost per view is usually best; say so if the user wants bigger names.
- **Language**: hashtag search has no country filter, so keep creators whose captions and speech are in the user's language.
- **Count**: 20 creators by default.
- **Budget**: a default run costs about 5 x 2 + 40 + 25 = 75 credits for five hashtags. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the hashtags.** `instagram_search_posts` for each tag with `since: "month"`, two pages each (1 credit a page). Collect the authors, with how many matching posts each has and their best numbers. Drop brands: an author that is a company, or whose every row has `is_ad: true`. The profile's `kind` does not settle it, since many creators run business accounts too.
2. **Size them.** `instagram_get_profile` on the top 40 authors by matching posts, then engagement (1 credit each). Keep the ones inside the size band. Note the `bio` (the topic, often a business email) and `website`.
3. **Check activity and engagement.** For the best 25, `instagram_get_reels`, one page each (1 credit): the last post date, reels a week, median reel views, median views over followers, median reel engagement rate, how many recent reels are on the niche, and any paid posts (`is_ad: true`). For a creator who posts mostly images, use `instagram_get_posts` instead (1 credit) and the engagement rate over followers. Apply the [floors](../SKILL.md#floors).
4. **Rank.** Niche fit first (the share of recent posts on the topic), then median reel views, then engagement rate. Cut to the count.
5. **Find a contact.** Take a business email from the bio where there is one. Where a creator lists a website, run the link-building [contact steps](../../link-building/SKILL.md#contact-steps) on that domain with `limit: 10`. Otherwise the contact is an Instagram DM, which the user sends.
6. **Deliver** a table: creator (handle and link), followers, median reel views, views over followers, median engagement rate (and its base: views or followers), posts a week, last post, niche fit (the share of recent posts, and one example link), paid posts seen, contact (bio email, website address and its verification status, or DM), and one line on why they fit.

## Judgment

- A creator's median reel views, not their followers, predicts what a sponsored reel will get. For a creator who posts mostly images, the engagement rate over followers is the measure.
- Paid partnership posts in a creator's feed show they already take deals. Compare those posts' numbers with the creator's median: a sponsored post far below it means their following tunes out ads.
- Hashtag search finds only creators who tag their posts, and many good ones do not. When the list is thin, add tags, or start from the creators in competitors' paid partnerships ([competitor accounts](competitor-accounts.md)).
- Instagram publishes no audience split. For a creator also on TikTok, run the TikTok [audience check](../../tiktok/references/audience-check.md) on that handle as a proxy; for vetting across platforms, use the [influencers](../../influencers/references/vet-creator.md) group's vet a creator.
- For creators across Instagram, TikTok and YouTube, the [influencers](../../influencers/references/find-creators.md) group combines this playbook with the others.
- Never send a DM from here. Outreach copy and the brief belong to the [influencers](../../influencers/references/campaign-brief.md) group, and only when the user asks.
