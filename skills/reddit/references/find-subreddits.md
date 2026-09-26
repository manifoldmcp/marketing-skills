# Find subreddits

The communities where the user's buyers ask questions, ranked by how often the topic comes up and how close the members are to the ICP, with each community's stance on self promotion. It ends in a short list the other playbooks use: threads to reply, pain points, a launch, a watch.

## Inputs to settle first

- **Topic**: the problem the product solves, in the buyer's words ("cold emails landing in spam", not "deliverability platform"). Ask for two to four phrasings.
- **ICP**: who buys (role, company size, industry), to tell a buyer community from a vendor, hobbyist or job-seeker one.
- **Competitors**: one or two names. Where they are discussed is where buying conversations happen.
- **Known communities**: any the user already reads. They are checked in step 3, not searched for.
- **Budget**: a default run costs about 6 + 12 + 10 = 28 credits (six searches, twelve communities read, one check-in over ten). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Sample the conversation.** `reddit_search_subreddits` for each topic phrasing and each competitor name, with `time_range: "year"` (1 credit each). Each call counts the communities behind one page of posts, so a quiet community can be missing: that is why several phrasings run. Keep `name`, `subscribers`, `posts_in_sample` and `example_post_url`.
2. **Merge and shortlist.** Merge by name and add up `posts_in_sample`: a community that shows up for several phrasings is where the topic lives. Drop general news and meme communities, `over_18` ones, other languages unless the market is there, and a competitor's own subreddit (a support forum for its users; note it for [competitor complaints](competitor-complaints.md)). Add the user's known communities and keep up to 12.
3. **Size and read.** `reddit_get_subreddit` on each (1 credit, cached 7 days): `subscribers`, `weekly_active_users`, `weekly_contributions`, `description`, `rules` and `submit_text`. A community that returns `NoData` cannot be read (private, banned or misspelled): drop it. Apply the activity floor in the [router](../SKILL.md#floors).
4. **Measure the topic.** `reddit_get_new_posts` on the top 10 in one call, with `since: "7d"` and `match` set to the topic terms and competitor names (1 credit per subreddit, 10 in all). The matching posts per community are how often the topic comes up in a week, complete for the window. Where `coverage[]` shows `complete: false`, the count covers only back to `covered_from`: report it as "at least" and scale it to a week, or rerun that community with `pages: 3`. Read the matching titles to see who posts: buyers asking, vendors promoting, or students and job seekers.
5. **Read the rules stance.** From the `rules` and `submit_text` already fetched, mark each community the way [Subreddit rules](subreddit-rules.md) does: product mentions allowed, allowed in a set thread or with conditions, not allowed, or not stated. No extra call is needed; run that playbook in full before anyone posts.
6. **Deliver** a table: subreddit, subscribers, weekly contributions, topic posts in the last 7 days ("at least" where coverage was cut), who posts (buyers, vendors, mixed), self-promotion stance, an example thread URL, and one line on why it fits. Rank by topic posts a week first, fit to the ICP second.

## Judgment

- Activity beats size. A community with 30 topic posts a week and 40,000 members is worth more than one with 3 million members where the topic comes up twice.
- Look for the ICP's job, not the product category: a sales tool's buyers are in sales communities, not only in communities about CRMs.
- A community where most matching posts are vendors promoting is a poor target: members are tired of pitches and moderators are strict.
- `reddit_search_subreddits` misses quiet communities by design. Ask the user which ones they read, and check them in step 3.
- Where competitors are discussed shows buying conversations; where only the problem is discussed shows research and content opportunities. Say which each community is.
- For a launch day across Reddit and other communities, the `launch` group's [communities](../../launch/references/communities.md) playbook uses this list.
