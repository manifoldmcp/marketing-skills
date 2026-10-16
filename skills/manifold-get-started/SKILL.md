---
name: manifold-get-started
description: When the user is new to the Manifold marketing skills, asks where to start, or wants to set up or update the context every other skill reads first. Writes .agents/product-marketing.md (the business type, what it sells, the customers, competitors, positioning, customer language, proof, goals, accounts and what to track) for any kind of business, from B2B software to a shop, a local service, an app, a creator or a nonprofit, drafting from the site and asking only for the gaps. Then recommends the two or three skills that fit the goal, and the first job to do in each, with what it costs. Also use when the user mentions getting started, I just installed this, which skill should I use, what can Manifold do, product or marketing context, describe my business, ICP, target audience, brand voice, or update our context after a pivot, a new competitor or a price change. A growth plan built on real data goes to growth-strategy.
compatibility: Works without the Manifold MCP connector. With it, the draft also reads search data for the site at about 25 credits, asked first.
---

# Manifold get started

The first skill to run. It writes down once what every marketing task needs to know about the business, so no other skill has to ask again, and then picks the skills that fit the user's goal and the first job in each. Each skill covers one area of marketing (AI search, SEO, Reddit, outbound and so on) and holds what changed in that area since models were trained. Setting up the context costs nothing without the connector and about 25 credits with it; picking the skills runs no tools.

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
2. **Skip ahead when the job is clear.** If the request already names a job in one area ("audit our TikTok", "why did traffic drop"), name that area's skill, say what the job costs from the skill's Using Manifold section, and offer to do it now and set up the context afterwards. Most skills ask what they need when the file is missing.
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
8. **Pick two or three skills** from the map for that goal, and the first job in each, in order: a cheap reading of where the user stands first, then the plan. Before you quote a cost, open each skill's SKILL.md and take the figure from its Using Manifold section or the reference for that job; do not guess it.
9. **Deliver** a short table: order, skill (as `/name`), the first job in it, why it fits this business, what it hands back, and its cost. Then offer to start the first one now.

## The map

Start with the skill in the middle column; its References table names the job to do first. Bring in the others when the plan needs them.

| Goal | Start with | Then |
|---|---|---|
| Growth with no channel in mind | [growth-strategy](../growth-strategy/SKILL.md) | [market-research](../market-research/SKILL.md), [voice-of-customer](../voice-of-customer/SKILL.md) |
| More traffic from Google | [seo](../seo/SKILL.md), or [technical-seo](../technical-seo/SKILL.md) if traffic fell or pages are not indexed | [link-building](../link-building/SKILL.md), [ai-search](../ai-search/SKILL.md) |
| Show up in AI answers | [ai-search](../ai-search/SKILL.md) | [seo](../seo/SKILL.md), [digital-pr](../digital-pr/SKILL.md), [reddit](../reddit/SKILL.md) |
| Links and press | [link-building](../link-building/SKILL.md), [digital-pr](../digital-pr/SKILL.md) | [seo](../seo/SKILL.md), [ai-search](../ai-search/SKILL.md) |
| Grow on one social platform | The platform's skill: [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [youtube](../youtube/SKILL.md), [linkedin](../linkedin/SKILL.md), [facebook](../facebook/SKILL.md), [x-twitter](../x-twitter/SKILL.md) | [creators](../creators/SKILL.md), and [growth-strategy](../growth-strategy/SKILL.md) for calendars and repurposing |
| Show up in communities | [reddit](../reddit/SKILL.md) | [linkedin](../linkedin/SKILL.md), [facebook](../facebook/SKILL.md) for groups |
| Work with creators | [creators](../creators/SKILL.md) | [tiktok](../tiktok/SKILL.md), [instagram](../instagram/SKILL.md), [youtube](../youtube/SKILL.md), [paid-ads](../paid-ads/SKILL.md) |
| A local business: Maps, reviews, nearby customers | [local-seo](../local-seo/SKILL.md) | [voice-of-customer](../voice-of-customer/SKILL.md), [facebook](../facebook/SKILL.md) for local groups |
| Selling products online | [google-shopping](../google-shopping/SKILL.md), or [amazon](../amazon/SKILL.md) for an Amazon seller | [paid-ads](../paid-ads/SKILL.md), [voice-of-customer](../voice-of-customer/SKILL.md) for reviews |
| An app in the App Store or Google Play | [app-store](../app-store/SKILL.md) | [competitor-intel](../competitor-intel/SKILL.md), [paid-ads](../paid-ads/SKILL.md) |
| Paid ads | [paid-ads](../paid-ads/SKILL.md) | [competitor-intel](../competitor-intel/SKILL.md), [creators](../creators/SKILL.md) |
| Leads and outbound | [outbound](../outbound/SKILL.md) | [market-research](../market-research/SKILL.md) for how many fit the ICP, [linkedin](../linkedin/SKILL.md) |
| Understand customers or a market | [voice-of-customer](../voice-of-customer/SKILL.md), [market-research](../market-research/SKILL.md) | [reddit](../reddit/SKILL.md), [competitor-intel](../competitor-intel/SKILL.md) |
| Beat competitors | [competitor-intel](../competitor-intel/SKILL.md) | [paid-ads](../paid-ads/SKILL.md), [voice-of-customer](../voice-of-customer/SKILL.md) |
| Keep watch | [brand-monitoring](../brand-monitoring/SKILL.md) | [seo](../seo/SKILL.md) for rankings and Search Console, [competitor-intel](../competitor-intel/SKILL.md), [ai-search](../ai-search/SKILL.md) for AI answer tracking |
| Agency work | [agency](../agency/SKILL.md) | The channel skills for the client's goal |

