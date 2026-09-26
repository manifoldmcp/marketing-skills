# Find TikTok creators

TikTok creators who already make videos in the user's niche, found through the authors of the niche's search results, sized with their profile, and kept only when they post often and their audience responds. It ends in a ranked table of about 20 creators with the evidence for each.

## Inputs to settle first

- **Niche**: three to five search terms: the category, the problem the product solves, a use case, and a competitor's name (creators who reviewed a rival). Default: the category and two problems in the user's own words.
- **Size**: a follower band. Default: 10,000 to 250,000, where views per dollar are usually best; say so if the user wants bigger names.
- **Language**: search has no country filter, so keep creators whose captions and speech are in the user's language. The [audience check](audience-check.md) confirms the country for the finalists.
- **Count**: 20 creators by default.
- **Budget**: a default run costs about 5 x 3 + 40 + 25 = 80 credits for five terms. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the niche.** `tiktok_search_videos` for each term with `since: "month"` and the default relevance sort, three pages each (1 credit a page). Niche fit matters more than one viral video here. Collect the authors, with how many matching videos each has and their best views. Drop brands: an author that is a company, or whose every row has `is_ad: true`.
2. **Size them.** `tiktok_get_profile` on the top 40 authors by matching videos, then views (1 credit each). Keep the ones inside the size band. Note the `bio` (the topic, often a business email), `website` and `verified`.
3. **Check activity and engagement.** `tiktok_get_videos` with `sort: "latest"`, one page each for the best 25 (1 credit each): the last post date, videos a week, median views, median views over followers, median engagement rate, how many recent videos are on the niche, and any paid posts (`is_ad: true`). Apply the [floors](../SKILL.md#floors): active in the last 30 days, engagement and views over followers above the floor.
4. **Rank.** Niche fit first (the share of recent videos on the topic), then median views, then engagement rate. Cut to the count.
5. **Find a contact.** Take a business email from the bio where there is one. Where a creator lists a website, run the link-building [contact steps](../../link-building/SKILL.md#contact-steps) on that domain with `limit: 10`. Otherwise the contact is a TikTok message, which the user sends.
6. **Deliver** a table: creator (handle and link), followers, median views, views over followers, median engagement rate, videos a week, last post, niche fit (the share of recent videos, and one example link), paid posts seen, contact (bio email, website address and its verification status, or TikTok message), and one line on why they fit.

## Judgment

- A creator's median views, not their followers, predicts what a sponsored video will get. Rank and price on it.
- Paid partnership posts in a creator's feed show they already take deals. Compare those videos' views with the creator's median: a sponsored video far below it means their audience tunes out ads.
- Search is ranked, so a fourth page of one term is mostly weaker matches. Add a term rather than a page.
- Run the [audience check](audience-check.md) on the finalists before any money moves (about 32 credits each): it shows whether the audience is real and in the user's market.
- For creators across TikTok, Instagram and YouTube, the [influencers](../../influencers/references/find-creators.md) group combines this playbook with the others, and its [vet a creator](../../influencers/references/vet-creator.md) handles cross-platform vetting.
- Never message a creator from here. Outreach copy and the brief belong to the [influencers](../../influencers/references/campaign-brief.md) group, and only when the user asks.
