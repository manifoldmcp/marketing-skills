# How YouTube distributes videos now

What YouTube itself has said about recommendations, measurement, Shorts and monetisation, and what changed between mid-2025 and October 2026. Checked October 2026. YouTube changes these rules several times a year, so re-check anything here older than six months against YouTube's Help Center or blog before quoting it to a client.

## Recommendations are pulled per viewer

- **There is no single feed a video is pushed into.** Todd Beaupré, YouTube's senior director of growth and discovery, described recommendations as assembled for each viewer on each visit from their history, context, time of day and device; notifications to opted-in viewers are the only push (Creator Insider interview with Rene Ritchie, 1 September 2026). No ranking rule changed with it; it is YouTube describing the system.
- **Subscribers skip most uploads.** In the subscribers traffic source he put click-through at "probably" 10% or less, for good videos too. Subscribers rank a channel's video higher, but a new upload is also tried on non-subscribers within its first hour, and YouTube learns from each audience separately rather than averaging them. This is why subscriber count predicts a new video's views badly, and why the median rule in [channel data](channel-data.md#channel-health) judges a channel by recent views.
- **Day-one numbers are not the verdict.** He said early analytics are not the sole basis for decisions, some videos peak late, and evergreen videos stay eligible. Do not call a video a failure from its first 48 hours, and do not delete and re-upload to "reset" it.
- **No single metric decides.** Click-through shows how appealing the title, thumbnail and idea were; watch time can reward idle viewing; surveys ask viewers whether the time was well spent. The weighting varies by person and surface: time spent matters more on TV, likes and dislikes carry more signal on phones. YouTube has never published numeric weights; "signal weight" tables online are invented.
- **Interview caveats.** A YouTube employee interviewing a YouTube director on a YouTube channel, with no data shown for most claims. Treat it as YouTube's stated intent, which is still more than anyone outside can measure.

## Click-through rate in context

- **Half of all channels and videos have an impressions CTR between 2% and 10%**, per [YouTube's help page](https://support.google.com/youtube/answer/7628154); new videos and those under 100 views range wider.
- **CTR falls as impressions grow.** YouTube says a video shown widely (the Home page) naturally has a lower CTR, and a small loyal audience a higher one. A falling CTR while impressions climb is usually a video reaching colder viewers, not a worse thumbnail. Compare CTR only within one traffic source and a similar impression count.
- Impressions on external sites and end screens are not counted.
- None of this is in Manifold's data: CTR, impressions, average view duration and retention live in the channel owner's YouTube Studio. Ask the user for them; never estimate them from views.

## Test & Compare picks by watch time

- YouTube Studio's [Test & Compare](https://support.google.com/youtube/answer/16391400) runs up to three titles, thumbnails or title-and-thumbnail pairs on one video, desktop only, with advanced features turned on. **The winner is the variant with the most watch time**, and YouTube says it optimises for watch time over click-through rate. A thumbnail that wins clicks and loses viewers in the first minute loses the test.
- Results come within about two weeks, labelled Winner (statistically significant), Performed same, or Inconclusive; when nothing wins, the first upload stays. A small control group sees only the default. Results can differ between runs.
- Not available on Shorts (a video that becomes a Short loses it), scheduled Lives (Live archives can), made-for-kids, mature-audience or private videos.
- **Inconclusive is normal on small channels**: too few impressions, or variants too alike. Test different ideas (a different promise, a different face, a different frame), not colour tweaks. YouTube publishes no win rates, and the third-party figures that circulate (false-positive rates under 1,000 impressions and the like) have no primary data behind them.

## What a view is now

- **Shorts, from 31 March 2025:** a view counts each time a Short starts or replays, with no minimum watch time. The old measure was renamed engaged views.
- **Every format, from 24 August 2026:** a public view counts from the first frame, including a thumbnail that autoplays on hover ([YouTube blog](https://blog.youtube/inside-youtube/engaged-views-youtube-explained/), 19 August 2026). An engaged view is a click, or playback past the first seconds. CTR, average view duration, retention, recommendations and Partner Program eligibility all still run on engaged views.
- **What this does to public numbers** (the only views Manifold can read): counts are higher than they were, more so for videos shown on autoplay surfaces, and a video's lifetime count mixes both definitions. A 2025 video and a September 2026 video are no longer measured the same way. Compare videos published on the same side of the date, prefer a channel's own median over cross-channel comparisons, and for the user's own videos use engaged views in Studio.

## Shorts

- Up to three minutes long since 15 October 2024, if vertical or square; a horizontal three-minute video is a regular video. Guides still saying 60 seconds are out of date.
- Over 200 billion daily views (Neal Mohan, Cannes Lions, June 2025), counted the start-of-play way.
- Creators keep 45% of the Shorts revenue allocated to them, against 55% of ad revenue on long-form. Shorts watch hours in the Shorts feed do not count toward the 4,000-hour Partner Program threshold.
- **Originality now affects reach.** YouTube posted on 1 October 2026 that Shorts recommendations favour original content; secondary coverage says re-uploads with nothing added get less distribution and that minor edits and template changes do not count as adding something.
- Manifold's data cannot tell a Short from a short video: see [channel data](channel-data.md#traps).

## Mass-produced content

- **15 July 2025:** the Partner Program's "repetitious content" rule was renamed "inauthentic content", aimed at mass-produced and repetitive videos: templated with little variation, easy to produce at scale, little author input. YouTube called it a minor update; the separate reused-content rule (clips, compilations, reactions) did not change, and using AI tools is allowed.
- **July 2026:** YouTube's guidance now names three groups that cannot earn: generic or repetitive content (templated slideshows, the same set-up with the same outcome), unsatisfying or off-putting content (manipulative or shock formulas, AI clips stitched with no arc), and AI personas presented as human experts on health, legal, financial or political topics (Tubefilter, 13 July 2026; a clarification, not a new rule).
- **The reach side:** Mohan's January 2026 letter made reducing low-quality AI content a 2026 priority, through the systems that already reduce the spread of spam, clickbait and repetitive content. Realistic altered or synthetic content must be disclosed.
- **For a brand**, YouTube ad revenue rarely matters; reach does. A faceless channel turning out templated videos at volume is the format both the monetisation rule and the reach systems target. Using AI to script, dub or edit a video with a real point of view is not.
- Google Search is a different system. In our runs Google's AI Overview cited a video from a channel posting about three templated "6 Best X" affiliate videos a day ([field notes](field-notes.md)).

## Partner Program thresholds

- Now: 1,000 subscribers plus either 4,000 public watch hours in 12 months or 10 million Shorts views in 90 days ([YouTube Help](https://support.google.com/youtube/answer/72851)).
- **From 1 February 2027**, for new applicants: 8,000 watch hours in 365 days or 20 million Shorts views in 90 days. For every partner, existing ones included, Shorts revenue sharing needs 10 million qualified Shorts views in the last 90 days; below that a channel keeps long-form earnings ([YouTube blog](https://blog.youtube/news-and-events/youtube-partner-program-updates-2027-new-opportunities-earn/), 10 August 2026). Partners must accept the new terms in Studio by 31 January 2027.
- A brand channel that wants ad revenue from Shorts alone now needs scale few brands reach. Plan Shorts for reach and search, not revenue.

## Viewers get more control

Made on YouTube (23 September 2026) announced custom Home feeds viewers build by mood or interest, and Ask YouTube, recommendations from a question. No data yet. The direction rewards a title that names the question or job the video answers, which is also what search and AI answers reward.
