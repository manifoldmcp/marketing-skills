---
name: app-store
description: When the user wants more installs from Apple's App Store or Google Play search, or research on apps in a category. Holds dated, sourced findings a model lacks (what each store indexes, App Store tags, custom product pages in search, AI review summaries, more Apple Ads slots since March 2026, conversion benchmarks, rating prompt rules, why store volumes are models) and how to read Manifold's store data. Also use when the user mentions ASO, app store optimization, App Store keywords, the keyword field, app title or subtitle, Play Store description, app screenshots or icon tests, product page optimization, store listing experiments, competitor apps, app reviews, replying to reviews, raising an app rating, top charts, Apple Search Ads and organic, apps in ChatGPT, finding app developers to contact, or an ASO plan. Web search goes to seo, AI answers in general to ai-search, rivals beyond the store to competitor-intel, review themes to voice-of-customer, ad budgets to paid-ads.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# App store

How apps get found and installed on Apple's App Store and Google Play, as the evidence stood in October 2026, and how to read store data without overclaiming. A model's memory of app store optimisation is mostly 2023 and is now wrong in places: Apple generates search tags from listings with an LLM, custom product pages rank in organic search, both stores write an AI summary of the reviews above them, Play answers questions about an app with Gemini, and App Store search carries several ads since March 2026. It is also likely to quote third-party keyword volumes as searches; no store publishes counts. Prefer what is written here to what you remember, and re-check any figure older than six months before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the app, its store links and ids, the platforms, the category, the ICP, competitors and the words customers use from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. Ask the user for what no public tool shows: the App Store keyword field, App Store Connect or Play Console analytics, and Apple Ads data.
- **Connector.** The knowledge here needs no tools. Reading listings, store keywords, competitors, reviews and charts needs the Manifold connector: look for `apps_get_app`, `apps_get_keywords`, `apps_get_competitors` and `apps_get_reviews` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand method in [keywords](references/keywords.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. If some tool groups are there and others are not, they are switched off on the app's Tools page: say which, and mark those rows "not checked". Never invent a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Apple indexes 160 characters, Play indexes the description.** Apple ranks on the name (30), subtitle (30), keyword field (100) and categories, plus downloads, ratings and reviews; promotional text and the description do not count (Apple's search docs, 2026), and practitioner tests show words combining across the three fields. Play has no keyword field: title (30), short description (80) and full description (4,000) carry the terms, and Google calls keyword lists and repetition a policy violation ([stores](references/stores.md)).
2. **Store search volumes are models.** Apple publishes only a 5 to 100 popularity index in Apple Ads; Google publishes nothing; every tool's volume, Manifold's included, is an estimate with an undocumented method and no independent accuracy test. Rank terms against each other in one source and one store; never quote one as searches. In our test the App Store gave "habit" 980 and the Play figures came in steps of 10, two scales that do not compare ([keywords](references/keywords.md)).
3. **Apple's ranking surface grew in 2025.** LLM-generated App Store tags (US, from the metadata, deselect only); custom product pages that replace the default page in organic search for keyword-field terms (from July 2025, 70 per app from October 2025). Screenshot captions as a ranking factor is unproven: in ConsultMyApp's test of 64 caption phrases on 8 apps, one ranked without other metadata to explain it.
4. **AI now sits between the search and the install.** Apple's review summaries (April 2025), Play's "Users are saying" (October 2025), Ask Play answering from the description, reviews and website (from December 2025), Guided Search, and Gemini recommending apps outside Play (July 2026). A recurring complaint gets written into the paragraph every visitor reads; vague copy produces vague answers ([reviews](references/reviews.md)).
5. **Organic first place sits lower now.** Apple added ad slots below the top of search results from March 2026 (UK and Japan on 3 March, other markets by month end), and App Store Connect counts Apple Ads installs inside the search source. Split paid from organic before reading a trend. No study has measured the organic loss ([Apple Ads](references/apple-ads.md)).
6. **Conversion benchmarks differ by denominator.** US App Store search installs per impression ran from 2.6% (Games) to 12.4% (Food & Drink) in August 2026 (AppFollow); page view to install averaged 8.56% on the App Store and 16.15% on Play in 2025 (AppTweak). The user's own peer percentiles in App Store Connect beat any published table ([listing audit](references/listing-audit.md)).
7. **The rating prompt is the system's, and it is rationed.** Apple shows it at most three times in 365 days to someone who has not rated and disallows custom prompts (5.6.1); Play forbids any question before the prompt and any reward. Replies help: Google reported +0.7 stars on average for users who got a reply (2019), yet only 24.5% of 92.9M reviews got one (AppFollow, 20,700 apps, July 2025 to July 2026).
8. **Tests need traffic.** Apple's product page optimisation runs up to three treatments for up to 90 days at a 90% confidence label; Play's experiments target retained first-time installers on a 90% interval. A small app rarely reaches significance: test bold changes or call it a before-and-after.
9. **Store listings feed AI answers about apps, mostly when the platform is named.** Listings were 47.5% of ChatGPT's citations on app prompts (AppTweak, 125K+ US responses, May 2026, vendor). In our six-engine run they were 11% overall, nearly all on "for iPhone" and "for Android" prompts; lists, many written by app makers, were two-thirds of citations ([AI answers](references/ai-answers.md)).

