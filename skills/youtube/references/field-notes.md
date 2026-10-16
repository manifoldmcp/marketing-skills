# Field notes

What Manifold has seen in its own YouTube calls. Small samples: read them as evidence of how the data and the engines behave, not as market figures. Newest first.

## The YouTube videos Google's AI answers cite (15 October 2026)

**Inputs.** Every YouTube citation in two back-to-back `aeo_run_ai_answers` runs (United States, English, all six engines) on five prompts: "best CRM for startups", "best email marketing tool for a small online store", "best project management software for agencies", "best running shoes for flat feet", "best tool to track brand visibility in AI search". Each video read with `youtube_get_video`, each channel with `youtube_get_channel`, each of the four prompts that drew a video searched once on YouTube with `since: "all"`, and three channels' latest uploads listed. 34 credits for the YouTube calls; the answer runs belong to the [ai-search](../../ai-search/SKILL.md) field notes.

**Only Google cited YouTube.** 21 citations of 14 distinct videos, 14 from AI Overview and 7 from AI Mode; none from ChatGPT, Claude, Gemini or Perplexity. The AI visibility prompt drew no video.

**The cited videos were small, recent comparisons.**

| | Cited videos (14) |
|---|---|
| Views | median 613; range 155 to 103,859; 12 under 3,100 |
| Channel subscribers | 1,050 to 98,300; median 19,800; none over 100,000 |
| Uploaded | 10 in 2026, 3 in 2025, 1 in August 2024 |
| Format | all "best X" lists or comparisons; 12 long-form of 7 to 16 minutes, 2 Shorts (83 and 106 seconds) |
| Title | all name the category and the buyer's question; 8 carry the year |
| Description | 10 carry affiliate or tracking links; 8 of the 12 long videos have chapters |
| Owner | mostly independent reviewers; 2 publishers (Runner's World, The Digital Project Manager); 2 vendors (NetHunt, MailerLite); 1 affiliate review site |
| Paid flag | 1 declared paid promotion (`is_ad` true) |

- **Google's pick is not YouTube's ranking.** Of the 14, 5 were in YouTube's top 20 for the same query: first on two queries (project management, flat feet), third, sixth and fourteenth on the others. The two most-viewed cited videos were YouTube's first and third results.
- **Citations point at a moment.** 11 of 21 citation links carried a timestamp. Some matched a chapter start in the description (481 s, 513 s, 750 s against a 751 s chapter), others did not (77 s, 96 s, 242 s), so Google appears to cite the passage it drew on rather than the chapter.
- **Fairly stable between runs.** 6 of the 8 videos cited in the first run were cited again a minute later.
- **A vendor's own video can be cited.** NetHunt's "Best CRM for Startups 2026" comparison, which includes NetHunt, had 441 views and was cited by AI Mode in both runs. It was about four times its channel's recent median of about 107 views, an outlier on a small vendor channel.
- **Mass production does not stop Google citing.** Pro Picks, cited for CRM, uploaded 30 templated "6 Best X 2026" affiliate videos in ten days (some already titled 2027). YouTube's monetisation and reach rules ([platform](platform.md#mass-produced-content)) are not Google's citation rules.
- **Views per subscriber are tiny on AI-cited videos.** George Vlasyev (42,400 subscribers, near-daily uploads) had a recent median around 250 views, under 1% of subscribers; his cited project management video had about ten times that.

**Data notes from the same calls**, folded into [channel data](channel-data.md#traps):

- `author_followers` was null on all 14 video rows and every search and listing row; only `youtube_get_channel` gives subscribers.
- `website` was filled for 8 of 13 channels, twice with a signup or newsletter page; `email` for 4 of 13.
- Search `created_at` shared one time of day across all rows and was up to three weeks off the exact upload date that `youtube_get_video` gave.
- `/shorts/` URLs worked in `youtube_get_video` and came back as watch URLs with no Shorts marker.
- `is_ad` was null on every search and listing row; only `youtube_get_video` filled it. Listing rows carried likes and comments; search rows did not.
- A listing page held 30 videos; for Pro Picks all 30 were under 14 days old, so its recent median could not be read from one page.
