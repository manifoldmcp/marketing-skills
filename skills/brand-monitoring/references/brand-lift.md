# Brand lift without a panel

Whether a campaign, launch or sponsorship raised awareness, when there is no survey panel and no platform lift study. The short answer: hold some places back, and measure branded search and direct traffic in both. Checked October 2026.

## What the platforms offer, and what they need

- **Google Ads Brand Lift** surveys exposed and held-out users on ad recall, awareness, consideration and brand association. It is not offered on every account: it needs the account rep and a minimum spend reached within a 10-day window (Google Ads help). Most small advertisers cannot get it.
- **Meta Brand Lift and Conversion Lift** are people-based tests run through Meta, whose eligibility Meta sets per account. Meta's own guidance prefers them to geo tests where available, because they have more statistical power.
- **Both measure only the platform's own ads.** A podcast sponsorship, a conference, a PR push or organic social has no lift study; a geo test or a matched comparison is the only route.

## Geo holdout

Run the activity in some regions, hold it back in comparable ones, and compare the outcome. Meta's open-source GeoLift documents the requirements (GeoLift best practices, 2025):

- Daily data strongly preferred over weekly.
- At least 20 geo units and 25 pre-treatment periods; historical data four to five times the test length, with no structural breaks; a year of history to cover seasonality.
- The test lasts at least one purchase cycle, and at least 15 days on daily data or 4 to 6 weeks on weekly.
- Test and control matched on the same outcome; local and national media held constant across both during the test.

What it needs from the user is a lever that can be switched off by region (paid media, local events, out-of-home, regional creators) and an outcome measured by region.

## Outcomes to measure by region

- **Branded impressions from Search Console** are free and daily, but `console_get_search_analytics` groups by country only. For a multi-country brand, countries are the geo units; within one country it does not work.
- **Google Analytics** reports sessions, new users and key events by region or city (list the dimensions with `analytics_get_fields`, then `analytics_get_report`, free). Direct and branded-organic sessions by region are the closest proxy for awareness the user owns.
- **Google Trends** gives relative interest by subregion and metro, free, weekly or daily; noisy for small brands.
- **Sign-ups or sales by region** from the user's own systems, where awareness is expected to convert inside the test.

`seo_get_keyword_metrics` takes a `location` but returns monthly buckets with a lag, which is too coarse for a test of weeks.

## When a holdout is impossible

- **Before and after with a control series.** Compare the brand's branded search or direct traffic with a series the activity cannot have touched (a competitor's branded interest in Trends, the user's own traffic in a country where nothing ran), over the same weeks. A synthetic control such as CausalImpact formalises this; say it is weaker than a holdout.
- **Staggered starts.** Launch in one market first, then another two weeks later; each start is a small natural experiment.
- **Ask.** A "how did you hear about us" field on sign-up, with podcasts, creators, AI assistants and events as options, catches what analytics labels direct. Self-report is biased toward the memorable, but it is the only source that sees an AI answer or a podcast mention.

## Reading the result honestly

- A one-off before-and-after on one market cannot separate the campaign from season, news or a competitor's move. Say so.
- Branded search lift shows within days for a big burst and months for slow work; report the window measured.
- AI assistants now send people to search the brand by name (Similarweb, June 2026), so a rise in branded search during a campaign can also be AI answers changing. Check [ai-search](../../ai-search/SKILL.md) share for the same weeks before crediting the campaign.
