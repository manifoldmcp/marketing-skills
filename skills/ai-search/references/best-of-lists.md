# Best-of lists AI engines cite

When a buyer asks an engine for "the best X", the answer is often built from a handful of listicles, and the engines cite them. Getting onto those lists changes the answer. This playbook picks the lists by what the engines cite; getting onto a list works the way it does for lists that rank on Google, so it hands over to the link-building playbook for that part.

## Inputs to settle first

- **Category prompts**: three to five "best X", "top X for Y" and "X alternatives" prompts. Default: the category prompts of a [visibility check](visibility-check.md) run, if there is one.
- **Brand and competitors**: as in the visibility check.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 5 x 18 + 5 x 2 + 10 x 8 + 10 = 190 credits for five prompts, five AI overviews and contacts at ten lists. With a visibility check run already in hand, about 100. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Get the answers.** Reuse a visibility check run with `get_task` (free), or run `aeo_run_ai_answers` with the category prompts and the `brands` (18 credits per prompt on the default five engines), then `get_task` after `poll_after_s`.
2. **Add Google's AI overviews.** `seo_get_serp` with `ai_overview: true` for each category prompt written as a keyword (2 credits each). `ai_overview.references` are the pages Google's answer cites, and the organic results show which of them also rank.
3. **Pick the lists.** From the citations in step 1 and the references in step 2, keep the pages whose title reads as a list (best, top, alternatives, compared, vs) on a publisher, not on a vendor. For each, count the engines and prompts citing it. Vendor-owned lists never name a rival; note them for the [seo](../../seo/references/comparison-pages.md) group's comparison pages.
4. **See who each list names, and find its editor.** Do this the way the link-building [best-of lists](../../link-building/references/best-of-lists.md) playbook does in its steps 3 and 5: `seo_get_page` on each list for the products in its headings, then its contact steps. Skip its domain rank floor: here the citations are the proof a list matters.
5. **Deliver** a table: list URL, engines citing, prompts citing, cited in Google's AI overview (yes or no), organic position if it ranks, competitors named, user named (yes, no, unclear), contact, role, email, verification status, and the angle: what the user adds to the list.

## Judgment

- A list three engines cite for the category prompt outranks any list with more traffic. Order by engines citing, then prompts.
- A list that already names the user but low down is still worth an email: engines read order, and a line on what the user does best can move it up.
- Many cited lists are affiliate pages. The editor may ask for an affiliate deal or a fee; flag it and let the user decide, and never agree to pay.
- Lists refresh slowly and engines refresh their sources at their own pace. Re-run the same prompts after four weeks to see whether an inclusion changed the answers.
- For lists that rank on Google but that no engine cites, use the link-building [best-of lists](../../link-building/references/best-of-lists.md) playbook directly.
