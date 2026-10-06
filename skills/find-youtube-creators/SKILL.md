---
name: find-youtube-creators
description: When the user wants to find YouTube creators or channels to sponsor in a niche. Finds channels that cover the niche from what ranks for its YouTube searches, sizes them by the median views their recent videos get rather than by subscribers, checks engagement and declared sponsorships, and ends in a shortlist with a contact where the channel gives one. Also use when the user mentions YouTubers to sponsor, YouTube influencers, YouTube channels in a niche, a sponsored integration or dedicated review on YouTube, or real view counts for YouTubers. Creators across several platforms go to find-creators, TikTokers to find-tiktok-creators, Instagram creators to find-instagram-creators, fans already posting about the brand to find-brand-fans, and vetting one channel to vet-creator.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Find YouTube creators

A shortlist of YouTube channels in the user's niche that are active and watched. The skill finds them from what ranks for the niche's searches, sizes them by the median views their recent videos get rather than by subscribers, and checks sponsorship evidence. It ends in a table with a contact where the channel gives one.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_search_videos`, `youtube_get_channel` and `youtube_get_videos` (hosts often add a prefix, for example `mcp__manifold__youtube_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the manifold tools are there but the `youtube_*` tools are not, the YouTube tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so and stop.
- Contacts at a creator's own website need the `leads_*` tools. If that group is missing, it is switched off: deliver the table with the bio contact only, and say so.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and category, the niche, the ICP, the countries and languages that matter, the competitors) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.

## Inputs to settle first

- **Niche**: the topics the right creators cover, as four to six searches ("<category> tutorial", "<competitor> review", "best <category>", the audience's jobs such as "how to run payroll").
- **Purpose**: a sponsored integration, a dedicated review, an affiliate deal or a long-term partnership. It decides whether sponsorship evidence matters.
- **Size band**: default channels whose median recent video gets 2,000 to 100,000 views. Below that a sponsorship reaches too few people; above it prices usually outgrow a first test.
- **Market and language**: the language the videos should be in. YouTube publishes no audience location here.
- **Count**: default a shortlist of 15.
- **Budget**: a default run costs about 6 x 2 + 30 + 30 + 15 x 2 = 102 credits. Contacts through a creator's own website add about 8 to 10 each. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the niche.** `youtube_search_videos` with `since: "year"`, two pages each (1 credit a page), for the four to six searches. Group the rows by channel (`author`). Keep channels with at least two relevant videos in the results: a channel that covers the niche, not one that hit it once. Stop at about 30.
2. **Size them.** `youtube_get_channel` on each (1 credit): subscribers, video count, `bio`, and `location` where the channel gives it.
3. **Filter by views and recency.** `youtube_get_videos` with the default sort (1 credit each): the median views of the latest 10, the median as a share of subscribers, uploads in the last 90 days and the date of the last upload. Apply the [channel health](../create-youtube-plan/references/platforms/youtube.md#channel-health) rules and the size band. For a sponsorship, also drop channels without an upload in the last 30 days: they may not publish the user's slot on time.
4. **Check engagement and sponsors.** YouTube listings carry views but not likes or comments. For the 15 best, `youtube_get_video` on two recent videos (1 credit each): likes and comments per 1,000 views, and `is_ad`, which is true on a video that declares a paid promotion. A channel with declared promotions already takes sponsors; titles that review competitors show the channel covers the category and may have a deal with one. Over about a third of recent videos sponsored, the audience is used to skipping the read.
5. **Find the contact.** Read the `bio`: most creators who take sponsors list a business address or a site (`website` is null on YouTube, so a site shows in the `bio`). For a creator with their own site and no address in the bio, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) on that domain with `department: ["executive", "marketing"]` and `limit: 10`.
6. **Deliver** a table: channel, URL, subscribers, median views of the latest 10, median as a share of subscribers, uploads in the last 90 days, last upload, likes and comments per 1,000 views, the one or two most relevant videos (URL, views), sponsors or competitors seen, contact and its source, and one line on fit.

## Judgment

- Read every number by the [YouTube notes](../create-youtube-plan/references/platforms/youtube.md): medians over recent videos, never the mean; paid videos stay out of the organic median; a null is left out, not read as zero.
- Price and pick by median views, never by subscribers. A 400,000-subscriber channel with a 6,000-view median is a 6,000-view channel.
- Relevance beats size. A channel of 8,000 subscribers whose every video is about the user's category beats a general tech channel ten times its size.
- Judge a channel's sponsored videos against its own median. Near the median means the audience trusts the creator's recommendations; under half of it means the audience skips their ads.
- YouTube publishes no audience location or age here; only TikTok has an audience split. Use the language of the videos and the `location` of the channel as weak signals, mark them unproven, and ask the creator for their analytics before paying.
- Search surfaces what YouTube ranks, which favours channels already winning, and it is never complete. Small, excellent channels can be missing; a second pass with narrower searches finds more, and searches are cached for 6 hours, so repeats are cheap.
- A channel that reviewed a competitor is a good fit and a risk: it may have an exclusive deal. Ask before assuming.
- A list is not a vetting. Before any fee, run [vet-creator](../vet-creator/SKILL.md) on the shortlist.
- Credits: searches, channels, listings, videos, comments and transcripts cost 1 credit a call or a page. If the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Channels are cached 24 hours, listings and searches 6 hours.
- Never message, comment on, email, contract or pay a creator, and never ship product. Never guess an address. The brief belongs to [write-creator-brief](../write-creator-brief/SKILL.md), and only when the user asks.

## Related skills

- Creators across YouTube, TikTok and Instagram in one table: [find-creators](../find-creators/SKILL.md). The same search on one other platform: [find-tiktok-creators](../find-tiktok-creators/SKILL.md), [find-instagram-creators](../find-instagram-creators/SKILL.md).
- Long-form reviewers who could film for the user's ads: [find-ugc-creators](../find-ugc-creators/SKILL.md). Fans already posting about the brand: [find-brand-fans](../find-brand-fans/SKILL.md).
- One channel vetted in depth: [vet-creator](../vet-creator/SKILL.md). The brief: [write-creator-brief](../write-creator-brief/SKILL.md). A creator program plan: [create-influencer-plan](../create-influencer-plan/SKILL.md).
- Podcasts to appear on: [find-podcasts](../find-podcasts/SKILL.md). A plan for the user's own channel: [create-youtube-plan](../create-youtube-plan/SKILL.md).