## Judgment

- The user's words beat the draft's. Where the site and the user disagree, the user wins, and the gap is worth one line in the changelog: the site may need new copy.
- Never invent a metric, a customer, a quote or a review. A proof point the user cannot back stays out.
- Do not force a business into software terms. A bakery has no ICP or demo; write "regulars within two miles" and "a pre-order", in the words the owner uses.
- Competitors from `seo_get_serp_competitors` are sites that share search results; many are publishers or marketplaces, not rivals. List only the businesses the user confirms.
- Keep the file under about two pages. Each skill reads it in full before every task, so length costs in every task after this one.
- The file goes into the project and is easy to share or commit. Leave out anything private: revenue, customer contact details, internal names the user would not publish.
- One goal at a time. If the user names several, ask which matters most this month and plan for that one; name the next goal's first skill in one line.
- Free and cheap first: the context file, Search Console and AI crawler checks cost nothing, and a reading of where the user stands costs less than a plan built on guesses.
- Two or three skills, not more. A long list is the problem this skill exists to solve.
- Match the stage: before launch, demand and customer research come before SEO or ads; a site with traffic and Search Console starts from its own data.
- If the user cannot say what the goal is, recommend [growth-strategy](../growth-strategy/SKILL.md): it compares the channels and picks two or three.
- If the Manifold connector is missing, write the context without the search data. The skills still give their knowledge without it; say that measuring anything live needs the connector, and how to connect: https://www.manifoldmcp.com/docs/clients.

## Related skills

- Buyer personas, pain points, objections, switching forces and customer language from evidence rather than memory: [voice-of-customer](../voice-of-customer/SKILL.md). Its results fill sections 3, 4, 7, 8 and 9; keywords and AI prompts worth tracking, dated, fill 14 ([market-research](../market-research/SKILL.md) for demand, [ai-search](../ai-search/SKILL.md) for prompts).
- Who the real competitors are, and how to position against them: [competitor-intel](../competitor-intel/SKILL.md) fills sections 5 and 13, and [voice-of-customer](../voice-of-customer/SKILL.md) fills 6.
- A growth plan built on real data across channels: [growth-strategy](../growth-strategy/SKILL.md).
