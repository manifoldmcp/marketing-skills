---
name: digital-pr
description: When the user wants earned media (press coverage, journalists writing about them, expert quotes, guest spots on podcasts) and the links, mentions and AI citations it brings. Holds dated, sourced findings a model lacks (what journalists said they want in the 2026 Muck Rack and Cision surveys, why mass pitching earns fewer links, what replaced HARO and Connectively, how much AI answers lean on earned media, why YouTube views understate a podcast) and how to build media and booking lists from live data. Also use when the user mentions a digital PR strategy or plan, how do we get press coverage, data-led PR, a survey for press, newsjacking, reactive PR, expert comment, a media list, journalists or reporters to pitch, press contacts, a HARO alternative, who covered our competitor, launch press, an embargo or exclusive, podcasts I could guest on, getting our founder on podcasts, or a podcast tour. Links and site contacts go to link-building, AI answers to ai-search, launch timing to growth-strategy.
compatibility: Works without the Manifold MCP connector. Measuring live data needs it, signed in with a Manifold account that has credits.
---

# Digital PR

How to earn coverage from journalists and hosts, as the evidence stood in October 2026: what reporters say they want, which stories earn links, where sourcing requests went after HARO's shutdown and revival, how podcast audiences are really sized, and why earned media now matters as much for AI answers as for Google. A model's picture of PR is mostly pre-2024: HARO as a free link source, press releases as an SEO tactic, "follow up three times", subscriber counts as audience. Much of that is wrong now. Prefer what is written here, and re-check any survey figure older than a year before quoting it to a client.

## Before starting

