# Brand bidding

Who pays to show when someone searches the user's brand on Google, what those ads say, whether to bid on the user's own name, and what trademark rules allow. It ends in a verdict per brand term; buying the brand campaign or filing a complaint stays with the user. Checked October 2026.

## The rules

- **Bidding on a competitor's trademark as a keyword is allowed in Google Ads.** Google's trademark policy restricts trademarks in the ad text, not as keywords. A trademark owner can file a complaint, and Google may then restrict that term in the text of ads from the named advertisers (resellers and informational sites selling the owner's products are allowed). Ads are restricted, not suspended at first; Google warns at least seven days before any suspension.
- **The courts agree, broadly.** In the EU, the Court of Justice held in Google France v Louis Vuitton (2010) that buying a rival's mark as a keyword is lawful unless the ad makes it hard to tell whose goods are offered; Interflora v Marks & Spencer (2011) applied the same test to the advertiser. In the US, keyword bidding alone is generally not infringement; the risk is ad text that confuses.
- **Agreeing with a competitor not to bid on each other's names carries antitrust risk.** The FTC called 1-800 Contacts' bidding agreements anticompetitive in 2018; the Second Circuit overturned that in 2021 on the facts, but the case is why counsel review such agreements. Never suggest one without the user's lawyer.
- **Affiliates bidding on the brand** often break the user's affiliate terms. That is a contract matter between the user and the affiliate, not a Google policy.

## Is a brand campaign worth paying for?

- **When nobody else bids, often not.** eBay's large field experiment found brand-keyword ads had no measurable short-term benefit: the organic result took the clicks anyway (Blake, Nosko and Tadelis, Econometrica 2015). A later study on Bing found the own-brand ad matters mainly when competitors bid on the name: 1% to 4% more clicks with no rival present, while a brand that did not bid against rivals lost 18% to 42% of its clicks (Simonov, Nosko and Rao, Marketing Science 2018).
- **When a competitor bids, usually yes.** The brand ad is cheap (the user's ad is the most relevant on the page, so its quality score is high) and keeps the clicks a conquester would take. Its value is the clicks it keeps, not the clicks it adds.
- **Settle it with a test, not an opinion.** Where no competitor bids, the user can pause the brand campaign for two to four weeks in some regions and compare total brand clicks (paid plus organic) against regions left on. Ask for the result.
- **AI Overviews change the page.** Brand queries often carry an AI Overview and a knowledge panel now (in our October 2026 fetch, "pipedrive" did); the ad slot above them is the first thing a searcher sees.

## Finding who bids

1. **Price the brand terms.** `seo_get_keyword_metrics` on the name and its buying variants ("<brand> pricing", "<brand> reviews", "<brand> alternative", "<brand> vs"), about 6 credits. A `cpc` and a bid range on the bare name mean advertisers bid on it. "<Brand> alternative" terms often price far above the name itself (in our data, "hubspot alternative" at a $63 top-of-page bid against a 170-search volume).
2. **The user's Auction Insights is the ground truth.** If the user already runs a brand campaign, Google Ads' Auction Insights report names every domain in the same auctions with its impression share. Ask for it first; nothing outside the account sees the auction.
3. **Do not rely on result pages for ads.** `seo_get_serp` can return `type: "paid"` rows, but in six fetches in October 2026 it returned none, including on terms where advertisers certainly bid ([field notes](field-notes.md)). Use it for the organic picture only: the user's own `organic_rank`, competitors' "vs" and "alternative" pages, review sites and resellers on the brand page. Fetch on `device: "mobile"` too. In our "pipedrive" fetch the mobile page did not show the brand's own site in the top results, which may be the fetch rather than Google: check by hand before acting on it.
4. **Read the likely bidders' ads.** For each competitor, reseller or affiliate the user suspects, `ads_get_advertiser_ads` with `platform: "google"` and `country`, then `ads_get_ad` on the three longest-running text ads: keep those whose `headline` or `body` names the user's brand or says "alternative" (see [ad libraries](ad-libraries.md#google)). A conquesting ad running 90 days or more pays for the bidder. `seo_get_page` on its `destination_url` shows whether it lands on a comparison page.

## Verdict per term

- **Defend** (bid) when a competitor's ad names the brand or shows in Auction Insights, or the user's organic result is not first.
- **Watch** when nothing was seen once: ads rotate by auction, place, device and hour, and one look is not a clean term.
- **Leave** when Auction Insights shows nobody else and a pause test showed no loss.
- The monthly cost is at most `volume` x `cpc`, and in practice a fraction, since most searchers still click the organic result.

## Deliver

Two tables. Terms: term, `volume`, `cpc`, bid range, the user's organic rank, other organic rows about the brand, verdict. Bidders: advertiser, kind (competitor, reseller, affiliate), headline and body, days running, still running, landing page title, whether the text names the brand. Then the user's actions: the brand campaign, an answer in the user's own ad copy to each conquesting angle, and a trademark complaint to Google where an ad's text uses the user's registered mark. A default run costs about 28 credits.

## Judgment

- Do not answer a conquester by bidding on their name by reflex: competitor terms cost more and convert worse ([Google search ads](google-search-ads.md)).
- A competitor's organic "<brand> alternative" or "vs" page is conquesting without ads. The user's own comparison pages belong to [seo](../../seo/SKILL.md).
- Conquesting starts and stops. A recurring check for new bidders belongs to [competitor-intel](../../competitor-intel/SKILL.md).
