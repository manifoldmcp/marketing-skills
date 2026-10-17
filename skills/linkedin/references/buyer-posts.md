# Buyer posts

People post on LinkedIn about the problem the user solves: asking for a recommendation, switching off or complaining about a competitor, describing the pain in their own words. Those posts are signals: a reason to help in public, and evidence of which pains resonate. Checked October 2026.

## Searching

Inputs: the problem in the buyer's words, five or six phrasings across four kinds (the pain, "chasing invoices"; asking, "recommend an invoicing tool", "looking for a tool that"; switching, "moving off <competitor>", "<competitor> alternative"; frustration, "<competitor> support", "hate doing <task>"); the ICP; two or three competitors; a window (default `since: "month"`, `since: "week"` for a fresh list). A default run is about 6 x 2 search pages + 25 profiles + a few posts, about 40 credits.

1. **Search the phrasings.** `linkedin_search_posts` for each, two pages.
2. **Sort each post** from its `text`: asking for a recommendation (hot), switching or complaining about a competitor (hot), describing the pain (warm), discussing the topic in general (drop), a vendor, consultant or recruiter marketing to the pain (drop; about half of a topic search in our October read, see [field notes](field-notes.md)). Keep the sentence that shows the signal.
3. **Qualify the authors.** `linkedin_get_profile` with the row's `author_url` on the hot and warm authors, up to 25: role, company type and market from the `bio`, `experience` and `location`. The profile has no headline or title; when the role decides it, `leads_get_person` (9 credits by profile URL) has the current title.
4. **Weigh the post.** `likes` and `comments` on the row. A complaint with many likes means many people share it: a theme for content even when its author is not a buyer.
5. **Read the replies.** `top_comments` names up to 10 people replying: others with the same problem, or competitors already answering. The user opens the post and reads the thread before acting.

Deliver a table: author, profile URL, role and company, post URL, posted (approximate), signal (asking, switching, complaining, pain), the quote, likes and comments, and a suggested first move: a helpful public comment, or a direct message from the user that answers the post, not a pitch.

## Judgment

- A public request for a recommendation is the strongest signal LinkedIn has, and it goes stale in days. Recent hot posts go first.
- People who complain in public want to be heard, not sold to. The first move is help in the thread; a pitch in reply to a complaint usually backfires. Never a link to the product or "great post".
- Posts with the problem but no fitting author still count: the wording and the likes show which pain resonates, which is material for the user's own posts (see [formats](formats.md)) and for [voice-of-customer](../../voice-of-customer/SKILL.md).
- Search is ranked and incomplete: a sample of who is talking, not everyone with the problem.
- Emails, company records and combining these posts with other buying signals belong to [outbound](../../outbound/SKILL.md). Do not look up emails here.
- Never post, comment, like, connect or message. A daily or weekly list is the host's schedule, keeping the post URLs already listed.
