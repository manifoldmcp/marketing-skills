# How each engine picks sources

Each engine retrieves from a different index, through different crawlers, and changes on its own schedule. Advice that holds for one is often wrong for another, so plan and report per engine. Checked October 2026; the engines change this often, so re-check anything here older than six months against the vendor's own docs.

## At a glance

| Engine | Retrieves from | Crawlers that decide citation | Moves |
|---|---|---|---|
| ChatGPT | OpenAI's own crawl, Bing, Google results (indirectly), licensed publishers | OAI-SearchBot (index), ChatGPT-User (live fetch) | Model releases reshape it overnight (5.6, Aug 2026) |
| Claude | Brave's index, almost certainly (never named by Anthropic) | Claude-SearchBot (index), Claude-User (live fetch) | Steady; most consistent in repeat runs |
| Gemini | Google's index and Knowledge Graph | Googlebot | Leans to brand-owned sites on local and brand queries |
| Perplexity | Its own index, searched on every answer | PerplexityBot (index), Perplexity-User (live fetch) | Fastest to reflect a changed page |
| Google AI Overviews | Google's index, with query fan-out | Googlebot | Sources change between generations |
| Google AI Mode | Google's index, wider fan-out | Googlebot | Cites a mostly different URL set from AI Overviews |

## ChatGPT

- **Blocking OAI-SearchBot removes the site from ChatGPT search answers**, per [OpenAI's crawler docs](https://developers.openai.com/api/docs/bots); a robots.txt change takes about 24 hours. ChatGPT-User fetches pages a user's conversation asks for and may not follow robots.txt. GPTBot is training only. OAI-AdsBot visits only pages submitted as ads.
- **It is not just Bing.** OpenAI names Bing among its providers, but when Google removed the `num=100` results parameter in September 2025, ChatGPT's Reddit citations fell from about 60% of responses to about 10% (Semrush, 230K+ prompts). That only makes sense if Google results feed it too. Bing indexing is necessary, not sufficient.
- **Since GPT-5.6 (August 2026) it searches inside sites it already has in mind.** Fan-out queries with `site:` jumped from under 1% to 17% (Promptwatch) or more (Decageo claims about 90%; the vendors disagree badly, and our own October runs of the ChatGPT app showed none, see [field notes](field-notes.md)), with "official" rising and "best", "top", "vs" and "reviews" falling. The practical effect: ChatGPT decides which domains to check before it searches. A brand it does not already associate with the category is not searched for at all, which makes off-site mentions (see [citations](citations.md)) the lever.
- **Reddit citations collapsed in mid-August 2026**, by 73% to 95% depending on the tracker, on ChatGPT only. One tracker found Reddit threads were still about a quarter of the pages ChatGPT retrieved; it stopped showing them. Reddit may still shape the answer without appearing as a citation.
- **Ads are live** on the Free and Go tiers in the US since February 2026, and in more markets since August, as a labelled sponsored card below the answer. OpenAI says ads do not influence the answer. Paid tiers see none.
- **Products:** Instant Checkout was dropped in March 2026; purchases go to the merchant. Most product picks now come from integrated merchant feeds (about 62% to 65% from July 2026, Profound). For a shop, the product feed is the AI search work.

## Claude

- Three bots, and Anthropic says all three honour robots.txt, unlike OpenAI's and Perplexity's live fetchers: ClaudeBot (training), Claude-SearchBot (index for search answers), Claude-User (fetches for a user). [Anthropic's docs](https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler).
- Search almost certainly runs on Brave: Brave is on Anthropic's subprocessor list, and in one small test 87% of Claude's citations matched Brave's top results. For Claude, ranking in Brave matters more than ranking in Google. Whether submitting URLs to Brave helps is untested.
- In SparkToro's repeat-run study it was the most consistent of the engines tested.

## Gemini

- Retrieves from Google's index. Google-Extended controls Gemini training and grounding in the Gemini app and Vertex; it does not affect Google Search or its AI features.
- **Brand-owned sites: it depends on the query.** On local queries about 60% of Gemini's citations went to the business's own site (Steady Demand, 14,472 citations, Aug 2026); another dataset found Gemini the least likely engine to cite brand pages on category queries (14%). Owned pages win local, navigational and brand questions, not "best X" questions.

## Perplexity

- Searches on every answer and always cites. PerplexityBot builds its index and is not used for training. Perplexity-User "generally ignores robots.txt" ([Perplexity's docs](https://docs.perplexity.ai/guides/bots)); Cloudflare accused it of undeclared crawling in August 2025, which Perplexity disputes. Keeping a site out of Perplexity takes a block at the CDN, not robots.txt.
- G2 and review sites are top sources here more than on any other engine. Its Reddit citations rose while ChatGPT's fell.
- Perplexity has been phasing out ads since late 2025.

## Google AI Overviews and AI Mode

See [Google's AI answers](google-ai-overviews.md): rankings no longer predict citation the way they did, and the two surfaces cite different pages.

## Copilot and Bing

Bing Webmaster Tools' AI Performance report (public preview since February 2026) is the only first-party count of AI citations any engine offers: citations in Copilot and Bing's AI answers per page, with the grounding queries. No clicks and no API yet. If the user has Bing Webmaster Tools, it is free ground truth for one engine family.

## How Manifold's engines map to these

`aeo_run_ai_answers` asks `chatgpt` and `gemini` the way their consumer apps answer a person, model chosen by the app; `claude` and `perplexity` through their model APIs with web search on, which can differ from the apps; `ai_overview` and `ai_mode` from Google Search. Each row's `model` says which model answered. `search_queries[]` shows the fan-out queries for ChatGPT and Claude: read them to see which brands ChatGPT searches for by name. A brand absent from those queries is not in the engine's shortlist before it searches.
