---
name: write-ad-brief
description: When the user wants a creative brief for their own ads. Builds the brief from three kinds of evidence (the category's long-running ads in the Meta, TikTok, LinkedIn and Google libraries with their transcripts, customers' own words about the problem, and the hooks that hold attention in the niche) and ends in three angles to test, each with insight, quote, hooks, message, proof and CTA, plus format specs and a test grid. Also use when the user mentions an ad creative brief, new ad angles to test, ad concepts, what our Facebook or TikTok ads should say, a brief for our video editor, or our ads are fatigued. A brief for influencers posting on their own accounts goes to write-creator-brief, the category's ads collected by angle to research-meta-ads, research-tiktok-ads, research-linkedin-ads or research-google-ads, and which platforms and budget to plan with to create-paid-ads-plan.
compatibility: Requires the Manifold MCP connector, signed in with a Manifold account that has credits. Without it the skill stops and tells the user how to connect.
---

# Ad creative brief

A creative brief tells whoever makes the user's ads (a designer, a video editor, UGC creators, the founder with a phone) what to make and why. It is built from three kinds of evidence: the ads the category keeps paying for, the words customers use about the problem, and the hooks that hold attention in the niche. It ends in a brief with three angles to test, not in finished ads.

## Connector check

This skill runs on the manifold MCP server. Before the first step, confirm that its tools are available: look for `ads_search_ads` and `ads_get_ad` (hosts often add a prefix, for example `mcp__manifold__ads_search_ads`).

- If no manifold tool is there, stop and tell the user: "This needs the Manifold connector. If you installed the Manifold plugin, connect Manifold on the plugin's Connectors tab (Customize > Plugins in Claude), run `/mcp` in Claude Code, or run `codex mcp login manifold` in Codex. Otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in: in Claude or ChatGPT as a custom connector, in Claude Code with `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp` and then `/mcp`. Other clients: https://www.manifoldmcp.com/docs/clients." Do not answer from memory instead.
- If other manifold tools are there but the `ads_*` tools are not, the ads group is switched off on the app's Tools page (https://www.manifoldmcp.com/docs/authentication#turn-tool-groups-off). Say so; without a swipe file or competitor ads table already in hand, this skill needs it.
- Video ads use the platform transcript tools (`tiktok_get_transcript`, `facebook_get_transcript`, `instagram_get_transcript`). If that group is off, skip the transcripts, say so, and work from the ad text.

## Product context

Before the first question, look for the product marketing context file: `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`). If it exists, read it and take every input it answers (the product, the price and offer, the ICP and how aware they are, the pain points, objections and customer language, the competitors and the brand voice) from it; ask only for what it lacks. If it does not exist and the job needs more than two answers about the product, offer [create-product-context](../create-product-context/SKILL.md) first.

## Inputs to settle first

