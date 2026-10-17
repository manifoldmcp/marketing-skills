# Unlinked mentions

Pages that already name the brand, the product or the founder without linking. The writer chose to mention it, so the ask is one line. Checked October 2026.

## Find them

1. **Search Google for the names.** `seo_get_serp` with the keyword `"<brand>" -site:<domain>` and `depth: 100` (10 credits), and the same for each product name and for the founder if they are quoted in press. Google's operators pass through: quotes keep exact matches, `-site:` drops the user's own pages, and `after:YYYY-MM-DD` keeps recent coverage. If the name is a common word, add a category word inside the query ("Acme" crm). Google returns about 100 results a query at most, so this is a sample of the best-ranking mentions, not all of them.
2. **Drop what is not pitchable.** Social platforms, forums and video (Reddit, YouTube, LinkedIn, X, Facebook), the user's own profiles, and review sites and directories (G2, Capterra, Trustpilot, Product Hunt, Crunchbase): a profile is claimed, not pitched. If nothing is left, the brand has no indexed mentions to convert yet; say so.
3. **Drop sites that already link.** `seo_get_referring_domains` on the user's domain with `limit: 1000` (25 credits) and remove every mention whose domain is in it. The index lags by weeks, so a mention from the last month may link already: open the page if the host can, otherwise mark it "no link in the index".
4. **Check the pages.** `seo_get_page` on each (free, rate limited); drop a `status` other than 200 or a `final_url` that moved elsewhere. From `title`, `h1` and the SERP `snippet`, note what the mention is: a review, a list, a news story, an interview, a passing reference.
5. **Rate, then contact.** `seo_get_domain_ratings` on all kept domains in one call, with relevance over rank ([authority metrics](authority-metrics.md#floors)): a trade blog that reviewed the product beats a bigger unrelated site. Then [contacts](contacts.md) for the top 20, the page's author first.

About 695 credits for two names and 20 contacts.

## The ask

- **A thank-you and one URL.** "You mentioned us in <title>; here is the page if you want to link it." Nothing else in the email.
- **Link the page that helps the reader**: the source page for a statistic, the product page for a review, the homepage only when the mention is the brand itself.
- **Newest first.** Recent articles get edited; pages over a year old rarely do.
- **Many news sites never link to companies they cover, or link nofollow.** One polite email, no follow-up chase. The mention still counts for brand demand and for AI answers, which follow mentions more than links (see [ai-search](../../ai-search/SKILL.md)).
- **A mention that gets a fact wrong** (an old price, a dropped feature) is a correction first and a link second.
- **Re-run each quarter.** The server keeps no history; the host keeps the table and drops rows already asked. Watching for new mentions as they appear belongs to [brand-monitoring](../../brand-monitoring/SKILL.md).
