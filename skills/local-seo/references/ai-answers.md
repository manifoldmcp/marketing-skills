# Local businesses in AI answers

Which sources ChatGPT, Gemini, Google's AI Overviews and AI Mode use when they recommend a local business, how few businesses they name, and how to measure it. Checked October 2026. The general mechanics of AI answers (engines, noise, margins of error) belong to [ai-search](../../ai-search/SKILL.md); this page holds what is different for local.

## How often people ask

45% of US adults said they had used ChatGPT or another AI tool to find a local business recommendation, up from 6% a year earlier; 82% read AI review summaries (BrightLocal, 1,002 US adults, February 2026; self-reported). Google's own surfaces still carry most local search.

## What each engine reads

- **Google AI Overviews and AI Mode lean on the Business Profile.** Across 1,967,762 citations for 1,355 locations in the US, UK and Australia (five prompts at nine points per location), the Business Profile was 28.6% of all citations and Yelp 9.5%, the largest non-Google source; Facebook and Tripadvisor followed on Google's engines (BrightLocal, August 2026). AI Mode drew on the most sources (81,817 domains). In Steady Demand's 7,990-query study (vendor, August 2026), 79.8% of AI Mode's local-intent citations went to Google Maps listings and 19.7% to business sites, while AI Overviews sent 73.5% of their local citations to business sites.
- **ChatGPT leans on Yelp and directories.** In the same BrightLocal data Yelp appeared in about 80% of ChatGPT's local answers, followed by Tripadvisor, OpenTable, BBB and Angi; Apple's listings were cited only about 600 times. In our runs the ChatGPT app cited best-of lists (Expertise, Best Pick Reports, a local awards site) for "best plumber in Austin" and retrieved health directories (Zocdoc, Healthgrades, WebMD, Yellow Pages) for "dentist near me" ([field notes](field-notes.md)). OpenAI has not said where its local map cards come from.
- **Gemini answers from Google Maps.** In our runs every Gemini citation was a Google Maps listing (16 of 16, three cities, plus 20 of 20 in October tests recorded in the [facebook](../../facebook/SKILL.md) skill). For a "near me" prompt at a ZIP code it named the three businesses in that ZIP's local pack plus two more, all within about 3 miles, with distances. For Gemini the profile is the page.
- **Brand-owned pages win local and brand questions.** Yext (6.8M citations, ChatGPT, Gemini and Perplexity, October 2025; vendor) found 86% of local citations came from sources brands control, 44% websites and 42% listings, with Reddit-style forums at 2% once location and intent were applied. A business site with a page per service and location is cited for unbranded objective questions; listings and reviews for "best" and branded ones.

## Few are named

- ChatGPT recommended 1.2% of about 350,000 locations of 2,751 multi-location brands, against a 35.9% appearance rate in the local 3-pack; the locations it recommended averaged 4.3 stars (SOCi Local Visibility Index, January 2026; method not published).
- 83% of restaurants tracked by Local Falcon never appeared in ChatGPT results, against 14% invisible in Google's local results (189,905 ChatGPT results, February 2026; tracked customers only). Even a 4.8-star restaurant with 1,005 reviews was missing from over half of ChatGPT's answers.
- **AI picks follow prominence more than the pack does.** In our Austin run no engine named any of the three plumbers in Google's local pack; AI Mode named businesses with 1,700 to 18,000 reviews, and one company appeared on all three engines. For "best X in town", reviews volume, rating and being on local best-of lists decide; for "X near me", Gemini and AI Mode stayed close to the pack.

## AI Overviews mostly stay off local searches

On "service in town" searches the local pack still dominates: AI Overviews appeared on 15% of local-intent queries against 92% of informational ones (Whitespark, 540 queries, three cities, May 2025) and on 21.8% against 75.8% (Steady Demand, August 2026). The overview shows on the questions around the hire: costs, how to choose, what a problem means. In our test the one overview among 12 local checks answered a pipe-repair cost question and cited a local plumber's own published price guide beside Angi. A local business wins those overviews with a plain, specific page on prices and process for its town; that page is [seo](../../seo/SKILL.md) work.

## What to do about it

- **The profile, complete and current**, for Gemini and AI Mode ([Business Profile](business-profile.md)).
- **Yelp and the trade's main directory**, claimed and consistent, for ChatGPT. Check which directories the engines cite for the user's own prompts before claiming dozens.
- **Local best-of lists and press** (the survey's top AI factor): find the lists the engines cite in the user's answers, then pursue inclusion as [digital-pr](../../digital-pr/SKILL.md) describes. Never pay for an undisclosed placement.
- **A page per service and per location on the site**, stating services, areas, prices and hours in plain text.
- **Reviews,** in volume and recent ([reviews](reviews.md)).

## Measuring it

- `aeo_run_ai_answers` with three to five prompts: two "best service in town" prompts, one "service near me", one problem prompt ("my boiler is making a banging noise, who can fix it in Leeds"), and one brand prompt ("is Acme Plumbing any good"). On chatgpt, gemini, ai_overview and ai_mode that is 8 credits a prompt; claude and perplexity add 21 and are less used for local.
- **Put the town in the prompt** and keep `location` a country for ChatGPT, Claude and Perplexity. A city `location` failed twice on ChatGPT in our runs (reported as "rate limited", which it was not), and the Claude and Perplexity engines take only a country, so a city reaches them as no location at all.
- **For "near me" prompts, set `location` to the town or ZIP and ask only gemini, ai_mode and ai_overview.** With `location` "United States" a near-me prompt was answered for Quincy, Massachusetts (ChatGPT) and rural Colorado (Gemini, AI Mode): a national location makes near-me answers meaningless.
- **Read `mentions[]` for the user and named competitors and the answer text for everyone else;** `named_brands[]` is ChatGPT only. AI Mode's `citations[]` were Google search viewer links (domain google.com), not the underlying sources, so count AI Mode by names in the answer.
- **Treat one run as a sample.** Answers change between runs ([ai-search](../../ai-search/SKILL.md) has the margins); a local answer also changes with the point it is asked from. Report how often the business is named across prompts and engines, and re-measure two to four weeks after a change.

## Without the connector

Ask each engine the prompts in a private window with location services on, from the town (or with the town in the prompt), and log who is named and what is cited, with the date. Say that a handful of hand-run answers cannot separate a real gap from noise.
