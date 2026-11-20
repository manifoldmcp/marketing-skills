# Demand signals

Whether people want X, read from public signals, and what each signal can and cannot prove. No public signal proves anyone will pay; that takes a test (see [validation](validation.md)). Checked October 2026.

## Search volume

Search volume proves that people type the words into Google. It is the best-measured demand signal there is, and it is still an estimate with known biases.

- **It is a bucket, not a count.** Keyword Planner reports volume on a fixed scale of roughly 60 to 80 logarithmic values (10, 20, 30 ... 90, 110, 140, 170, 210, 260, 320, 390, 480, 590, 720, 880, 1,000, 1,300, 1,600 ... 135,000, 165,000, 201,000 ...); a figure of 1,000 can stand for anything from about 720 to 1,600. Accounts with little ad spend see ranges ("1K to 10K"). DataForSEO, which Manifold uses, says its Labs volumes start from Google Ads data, and in our pull they were the same buckets (see [field notes](field-notes.md)).
- **It is a 12-month mean.** The headline `volume` averages the last twelve months, so a seasonal product, a news spike or an artefact sets it. Read `trend[12]`: compare the last three months with the same three a year earlier where the product is seasonal, and treat a one-month spike as news, not demand.
- **Generic English software terms jumped in 2026.** In Google's own Keyword Planner, "crm software", "project management software" and "email marketing software" rose 4x to 100x between March and May 2026 and again in September, in the US and in Germany, while long-tail and local-language terms did not ([field notes](field-notes.md)). The cause is unknown. On a head term with a step change, size from the months before it or from the median month and say so.
- **Close variants are grouped.** Since 2016 Keyword Planner has given plurals, misspellings, stems and spacing variants one shared figure. Summing "crm software" and "crm softwares" counts the same searches twice. Sum only keywords that are different searches, and say the total is an upper bound.
- **It overstates against what sites see.** Ahrefs compared Keyword Planner with Search Console impressions on 72,635 keywords and found 54% of the Keyword Planner figures more than 50% too high (2021, a vendor study; impressions are not a perfect benchmark either). Semrush's 2022 study by a similar method found Semrush closest. Every vendor's accuracy study favours the vendor.
- **Small numbers are noise.** Under about 50 a month the bucket is wider than the number. `null` means the provider has no data, never zero: do not add it in as 0. A new category with no agreed name has no volume and can still be an early market (see Reddit and video below). In our pull Keyword Planner had no data at all for "generative engine optimization", which Labs put at 4,400.
- **Search Console is the only ground truth, and only for the site's own queries.** Impressions for a keyword the site ranks in the top three for come closest to real volume. Desktop impressions fell sharply from about 10 September 2025, when Google stopped serving 100 results a page and rank-tracker bots stopped generating impressions (widely reported; Google never confirmed). Compare impressions only within one side of that date. Search Console's own traps belong to [seo](../../seo/SKILL.md).
- **`cpc` and the bid range prove a market and competition at once.** Advertisers pay for the click because it earns. A high `cpc` on a thin volume is a valuable niche; zero `cpc` on a big volume is curiosity.
- **Intent decides what the volume means.** Volume on "how to" and "what is" phrases is interest; on "best X", "X pricing", "X alternative" and "X for <buyer>" it is buying. Report the share of volume on commercial and transactional `intent`, not just the total.

## Google Trends

Trends shows direction and seasonality, never volume, and it disagrees with itself.

- **It is a sample, rescaled on every request.** Each query returns a random sample of searches, indexed so the peak of the window is 100. The same query on another day, or downloaded from another country, returns different values, most for low-volume queries (Hölzl, Keusch and Collins, GOR conference, April 2025). Behnen and colleagues found the inconsistencies systematic, and largest on windows of eight months or less (2020).
- **What to do by hand:** compare terms in one request (up to five), add an anchor term of known volume, use windows of a year or more, pull the same comparison on two or three days and average, and log the date and region. Never compare two separate downloads.
- **The Trends API** (alpha since July 2025, still alpha in October 2026) returns consistently scaled values over a rolling five years, so separate pulls can be joined. Access is by application only. No Manifold tool reads Trends.
- 100 on Trends means "the most this term was searched in this window", not "a lot". A niche B2B term can sit at 100 on a few hundred searches a month; read Keyword Planner volume beside it.

