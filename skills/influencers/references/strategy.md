# Influencer strategy

An influencer strategy settles where the niche's creators are, what competitors already get from creators, and which kind of deal fits the goal and the budget: gifting, paid posts, affiliate codes or content for ads. It ends in a 90-day plan the group's playbooks can execute, not a creator list.

## Inputs to settle first

Ask for these in one message. For anything the user does not answer, use the default and say that you assumed it.

- **Goal**: awareness, sales through codes and links, content to run as ads, or buzz for a moment. Default: sales through codes, because the user can measure them.
- **Stage**: pre-launch, early or established, and whether the user has worked with creators before. Default: early, no creators yet.
- **ICP**: who buys and what they watch. It picks the platforms.
- **Budget**: cash for creator fees and product for gifting, and credits for the research (this playbook costs about 35). Default: $3,000 and 30 units of product for the quarter, 500 credits.
- **Team**: who manages creators, who approves drafts, and whether product can be shipped. Default: the founder, a few hours a week.
- **Competitors**: two or three. Default: the brands that show up in the niche's sponsored posts in step 2.
- **Horizon**: default 90 days.

## Steps

1. **Intake.** Settle the inputs above and restate them in one short list, marking the defaults.
2. **Baseline.** Say the cost first: about 28 credits.
   - Where the niche lives: `tiktok_search_videos` and `youtube_search_videos` with the niche keyword (`since: "year"` on YouTube), and `instagram_search_posts` with the niche hashtag, two pages each (6 credits). Per platform, note how many distinct creators post and the median views of their posts.
   - What competitors get from creators: the same three searches with each competitor's name, one page each (9 credits for three). Keep the rows with `is_ad: true` or a sponsored caption: those are the creators competitors pay.
   - The user's own footprint: the same three searches with the brand name (3 credits).
   - Size: `tiktok_get_profile`, `instagram_get_profile` or `youtube_get_channel` on the ten authors who appear most (10 credits), for the size bands the niche runs on.
3. **Gaps.** About 5 credits more. `tiktok_get_videos`, `instagram_get_posts` or `youtube_get_videos` on five creators competitors paid (1 credit each).
   - Re-hires: a competitor paying the same creator twice is the best public sign a deal paid off. Name those creators and the size band they sit in.
   - Deal types: discount codes in captions mean affiliate deals, "gifted" means seeding, `is_ad` without a code means a flat fee.
   - Platforms: the platform where the niche has many creators and competitors have few is the opening; the one where every competitor is present is table stakes.
4. **Tactics.** Choose from this group, and say why each fits:
   - [Find creators](find-creators.md), always: the shortlist across platforms, sized to the budget.
   - [Vet a creator](vet-creator.md) before any flat fee above a few hundred dollars. Skip it for product-only gifting to nano creators, where the check would cost more than the risk.
   - [UGC creators](ugc-creators.md) when the goal is content to run as ads or to use on the product page, not reach.
   - [Campaign brief](campaign-brief.md) before the first creator posts, always.
   - For creators timed to a launch day, the `launch` group's creators playbook.
5. **Plan.** A 30-60-90 day plan with an owner and a KPI per period, and three first actions for this week.
   - The shape: by day 30, a shortlist of 30, the top 10 vetted, product sent to 15 to 20 nano and micro creators, and the brief written. By day 60, three to five paid posts from vetted micro creators, each with its own code. By day 90, re-hire the two creators whose posts beat their own median, turn the best posts into ads with the `paid-ads` group, and drop the rest.
   - KPIs the tools can measure again later: views, likes and comments on each campaign post (`tiktok_get_video`, `instagram_get_post`, `youtube_get_video`), each post's views against the creator's own median, questions about the product in its comments (`tiktok_get_comments`, `instagram_get_comments`, `youtube_get_comments`), and the count of posts naming the brand in the platform searches from step 2. Code redemptions and sales live in the user's store; ask for them at each checkpoint.
   - **Deliver** one document: the inputs with defaults marked, a baseline table (platform against creators in the niche, median views, competitors present, the user's footprint), the creators competitors pay and re-hire, the gaps in three lines each, the chosen tactics with the linked playbook and why, the 30-60-90 table, and three first actions for this week.

## Judgment

- Twenty gifted nano creators teach more in the first month than one paid macro creator, for the same money. Pay only after the gifting shows which creators and angles move people.
- A code per creator is the only clean attribution. Without codes or tagged links, the user cannot tell which creator sold.
- Creators whose sponsored posts do as well as their own posts are the ones to re-hire; the audience trusts them. Sponsored posts at half the creator's median are a warning, whatever the follower count.
- Competitors' creators are proven for the niche, but a creator mid-contract with a competitor may be bound by exclusivity. Flag them; do not exclude them.
- The tools see public posts and numbers. They do not see fees, contracts, sales or a post's paid boost. Keep the KPIs to what can be measured again.
