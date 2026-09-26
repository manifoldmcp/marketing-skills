# Positioning

How the user differs, turned into two or three positioning options they can choose between. Each option fills the same five parts: the competitive alternatives buyers would use instead, the attributes only the user has, the value those attributes deliver, the customers who care most about that value, and the market category that makes the value obvious (the components of April Dunford's method). The tools supply the evidence; the user makes the choice.

## Inputs to settle first

- **Best customers**: the customers who bought fastest, use the product most or stayed longest, and why they chose it. This is the most important input; ask for it, and say the options are weaker without it.
- **Product**: the user's domain, and the features or ways of working they believe set them apart.
- **Alternatives**: the direct and indirect competitors. Default: the table from [find competitors](find-competitors.md), or the two or three the user names plus "a spreadsheet or doing nothing".
- **Messaging**: the table from [messaging and pricing](messaging-pricing.md) if it exists. Otherwise step 1 builds a short version.
- **Voice of the customer**: why buyers pick, leave or complain about each alternative, from [customers pain points](../../customers/references/pain-points.md) or [reddit competitor complaints](../../reddit/references/competitor-complaints.md). If neither has run, run the Reddit one on the top two competitors first and add its cost.
- **Budget**: a default run costs about 10 + 36 = 46 credits, plus the voice-of-the-customer playbook if it has not run; the pages are free. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **List the claims.** `seo_get_page` (free) on the user's homepage and each competitor's: `h1[]`, `h2[]` and `meta_description`, or take them from the messaging table. Write each alternative's claims in a column. A claim every competitor makes is table stakes, not a difference.
2. **Take the buyers' words.** From the voice-of-the-customer output, pull the reasons buyers give for choosing an alternative, for leaving it, and for what they still lack. Keep quotes with links. These are the values buyers already care about, in their own words.
3. **Map attributes to value.** For each attribute the user has and the alternatives lack (step 1, plus the user's own list, confirmed by the user), write the value it delivers as a buyer outcome in the words from step 2, and the proof the user can show (a number, a customer, a demo). Drop any attribute that maps to no value a buyer asked for.
4. **Find who cares most.** Group the values by the kind of customer who needs them most: the segment the best customers come from, a use case, a company size. That group is the best-fit customer for the option. For personas with evidence behind them, use [customers personas](../../customers/references/personas.md).
5. **Test the categories.** List three or four candidate categories: the one the user claims now, the one competitors claim, a narrower subcategory ("<category> for <segment>"), and an adjacent one. `seo_get_keyword_metrics` on two or three phrasings of each (10 credits for up to 100 keywords): `volume` and `trend[12]` show whether buyers search with those words. `aeo_run_ai_answers` with "best <category>" for the two strongest candidates, `brands` holding the user and the competitors (18 credits per prompt on the default engines, 36), then `get_task` (free) after `poll_after_s`: the brands the engines list show who owns each category today and whether the user fits in it. For a fuller demand read, use [customers demand check](../../customers/references/demand-check.md).
6. **Deliver** a table with one row per option (two or three) and these columns: competitive alternatives, unique attributes, value (in buyer words, with proof), best-fit customers, market category, the evidence (quotes with links, search volume, AI answer counts), a one-line positioning statement, and the trade-off (what the option gives up). Recommend one if the evidence clearly favours it, and say why; the user decides.

## Judgment

- Start from the best customers, not from all customers. Positioning built for everyone lands on the same claims as the competitors.
- A feature is not value. "Two-way sync" is an attribute; "no more double entry after every call" is the value, and only buyers' words make it credible.
- An attribute a competitor also claims in its `h1` or `h2[]` is not unique, however much better the user thinks theirs is. Unique can mean "only one that does it for this segment".
- A category with an entrenched leader favours a subcategory: "<category> for <segment>" lets the user win a smaller fight. A category with no search volume can still be right, but then buyers must be taught it exists; say what that costs.
- AI answers show which brands the engines put in a category now. A category where the engines name no clear leader is easier to claim.
- Every attribute needs the user's confirmation. Never position on a capability the product does not have.
- The output is positioning, not copy. The homepage, ads and comparison pages follow from the chosen option in a separate request.
