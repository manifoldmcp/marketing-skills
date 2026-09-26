# Topic leaders

The people who post most and best on a topic on LinkedIn: the authors behind the top search results, traced to their profiles, sized by followers and by the engagement their posts get. It ends in a ranked table of people to learn from, comment on, or partner with.

## Inputs to settle first

- **Topic**: three to five searches in the words the ICP uses ("revops", "sales compensation", "outbound SDR"), not the product category alone.
- **Window**: default `since: "month"` for who is active now; `since: "year"` for who has owned the topic longer.
- **Who counts**: everyone, or only practitioners (people who do the job) rather than creators, vendors and consultants. Default: everyone, labelled in step 5.
- **Count**: default the top 20.
- **Budget**: a default run costs about 4 x 3 + 25 + 5 = 42 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the topic.** `linkedin_search_posts` for each search, three pages (1 credit a page). Keep every row with its `author_name`, `url`, `likes`, `comments` and `created_at`.
2. **Group by author.** Trace each author's handle from the post URL per the [router](../SKILL.md#what-linkedin-shows). Per author: posts in the results (how much), median likes and comments (how well), and the best post. Keep authors with at least two posts in the results, or one post in the top tenth by engagement. Set company pages aside unless the user wants them.
3. **Read the profiles.** `linkedin_get_profile` on the top 25 authors (1 credit each): `followers`, `location` and the `bio`. Compute engagement per 1,000 followers per the [thresholds](../SKILL.md#thresholds).
4. **See their range.** A person's feed cannot be listed. For the top five, `linkedin_search_posts` on the topic they post about most with `since: "year"` (1 credit each) and keep the rows under their name: whether they post on the topic every week or had one hit.
5. **Label each.** From the `bio` and the posts: creator (writes for an audience), practitioner (writes about their own work), vendor or consultant (writes to sell), or buyer (in the user's ICP). Mark competitors and their employees.
6. **Deliver** a table: name, profile URL, label, `bio` in a line, followers, posts in the results, median likes and comments, engagement per 1,000 followers, best post (URL, first line, likes, comments), and why they matter: learn from, comment on, partner with, or watch as a competitor.

## Judgment

- "Posts most" means most in the ranked results, which favour posts that already did well. A steady poster with modest numbers can be missing; a second search with narrower terms finds more.
- Followers are reach; engagement per 1,000 followers is resonance. A leader with 8,000 followers and high resonance is often a better partner than a famous one who posts about everything.
- A post with far more comments than likes is often a "comment X to get the guide" giveaway. Discount it per the [thresholds](../SKILL.md#thresholds).
- Buyers who post about the topic are worth more to the user than creators with bigger audiences. Keep them in the table even when their numbers are small.
- The comments on a leader's posts, where much of the value is, cannot be read here. The user reads the threads of the top posts before engaging.
- For people to sponsor on TikTok, Instagram or YouTube, use the [influencers](../../influencers/SKILL.md) group. For emails of the people found here, use the [leads](../../leads/SKILL.md) group.
