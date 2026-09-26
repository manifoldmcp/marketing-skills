# Post formats that work

Which post structures, hooks and lengths get engagement in the user's niche, measured on a sample of real posts rather than on generic LinkedIn advice. It ends in a table of format features with the median engagement of each, example posts, and a few rules the user can apply to the next post.

## Inputs to settle first

- **Niche**: four or five searches the ICP's world posts about. The [topic leaders](topic-leaders.md) searches serve if they were run.
- **Sample**: default 80 posts or more, `since: "month"`; add `since: "year"` in a quiet niche.
- **The user's own posts**: the URLs of their last 10, to compare against the niche. Optional.
- **Budget**: a default run costs about 5 x 3 + 10 + 30 + 10 = 65 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect the sample.** `linkedin_search_posts` for each search, three pages (1 credit a page). Keep rows with `text`, `likes` and `comments`; drop duplicates and company-page posts unless the user runs a page. For rows with null counts, `linkedin_get_post` (1 credit each).
2. **Size the authors.** Trace the handles of the 30 most frequent authors per the [router](../SKILL.md#what-linkedin-shows) and call `linkedin_get_profile` (1 credit each) for `followers`, so engagement can be compared per 1,000 followers.
3. **Code each post from its text.**
   - Length: under 500 characters, 500 to 1,300, 1,300 to 2,000, and cut (the `text` stops at 2,000, so a longer post is only known as long).
   - Hook, the first line: a number or a result, a contrarian claim, a story opening, a question, a how-to promise, or news.
   - Structure: a list or numbered steps, a story, a one-liner, a framework, a lesson list, a before-and-after.
   - Ending: a question to readers, a link, a "comment X to get it" giveaway, or none.
   - Other: a link in the text, tags of people, hashtags.

   The media is not visible (every row reads `media: "text"`), so carousel, image, video and poll are not codes here. If the host has a browser it can open the top 20 post URLs and add the media type; otherwise the user checks them by eye.
4. **Compare.** For each feature, the median likes and comments and, where followers are known, the median per 1,000 followers. Leave giveaways out of the comment medians per the [thresholds](../SKILL.md#thresholds), and compare only groups with at least 10 posts.
5. **Place the user.** When the user gave URLs, `linkedin_get_post` on each (1 credit): code them the same way and show where they sit against the niche's medians.
6. **Deliver** a table: feature, group, posts, median likes, median comments, median engagement per 1,000 followers, and an example post (URL, first line). Under it, three to five rules for this niche ("posts of 800 to 1,300 characters with a numbered list get twice the median"), each with its evidence, and how the user's own posts compare.

## Judgment

- The sample is search results, which favour posts that already did well. It shows what top posts share, not what every post gets. Say so.
- These are correlations. Big authors write in their own style; per-1,000-follower numbers reduce the effect but do not remove it.
- A giveaway post buys comments. Leaving it in makes "question endings" and "long posts" look better than they are.
- Hooks matter most because LinkedIn shows two or three lines before "see more". Quote real first lines in the table rather than describing them.
- Rules differ by niche. A recruiting audience and a developer audience reward different posts; do not carry this table to another niche.
- Links in the post text are worth a column: many authors in a niche avoiding them is itself a finding.
