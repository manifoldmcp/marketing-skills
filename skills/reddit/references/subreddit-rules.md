# Subreddit rules

Before anyone posts or replies with a product in mind, read what each community allows. This playbook reads the rules the moderators wrote, finds the place a community sets aside for promotion, and checks what survives in practice. It ends in a verdict per community and action. Every playbook in this group that suggests a reply or a post links here for this step.

## Inputs to settle first

- **Communities**: subreddit names from the user or from [find subreddits](find-subreddits.md). Up to about 10.
- **Actions**: what the user wants to do. Reply in threads, mention the product in a reply, add a link, post a launch or "I built this", share a blog post, run an AMA, post a survey. Rules differ by action. Default: reply in threads and mention the product where it answers the question.
- **Account**: how old the user's Reddit account is and roughly how much karma it has. No tool can see it, so ask; many communities filter new or low-karma accounts automatically.
- **Budget**: a default run for 10 communities costs about 10 + 5 + 10 = 25 credits (rules, the promotion thread where one exists, a practice check). Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the rules.** `reddit_get_subreddit` on each community (1 credit, cached 7 days, so free when another playbook read it this week). Read three fields, all text: `rules` (the numbered rules), `submit_text` (what the community shows a poster on submit, often the strictest line) and `description` (the sidebar, which often adds rules). The rules are not parsed into flags: read them.
2. **Decide each action.** For every action the user named, give one verdict: allowed; allowed with conditions (only in a weekly or pinned thread, on a set day, with a flair, with a disclosure, after a karma or account-age minimum, with moderator approval); not allowed; or not stated. Quote the rule text that decides it. Words to look for: self promotion, advertising, spam, links, affiliate, surveys, low effort, flair, karma, account age, AMA, and named days or threads ("Self-Promotion Saturday", "weekly share thread").
3. **Find the set-aside thread.** Where the rules send promotion to a recurring thread, find the current one: `reddit_search_posts` with `subreddit`, the thread's name as the rules give it, and `sort: "new"` (1 credit). Check the rows are on topic, per the [router](../SKILL.md#search-and-watching); if they are not, search with the relevance sort and pick the newest by `created_at`. Keep the newest thread's URL and its `comments` count: a busy share thread is worth a post, an empty one is not.
4. **See what survives.** `reddit_search_posts` with `subreddit` and "I built", "my startup" or a competitor's name, `time_range: "year"` (1 credit per community). Removed posts mostly do not appear, so what comes back is largely what the moderators let stand. A vendor post with a good `score` shows what is tolerated; none at all, in a busy community, means promotion is removed on sight.
5. **Deliver** a table: subreddit, action, verdict (allowed, with conditions, not allowed, not stated), the rule text quoted, conditions (flair, day, the share thread URL, disclosure, karma or age minimum), an example that survived (URL, or none), and a one-line how-to. Where the verdict is "not stated", the how-to is "ask the moderators through modmail before posting": the user sends that message, not the agent.

## Judgment

- Not stated is not permission. Moderators also enforce unwritten rules and Reddit's sitewide rules on spam and vote manipulation, and a removal can come with a ban.
- Karma and account-age minimums are often enforced by an automatic filter and not posted. A new account's post can vanish without notice. Tell the user to build history with plain helpful comments first.
- Say the affiliation in every reply that mentions the product, even where the rules do not require it. A hidden affiliation that is found out costs more than the reply earned.
- Rules change. The record is cached 7 days; read it again before a launch post.
- The tools read the rules the community publishes on its profile, not its wiki pages. If the rules point to a wiki page and the host can open web pages, read it there; otherwise tell the user to check it.
- Never suggest a second account, asking friends to upvote, or deleting and reposting a removed post. Each breaks Reddit's rules and risks a sitewide ban.
