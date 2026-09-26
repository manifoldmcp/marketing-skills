# Lookalike companies

The best predictor of the next customer is the last good one. This playbook reads the records of the user's best customers, finds what they share, searches for companies with the same traits and ranks them by how many they match. It ends in an account list ready for the [lead list](lead-list.md).

## Inputs to settle first

- **Best customers**: three to ten domains. The best by fit (bought fast, stayed, paid well), not the biggest logos: a famous customer that took a year to close teaches the wrong pattern.
- **Exclusions**: current customers, open deals and accounts already contacted, as domains. The host holds them; the server keeps no state.
- **Market**: the regions to search. Default: the regions the customers are in.
- **Size of the list**: default 100 candidates, filled down to the best 20.
- **Budget**: a default run costs about 5 x 10 + 3 x 10 + 60 x 1 + 20 x 10 = 340 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Profile the customers.** `leads_get_company` on each best customer (10 credits each). Read `keywords[]`, `industries[]`, `employees`, `location`, `founded_year`, `funding_stage` and `technologies[]`. Write down the traits most of them share: keywords that appear on three or more records, the employee band that covers most of them, the regions, a funding stage, and technologies they have in common. The provider's own keywords match its search better than the user's description.
2. **Search per trait.** `leads_search_companies` once per shared keyword (up to three), each with the shared `employee_ranges` and `locations` (10 credits a page of 100). Keep the companies that appear in two or more of the searches first: they match more of the pattern. Remove the exclusions and the customers themselves by domain.
3. **Check size and industry cheaply.** The stubs carry no size or industry. For the top 60, `linkedin_get_company` with `url` set to the stub's LinkedIn URL (1 credit each) gives LinkedIn's `employees`, `industry` and `location`. Drop the ones outside the band or in the wrong industry. A stub with no LinkedIn URL stays in only if the rest of its evidence is strong.
4. **Fill the shortlist.** `leads_get_company` on the best 20 (10 credits each). Score each one against the customer profile, one point per match: shared keywords, industry, employee band, region, funding stage, shared technologies. Drop anything that misses on two of the first four.
5. **Deliver** a table: company, domain, LinkedIn URL, industry, employees, location, funding stage, the traits matched (for example "4 of 6: payments, 51 to 200, London, Series A"), the technologies it shares with the customers, the customer it most resembles, and the score. Offer to run the [lead list](lead-list.md) on it from its people step.

## Judgment

- Three good customers make a pattern; one does not. With fewer than three, say that the list is a guess and ask which traits the user thinks matter.
- A trait every company in the market has (the country, "software") does not separate anyone. Drop it from the scoring and keep the ones that set the customers apart.
- Shared technologies are strong when they are specific (a niche tool the product plugs into) and weak when they are common (Google Analytics). `technologies[]` is detected from outside, so it misses internal tools.
- Headcount from `linkedin_get_company` is LinkedIn's own figure and often differs from the provider's `employees`. A band is enough; do not argue over 10 percent.
- A lookalike of the current customers repeats their bias. If the user wants a new segment, that is a [market size](market-size.md) question first.
- The provider's index is not the whole market: small firms with little web presence are underrepresented, so a thin result in a local or offline market does not mean there are no lookalikes.
