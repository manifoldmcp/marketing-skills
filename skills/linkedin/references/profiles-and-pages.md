# Profiles and company pages

Whose voice should carry the account, how to audit a company page (the user's or a competitor's), and where newsletters and articles fit. Checked October 2026.

## People first, the page second

- **Organic company-page posts are about 2% of what members see**; top creators' posts about 31%, promoted company content 28%, ads 11% (van der Blom, as reported by AuthoredUp, 2025). Text posts from pages reach 0.42 times a page's average, against 0.88 for people (AuthoredUp, 2025).
- **AI engines cite people more than pages:** 75% of LinkedIn citations in AI answers point to members' content, 25% to company pages (Meltwater with LinkedIn, 9.5M citations, August 2026; sponsored, no method published).
- **So for an early B2B company, lead with the founder.** The page reshares, covers product news, hiring and events, and gives employees something to share. A page with modest numbers whose people post about the company is healthier than a page posting alone.
- **More voices beat one loud one.** Each employee who posts inside the company's topic reaches a different slice of the interest graph. LinkedIn added collaborative posts (June 2026) for co-authored posts; Thought Leader ads, which pay to boost an employee's post, belong to [paid-ads](../../paid-ads/SKILL.md).
- **When the page leads:** brands whose buyers follow companies (enterprise software, regulated industries, recruiting) or with no person willing to post. Even then, documents and images outperform text and links on pages (see [formats](formats.md)).

## Auditing a company page

For the user's page the output is fixes; for a competitor's it is what to learn and what to avoid. Default: the page's last 20 posts and 10 on each of two peers, about 50 credits.

1. **The page.** `linkedin_get_company`: `bio` (does the first line say who it serves and what it does?), `website` set, `industry` the one buyers expect, `employees`, `followers`, `topics`. Logo, banner and button are not returned; list them for the user to check by eye.
2. **The feed.** `linkedin_get_company_posts`, two or three pages for the last 20 posts: posts a week over 90 days (from `created_at`, reliable at that scale), and the topic of each (product, customer story, thought leadership, hiring and culture, events, reshared press), its length and hook, whether it carries a link.
3. **Engagement.** `linkedin_get_post` on each of the 20, since the feed has none: median likes and comments, top five and bottom five. What the top five share (a topic, a person shown, a number in the first line) is the page's best evidence; six outliers mean a repeatable topic, one means luck.
4. **Peers.** The same reads on two peers over the same window: `linkedin_get_company`, one page of posts, `linkedin_get_post` on their last 10.
5. **Who else talks about the company.** `linkedin_search_posts` with the company name and `since: "month"`, two pages: employees, customers and partners posting about it. Their reach is usually the page's real reach.

Deliver a scorecard: bio, website, industry, employees, followers, posts a week, median likes, median comments, likes per 1,000 followers, topic mix, best topic, mentions by others last month, each for this page and each peer, with a verdict per row. Then the top five posts (URL, first line, likes, comments) and five fixes or five lessons.

- Judge a post against its own page's median, never against LinkedIn at large; compare pages per 1,000 followers and over the same window.
- Hiring and culture posts often earn the most likes, from employees and candidates rather than buyers. Separate them when judging what works for pipeline.
- A rival whose cadence fell in the last month may have cut effort: say so, it is an opening. A weekly watch of rivals is the host's schedule (see [competitor-intel](../../competitor-intel/SKILL.md)).

## Auditing a person's profile

`linkedin_get_profile` gives `followers`, the start of the `bio`, `location`, companies in `experience` and `recent_posts`; the headline is not returned, so ask the user for it or read it by eye. For the posts, `linkedin_get_post` on the five or six in `recent_posts` and on five more URLs the user gives, for a baseline of 10: median likes and comments, per 1,000 followers, and posts a week. With no URLs, search the person's two main topics and keep rows whose `author` is theirs; mark the baseline partial. The user's own analytics add what the tools cannot: impressions, the in-network and out-of-network split, and who engaged.

## Newsletters and articles

- **Feed reach is low, the job is different.** Standalone articles get about 0.4 times a normal post's reach (van der Blom, 2026). A LinkedIn newsletter sends each edition to its subscribers by push, in-app and email notification (LinkedIn Help), so it reaches people the feed would not.
- **One creator's numbers, not a study:** van der Blom says about 60% of his followers subscribe to his newsletter and about 80% of his LinkedIn-sourced conversions come from it (his own account, June 2026). Treat as a case, not a benchmark.
- **They are what AI engines cite.** LinkedIn's own guide (August 2026) says long-form articles earn more AI citations than feed posts; the most-cited LinkedIn articles were how-tos, how-to-choose guides, comparisons, ranked lists and buyer explainers, all with lists, 92% with clear subheads, 67% with statistics (Meltwater with LinkedIn, sponsored). LinkedIn's VP of Marketing suggests 800 to 1,200 words. See [AI answers](ai-answers.md).
- **When to start one:** when the person already posts steadily on one topic and has something to say at length every week or two. A newsletter with three editions and silence does more harm than none.
- The tools do not read newsletters or their subscriber counts; the user's own analytics do.
