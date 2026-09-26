# Facebook group mining

What members of Facebook groups ask, complain about and recommend. Many buyers (local businesses, parents, trades, hobbyists, niche professionals) talk in Facebook groups more than anywhere else. There is no group search and no keyword search here, so the job starts from group links. It ends in a table of themes with quotes and links, and a short list of posts where the user could help.

## Inputs to settle first

- **Groups**: links to public groups (`facebook.com/groups/...`). Ask the user first: they usually know the groups their customers are in. If they have none, step 1 finds some.
- **Topic**: the problem and category words and competitor names that mark a relevant post. There is no filter parameter on the group tool, so these guide the reading.
- **Depth**: default 5 pages of posts per group, newest first.
- **Budget**: a default run over three groups costs about 3 x 5 + 15 + 3 = 33 credits (five pages of posts per group, fifteen posts' comments, three searches for groups when needed). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find groups if the user has none.** `seo_get_serp` for "<topic> facebook group", "<ICP> facebook group" and "<city or niche> <topic> group" (1 credit each at the default depth, or `depth: 20` for 2). Keep the facebook.com/groups URLs. Google shows only some groups, and only public groups can be read; ask the user to add any they belong to.
2. **Pull the posts.** `facebook_get_group_posts` with the group's `url` (1 credit per page of results), newest first, paging with `meta.cursor` up to 5 pages. Dedupe on `id`. A private group, or a link that is not a group, returns nothing: tell the user and move on.
3. **Keep what matters.** Keep posts that ask a question, describe a problem, ask for a recommendation, or name the category or a competitor. Drop promotions, `is_ad` posts, giveaways and off-topic posts. Note how many days the 5 pages covered: that is the group's pace.
4. **Read the discussions.** `facebook_get_comments` on the 15 kept posts with the most `comments` (1 credit each, one page). Note what members recommend, which tools and brands they name and how often, and the objections to each.
5. **Cluster.** Group the kept posts and comments into themes: questions people repeat, pains, tools recommended (a count per brand), and how members talk about competitors. Count distinct posts per theme.
6. **Deliver** two tables. Themes: theme, posts (count), a representative quote with the post link, brands named (with counts), and the implication (a message, a content idea, a feature). Open posts: the recent posts that ask something the user's product or knowledge answers and have few comments, with URL, group, age and comments. Say that each group's own rules decide whether the user may answer with the product.

## Judgment

- No tool reads a group's rules. Before the user posts, they read the group's About section. Many groups ban promotion outside a set thread or day, and admins remove members for it. Not stated is not permission.
- The pace tells how useful a group is. If 5 pages reach back only two days, the group is busy and the sample is recent; if they reach back six months, the group is quiet and is not worth watching.
- Group posts are newest first but not a complete window. To watch a group over time, the host pages back to the last `id` it saw and stores it: the `monitoring` group's [brand mentions](../../monitoring/references/brand-mentions.md).
- A group run by a vendor or a competitor skews toward that vendor. Say who runs it when the group name or its top posts show it.
- Members are private people. Quote without names, and never turn group members into a lead list.
- Posting a launch in groups is the `launch` group's [communities](../../launch/references/communities.md), which uses this list.
