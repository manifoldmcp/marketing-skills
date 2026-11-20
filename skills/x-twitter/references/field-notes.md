# Field notes

What Manifold has seen in its own reads of X through `twitter_get_profile` and `twitter_get_tweets`. Small samples: read them as evidence of how the tools and accounts behave, not as market benchmarks. Newest first.

## Eleven feeds, one page each (15 October 2026)

`twitter_get_tweets` on 11 handles (stripe, vercel, coinbase, TechCrunch, posthog, levelsio, VitalikButerin, paulg, supabase, nytimes, sama) and `twitter_get_profile` on two (paulg, stripe): 13 credits. Provider scrapecreators.

**Two kinds of page come back.** The tool's `feed` now names them, `recent` or `most_liked`, from the order of the dates.

| Page | Accounts | Rows | Order | Span |
|---|---|---|---|---|
| Recent | stripe, vercel, coinbase, TechCrunch, posthog | 5 to 20 | newest first, pinned post first | 13 hours (TechCrunch) to 7 weeks (Vercel) |
| Greatest hits | levelsio, VitalikButerin, paulg, supabase, nytimes, sama | 99 to 100 | by likes, descending | years; newest post November 2025 for five of six |

- Every personal account tried got the greatest-hits page; so did two organisations. Follower count does not predict it (Coinbase at 7.2M and TechCrunch at 11M got recent pages). The likely cause is that the vendor sees what X shows a signed-out visitor, a profile's popular posts, from a snapshot; unconfirmed.
- On a greatest-hits page, cadence, current medians and "how are we doing" cannot be measured. It is still a fair list of an account's best-ever posts.

**Field contents.**

- `media` is filled: text, image or video, matching the posts checked. A post with a link card reads text. (Earlier notes said every row read text; no longer true.)
- `author_verified` and profile `verified` read false for Stripe and Vercel, organisations with X's gold check, and true for Coinbase, TechCrunch and PostHog. Profile `kind` read "person" for Stripe. Neither field says anything about standing.
- Repost rows ("RT @") show 0 likes, a `shares` count that looks like the original's, and a `views` number that is neither the account's nor clearly the original's (PostHog's reposts about 100, Coinbase's about 40,000). Drop them.
- An X Article appears as a post whose text is only an x.com/i/article link.
- `top_comments` was empty and `is_ad` null on every one of 666 rows. Profile `recent_posts` was empty on both profiles.
- `views` was null on posts from before late 2022 (72 of Vitalik Buterin's 100 top posts), when X began showing view counts.
- paulg's profile returned a null `bio`.

**What the recent pages showed** (own posts only, reposts dropped, posts under 48 hours dropped except TechCrunch, whose whole page was under 13 hours old):

| Account | Followers | Posts used | Median views | Views per follower | Median engagement per view | Link posts |
|---|---|---|---|---|---|---|
| Vercel | 463K | 17 | 101,732 | 0.22 | 0.38% | 16 of 17 |
| Stripe | 293K | 4 | 31,809 | 0.11 | 1.19% | 2 of 4 |
| Coinbase | 7.2M | 3 | 124,143 | 0.017 | 0.63% | 0 of 3 |
| TechCrunch | 11.1M | 20 | 15,625 | 0.0014 | 0.16% | 20 of 20 |

- Vercel's feed is almost all link posts, each with a stand-alone list of facts, and still reached a median of a fifth of its followers, with two posts over 700K views. Consistent with X's claim that a link with real context is not buried; one account proves nothing about links in general.
- TechCrunch posts a headline and a link every 20 to 60 minutes: 11M followers, about 15K views a post, the lowest views per follower and engagement per view in the set. Volume without conversation.
- Vercel's best post drew 9 times its median; Stripe's and Coinbase's pages were too short to show outliers. Pages of three or four usable posts cannot carry an audit; ask for post URLs.
