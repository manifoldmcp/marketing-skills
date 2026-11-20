# Content: length, frequency, format and music

What the large studies say about how long, how often and in what form to post, and the music rules that decide what a brand may post at all. Every figure here is an aggregate across many accounts; the user's own account, read as [reading the numbers](reading-the-numbers.md) says, beats any of them once it has 20 videos.

## Length and watch time

- **Most TikToks are short; longer ones get more views.** About 86% of videos run under a minute (Buffer, 1.1M videos, March 2025). Videos over 60 seconds had 43% more median reach than 30 to 60 second ones and 64% more watch time.
- **But people watch seconds, not minutes.** In the same data the median watch time was 3.1 seconds on 5 to 10 second videos, 6.9 on 30 to 60 second videos and 11.3 on videos of 1 to 10 minutes. A three-minute video is mostly a longer chance to hold the few who stay.
- **Brands see the same split.** Across 6M brand videos in January to June 2026 (Socialinsider): median views 1,274 under 15 seconds, 1,000 at 15 to 30, 2,200 at 30 to 60, 7,200 at 60 to 90, 9,620 at 90 to 120, 11,136 at 120 to 180, 10,150 above 180. Engagement rate peaked at 15 to 30 seconds (6.0%) and was lowest at 30 to 60 (4.2%). The method is not published, and accounts that post long videos may simply be bigger: read it as "long can work", not "length causes views".
- **Why creators post longer.** TikTok's Creator Rewards Program pays only on original videos over a minute long (TikTok newsroom, March 2024), and uploads can run much longer (up to 60 minutes for some accounts by third-party guides; TikTok has confirmed only a test, May 2024). Top niche results skew long as a result: 23 of the 30 most liked meal-prep videos of the month ran over 60 seconds, median 88 ([field notes](field-notes.md)).
- **What to do.** Make the video as long as the content holds attention, test one longer format against the account's usual length, and judge it on the average watch time and retention curve in TikTok Studio, which the tools cannot see. Do not pad to cross 60 seconds.

## Posting frequency

- **The study.** 11.4M posts from over 150,000 accounts connected to Buffer, compared each account with itself over time (Buffer, October 2025). Against one post a week, views per post rose 17% at two to five a week, 29% at six to ten, 34% at eleven or more.
- **The catch.** Median views per post barely moved (489 at one a week, 506 at two to five, 487 at six to ten, 459 at eleven or more). What rose was the top tenth: 3,722, 6,983, 10,092 and 14,401 views. Posting more raises the chance of a hit; it does not lift the typical video. The accounts skew to small and medium businesses.
- **Everyone is posting more.** Brand posting frequency rose in every size tier from 2024 to 2025 while small accounts' average views fell (Socialinsider, 2026). More volume is the field's response, not proof it pays.
- **What to do.** Two to five a week is the efficient range on the evidence. Set the number from the team's hours: a cadence held for 90 days beats a burst that stops in week three. Volume pays only if each extra video is a real attempt (a new hook, topic or format), since the gain comes from more draws.
- **When to post.** Aggregate best-time studies average over every audience. Use the hours the account's followers are active in TikTok Studio, and read a rival's posting hours from `created_at` converted from UTC to the audience's time zone.

## Format

- **Name the format from what the tools show:** talking head, voiceover over footage, list, tutorial, before and after, reply to a comment (captions starting "Replying to"), skit or POV, photo carousel (`media: "image"`, no `duration_s`), text on screen over music (a transcript that comes back `NoData`).
- **Photo carousels** are a separate surface with their own swipe behaviour. No large independent study compares carousel and video reach on TikTok in 2026; measure the share each account posts and the median views of each, apart.
- **Series** (part 1, part 2, episode numbers in the caption) turn one hit into a reason to follow. An account with several outliers in one series has a repeatable format; one with a single outlier has luck ([audit](audit.md)).
- **Comment-to-DM bait** ("comment PATTY and I'll send it") inflates comment counts with one-word replies. Read such videos' comment rate as a mechanic, not as interest, and keep them out of a comment mine ([comments](comments.md)).
- **AI-made video** is labelled and can be turned down by viewers (since November 2025). The top comments on an AI product video in our sample were about it being AI. A brand's product claims belong on real footage.

## Music and sounds

- **Two libraries.** Personal accounts can use TikTok's general music library, licensed for personal, non-commercial use. Business accounts get the Commercial Music Library: pre-cleared tracks for organic posts and ads, but licensed for TikTok only, so a video moved to Reels, Shorts or TV needs its own licence.
- **Brands get sued for songs in organic posts.** Warner Music sued Crumbl over at least 159 recordings and compositions in TikTok and Instagram posts (April 2025; agreement in principle to settle filed 22 May 2026, terms undisclosed) and PacSun (at least 290 works, December 2025), arguing the platforms' music licences cover users, not brands. Universal and Concord sued Quince (16 April 2026) over 67 recordings and 71 compositions, seeking up to $150,000 a work; some of its videos were tagged "Original Audio" (court filings, as reported by Music Business Worldwide and Bloomberg Law). An earlier court ruled Bang Energy liable for songs from TikTok's own library: infringement is strict liability, so believing TikTok cleared it is no defence.
- **What it means.** A trend that rides a popular song is closed to a brand account, and paying a creator to post it does not remove the brand's exposure. Flag every trend that depends on its audio. Original audio (a voice, a sound the brand made) and Commercial Music Library tracks are the safe options. This is general information, not legal advice; a high-profile campaign needs counsel.
- **Reading sounds in the data.** The `music` field on each row names the sound. "original sound - <handle>" with the video's own author is original audio; with another handle it is a borrowed sound (a reused voiceover or meme audio); a song title and artist is a track. A sound shared by several fast-rising videos from different authors is a sound trend; check in the app whether the brand's library has it.
