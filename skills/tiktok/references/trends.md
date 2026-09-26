# TikTok trends

What is rising in a niche right now: the formats, topics and lengths that several creators are using this week and that beat their own usual numbers. It ends in a short table of trends, each with example videos and a line on how the user could use it.

## Inputs to settle first

- **Niche**: three or four search terms: the category, a problem the product solves, a use case ("skincare routine", "acne", "glass skin"). Default: the category and two problems in the user's own words.
- **Window**: `since: "week"` by default. Use `since: "month"` for a slow niche or when the user asks about the month.
- **Language**: search has no country filter, so name the language to keep. Default: English.
- **Budget**: a default run costs about 4 x 2 + 10 + 10 = 28 credits for four terms. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pull what is new and popular.** `tiktok_search_videos` for each term with `since: "week"` and `sort: "popular"`, two pages each (1 credit a page). Drop rows with `is_ad: true`, duplicates across terms, and captions in other languages.
2. **Measure speed.** For each video, views per day since `created_at`. Rank on it: `sort: "popular"` ranks by likes, which favours the videos that had the most days to collect them.
3. **Separate the trend from the account.** For the authors of the 10 fastest videos, `tiktok_get_videos` with `sort: "latest"`, one page each (1 credit), and compare each video with its author's median views. A video at 3 times its author's median or more was lifted by what it did. A large account posting at its usual numbers is no evidence of a trend.
4. **Name the format.** `tiktok_get_transcript` on the 10 strongest outliers (1 credit each). With the caption, name each format: talking head, voiceover over footage, list, tutorial, before and after, reply to a comment, skit, photo carousel (`media: "image"`), or no speech (`NoData`: usually text on screen over music).
5. **Group.** Group the outliers by format, by topic (the words and hashtags that recur in `text`) and by length (`duration_s` under 15 seconds, 15 to 60, over 60). A group is a trend when it holds three or more videos from three or more authors in the window. The rows carry no sound data: say so, and point the user to the sound pages in the TikTok app or TikTok's Creative Center for sound trends.
6. **Deliver** a table: trend (a format, a topic or a length), what it is in one line, videos in the window, authors, median views, median views per day, median multiple of the author's own median, two example links with their first spoken or caption line, how the user could use it, and the date the data was pulled.

## Judgment

- One viral video is not a trend. Three authors doing the same thing and beating their own medians is.
- TikTok trends fade within one to three weeks. Date the table; a trend whose oldest example is two weeks old is probably at its peak.
- A niche with fewer than about 20 results for a term in a week is too thin for a weekly read. Run it again with `since: "month"` and say the window changed.
- The trend is the format or the topic, never the video. Advise the user to make their own version, not a copy.
- A business account on TikTok can use only the platform's commercial sound library, so a trend that rides a popular song may be closed to a brand. Flag the ones that depend on the audio.
- For the same read every week, the host schedules this playbook and keeps the previous table: see the [monitoring](../../monitoring/SKILL.md) group.
