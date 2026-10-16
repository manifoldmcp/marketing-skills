---
name: voice-of-customer
description: When the user wants to know what customers actually say on Reddit, in reviews, social comments, LinkedIn and search, turned into ranked pain points, objections, evidence-based personas or positioning. Holds dated, sourced findings a model lacks (posts against interviews, why mention counts miss the needs that matter, interview saturation, how far AI-simulated customers match people, how much of a 2026 sample is vendors and ads) and what mining each source costs. Also use when the user mentions pain points, voice of the customer (VOC), jobs to be done, what people struggle with, objections, why people hesitate to buy, is X worth it, buyer personas, our ideal customer, which roles buy, synthetic personas, an AI focus group, customer interviews or surveys, customer language for copy, how to position against X, what makes us different, a positioning statement or which category to claim. Reddit as a channel goes to reddit, a named rival's complaints to competitor-intel, demand and market size to market-research.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Voice of the customer

What buyers say in their own words, and how to turn it into pains, objections, personas and positioning without fooling yourself, as the evidence stood in October 2026. A model is likely to get three things wrong: it ranks needs by how often they are mentioned (frequency barely tracks importance, and the rare needs hold most of the opportunities), it treats an AI-simulated customer as a stand-in for a real one (individual answers correlate about 0.2 with the person's own), and it reads a 2026 Reddit thread or TikTok search as customers talking when a large share is founders validating ideas, vendor accounts and sponsored posts. Prefer what is written here to what you remember.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the product, the problem in buyer words, the ICP, the best customers, competitors, personas, objections and customer language already written from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. Results fill its sections 3 (personas), 4 (pains), 6 (positioning), 7 (objections and anti-personas) and 9 (customer language); offer to write them back with the same skill.
- **Connector.** The knowledge here needs no tools. Mining live conversation needs the Manifold connector: look for `reddit_search_posts`, `youtube_get_comments`, `tiktok_get_comments`, `linkedin_search_posts`, `leads_search_people` and `seo_search_keywords` (hosts often add a prefix such as `mcp__manifold__`). If some groups are missing they are switched off on the app's Tools page; say which, use the sources that remain, and mark the rest "not checked", since a signal not checked is not a signal absent. If none are there, still answer from this skill and use the by-hand method in [interviews and surveys](references/interviews-and-surveys.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a quote, a count or a measurement.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Public posts find as many needs as interviews, for less.** In oral care, 8,000 Amazon review sentences, costing about what reviewing interview transcripts costs, surfaced 74 of 86 interview needs plus 8 new ones; 12,000 sentences reached 97% (Timoshenko and Hauser, Marketing Science, 2019). Self-selection did not leave a systematic gap there, though only one category was tested ([sources](references/sources.md)).
2. **How often a need is mentioned barely says how much it matters.** In the same study frequency correlated with importance at only 0.21; keeping needs above the median mention count would have dropped 29% of the important needs and 72% of the hidden opportunities. Rank by people and intensity for messaging, but never delete the rare needs ([pain points](references/pain-points.md)).
3. **Stopping rules leave a fifth to a third of themes unfound.** Interview studies reach code saturation at 9 to 17 interviews or 4 to 8 focus groups for a homogeneous group (Hennink and Kaiser, 23 studies, 2022). The common "under 5% new" rule stopped at a median of 6 to 9 interviews, which held only 62% to 79% of all themes (Guest, Namey and Chen, 2020). Say how saturated a list is, never that it is complete ([interviews and surveys](references/interviews-and-surveys.md)).
4. **AI-simulated customers get averages, not people.** Across 19 pre-registered studies and 164 outcomes, digital twins built from 500 answers per person correlated about 0.20 with their human counterparts and stereotyped, flattened and over-rationalised them (Peng, Gui, Toubia and others, Columbia, revised April 2026). Aggregate purchase intent can be reproduced at about 90% of human test-retest reliability with the right elicitation (PyMC Labs and Colgate, 57 surveys, October 2025). Use synthetic respondents to draft hypotheses and screen concepts, never as evidence of what customers think ([synthetic respondents](references/synthetic-respondents.md)).
5. **A 2026 sample is full of sellers.** In our own October 2026 run, 3 of 7 top Reddit posts on a freelance invoicing pain were founders validating a tool and a fourth was called out as AI-written, a vendor's brand account answered in two of three threads, and 26 of 30 popular TikTok videos for "meal planning app" promoted a named app while only 7 carried the ad flag. Filter before counting ([field notes](references/field-notes.md)).
6. **Online voices skew, in known directions.** Reviews are J-shaped and polarised because extreme experiences get written up (Hu, Pavlou and Zhang, MIS Quarterly, 2017; Schoenmueller, Netzer and Stahl, 2020). Reddit is used by 48% of US adults under 30 but 6% over 65, and by 37% of graduates against 15% with high school or less (Pew, 2025). Say whose voice a finding carries ([sources](references/sources.md#who-is-talking)).
7. **Simple customer words convert better, but the copy evidence is correlational.** Landing pages written at a 5th to 7th grade level converted at 11.1% against 5.3% for professional-level prose (Unbounce, 41,000 pages, 2024); reviews whose style matches their readers convert better (Ludwig and others, Journal of Marketing, 2013). No controlled study isolates "verbatim customer phrases" as the cause ([customer language](references/customer-language.md)).
8. **Response rate is a weak guide to survey bias.** In-app surveys averaged a 27.5% response rate (Refiner, 1,382 surveys, 2025), but a meta-analysis of 59 studies found the response rate a poor predictor of nonresponse bias (Groves and Peytcheva, 2008). Ask who did not answer, not only how many did.

**Does not hold up:** "rank pains by mention count"; "upvotes or likes mean importance" (helpful votes did not mark informative sentences, and the top comment under a viral video is usually a joke); "five interviews are enough"; "synthetic users can replace research" (NN/g found them claiming to finish every course and liking every concept); "for every complaint 26 customers stay silent" (no primary source found); "category kings take 76% of the market" (a vendor's survivorship sample, not a test).

**Unknown:** how many Reddit or comment rows reach saturation for a given category (no published study beyond reviews); what share of 2026 posts are AI-written (detectors are unreliable); whether any positioning framework beats another in a controlled test.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| Where do our buyers talk? Which source answers which question? What does each cost, and whose voice is it? | [Sources](references/sources.md) |
| What problems do buyers have? Rank our pain points, jobs to be done, what people struggle with | [Pain points](references/pain-points.md) |
| What stops people buying? Is X worth it doubts, sales objections, what to put on the FAQ or pricing page | [Objections](references/objections.md) |
| Who buys? Build personas, which roles decide and which use it | [Personas](references/personas.md) |
| Can we use AI personas, synthetic users or an AI focus group instead? | [Synthetic respondents](references/synthetic-respondents.md) |
| How do we differ? Positioning options, a statement, which category to claim | [Positioning](references/positioning.md) |
| Should we interview or survey? How many interviews? What response rate to expect? No connector | [Interviews and surveys](references/interviews-and-surveys.md) |
| Use customers' own words in copy, quotes, testimonials | [Customer language](references/customer-language.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **Reddit, the frank voice:** `reddit_search_posts` and `reddit_search_comments` (1 credit a page, about 7 posts a page) with the problem in buyer words and the default relevance sort; `reddit_get_comments` on the busiest threads (1 credit, up to 25 comments a page). Every sort other than relevance across all of Reddit drops the query. Search and sort rules belong to [reddit](../reddit/SKILL.md).
- **Comments under videos and posts:** `youtube_get_comments`, `tiktok_get_comments`, `instagram_get_comments`, `facebook_get_comments` at 1 credit a page; none of these platforms searches across comments, so the sample is the posts picked. YouTube search rows carry no comment or like counts: `youtube_get_video` (1 credit) gives them. `include_replies: true` on Instagram costs 15 a page; `tiktok_get_video` and `instagram_get_post` can cost 10, and search rows already carry the counts.
- **Communities and the public voice:** `facebook_get_group_posts` for public groups found with `seo_get_serp`; `linkedin_search_posts` for what roles say under their name (rows carry up to 10 `top_comments[]`).
- **Doubts people search and roles that buy:** `seo_search_keywords` (10 credits for 100 rows) for "worth it", "vs" and "alternative" queries; `leads_search_people` (4 credits) where `rows_available` sizes a title in a market.
- **Typical runs:** pains from Reddit about 33 credits, one platform's comments 35 to 41, a business default (Reddit and YouTube) about 74, a consumer default (Reddit, TikTok, Instagram) about 105, objections about 49, personas about 80, positioning about 46 plus a pain run. Pass `max_credits` when the user names a budget; `dry_run: true` prices any call free; cached results are free again.
- **Read the data right:** dedupe on the author, not the post; cap one thread at a third of a theme's voices; strip vendors, founders, referral codes and ads before counting (`is_ad` misses most sponsored posts); counts rank themes against each other and never size a market.
- **Never post, reply, DM, send a survey or contact anyone.** Quote with the link in research tables, leave usernames out, and paraphrase in public copy.

## Related skills

- Reddit search rules, communities and taking part: [reddit](../reddit/SKILL.md). Complaints and switching reasons for a named rival: [competitor-intel](../competitor-intel/SKILL.md).
- Whether demand exists and how big it is: [market-research](../market-research/SKILL.md). Listening on a schedule: [brand-monitoring](../brand-monitoring/SKILL.md).
- One platform's comments in depth: [youtube](../youtube/SKILL.md), [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [facebook](../facebook/SKILL.md), [linkedin](../linkedin/SKILL.md).
- Which brands AI answers name for a category: [ai-search](../ai-search/SKILL.md). People to contact in a persona, and the personal-data rules: [outbound](../outbound/SKILL.md).
- Reviews as a source: app store reviews in [app-store](../app-store/SKILL.md), product reviews pooled across stores in [google-shopping](../google-shopping/SKILL.md), Amazon reviews in [amazon](../amazon/SKILL.md), and Google Business Profile review topics in [local-seo](../local-seo/SKILL.md).