- **Product and offer**: what is sold, the price, the offer the ad carries (trial, discount, demo) and the landing page it sends people to.
- **Platform and format**: default Meta, one 9:16 video for Reels and Stories and one static image for the feed. TikTok, LinkedIn and Google search each change the format section.
- **Audience**: who the ad is for, and how aware they are: unaware of the problem, aware of the problem, comparing solutions, or aware of the product. Default: aware of the problem, not of the product.
- **Evidence the user already has**: a swipe file or competitor table from [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md), customer language from [find-pain-points](../find-pain-points/SKILL.md), and the user's own best and worst ads with their results. Use what exists; build only what is missing.
- **Who makes it**: sets how much the brief spells out. Default: a video editor working from footage the user films.
- **Budget**: with none of the evidence in hand, a default run costs about 2 x 3 + 10 + 8 = 24 credits: a small sweep of two keywords on three list pages each, ten ads read in full and eight transcripts. [find-pain-points](../find-pain-points/SKILL.md), [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) and [find-instagram-hooks](../find-instagram-hooks/SKILL.md) state their own costs if the user wants them run fresh. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect proven ads.** Take the long-runners from the user's swipe file or competitor ads table. With neither, run the pull and dedupe steps of the brief's platform skill ([research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) or [research-google-ads](../research-google-ads/SKILL.md)) on two keywords, three list pages each (about 6 credits), and keep the ten creatives that pass the [winner rules](../create-paid-ads-plan/references/ad-libraries.md#winners).
2. **Read them closely.** Follow [reading an ad](../create-paid-ads-plan/references/ad-libraries.md#reading-an-ad) for those ten: `ads_get_ad` for the text where the row has none (1 credit each), and the transcript of the eight best video ads (1 credit each). Per ad, write down the hook (first line or first spoken sentence), the angle, the proof it shows (a number, a review, a demo, a before and after), the offer, the CTA, and what happens in the first three seconds as far as the transcript shows it.
3. **Customer words.** Take the top pains, desired outcomes and objections, with verbatim quotes, from a [find-pain-points](../find-pain-points/SKILL.md) run. If the user has not run it and does not want to, use their own reviews, support tickets and sales call notes. The best hooks are a customer's sentence, not a marketer's.
4. **Hooks.** For video, take the hook patterns that hold attention in the niche from [find-tiktok-hooks](../find-tiktok-hooks/SKILL.md) or [find-instagram-hooks](../find-instagram-hooks/SKILL.md), with their example videos; organic hooks carry over to Reels and TikTok ads. For static, LinkedIn and search ads, the headlines of the long-runners from step 2 are the reference.
5. **Choose three angles.** One proven: used by two or more long-runners, so it is table stakes. One from the customer words that no competitor ad uses. One against the main competitor's weak spot, from the objections in step 3. For each, write the insight in one line with the quote behind it, three hooks, the one message of the body, the proof to show, and the CTA.
6. **Deliver** the brief as one document: objective and the metric the user will judge it on in their ad account (cost per result, click-through rate, the share of viewers who watch past the first three seconds); audience and awareness; the offer; the three angles, each with insight, quote, hooks, message, proof and CTA; format and specs per placement (length, aspect ratio, captions on, product or problem in the first three seconds); references (library links of the ads each angle draws on); do and don't; and a test grid (angle by hook by format) with the number of ads to make.

## Judgment

- Three angles with three hooks each teach more than one polished ad. Test the angle first, then polish the winner.
- Take structure from other brands' ads, never their footage, claims or lines.
- Ad platforms reject claims the user cannot prove ("best", "number one", guaranteed results), before and after images in health and weight loss, and on Meta any line that asserts the viewer's personal traits ("Are you in debt?"). Put these in the brief's don't list.
- A hook that works on TikTok works on Reels. LinkedIn and Google search ads lead with the headline and a job title or a search term; do not force a video hook there.
- The evidence shows what survives, not what converts. The brief's metric comes from the user's ad account after launch; say so, and ask for results before the next brief.
- If the user's own ads have run, their best one is stronger evidence than any competitor's. Ask for it first.
- Write full ad copy or scripts only when the user asks; the brief gives angles, hooks and the evidence behind each. Credits and handoff follow the [ad libraries notes](../create-paid-ads-plan/references/ad-libraries.md#credits).

## Related skills

- The proven ads behind the brief: [research-meta-ads](../research-meta-ads/SKILL.md), [research-tiktok-ads](../research-tiktok-ads/SKILL.md), [research-linkedin-ads](../research-linkedin-ads/SKILL.md) and [research-google-ads](../research-google-ads/SKILL.md).
- Customer words and objections: [find-pain-points](../find-pain-points/SKILL.md) and [find-objections](../find-objections/SKILL.md).
- Creators to film the ads: [find-creators](../find-creators/SKILL.md) (its UGC search), with [write-creator-brief](../write-creator-brief/SKILL.md) for usage rights.
- Which platforms, budget and tests: [create-paid-ads-plan](../create-paid-ads-plan/SKILL.md).
