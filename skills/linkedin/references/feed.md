# How the LinkedIn feed works in 2026

What decides who sees a post, what changed in 2025 and 2026, and which popular rules no longer hold. Checked October 2026. LinkedIn publishes the architecture but no weights, so most numbers here are third-party observations; the source and method sit beside each one.

## What LinkedIn has published

- **One retrieval system, matched on meaning.** LinkedIn's engineering post of 12 March 2026 ([Engineering the next generation of LinkedIn's Feed](https://www.linkedin.com/blog/engineering/feed/engineering-the-next-generation-of-linkedins-feed)) replaced separate retrieval sources (chronological network activity, trending by place and industry, collaborative filtering) with one: members and posts are embedded by an LLM in the same space, so a post about small modular reactors can reach someone whose history is electrical engineering without sharing a word. Retrieval mixes the member's network with posts from people they do not follow.
- **A ranker that reads history as a sequence.** A generative recommender reads a member's last 1,000+ interactions in order and predicts passive actions (click, skip, long dwell) and active ones (like, comment, share) through separate heads. Long dwell is a named target; no published threshold exists, so "30 seconds" or "61 seconds" rules are invention.
- **What it means in practice.** A post is shown to people whose past behaviour matches its topic, so an author who posts inside one area builds a consistent audience the model can find; a founder who alternates between AI, hiring, golf and the product makes each post compete for a different audience. Followers matter less than they did. Hashtags are not how topics are found.
- **"360Brew"** is the name of a large LinkedIn research model described in a January 2025 paper. Many explainers say it runs the feed; LinkedIn has not said so, and the March 2026 post describes a different ranking model. Say "the 2026 feed", not "360Brew", unless quoting.
- **Timing of the change.** Apptopia's panel shows time spent per user flat for 18 months, then up 38% from November 2025 (5.3 minutes) to May 2026 (7.3). It assumes a broad rollout around November 2025; LinkedIn has given no timeline.

## Reach: how far it fell

- **Richard van der Blom's Algorithm Insights 2026** (1.3M posts from 50K creators; summarised on the Creator Science podcast, June 2026): reach down about 60% over two years for active creators, while less active members (about 60% of members, his estimate) saw 10% to 15% more; engagement down 20% to 25%; about 9% of the feed is suggested posts; standalone articles get about 0.4 times normal reach. His 2025 edition is widely quoted as views −50%, engagement −25%, follower growth −59%; those figures come from secondary write-ups, not the report itself.
- **AuthoredUp** (LinkedIn tool vendor, own dataset, September 2025): average reach down 34% in 2025, down for 98% of users.
- **MagicPost** (tool vendor, 687K posts from 16K creators, June 2025 to September 2026, each post compared with the same author's average within 30 days): about 8% lower by mid-May 2026 and 2% more on 27 July, all posts. Smaller than the headline numbers because it removes the effect of who is posting.
- **Use it this way:** when a user's reach fell, first compare with these before blaming their content. A 30% to 50% drop over 2024 to 2026 is the platform, not the user. Judge their posts against their own recent median, never against 2023.

## Dated changes, 2025 to 2026

| Date | Change | Source |
|---|---|---|
| Aug 2025 | Help docs: comments from excessive creation or automation tools may have limited visibility | LinkedIn, via Social Media Today |
| Nov 2025 | "Make engagement pods entirely ineffective": detection widened, boosted content flagged and reach limited, mass-commenting plug-ins targeted | Gyanda Sachdeva, VP Product |
| Nov 2025 | Time per user starts rising; assumed broad feed rollout | Apptopia |
| Feb 2026 | Automated comments removed from "Most relevant", may reach only the commenter's own network; repeat offenders restricted | Gyanda Sachdeva |
| Mar 2026 | New feed architecture published; carousels' lead over photos falls from +21% to +9% | LinkedIn Engineering; MagicPost |
| May 2026 | Polls fall from +9% to −22% against photo posts; link cards' cost eases from −39% to −19% | MagicPost |
| Jun 2026 | Post analytics split reach into in-network and out-of-network | LinkedIn |
| 27 Jul 2026 | "Comment KEYWORD" posts fall from +10% to −47%; posts without substance fall further | MagicPost |
| Aug 2026 | "Seems like AI slop" report option; 1M+ uses in two weeks; slop views −40%. Comments ranked by relevance to each viewer | LinkedIn (Hari Srinivasan, CPO) |
| Sep 2026 | Detected inauthentic activity +46% in H1 2026 against H2 2025 (EU DSA report); Network tab test, a feed of connections only | LinkedIn |
| Oct 2026 | Slop classifiers extended to every post shown beyond the author's network, 94% precision | Tim Jurka, VP Engineering |

## Substance, bait and AI writing

- **Since 27 July 2026, substance is measured.** MagicPost had a language model rate 40,937 posts for a concrete detail (a number, a name, a specific case) and lived experience (something the author did or saw). Posts with neither fell from 13% to 23% below their author's average; posts with both rose from 10% to 15% above. Random control posts stayed near 0%.
- **Comment bait.** "Comment GUIDE and I'll send it" went from +10% to −47%; other requests for comments from −2% to −19%. A closing question to readers is neutral (0%). On the tools' side, these posts still show huge comment counts; see [reading the data](data.md#thresholds).
- **AI-sounding writing.** At equal substance, posts that read more like AI had about 9% fewer impressions even before July (MagicPost); Pangram Labs estimated 41% of LinkedIn posts show signs of AI generation and 30% of comments from April to June 2026 were wholly AI-written (57K posts). LinkedIn says using AI to polish language is fine; generic posts with no insight are the target.
- **For drafts:** a post the user can stand behind has one specific fact or story only they could write. Draft only when asked, and never from a creator's post.

## Links, hashtags, timing

- **Links**, same author, since mid-May 2026 (MagicPost): link card −19%, URL typed in the text −21%, "link in the comments" −16% (it was −8% before). Van der Blom puts one link in the body at −18.8% median reach. Pooled comparisons (different authors) give 36% to 60%; AuthoredUp found posts with four or more links did better, which is about who writes them, not the links. Advice: when the link is the point (a launch, a signup), use it and accept roughly a fifth less reach; when reach is the point, put the substance in the post and the link nowhere.
- **Hashtags:** one to three cost about 5% since July 2026 (range −14% to +5%, so within noise; MagicPost); AuthoredUp found no effect over eight months and harm above six. LinkedIn's VP of Marketing advises against opening with one, since the first line becomes the post's URL.
- **Timing:** the median post has 2% of its final views at 15 minutes, 8% at one hour, 23% at four hours, 44% at twelve hours, 72% at three days. Posting hour shifts the first hour by −21% to +11% and the final count by under 2%. Replying within the hour: +2% (range −11% to +16%). Commenting on others' posts before publishing: −4% (range −17% to +10%). LinkedIn still advises replying in the first hour; it does no harm, and the reply matters to the commenter.
- **Breaks:** a first post after two weeks or more away ran 19% above the author's average, with later weeks slightly lower (MagicPost; causation unknown). A missed week is not a penalty.
