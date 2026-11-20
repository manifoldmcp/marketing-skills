# Messaging

What reply rates to expect, what moves them, whether AI personalisation still works, and how to write a first line from evidence. Checked October 2026. Every large dataset here comes from a vendor with something to sell; none publishes a full method. Read them for direction and relative size, not as precise rates.

## Benchmarks

| Source | Data | Reply rate | Denominator |
|---|---|---|---|
| Instantly, 2026 report | "Billions" of 2025 sends, thousands of workspaces | 3.43% average; top quartile 5.5%+; top 10% about 10.7%+ | Per recipient, platform-wide |
| Hunter, State of Cold Email 2026 | 31 million emails sent by users in 2025 | 4.5% (up from 4.1% in 2024); sales outreach 3%, marketing 6.2%, headhunting 7.5%, digital PR 13% | Per recipient of a sequence |
| Belkins, 2026 study | 7,530,489 emails from its managed campaigns, 2025 | 0.45% | Per email sent, auto-replies excluded |

- **Ask for the denominator before comparing.** Per email sent counts every follow-up, so a four-step sequence at 0.45% per email can be about 2% per recipient. Saleshandy's 3.7% and Belkins' 0.45% are not a contradiction.
- **The user's sales outreach should be judged against about 3%, not 4.5%.** Hunter's average is lifted by digital PR and recruiting, which reply far more.
- **Reply is not positive reply.** Vendor rates include "not interested" and often out-of-office replies. Ask the user's sequencer for positive replies and meetings, which no Manifold tool can see.
- By seniority, Belkins saw founders and owners reply most (0.57% per email) and VPs least (0.32%).

## What moves replies

- **Smaller segments.** 21-50 recipients 6.2% against 500+ at 2.4%; one or two people per company 5.1% against 3+ at 3.5% (Hunter). The single largest lever in the data, and it is a list decision ([lists](lists.md)).
- **Length.** Instantly's best first emails ran under 80 words; Hunter found 61-80 word bodies among the best (and, oddly, 181-200). Short is the default; a long email needs a reason.
- **Steps.** 58% of replies come from the first email (Instantly). Three messages peaked at 6.8% against 3.3% for one, with four or more lower (Hunter). Yet Belkins' step 3 booked 35.6% of email-sourced meetings, more than steps 1 and 2 together. Three to four steps, three or four days apart, each with a new angle (a case, a number, a different pain), not "bumping this".
- **A follow-up written like a reply** beat a formal follow-up by about 30% (Instantly, no method).
- **One ask.** A single, easy question beats a calendar link in a first email (Instantly's top campaigns; consistent with Hunter's recipients, 65% of whom call cold email too pushy).
- **Subject lines:** 5 to 6 words did best; "quick question" did worse than average (2.5% against 2.9% replies, Hunter). Emojis, images, attachments and day of the week showed no meaningful effect in Hunter's data; Instantly saw Tuesday and Wednesday peak.
- **What recipients say fails:** too pushy (65%), irrelevant (61%), generic (48%), from Hunter's survey of US decision makers (sample size not published). 28% say they never get a cold email worth reading; 50.5% prefer to be approached on LinkedIn against 25% on email.

## Personalisation and AI

- **Personalisation helps, by tens of percent, not multiples.** Two custom fields in the body replied at 5.6% against 3.6% with none (Hunter; self-selected campaigns, not a test). The one randomised evidence is older: field experiments by Sahni, Wheeler and Chintagunta (Marketing Science, 2018) found a name in the subject line raised opens by 20% and sales leads by 31% (0.39% to 0.51%). Claims of 3x to 5x lifts are vendor marketing.
- **First name and company no longer count as personal.** Recipients read them as a mail merge; the gain comes from relevance: the segment's problem, a trigger, the person's own words.
- **Evidently AI-written email costs replies.** 69% of US decision makers are bothered when AI was clearly used to write a pitch, yet 67% are more likely to reply to personalisation built on public information (Hunter survey). Manually edited emails replied at 5.2% against 4.4% fully automated (Hunter). Vendor tests claiming AI-only copy is flagged as spam more often exist but publish too little method to quote.
- **So:** let AI gather the evidence and draft; have a person choose the angle and edit the line. Never send AI copy unread. Cut the tells: "I came across", "I noticed that", "hope this finds you well", flattery about a post, three-part lists, em dashes.
- **Above about 200 new contacts a month,** lines by segment beat lines by person: the research cost outruns the lift.

## First lines

One opening line per lead that could only have been written to that person, built on something real. The tools gather the evidence; the host writes.

- **Hooks, strongest first:** their own post in the last 30 days; company news in the last 60 (a launch, a round, a new market, a leadership hire); something specific in how they describe their work; a recent move (the role started in the last six months). If none exists, write "no hook" and lead with the problem the role usually has. Never invent one.
- **Where the evidence comes from:** `linkedin_get_profile` (1 credit) for `bio` and `recent_posts[]`; `linkedin_search_posts` with the person's full name and `since: "month"` (1 credit), keeping only rows whose `author` is their handle, because common names return namesakes; `linkedin_get_company_posts` once per company, shared by every lead there; `leads_get_person` (3 credits) when the LinkedIn URL is missing. About 65 credits for 25 leads with URLs.
- **A name search finds some of a person's posts, not all.** No tool lists one person's feed; an empty result means none found.
- **The line:** one sentence, under about 25 words, naming the specific thing and connecting it to the problem the user solves. If it survives swapping the name, it is not personal: rewrite it or mark "no hook".
- **Dates on LinkedIn are approximate** ("3 weeks ago"). Write "recently", never "yesterday" or "this week".
- **Stay on work.** Nothing from private life, family or health, and nothing that shows how much was looked up.
- **A post asking for help** with the problem the user solves is more than a hook: flag it as a [signal](signals.md).
- **Deliver** a table: name, company, first line, hook type, source URL, source date. Write the rest of the email only when the user asks.

## Without the connector

Benchmarks and rules above need no tools. First lines can be researched by hand from each lead's LinkedIn profile and company page; keep the source link and date beside every line so a person can check it before sending.
