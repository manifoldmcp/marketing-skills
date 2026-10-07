---
name: manifold-get-started
description: When the user is new to the Manifold marketing skills, asks where to start, or wants to set up or update the context every other skill reads first. Writes .agents/product-marketing.md (the business type, what it sells, the customers, competitors, positioning, customer language, proof, goals, accounts and what to track) for any kind of business, from B2B software to a shop, a local service, an app, a creator or a nonprofit, drafting from the site and asking only for the gaps. Then recommends three to five skills in the order to run them, with what each returns and costs. Also use when the user mentions getting started, I just installed this, which skill should I use, what can Manifold do, product or marketing context, describe my business, ICP, target audience, brand voice, or update our context after a pivot, a new competitor or a price change. A growth plan built on real data goes to create-growth-plan.
compatibility: Works without the Manifold MCP connector. With it, the draft also reads search data for the site at about 25 credits, asked first. The skills it recommends need the connector.
---

# Manifold get started

The first skill to run. It writes down once what every marketing task needs to know about the business, so no other skill has to ask again, and then picks the three to five skills that fit the user's goal from the whole library, so they do not have to read every name. Setting up the context costs nothing without the connector and about 25 credits with it; picking the skills runs no tools.

## The file

The context lives in the project at `.agents/product-marketing.md`, plain Markdown, the same path and sections other marketing skill libraries use, so one file serves them all. Every other skill reads it before its first question and asks only for what it lacks.

Sections, in this order. Keep each short: bullets, the user's own words, no filler. A section the user cannot answer yet says "Unknown" rather than a guess. The sections are the same for every business, but what each one asks depends on the business type: read them through [business-types.md](references/business-types.md).

1. **Product overview**: the business type, the one-line description of what it sells, the category buyers would put it in, how it makes money and the price points.
2. **Target audience**: who buys (a company, a person, both sides of a marketplace), who decides, and the main reason they buy.
3. **Personas**: each person involved in a purchase and what each one values.
4. **Problems and pain points**: the problem or the want, what it costs the buyer, and the emotional side.
5. **Competitive landscape**: direct competitors, secondary ones, and the indirect option (doing it yourself, a cheaper substitute, doing nothing).
6. **Differentiation**: the main advantages and why customers choose this business.
7. **Objections and anti-personas**: the top three objections with the answer to each, and who is not a fit.
8. **Switching dynamics**: push, pull, habit and anxiety, the four forces behind a switch.
9. **Customer language**: verbatim phrases customers use, with the source, and words to use and to avoid.
10. **Brand voice**: tone, style and personality, with one example line.
11. **Proof points**: numbers, customer names, reviews and testimonials the user can stand behind.
12. **Goals**: the business goal now, the conversion that counts (a purchase, a booking, a signup, a demo, a donation), the stage (idea, pre-launch, launched, growing) and the monthly marketing budget.
13. **Accounts and markets**: the site's domain, the Search Console property and Google Analytics property if any, each social account's handle or URL, store or app listings, the competitors' domains and handles, the countries, cities and languages that matter.
14. **Tracking set**: the 10 to 20 keywords and the AI prompts the business wants to win, with the date they were chosen.

The file opens with a version number and the date, and ends with a changelog, newest first: one line per change, with what changed and why.

## Steps

1. **Look for the file.** Read `.agents/product-marketing.md`; in older setups it is `.claude/product-marketing.md` or `product-marketing-context.md` at the project root. If an old path holds it, move it to `.agents/product-marketing.md` and say so.
   - If it exists and the user asked where to start, take the business, the goal, the stage, the accounts and the budget from it and go to step 7.
   - If it exists and the user wants it updated, show its sections in one line each, ask what changed, and go to step 6 with only those sections.
   - When another skill hands over its results, update only the sections they fill (see Related skills), show the change, and go to step 6.
   - If it does not exist, go on to step 2. If the user only wants a skill picked and declines the setup, go to step 7 and ask the questions there instead; the context can wait.
