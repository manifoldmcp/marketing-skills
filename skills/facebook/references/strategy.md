# A Facebook plan

When the user wants a plan for a page and the groups its buyers are in, rather than a one-off check: whether Facebook fits, where the page stands against its peers, and what to change over 90 days for the hours the team has.

## Does Facebook fit?

- **Who is there (US).** 71% of US adults use Facebook, second only to YouTube: 68% of 18 to 29s, 80% of 30 to 49s, 74% of 50 to 64s, 57% of those 65 and over; 78% of women against 63% of men; the same 71% to 72% at every income level and in urban, suburban and rural areas (Pew Research Center, 5,022 adults, February to June 2025). About half of US adults use it daily, 58% of 30 to 49s. Instagram, by contrast, reaches 80% of 18 to 29s but 19% of over-65s.
- **Who is there (world).** 56.9% of online adults 16 and over used Facebook in the past month, the highest of any platform GWI asked about, and over-65s were the most likely age group to use it (GWI Q2 2025, in DataReportal's Digital 2026 report, October 2025). Use outside the US varies a lot by country; check the market's own figures before assuming the US picture.
- **So the old line "Facebook is for over-50s" is wrong.** Its peak is 30 to 49. It is the channel for buyers who are parents, homeowners, local customers, trades and hobby communities, and for women over 30 more than any other platform. It is a weak channel for under-25s and for B2B buyers searching for software, who are on [linkedin](../../linkedin/SKILL.md).
- **Best fit:** local and small businesses (shops, clinics, studios, trades, venues, garden centres), anything bought by parents or homeowners, hobby and enthusiast categories with active groups, and markets where Facebook is the main network. If the user has not chosen channels yet, that comparison is [growth-strategy](../../growth-strategy/SKILL.md).

## Inputs

Take them from `.agents/product-marketing.md` where it answers; ask the rest in one message and mark defaults as assumed.

- **Goal:** calls and visits, leads, sales or community. Default leads, measured in comments and messages.
- **Page**, and two or three **competitor pages** in the same area or niche (no page search exists; the user names them, or find them with `seo_get_serp` on "<category> <town> facebook").
- **Area or niche:** it decides which groups count.
- **Team:** who can be on camera, who answers comments and messages. Default the owner, filmed on a phone.
- **Hours a week** (default 3), any **paid budget**, and the **credits** for the research (this plan costs about 75).

## Baseline

For the user's page and each competitor, as [page audit](page-audit.md) describes, at about 15 credits a page:

- followers, page category and completeness (phone, address, website), and `running_ads`;
- posts a week, video share, median engagement per 1,000 followers over about 30 posts, and the count of posts at three times the page's median;
- the shortlist's shares and reel views from `facebook_get_post`;
- active ads, with boosted organic posts taken out of the medians.

Then, about 15 credits more: the three best competitor reels transcribed for their first line and offer; one call of comments on the three most-commented competitor posts for questions and objections ([comments](comments.md)); and two or three public groups or Google-ranked threads where the buyers ask for recommendations ([groups](groups.md)). For a local business, the local search check in [local search](local-search.md) too.

## The order of work

1. **Fix what blocks.** A page that recycles other people's content, or shows a recommendation penalty in Professional Dashboard, is held back on everything it posts ([feed](feed.md#original-content-since-july-2025)). Stop that first.
2. **Make the page findable and complete.** Category, hours, phone, address and website: Google shows the page and its recommendations in local results, and AI answers draw local facts from listings rather than from Facebook ([local search](local-search.md)).
3. **Short original video.** Reach beyond followers now comes from recommendations, and video is where Facebook time is growing. One reel a week the team actually makes beats five reposts.
4. **Answer in groups.** For a local business, a useful answer in a town group's "who do you recommend" thread can reach more buyers than a post, and those threads rank in Google. Each group's rules decide whether the business may be named.
5. **Turn questions into posts.** The questions from comments and groups become the next month's posts and the website's FAQ.
6. **Paid last.** Boost or advertise the organic posts that already earned the most, through [paid-ads](../../paid-ads/SKILL.md), once there is budget.

## The 90 days

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | Page completed; reposting stopped; a cadence the team can hold (default three posts a week, one a reel); one helpful answer a day in two or three groups where rules allow | Posts a week; video share; followers (`facebook_get_profile`) |
| Days 31 to 60 | Keep the format with the best median per 1,000 followers; turn the top comment and group questions into posts; a weekly series (a tip, a behind-the-scenes, a customer story) | Median engagement per 1,000 followers against the day-0 baseline and against competitors |
| Days 61 to 90 | Repeat what produced outliers; a boost or ad test on the best organic post if there is budget; re-run the baseline | Count of posts at 3x the median; reel views on the shortlist; active ads |

Give each period an owner and three first actions for this week. Messages, calls, reach, link clicks and Professional Dashboard data are not visible to the tools; the user reads them in Meta Business Suite, and for a local business those numbers matter more than reactions.

## Setting targets honestly

- Promise inputs (cadence, original video, groups answered, a complete page), not reach. Recommendations decide reach and Meta changes them often.
- Benchmarks are averages across very different pages: 0.15% engagement by followers across 130,683 business pages in 2025 (Socialinsider). Judge the page against its own baseline and its direct competitors over the same window, never against an industry figure.
- With about 30 posts per page in the baseline, a move in median engagement needs to hold for two re-measures before it is a change.
- Re-check the plan after any Meta policy change on links, original content or Reels ranking.
