# Positioning

How the user differs, turned into two or three options they choose between. The output is positioning, not copy: the homepage, ads and comparison pages follow from the chosen option in a separate request.

## The frame

Each option fills the same five parts (the components in April Dunford's method, Obviously Awesome, 2019): the **competitive alternatives** buyers would use instead (including a spreadsheet, a hire or doing nothing), the **attributes** only the user has, the **value** those attributes deliver in buyer outcomes, the **best-fit customers** who care most about that value, and the **market category** that makes the value obvious. Dunford's is a practitioner method from client work; it has not been tested against other frameworks, and neither has any rival (jobs to be done, the classic "for X who need Y" statement). Its strength is that each part can be tied to evidence, which is how it is used here.

## What the evidence says about difference

- **Buyers perceive little difference between brands.** Ehrenberg-Bass work across many categories found that few users of any brand see it as different or unique, and that the share is similar for competing brands (Romaniuk, Sharp and Ehrenberg, Australasian Marketing Journal, 2007). A difference the user believes in is not one buyers notice. Being easy to recognise and to recall in a buying situation does more work than most teams expect; this view is contested by agencies measuring "meaningful difference", whose data are proprietary.
- **Category-creation claims are weak evidence.** "Category kings capture 76% of the category's value" comes from a consultancy's look back at successful venture-backed companies (Play Bigger, 2016): a survivorship sample, not a test of what claiming a category does.
- **A feature is not value, and only buyers' words make value credible.** "Two-way sync" is an attribute; "no more double entry after every call" is the value. Take the value wording from [pain points](pain-points.md) and [customer language](customer-language.md).

## Gathering the evidence

1. **Claims.** `seo_get_page` (free) on the user's homepage and each competitor's: `h1[]`, `h2[]` and `meta_description`. A claim every competitor makes is table stakes, not a difference. For a fuller messaging comparison see [competitor-intel](../../competitor-intel/SKILL.md).
2. **Buyers' reasons.** Why buyers pick, leave or complain about each alternative, from a [pain points](pain-points.md) run or competitor complaints. If neither has run, run one first and add its cost.
3. **Best customers.** The customers who bought fastest, use most or stayed longest, and why. The most important input: positioning built for all customers lands on the same claims as the competitors. Say the options are weaker without it.
4. **Attributes to value.** For each attribute the user has and the alternatives lack (confirmed by the user; never position on a capability the product lacks), write the value as a buyer outcome in buyer words, and the proof (a number, a customer, a demo). Drop attributes that map to no value a buyer asked for. Unique can mean "the only one that does it for this segment".
5. **Who cares most.** Group values by the customer who needs them most: a segment, a use case, a company size. For evidence-backed personas see [personas](personas.md).
6. **Categories.** Three or four candidates: the one claimed now, the one competitors claim, a narrower "<category> for <segment>", an adjacent one. `seo_get_keyword_metrics` on two or three phrasings of each (10 credits for up to 100 keywords): `volume` and `trend[12]` show whether buyers use those words. `aeo_run_ai_answers` with "best <category>" for the two strongest, all competitors in `brands` (18 credits a prompt on the default engines, never cached), read with `get_task`: the brands engines list show who owns each category in AI answers now ([ai-search](../../ai-search/SKILL.md) explains how noisy those lists are).

Default run about 46 credits plus the pain run if needed; pages are free.

## Choosing

- A category with an entrenched leader favours a subcategory: a smaller fight the user can win.
- A category with no search volume can still be right, but buyers must be taught it exists; say what that costs.
- A category where AI answers name no clear leader is easier to claim.
- Recommend one option when the evidence clearly favours it, and say why. The user decides.

## Deliver

One row per option: alternatives, unique attributes, value (buyer words, with proof), best-fit customers, category, the evidence (quotes with links, search volume, AI answer counts), a one-line positioning statement, and the trade-off (what the option gives up). Quote competitors only as short evidence with the URL. Offer to write the chosen option into section 6 of `.agents/product-marketing.md`.
