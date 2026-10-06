---
name: create-product-context
description: When the user wants to set up or update the product marketing context that every other marketing skill reads first, so they stop answering the same questions in each task. Writes .agents/product-marketing.md with the product, the ideal customer, personas, pain points, competitors, differentiation, objections, customer language, brand voice, proof points and goals, plus the site, the social accounts and the keywords and prompts to track. Drafts it from the site, the codebase and the manifold tools, then asks only for the gaps. Also use when the user mentions product context, marketing context, set up context, describe my product, who is my target audience, ICP, ideal customer profile, positioning basics, brand voice, or update our context after a pivot, a new competitor or a pricing change. Deep research on customers or competitors goes to customer-research or competitor-research.
compatibility: Works without the Manifold MCP connector. With it, the draft also reads search data for the site's size, competitors and keywords, at about 25 credits, asked first.
---

# Product context

One file that holds what every marketing task needs to know about the product, so no skill has to ask again. Every other skill in this library reads it before its first question and asks only for what it lacks. The file is plain Markdown in the project at `.agents/product-marketing.md`, the same path and sections other marketing skill libraries use, so one file serves them all.

## The file

Sections, in this order. Keep each short: bullets, the user's own words, no filler. A section the user cannot answer yet says "Unknown" rather than a guess.

1. **Product overview**: the one-line description, the category buyers would put it in, the business model and the price points.
2. **Target audience**: the kind of company or person who buys, the decision makers, the main use case.
3. **Personas**: each role involved in a purchase and what each one values.
4. **Problems and pain points**: the core problem, what it costs the buyer, and the emotional side.
5. **Competitive landscape**: direct competitors, secondary ones, and the indirect option (a spreadsheet, an agency, doing nothing).
6. **Differentiation**: the main advantages and why customers choose the product.
7. **Objections and anti-personas**: the top three objections with the answer to each, and who is not a fit.
8. **Switching dynamics**: push, pull, habit and anxiety, the four forces behind a switch.
9. **Customer language**: verbatim phrases customers use, with the source, and words to use and to avoid.
10. **Brand voice**: tone, style and personality, with one example line.
11. **Proof points**: metrics, customer names and testimonials the user can stand behind.
12. **Goals**: the business goal now, the conversion that counts (signup, demo, purchase) and the budget per month for marketing.
13. **Accounts and markets**: the site's domain, the Search Console property if any, each social account's handle or URL, the competitors' domains and handles, the countries and languages that matter.
14. **Tracking set**: the 10 to 20 keywords and the AI prompts the business wants to win, with the date they were chosen.

The file opens with a version number and the date, and ends with a changelog, newest first: one line per change, with what changed and why.

## Steps

1. **Look for the file.** Read `.agents/product-marketing.md`; in older setups the file is `.claude/product-marketing.md` or `product-marketing-context.md` at the project root. If one exists, show the user its sections in one line each and ask what changed; then go to step 5 with only those sections. When another skill hands over its results, update only the sections they fill (see Related skills) and show the change before writing. If an old path holds it, move it to `.agents/product-marketing.md` and say so.
2. **Draft from what is already there.** Before asking anything, collect:
   - The conversation so far and any files the user shared.
   - In a codebase: the README, the landing page and pricing page copy, and any docs folder.
   - The site: the home, pricing and about pages through the host's browser or fetch tool, if it has one.
   - With the manifold tools, and only after telling the user the cost: `seo_get_page` on the home page (free) for its title and description; `seo_get_domain_overview` on the domain (5 credits) for the site's size in search; `seo_get_serp_competitors` on the domain (10 credits) for the sites that share its search results, as candidates for the competitive landscape; `seo_get_ranked_keywords` on the domain (10 credits) for the keywords it already ranks for, as candidates for the tracking set. If the tools are missing, skip this part and do not stop.
3. **Interview for the gaps.** Ask about the empty sections, three questions at a time at most, in the file's order. Offer the draft's guess where there is one, so the user can confirm rather than write. Ask for real customer quotes: they are worth more than a polished summary.
4. **Check the draft.** Read the full draft back in one message and ask the user to correct it. Mark every line that came from the site or the tools rather than from the user, so they know what to check.
5. **Write the file.** Save it to `.agents/product-marketing.md`, creating the `.agents` folder if needed. Raise the version (1.0 for a new file, 1.1 and so on for an update), set the date, and add a changelog line. If the host cannot write files, give the user the whole file in one block to save themselves.
6. **Hand back.** Say in one line that every marketing skill now reads this file first, and name the one or two skills that fit the user's goal from section 12.

## Judgment

- The user's words beat the draft's. Where the site and the user disagree, the user wins, and the gap is worth one line in the changelog: the site may need new copy.
- Never invent a metric, a customer, a quote or a testimonial. A proof point the user cannot back stays out.
- Competitors from `seo_get_serp_competitors` are sites that share search results; many are publishers or marketplaces, not rivals. List only the businesses the user confirms.
- Keep the file under about two pages. Each skill reads it in full before every task, so length costs in every task after this one.
- The file goes into the project and is easy to share or commit. Leave out anything private: revenue, customer contact details, internal names the user would not publish.

## Related skills

- Buyer personas, pain points and objections from evidence rather than from the user's memory. Their results fill sections 3 ([build-personas](../build-personas/SKILL.md); anti-personas go to 7), 4 ([find-pain-points](../find-pain-points/SKILL.md)), 7 ([find-objections](../find-objections/SKILL.md)), 8 ([find-competitor-complaints](../find-competitor-complaints/SKILL.md): push, pull, habit and anxiety), 9 (verbatim phrases from any of them, or from [find-reddit-threads](../find-reddit-threads/SKILL.md) and [find-linkedin-buyer-posts](../find-linkedin-buyer-posts/SKILL.md)) and 14 ([check-demand](../check-demand/SKILL.md) keywords and prompts, dated).
- Who the real competitors are, and how to position against them: [find-competitors](../find-competitors/SKILL.md) fills sections 5 and 13 (the classes, domains and handles), and [find-positioning](../find-positioning/SKILL.md) fills 6.
- A plan once the context is set: [create-growth-plan](../create-growth-plan/SKILL.md).
