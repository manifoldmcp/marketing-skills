---
name: find-linkedin-post-formats
description: When the user wants to know which LinkedIn post formats work in their niche. Codes a sample of real LinkedIn posts by length, hook, structure and ending, compares the median likes and comments of each, per 1,000 followers where it can and gives rules for the next post, with the user's own posts placed against the niche. Also use when the user mentions what kind of LinkedIn posts work, how long a LinkedIn post should be, hooks that work on LinkedIn, LinkedIn post structure, which LinkedIn posts get engagement, or why our LinkedIn posts get no reach. Ideas across channels go to find-content-ideas; YouTube video ideas to find-youtube-video-ideas; who leads the conversation to find-linkedin-topic-leaders; posts to comment on to find-linkedin-posts-to-comment; a LinkedIn plan to create-linkedin-plan; TikTok hooks to find-tiktok-hooks.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find LinkedIn post formats

Which post structures, hooks and lengths get engagement in the user's niche, measured on a sample of real posts rather than on generic LinkedIn advice. It ends in a table of format features with the median engagement of each, example posts, and a few rules the user can apply to the next post.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `linkedin_*` tools are not, that tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop: this skill needs them.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the niche, the ICP, customer language, the brand voice, the user's LinkedIn profile or page) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Niche**: four or five searches the ICP's world posts about. The topic leader searches from [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md) serve if they were run.
- **Sample**: default 80 posts or more, `since: "month"`; add `since: "year"` in a quiet niche.
- **The user's own posts**: the URLs of their last 10, to compare against the niche. Optional.
- **Budget**: a default run costs about 5 x 3 + 10 + 30 + 10 = 65 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Read the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md)** before the first call. They say what LinkedIn's rows show and what each call costs.
2. **Collect the sample.** `linkedin_search_posts` for each search, three pages (1 credit a page). Keep rows with `text`, `likes` and `comments`; drop duplicates and company-page posts unless the user runs a page. For rows with null counts, `linkedin_get_post` (1 credit each).
3. **Size the authors.** Trace the handles of the 30 most frequent authors per the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md#what-linkedin-shows) and call `linkedin_get_profile` (1 credit each) for `followers`, so engagement can be compared per 1,000 followers.
4. **Code each post from its text.**
   - Length: under 500 characters, 500 to 1,300, 1,300 to 2,000, and cut (the `text` stops at 2,000, so a longer post is only known as long).
   - Hook, the first line: a number or a result, a contrarian claim, a story opening, a question, a how-to promise, or news.
   - Structure: a list or numbered steps, a story, a one-liner, a framework, a lesson list, a before-and-after.
   - Ending: a question to readers, a link, a "comment X to get it" giveaway, or none.
   - Other: a link in the text, tags of people, hashtags.

   The media is not visible (every row reads `media: "text"`), so carousel, image, video and poll are not codes here. If the host has a browser it can open the top 20 post URLs and add the media type; otherwise the user checks them by eye.
5. **Compare.** For each feature, the median likes and comments and, where followers are known, the median per 1,000 followers. Leave giveaways out of the comment medians per the [thresholds](../create-linkedin-plan/references/platforms/linkedin.md#thresholds), and compare only groups with at least 10 posts.
6. **Place the user.** When the user gave URLs, `linkedin_get_post` on each (1 credit): code them the same way and show where they sit against the niche's medians.
7. **Deliver** a table: feature, group, posts, median likes, median comments, median engagement per 1,000 followers, and an example post (URL, first line). Under it, three to five rules for this niche ("posts of 800 to 1,300 characters with a numbered list get twice the median"), each with its evidence, and how the user's own posts compare.

## Judgment

- The sample is search results, which favour posts that already did well. It shows what top posts share, not what every post gets. Say so.
- These are correlations. Big authors write in their own style; per-1,000-follower numbers reduce the effect but do not remove it.
- A giveaway post buys comments. Leaving it in makes "question endings" and "long posts" look better than they are.
- Hooks matter most because LinkedIn shows two or three lines before "see more". Quote real first lines in the table rather than describing them.
- Rules differ by niche. A recruiting audience and a developer audience reward different posts; do not carry this table to another niche.
- Links in the post text are worth a column: many authors in a niche avoiding them is itself a finding.
- Compare likes and comments only within this sample. The tools see engagement, not what converts; say so when the user asks which format will bring customers.
- **Credits.** Say the estimate before the first paid call. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. LinkedIn searches, posts and profiles cost 1 credit a page or a call; a result this account already paid for is free for 6 hours.
- **Handoff.** The deliverable is a table of evidence. When the user asks for drafts, the host writes them on a format the table shows. Never pass off a creator's post or words as the user's. Never post, comment or schedule.

## Related skills

- Ideas across channels: [find-content-ideas](../find-content-ideas/SKILL.md). Four weeks of dated posts: [create-content-calendar](../create-content-calendar/SKILL.md).
- Who leads the conversation in the niche: [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md). Posts worth commenting on: [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md).
- A LinkedIn plan for a founder or a page: [create-linkedin-plan](../create-linkedin-plan/SKILL.md). A company page audited: [audit-linkedin-page](../audit-linkedin-page/SKILL.md).
- One video or post turned into LinkedIn posts: [repurpose-content](../repurpose-content/SKILL.md).
