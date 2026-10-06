---
name: find-podcasts
description: When the user wants to appear as a guest on podcasts. Finds video podcasts on YouTube that cover the topic and interview people like the user, sizes each show by the median views its episodes get, checks it is active and books guests like the user, and finds the host or booking contact with a verified address. Also use when the user mentions podcasts I could guest on, a podcast guesting list, getting our founder on podcasts, shows that interview SaaS founders, or a podcast tour. Creators to sponsor go to find-creators, a YouTube channel or podcast of the user's own to create-youtube-plan, journalists to find-journalists and press coverage to create-digital-pr-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Podcast guesting

A booking list: video podcasts on YouTube that cover the user's topic and interview people like the user, sized by the views their episodes actually get, with a contact for each. A guest spot borrows an audience that already trusts the host, with no production on the user's side. It hands back a table of shows with a verified contact each.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `youtube_search_videos` and `leads_get_domain_emails` (hosts often add a prefix, for example `mcp__manifold__youtube_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If the `youtube_*` tools are there but the `leads_*` tools are not, the leads tool group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and deliver the show list with the contacts the channel bios give.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the founder and their story, the topics, the audience, the competitors, the proof points a host would want) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Guest**: who will appear (usually the founder) and why they are worth hearing: a result, a number, a strong opinion, a story. The pitch needs it; ask.
- **Topics**: two or three subjects the guest can talk about for an hour, and the audience that should hear it ("bootstrapped SaaS founders", "HR leaders").
- **Size band**: default shows whose median episode gets 500 to 20,000 views. Smaller shows book guests readily and the host often replies in person; the largest shows book through introductions, not cold pitches.
- **Competitors**: optional. A show that has had a competitor's founder on already books the category.
- **Budget**: a default run costs about 10 + 25 + 25 + 5 + 5 + 10 x 8 + 10 = 160 credits for 25 shows looked at and 10 with a verified contact. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the shows.** `youtube_search_videos` with `since: "year"`, two pages each (1 credit a page), for five queries: "<topic> podcast", "<audience> podcast", "<topic> interview", "<competitor> founder podcast" and "<competitor> CEO interview". Keep rows that read as episodes (podcast, episode, "ep", a number, "with <name>", "ft.") and run over 20 minutes (`duration_s` above 1,200). Group them by channel (`author`) and keep the 25 channels with the most matching episodes.
2. **Size and check activity.** `youtube_get_channel` on each (1 credit): subscribers, `bio`, video count. `youtube_get_videos` with the default sort (1 credit each): the date of the last episode, episodes in the last 90 days, and the median views per episode. Apply the [channel health](../create-youtube-plan/references/platforms/youtube.md#channel-health) rules: drop shows with no episode in 90 days, and size each by its median, not its subscribers. Keep the shows inside the size band.
3. **Check the fit.** From the episode titles: does the show interview guests (names in titles) or is it a solo show (skip)? Are the guests founders, operators or experts like the user? Has a competitor been on? For the top five, `youtube_get_transcript` on one recent episode (1 credit each) shows the host's questions and whether guests may mention their product.
4. **Find the host and the booking contact.** The channel `bio` often names the host, a booking address or the show's website; `website` is null for YouTube channels, so the bio is the source. When the bio names no site, `seo_get_serp` for "<show name> podcast" (1 credit) usually finds it. On the show's domain, run the [contact steps](../create-link-building-plan/references/outreach.md#contact-steps) with `limit: 10`; when you know the host's name, use its step 2 for the host. A booking address from the bio still gets step 3, the verification.
5. **Deliver** a table: show, host, channel URL, subscribers, episodes in the last 90 days, median views per episode, last episode date, fit (one line: a past guest or episode that matches the user), contact name, role, email, verification status, and where the contact came from (bio, show website, domain search).

## Judgment

- These tools see video podcasts on YouTube only. Audio-only shows on Apple Podcasts or Spotify are invisible here, and many shows' audio audience is larger than their YouTube views. Say that the list is a start, and that YouTube views understate a show's reach.
- The median views per episode is the audience; subscribers are history. A show with 200,000 subscribers and 1,500 views an episode is a 1,500-view show.
- A show that had a competitor or a peer on is the best fit there is: the host already believes the topic works for that audience.
- A clips channel, a network channel or a re-upload of someone else's show is not the show. Check the channel name and the bio before contacting.
- Hosts get many pitches. The pitch that works names an episode, proposes one topic with a number or a story behind it, and asks for nothing else. Write it only if the user asks.
- Never send. Hand the table to the user, or to an email or sequencer tool their agent has (see the [YouTube notes](../create-youtube-plan/references/platforms/youtube.md#handoff)).

## Related skills

- Creators to sponsor rather than pitch: [find-creators](../find-creators/SKILL.md). A channel of the user's own: [create-youtube-plan](../create-youtube-plan/SKILL.md).
- Journalists: [find-journalists](../find-journalists/SKILL.md). Press coverage: [create-digital-pr-plan](../create-digital-pr-plan/SKILL.md). Links and a 90-day link plan: [create-link-building-plan](../create-link-building-plan/SKILL.md).
- Mentions the appearances earn, turned into links: [find-unlinked-mentions](../find-unlinked-mentions/SKILL.md).
