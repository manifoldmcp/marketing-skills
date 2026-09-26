# Problem posts

People posting on LinkedIn about the problem the user solves: asking for a recommendation, complaining about a tool, describing the pain in their own words. It ends in a table of posts and authors with the signal each shows, for the user to engage in public or to hand to the `leads` group for contact details.

## Inputs to settle first

- **The problem**: in the buyer's words, not the product's. Five or six phrasings across four kinds: the pain ("chasing invoices"), asking for a tool ("recommend an invoicing tool", "looking for a tool that"), switching ("moving off <competitor>", "<competitor> alternative"), and frustration ("<competitor> support", "hate doing <task>").
- **ICP**: the titles and company types that buy, to keep only the right authors.
- **Competitors**: two or three names for the switching and frustration searches.
- **Window**: default `since: "month"`; `since: "week"` for a fresh list.
- **Budget**: a default run costs about 6 x 2 + 25 + 3 = 40 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the phrasings.** `linkedin_search_posts` for each phrasing with the window, two pages (1 credit a page).
2. **Read and sort each post.** From the `text`: asking for a recommendation (hot), switching or complaining about a competitor (hot), describing the pain (warm), discussing the topic in general (drop), and a vendor, consultant or recruiter marketing to that pain (drop). Keep the sentence that shows the signal.
3. **Qualify the authors.** Trace each author's handle per the [router](../SKILL.md#what-linkedin-shows) and call `linkedin_get_profile` on the hot and warm authors, up to 25 (1 credit each). From the `bio` and `location`: do they fit the ICP (role, company type, market)? Drop those who do not.
4. **Weigh the post.** `likes` and `comments` on the row (or `linkedin_get_post`, 1 credit, when null). A complaint with many likes means many people share it: a theme for content, even when its author is not a buyer.
5. **Deliver** a table: author, profile URL, role and company (from the `bio`), post URL, posted (approximate), signal (asking, switching, complaining, pain), the quote, likes and comments, and a suggested first move: a helpful public comment, or a direct message from the user that answers the post, not a pitch.

For email addresses and company records, hand the table to the [leads](../../leads/SKILL.md) group: its [enrichment](../../leads/references/enrichment.md) playbook adds contact details to a list of names, and [buying intent](../../leads/references/buying-intent.md) combines these posts with other buying signals. Do not look up emails here.

## Judgment

- A public request for a recommendation is the strongest signal LinkedIn has, and it goes stale in days. Recent hot posts go first.
- People who complain in public want to be heard, not sold to. The first move is help in the thread; a pitch in reply to a complaint usually backfires.
- The comments are not readable here, so competitors' replies and other people with the same problem are invisible. The user opens the post before acting.
- Search is ranked and incomplete. This is a sample of who is talking, not everyone with the problem.
- Posts with the problem but no fitting author still count: the likes and the wording show which pain resonates, which is content for the [strategy](strategy.md).
- For the same watch every week, use the [monitoring](../../monitoring/SKILL.md) group; the host runs this playbook on a schedule and keeps the posts already seen.
