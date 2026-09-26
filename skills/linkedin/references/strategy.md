# LinkedIn strategy

A LinkedIn strategy settles whose voice carries the account (a founder, the company page, or both), what the ICP already engages with on LinkedIn, and which of this group's playbooks gets the most reach and pipeline for the hours the team has. It ends in a 90-day plan, not a stack of drafts.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Mode**: founder-led (a person's profile), the company page, or both. Ask for the profile URL and the company page. Default: founder-led, with the page resharing.
- **Goal**: pipeline and leads, hiring, awareness, or investors. Default: pipeline, measured by conversations the posts start.
- **Stage**: pre-launch, early customers, or established. It decides whether the story is building in public or results.
- **ICP**: the titles and companies that buy, so the topics are theirs and not the founder's peers'.
- **Budget**: credits for the research (this playbook costs about 60) and hours a week. Default: 500 credits, 3 posts a week and 20 minutes a day for comments.
- **Team**: who posts, who writes, whether employees will reshare. Default: the founder writes and posts, no ghostwriter.
- **Competitors**: two or three companies, and two or three people in the space whose audience overlaps. Default: the companies the user names, and the people from step 3.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 30 credits for both modes.
   - Founder: `linkedin_get_profile` on the founder (1 credit) for `followers` and the `bio`. There is no way to list a person's posts, so ask for the URLs of their last 10 and read each with `linkedin_get_post` (1 credit each): median likes and comments, and how often they post. With no URLs, record the baseline as "not posting" or ask.
   - Company: `linkedin_get_company` on the page and each competitor (1 credit each) for the `bio`, `website`, `industry` and `employees`; `linkedin_get_company_posts` for each (1 credit a page) for posting cadence and topics; `linkedin_get_post` on the page's last 10 posts (1 credit each) for engagement, since the feed carries none.
3. **Gaps.** About 30 credits more.
   - Topic gap: `linkedin_search_posts` for four ICP topics with `since: "month"`, two pages each (1 credit a page). Name the topics and angles with the most engagement, and whether the founder or the page posts on any of them.
   - Voice gap: the authors of the top posts, traced to a profile per the [router](../SKILL.md#what-linkedin-shows) (1 credit each, top 10): their followers and engagement per 1,000 followers against the founder's. These are the people to learn from and comment on.
   - Page gap, in company mode: `linkedin_get_post` on the strongest competitor's last 10 posts (1 credit each): what they post that earns more than the user's page.
4. **Tactics.** Choose two or three from this group and say why each fits the numbers:
   - [Topic leaders](topic-leaders.md) when the founder does not know who owns the conversation: the people to learn from, comment on and partner with.
   - [Post formats](post-formats.md) when the founder posts but engagement is low: the structures, hooks and lengths that work in this niche.
   - [Posts to comment on](posts-to-comment.md) when the founder has under a few thousand followers: comments on bigger posts in the niche reach the audience before the founder's own posts can.
   - [Company page audit](company-page-audit.md) in company mode, or when a competitor's page clearly outperforms.
   - [Problem posts](problem-posts.md) when the goal is pipeline: people already asking for what the user sells.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - By day 30, a posting cadence the founder keeps (default three a week) on the two topics with the most engagement, and a daily list of posts to comment on. By day 60, the formats that worked doubled and one recurring series. By day 90, a second topic, and in company mode the page resharing the founder with its own weekly post.
   - KPIs the tools can measure again later: founder followers (`linkedin_get_profile`), median likes and comments on the last 10 posts (`linkedin_get_post` on each URL; the host keeps the URLs, since no feed can be listed), company page posts a week (`linkedin_get_company_posts`) and their median engagement (`linkedin_get_post`), and how often the founder appears among the authors of the top results for the topic searches (`linkedin_search_posts`). Conversations and leads the posts start are not visible to the tools; the user counts them.
   - **Deliver** one document: the inputs with defaults marked, a baseline table (founder or page against each competitor and leader: followers where published, posts a week, median likes and comments), the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and the three first actions.

## Judgment

- For an early B2B company, a person's posts usually reach far more people than the company page's. Lead with the founder; the page reshares and covers hiring and product news.
- Consistency beats volume. Three posts a week for 90 days beat daily posts for three weeks and then silence.
- Engagement from peers and other founders feels good and rarely buys. Weight topics by what the ICP engages with, and ask the user who writes to them after a post.
- Comments come before followers. A founder with 800 followers grows faster through ten thoughtful comments a day on the right posts than through a fourth weekly post.
- The tools cannot see the founder's feed, the post type, or who engaged. Say what the plan assumes because of it, and let the user fill those gaps with what they see in their own analytics.
