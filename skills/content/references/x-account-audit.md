# X account audit

X has three tools here: a profile, one page of an account's recent posts, and one post by URL. There is no search and no replies. So the audit reads one account's own feed (how often it posts, what it posts, what earns views and engagement) and sets it against a competitor's feed read the same way. It ends in a scorecard and a short list of changes.

## Inputs to settle first

- **Account**: the user's X handle.
- **Competitors**: one or two handles in the same market: a peer, and one account the user would like to be. X has no search, so they cannot be found here; ask, or take them from the competitor's site if the host can open it.
- **Goal**: what the account is for: followers, clicks to the site, conversation, or the founder's reputation. It picks the metric that leads the scorecard.
- **Posts older than the feed**: the URLs of a pinned post, a launch post or a post the user remembers doing well, if they want those in the audit.
- **Budget**: a default run costs about 3 x (1 + 1) + 5 = 11 credits for the account, two competitors and five older posts. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Profiles.** `twitter_get_profile` on each handle (1 credit each). Read `followers`, `following`, `posts_count`, `created_at`, `bio` and `website`. Does the bio say who the account is for, and does it link the site? `verified` is true for a paid checkmark too, so it says nothing about standing.
2. **Recent posts.** `twitter_get_tweets` on each handle (1 credit each). X returns one page, newest first, with a null cursor: there is no older history to page into. Note the span of the page, from the oldest to the newest `created_at`. Per post: views, likes, `comments` (replies) and `shares` (reposts).
3. **Measure.** For each account, leave out posts under 48 hours old (most of a post's views arrive in its first day or two), then compute:
   - posts per week: posts on the page divided by the span in days, times 7;
   - median views per post, and median views divided by followers (whether the audience is alive);
   - median engagement per post: likes, replies and reposts divided by views;
   - the share of posts with a link, of thread openers (a "1/" marker or a numbered list), and of questions.
   Use medians: one viral post makes an average meaningless.
4. **Tag the posts.** Read each post's text and tag its topic (product news, how-to, opinion, story, industry news, promotion, engagement bait) and its shape (one line, long post, thread opener, link post, question). The rows do not say whether a post carried an image or a video, so shape comes from the text; open a post on X when it matters.
5. **Find the outliers.** For each account, the three posts with the most views relative to its median and the three with the least. Read them side by side: what do the top ones share (a topic, a shape, a number in the first line, a link or none)?
6. **Read the older posts.** `twitter_get_tweet` on each URL the user gave (1 credit each): the feed page does not reach them. Set their numbers against today's median to see whether the account has grown or faded since.
7. **Deliver** two tables and a list.
   - Scorecard: metric (followers, posts per week, median views, views per follower, median engagement, link share, thread share, top topic), the user's account, each competitor, and what the gap means. The metric that matches the goal comes first.
   - Posts to learn from: account, post URL, views, engagement, topic, shape, and why it worked in one line.
   - Three to five changes, each tied to a row (for example: post four times a week instead of one; lead with the numbered how-to threads that earn three times the median; add the site link to the bio, which has none).
   Drafts of new posts only when the user asks; the host writes them from the posts that worked.

## Judgment

- One page is a sample, and its span varies: an account posting ten times a day covers a few days, one posting weekly covers months. Always state the span, and compare cadence per week, never per page.
- Followers are not reach. Median views per follower is the better health number: a large account with low views per follower has an audience that stopped looking.
- A competitor ten times the size is a benchmark for topics and shapes, not for raw numbers. Compare views per follower and engagement per view instead.
- Replies are counted, not readable: the tools do not return what people said under a post. Mentions of the brand by other accounts are not visible either, since X has no search here.
- A views figure of null means X did not publish one for that post; leave it out of the median rather than counting it as zero.
- An audit is a snapshot. To see whether the changes worked, run it again in four to six weeks on the same handles; the server keeps no history, so the host keeps this run's scorecard.