2. **Skip ahead when the job is clear.** If the request already names a job that one skill does ("audit our TikTok", "why did traffic drop"), name that skill, say what it costs from its Budget line, and offer to run it now and set up the context afterwards. Most skills ask what they need when the file is missing.
3. **Draft from what is already there.** Before asking anything, collect:
   - The conversation so far and any files the user shared.
   - In a codebase: the README, the landing page, pricing or product page copy, and any docs folder.
   - The site: the home, pricing or shop, and about pages through the host's browser or fetch tool, if it has one.
   - With the manifold tools, and only after telling the user the cost: `seo_get_page` on the home page (free) for its title and description; `seo_get_domain_overview` on the domain (5 credits) for the site's size in search; `seo_get_serp_competitors` on the domain (10 credits) for the sites that share its search results, as candidates for the competitive landscape; `seo_get_ranked_keywords` on the domain (10 credits) for the keywords it already ranks for, as candidates for the tracking set. If the tools are missing, skip this part and do not stop.
   From the draft, name the business type from [business-types.md](references/business-types.md) and ask the user to confirm it.
4. **Interview for the gaps.** Ask about the empty sections, three questions at a time at most, in the file's order, worded for the business type: "who signs the contract" for B2B software, "who is it for and when do they buy" for a shop, "which area do you serve" for a local business. Offer the draft's guess where there is one, so the user can confirm rather than write. Ask for real customer quotes and reviews: they are worth more than a polished summary.
5. **Check the draft.** Read the full draft back in one message and ask the user to correct it. Mark every line that came from the site or the tools rather than from the user, so they know what to check.
6. **Write the file.** Save it to `.agents/product-marketing.md`, creating the `.agents` folder if needed. Raise the version (1.0 for a new file, 1.1 and so on for an update), set the date, and add a changelog line. If the host cannot write files, give the user the whole file in one block to save themselves.
7. **Settle the goal.** Take what the file already answers. Ask the rest in one message, at most four questions, each with the default you will use:
   - The goal, from the map below (more Google traffic, AI answers, links and press, social growth, communities, creators, paid ads, leads, customer research, competitors, growth in general, monitoring, agency work). Default: the usual first goal for the business type, or growth in general.
   - The stage: idea, pre-launch, launched, or growing.
   - What is in place: the site, a connected Search Console or Google Analytics, the social accounts, an ad budget, the hours a week for marketing.
   - The credit budget for the first runs. Default: about 300 credits.
8. **Pick three to five skills** from the map for that goal, in order: a cheap reading of where the user stands first, then the plan, then the jobs the plan will call. Before you quote a cost, open each skill's SKILL.md and take the figure from its Budget line; do not guess it.
9. **Deliver** a short table: order, skill (as `/name`), why it fits this business, what it hands back, and the cost of a default run. Then offer to run the first one now.

## The map

