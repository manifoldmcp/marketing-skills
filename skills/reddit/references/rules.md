# Rules, accounts and bans

What each community allows, what the account needs before it may post, and what gets accounts removed. Run the rules check before anyone posts or replies with a product in mind. Checked October 2026.

## Two layers of rules

- **Reddit's sitewide rules** forbid content manipulation: spam, vote manipulation, ban evasion, and coordinated or inauthentic accounts. Asking anyone to upvote, using a second account to agree, buying aged accounts, or reposting after a removal breaks them, and the penalty can be a sitewide suspension of every account linked to the user. The sitewide rules set no promotion ratio: the "9:1 rule" was an old Reddit guideline, and only 1 of the 20 communities we read still cites it.
- **Each community's own rules**, written and enforced by its moderators, decide what counts as promotion. They are stricter than the sitewide rules, they change, and moderators also enforce unwritten ones. "Not stated" is never permission.

## What communities ban now

From the rules of 20 business and consumer communities read on 15 October 2026 ([field notes](field-notes.md#twenty-communities-rules-15-october-2026)):

- **The product mention.** 10 of 20 ban promoting the user's own product anywhere, 3 confine it to a set-aside thread (r/startups' monthly share thread, r/Entrepreneur's weekly threads, r/webdev's Showoff Saturday), 4 allow it in a relevant reply with conditions, and 3 publish no rule. r/sales bans recommending what you sell even when someone asks for a recommendation; r/smallbusiness has removed product mentions that look promotional since June 2026; r/SaaS allows one mention or three links per 60 days, disclosed, with alt accounts counted, and bans promotion of marketing and Reddit-automation tools entirely.
- **AI-written text.** 11 of the 18 that publish rules remove AI-written posts or comments, often with a ban; r/SaaS includes text the poster edited. A third-party index of 3,845 communities found about one in five with such a rule, more among writers, artists and developers (ThreadFox, September to October 2026, a vendor that does not say how it chose communities).
- **Market research.** 5 of 20 ban "pain points", validation, beta-tester and "would you pay for this" posts outright, and 2 more send them to a thread or to moderator approval.
- **Links.** r/SEO removes every link automatically; r/ecommerce and r/shopify ban external links; several ban URL shorteners and affiliate links. A link to the user's own site is the most common removal trigger.
- **AMAs** need moderator approval in 5 of 20, with up to two weeks' notice.
- **Brand accounts.** r/SkincareAddiction bans brand-affiliated accounts from posting at all; r/sales bans company-branded accounts. Elsewhere a disclosed personal account is the norm.

## The account

- **Posted minimums are common in the communities brands want**: r/marketing 30 days and 300 karma, r/shopify 30 days with 20 post and 20 comment karma, r/sales 10 karma earned in r/sales before posting, r/sysadmin 24 hours, r/Entrepreneur a history of comments there before a first post. Many more filter without saying so.
- **Hidden filters.** Reddit gives moderators a Contributor Quality Score per account (five tiers, lowest to highest), which AutoModerator can filter on; Reddit calls it an internal classification and does not show it on the account. It draws on the account's past actions, network and location signals and whether the account is secured, such as a verified email (r/modnews, September 2023).
- **Removals are silent.** A filtered comment looks normal to its author. Three things can hide it: a sitewide shadowban or suspension by Reddit's admins, Reddit's spam filter, or a community's AutoModerator. Check every early reply from a logged-out private window; if the account's profile shows "not found" logged out, the problem is sitewide.
- **Build history first.** Two to four weeks of plain helpful comments with no product and no links, in the communities that matter, before the first mention. Spread replies out: many similar comments in a day look like spam to Reddit's filters and to moderators.
- **Verified profiles.** Reddit began testing grey verification checkmarks in December 2025 for a curated group of public figures and businesses, replacing the old "official" badge for brands. Verification grants no privileges and no exemption from a community's rules. The user cannot request one.
- **AI personas get found.** In April 2025 researchers from the University of Zurich were exposed for running undisclosed AI accounts in r/changemyview; Reddit banned the accounts and its chief legal officer said it would send the university and the researchers formal legal demands. In June 2026 Reddit said it would ask suspicious automated accounts to prove they are human.

## The rules check

For each community and each action the user wants (reply, mention the product in a reply, add a link, post a launch or "I built this", share a blog post, run an AMA, post a survey):

1. **Read the rules.** `reddit_get_subreddit` (1 credit, cached 7 days, so free if read this week): `rules` and `description`. The rules are prose, not flags; read them. `submit_text` came back empty on all 20 communities in October 2026, so the posting guidelines Reddit shows on submit are not available; tell the user to read them when they post. Where the rules point to a wiki page, read it if the host can open web pages; otherwise tell the user to.
2. **Give one verdict per action**: allowed; allowed with conditions (a set thread or day, a flair, a disclosure, a quota, a karma or age minimum, moderator approval); not allowed; or not stated. Quote the rule that decides it. Words to look for: self promotion, advertising, spam, links, affiliate, surveys, low effort, AI, flair, karma, account age, AMA, and named days or threads.
3. **Find the set-aside thread** where the rules send promotion: `reddit_search_posts` with `subreddit`, the thread's name as the rules give it, and the default relevance sort (1 credit); pick the newest by `created_at`. Keep its URL and `comments` count: a busy share thread is worth a post, an empty one is not.
4. **See what survives.** `reddit_search_posts` with `subreddit` and "I built", "my startup" or a competitor's name, `time_range: "year"` (1 credit). Removed posts mostly do not appear, so what comes back is largely what the moderators let stand. A vendor post with a good `score` shows what is tolerated; none at all in a busy community means promotion is removed on sight.
5. **Deliver** a table: subreddit, action, verdict, the rule quoted, conditions, an example that survived (URL, or none), and a one-line how-to. Where the verdict is "not stated", the how-to is "ask the moderators through modmail first": the user sends that message, never the agent. Default cost about 25 credits for 10 communities.

## Always

- Say the affiliation in every reply that mentions the product, even where the rules do not require it ("I work on X"). A hidden affiliation found out costs more than the reply earned.
- Rules change, and records are cached 7 days. Read them again before a launch post.
- Never suggest a second account, asking friends to upvote, buying accounts or votes, or deleting and reposting a removed post.
