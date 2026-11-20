# Mining Facebook groups

What members of public groups ask, complain about and recommend, and the open threads where the user could help. Many buyers (parents, homeowners, local customers, trades, hosts, hobbyists, niche professionals) talk in groups more than anywhere else, and posts from joined groups are part of the connected half of what Facebook shows people ([feed](feed.md)). Field traps are in [data](data.md).

## Two ways in

**A group, newest first.** `facebook_get_group_posts` with the group `url` returns 3 to 5 posts a call with no keyword filter. Good for a group's pace and its current mix; poor for a topic, because five calls are a few hours of a busy group.

**Threads on a topic, through Google.** Google indexes public group threads and shows them in organic results and in its discussions and forums block. In our October 2026 test, Facebook group threads appeared in four of six local "best X in town" searches ([field notes](field-notes.md)). So:

1. `seo_get_serp` for the buyer's question as they would type it ("good plumber sacramento", "best bakery portland maine"), and for "<topic> facebook group", "<ICP> facebook group" and "<town> <topic> group". Keep rows whose `url` contains facebook.com/groups. `depth: 20` reaches further at 2 credits. A `site:` operator came back unfiltered in our test; do not rely on it.
2. `facebook_get_post` on each thread (1 credit) for its text, date and counts, and `facebook_get_comments` for what members answered (10 top-level comments a call). This works on year-old threads.
3. Note each thread's group: those are the groups worth a pace check.

Ask the user first, too: they usually know the groups their customers are in, including private ones the tools cannot read.

## Reading a group

- **Pace.** How many days five calls cover. Busy and recent means the group is worth watching; months for five calls means it is quiet.
- **Keep** posts that ask a question, describe a problem, ask for a recommendation, or name the category or a competitor. Drop promotions, giveaways and off-topic posts; note how much of the group is promotion, because that tells how strictly it is moderated.
- **Who runs it.** A group run by a vendor or a competitor skews toward that vendor; say so when the name or top posts show it.
- **Recommendations.** On "who do you recommend" threads, count each business or brand members name, and the objections to each. In a Portland thread we read, a request for a local bakery drew 29 comments naming at least eight different small businesses, several of them home bakers self-promoting in the replies.

## Clustering

Group kept posts and comments into themes by the underlying problem, not the words: questions people repeat, pains, tools or businesses recommended (a count each), and how members talk about competitors. Count distinct posts per theme. A feature request is a pain stated as a solution: record the problem behind it. Counts rank themes against each other; they never size a market, because no window is complete.

## Rules before anyone posts

- No tool reads a group's rules. Before the user answers in a group, they read its About section and pinned posts. Many groups ban promotion outside a set thread or day, and admins remove members for it. Not stated is not permission.
- A helpful answer that solves the asker's problem, from the owner under their own name, is what works in groups; a link drop is what gets removed.
- Never post, comment, join a group or contact members on the user's behalf. Never turn group members into a lead list.

## Deliver

Two tables. Themes: theme, posts (count), a representative quote with the post link, brands or businesses named with counts, and the implication (a message, a post idea, a feature, an FAQ). Open threads: recent posts that ask something the user's product or knowledge answers and have few comments, with URL, group, age and comments, for the user to decide on. Say how many posts across how many groups and threads the tables rest on. Ranking these pains with other sources is [voice-of-customer](../../voice-of-customer/SKILL.md); launching in groups is part of a launch plan in [growth-strategy](../../growth-strategy/SKILL.md).