| Goal | Start with | Then |
|---|---|---|
| Growth with no channel in mind | [create-growth-plan](../create-growth-plan/SKILL.md) | [pick-channels](../pick-channels/SKILL.md), [find-first-customers](../find-first-customers/SKILL.md), [create-gtm-plan](../create-gtm-plan/SKILL.md), [create-market-entry-plan](../create-market-entry-plan/SKILL.md), [measure-brand-awareness](../measure-brand-awareness/SKILL.md), [create-launch-plan](../create-launch-plan/SKILL.md) |
| More traffic from Google | [audit-technical-seo](../audit-technical-seo/SKILL.md) or, if traffic fell, [diagnose-traffic-drop](../diagnose-traffic-drop/SKILL.md) | [create-seo-plan](../create-seo-plan/SKILL.md), [find-seo-quick-wins](../find-seo-quick-wins/SKILL.md), [refresh-content](../refresh-content/SKILL.md), [create-seo-content-plan](../create-seo-content-plan/SKILL.md), [write-seo-brief](../write-seo-brief/SKILL.md), [optimize-page](../optimize-page/SKILL.md), [plan-comparison-pages](../plan-comparison-pages/SKILL.md), [fix-keyword-cannibalization](../fix-keyword-cannibalization/SKILL.md), [create-migration-plan](../create-migration-plan/SKILL.md) |
| Show up in AI answers | [check-ai-visibility](../check-ai-visibility/SKILL.md) and [check-ai-crawler-access](../check-ai-crawler-access/SKILL.md) (free) | [create-ai-search-plan](../create-ai-search-plan/SKILL.md), [build-ai-citations](../build-ai-citations/SKILL.md), [check-ai-overviews](../check-ai-overviews/SKILL.md), [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md), [find-best-of-lists](../find-best-of-lists/SKILL.md) |
| Links and press | [create-link-building-plan](../create-link-building-plan/SKILL.md) | [find-backlink-targets](../find-backlink-targets/SKILL.md), [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md), [reclaim-lost-links](../reclaim-lost-links/SKILL.md), [find-affiliate-partners](../find-affiliate-partners/SKILL.md), [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md), [find-journalists](../find-journalists/SKILL.md), [find-podcasts](../find-podcasts/SKILL.md) |
| Grow on one social platform | The platform's plan: [create-tiktok-plan](../create-tiktok-plan/SKILL.md), [create-instagram-plan](../create-instagram-plan/SKILL.md), [create-youtube-plan](../create-youtube-plan/SKILL.md), [create-linkedin-plan](../create-linkedin-plan/SKILL.md), [create-facebook-plan](../create-facebook-plan/SKILL.md) | Audits: [audit-tiktok-account](../audit-tiktok-account/SKILL.md), [audit-instagram-account](../audit-instagram-account/SKILL.md), [audit-youtube-channel](../audit-youtube-channel/SKILL.md), [audit-facebook-page](../audit-facebook-page/SKILL.md), [audit-linkedin-page](../audit-linkedin-page/SKILL.md), [audit-x-account](../audit-x-account/SKILL.md). Hooks and trends: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md), [find-instagram-hooks](../find-instagram-hooks/SKILL.md), [find-tiktok-trends](../find-tiktok-trends/SKILL.md), [find-reels-trends](../find-reels-trends/SKILL.md). Viral videos: [analyze-viral-tiktok](../analyze-viral-tiktok/SKILL.md), [analyze-viral-reel](../analyze-viral-reel/SKILL.md), [analyze-viral-youtube-video](../analyze-viral-youtube-video/SKILL.md) |
| Know what to post | [find-content-ideas](../find-content-ideas/SKILL.md) | [find-youtube-video-ideas](../find-youtube-video-ideas/SKILL.md), [find-linkedin-post-formats](../find-linkedin-post-formats/SKILL.md), [create-content-calendar](../create-content-calendar/SKILL.md), [repurpose-content](../repurpose-content/SKILL.md) |
| Show up in communities | [find-subreddits](../find-subreddits/SKILL.md) | [create-reddit-plan](../create-reddit-plan/SKILL.md), [find-reddit-threads](../find-reddit-threads/SKILL.md), [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md), [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md), [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md) |
| Work with creators | [create-influencer-plan](../create-influencer-plan/SKILL.md) | [find-creators](../find-creators/SKILL.md), [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-instagram-creators](../find-instagram-creators/SKILL.md), [find-youtube-creators](../find-youtube-creators/SKILL.md), [find-ugc-creators](../find-ugc-creators/SKILL.md), [find-brand-fans](../find-brand-fans/SKILL.md), [vet-creator](../vet-creator/SKILL.md), [write-creator-brief](../write-creator-brief/SKILL.md) |
| Paid ads | [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md) | [research-meta-ads](../research-meta-ads/SKILL.md), [research-google-ads](../research-google-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md), [write-ad-brief](../write-ad-brief/SKILL.md), [find-google-ads-keywords](../find-google-ads-keywords/SKILL.md), [check-google-brand-bidding](../check-google-brand-bidding/SKILL.md) |
| Leads and outbound | [size-market](../size-market/SKILL.md) | [create-outbound-plan](../create-outbound-plan/SKILL.md), [build-lead-list](../build-lead-list/SKILL.md), [find-lookalike-companies](../find-lookalike-companies/SKILL.md), [find-buying-signals](../find-buying-signals/SKILL.md), [track-job-changes](../track-job-changes/SKILL.md), [research-account](../research-account/SKILL.md), [write-first-lines](../write-first-lines/SKILL.md), [clean-email-list](../clean-email-list/SKILL.md), [enrich-lead-list](../enrich-lead-list/SKILL.md) |
| Understand customers | [find-pain-points](../find-pain-points/SKILL.md) | [check-demand](../check-demand/SKILL.md), [build-personas](../build-personas/SKILL.md), [find-objections](../find-objections/SKILL.md), [map-market](../map-market/SKILL.md), [find-reddit-pain-points](../find-reddit-pain-points/SKILL.md), [mine-facebook-groups](../mine-facebook-groups/SKILL.md), [mine-tiktok-comments](../mine-tiktok-comments/SKILL.md), [mine-instagram-comments](../mine-instagram-comments/SKILL.md), [mine-youtube-comments](../mine-youtube-comments/SKILL.md), [mine-facebook-comments](../mine-facebook-comments/SKILL.md) |
| Beat competitors | [find-competitors](../find-competitors/SKILL.md) | [create-competitor-plan](../create-competitor-plan/SKILL.md), [tear-down-competitor](../tear-down-competitor/SKILL.md), [compare-messaging](../compare-messaging/SKILL.md), [find-positioning](../find-positioning/SKILL.md), [write-battlecard](../write-battlecard/SKILL.md), [find-competitor-complaints](../find-competitor-complaints/SKILL.md) |
| Keep watch | [monitor-search-console](../monitor-search-console/SKILL.md) (free) | [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md), [monitor-competitors](../monitor-competitors/SKILL.md), [track-rankings](../track-rankings/SKILL.md), [write-weekly-report](../write-weekly-report/SKILL.md) |
| Agency work | [prepare-client-pitch](../prepare-client-pitch/SKILL.md) | [onboard-client](../onboard-client/SKILL.md), [write-client-report](../write-client-report/SKILL.md) |

