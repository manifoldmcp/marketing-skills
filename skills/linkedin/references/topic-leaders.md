# Topic leaders

Who leads the conversation on a topic on LinkedIn: the people to learn from, comment on, partner with or watch. Found from the authors behind LinkedIn's top search results, sized by followers and by the engagement their posts earn. Checked October 2026.

## Why it matters more in 2026

The feed now matches posts to members by topic (see [the feed](feed.md)), so the people who already own a topic are the ones whose audience the model associates with it. Comments on their posts reach that audience, and a co-authored or collaborative post (available since June 2026) puts the user in front of it directly. Paying a LinkedIn creator to post is sponsorship and belongs to [creators](../../creators/SKILL.md); LinkedIn's Creator Marketplace (US and Canada, June 2026) is one route.

## Finding them

Inputs: three to five searches in the words the ICP uses ("revops", "sales compensation", "outbound SDR"), not the product category alone; the ICP, to spot buyers among the leaders; a window (`since: "month"` for who is active now, `since: "year"` for who has owned the topic longer); who counts (everyone, or only practitioners); a count (default the top 20). A default run is about 4 x 3 search pages + 25 profiles + 5 range checks, about 42 credits.

1. **Search the topic.** `linkedin_search_posts` for each search, three pages. Keep every row with its `author`, `author_name`, `url`, `likes`, `comments`, `author_followers` and `created_at`.
2. **Group by author.** Per author: posts in the results (how much), median likes and comments (how well), and the best post. Keep authors with at least two posts in the results, or one post in the top tenth by engagement. Set company pages aside unless the user wants them.
3. **Size them.** Engagement per 1,000 followers from `author_followers`. Followers are reach; per-1,000 engagement is resonance. A leader with 8,000 followers and high resonance is often a better partner than a famous one who posts about everything.
4. **Read the profiles.** `linkedin_get_profile` on the top 25 with their `author_url`: `bio`, `location`, `experience`, `recent_posts`.
5. **See their range.** `recent_posts` shows how often they post. For the top five, one search on their main topic with `since: "year"` and keep their rows: a steady weekly poster or one hit?
6. **Label each.** Creator (writes for an audience), practitioner (writes about their own work), vendor or consultant (writes to sell), or buyer (in the user's ICP). Mark competitors and their employees.

Deliver a table: name, profile URL, label, bio in a line, followers, posts in the results, median likes and comments, engagement per 1,000 followers, best post (URL, first line, likes, comments), and why they matter: learn from, comment on, partner with, or watch.

## Judgment

- "Posts most" means most in ranked results that favour posts already doing well; a steady poster with modest numbers can be missing. A second, narrower search finds more. The table is a sample of who is talking.
- Discount giveaway posts (comments far above likes) and posts with suspect engagement, per [reading the data](data.md#thresholds); a leader whose numbers rest on them is weaker than they look.
- Buyers who post about the topic are worth more to the user than creators with bigger audiences; keep them in the table even when their numbers are small.
- `top_comments` on a leader's best posts names up to 10 people engaging with the topic: often the most active members of the user's audience. Read them, but treat them as people to learn about, not a contact list; emails and outreach belong to [outbound](../../outbound/SKILL.md).
- Never post, comment, like, connect or message. The deliverable is a list with a link to every post and person.
