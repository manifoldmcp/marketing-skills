# Account brief

One company, researched before a sales call: what it is, who sits on the buying committee, what the company and those people said lately, what it is paying to advertise, and how it shows up in search. It ends in a one-page brief with the evidence linked and three questions for the call.

## Inputs to settle first

- **Account**: the company domain.
- **People on the call**: names and titles, or LinkedIn URLs. Default: the likely buyer found in step 2.
- **The call**: discovery, demo or negotiation, and what the user sells. It decides which facts matter.
- **Technologies**: competitors or complements whose presence in the stack the user wants to know about.
- **Budget**: a default brief costs about 10 + 1 + 1 + 1 + 3 x 10 + 3 x 1 + 3 x 1 + 3 + 5 = 57 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Company.** `leads_get_company` on the domain (10 credits): `description`, `industry`, `employees`, `location`, `founded_year`, `revenue`, `funding_stage`, `total_funding`, `technologies[]` and the LinkedIn page URL. `linkedin_get_company` with that `url` (1 credit) adds LinkedIn's headcount and `followers`. Note any competitor or complement in `technologies[]`.
2. **Buying committee.** `leads_search_people` with `company_domains` set to the domain and the titles around the deal: the economic buyer, the likely champion, the user of the product, and whoever signs off on security or procurement (1 credit). `leads_get_person` on the people on the call and up to one more (10 credits each): `title`, `headline`, `location`, the LinkedIn URL, and `employment_history` for tenure and where they came from. A former employer that was the user's customer, or a competitor's, is worth a line.
3. **What they said lately.** `linkedin_get_company_posts` with the company `url` (1 credit a page): launches, funding, new markets, hires and events in the last 90 days. For each person on the call, `linkedin_get_profile` (1 credit) for their `bio`, and `linkedin_search_posts` with their full name as `query` and `since: "month"` (1 credit), keeping only rows whose `author` is their profile handle. No tool lists one person's feed, so this finds some of their posts, not all. If they post on X, `twitter_get_tweets` with their handle (1 credit).
4. **Ads.** `ads_get_advertiser_ads` with `active_only: true` three times (1 credit a page each): `platform: "linkedin"` and `platform: "facebook"` with the company name as `advertiser`, and `platform: "google"` with the domain. Only Facebook applies `active_only`; on the other libraries keep the rows whose `active` is true or whose `last_shown` is recent. If the Google call finds nothing, `ads_search_advertisers` with the brand (1 credit) gives the advertiser id to try instead. What a company pays to promote is what it is trying to grow this quarter.
5. **Search footprint.** `seo_get_domain_overview` on the domain (5 credits): `organic_traffic`, `organic_keywords`, `domain_rank` and `top_pages[]`. Keep it to one line unless the user sells marketing or search, in which case `seo_get_ranked_keywords` with `limit: 100` (10 credits) shows what they rank for.
6. **Deliver** a one-page brief:
   - A company snapshot: size, funding, location, what they do in one sentence, stack items that matter.
   - A committee table: name, title, role in the deal, tenure, background, LinkedIn URL.
   - Recent signals with dates and links: posts, announcements, active ads.
   - Search footprint in one line.
   - Three questions for the call and one angle, each tied to a piece of evidence above.

## Judgment

- Check the current role before the call. `employment_history` with `current: true` and a recent LinkedIn profile agree most of the time; when they disagree, trust the profile and say so.
- Records lag. A company record can be a month old and a headcount band a quarter behind. Date anything that decides the pitch.
- `technologies[]` is detected from outside: say "their site shows X", never "they use X", and confirm it on the call.
- Stay on work. A brief that mentions someone's family, health or private accounts does harm on the call; leave that out even when it is public.
- LinkedIn post dates are approximate ("3 weeks ago"). Say "recently", not a date, when the age is in weeks.
- A brief on a prospect client for an agency pitch, centred on their marketing, is the [agency prospect audit](../../agency/references/prospect-audit.md). A competitor researched as a rival is the [competitors](../../competitors/SKILL.md) group.
