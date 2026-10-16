# Objections

What makes buyers hesitate before they buy a product like the user's: doubts about price, trust, fit, effort and timing, from what they search, ask and argue about. Ends in a ranked table that says where each objection needs an answer.

## Where doubts show

- **Google.** `seo_search_keywords` with the category as `seed`, then the best-known competitor (10 credits each). Keep the keywords that carry a doubt: worth it, legit, scam, safe, review, cost, price, hidden fees, free, cancel, refund, vs, alternative. Their `volume` shows which doubts buyers take to Google before they ever talk to sales.
- **Reddit.** Post searches such as "is <competitor> worth it", "<category> worth it", "<competitor> legit", "regret buying <category>", "should I get <category>" (1 credit each), comment searches on "<competitor> worth it" and "<category> not worth", then the replies of the eight busiest on-topic threads: they hold the doubts other buyers add and the answers that settled them.
- **Comments under reviews.** `youtube_search_videos` for "<competitor> review" and "is <category> worth it" with `since: "year"`, `youtube_get_video` on the most viewed for the `comments` count, `youtube_get_comments` on the five with most. Viewers of a review are deciding; their comments are objections more than complaints.
- **Comments under ads** in a consumer category: objections to the offer ("is it free?", "why would I pay for this when my notes app works"), sitting under the referral codes.
- **The user's own** sales calls, demos, support tickets and churn reasons. Check them against what buyers say in public: the gaps both ways are findings.

Default run about 49 credits (two keyword searches, about 12 Reddit calls, about 15 YouTube calls).

## Code each doubt

One type per quote:

| Type | Sounds like |
|---|---|
| Price or value | "not worth $30 a month", "free version does enough" |
| Trust or risk | "will it work for us", "is the company going to be around", "what about my data" |
| Fit | "not for a team like mine", "too much for a solo" |
| Effort or switching | setup, migration, learning, getting the team to use it |
| Timing | "not now", "after the busy season" |
| Status quo | "a spreadsheet does this", "we just eat the cost" |

Drop complaints from people who already use a product: that is [pain points](pain-points.md), or [competitor-intel](../../competitor-intel/SKILL.md) for a named rival. Note any reply that resolved the doubt ("I worried about that too, but...") and what did it: a number, a guarantee, a trial, a story.

## Rank and place

- Rank by distinct authors; break ties by summed search `volume`. A doubt people both ask and search is the one most buyers carry silently.
- **Status quo is the most common objection and the least often stated.** "We just use a spreadsheet" is an objection to the effort of changing, not to the price.
- Price objections from people far outside the ICP (students on a business tool) are noise: keep them apart.
- **Where to answer:** a doubt with search volume needs a page, because buyers look for the answer alone; a doubt only in threads belongs in sales talk tracks, onboarding and the FAQ. Options: pricing page, FAQ entry, comparison page, guarantee or trial, sales talk track, ad.
- **Answer with proof, not a claim.** For each of the top three, name the proof that would settle it (a number, a named customer, a demo, a refund policy) and whether the user has it.

## Deliver

A table: rank, objection (buyer words), type, voices, sources, Google keywords with volume, two quotes with links, what resolved it in public (or "nothing seen"), where to answer it. Mark the objections the user listed that buyers did not raise, and the ones buyers raised that the user never mentioned. Offer to write the top three and their answers into section 7 of `.agents/product-marketing.md`.

Never answer in the threads from here. Turning an objection into a reply belongs to [reddit](../../reddit/SKILL.md); a talk track against one rival to [competitor-intel](../../competitor-intel/SKILL.md).
