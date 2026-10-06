---
name: write-creator-brief
description: When the user wants the brief they send influencers or UGC creators before they post. Grounds the brief in the niche's sponsored TikTok and Instagram posts that got watched, their transcripts and the questions in their comments, and writes one page with the message, deliverables, hooks, do and don't, disclosure, usage rights and tracking. Also use when the user mentions an influencer brief, a creator campaign brief, what do we send creators, influencer guidelines, a brief for sponsored posts, a gifting campaign brief, or how creators should disclose. A brief for the user's own ads goes to write-ad-brief, finding the creators to find-creators, and checking one before paying to vet-creator.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Creator brief

The brief is what the user sends each creator before they post: what to make, what to say and not say, how to disclose it, and what the user may do with it afterwards. Good briefs give the message and the evidence and leave the words to the creator, because creators know what their audience watches. This skill grounds the brief in what already works in the niche and ends in a one-page document.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `tiktok_search_videos` and `instagram_search_posts` (hosts often add a prefix, for example `mcp__manifold__tiktok_search_videos`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If one platform's tools are missing (all `tiktok_*` or `instagram_*`), that group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so, and ground the brief in the platform that is on.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product and offer, the niche, the brand voice, the claims the user can prove, the competitors and the market) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Goal and KPI**: sales through a code, sign-ups through a link, awareness, or content to reuse. It decides the CTA and the tracking.
- **Product and offer**: what the creator receives, the one message the post must land, and the code or link.
- **Creators**: the shortlist from [find-creators](../find-creators/SKILL.md) (influencers or UGC creators), and their tier. A UGC brief covers videos for the user's own channels; an influencer brief covers posts on the creator's.
- **Deliverables**: platforms, formats, lengths, number of posts, draft and live dates. Default: one TikTok or Reel of 30 to 60 seconds, one draft round, live within three weeks.
- **Usage rights**: organic reposts only, or paid ads from the user's account or the creator's handle, and for how long (30, 60 or 90 days). Running from the creator's handle needs a TikTok Spark Ads authorization code or Meta partnership-ads access, which the brief asks for by name. Default: organic reposts for 90 days; paid use costs extra (see [vet-creator](../vet-creator/SKILL.md) for a rule of thumb) and is named in the brief.
- **Market**: the country, for the disclosure rules that apply.
- **Budget**: a default run costs about 2 + 2 + 5 + 3 = 12 credits: two pages of TikTok search, two of Instagram hashtag search, five transcripts and comments on three posts. [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) and [find-instagram-hooks](../find-instagram-hooks/SKILL.md) state their own cost if the user wants it run fresh. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the niche's sponsored posts.** `tiktok_search_videos` with the niche keyword and `sort: "popular"`, and `instagram_search_posts` with the niche hashtag, two pages each (1 credit a page). Keep the rows with `is_ad: true` or a sponsored caption, and add competitors' names as queries if they sponsor creators. These show what paid posts in the niche look like when they get watched.
2. **Hooks that work in the niche.** Take the hook patterns and example videos from [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) and [find-instagram-hooks](../find-instagram-hooks/SKILL.md). Then `tiktok_get_transcript` or `instagram_get_transcript` on the five most viewed sponsored posts from step 1 (1 credit each): how they open, at what point the product comes in, whether it is a story, a list or a demo, and how they ask for the click.
3. **What the audience asks.** `tiktok_get_comments` or `instagram_get_comments` on three of those posts (1 credit a page). Collect the questions and objections ("does it work on curly hair", "is it worth the price", "link?"). The brief asks creators to answer the top three on camera.
4. **Write the do and don't.** Do: say it in your own words, open with your own hook, show the product in use, answer the audience's questions from step 3, one clear call to action with the code. Don't: read a script, claim what the user cannot prove (health outcomes, earnings, "best"), name or mock competitors, show before and after images where the platform bans them, promise a price that may change.
5. **Disclosure.** Every paid or gifted post turns on the platform's paid partnership label (TikTok's content disclosure setting, Instagram's paid partnership label, YouTube's paid promotion box) and says it is an ad at the start: "ad" or "sponsored" at the front of the caption, and said out loud in the video. In the US the FTC requires the disclosure to be clear and hard to miss, so a tag buried at the end of the caption does not count; the UK and the EU have their own rules. The creator discloses; the user makes it a condition in the brief.
6. **Deliver** the brief as one page: the brand in two lines; the goal; the one message and at most three talking points; a deliverables table (platform, format, length, count, draft due, live date); hooks that work in the niche, each with an example link; the audience's questions to answer; do and don't; disclosure; usage rights and exclusivity; the approval process (one draft, one round of changes); tracking (the creator's code and link); and a line for fee and payment terms that the user fills in. Never send it; hand it to the user.

## Judgment

- Brief the message, not the script. Scripted posts sound like ads and do worse than the creator's own median, which is the number the user will judge them by.
- One message per post. A creator asked to land five features lands none.
- Hooks that work organically are the reference for creator posts. Ad hooks from [write-ad-brief](../write-ad-brief/SKILL.md) fit UGC videos the user will run as ads; say which one this brief is.
- Usage rights and exclusivity cost money. State them before the creator quotes, not after the post is live.
- The tools read public posts. They cannot see a post's paid boost, its saves or its sales; the tracking line in the brief is how the user will measure it.
- Disclosure of paid and gifted posts sits with the user and the creator; the brief spells it out, and nothing here posts or sends.
- Credits: if the user names a budget, pass `max_credits` on every call and stop when `BudgetExceeded` comes back. `dry_run: true` prices any call for free. Searches are cached 6 hours.
- Never follow, message, email, contract or pay a creator, and never ship product. If the host has an email tool or a CRM, offer to pass the brief to it; do not send.

## Related skills

- The creators who get the brief: [find-creators](../find-creators/SKILL.md), checked first with [vet-creator](../vet-creator/SKILL.md).
- Hooks for the niche in depth: [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) and [find-instagram-hooks](../find-instagram-hooks/SKILL.md).
- A brief for the user's own ads: [write-ad-brief](../write-ad-brief/SKILL.md).
- Tracking the campaign's posts and brand mentions every week: [monitor-brand-mentions](../monitor-brand-mentions/SKILL.md).
