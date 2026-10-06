# Channel signals

When a baseline supports a channel, with the reason for each threshold. [create-growth-plan](../SKILL.md) and [create-gtm-plan](../../create-gtm-plan/SKILL.md) check their candidate channels against these.

- **Search**: the category's main keywords add up to more than about 1,000 searches a month, with some at KD under 30. Below that, search captures little until demand exists.
- **AI answers**: the engines answer the buyer's questions and name competitors. Competitors named and the user not is a gap worth closing now; nobody named means the category is early.
- **Reddit**: communities where the problem comes up every week, and threads that ask for a recommendation. Count in the full window of `reddit_get_new_posts`, not from search, which is a sample (see the [Reddit notes](../../create-reddit-plan/references/platforms/reddit.md)).
- **Short video**: TikTok or YouTube videos on the category in the last month with views in the tens of thousands.
- **LinkedIn**: a B2B buyer, and posts about the problem in the last month that draw comments.
- **Paid**: competitors with ads still running (`active: true` on Meta, `last_shown` within 7 days elsewhere) whose `first_shown` is 90 or more days ago. Advertisers rarely keep paying for a losing ad for three months.
- **Outbound**: a B2B buyer and an ICP a company search can express (industry, headcount, location). As a rule of thumb, above about $10,000 a year it is a core channel; between $1,000 and $10,000, small and founder-sent; below $1,000, a sales conversation costs more than the customer pays. The same bands set the motion in [create-gtm-plan](../../create-gtm-plan/SKILL.md).
