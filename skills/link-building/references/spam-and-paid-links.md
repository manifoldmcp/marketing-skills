# Spam policies and paid links

What Google counts as link spam, what happens to it, what paid placements really cost, and the disclosure rules. Read it before recommending any placement someone has to pay for. Checked October 2026.

## Google's rules

- **Link spam is any link made mainly to manipulate rankings** ([spam policies](https://developers.google.com/search/docs/essentials/spam-policies), updated 28 August 2026). Named examples: links bought for money, goods, services or free products; excessive link exchanges; automated link building; contract or terms-of-service links the other site cannot qualify; text ads that pass credit; advertorials and guest posts that pass credit or use optimised anchors; low-quality directory and bookmark links; widget, footer and template links spread across sites; forum signature links.
- **Paid links are allowed when marked.** `rel="sponsored"` (preferred for paid placements) or `rel="nofollow"`; `rel="ugc"` is for user content ([qualify outbound links](https://developers.google.com/search/docs/crawling-indexing/qualify-outbound-links), December 2025). A marked paid link is advertising: it can send visitors, it passes no ranking credit.
- **What spam detection does to links is mostly silent neutralisation.** When Google's systems discount spammy links, any ranking they produced "cannot be regained" (Google's spam update docs, December 2025). SpamBrain, Google's AI spam system, is retrained between updates. The usual outcome of bought links is money spent for nothing, with a manual action ("unnatural links", in Search Console) for blatant cases.
- **Spam updates are frequent and unlabelled.** Five in fourteen months: August 2025 (27 days to roll out), March 2026 (under a day), June 2026, August 2026 (three days), September 2026 (13.7 days), per the [Search Status Dashboard](https://status.search.google.com/). None was announced as a link spam update, and Google published no new policy with the ones reported on; the last named link spam update was December 2022. A ranking drop during one is a reason to check links bought or exchanged, not proof.
- **Site reputation abuse.** Third-party content placed on a strong site mainly to borrow its ranking signals (coupon, casino or "best X" sections on news sites) is spam under Google's site reputation abuse policy, and since its 19 November 2024 update whatever the host's involvement or oversight. Outside the EEA it draws manual actions on that section; since 30 August 2026 Google no longer applies those manual actions to results shown in the EEA, after a European Commission requirement, and may rank such sections separately instead. A link from such a section is a weak placement everywhere.
- **Expired-domain abuse and private blog networks** (buying a dropped domain to host links) are named spam. Never recommend them.

## Negative SEO and disavow

- **Spam pointing at a site mostly does nothing.** John Mueller, January 2026, on comment-spam links: no effect, positive or negative. Every site collects spammy links; the CRM startup we sampled had 169 of its top 1,000 linking domains above a spam score of 30 ([field notes](field-notes.md)).
- **Disavow only links the site is responsible for**, or after a manual action. Google calls it not part of normal maintenance; Mueller has also said it "does have effects" (Reddit, reported by Search Engine Roundtable, 2 September 2026), so a careless disavow of good links can hurt. Order in the file does not matter; domain-level lines are simpler. A third-party "toxic score" is not Google's view.

## What paid placements cost

- **Marketplace guest posts:** $295 average bought direct from the site, $461 through a vendor, across 257,267 sites priced on a large link marketplace; only 1.37% met a basic quality bar (Ahrefs DR 71+ and 50,000+ monthly organic visits) (BuzzStream, June 2026). Sites that sell to anyone tend to sell to link networks too.
- **Earned links cost money as well.** SEOs' average acceptable price for one high-quality link is $509 (Editorial.link, 412 respondents, March 2026); digital PR teams most often report $300 to $500 a link, and 10.2% report $750+ (BuzzStream, February 2026). Use these to compare an earned-link plan with a paid one honestly, not to justify buying.
- **91.9% of SEOs believe their competitors buy links** (Editorial.link, 2026). Belief, not measurement; a competitor that ranks with bought links is not evidence they work, since neutralised links leave no trace in the tools.

## Disclosure

- **Anything given for coverage must be disclosed to readers.** The FTC's Endorsement Guides apply to a site paid or given a free product to write about a brand, and to affiliate links: disclose clearly and near the content ("paid link", "BRAND paid me to review this"), not in a footer or behind a link. Other markets have equivalents (the UK's CMA and ASA).
- **For Google, the link carries `rel="sponsored"`.** Both rules apply at once: a disclosed sponsored review with a followed link is still link spam.
- **Never pay, or offer to pay, for an undisclosed link or mention on the user's behalf.** When a site asks for a fee, put it in the table as "asks for payment" and let the user decide; if they go ahead, the placement is marked sponsored and disclosed, and it is bought for its readers, not its link.