**Does not hold up:** "the App Store description is indexed"; "repeat the keyword in the name and keyword field for weight" (a word counts once); "competitor names in the keyword field" (rejection risk under 2.3.7); "screenshot captions are the new keyword field"; "ask if they like the app, then prompt only the happy ones" (banned on Play); "an in-app event lifts downloads 50%" (one 2021 client).

**Unknown:** the weights of each field on either store; whether App Store tags change ranking or only browse; how much Apple's extra ad slots cut organic installs; whether paid installs lift organic rank (the halo) by any measured amount; how DataForSEO derives store volumes.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| How does each store rank apps? What is indexed? What changed in 2025 and 2026? | [Stores](references/stores.md) |
| Which keywords should we target? Title, subtitle, keyword field, Play description wording | [Keywords](references/keywords.md) |
| Audit our listing. Screenshots, icon, conversion benchmarks, A/B tests | [Listing audit](references/listing-audit.md) |
| Who competes with our app? Compare us with rivals, top charts, watch competitors | [Competitors](references/competitors.md) |
| What do users complain about? Reply to reviews, raise the rating, prompt rules | [Reviews](references/reviews.md) |
| Does Apple Ads help or hurt organic? Brand bidding, popularity scores | [Apple Ads](references/apple-ads.md) |
| Does ChatGPT recommend our app? Ask Play, apps in AI answers | [AI answers](references/ai-answers.md) |
| Find app makers in a category to contact | [App makers](references/app-makers.md) |
| We want a 90-day ASO plan | [Strategy](references/strategy.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

Tested live on both stores on 16 October 2026 ([field notes](references/field-notes.md#store-tools-on-habit-trackers-16-october-2026)). A repeat call inside the cache window came back free.

- **One app:** `apps_get_app` (1 credit, cached a day) with `store` and `app_id` (a Play package name, case sensitive, or the App Store number from the URL) and `location` for the storefront. On Play `installs` is an exact count (8,369,496), not the 5M+ bucket the store shows, and the developer email and address are filled; Play leaves `content_rating` and `size` empty and often `version`. The App Store sends no developer website, email or address (open the page for the website), `category` is sometimes null, and `description` starts with the promotional text when there is one. A free app's `price` is 0, and an App Store app with no subtitle has `subtitle` null. No keyword field, events or icon.
- **Keywords and competitors:** `apps_get_keywords`, `apps_get_competitors` and `apps_get_shared_keywords` (5 plus 5 per 100 rows, 10 by default, cached 7 days). US English only; keyword data was two to four weeks old. `apps_get_keywords` now puts the app's own name first (best rank, then volume). App Store lists carry irrelevant terms that share a word with the name; Play lists read like web searches with volumes in steps of 10. `results_count` counts at most about 250 apps on the App Store and is empty on Play: only a figure well under 250 says something (a thin search). `apps_get_shared_keywords` returns only terms every app ranks for: gaps come from comparing keyword lists. Competitors are ids only, giants included; name them with `apps_get_app`.
- **Reviews:** `apps_get_reviews` (1 credit per 100, cached 6 hours), newest or most relevant, per `location`; at most 500 on the App Store. `developer_reply` and `helpful_count` come back on Play only, `title` on the App Store only. `ratings_count` counts star ratings, not reviews you can fetch; `rows_available` is set only once a call reaches the end. A hundred newest reviews covered 16 months of a 27,000-rating iOS app and 35 days of a 63,000-rating Play app; Play's most relevant reach back years.
- **Charts and lists:** `apps_get_top_charts` (1 per 100 rows; up to 100 on the App Store, 200 on Play), with `category` as the store's id in lower case with underscores (`health_and_fitness`, `food_and_drink`; the tool description lists them, and a name such as "Health & Fitness" is converted). It ranks velocity or revenue, not quality; Play rows lack `ratings_count`, App Store rows lack `developer`. `apps_search_apps` (31 plus 31 per 100 rows, 33 for five) matches any word of the query in titles and returns the biggest apps first, so "habit tracker" brought back period, transit and flight trackers. Search one distinctive word, with `category`.
- **AI answers about apps:** `aeo_run_ai_answers`, 27 credits a prompt on five engines ([AI answers](references/ai-answers.md)).
- **Credits:** say the estimate first, price with `dry_run: true`, and pass `max_credits` when the user sets a budget. A refused input (a wrong category id) costs nothing. A one-store baseline is about 70 credits.
- **Never post a reply, submit a listing, contact a developer or buy reviews.** Draft; the user acts.

## Related skills

- Web search, Search Console and keyword data for the app's website: [seo](../seo/SKILL.md). Being named in ChatGPT, Gemini and Google's AI answers in general: [ai-search](../ai-search/SKILL.md).
- Rivals beyond the store (traffic, ads, pricing, battlecards): [competitor-intel](../competitor-intel/SKILL.md). Review themes turned into pains, objections and positioning: [voice-of-customer](../voice-of-customer/SKILL.md).
- Paid budgets across Meta, Google, TikTok and more: [paid-ads](../paid-ads/SKILL.md). Creators for app installs: [creators](../creators/SKILL.md), [tiktok](../tiktok/SKILL.md).
- Market size and new countries for the app: [market-research](../market-research/SKILL.md). Contacting app makers, and the personal-data rules: [outbound](../outbound/SKILL.md). A growth plan beyond the stores: [growth-strategy](../growth-strategy/SKILL.md).
