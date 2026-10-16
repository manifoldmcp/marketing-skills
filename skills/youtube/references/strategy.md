# A YouTube plan

When the user wants a plan, not a check: which topics already pull views in the category, who owns them and how, and whether to build a channel, borrow other people's audiences, or both, over 90 days. Whether to be on YouTube at all is a channel choice for [growth-strategy](../../growth-strategy/SKILL.md).

## Inputs

Ask in one message; use the default and say so for anything unanswered.

- **Goal:** awareness, signups or leads, search traffic that keeps coming, presence in AI answers, or videos sales can send. Default: signups, measured by views on videos that answer buying questions.
- **Stage:** no channel, a few videos, or established. Ask for the handle; the baseline answers the rest.
- **Buyer:** who buys, so topics are what those people search, not what entertains everyone.
- **Capacity:** videos a month the team can make, who can be on camera, who edits, whether anyone can guest on podcasts. Default: two videos a month, the founder on camera, no editor.
- **Competitors:** two or three channels. Default: the three channels that appear most in the baseline searches.
- **Budget:** research credits (about 40 for the baseline and gaps) and any sponsorship budget.

## Baseline and gaps

- **Who owns the category** (about 15 credits): three category searches ("<category>", "<category> tutorial", "best <category>") with `since: "year"`; then, for the user and each competitor, the channel, the recent median and the popular list, judged by [channel health](channel-data.md#channel-health).
- **Topic gap:** the topics and formats that produced outliers for competitors and that the user has not covered.
- **Demand gap** (about 20 credits): `seo_search_keywords` on the category, keeping question, "how to", "tutorial", "review", "vs" and "alternative" phrases, then `seo_get_serp` on the top ten to mark those where Google shows videos. Google volume is a proxy; say so.
- **Freshness gap:** the same searches with `since: "all"`; a query whose top five are all over two years old is one a current video can take.
- **AI gap**, when the goal includes AI answers: whether Google's AI answers cite videos for the buyer questions, and which ([AI answers](ai-answers.md)). Usually the fastest win for a brand with no audience, because cited videos are small.

## Build, borrow, or both

- **Build a channel** when someone can publish at least two videos a month for a year and the buyer questions have search demand or AI citations. A new channel is found through search, suggested videos and AI answers long before the home feed; "how to", comparison and review videos keep being found for years, while trend topics favour channels that already have an audience.
- **Borrow audiences** when nobody can publish steadily: sponsor channels whose recent median fits ([creators](../../creators/SKILL.md)), or send the founder onto podcasts that publish on YouTube ([digital-pr](../../digital-pr/SKILL.md)). Get-featured asks on videos AI answers already cite are borrowing too.
- **No one on camera:** screen-recorded tutorials and comparisons still work, and none of the AI-cited videos in our runs needed a large audience. A faceless channel of templated videos at volume is what YouTube's rules now target ([platform](platform.md#mass-produced-content)).
- **Shorts:** plan them for reach and for search, not revenue: from February 2027 Shorts revenue needs 10 million views in 90 days ([platform](platform.md#partner-program-thresholds)), and the tools cannot measure a Shorts strategy.

## The 30-60-90 days

| Period | Typical work | KPI the tools re-measure |
|---|---|---|
| Days 1 to 30 | First videos on the strongest ideas ([video ideas](video-ideas.md)), each on a query with evidence; first get-featured or sponsorship asks; podcast pitches if chosen | Videos published; their views; whether they appear in `youtube_search_videos` for the target queries |
| Days 31 to 60 | A cadence the team can keep; formats that worked doubled; titles and thumbnails tested with Test & Compare on the videos with traffic | Median views of the latest 10 against the baseline; outliers |
| Days 61 to 90 | A second round of ideas from comment mining ([comments](comments.md)) and the gaps; re-measure AI citations on the same prompts | Subscribers; videos cited by Google's AI answers for the buyer prompts (`aeo_run_ai_answers`, or `seo_get_serp` with `ai_overview: true`) |

Give each period an owner, and three first actions for this week. Deliver one document: inputs with defaults marked, a baseline table (user against each competitor: subscribers, videos, uploads in the last 90 days, recent median, median over subscribers, best topics), the gaps in three lines each, the chosen routes and why, the 30-60-90 table, and the first actions.

## Setting targets honestly

- YouTube compounds slowly. The 30-day KPI is videos published and their views; subscribers belong to day 90.
- Set cadence from the team's hours. Two good videos a month for a year beat eight in a month and then silence.
- For B2B, views from the right people matter more than subscribers. Say so when the user benchmarks against an entertainment channel.
- Since 24 August 2026 public views count from the first frame ([platform](platform.md#what-a-view-is-now)). Compare the plan's KPIs against a baseline taken after that date, and read engaged views, CTR and average view duration in Studio at each review: the tools cannot see them.
- The server keeps no history. Store the baseline numbers with their date; a weekly or monthly re-check is the host's scheduler, as in [competitor-intel](../../competitor-intel/SKILL.md).
- Never upload, post, comment, subscribe, send a pitch or schedule anything. Write titles or scripts only when the user asks, each based on a pattern the evidence shows.
