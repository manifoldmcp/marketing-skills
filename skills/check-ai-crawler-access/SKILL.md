---
name: check-ai-crawler-access
description: When the user wants to know whether AI crawlers can reach and read their site. Checks robots.txt, the CDN or firewall, content that only appears after JavaScript, noindex and nosnippet tags and the key pages for GPTBot, OAI-SearchBot, PerplexityBot, ClaudeBot and the other answer and training crawlers, and gives the exact fix per failure, at no credit cost. Also use when the user mentions is our site ready for AI search, do we block GPTBot, are we blocking ChatGPT at Cloudflare, do we need an llms.txt, AI crawler access, or an AI crawler audit. A whole technical SEO audit goes to audit-technical-seo, whether the engines actually mention or cite the site to check-ai-visibility, and the sources they cite instead to build-ai-citations.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# AI crawler readiness

Whether the AI engines can reach and read the site. An engine cannot cite a page its crawler is turned away from, or a page whose content only appears after JavaScript runs. This skill checks the site the way the engines document, separates what hides the site from answers from what is only hygiene or a policy choice, and gives the exact fix for each failure. It costs nothing, and hands back a check table with the fix, owner and priority for each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `aeo_get_site_readiness` and `seo_get_page` (hosts often add a prefix, for example `mcp__manifold__aeo_get_site_readiness`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the site, the key pages, a competitor's domain) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

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
6. **Deliver** a table: check, tier, status, what was found, the fix (the exact `robots.txt` line, the CDN setting, the tag to remove), owner, priority. Then one line on the training crawler policy as the user chose it. For a whole technical SEO audit beyond AI crawlers, point to [audit-technical-seo](../audit-technical-seo/SKILL.md).

## Judgment

- Blocking training crawlers (for example GPTBot or Google-Extended) is a legitimate choice and does not hide the site from answers. Blocking the search and user-fetch crawlers does. Never tell a user to open training access to fix visibility.
- The check reports each item with its evidence tier and a source URL on purpose. Quote the tier; do not turn an `unproven` item into a recommendation.
- A readiness pass does not make engines cite the site; it removes the reasons they cannot. What they cite is the job of [build-ai-citations](../build-ai-citations/SKILL.md).
- The result is cached for an hour. After the user deploys a fix, wait an hour before checking again, or the old result comes back.
- Cloudflare and other CDNs change bot defaults. A site that passed last quarter can fail today without anyone touching `robots.txt`.

## Related skills

- Crawl errors, indexing and the rest of technical SEO: [audit-technical-seo](../audit-technical-seo/SKILL.md).
- Whether the engines mention and cite the site once they can read it: [check-ai-visibility](../check-ai-visibility/SKILL.md). The plan around it: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
- Wrong facts the engines took from other sites while the site was unreadable: [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md).