## Judgment

- The user's words beat the draft's. Where the site and the user disagree, the user wins, and the gap is worth one line in the changelog: the site may need new copy.
- Never invent a metric, a customer, a quote or a review. A proof point the user cannot back stays out.
- Do not force a business into software terms. A bakery has no ICP or demo; write "regulars within two miles" and "a pre-order", in the words the owner uses.
- Competitors from `seo_get_serp_competitors` are sites that share search results; many are publishers or marketplaces, not rivals. List only the businesses the user confirms.
- Keep the file under about two pages. Each skill reads it in full before every task, so length costs in every task after this one.
- The file goes into the project and is easy to share or commit. Leave out anything private: revenue, customer contact details, internal names the user would not publish.
- One goal at a time. If the user names several, ask which matters most this month and plan for that one; name the next goal's first skill in one line.
- Free and cheap first: the context file, Search Console and AI crawler checks cost nothing, and a reading of where the user stands costs less than a plan built on guesses.
- Three to five skills, not more. A long list is the problem this skill exists to solve.
- Match the stage: before launch, demand and customer research come before SEO or ads; a site with traffic and Search Console starts from its own data.
- If the user cannot say what the goal is, recommend [create-growth-plan](../create-growth-plan/SKILL.md): it measures every channel and picks two or three.
- If the Manifold connector is missing, write the context without the search data, then say that every recommended skill needs the connector, and how to connect: https://www.manifoldmcp.com/docs/clients.

## Related skills

- Buyer personas, pain points and objections from evidence rather than from the user's memory. Their results fill sections 3 ([build-personas](../build-personas/SKILL.md); anti-personas go to 7), 4 ([find-pain-points](../find-pain-points/SKILL.md)), 7 ([find-objections](../find-objections/SKILL.md)), 8 ([find-competitor-complaints](../find-competitor-complaints/SKILL.md): push, pull, habit and anxiety), 9 (verbatim phrases from any of them, or from [find-reddit-threads](../find-reddit-threads/SKILL.md) and [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md)) and 14 ([check-demand](../check-demand/SKILL.md) keywords and prompts, dated).
- Who the real competitors are, and how to position against them: [find-competitors](../find-competitors/SKILL.md) fills sections 5 and 13 (the classes, domains and handles), and [find-positioning](../find-positioning/SKILL.md) fills 6.
- A growth plan built on real data across channels: [create-growth-plan](../create-growth-plan/SKILL.md).
