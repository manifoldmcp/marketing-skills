# Site access

Whether engines can fetch and read the site. Access does not make engines cite a site; it removes the reasons they cannot. Check it first in any AI search work, because nothing else matters while it fails. Checked October 2026.

## What actually blocks citations

- **Blocking the search and live-fetch crawlers.** OAI-SearchBot, Claude-SearchBot, Claude-User and PerplexityBot (see [engines](engines.md)). OpenAI says plainly that a site opted out of OAI-SearchBot will not be shown in ChatGPT search answers.
- **The CDN or firewall turning them away before robots.txt is read.** Cloudflare and other CDNs ship bot-blocking defaults and change them; a site that passed last quarter can fail today with nobody touching robots.txt. The fix is in the CDN's bot settings, not robots.txt.
- **Content only in JavaScript.** AI fetchers mostly read the raw HTML. A client-rendered page arrives as an empty shell. The fix is server-side rendering or prerendering.
- **`noindex` or `nosnippet`** in a robots meta tag or `X-Robots-Tag` header. On Google these are also the only controls over AI Overviews and AI Mode, and they cut the classic snippet too.
- **Missing from the index the engine searches.** Google for Gemini and Google's AI answers, Brave (almost certainly) for Claude, Bing among others for ChatGPT. A page the index has not got cannot be retrieved.

## What blocks nothing

- **Training crawlers are a separate decision.** GPTBot, ClaudeBot and Google-Extended govern training (Google-Extended also covers Gemini app grounding), not search citations; Google-Extended does not touch AI Overviews or AI Mode. Blocking them is a legitimate choice. Never tell a user to open training access to fix visibility.
- **robots.txt does not keep a site out of live fetches.** ChatGPT-User may ignore it and Perplexity-User generally does; only Anthropic says all three of its bots honour it. A user who wants out of answers needs a CDN or IP block.

## Unproven: report, do not prescribe

- **llms.txt.** Bots barely request it: 97% of llms.txt files got no traffic and bots never asked for it on sites without one (Ahrefs, 137K domains, May 2026); 408 requests in over 500M AI bot events (Limy). No correlation with citations (SE Ranking, 300K domains). Google does not use it, and no other engine has said it does. Fine to have; never a fix.
- **Schema markup.** Adding JSON-LD produced no citation uplift on AI Overviews, AI Mode or ChatGPT in a controlled test of 1,885 pages against 4,000 matched controls over 30 days (Ahrefs, May 2026). The older finding that cited pages carry more schema is confounded by site quality. Google says no special schema is needed. Schema still earns rich results in classic search; that is its case.
- **Chunking pages into bite-size passages.** Google says not to; the evidence for it measures retrieval scoring, not citations.
- **Markdown versions of pages** (`Accept: text/markdown`). Harmless and passes the JavaScript check, but no engine has said it prefers them.

## Checking it with Manifold

`aeo_get_site_readiness` (free, cached one hour) runs about seventeen live fetches and returns `checks[]` failing first, each with a `tier` that encodes the evidence above: `blocks_citations` (documented by an engine: fix these first), `evidence_backed` (measured: freshness signals), `hygiene`, `policy` (training crawlers; never fails) and `unproven` (llms.txt, schema presence; never fails on absence). Quote the tier and do not promote an `unproven` item to a recommendation.

- `robots_txt.ai_bots[]` names each bot with its `kind` (search, user_fetch, training) and whether it `honours_robots`. `edge.probes[]` shows which user agents the CDN turned away.
- `js_shell` and `word_count` on `homepage` and `sample_page` show a JavaScript shell; check key pages one by one with `seo_get_page`, since one sampled page does not cover a site.
- After the user deploys a fix, wait an hour before re-checking, or the cached result comes back.
- Run it on a competitor too: a competitor that admits the answer crawlers the user blocks has an easy edge.
- For crawl errors, indexing and the rest of technical SEO, see [technical-seo](../../technical-seo/SKILL.md).
