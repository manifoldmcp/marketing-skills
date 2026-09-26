# Reels trends

What is rising on Reels in a niche right now: the formats, topics and lengths that several accounts are using this week and that beat their own usual numbers. It ends in a short table of trends, each with example reels and a line on how the user could use it.

## Inputs to settle first

- **Hashtags**: three or four tags the niche uses ("homedecor", "smallapartment", "rentalfriendly"). Default: the category tag, then the tags that recur in the captions of its first results.
- **Window**: `since: "week"` by default. Use `since: "month"` for a slow niche or when the user asks about the month.
- **Language**: hashtag search has no country filter, so name the language to keep. Default: English.
- **Budget**: a default run costs about 4 x 2 + 10 + 10 = 28 credits for four hashtags. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Pull the recent reels.** `instagram_search_posts` for each hashtag with `since: "week"`, two pages each (1 credit a page). Keep the reels (`media: "video"`). Drop rows with `is_ad: true`, duplicates across hashtags, and captions in other languages. The rows come in Instagram's own order; there is no sort to ask for.
2. **Measure speed.** For each reel, views per day since `created_at`, and rank on it.
3. **Separate the trend from the account.** For the authors of the 10 fastest reels, `instagram_get_reels`, one page each (1 credit), and compare each reel with its author's median reel views. A reel at 3 times its author's median or more was lifted by what it did. A large account posting at its usual numbers is no evidence of a trend.
4. **Name the format.** `instagram_get_transcript` on the 10 strongest outliers (1 credit each). With the caption, name each format: talking head, voiceover over footage, tutorial, list, before and after, day in the life, POV or skit, or no speech (`NoData`: text on screen over music). A reel over two minutes comes back `InvalidTarget`: name its format from the caption.
5. **Group.** Group the outliers by format, by topic (the words and other hashtags that recur in `text`) and by length (`duration_s` under 15 seconds, 15 to 60, over 60). A group is a trend when it holds three or more reels from three or more accounts in the window. The rows carry no audio data: say so, and point the user to the trending audio shown in the app's Reels audio picker.
6. **Deliver** a table: trend (a format, a topic or a length), what it is in one line, reels in the window, accounts, median views, median views per day, median multiple of the account's own median, two example links with their first spoken line or caption line, how the user could use it, and the date the data was pulled.

## Judgment

- One viral reel is not a trend. Three accounts doing the same thing and beating their own medians is.
- Reels trends fade within one to three weeks. Date the table; a trend whose oldest example is two weeks old is probably at its peak.
- Hashtag search sees only posts that carry the tag, and many top reels carry none. The sample leans towards accounts that tag their posts; say so.
- A hashtag with fewer than about 20 reels in a week is too thin for a weekly read. Run it again with `since: "month"` and say the window changed.
- The trend is the format or the topic, never the reel. Advise the user to make their own version, not a copy.
- Business accounts on Instagram get a narrower music library than personal ones, so a trend that rides a popular song may be closed to a brand. Flag the ones that depend on the audio.
- For the same read every week, the host schedules this playbook and keeps the previous table: see the [monitoring](../../monitoring/SKILL.md) group.
