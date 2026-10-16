# Wrong answers about the brand

When an engine states something wrong about the brand: an old price, a feature it lacks, the wrong audience, or a fact about another company with a similar name. Being named with the wrong price can cost more than not being named.

## Find them

Ask prompts that make an engine state facts: "What is <brand>?", "How much does <brand> cost?", "Is <brand> good for <use case>?", "Does <brand> integrate with <tool>?", "<brand> vs <competitor>", "<brand> alternatives", plus one prompt per error the user already knows about. Run them on every engine the buyers use, `ai_mode` included (`aeo_run_ai_answers`, 20 credits a prompt on all six), and mark each claim about the brand: correct, wrong, outdated, missing (a key fact left out) or confused (another company's fact).

Confirm before chasing: a claim wrong once may be noise; wrong on two engines, or twice on one, is real.

## Trace where it came from

The row's `citations[]` are the likely source. Three cases:

- **The brand's own page.** An old pricing page, a stale plan name in a page title, a blog post announcing a feature that was later dropped. `seo_get_page` shows the title, description and headings. This is the most common cause and the fastest fix.
- **A third-party page.** A review, directory listing, comparison post or old article. Note the URL and what it says.
- **No citations at all.** The answer came from training data. It changes only when a new model is trained on corrected sources, so the fix is the same (correct the sources) but the wait is months.

If the engines cannot read the site (answer crawlers blocked, content only in JavaScript, see [site access](site-access.md)), they fall back on what others wrote, which is why unreadable sites collect wrong answers. Fix access first.

## Fix it

- **Own page:** correct it, and state the fact in plain text on a crawlable page (pricing, about, an FAQ or facts page). Engines that search live re-read it on their next fetch.
- **Third-party page:** ask the author or site for a correction, with the right fact and a source. The tools can find the contact; never send on the user's behalf.
- **Review site or directory:** claim and update the listing.
- **Confused with a similar name:** make the entity distinct. The full product name, the category and the domain together on the site and every profile; Organization schema whose `sameAs` lists the official LinkedIn, Crunchbase, Wikidata and review-site profiles (`aeo_get_site_readiness` returns the homepage's `same_as`). Schema has shown no citation uplift in testing (see [site access](site-access.md)), but tying a name to one entity is a different job and costs little.
- **Engine feedback buttons** exist and cost nothing, but changing the sources is what changes answers that search the web.

Rank the fixes by what a buyer decides on: wrong pricing and wrong "does it do X" answers lose sales; a wrong founding year does not. Re-run the same prompts two to four weeks after the fixes.
