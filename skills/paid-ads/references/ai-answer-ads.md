# Ads in AI answers

Paid placement inside ChatGPT and inside Google's AI Overviews and AI Mode, which a model trained before 2026 will not know exists. Checked October 2026. Winning the unpaid answer is a different job: [ai-search](../../ai-search/SKILL.md).

## ChatGPT

- **Timeline.** OpenAI announced ads on 16 January 2026 and began a US test on 9 February 2026 for logged-in adults on the Free and Go ($8 a month) tiers. A self-serve Ads Manager in beta, CPC bidding and more measurement followed on 5 May 2026; international markets from May. Similarweb tracked ads in nine markets by September 2026 (US, UK, Canada, Australia, New Zealand, Japan, South Korea, Brazil, Mexico). Amazon began selling ChatGPT inventory through its DSP to select US advertisers on 10 September 2026 (Digiday, as reported).
- **Format.** A labelled sponsored unit below the answer, chosen from the conversation's topic, past chats and past ad interactions. OpenAI says ads do not influence the answer, and shows none in conversations about health, mental health or politics, or to under-18 accounts. Advertisers see aggregate views and clicks, never conversations.
- **Who can be reached.** Plus, Pro, Business, Enterprise and Education users see no ads. For B2B this matters: a buyer using a company seat is out of reach, so the audience is individuals, small companies and people choosing tools for themselves. Check the user's buyers against that before testing.
- **Price, as reported.** The pilot sold at about a $60 CPM with commitments reported at $200,000 to $250,000; by April vendors reported about $25 CPMs and a $50,000 minimum; with self-serve, recommended CPC bids of $3 to $5 and no published minimum. None of this is an OpenAI rate card; quote it as reported.
- **Results, as measured.** Similarweb and Graphite (September 2026; a user panel plus 95,000+ US ads from 500+ advertisers): all-time CTR 0.73% in the US, 0.74% in the UK, up to 1.91% in Brazil; CTR falls with each turn of a conversation (about 0.66% on the first turn, under 0.5% after the fourth). 26% of US desktop responses carried an ad in June, and 43% of ad-eligible US users had seen one by September. Against Google Search's 6.64% CTR, that is about a ninth.
- **Measurement is thin.** Early advertisers reported dashboards that could not prove ROI; OpenAI added third-party measurement only gradually. OpenAI reported a $100 million annualised run rate within about six weeks of launch (The Information, via Reuters and CNBC), which says advertisers are buying, not that it pays.
- **When to test:** a consumer or self-serve product whose buyers ask ChatGPT for recommendations, with a budget the user can afford to learn on, and conversion tracking in place first. Not as a first channel.
- **No Manifold tool reads ChatGPT's ads.** There is no public ChatGPT ad library. `aeo_run_ai_answers` reads the unpaid answer only.

## Google AI Overviews and AI Mode

- **AI Overviews.** Ads can show above, below or inside an overview. Above and below: every market with AI Overviews, from Search, Shopping, Performance Max and App campaigns. Inside: text and Shopping ads from Search, Shopping and Performance Max, English only, in the US, Australia, Canada, India, Indonesia, Kenya, Malaysia, New Zealand, Nigeria, Pakistan, the Philippines and Singapore (Google Ads Help, October 2026). Never shown inside overviews for adult, alcohol, gambling, finance, healthcare or politics queries, among others.
- **No opt-out, no report.** Advertisers cannot target or exclude the overview placement, and Google Ads gives no segmented reporting; ads inside an overview are counted as top ads.
- **AI Mode.** Ads have run in AI Mode in the US since 2025. At Google Marketing Live (20 May 2026) Google announced formats built by Gemini for the query (conversational ads that answer the user's question, "highlighted answers" inside recommendation lists, a lead agent that answers from the advertiser's site) and wider Direct Offers (discounts and bundles Gemini assembles, launched January 2026). Eligibility runs through AI Max for Search or Performance Max with text customisation; an exact-match-only Search account is not eligible.
- **Clicks.** Paid CTR on a results page with an AI Overview is about 15% to 16% against 21% to 26% without one (Seer Interactive, 2025): the overview takes clicks from ads too.
- **Seeing it with Manifold:** `seo_get_serp` with `ai_overview: true` shows whether an overview appears for a keyword and what it cites. It does not reliably show ads (see [field notes](field-notes.md)).

## Elsewhere

- Perplexity has been phasing out ads since late 2025. Microsoft Copilot shows ads from Microsoft Advertising campaigns; no independent performance data has been published.

## Unknown

- Whether ads in AI answers are incremental to the same advertiser's search ads, or take the same clicks at a new price. Nobody has published a holdout test.
- How ChatGPT chooses between advertisers beyond bid and topic, and whether conversation history targeting lifts results.
