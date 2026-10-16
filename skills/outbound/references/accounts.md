# Account research

One company researched before a sales call: what it is, who sits on the buying committee, what the company and those people said lately, what it pays to advertise, and how it shows in search. It ends in a one-page brief with the evidence linked and three questions for the call. A prospect researched for an agency pitch is [agency](../../agency/SKILL.md); a rival is [competitor-intel](../../competitor-intel/SKILL.md).

## What to pull, and what it costs

About 22 credits for a default brief.

| Part | Tool | Cost | Read |
|---|---|---|---|
| Company | `leads_get_company` on the domain | 1 | `description`, `industry`, `employees`, `location`, `founded_year`, `keywords[]`, the LinkedIn page as `linkedin_url` |
| Its LinkedIn page | `linkedin_get_company` | 1 | `followers`, `bio`, LinkedIn's own `employees` count |
| Buying committee | `leads_search_people` with `company_domains` set to the domain and the full titles around the deal | 4 | The economic buyer, the champion, the user, and whoever signs off on security or procurement |
| A person the search missed | `leads_get_person` by profile URL, or name and domain | 3, 1 on a miss | `headline`, `title_start`, `employment_history[]` |
| What they said lately | `linkedin_get_company_posts`; `linkedin_get_profile` and `linkedin_search_posts` for each person on the call; `twitter_get_tweets` if they post on X | 1 each | Launches, funding, new markets and hires in the last 90 days |
| Funding | `seo_get_serp` with `"<company>" raises` as the keyword | 1 | A dated news result names the round |
| Ads | `ads_get_advertiser_ads` on Facebook with `active_only: true`, LinkedIn by company name, Google by domain | 1 each | What it pays to promote is what it is trying to grow this quarter |
| Search footprint | `seo_get_domain_overview` | 5 | One line, unless the user sells marketing or search |

On `NoData` from `leads_get_company`, the provider does not know the domain: take what the company does from `seo_get_page` on the home page (free) and ask the user for the LinkedIn page. If Google ads find nothing by domain, `ads_search_advertisers` gives the advertiser id to try.

## Judgement

- **Check the current role before the call.** The search row, the person record and the live profile agree most of the time; when they disagree, trust the profile and say so. Headlines lag titles.
- **Date anything that decides the pitch.** A record can be a month old and a headcount band a quarter behind.
- **The data has no tech stack, and revenue is an estimated range** (`revenue_min` to `revenue_max`) unless the company discloses it. Ask about both on the call rather than guess.
- **LinkedIn dates are approximate.** Say "recently" when the age is in weeks.
- **Stay on work.** A brief that mentions someone's family, health or private accounts does harm on the call; leave it out even when it is public.
- **Under about 500 target accounts, or deals large enough** that each call deserves prep, brief every account that replies; above that, brief only the ones that book.

## The brief

- A snapshot: size, location, what they do in one sentence, the latest round if one was found.
- A committee table: name, title, role in the deal, background, LinkedIn URL.
- Recent signals with dates and links: posts, the round, active ads.
- The search footprint in one line.
- Three questions for the call and one angle, each tied to a piece of evidence above.

## Without the connector

The same brief by hand: the company's site, LinkedIn page and recent posts, a news search for the company name, the public ad libraries (Meta's, LinkedIn's, Google's Ads Transparency Center), and each attendee's LinkedIn profile. Link every claim.
