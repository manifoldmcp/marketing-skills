# Finding communities

Which subreddits the user's buyers ask questions in, how busy each is, and whether its members are buyers, vendors or hobbyists. Ends in a short list of three to five communities the rest of the work uses. Checked October 2026.

## Size is not what it was

- Reddit replaced the public member count with **weekly visitors** (unique visitors over seven days, averaged over 28, bots and logged-out readers excluded) and **weekly contributions** (posts plus comments), announced on r/modnews in September 2025. Manifold returns no member count; `weekly_active_users` is the visitors figure and `weekly_contributions` the activity one ([field notes](field-notes.md#twenty-communities-rules-15-october-2026)). An old "2.4M members" figure from memory or a blog is from the previous metric and not comparable.
- **Activity beats size.** A community with 30 topic posts a week and 15,000 weekly visitors is worth more than one with a million visitors where the topic comes up twice. Visitors also count readers who never post; r/BuyItForLife had 1.8M weekly visitors and 12K contributions, r/SaaS 282K visitors and 11K contributions.
- **Floors.** Keep a community when people post there daily: `weekly_contributions` above about 100, or at least 7 posts in a 7-day check-in. Below that a reply is read by few people unless Google shows the thread.
- Since December 2025 Reddit has been phasing in a limit: a moderator may moderate at most five communities with more than 100K weekly visitors, and moderators over it are removed from the ones where they are least active (r/modnews, September 2025). Large communities have seen moderator turnover, and new teams rewrite rules: read them again rather than relying on last year's.

## Finding them

1. **Phrase the problem, not the product.** "cold emails landing in spam", not "deliverability platform". Run `reddit_search_subreddits` (1 credit) with four to six phrasings and each competitor's name, `time_range: "year"`. Each call counts the communities behind one page of about seven posts, so one call names three to six communities and misses quiet ones by design. Merge by name and add up `posts_in_sample`: a community that shows up for several phrasings is where the topic lives.
2. **Ask the user** which communities they already read; search misses them.
3. **Drop** general news and meme communities, `over_18` ones, other languages unless the market is there, and a competitor's own subreddit (a support forum for its users: note it for [competitor-intel](../../competitor-intel/SKILL.md), not for posting).
4. **Size and read** up to 12 with `reddit_get_subreddit` (1 credit each, cached 7 days): weekly visitors, weekly contributions, `description`, `rules`. A `NoData` reply means private, banned or misspelled. The rules are prose; read them the way [rules](rules.md) says.
5. **Measure the topic** with `reddit_get_new_posts` on the top 10 in one call, `since: "7d"`, `match` set to the problem terms and competitor names (1 credit per subreddit per page). The matching posts are how often the topic comes up in a week. A busy community will come back cut: one page of r/SaaS reached back under 8 hours, of r/CRM 6 days. Where `coverage[]` shows `complete: false`, report the count as "at least N since `covered_from`" and scale it to a week, or rerun that community with `pages: 3` and a shorter `since`.
6. **Read the matching titles** to see who posts. Mark each community: buyers asking, vendors promoting, mixed, or students and job seekers.

## Judging fit

- **Look for the ICP's job, not the category.** A sales tool's buyers are in r/sales, not only r/CRM; a skincare brand's are in r/SkincareAddiction, which bans brand accounts outright, so it is a listening community, not a posting one.
- **Where competitors are discussed** shows buying conversations; where only the problem is discussed shows research and content opportunities. Say which each community is.
- **A community full of vendor posts is a poor target.** Members are tired of pitches and either moderators are strict or nobody is moderating. In our week of r/CRM, half the new posts were vendors'.
- **No published rules is not an open door.** r/CRM and r/SideProject returned no rules text; the first was saturated with pitches that scored 0 to 3.
- **Three to five communities, not twenty.** Moderators and regulars notice a newcomer who appears everywhere with the same message.
- A community that bans any product mention still counts for listening and for plain helpful answers that build the account's history. Say so rather than drop it.

## The deliverable

A table: subreddit, weekly visitors, weekly contributions, topic posts in 7 days ("at least" where coverage was cut), who posts, self-promotion stance from [rules](rules.md), an example thread URL, and one line on why it fits. Rank by topic posts a week first, fit to the ICP second. Default cost about 28 credits (six searches, twelve communities read, one check-in over ten).
