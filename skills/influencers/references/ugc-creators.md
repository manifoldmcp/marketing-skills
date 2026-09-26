# UGC creators

UGC creators make videos for the brand to run as ads or put on its product pages: reviews, unboxings, demos, "TikTok made me buy it" stories. The user pays per video and posts it on their own accounts, so the creator's audience matters little and the craft matters most. This playbook finds smaller creators already making product-style videos in or near the category and ends in a table with an example of each one's work.

## Inputs to settle first

- **Product and category**: what the videos will show, and two or three competitor or adjacent products whose reviews count as proof of fit.
- **Styles**: which kinds of video the user wants: review, unboxing, demo, problem and solution, before and after, "TikTok made me buy it", get ready with me. Default: review and unboxing.
- **Platforms**: default TikTok and Instagram, where the style lives. Add YouTube when the category has long-form reviewers; its search returns regular videos, not Shorts.
- **Size**: default 1K to 50K followers. The user pays per video, and small creators charge less, answer faster and look like customers, which is the point of UGC.
- **Count**: default 20 creators in the final table.
- **Budget**: a default run costs about 10 + 6 + 30 + 20 + 10 = 76 credits: five TikTok queries at two pages, three Instagram hashtags at two pages, 30 profiles, 20 listings and 10 transcripts. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search for product-style videos.** `tiktok_search_videos` with queries that pair a style with the category: "<category> review", "<product type> unboxing", "<competitor> honest review", "tiktok made me buy it <category>", "<category> haul"; two pages each (1 credit a page), `since: "year"` in a small category. `instagram_search_posts` with hashtags such as ugccreator, ugc plus the category, and the category plus review (1 credit a page). On YouTube, `youtube_search_videos` with "<category> review" and `since: "year"` when the user added it (1 credit a page).
2. **Keep the makers.** Group the rows by `author`. Keep authors with two or more product-style videos in the results, or one strong one (a clear demo with a verdict). Drop brand accounts, resellers and aggregator pages that repost other people's videos.
3. **Profiles.** `tiktok_get_profile` or `instagram_get_profile` on up to 30 authors (1 credit each). Read `followers` against the size band, and the `bio` and `website` for the signs of a working UGC creator: "UGC creator", "content for brands", a rate card, a portfolio link, a business email.
4. **Their recent work.** `tiktok_get_videos` or `instagram_get_reels` on the top 20 (1 credit a page). Count the product-style videos, the `is_ad: true` posts (paid brand work before), and read `duration_s`: 15 to 60 seconds is ad length. Note whether they show their face, talk over hands or only film the product.
5. **Hear them.** `tiktok_get_transcript` or `instagram_get_transcript` on one product video each for the top 10 (1 credit each). A creator who opens with a hook, names the problem, shows the product working and ends with a verdict can make an ad. One who rambles for twenty seconds before the product appears cannot.
6. **Add contacts.** From the bio, then `website`. For a creator's own domain, run the [contact steps](../../link-building/SKILL.md#contact-steps) with `limit: 10`.
7. **Deliver** a table: creator, platform and link, followers, styles they make, best example (URL and views), its hook from the transcript, on camera (face, voice-over, hands only), brand work signs (`is_ad` count, brands named), portfolio or contact, and a one-line note on fit. Sort by craft, not by followers.

## Judgment

- For UGC, views on the creator's own posts are a weak signal. A 3K-follower creator with tight 30-second reviews beats a 200K creator who rambles; judge the transcript and the structure.
- A creator whose own "made me buy it" video drew comments asking where to buy has already shown they can sell. Mark those rows.
- UGC is bought with usage rights: where the video may run (organic, paid ads, product page), for how long, and whether it runs from the creator's handle (whitelisting, Spark Ads). The tools cannot see rights; the [campaign brief](campaign-brief.md) states them.
- Search is ranked and leans recent. Widen `since` and add competitor names when the category is small.
- Faceless formats (hands, voice-over) suit products that do the talking. A face on camera suits trust-heavy categories (skincare, supplements, finance). Ask which the user wants before cutting the list.
- For the ad itself (angles, hooks, what the category's ads already say), the `paid-ads` group's creative brief is the input the UGC creator works from.
