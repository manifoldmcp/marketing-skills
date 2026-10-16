# Commenting

Comments on other people's posts reach an audience before the user's own posts can, and since 2026 LinkedIn both rewards real comments and punishes automated ones. This covers why, and how to build a daily or weekly list of posts worth a comment. Checked October 2026.

## Why it works now

- **Comments are where time is going.** Time spent in post comments rose 18% year on year (LinkedIn, Q2 2026), and since August 2026 LinkedIn ranks a post's comments by relevance to each viewer (their interests, connections and activity), so a substantive comment can be shown near the top to the people who care about its topic, not just in time order.
- **The ranker predicts comments** as one of the active actions it scores (LinkedIn Engineering, March 2026). No weight is published; the "a comment is worth 15 likes" figure has no primary source.
- **Comments before followers.** A founder with under a few thousand followers reaches more of the ICP through ten useful comments a day on the right posts than through a fourth weekly post.
- **Automation is detected and costs the account.** Comments LinkedIn judges automated are removed from the default "Most relevant" view and may reach only the commenter's own network; repeat offenders are restricted (February 2026). Pangram estimated 30% of comments in April to June 2026 were wholly AI-written, and LinkedIn's classifiers target exactly that. Pod comments raise the first hour by 31% and lower final reach by 6% (MagicPost). Never suggest a commenting tool, a pod or an AI that comments for the user.
- **Commenting on others just before posting does not lift your own post** (−4%, range −17% to +10%, MagicPost). Comment because the comment is worth reading.

## Building the list

Inputs: three to five topics the user can speak to with authority, in the buyer's words; the ICP (titles and company types); the user's edge (their data, a result, years in the role), since every angle comes from it; whose posts (leaders, buyers or both; default both); the window (`since: "day"` for a daily list, `since: "week"` for a weekly one); and a count (default 10 a day; more becomes spam). A default run is about 4 x 2 search pages + 20 profiles + a few posts, about 33 credits.

1. **Find recent posts.** `linkedin_search_posts` for each topic in the window, two pages each. For named leaders, `linkedin_get_profile` lists their latest posts in `recent_posts`; read the ones in the window with `linkedin_get_post` (see [topic leaders](topic-leaders.md)).
2. **Sort the authors.** Each row carries `author_url` and `author_followers`. `linkedin_get_profile` on the 20 most promising: from the `bio`, `followers` and `location`, a buyer in the ICP, a leader with reach, or a vendor, consultant, competitor or recruiter (drop those).
3. **Check there is room.** Under about 50 comments, the author and readers still see a new one; past a few hundred it sinks. A post keeps gathering views for about three days, so anything older is past its window. Drop giveaways ("comment X to get the guide") and posts whose `top_comments` look automated.
4. **Pick where a comment adds something.** Keep posts that ask a question, make a claim the user can back or test with data, or describe a problem the user knows well. `top_comments` shows what has been said, so the angle does not repeat it; it is 10 comments at most, so the user reads the thread before writing.
5. **Write the angle in one line:** a number, an example, a counterpoint, a question that moves the thread. Never a pitch, a link to the product, or "great post".

Deliver a table: post URL, author, who they are (buyer or leader), followers, posted (approximate), likes, comments, why this post, and the angle. Buyers first, then the most recent.

## Judgment

- A comment on a buyer's post is worth more than one on a famous creator's, even with ten times fewer readers: the buyer reads every comment.
- Consistency over volume. Ten useful comments a day for a month builds a name; fifty in a day looks automated, and LinkedIn may cap how many comments a member makes in a period.
- The comment is the user's own. Draft one only when asked, mark it a draft, and keep it to what the user actually knows.
- A daily or weekly list is the host's schedule, keeping the post URLs already listed; the server keeps no state.
- The same engagement on Reddit threads: [reddit](../../reddit/SKILL.md).
