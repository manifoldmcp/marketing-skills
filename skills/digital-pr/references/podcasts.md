# Podcast guesting

Guest spots on other people's shows: a borrowed audience that already trusts the host, an hour of the guest's thinking, no production. How big the channel is, how to judge one show's real audience, and how to find and book shows. Checked October 2026. Sponsoring a show rather than guesting on it is creator work: [creators](../../creators/SKILL.md).

## The channel

- **Reach is at a record.** 58% of Americans 12+ (about 167M) consumed a podcast in the past month and 45% in the past week; 68% of 35 to 54 year olds did monthly (Edison Research Infinite Dial 2026, 2,050 people 12+, fielded January 2026, published 12 March 2026).
- **Podcasts are video now.** 57% of Americans 12+ have both listened to and watched a podcast (Edison 2026). YouTube says it had over a billion monthly podcast viewers in January 2025 and is the most-used podcast service in the US (YouTube, 26 February 2025).
- **Supply is vast and mostly dead.** Listen Notes indexed about 3.82M podcasts and 196M episodes in October 2026; most have not published in months. Activity is the first filter.
- **What a guest spot is worth is not measured.** No study ties guest appearances to signups or revenue. The case rests on reach to a precise audience, a long-form asset the user can clip and cite, and the links and mentions in show notes. Say so when the user asks for ROI.

## Judging one show's audience

- **Median views per recent episode is the audience on YouTube.** Take the latest ten episodes from `youtube_get_videos`, leave out any under 14 days old (still climbing), and take the median. One viral episode drags a mean far from what the next guest will get. Creator sizing rules in full live in [creators](../../creators/SKILL.md).
- **Subscribers are history.** A show with 200,000 subscribers and 1,500 views an episode is a 1,500-view show. In our test the ratio ran from 15% to 62% of subscribers across three small B2B shows ([field notes](field-notes.md)).
- **YouTube views understate the show.** Audio downloads on Apple Podcasts, Spotify and the rest are invisible to every public tool, and for interview shows audio is often the larger audience. A show that has run weekly for ten years with 1,000 views an episode on YouTube may have many times that in downloads. Treat the YouTube median as a floor, and ask the host for downloads per episode in the first 30 days when it matters.
- **Third-party scores are estimates.** Listen Notes' Listen Score (0 to 100, shown only for the top 10% of shows, updated about monthly) is a model built from Listen Notes' own traffic and public signals, not downloads. Apple and Spotify chart positions reflect recent momentum (new followers, recent listeners), not total audience. Other vendors' audience estimates publish no method.
- **Engagement shows fit.** Comments that ask the guest questions, and guests who are the user's peers, matter more than raw size.
- **Not the show:** a clips channel, a network channel, or a re-upload of someone else's interview (our search for "saas founder podcast" returned a channel re-uploading a famous founder's lecture). Check the channel name and `bio` before contacting.

## Finding shows

1. `youtube_search_videos` with `since: "year"`, one or two pages each (1 credit a page), for "<topic> podcast", "<audience> podcast", "<topic> interview", "<competitor> founder podcast" and "<competitor> CEO interview". Keep rows that read as episodes (podcast, episode, a number, "with <name>", "ft.") and run over 20 minutes (`duration_s` above 1,200).
2. Group by channel. Use `author_url` or `author_name`: in our test `author` came back null on 2 of 20 search rows, and `author_followers` was null on every search row, so size channels with `youtube_get_channel`, not from search.
3. `youtube_get_channel` (1 credit) for subscribers, `bio` and `links`; `youtube_get_videos` (1 credit) for the last episode date, episodes in the past 90 days and the median. Drop shows with no episode in 90 days.
4. **Check the fit from titles:** does the show interview guests (names in titles) or is it solo? Are the guests founders, operators or experts like the user? Has a competitor been on? A show that had a competitor or a peer on is the best fit there is. For the top few, `youtube_get_transcript` (1 credit) on a recent episode shows the host's questions and whether guests may mention their product.
5. **Size band.** Default to shows whose median episode gets about 500 to 20,000 views: these book guests readily and the host often replies in person. The largest shows book through introductions and agents, not cold pitches.
6. **Contacts.** The channel `bio` and `links` often name the host, a booking form or the show's site (`website` is the first non-social link). On the show's domain, run the contact steps in [link-building](../../link-building/SKILL.md); for a named host, `leads_get_email` with the name and domain.

About 390 credits for 25 shows looked at and 10 with a verified contact. Say the estimate first.

## The pitch

- Name an episode and what in it connects to the guest. Propose one topic with a number or a story behind it, and two or three questions the host could ask. Say why the guest is worth an hour: a result, a strong view, a story.
- Ask for nothing else: no links, no promotion in the pitch. Offer to share the episode with the user's own audience; hosts weigh that.
- One follow-up after a week. Hosts get many pitches and book months ahead.
- Afterwards: ask for a link in the show notes, clip the episode for the user's channels, and check for unlinked mentions (see [link-building](../../link-building/SKILL.md)).

## The deliverable

A table: show, host, channel URL, subscribers, episodes in the past 90 days, median views per episode, last episode date, fit (a past guest or episode that matches the user), contact, role, email, verification status, and where the contact came from. State that the list covers video podcasts on YouTube only and that audio-only shows are missing. Never send the pitch or book the slot.
