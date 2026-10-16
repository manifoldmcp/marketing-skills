# Reviews

How reviews affect local ranking and choice, how to earn more of them within Google's policy and the law, and how to reply. Checked October 2026. Not legal advice: for a programme with incentives or employee involvement, the user should check with counsel in their country.

## What reviews do

- **For ranking,** Google names review count and score as part of prominence. The 2026 Whitespark survey ranks reviews highly for the pack, and practitioner studies (correlational, mostly vendors) find count and recency associate with position more than the average rating. Sterling Sky's best-known recency case is one client in 2022 whose rankings fell when its review flow stopped and recovered when it restarted; there is no measured threshold, and the "three weeks without reviews" figure that circulates traces to no study.
- **For the click,** the rating and the freshness decide. BrightLocal's 2026 consumer survey (1,002 US adults, February 2026): 41% always read reviews (29% in 2025); 74% read only reviews from the last three months and 32% want some from the last two weeks; 47% will not use a business with fewer than 20 reviews; 68% require 4 stars and 31% require 4.5 (up from 55% and 17%). Google remained the most used review site (71%, down from 83%), with Apple Maps rising (27%, from 14%).
- **For AI answers,** reviews and ratings are part of what engines read from listings; locations ChatGPT recommended averaged 4.3 stars (SOCi, January 2026), and the survey's AI column rates reviews on authoritative third-party sites highly ([AI answers](ai-answers.md)).
- **Survey top conversion factors** (Whitespark 2026): high rating, positive sentiment in the text, many reviews with text, recency, owner replies to most reviews, and a steady flow rather than bursts.

## What is allowed

**Google's policy** (Maps user-generated content policy, rating manipulation section):

- Allowed: asking every customer for a review, with a link or a QR code; reminding them by email or text after the job.
- Not allowed: paying, discounting or giving anything in exchange for a review; asking only customers likely to leave a good one (gating) or discouraging negative ones; reviews by owners, staff or family; reviews written for the business or posted from one device on the premises (kiosks, a tablet at the counter); and since April 2026 asking staff to collect a set number of reviews or asking customers to name a staff member (spotted 17 April 2026 by Google Product Expert Amy Toman; Google did not announce it, and it is unclear whether old reviews that name staff are affected).
- Enforcement is automated: Google says it blocked or removed over 292 million policy-violating reviews in 2025, revisits reviews months after posting, pauses new reviews on a profile during a spike of suspected spam, and shows searchers a warning when it has removed fake reviews. Removals can hit genuine reviews too; Google has restored batches after mistakes.

**United States:** the FTC's Consumer Review Rule (16 CFR Part 465, in force 21 October 2024) bans fake or AI-written reviews presented as real, buying reviews, incentives conditioned on a positive (or negative) review, undisclosed insider reviews, suppressing negative reviews on the business's own site while implying the shown reviews are all or most of them, threats to remove negative reviews, company-controlled "independent" review sites and fake social proof. Civil penalties run up to $53,088 per violation (2025 figure, adjusted yearly). The FTC sent its first warning letters under the rule to 10 unnamed companies on 22 December 2025 and had announced no lawsuit under it by August 2026. An incentive offered for any honest review, positive or negative, is not banned by the rule (the FTC's endorsement guides still expect it disclosed), but Google forbids incentives for Google reviews outright.

**United Kingdom:** since 6 April 2025 the Digital Markets, Competition and Consumers Act lists fake reviews, undisclosed incentivised reviews and misleading presentation of reviews as banned practices, with CMA fines up to 10% of worldwide turnover. The CMA opened five investigations into review practices on 27 March 2026. Google gave the CMA undertakings in January 2025, including warnings on and restrictions for businesses that benefit from fake reviews.

## Earning more reviews

- **Ask everyone, at the moment of satisfaction,** in person and by a link sent the same day. The ask names Google, gives the direct review link (from the owner's dashboard), and says honest feedback helps; it never steers to five stars or filters who gets the link.
- **A steady flow beats a burst.** Practitioners pace the ask to the business's job volume, roughly matching the leading competitors' rate; a sudden flood is a suspension and filter risk.
- **Ask about what the business wants known.** Customers write about what they are asked about; a request that mentions the job ("how did the boiler install go?") yields reviews that mention the service, which feed relevance and the `topics` Google shows. Never ask them to name a staff member.
- **Measure monthly:** the new-review count, against the top three competitors, read from `reviews` a month apart (no Manifold tool returns review dates).

## Replying

- **Reply to all of them, soon, in a person's voice.** 89% of consumers expect owners to reply and 81% within a week, while half are put off by templated replies (BrightLocal, 2026). Google says replying helps. Draft replies for the owner; the owner posts them.
- **A negative review:** thank, state the facts briefly without customer details (health, legal and financial businesses have privacy rules about confirming someone was a customer), offer to resolve it offline, and never argue. A future customer is the reader.
- **A fake or policy-breaking review** (a competitor, a non-customer, hate speech, a conflict of interest) can be reported from the profile and appealed through the owner's review management tool. Disagreement is not a policy breach. Never threaten a reviewer: under the FTC rule a threat to get a negative review removed is itself a violation.

## Reading what reviews say

`maps_get_place` returns `topics` (the top 10 themes Google extracted, with mention counts; they can include a staff member's first name) and `rating_counts` (the star distribution, 1 star first), not the review text. For the text and what it says about customers' needs and objections, see [voice-of-customer](../../voice-of-customer/SKILL.md); the text itself is read by hand on the profile, signed in, or exported from the owner's dashboard.
