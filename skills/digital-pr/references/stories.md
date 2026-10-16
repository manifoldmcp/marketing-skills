# Stories

No story, no coverage. A journalist wants something their readers need to know, and a link is a by-product. This reference covers the four kinds of story that earn coverage, what each is worth, and the benchmarks behind them. Checked October 2026.

## Benchmarks

The honest state of the evidence: there is no large, independent study of how much coverage or how many links a digital PR campaign earns. What exists:

- **List size against links.** BuzzStream analysed about 31M outreach emails in 86K campaigns from agencies using its tool (updated April 2025). Average reply rate 0.85%: 1.15% for campaigns of 1 to 50 emails, 0.37% for 1,001 to 5,000. Campaigns of 51 to 100 emails earned about 12 links each; those over 1,000 earned under 3.5, about 70% fewer. Links were counted as new referring domains to the pitched URL, so some came from elsewhere, and unlinked mentions were not counted. Correlational, but it is the largest dataset there is, and it says targeting beats volume.
- **What practitioners report.** BuzzStream's State of Digital PR (150+ practitioners, mostly UK and US agencies): in the 2025 edition the weighted average was about 15.6 links per person per month, and about half saw results in three to six months. In 2026, 95.9% pitch data-led content, 75% say the work is harder than a year earlier, getting coverage is the top challenge (26.4%), the most common cost per link is $300 to $500, and about 80% never pay for links. Self-reported, small sample.
- **Single case studies** publish link-to-coverage rates (one agency's cost-of-living index: 34 pieces of coverage, 22 linked). One campaign is not a benchmark; never quote one as an expected rate.

So promise inputs (stories pitched, reporters reached, follow-ups sent) and report outcomes against the user's own baseline, not against an industry rate.

## Data-led stories

The most-pitched tactic, and the one journalists rank among the most valuable (40% value original data or research, Muck Rack 2026; Cision lists original research first).

- **A data story answers a question the reporter's readers already argue about,** with a number nobody had. Find the argument first: rising or falling search demand (`seo_search_keywords` on the category, 10 credits, reading `trend[12]`), and the debates in `reddit_search_posts` with `time_range: "year"` (1 credit a page).
- **Sources of data, best first:** the user's own product or customer data (aggregated, with permission and no personal data); a survey the user runs (state the sample size, who was asked, when, and how, and the margin of error: about ±4.4 points at 500 respondents, ±9.8 at 100, so a small sample cannot carry a headline split by segment); public data recombined (government statistics, public filings, search demand). Public data is citable, so name every source.
- **Publish the method.** A page with the method, the sample, the dates and downloadable charts. Journalists check, and AI answers cite pages that state their numbers plainly (see [AI answers](ai-answers.md)).
- **Localise and segment.** A national figure plus a breakdown by city, industry or age gives each regional or trade reporter their own angle from one dataset.
- **It takes weeks to build and earns links for months.** Pitch it, then re-pitch it to new reporters as the topic comes up in the news.

## Reactive comment and newsjacking

The fastest route, and the one a small team can run every week.

- A news event in the user's field, plus a spokesperson with a specific, quotable view, sent to reporters covering that event within hours. A day late is too late.
- The quote must add something the reporter does not have: a number, a prediction, a contrarian read, first-hand experience. "This is an exciting development" is never used.
- Sourcing services (see [sourcing services](sourcing-services.md)) are the inbound version: reporters post the question, the expert answers.
- Watch for the news: `reddit_get_new_posts` or `reddit_search_posts` with `sort: "new"` on the category; Google News alerts by hand.

## Expert positioning

Being the person reporters call. Slower and compounding.

- One spokesperson per topic, with a short bio, a headshot, two or three topics they can speak on with authority, and proof (data they hold, a track record, a past quote).
- Turn every placement into the next: reporters who quoted the user once are the first list for the next story.
- Podcasts build the same reputation by voice (see [podcasts](podcasts.md)).

## Launch news

Most launches are not news outside trade press, newsletters and podcasts. A launch becomes a story with a new category, real numbers, a known customer, funding or a founder with a story. Without one, say national press is unrealistic.

- **Find the angle that got similar launches covered:** `seo_get_serp` for "<competitor> launches", "<competitor> announces" and "<category> launch" (1 credit each). Keep publication articles, not the vendors' own pages, and note what each one led with.
- **Tier the list and set dates around the embargo:**

| When | Who | What they get |
|---|---|---|
| T-14 | At most one exclusive outlet, the one that matters most to the buyers | The full story first, under embargo |
| T-7 | 5 to 10 reporters whose last article is closest to the story | Embargoed briefing with the lift time and time zone |
| Launch day | Trade press, newsletters, bloggers, podcasts | The news live, with the press kit |
| T+7 | Reporters who did not reply | A second pitch with results: signups, a customer story, what users said |

- **The press kit** is the user's checklist: the release, screenshots, founder photo and bio, pricing, a customer who will talk, a demo. No tool builds it.
- A launch-day pitch to a stranger rarely lands the same day; the T+7 follow-up with numbers is the second chance. After T+30 it is no longer launch press; it becomes a data or expert story. Launch sequencing across channels belongs to [growth-strategy](../../growth-strategy/SKILL.md).

## Press releases

- A release on a newswire is a record of the news, not a pitch. Journalists rank direct pitches, data and sources above it, and Google's link spam policies list keyword-rich links in distributed press releases as a link scheme. Major wires mark links nofollow.
- Use a release for material news a reporter may need to cite (funding, an acquisition, a result) and post it on the user's own newsroom page, where it can be cited by AI answers too. Never use one as a link-building tactic.

## Things to refuse

- Paying for an article presented as editorial, or buying a "guest post" on a news site for its links. Google's site reputation abuse policy (clarified November 2024) covers third-party content placed on a host site to borrow its ranking, whatever the host's involvement.
- Fabricated surveys, invented experts, or data with no method.
- Pitching a story the user cannot back with a spokesperson who will answer the phone.