## AI search volume

No one outside the AI companies measures what people ask assistants. Every "AI search volume" is a model, and the models disagree.

- **What each vendor measures.** DataForSEO, behind Manifold's `ai_volume` and `ai_search_volume`, builds it from questions in Google's People Also Ask boxes. Ahrefs (since August 2026) takes the Google volume of the prompt's parent keyword and multiplies it by each assistant's share of referral traffic against Google. Profound extrapolates from opt-in consumer panels; Ahrefs says some such figures are "significantly over-estimated". No vendor has published a validation against real prompt logs.
- **Only Microsoft shares query data.** Bing Webmaster Tools' AI Performance report (public preview since February 2026) lists the grounding queries Copilot and Bing's AI answers used when they cited the site. It is first-party, free, for one engine family, and only for queries where the site was a source.
- **In our pull it bore no relation to search volume** (0.07% to 77% of Google volume across twenty keywords) and fell through early 2026 on head terms while ChatGPT use grew ([field notes](field-notes.md)). Use `ai_volume` to rank prompts against each other within one pull, never to size a market or to say how many people ask ChatGPT.
- **The scale, for context.** ChatGPT handled about 2.5 billion prompts a day in July 2025 (OpenAI, via Axios); Google handles more than 5 trillion searches a year, about 14 billion a day (Google, March 2025). About a quarter of ChatGPT messages seek information (24% in July 2025, up from 14% a year earlier, consumer plans only; OpenAI and Harvard economists, NBER working paper 34255, September 2025). AI platforms sent about 0.3% of all website visits in 2026 (SE Ranking, vendor study), and Google still held about 90% of worldwide search (StatCounter, 2026). Search volume is still the larger, better-measured demand signal; it undercounts categories buyers research in assistants (software comparisons, how-to, B2B shortlists).
- **The better AI read is qualitative.** `aeo_search_prompts` shows which recommendation prompts exist ("best tool for...", "how do I...") and `cited_domains[]` shows who answers them now; `aeo_run_ai_answers` shows which brands the engines already name. That says whether buyers ask and who wins them, not how many ask. Measuring AI visibility itself belongs to [ai-search](../../ai-search/SKILL.md).

## Conversation and attention

- **Reddit** shows the problem in buyers' own words and who asks for help. The strongest public intent signal is a post asking for a solution ("is there a tool", "looking for", "would pay for"). Count on-topic posts in the past month and year and their `comments`. Search is ranked and never complete; one heated thread is not a market. Reddit's own traps belong to [reddit](../../reddit/SKILL.md).
- **TikTok and YouTube** views measure attention, not intent: the weakest proof of buying and the best early read on awareness. Creators reviewing products means a market exists; creators explaining the problem with no product to name means awareness without a product.
- **Ads running now.** Advertisers paying for the category now are proof someone earns on it. An ad still running 90 or more days after its `first_shown` is paying for itself. Many advertisers and a high `cpc` mean a crowded market, not an empty one. Reading ad libraries belongs to [paid-ads](../../paid-ads/SKILL.md).

## The verdict

Weigh the signals, then give one of four verdicts with its confidence (high, medium, low) from how many signals agree:

| Verdict | Pattern |
|---|---|
| Strong | Commercial search volume with a flat or rising trend, recommendation prompts, Reddit posts asking for a tool |
| Emerging | Little volume but a rising trend, lively Reddit or video talk, few products |
| Crowded | Strong demand, high `cpc`, many products reviewed on video, many advertisers running for months |
| Weak | None of these |

Deliver a table (signal, tool, the number, what it proves, what it cannot), the verdict, and the one test that would settle what the signals cannot (see [validation](validation.md)). The user runs the test.

## Without the connector

Keyword Planner is free with a Google Ads account (ranges unless the account spends); Google Trends is free; Reddit, YouTube and TikTok search work by hand; Meta's and LinkedIn's ad libraries are public. Log the date and region of every reading and say that hand counts are samples.
