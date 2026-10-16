# How X ranks posts

What the For You feed rewards, from the code X publishes and what X's leaders have said, set against outside measurements. Checked October 2026 against github.com/xai-org/x-algorithm (README, `home-mixer/params/param.rs`, `xai-value-model/scoring.rs`, `docs/BIDIRECTIONAL_BOOST_CHANGE.md`). X says it syncs the repository's defaults to production values by cron and shows any experiment on 10% or more of traffic; re-read the README's "Notable Updates" before quoting a weight.

## The pipeline

- **Release history.** Musk promised the full algorithm on 10 January 2026; the code went up around 20 January as a Rust and Python rebuild with a Grok-based transformer (Phoenix) ported from Grok-1. 15 May 2026 added a runnable end-to-end inference pipeline, the Grox content classifiers and ad blending (the repository's commits and README of that date); 13 August 2026 added the production weights, visibility filtering, labelling systems and SimClusters; 14 August a Brazil election filter; 18 September legal-withholding notices in the Under the Hood tool. The 2023 release (Scala, the "heavy ranker") is a different system.
- **Candidates.** In-network posts from accounts the viewer follows (Thunder), plus out-of-network posts from embedding retrieval (Phoenix), SimClusters, and a shared list of popular recent posts from the most-followed active accounts.
- **Filters before scoring** drop posts older than 48 hours, posts already seen or served, muted keywords and blocked or muted authors, and **replies and reposts from accounts the viewer does not follow**. New accounts' out-of-network posts must clear an engagement threshold.
- **Scoring.** Phoenix predicts, from the viewer's own recent engagement history, the probability of each action, and the score is a weighted sum. Then an **out-of-network discount** (a multiplier below 1, which also applies to replies and reposts from followed accounts), a **new-author boost** and a diversity reranker (a determinantal point process that trades a little score for less similar neighbours).
- **Visibility is separate from ranking.** After the order is set, visibility filtering can drop a post, put it behind an interstitial or attach a notice, from labels on the account and post (spam, inauthentic behaviour, adult media, blocks and reports relative to likes). Some rules drop a post only when it is recommended to non-followers: the same post still reaches followers. This is what people call a shadowban.
- **Under the Hood** (x.com/i/jf/under_the_hood, a pilot with expanding availability) shows an account the visibility-limiting labels on it and its posts. For "why did our reach collapse", check it before theorising.

## The weights

Production defaults in `param.rs`, August 2026:

| Action | Weight | Action | Weight |
|---|---|---|---|
| Share via copy link | 20 | Click the post | 0.3 |
| Reply | 5 | Open a link | 0.2 |
| Quote | 5 | Photo expand, dwell | 0.05 each |
| Share via DM | 5 | Video open | 0.02 |
| Follow the author | 4 | Not dwelled | −0.02 |
| Share (other) | 2 | Block author | −31.2 |
| Repost | 1 | Not interested | −52.3 |
| Like | 0.66 | Mute author | −58.8 |
| Click-dwell time | 0.4 (continuous) | Report | −234 |

How to read them, per X's own comments in the code:

- **They multiply predicted probabilities, not counts.** A report is rarer than a like by over a thousand times, so its weight is large just to register. "One report cancels 468 likes" is wrong, and so is "one reply equals 7.5 likes". What the weights do say: a post people want to answer, quote, or send to someone privately outranks a post people merely like.
- **Ranking is personal.** The probabilities come from each viewer's own history, so a coordinated group engaging on purpose mostly changes what people like them see. X adds that engagement only counts on posts served in the Home timeline: sending people to a post by link has no ranking effect.
- **Zero weights are decisions too:** profile clicks, video quality views and profile visit time carry 0 in the published defaults.

## Changes in 2026 that matter

- **Mutual-follow reply boost.** A/B test from 10 July 2026, launched broadly at 20 on 13 July, set to 15 on 24 July after complaints that World Cup talk from non-followed accounts was crowded out (X's `BIDIRECTIONAL_BOOST_CHANGE.md`). Posts from people the viewer follows back get their reply probability weighted extra. A tight, reciprocal network in the niche is now a ranking input, not only a social nicety.
- **New-author boost.** Low-impression authors' fresh posts are lifted toward a target slot. Published defaults: accounts under 50,000 followers, posts under 2 hours old, an impression threshold of 200, with Thompson sampling choosing which to lift. A new or small account gets a short trial for each post, not a free ride.
- **Author diversity** is described in the README but its multiplier is set to 1 (off) in the published scorer; the reranker handles similarity instead. So posting several times a day does not, by any published rule, push an account's later posts down.

## Links

- **What X says.** October 2025: Nikita Bier (head of product) said X was testing an in-app browser so link posts keep their engagement buttons, and that a post with a link "should stand alone as great content"; Musk said a link with almost no description gets weak distribution. 12 April 2026 (x.com/nikitabier/status/2043428612108644848): Bier called the link-penalty claim "fake news", said link clicks were higher than ever, and said links belong in the post body with context. 29 July 2026: Musk, asked whether X still penalises links, replied "We haven't for over a year."
- **What the code shows.** No link penalty term. Opening a link carries a small positive weight (0.2). Any effect comes from the model predicting fewer likes, replies and dwell on link posts, which the code cannot show.
- **What was measured.** Buffer (October 2025, 18.8M posts): median engagement on non-Premium link posts fell to 0% from March 2025 and stayed there; Premium link posts held about 0.28%, the lowest of any format. OpenTweet (October 2026, a scheduler selling link posting, 28,124 posts, 60 accounts with at least five of each kind, January to October 2026): each account's link posts got a median 1.05 times the impressions of its other posts, worse on 27 of 60; mostly small accounts. In our own reads, Vercel posted links in 16 of 17 posts and still drew a median 0.22 views per follower ([field notes](field-notes.md)). A figure of 94% fewer views on link posts, attributed to a January 2026 PPC Land analysis (as cited by OpenTweet), is widely quoted; we could not find the original or check its method.
- **The verdict to give.** Write the post to stand alone, with the link in the body. Test on the account: compare its link and no-link posts of similar topic over a month, by views per follower. Links in replies are an unproven workaround, not a rule.
- **API cost.** X's pay-per-use API charges $0.20 to create a post with a URL against $0.015 for a plain one (docs.x.com pricing, read October 2026; the page gives no date, and third-party trackers report the jump from $0.01 as 20 April 2026). A scheduler posting through the API pays this for a link reply too, which is why some tools strip links.

## Premium and the checkmark

- **Measured reach gap.** Buffer (October 2025, August 2024 to August 2025, 71K accounts, 27% paying): median impressions per post under 100 free, close to 600 Premium, over 1,550 Premium+; median engagement about 0.5% on every paid tier against 0% free. Buffer says follower size alone does not explain the gap, without describing a control. Paying accounts are also the more active ones; read it as an association.
- **What the code shows.** No Premium or checkmark term in the published For You weights or scorer; verification appears in spam-labelling rules. X markets Premium on reply prioritisation, larger on each higher tier (help.x.com, About X Premium). A plausible reading, unproven: Premium helps most where X ranks replies, and indirectly by keeping an account clear of low-trust labels.
- **Advice.** For a business or founder whose buyers are on X, Premium is cheap relative to the measured gap; take it and judge by views per follower over a month. Do not expect it to rescue posts nobody replies to.

## Formats

- **Text holds up.** Buffer's 2026 format study ranks X's median engagement text 3.56%, image 3.40%, video 2.96%, link 2.25%; its Premium study found text strongest for Premium accounts. X is the one major network where text is not behind video.
- **Video** has dedicated weights (video open, continuation) and a minimum length of 10 seconds for video quality views; their weights are 0 or near it in For You. No study in 2026 shows video beating text on X.
- **Threads** are not scored as a unit: each post is a candidate, and branches of one conversation are collapsed so only one shows. A thread's opener carries it. X Articles (native long-form) appear as a post with an x.com/i/article link.
- **Timing.** Buffer (2026) found higher-performing posts cluster on weekday mornings; with a 48-hour window and personal ranking, timing matters less than whether the post draws replies.
