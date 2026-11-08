# Competitor engagers

Who comments on a competitor's LinkedIn posts, sorted into the people the user can act on: the rival's customers, its partners and agencies, and buyers weighing it up. This gives a rough view of the rival's deal flow from public data. Checked November 2026.

## What LinkedIn shows

- **Commenters, never reactors.** LinkedIn shows a logged-out reader up to 10 comments on a post and no list of who reacted. Every tool that returns more runs on logged-in accounts, which Manifold does not use. Tell a user who asks for reactors that no tool here reads them, and use commenters instead.
- **Fewer than the count.** The returned commenters fall short of the post's `comments` count, which also counts replies: 26 returned against 40 counted across six posts in our read ([field notes](field-notes.md)). Report a sample, not the thread.
- **The company feed has no commenters.** `linkedin_get_company_posts` rows carry empty `top_comments` and no engagement, so each post needs `linkedin_get_post` (1 credit) for its commenters and counts. `linkedin_get_comments` returns the same people again; do not pay for both.

## Steps

Inputs: one to three competitors' company pages; optionally their founders' and sales leaders' profile URLs; a window of 20 to 30 posts per competitor (about a month on an active page). One competitor costs about 4 feed pages + 30 posts + 20 profiles, about 55 credits, before any title lookups.

1. **Collect the posts.** `linkedin_get_company_posts` with the page's handle, paging until the window is covered (6 to 9 posts a page). For founders and sales leaders, `linkedin_get_profile` gives their latest five or six posts in `recent_posts`; their posts often draw more buyers than the company page.
2. **Sort the posts by type first, from `text`**, because the type predicts who comments:
   - **Customer story or case study** (names a customer, results, "thanks to the team at"): the commenters are mostly the customer's own team. This is the strongest deal-flow signal, and the post names the account.
   - **Webinar, event or co-marketing**: the commenters are co-hosts, partner agencies and the rival's community. These are partners, not buyers.
   - **Product, data or opinion post**: vendors selling to the same audience, consultants and generic commenters. Buyers asking a real question turn up here.
   - **Culture, hiring or office**: the rival's own staff. Drop these posts.
3. **Pull the commenters.** `linkedin_get_post` on each kept post; keep each commenter's name, `author_url`, comment text and the post it came from.
4. **Remove duplicates and count across posts.** A name on three or more posts deserves a look, but check who it is before treating it as a lead: over a short window the repeat names are mostly staff and automated commenters (both repeats in our six-post read looked like one or the other).
5. **Classify each person**, with `linkedin_get_profile` on the ones who are not obvious (up to 20; `experience` gives the current company, usually without a title):
   - **Rival's staff**: current company is the competitor. Drop.
   - **Customer**: works at a company the post names as a customer, or the comment says "our team", "we use", "working with you". Note the account.
   - **Partner or agency**: co-hosts, implementation partners, agencies that resell or build on the rival.
   - **Vendor or self-promoter**: a company page, or a comment that pitches its own product or data ("the same approach works on…").
   - **Generic or automated**: a one-line or AI-sounding summary of the post that could sit under any post ([commenting](commenting.md) covers how LinkedIn treats these). Drop.
   - **Prospect**: works in the ICP, asks a buying question ("does this work for…", "how does it compare to…"), or comments with no tie to the rival.
6. **Add titles only where they decide the next step.** `leads_get_person` (9 credits by profile URL) has the current title and company. Emails and outreach belong to [outbound](../../outbound/SKILL.md).

Deliver a table grouped by class (customers first, then prospects, then partners): name, profile URL, company, class, the post and its type, the comment quote, how many of the rival's posts they commented on, and a suggested next move. Under it, list the accounts the rival's customer stories name, even those whose team did not comment.

## Judgment

- **A rival's customer is a long sales cycle, not a hot lead.** Someone who just praised the rival in public is unlikely to switch this quarter. The value is knowing the rival's accounts and segments, and having a list to approach at renewal or when the rival slips. Hot leads come from people asking for a tool ([buyer posts](buyer-posts.md)) or from complaints ([competitor-intel](../../competitor-intel/SKILL.md)).
- **Prospects asking questions under a rival's post are the fastest move.** They have the problem and have not chosen yet. The first move is a useful answer from the user's own account, never a pitch in the rival's thread.
- **Partners and agencies are channel targets.** An agency that builds on the rival may build on the user's product too.
- **Repeat engagement needs a month or more.** Over one week, repeat counting finds staff. Run 20 to 30 posts, or run it weekly and keep the earlier tables, so a person who keeps showing up stands out.
- **Never contact anyone from here.** Deliver the table; the user decides. Never mention to a person that they were found through their comment on a competitor's post.
- A weekly watch of the rival's whole marketing goes to [competitor-intel](../../competitor-intel/SKILL.md).
