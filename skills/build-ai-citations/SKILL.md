---
name: build-ai-citations
description: When the user wants AI engines to cite them where they now cite competitors. Finds the pages, Reddit threads and YouTube videos that ChatGPT, Perplexity, Gemini, Claude and Google AI Overviews cite for the category and not for the user, ranks them by the engines and prompts citing each, routes each by type and finds a person to contact where a person can change the page. Also use when the user mentions getting cited by ChatGPT, earning AI citations, which sources AI engines cite for our category, why Perplexity cites a competitor and not us, which Reddit threads ChatGPT cites, Reddit threads Google shows for our keywords, or which YouTube videos AI answers use. Measuring mention share goes to check-ai-visibility, list inclusion to find-best-of-lists, wrong facts to fix-wrong-ai-answers, and backlinks for Google rankings to find-backlink-targets.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Citation building

AI engines answer from sources they cite. When a competitor is named and the user is not, the sources in that answer are the reason. This skill finds those sources, ranks them by how many engines and prompts cite them, routes each one by type, and finds a person to contact where a person can change the page. It hands back a source table with the route and the ask for each; Reddit threads and YouTube videos get their own table from their reference.

It shares the contact mechanics with link building and nothing else: targets here are chosen by citations, not by domain rank, so the link building floors do not apply. A small site that ChatGPT cites often beats a large site it never cites.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `aeo_run_ai_answers` and `get_task` (hosts often add a prefix, for example `mcp__manifold__aeo_run_ai_answers`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- Citation building also needs the `leads_*` tools for contacts. If they are missing, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the source list without contacts.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the brand, the site, the competitors, the AI prompts and keywords in the tracking set, the proof points and data the user can offer a source) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Brand and competitors**: as in [check-ai-visibility](../check-ai-visibility/SKILL.md): the user first, then up to nine competitors, in `brands`.
- **Answers**: an check-ai-visibility run from this conversation or an earlier one (its `task_id`; results are kept 30 days). If there is none, run one first: its steps 1 and 2.
- **Sources to cover**: all of them (the default), or only Reddit threads or YouTube videos when the user asks about one.
- **What the user can offer a source**: data, a quote, a product trial, an updated fact, a sponsorship budget. The ask depends on it.
- **Budget**: with an existing run, about 15 x 8 + 15 = 135 credits for contacts at 15 sources, plus 90 to widen the source table from the prompt index in step 2. Reddit threads add about 55 (ten SERPs, fifteen threads read with their comments, the rules of five communities); YouTube videos about 75 (ten Google queries, their video searches and 15 videos read). Without a run, add the check-ai-visibility check (about 270). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the answers.** `get_task` with the check-ai-visibility run's `task_id` (free). Leave out rows whose `answer` is null. Keep the rows where at least one competitor is mentioned or cited and the user is neither. When the user asked only about Reddit or YouTube, go straight to that reference in step 4.
2. **Build the source table.** From those rows, list every citation by URL. For each, count the engines that cite it and the prompts it is cited for, across all rows, and note which competitors those rows mention. Drop the user's own domain and any source already citing the user in other rows (it knows the user; a nudge may do, but it is not a gap). Ten prompts are a small sample: when fewer than about ten sources reach step 3's bar, `aeo_search_prompts` with the category `keyword` and `limit: 200` (90 credits) adds `cited_domains[]` across the indexed prompts, and a domain cited across many of them is a target even if the live run missed it.
3. **Rank.** Order the sources by engines citing, then prompts citing. A source cited by two or more engines, or for two or more prompts, is a target. A source cited once is noise unless it is the only source for a prompt that matters.
4. **Route each source by type.** Read the URL, the domain and the citation `title`; when the type is unclear, `seo_get_page` on the URL (free) gives the title and headings. Then:
   - **Editorial page** (an article or guide on a publisher or blog): a contact target, step 5.
   - **Best-of list** (a title with best, top, alternatives or compared, on a publisher): [find-best-of-lists](../find-best-of-lists/SKILL.md), for inclusion.
   - **Reddit thread**: open [Reddit threads](references/reddit-threads.md); replies there follow the subreddit's rules, never outreach.
   - **YouTube video**: open [YouTube videos](references/youtube-videos.md), which sizes and vets the creator.
   - **Review site or directory** (G2, Capterra, Trustpilot, Product Hunt, AlternativeTo, Clutch and the like): no manifold tool covers these. Put them on a checklist: claim the profile, complete it, ask customers for reviews.
   - **A competitor's own page** (their comparison or alternatives page): the answer is the user's own page on the same question, through [plan-comparison-pages](../plan-comparison-pages/SKILL.md).
   - **Wikipedia or a reference site**: checklist only. Do not suggest editing a page about the user's own company.
5. **Find the contacts.** Run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on the editorial domains, with `limit: 10`, skipping the link building floors. When the citation names an author you know, use its step 2 for that person.
6. **Deliver** a table: source URL, type, engines citing, prompts citing, competitors in those answers, route (the skill, the reference or the checklist), contact, role, email, verification status, and the ask: what the page is missing that the user can supply (a mention in a list of tools, an updated fact, a data point). Add the Reddit and YouTube tables from their references when those sources came up.

## Judgment

- Rank by citations, never by domain rank. The engines have already said which pages they trust for these prompts.
- Answers are live and non-deterministic, and citation sets move between runs. A source cited by several engines is stable; a source one engine cited once may be gone next week.
- The ask is to make the page more complete or more correct for its readers, with evidence. Do not offer payment for a mention; a paid placement that the page does not disclose is a risk to the user.
- `ai_overview` and `ai_mode` citations follow Google's rankings closely, so ranking work through [create-seo-plan](../create-seo-plan/SKILL.md) helps there too.
- Some prompts have no gap to close: every engine names the user. Report them as held, and spend nothing on them.
- After the sources change, re-run the same prompt set to measure it. Changes reach engines at different speeds; re-measure after two to four weeks, not days, or on a schedule with [check-ai-visibility](../check-ai-visibility/SKILL.md).
- Never post, send or edit third-party pages. The deliverable is a table the user acts on.

## Related skills

- Where the brand stands in AI answers, once or tracked: [check-ai-visibility](../check-ai-visibility/SKILL.md). The plan around it: [create-ai-search-plan](../create-ai-search-plan/SKILL.md).
- Lists the engines cite: [find-best-of-lists](../find-best-of-lists/SKILL.md). Wrong facts in the answers: [fix-wrong-ai-answers](../fix-wrong-ai-answers/SKILL.md).
- Replying in Reddit threads: [find-reddit-threads](../find-reddit-threads/SKILL.md) and [find-subreddits](../find-subreddits/SKILL.md). The creators behind cited videos: [find-creators](../find-creators/SKILL.md) and [vet-creator](../vet-creator/SKILL.md).
- Links for Google rankings rather than citations: [find-backlink-targets](../find-backlink-targets/SKILL.md).
