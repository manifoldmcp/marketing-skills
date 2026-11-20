# Field notes

What Manifold has seen in its own runs of `aeo_run_ai_answers`, United States, English. These are small samples: read them as evidence of how the engines behave, not as market shares. Newest first.

## Same prompts, twice, a minute apart (15 October 2026)

Five prompts ("best CRM for startups", "best email marketing tool for a small online store", "best project management software for agencies", "best running shoes for flat feet", "best tool to track brand visibility in AI search") on all six engines, run twice back to back: 60 answers, 200 credits. Models reported: gpt-6 (ChatGPT app), claude-sonnet-5-5, Gemini 3.5 Flash-Lite (app), sonar-reasoning-pro.

**Brands are stabler than sources.**

| Engine | Brand overlap between runs (Jaccard) | Same brand first | Cited-domain overlap |
|---|---|---|---|
| ChatGPT | 0.75 | 4 of 5 | 0.48 |
| Claude | 0.76 | 4 of 5 | 0.54 |
| Gemini | 0.57 | 0 of 5 | 0.38 |
| Perplexity | 0.76 | 4 of 5 | 0.38 |
| AI Overview | 0.71 | 2 of 4 | 0.64 |
| AI Mode | 0.62 | 4 of 5 | 0.42 |

Overall the brand named first matched in 18 of 29 pairs (62%). On the AI visibility prompt every engine named the same four leaders (Profound, Peec AI, Semrush, Ahrefs Brand Radar), yet the first place flipped between runs on three engines. A stable core with a shuffled order, as SparkToro found at scale.

**Engines cite very different kinds of page.** Of 619 citations:

| Engine | Category brands' own sites | Editorial and publishers | YouTube | Reddit | Titles reading as lists |
|---|---|---|---|---|---|
| ChatGPT | 68% | 20% | 0 | 0 | 35% |
| Claude | 21% | 38% | 0 | 0 | 98% |
| Gemini | 17% | 23% | 0 | 0 | 78% |
| Perplexity | 71% | 12% | 0 | 0 | 47% |
| AI Overview | 26% | 14% | 20% | 4% | 80% |
| AI Mode | 23% | 17% | 15% | 17% | 81% |

- YouTube and Reddit appeared only in Google's AI Overview and AI Mode. Wikipedia never appeared; review sites once (one Capterra page).
- Listicles still dominate what Claude and Google's surfaces cite. The GPT-5.6 drop in listicle citations is a ChatGPT finding; ChatGPT here cited lists in about a third of its links.
- ChatGPT linked the named brand's own site for 96% of its mentions, through its product cards. Claude named the most brands (9.7 an answer) but linked a named brand's own site for only 7% of them.
- Perplexity piles up pages from one vendor: on the agency prompt, 14 of its 24 citations were teamwork.com's own pages. One vendor with deep, specific content on the question can own a Perplexity answer.
- The ChatGPT app's fan-out queries listed brands it already had in mind ("best CRM for startups 2026 HubSpot Pipedrive Zoho Salesforce pricing features") and used "official"; none used `site:`, unlike vendor reports from August. Claude's were the prompt plus "2026". Nearly every query carried the year.

**Product prompts break brand counting.** Gemini answered four of ten rows as a Google Shopping list of products (citations to google.com shopping results), which is why it never kept the same first brand. AI Overview and AI Mode put shoe names in a product carousel the text does not carry, so their brand counts on product prompts are undercounts. For a shop, read Gemini and Google's AI answers as shopping surfaces.

**Some Google answers do not change at all.** Three cells (AI Overview on CRM, AI Mode on CRM and agencies) came back byte-identical, while other cells on the same engines changed completely. Google appears to reuse answers for some queries.

**Data notes for this tool:** `named_brands[]`, `search_results[]` and `search_queries[]` come back only for the engines the vendor reports them on (see the `aeo_run_ai_answers` description). In this run ChatGPT tagged some category phrases as brands; the tool now drops a tag whose text is not in the answer. Pass `brands` and read `mentions[]` for the brands that matter.

## One question, six engines, seven days (7 to 13 October 2026)

"What is the best SEO MCP server?" once a day on ChatGPT, Claude, Gemini, Perplexity, AI Overview and AI Mode: 42 answers. Google Search Console MCP was named 41 times, Ahrefs 39, Semrush 36, DataForSEO 33, SE Ranking 28, Screaming Frog 16. Nearly every answer opened the same way: there is no single best, it depends on the stack and budget.

Google's AI Overview on "seo mcp" changed its sources on 5 of 7 days. On the last day every source that had held for six days dropped out at once, replaced by roundup listicles published or updated in the past month. A week of stability is not a guarantee of the next week.

## One prompt, four engines (6 October 2026)

"best CRM for startups" on ChatGPT, Gemini, Perplexity and AI Overview (Claude's engine did not answer that day). Each engine named a different set in a different order; HubSpot came first on all four; Pipedrive was named in three of four, third each time. Folk, Freshsales and Copper were tracked and never named.
