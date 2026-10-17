# Reading YouTube data in Manifold

What the `youtube_*` tools return, what they do not, the traps in their fields, and the rules for judging a channel from public numbers. Every other reference here relies on this one. Field behaviour checked against live calls on 15 October 2026 ([field notes](field-notes.md)).

## Which tool answers what

| Question | Tool | Cost |
|---|---|---|
| How big is this channel, what does it link to? | `youtube_get_channel` | 1 credit, cached 24 hours |
| What does it publish, how often, what are its hits? | `youtube_get_videos` (newest first, or `sort: "popular"`) | 1 credit a page of about 30, cached 6 hours |
| One video's likes, comments, length, links, paid flag | `youtube_get_video` | 1 credit, cached 6 hours |
| What covers a topic on YouTube now? | `youtube_search_videos` (`since` for recency, `sort: "popular"` for views) | 1 credit a page of about 20, cached 6 hours |
| What does the video actually say? | `youtube_get_transcript` | 1 credit, cached 30 days |
| What do viewers say? | `youtube_get_comments` (most liked first) | 1 credit a page |
| Is there Google demand for the phrase, and does Google show videos for it? | `seo_search_keywords`, then `seo_get_serp` | 10 credits for 100 rows (cached 7 days); 1 credit a SERP, +1 with `ai_overview: true` |

Every YouTube call costs 1 credit, so YouTube research is cheap; the expensive calls in this topic are elsewhere (`aeo_run_ai_answers`, 27 credits a prompt on the default engines). Transcripts are cheap in credits but long: read them for a shortlist, to spare the context. A result the account already paid for is free while cached, so a second pass the same day costs little. `dry_run: true` prices any call for free; pass `max_credits` when the user set a budget.

## Traps

- **Subscribers come only from `youtube_get_channel`.** `author_followers` is null on every search, listing and video row we tested. Its `followers` is subscribers, `posts_count` is the video count, `views` is lifetime channel views, `created_at` is when it joined.
- **`website` is now filled** for most channels that list a site (8 of 13 tested), but it is the first non-social link, which can be a signup or newsletter page rather than the homepage. `email` appears only where the channel publishes one (4 of 13); otherwise the contact is in the `bio`. Finding a creator's or publisher's contact properly is in [link-building](../../link-building/SKILL.md).
- **Search dates are rounded.** `created_at` on search rows and comments is back-computed from YouTube's age label ("3 weeks ago"): every row in a search shares one time of day, and we saw it three weeks off the real upload date. Fine for "uploaded this year"; for anything finer, take `created_at` from `youtube_get_video` or the channel listing, which are exact.
- **Views are lifetime and, since 24 August 2026, counted from the first frame** ([platform](platform.md#what-a-view-is-now)). Compare old and new videos by views per month since upload, and be wary of comparing across that date.
- **Likes, comments and the paid flag:** listing rows often carry likes and comments, search rows do not, and `is_ad` came back null on every search and listing row. For engagement and for whether a video declares a paid promotion, use `youtube_get_video`.
- **Shorts are not marked.** A Short fetched by its `/shorts/` URL comes back as a watch URL with the same media type as any video, and search can return rows under three minutes that may or may not be Shorts. Treat a vertical-looking row under three minutes as a probable Short and say it is a guess. The tools have no Shorts feed, so they cannot judge a Shorts strategy.
- **The video row's `transcript` field is empty.** Call `youtube_get_transcript`; `NoData` there means no captions, so take the hook from the title and mark it.
- **Search is YouTube's own ranked search for one logged-out viewer**: useful, never complete. A topic missing from it is not proof nobody made it. YouTube has no newest-first search; `since` does the narrowing.
- **The `author` handle** is what `youtube_get_channel` and `youtube_get_videos` take. When it is null (it happens on search rows), skip the channel or find it by name.
- **No YouTube search volume exists in any tool.** Google volume from `seo_search_keywords` is a proxy for YouTube demand; say so beside every number taken from it.

## Channel health

The rules that judge a channel from public numbers. They apply to auditing a competitor, vetting a channel to sponsor (sizing and vetting creators belongs to [creators](../../creators/SKILL.md)) and reading the user's own.

- **Median, not mean.** Judge a channel by the median views of its latest 10 videos, leaving out videos under 14 days old, whose views are still climbing. One hit drags a mean far from what a new video will get. A channel that posts daily fills a whole listing page in under 14 days: page on, or use the popular sort for its hits and say the recent median is not settled.
- **Real audience.** A median under about 5% of subscribers (a rule of thumb, not a study) means most subscribers no longer watch: size the channel by its median, not its subscriber count. YouTube's own description of subscribers skipping most uploads ([platform](platform.md#recommendations-are-pulled-per-viewer)) is why.
- **Active.** No upload in 90 days means the channel is paused: skip it for sponsorships and guesting, keep it as evidence for topics. YouTube may also remove monetisation from a channel inactive for six months.
- **Outlier.** A video with at least three times its channel's median views is an outlier: the topic or packaging pulled viewers beyond the channel's own audience. Outliers are the best public evidence of demand YouTube offers, and an outlier from a small channel is stronger proof than one from a big channel.
- **Engagement.** Likes and comments per 1,000 views, from `youtube_get_video`. High views with under about 5 likes per 1,000 often means the views were bought as ads, whose viewers rarely like or comment; mark it likely, not proven. `is_ad: true` marks a declared paid promotion: keep those out of medians.
- **Leave out what the channel did not make for YouTube.** Webinar recordings, event talks and live archives sit far below a brand's median; judge the videos made for YouTube without them.
- **Repeatable, not lucky.** Six outliers in one format mean a format that works; one means luck. Formats a channel keeps uploading usually work for it; one tried once and dropped probably did not.

## What the tools cannot see

Click-through rate, impressions, average view duration, retention curves, traffic sources, which title or thumbnail a channel is testing, the thumbnail itself, revenue, and subscriber growth over time. For the user's own channel, YouTube Studio has all but the competitor view; ask the user to read them at each review. For competitors they are unknowable: say so rather than infer them from views.
