# Facebook page audit

A read on one Facebook page, the user's own or a competitor's: how big it is, how often it posts, which formats and topics earn reactions and comments, and what it pays to run. With competitor pages beside it, the audit becomes a benchmark. It ends in a scorecard per page, the posts that worked, and three recommendations tied to the numbers.

## Inputs to settle first

- **Pages**: the page to audit, by name or URL, and up to three competitor pages to compare. Check each is the brand's real page in step 1; brands often have regional and fan pages.
- **Window**: default the last 90 days, capped at 5 pages of posts for a page that posts often.
- **What the page is for**: community, traffic to the site, sales or support. It decides which number matters most: comments for community, link posts for traffic.
- **Budget**: about 1 + 5 + 3 + 1 = 10 credits a page (the record, five pages of posts, three posts in full, one page of ads), so about 40 for the user's page and three competitors. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the page.** `facebook_get_profile` with `handle` or `url` (1 credit, cached 24 hours): `followers`, `likes`, `verified`, `bio`, `website`, `industry`, `created_at`. If `website` is not the brand's domain and the page is not `verified`, ask the user before going on: it may be a fan or regional page.
2. **Pull the posts.** `facebook_get_posts` with the same `handle` or `url` (1 credit per page of results), paging with `meta.cursor` until `created_at` passes 90 days back or 5 pages are read. For each post keep `created_at`, `media` (video, image, text), `duration_s`, `likes`, `comments`, `views`, `is_ad` and the first line of `text`. `shares` is null on every page post; leave it out.
3. **Score it.** Posts per week; the format mix (share of video, image and text posts); the median of `likes` plus `comments` per post and per 1,000 followers; the median `views` on videos. Then rank posts by engagement against the page's own median, and read the top ten and bottom ten: topic, first line, format, length, a link or not, a question or not. Keep `is_ad` posts apart; paid reach is not organic interest.
4. **Read the best posts in full.** `facebook_get_post` on the top three where the text was cut or a number looks off (1 credit each). What people said under them is [comment mining](comment-mining.md); what the top videos say is [video transcripts](video-transcripts.md). Offer those rather than run them.
5. **Read the ads.** `ads_get_advertiser_ads` with `platform: "facebook"`, `advertiser` set to the page name or page id, and `active_only: true` (1 credit per page of results). Count the active ads, their formats and offers, and the oldest `first_shown` among them: an ad that has run for months is one that pays. A teardown of the ads themselves is the `paid-ads` group's [competitor ads](../../paid-ads/references/competitor-ads.md).
6. **Compare.** Run steps 1 to 5 for each competitor page, and set the pages side by side per 1,000 followers.
7. **Deliver** a scorecard table with one row per page: followers, posts per week, format mix, median engagement per post, median engagement per 1,000 followers, median video views, active ads, and the page's best format. Then a table of the top posts: page, date, format, first line, likes, comments, views, and why it worked in one line. End with three recommendations, each tied to a number in the tables.

## Judgment

- Followers pile up over years and include people who never see a post. Engagement per post is the live number; judge the page on it.
- A page with 200,000 followers and 15 reactions a post is not a working channel, whatever its size. Say so plainly.
- Posting more is not the answer when engagement per post is falling. Compare the last 30 days with the 60 before them before recommending cadence.
- The tools see what a visitor sees: no reach, no clicks, no audience demographics. For the user's own page those sit in Meta Business Suite; ask for an export if they matter.
- If the competitor's page is quiet but its ads are many, its Facebook presence is paid. Say so; the organic comparison then says little.
- The same brand's Instagram account is the `instagram` group's [competitor accounts](../../instagram/references/competitor-accounts.md).
