# Content calendar

A calendar turns ideas into four weeks of dated posts the team can actually make. It takes an idea table, the channels and the team's hours, and lays out what goes where, when and by whom. It never schedules or posts: the host keeps the calendar and the user, or their own scheduler, publishes.

## Inputs to settle first

- **Ideas**: the table from [content ideas](content-ideas.md), or the user's own list. Without one, run content ideas first and say it adds about 68 credits.
- **Channels**: one to three, from [channels](channels.md) or the user. More than three for a small team is the first thing to cut.
- **Capacity**: who makes content and how many hours a week. Default: one person, 4 hours a week, writing only.
- **Effort per format**: rough defaults to replace with the team's own times: a LinkedIn or X text post 45 minutes, a Reddit reply 20 minutes, a carousel 2 hours, a newsletter issue 2 hours, a short video 90 minutes, an article 5 hours, a YouTube video 8 hours.
- **Fixed dates**: a launch, an event, a holiday, days the team does not post. Default start: next Monday.
- **Budget**: about 8 credits for the optional cadence check (two competitors, up to four listings each, 1 credit a page). Reading `trend[12]` from the idea table costs nothing more. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Turn hours into slots.** Divide the weekly hours by the effort of each format on each channel. Plan to 80 percent of the hours: the rest absorbs slips and one reactive post.
2. **Check the cadence around the user (optional).** For one or two competitors per channel, one listing each (1 credit a page): `linkedin_get_company_posts`, `twitter_get_tweets`, `youtube_get_videos`, `tiktok_get_videos` or `instagram_get_posts`. Count posts per week from `created_at` over the page and note the days they post; LinkedIn dates are approximate, so count LinkedIn posts per month instead. It shows how often a channel's regulars post, which is a floor for being noticed, not a target to match.
3. **Set the pillars.** Group the ideas into two to four recurring themes (for example: how-to answers, opinions on the category, customer stories, product). Each week touches each pillar once where the slots allow.
4. **Place the anchors.** One anchor piece a week: the highest-ranked idea that fits the main channel. Its cut-downs go on the other channels in the days after it, mapped with [repurposing](repurposing.md). Ideas with a rising `trend[12]` or tied to a fixed date go in the first weeks.
5. **Fill and balance.** Fill the remaining slots by idea rank. Keep product posts to about one in four: people follow a channel for answers, and the other three earn the attention the fourth spends. Leave one open slot a week for news or a reply to something that takes off.
6. **Check the load.** Sum the effort per week. No week goes over the planned hours; if one does, move a slot or swap a format for a cheaper one, never add hours the team does not have.
7. **Deliver** a table: week, date, day, channel, format, idea (working title), pillar, evidence (the question and its number, from the idea table), repurposed from (the anchor, if any), owner, effort in hours, status (idea, drafting, ready, published). Under each week, one line: planned hours against capacity. Drafts only when the user asks; the host writes them from the evidence column.

## Judgment

- A calendar the team cannot keep is worse than a short one. Two posts a week for four weeks beat ten in the first week and none after.
- The tools cannot say when to post. `created_at` shows when accounts published, not when their audience was online. Do not name a best time; the user's own platform analytics have it.
- One anchor feeding four channels is cheaper than four separate ideas and keeps the message consistent. When capacity is under one anchor a week, drop to one channel.
- Keep the evidence column. When a post does well or badly, the team can see which question it answered and which source said people ask it.
- The host keeps the calendar (a doc, a sheet, its notes). The server schedules nothing and sends no reminders. If the host has a scheduling tool, offer to pass the slots to it; do not publish.
- For the next calendar, run [content ideas](content-ideas.md) again: results cached within 7 days are free. To review the past four weeks, list the user's own posts with the same listings as step 2 and compare each post with the channel's median; for X, the [X account audit](x-account-audit.md) does this.
