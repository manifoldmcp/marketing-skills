# Google's AI answers

AI Overviews (the summary above the results) and AI Mode (the conversational tab) run on Google's index but behave unlike classic rankings, and unlike each other. Checked October 2026.

## Ranking is no longer a proxy

- **Only about 38% of AI Overview citations rank in Google's top 10 for the same query**, 31% rank 11 to 100 and 31% beyond 100 (Ahrefs, 863K SERPs and 4M cited URLs, January 2026). In July 2025 the same measure was 76%. The drop lines up with AI Overviews moving to Gemini 3 in January 2026; wider query fan-out is the likely cause but unproven. BrightEdge, by another method, puts top-10 overlap near 17%.
- So "rank first, then you will be in the overview" is out of date. Ranking still helps, but citation has to be measured directly, query by query.
- Google's AI features fan a query out into related sub-queries and retrieve for each. A page can be cited for a sub-question it answers well even when it ranks nowhere for the head query, which is the mechanism behind the 31% beyond position 100. Cover the sub-questions buyers would ask next, on the page or across the site.
- YouTube is the most-cited domain in AI Overviews and growing (+34% over six months; 18% of cited URLs that rank outside the top 100 are YouTube; Ahrefs, March 2026).

## AI Mode is a separate engine

- AI Mode and AI Overviews cite only about 14% of the same URLs for the same query, while saying much the same thing (86% semantic similarity; Ahrefs, 540K query pairs, September 2025). Wikipedia and Quora weigh more in AI Mode.
- 45% of AI Overview citations change between one generation and the next. Treat one SERP as a sample.
- Track AI Mode as its own engine (`ai_mode` in `aeo_run_ai_answers`), never as a copy of the overview.

## What Google says

[Google's AI features guide](https://developers.google.com/search/docs/appearance/ai-features) (updated December 2025): there are "no additional requirements to appear in AI Overviews or AI Mode, nor other special optimizations necessary", no special schema, and no need for "new machine readable files, AI text files, or markup". In January 2026 Danny Sullivan told publishers not to split content into bite-size chunks for LLMs: "We don't want you to do that." Take Google at its word for Google's surfaces; it says nothing about the other engines.

- **Controls:** Google-Extended does not affect AI Overviews or AI Mode. The only controls are `nosnippet`, `data-nosnippet`, `max-snippet` and `noindex`, and each also cuts the page's classic snippet. Reprocessing takes "several days to several months".
- **Self-promotional listicles are a risk here.** In Lily Ray's 100 "best B2B software" queries, AI Overviews cited 38% fewer self-ranking listicles in September 2026 than in June, and when one was cited, the brand it ranked first was left out of the answer 83% of the time. She also tied visibility losses of 29% to 49% at several SaaS brands in January 2026 to self-ranking listicles, scaled AI content and date-only refreshes (Google named no update). A vendor's own "best X, we're number one" page now tends to help its competitors.

## Clicks

- Organic CTR on queries with an AI Overview fell from 3.19% (January 2025) to 1.31% (December 2025), then partly recovered to 2.36% (February 2026), against 3.82% on queries with none (Seer Interactive, 53 brands, 5.47M queries). Paid CTR drops too, from 21% to 26% without an overview to 15% to 16% with one.
- Being cited in the overview roughly doubles organic CTR against not being cited on the same kind of query (2.07% against 0.94% on informational queries, Seer, 2025). Both stay below a query with no overview. Where the site is cited, the citation is the prize; where it is not, clicks lost to the overview do not come back by ranking higher.
- Pew's panel (900 US adults, March 2025): a regular result was clicked on 8% of visits with an AI summary against 15% without, and 1% clicked a link inside the summary. Google disputes the method.

## Measuring it

- Search Console counts AI Overview and AI Mode clicks inside `web` search with no filter. Its generative AI report (all sites from 31 August 2026) shows AI impressions by page, country and device, but no clicks and no queries, and Manifold's `console_*` tools cannot read it; the user can open it in Search Console. For clicks the only measurement is inference: compare the site's CTR on queries that show an overview against queries that do not, within the same position band (1 to 3, 4 to 10). Say it is an inference.
- `seo_get_serp` with `ai_overview: true` returns the overview's text and `references[]` beside the organic results, for 1 credit more than the SERP: the cheapest way to see, per keyword, whether an overview shows, whether the site is cited, and which domains are cited instead. `seo_get_keyword_metrics` with `ai_volume: true` adds how often AI engines get the same question.
- An overview shows for a query one day and not the next, and it differs by device: pass `device: "mobile"` when most of the site's clicks are mobile.
