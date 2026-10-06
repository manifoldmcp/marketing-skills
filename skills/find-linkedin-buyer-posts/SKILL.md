---
name: find-linkedin-buyer-posts
description: When the user wants to find people on LinkedIn posting about the problem they solve. Searches LinkedIn posts for people asking for a recommendation, switching off or complaining about a competitor, or describing the pain in their own words, qualifies each author against the ICP from their profile, and suggests a first move that helps rather than pitches. Also use when the user mentions people on LinkedIn complaining about a problem, LinkedIn posts asking for a tool like ours, people on LinkedIn frustrated with a competitor who might switch, or social selling to buyers with the problem. Posts to comment on as a routine go to find-linkedin-posts-to-comment, topic leaders to find-linkedin-topic-leaders, emails for the people found to enrich-lead-list, and pain points ranked across sources to find-pain-points.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find LinkedIn buyer posts

People post on LinkedIn about the problem the user solves: asking for a recommendation, complaining about a tool, describing the pain in their own words. This skill finds those posts, sorts them by the signal each shows, and qualifies the authors against the ICP. It ends in a table of posts and authors for the user to engage in public, or to hand on for contact details.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `linkedin_search_posts` and `linkedin_get_profile` (hosts often add a prefix, for example `mcp__manifold__linkedin_search_posts`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `linkedin_*` tools are not, the LinkedIn tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the problem the product solves, the ICP, the competitors, customer language) from it; ask only for what it lacks. Use its customer language as the search phrasings. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first. After delivering, offer to write new customer language into `.agents/product-marketing.md` with [manifold-get-started](../manifold-get-started/SKILL.md).

## Inputs to settle first

- **The problem**: in the buyer's words, not the product's. Five or six phrasings across four kinds: the pain ("chasing invoices"), asking for a tool ("recommend an invoicing tool", "looking for a tool that"), switching ("moving off <competitor>", "<competitor> alternative"), and frustration ("<competitor> support", "hate doing <task>").
- **ICP**: the titles and company types that buy, to keep only the right authors.
- **Competitors**: two or three names for the switching and frustration searches.
- **Window**: default `since: "month"`; `since: "week"` for a fresh list.
- **Budget**: a default run costs about 6 x 2 + 25 + 3 = 40 credits. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the phrasings.** `linkedin_search_posts` for each phrasing with the window, two pages (1 credit a page). Search is LinkedIn's own ranked search, read the way the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md#what-linkedin-shows) say.
2. **Read and sort each post.** From the `text`: asking for a recommendation (hot), switching or complaining about a competitor (hot), describing the pain (warm), discussing the topic in general (drop), and a vendor, consultant or recruiter marketing to that pain (drop). Keep the sentence that shows the signal.
3. **Qualify the authors.** Trace each author's handle from the post URL per the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md#what-linkedin-shows) and call `linkedin_get_profile` on the hot and warm authors, up to 25 (1 credit each). From the `bio` and `location`: do they fit the ICP (role, company type, market)? Drop those who do not.
4. **Weigh the post.** `likes` and `comments` on the row (or `linkedin_get_post`, 1 credit, when null). A complaint with many likes means many people share it: a theme for content, even when its author is not a buyer.
5. **Deliver** a table: author, profile URL, role and company (from the `bio`), post URL, posted (approximate), signal (asking, switching, complaining, pain), the quote, likes and comments, and a suggested first move: a helpful public comment, or a direct message from the user that answers the post, not a pitch. Write no comment or message unless the user asks; then follow the [handoff](../create-linkedin-plan/references/platforms/linkedin.md#handoff) in the LinkedIn notes.

For email addresses and company records, hand the table to [enrich-lead-list](../enrich-lead-list/SKILL.md), which adds contact details to a list of names; [find-buying-signals](../find-buying-signals/SKILL.md) combines these posts with other buying signals. Do not look up emails here.

## Judgment

- Every LinkedIn call follows the [LinkedIn notes](../create-linkedin-plan/references/platforms/linkedin.md): what the tools show, the [thresholds](../create-linkedin-plan/references/platforms/linkedin.md#thresholds), the credits and the handoff.
- A public request for a recommendation is the strongest signal LinkedIn has, and it goes stale in days. Recent hot posts go first.
- People who complain in public want to be heard, not sold to. The first move is help in the thread; a pitch in reply to a complaint usually backfires. Never a link to the product or "great post".
- Never post, comment, like, connect or send a message. The deliverable is a list with a link to every post and person; the user acts from their own account.
- The comments on a post cannot be read here, so competitors' replies and other people with the same problem are invisible. The user opens the post and reads the thread before acting.
- Posts with the problem but no fitting author still count: the likes and the wording show which pain resonates, which is content for a LinkedIn plan in [create-linkedin-plan](../create-linkedin-plan/SKILL.md).
- Search is ranked and incomplete. The list is a sample of who is talking, not everyone with the problem.
- `created_at` is approximate to the unit LinkedIn shows, which is enough for "today" and "this week".
- A list on a schedule, every morning or every week, is the host's job, the way [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md) runs: the host runs this skill on its schedule and keeps the post URLs already listed. The server keeps no state.

## Related skills

- Recent posts by leaders or buyers to comment on: [find-linkedin-posts-to-comment](../find-linkedin-posts-to-comment/SKILL.md). The leaders of a topic: [find-linkedin-topic-leaders](../find-linkedin-topic-leaders/SKILL.md).
- Email addresses and company records for the people found: [enrich-lead-list](../enrich-lead-list/SKILL.md). These posts combined with other buying signals: [find-buying-signals](../find-buying-signals/SKILL.md).
- The problems behind the posts, ranked across sources: [find-pain-points](../find-pain-points/SKILL.md). The same engagement on Reddit: [find-reddit-threads](../find-reddit-threads/SKILL.md).
- A plan for the user's own LinkedIn: [create-linkedin-plan](../create-linkedin-plan/SKILL.md).
