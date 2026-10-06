# Launch communities

Where to launch, and what to post in each place so the post stays up. The community lists and the rules come from [find-subreddits](../../find-subreddits/SKILL.md) and [mine-facebook-groups](../../mine-facebook-groups/SKILL.md); this reference adds the launch reading: which communities take a launch post and in what format, what a launch there looked like when it worked, when to post, and where only a useful post without a link will survive.

## Inputs to settle first

- **Product and problem**: what launches, and the problem in the buyer's words (the words people post with, not the category name).
- **Launch date**: from the launch plan in [the skill](../SKILL.md), or ask.
- **Facebook groups**: links to the groups the user belongs to or knows. There is no group search; [mine-facebook-groups](../../mine-facebook-groups/SKILL.md) can find a few public ones through Google.
- **Other venues**: Product Hunt, Hacker News, Slack, Discord, Indie Hackers, newsletters. Listed with the user's notes; no tool reads them (see the skill's [Judgment](../SKILL.md#judgment)).
- **Account**: how old the user's Reddit account is and whether it has a history in these communities. No tool can see it; ask.
- **Budget**: about 28 for finding the subreddits and 25 for reading their rules in [find-subreddits](../../find-subreddits/SKILL.md), and 33 for reading the groups in [mine-facebook-groups](../../mine-facebook-groups/SKILL.md) at their defaults, plus about 5 x 2 = 10 here: about 96 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Shortlist the subreddits.** Run [find-subreddits](../../find-subreddits/SKILL.md) with the problem words and the category. Keep its top ten by fit.
2. **Read the rules for a launch.** Run the rules check in [find-subreddits](../../find-subreddits/SKILL.md) on the ten with two actions: "post a launch or I built this" and "reply in threads". From its verdicts, sort each community into one launch format:
   - Launch post allowed, with its conditions (a flair, a set day, a karma minimum).
   - Only in the weekly or pinned promotion thread it found.
   - A useful post with no link: the story of building it, a lesson, a free resource. The link goes in a comment only if the rules allow it, otherwise in the user's profile.
   - Comments only: answer questions in the weeks around launch, no post.
3. **Study the launches that survived.** For each community where a launch post is allowed, take the example that survived from the rules table. `reddit_get_post` on it (1 credit) for `score`, `upvote_ratio` and `comments`, and `reddit_get_comments` (1 credit) for how the community treated it: questions, pushback, or a moderator's note. Note its title shape, whether it was a text or a link post, and the weekday and hour of `created_at`.
4. **Check the Facebook groups.** Run [mine-facebook-groups](../../mine-facebook-groups/SKILL.md) on the group links. For the launch, take from it whether members post their own products and how those posts were received, and the group's pace. No tool returns a group's rules: the user reads them in the group before posting.
5. **Add the other venues.** List each venue the user named, with its format and the user's notes, and mark it as having no data here.
6. **Deliver** a table: community, platform, members (`subscribers` on Reddit), weekly activity, the rule line quoted, launch format (launch post, promotion thread, useful post without a link, comments only), the angle or title for that community, the best day and hour seen, and the timeline point (T-30 start taking part, T-7 draft, launch day, T+7 results post). The user posts; nothing is posted from here.

## Judgment

- Read a community's rules before planning a post there, and quote the line that allows or forbids it. Not stated is not permission.
- A community that bans self-promotion bans launch posts. The useful post without a link is the only format there, and it works only when it would be worth reading with the product left out.
- Take part first. Start answering questions in the chosen communities at T-30. An account whose first post is its own launch looks like spam to moderators and to members, and many automatic filters remove it unseen.
- Timing comes from the evidence in step 3, not general advice. With no evidence, post on a weekday morning in the time zone where most of the ICP lives.
- One post per community, written for its rules and its readers' problem. The same text in ten places on one day reads as spam and gets accounts flagged.
- Space the posts across the day or the week so the founder can answer every comment in the first hours. Unanswered questions on a launch post read as an absent founder.
- A community with a high `weekly_contributions` buries a post within hours; a smaller one that fits the ICP exactly often does better.
- Rules change. The rules record is cached 7 days: read it again in launch week before posting.
- The user posts from their own accounts. Nothing here posts, submits, votes or messages.
