# First lines

One opening line per lead that could only have been written to that person, built on something real: a post they wrote, news from their company, or what they say about their own work. The tools gather the evidence; the host writes the lines from it. It ends in a table with one line per lead and the source behind it.

## Inputs to settle first

- **Leads**: a list with names and company domains, ideally LinkedIn URLs. The output of the [lead list](lead-list.md) works.
- **Offer**: what the user sells and the problem it solves, so each line can bridge to it.
- **Tone**: plain and peer to peer by default. Ask for one example of a line the user liked, if they have one.
- **How many**: default 25 leads. Research per lead is the cost; above about 200 a month, lines by segment beat lines by person.
- **Budget**: a default run of 25 leads with LinkedIn URLs costs about 25 x 2 + 15 x 1 = 65 credits (a profile and a post search per lead, one company feed per company). Add 10 for each lead with no LinkedIn URL, and 10 for each company whose LinkedIn page is unknown. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Get each LinkedIn URL.** From the list, or `leads_get_person` with `first_name`, `last_name` and `domain` (10 credits, 1 on `NoData`), which returns the LinkedIn URL, `headline`, `company_linkedin_url` and `employment_history`. Skip a lead with no match rather than guess a profile.
2. **Read the profile.** `linkedin_get_profile` with `url` (1 credit): `bio` is how they describe their work, the headline and summary a line can quote.
3. **Find their recent posts.** `linkedin_search_posts` with the person's full name as `query` and `since: "month"` (1 credit). Keep only rows whose `author` is the lead's profile handle; a common name returns namesakes. Read `text` and `created_at`. Most people post rarely, so expect posts for a minority of leads.
4. **Find company news.** `linkedin_get_company_posts` with the company's LinkedIn page as `url` (1 credit a page), once per company, shared by every lead there. The page URL is `company_linkedin_url` from step 1, a column in the list, or the LinkedIn page URL on `leads_get_company` (10 credits). Keep announcements from the last 60 days: a launch, a round, a new market, an award, a leadership hire.
5. **Pick one hook per lead**, strongest first: their own post from the last 30 days; company news from the last 60 days; something specific in their `bio` (a stated focus, a claim, a number); a recent move (a current role in `employment_history` with a `start` in the last six months). If none exists, write "no hook" and let the line lead with the problem the role usually has. Never invent one.
6. **Write the lines.** The host writes them from the evidence: one sentence, under about 25 words, that names the specific thing and connects it to the problem the user solves. No flattery ("loved your post"), no "I noticed that", no claim the evidence does not support.
7. **Deliver** a table: name, company, first line, hook type (post, company news, profile, new role, no hook), source URL, source date. The user or their copy tool writes the rest of the email; do not send.

## Judgment

- A line that could go to anyone at the company is not personal. If it survives swapping the name, rewrite it or mark "no hook".
- Their own words beat their company's. A post they wrote last week is the best hook there is; a company press release is second.
- Dates on LinkedIn posts are approximate ("3 weeks ago"). Do not write "yesterday" or "this week" from them; "recently" is safe inside a month.
- Stay on work. Nothing from their private life, family or health, and nothing that shows how much was looked up about them.
- A post that asks for help with the problem the user solves is more than a first line: it is a reason to reach out now. Flag it for [buying intent](buying-intent.md).
- A name search finds some of a person's posts, not all: no tool lists one person's feed. An empty result means none found, not none written.
