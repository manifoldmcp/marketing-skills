# Posts to comment on

A short daily or weekly list of recent LinkedIn posts, by topic leaders or by the user's buyers, where a comment from the user adds something: a number, an example, a counterpoint. The tools find and rank the posts; the user reads the thread and writes the comment.

## Inputs to settle first

- **Whose posts**: leaders on the topic (from [topic leaders](topic-leaders.md), or found in step 1), buyers in the ICP, or both. Default: both.
- **Topics**: three to five searches the user can speak to with authority.
- **The user's edge**: what they know that others do not (data from their product, a result, years in the role). Every angle in the table comes from it.
- **Window**: `since: "day"` for a daily list, `since: "week"` for a weekly one. Default: day.
- **Count**: default 10 posts a day. More than that becomes spam.
- **Budget**: a default run costs about 4 x 2 + 20 + 5 = 33 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find recent posts.** `linkedin_search_posts` for each topic with the window, two pages (1 credit a page). For named leaders, there is no feed to list: search the topics they post about and keep the rows under their name.
2. **Check the authors.** Trace each author's handle per the [router](../SKILL.md#what-linkedin-shows) and call `linkedin_get_profile` on the 20 most promising (1 credit each). From the `bio` and `followers`: a buyer in the ICP, a leader with reach, or a vendor, competitor or recruiter (drop those).
3. **Check there is room.** From the row's `likes` and `comments` (or `linkedin_get_post`, 1 credit, when null): on a post with fewer than about 50 comments, the author and the readers still see a new one; past a few hundred, it sinks. Drop giveaways ("comment X to get the guide") and posts older than the window.
4. **Pick where a comment adds value.** Keep posts that ask a question, make a claim the user can back or test with data, or describe a problem the user knows well. Write the angle in one line: what the user can add. Never a pitch, a link to the product, or "great post".
5. **Deliver** a table: post URL, author, who they are (buyer, leader), followers, posted (approximate), likes, comments, why this post, and the angle. Sort buyers first, then by how recent the post is.

## Judgment

- The comment is the user's own. Draft one only when asked, mark it a draft, and keep it to what the user actually knows.
- The existing comments cannot be read here. The user reads the thread before writing, so the comment does not repeat someone.
- A comment on a buyer's post is worth more than one on a famous creator's, even with ten times fewer readers: the buyer reads every comment.
- Consistency matters more than volume. Ten useful comments a day for a month builds a name; fifty in one day looks automated.
- `created_at` is approximate to the unit LinkedIn shows, which is enough for "today" and "this week".
- For a list on a schedule, every morning, use the [monitoring](../../monitoring/SKILL.md) group: the host runs this playbook daily and keeps the post URLs already listed.
