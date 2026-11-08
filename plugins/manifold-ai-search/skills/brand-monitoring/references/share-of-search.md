# Share of search and branded search

The cleanest awareness measure there is: nobody searches a name they do not know. What it predicts, where it misleads, and how to read Manifold's keyword data for it. Checked October 2026.

## What the evidence says

- **The origin.** Les Binet defined share of search as a brand's organic Google searches over all brands' searches in its category, and showed it tracked and led market share in cars, energy and mobile phones (IPA EffWorks, 14 October 2020). Lead times reported at the time: up to about a year for cars, nought to three months for energy (Campaign, October 2020). Binet called it "by no means a silver bullet": price moves conversion, and not every search is a positive one.
- **The validation.** The IPA's think tank, led by James Hankins, found share of search averaged 83% of share of market across 30 case studies, 12 categories and 7 countries, with the ratio varying by category and country, and stressed these are correlations, not causes (IPA, 12 October 2021). It recommends a 6 to 12 month rolling average. When share of voice runs above market share, share of search tends to rise; brands that go dark decline on a half-life tied to purchase frequency.
- **Extra share of search.** The gap between share of search and share of market (ESOS) is the predictive signal: a brand searched above its market share tends to grow. GfK tested this on hundreds of technology brands and reported it predictive once the method was refined (Sawtooth Analytics and Insights Summit, 2024; no figures published in the abstract).
- **The name list matters.** Kantar expanded five brands' names into about 30,000 related keywords (models, products, misspellings) and found 36% more search interest than the names alone (August 2025). A brand-name-only share undercounts brands whose products are searched without the brand ("RAV4", not "Toyota RAV4").
- **"83% accurate" is a misreading.** The 83% is an average ratio of share of search to share of market, not a forecast hit rate.

## Why it works and when it does not

It works where buyers search a brand before buying and the category's brands all get searched: cars, energy, phones, consumer software. It misleads when:

- **The name is a common word.** "linear" is 90,500 searches a month and "linear app" 6,600; "folk" is 60,500 and "folk crm" 1,900; "manifold" is 40,500 and almost none of it is the software. Use the qualified form for every brand in the set, and check with `seo_get_serp` on the bare name (1 credit) whether page one is about the brand. Qualifying drops some real searches; say so.
- **The brand is too small.** Keyword tools return null below their floor: "manifold mcp" was null in both sources while Search Console showed 21 impressions in 90 days. For a small brand, track its own branded impressions in Search Console and leave competitor share until the brand clears the floor.
- **Searches are negative.** A recall, outage or scandal spikes branded search (in Kantar's Tesla example, both the Cybertruck launch and recall coverage spiked share). Read a spike with the mentions behind it ([monitoring](monitoring.md)).
- **Price, not salience, is moving share.** A discounter can gain market share without gaining search; share of search reads salience.
- **AI answers sit in between.** People told about a brand by ChatGPT often search for it next (55.9% of the visits Similarweb traced, June 2026), so branded search now also reflects AI visibility. A brand that gains AI share of voice should see branded search follow; one that does not should check whether its AI mentions are on prompts buyers ask ([ai-search](../../ai-search/SKILL.md)).

## Reading Manifold's keyword data

- **`seo_get_keyword_metrics`** with the brand and competitor names returns `volume` and `trend[12]`. Volumes are Google Ads buckets (1,600, 1,900, 2,400, 2,900, ... 40,500, 49,500, 60,500, 74,000, 90,500, 110,000), the same with the default source and with `source: "google_ads"` in our run. A one-bucket move is 20% to 25%; nothing smaller can show. Months that sit on the same bucket are not evidence of a flat brand.
- **Lag.** On 15 October 2026 the default source's trend ended in August and the Keyword Planner source's in September. The Keyword Planner source costs 24 credits flat against about 11 for a dozen names, and returned null for one brand the default source had ("peec ai"). Use the default; pay for the extra month only when the latest month decides something.
- **`ai_volume`** is how often AI engines see the name, from an index built on People Also Ask style questions, not chatbot logs. Its whole index drifted: every established brand fell 35% to 40% from October 2025 to August 2026 (HubSpot 15,821 to 9,776, Notion 25,388 to 15,608) while fast-growing brands rose (Attio 71 to 217). Read it only as a share of the set, never as an absolute trend.
- **Share, not volume.** Compute each brand's share of the set's total per month, average over three months for a report and six to twelve for a trend, and compare the user's share with its market share where known (the ESOS gap).
- **Search Console for the user's own brand.** `console_get_search_analytics` with a query filter (`includingRegex` on the brand and its misspellings) gives exact daily impressions for queries where the site appeared, free, with about two days' lag. It undercounts: Google drops rare queries for privacy, and only searches where the site showed are counted. It cannot measure competitors. In our run one of the impressions was an AI agent's search (a query with a dozen `-site:` exclusions), so branded impressions now include some machine traffic.
- **Google Trends** gives weekly or daily relative interest and subregions, free, for up to five terms at a time; it adds random noise to protect privacy and shows low-volume terms as zero (Google's Trends FAQ). For more terms, chain comparisons through one anchor term. The Trends API (alpha since July 2025, testers only) returns consistently scaled data across requests.

## Without the connector

Google Trends by hand: compare the brand with up to four competitors in the right country and category, over five years, weekly. Export, compute each brand's share of the summed interest per week, and smooth over 12 weeks at least. Use the same terms every quarter, write them down, and say that Trends is sampled and noisy for small brands.