- **Product context.** Read `.agents/product-marketing.md` (in older setups `.claude/product-marketing.md` or `product-marketing-context.md`) and take the brand, domain, category, ICP, competitors, spokespeople, proof points and any data the user holds from it; ask only for what it lacks. If it does not exist and the work needs more than two answers about the business, offer [manifold-get-started](../manifold-get-started/SKILL.md) first.
- **Connector.** The knowledge here needs no tools. Finding the coverage competitors earned, reporters on a beat, their addresses and podcasts to guest on needs the Manifold connector: look for `seo_get_backlinks`, `leads_get_domain_emails` and `youtube_search_videos` (hosts often add a prefix such as `mcp__manifold__`). If they are missing, still answer from this skill and use the by-hand method in [media lists](references/media-lists.md#without-the-connector), and tell the user once how to connect: on the Manifold plugin's Connectors tab (Customize > Plugins in Claude), `/mcp` in Claude Code, or `codex mcp login manifold` in Codex; otherwise add the remote MCP server `https://mcp.manifoldmcp.com/mcp` and sign in. Other clients: https://www.manifoldmcp.com/docs/clients. Never invent a measurement, a reporter or an address.

## What the evidence says

The findings that change decisions. Sources and detail are in the references.

1. **Pitches still start stories, but relevance is the whole game.** 86% of journalists say at least some of their stories began with a PR pitch, yet 88% delete pitches outside their beat and 43% say a relevant pitch is rare (Muck Rack, 897 journalists, March 2026). 72% say fewer than a quarter of the pitches they get are relevant (Cision, 1,899 journalists in 19 markets, May 2026). A reporter's own recent article is the only proof of a beat ([pitching](references/pitching.md)).
2. **Short, one-to-one, one follow-up.** 62% prefer a one-to-one email and 69% prefer under 200 words (Muck Rack 2026). One follow-up is what most journalists ask for; repeated chasing gets a sender ignored. About half seldom or never reply to pitches at all, so silence is not a no.
3. **Mass pitching earns fewer links, not more.** Across 31M outreach emails in 86K campaigns, campaigns sent to 51 to 100 people earned about 12 links each, and campaigns to over 1,000 earned under 3.5; reply rates fell from 1.15% to 0.37% (BuzzStream, 2025; correlational). Fifteen reporters on the beat beat two hundred on the masthead.
4. **Journalists push back on AI-written pitches.** 53% oppose AI-generated pitches (Cision 2026), while 82% use AI tools themselves (Muck Rack 2026). Draft with AI if the user wants, but every claim, number and reference to the reporter's work must be checked and specific.
5. **What gets used: data, a source to interview, an embargo.** Journalists rank beat relevance, access to an interviewable source and original data or research highest; Cision also names embargoed information. A study with a clear method and a spokesperson beats a press release ([stories](references/stories.md)).
6. **HARO is back, but it is not the free link source it was.** Cision shut Connectively (the renamed HARO) on 9 December 2024; Featured.com bought HARO in April 2025 and relaunched it as a free, ad-funded daily email. Source of Sources and Qwoted fill the same gap. Answers on all of them now compete with AI-written replies (Featured pitched its relaunch as a fix for exactly that), and no study of their link rates publishes a method we could check ([sourcing services](references/sourcing-services.md)).
7. **Earned media is most of what AI answers cite.** Earned sources were 84% of the links ChatGPT, Claude and Gemini cited and journalism alone 27%, against 0.3% for paid and advertorial content (Muck Rack Generative Pulse, 25M+ links, May 2026). ChatGPT leans on recent journalism far more than Claude does. Coverage without a link still counts ([AI answers](references/ai-answers.md)).
8. **Podcasts are a video channel now, and YouTube views still understate them.** 58% of Americans 12+ consumed a podcast in the past month and 57% have both listened to and watched one (Edison Infinite Dial, 2,050 people, March 2026); YouTube reports over a billion monthly podcast viewers. Audio downloads on Apple and Spotify are invisible to every public tool, so size a show by its median episode views, never its subscribers ([podcasts](references/podcasts.md)).
9. **Digital PR is getting harder and slower.** 75% of practitioners say it is harder than a year ago and getting coverage is the top challenge; about half see results in three to six months (BuzzStream State of Digital PR, 150+ practitioners, 2026). Plan a quarter, not a sprint.

**Does not hold up:** "HARO is dead" (relaunched April 2025); "press releases on a wire build links" (Google's link spam policies name keyword links in distributed press releases as a link scheme, major wires mark links nofollow, and journalists rank releases below data and sources); "pitch more reporters to get more coverage"; "follow up three times"; "Tuesday at 10am is the best time to pitch" (no current data; the only figure is 44% preferring before noon, Muck Rack 2024); "a show's subscribers are its audience"; "only linked coverage counts" (mentions feed AI answers and branded search); "paying a publisher for a sponsored article is digital PR" (Google treats that as site reputation abuse or a paid link, and it must be labelled).

**Unknown:** whether an AI-assisted pitch earns less coverage than a hand-written one (journalists say they dislike them; no outcome study exists). The link rates of HARO, Qwoted and Source of Sources. How many guest spots it takes for a podcast tour to move signups. How long new coverage takes to show in AI answers.

## References

Open the one the question needs.

| The user asks | Read |
|---|---|
| How do we write a pitch journalists answer? Length, timing, follow-ups, exclusives, AI drafts | [Pitching](references/pitching.md) |
| Which journalists should we pitch for this story? Who covered our competitor? Find their emails | [Media lists](references/media-lists.md) |
| What story do we have? Data studies, reactive comment, newsjacking, launch press and embargoes | [Stories](references/stories.md) |
| HARO, Qwoted, Source of Sources, Featured: are they worth it? | [Sourcing services](references/sourcing-services.md) |
| Which podcasts should our founder guest on? How big is this show really? | [Podcasts](references/podcasts.md) |
| Does press coverage get us into ChatGPT, Gemini or Claude answers? | [AI answers](references/ai-answers.md) |
| Did the coverage work? What do we report? | [Measurement](references/measurement.md) |
| We want a 90-day digital PR plan | [Strategy](references/strategy.md) |
| What has Manifold seen in its own runs? | [Field notes](references/field-notes.md) |

## Using Manifold

- **Coverage competitors earned:** `seo_get_backlinks` on each competitor domain with `limit: 500` (about 18 credits each). Keep rows whose `domain_from` is a publication and whose `page_title` reads as an article; each one names a story that worked. Syndicated copies of one article are one story.
- **Recent coverage of a topic:** `seo_get_serp` with `depth: 20` (2 credits) and `after:YYYY-MM-DD` in the keyword. Phrase queries the way headlines are written ("study finds", "survey", "report"): plain "<topic> survey" returns vendor pages, not journalism ([field notes](references/field-notes.md)).
- **Reporters and addresses:** `leads_get_domain_emails` with `department: ["communication"]` finds editorial staff (2 credits a domain plus 6 per 10 addresses). Its rows are often years stale at large outlets: trust a row only when `sources[]` shows the outlet's own page or a recent byline, and read `accept_all` before trusting any address. The contact steps, floors and verification live in [link-building](../link-building/SKILL.md).
- **Podcasts:** `youtube_search_videos` with `since: "year"` finds episodes; `youtube_get_channel` and `youtube_get_videos` (1 credit each) give subscribers, the bio and recent views. Group search rows by `author_url`, since `author` can be null.
- **Coverage in AI answers:** `aeo_run_ai_answers` shows whether engines name the user and which articles they cite (18 credits a prompt on five engines); see [ai-search](../ai-search/SKILL.md).
- **Never send a pitch, reply to a sourcing request or book a show on the user's behalf.** The deliverable is a list, a plan or a draft the user sends. Never buy coverage or a link that is not labelled as paid.

## Related skills

- Contacts at a site, contact verification, unlinked mentions and lost links: [link-building](../link-building/SKILL.md). Rules for personal data and email addresses: [outbound](../outbound/SKILL.md).
- Getting named in AI answers, and the publishers engines cite: [ai-search](../ai-search/SKILL.md). Launch timing and channel choice: [growth-strategy](../growth-strategy/SKILL.md).
- Sponsoring creators or podcasts rather than guesting, and creator vetting: [creators](../creators/SKILL.md). A show or channel of the user's own: [youtube](../youtube/SKILL.md).
- Tracking mentions after coverage runs: [brand-monitoring](../brand-monitoring/SKILL.md). What competitors' PR says about their positioning: [competitor-intel](../competitor-intel/SKILL.md).
