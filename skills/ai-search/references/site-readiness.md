# Site AI readiness

Whether the AI engines can reach and read the site. An engine cannot cite a page its crawler is turned away from, or a page whose content only appears after JavaScript runs. This playbook checks the site the way the engines document, separates what hides the site from answers from what is only hygiene or a policy choice, and gives the exact fix for each failure. It costs nothing.

## Inputs to settle first

- **Site**: the domain. The check runs against its origin; `www` and the bare domain can differ, so use the one visitors land on.
- **Key pages**: three to five URLs that should be cited (pricing, the main product page, the best guides). Default: the homepage and the page the check samples from the sitemap.
- **Training crawlers**: whether the user wants AI companies to train on the site. It is a policy choice, not a readiness failure; ask.
- **A competitor** to compare with, optional.
- **Budget**: 0 credits. `aeo_get_site_readiness` and `seo_get_page` are free but rate limited, and cached for an hour.

## Steps

1. **Run the check.** `aeo_get_site_readiness` on the site (free). `checks[]` comes back failing first, each with a `tier`, a `detail` and a `source`.
2. **Fix what blocks citations first.** Read the checks whose `tier` is `blocks_citations`:
   - `homepage_reachable`: the site did not answer. Nothing else matters until it does.
   - `answer_bots_allowed`: `robots.txt` disallows a crawler that answers live questions or fetches for a user. `robots_txt.ai_bots[]` names each bot, its `kind` (search, user_fetch, training) and whether it honours robots. The fix is an `Allow` for that user agent, or removing the `Disallow`.
   - `edge_allows_answer_bots`: the CDN or firewall turns away answer crawlers before `robots.txt` is read. `edge.probes[]` shows which user agent was blocked. The fix is in the CDN's bot settings (for example a bot fight mode or a rule blocking verified AI crawlers), not in `robots.txt`.
   - `content_without_javascript`: the raw HTML is a shell (`js_shell` on `homepage` or `sample_page`, low `word_count`). Engines that do not run JavaScript see nothing. The fix is server-side rendering or prerendering; `markdown` shows whether the origin serves a text version on request.
   - `indexable_and_snippet_eligible`: a `noindex` or `nosnippet` in `robots_meta` or `x_robots_tag` keeps pages out of answers.
3. **Then the rest, in order.** `evidence_backed` (freshness signals: `date_modified`, `last_modified_header`), then `hygiene` (redirect chains, sitemap, title and description, structured data). `policy` is the training crawlers: report what is allowed and match it to the user's answer in the inputs; it never fails. `unproven` is llms.txt and schema presence: report them, but do not present them as fixes that move citations.
4. **Check the key pages.** `seo_get_page` on each key page (free): `status`, `robots_meta`, `canonical` pointing elsewhere, `word_count` near zero (content loaded by JavaScript), and `schema_types`. A page that fails here is not cited however good the site-wide checks are.
5. **Compare, if asked.** `aeo_get_site_readiness` on the competitor (free). A competitor that allows the answer crawlers the user blocks has an easy edge; say so.
6. **Deliver** a table: check, tier, status, what was found, the fix (the exact `robots.txt` line, the CDN setting, the tag to remove), owner, priority. Then one line on the training crawler policy as the user chose it. For a whole technical SEO audit beyond AI crawlers, point to the [seo](../../seo/references/audit.md) group's audit.

## Judgment

- Blocking training crawlers (for example GPTBot or Google-Extended) is a legitimate choice and does not hide the site from answers. Blocking the search and user-fetch crawlers does. Never tell a user to open training access to fix visibility.
- The check reports each item with its evidence tier and a source URL on purpose. Quote the tier; do not turn an `unproven` item into a recommendation.
- A readiness pass does not make engines cite the site; it removes the reasons they cannot. What they cite is the job of [citation building](citation-building.md).
- The result is cached for an hour. After the user deploys a fix, wait an hour before checking again, or the old result comes back.
- Cloudflare and other CDNs change bot defaults. A site that passed last quarter can fail today without anyone touching `robots.txt`.
