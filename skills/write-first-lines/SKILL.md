---
name: write-first-lines
description: When the user wants one personalized opening line per lead for cold emails. Gathers evidence from each lead's LinkedIn profile, their recent LinkedIn posts and their company's news, picks one hook per lead and writes a first line with its source and date, never an invented one. Also use when the user mentions personalized first lines, icebreakers, openers, personalize these cold emails, openers from their LinkedIn, or custom openers from recent posts. The rest of the email and follow-up sequences are not written here. Finding the leads goes to build-lead-list, a full brief on one account before a call to research-account, accounts with a reason to buy now to find-buying-signals.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Cold email first lines

One opening line per lead that could only have been written to that person, built on something real: a post they wrote, news from their company, or what they say about their own work. The tools gather the evidence; the host writes the lines from it. It ends in a table with one line per lead and the source behind it.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_get_profile` and `linkedin_search_posts` (hosts often add a prefix, for example `mcp__manifold__linkedin_get_profile`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so. Without it this skill cannot find a person from a name and domain.
- If the `linkedin_*` group is switched off, skip the steps that need it and say which signal is missing from the result.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (what the user sells, the problem it solves, the brand voice) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Leads**: a list with names and company domains, ideally LinkedIn URLs. The output of [build-lead-list](../build-lead-list/SKILL.md) works.
- **Offer**: what the user sells and the problem it solves, so each line can bridge to it.
- **Tone**: plain and peer to peer by default. Ask for one example of a line the user liked, if they have one.
- **How many**: default 25 leads. Research per lead is the cost; above about 200 a month, lines by segment beat lines by person.
- **Budget**: a default run of 25 leads with LinkedIn URLs costs about 25 x 2 + 15 x 1 = 65 credits (a profile and a post search per lead, one company feed per company). Add 3 for each lead with no LinkedIn URL, and 1 for each company whose LinkedIn page is unknown. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Get each LinkedIn URL.** From the list, or `leads_get_person` with `first_name`, `last_name` and `domain` (3 credits when matched, 1 on `NoData`), which returns the LinkedIn URL, `headline` and `company_linkedin_url`. Skip a lead with no match rather than guess a profile.
2. **Read the profile.** `linkedin_get_profile` with `url` (1 credit): `bio` is how they describe their work, the headline and summary a line can quote.
3. **Find their recent posts.** `linkedin_search_posts` with the person's full name as `query` and `since: "month"` (1 credit). Keep only rows whose `author` is the lead's profile handle; a common name returns namesakes. Read `text` and `created_at`. Most people post rarely, so expect posts for a minority of leads.
4. **Find company news.** `linkedin_get_company_posts` with the company's LinkedIn page as `url` (1 credit a page), once per company, shared by every lead there. The page URL is `company_linkedin_url` from step 1, a column in the list, or `linkedin_url` on `leads_get_company` (1 credit). Keep announcements from the last 60 days: a launch, a round, a new market, an award, a leadership hire.
5. **Pick one hook per lead**, strongest first: their own post from the last 30 days; company news from the last 60 days; something specific in their `bio` (a stated focus, a claim, a number); a recent move (their own post or `bio` saying they started the role in the last six months). If none exists, write "no hook" and let the line lead with the problem the role usually has. Never invent one.
6. **Write the lines.** The host writes them from the evidence: one sentence, under about 25 words, that names the specific thing and connects it to the problem the user solves. No flattery ("loved your post"), no "I noticed that", no claim the evidence does not support.
7. **Deliver** a table: name, company, first line, hook type (post, company news, profile, new role, no hook), source URL, source date. The user or their copy tool writes the rest of the email; do not send.

## Judgment

- A line that could go to anyone at the company is not personal. If it survives swapping the name, rewrite it or mark "no hook".
- Their own words beat their company's. A post they wrote last week is the best hook there is; a company press release is second.
- Dates on LinkedIn posts are approximate ("3 weeks ago"). Do not write "yesterday" or "this week" from them; "recently" is safe inside a month.
- Stay on work. Nothing from their private life, family or health, and nothing that shows how much was looked up about them; see the [personal data](../create-outbound-plan/references/lead-data.md#personal-data) rules.
- A post that asks for help with the problem the user solves is more than a first line: it is a reason to reach out now. Flag it for [find-buying-signals](../find-buying-signals/SKILL.md).
- A name search finds some of a person's posts, not all: no tool lists one person's feed. An empty result means none found, not none written.
- This is the one outbound skill that writes, and it writes one line per lead. Never send or sequence; see the [handoff rules](../create-outbound-plan/references/lead-data.md#handoff).

## Related skills

- The leads to write for: [build-lead-list](../build-lead-list/SKILL.md), or movers from [track-job-changes](../track-job-changes/SKILL.md).
- Leads whose posts show a reason to buy now: [find-buying-signals](../find-buying-signals/SKILL.md).
- One account researched in depth before a call: [research-account](../research-account/SKILL.md).
- When first lines are worth the credits, within an outbound plan: [create-outbound-plan](../create-outbound-plan/SKILL.md).
