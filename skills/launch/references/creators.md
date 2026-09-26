# Launch creators

Creators who post in launch week put the launch where buyers already watch. The creator search and the brief belong to the influencers group; this playbook adds the launch reading: who covers new products, who can post by the date, and when each one needs the product and the brief.

## Inputs to settle first

- **Product and niche**: what launches, the niche words creators use, and whether there is a physical product to ship.
- **Launch date**: from the [launch plan](launch-plan.md), or ask. Creators need two to four weeks.
- **Platforms**: default the ones where the launch plan's baseline found the ICP; otherwise TikTok, Instagram and YouTube.
- **Deal**: product seeding, paid posts, or affiliate commission, and the money available. Default: seeding only.
- **Count**: default 15 small creators (about 10,000 to 100,000 followers) rather than one large one.
- **Market**: the country the buyers are in.
- **Budget**: [find creators](../../influencers/references/find-creators.md) states its own cost (about 160 credits at its defaults) and [campaign brief](../../influencers/references/campaign-brief.md) its own (about 12); the launch check here adds about 1 credit a creator, about 20 for 20: about 190 in all. Say the total before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the creators.** Run [find creators](../../influencers/references/find-creators.md) with the niche, the platforms and the market. Take its shortlist of about 20: handles, followers, median views, sponsored share, contact.
2. **Check launch fit.** For each creator, one page of recent posts on their best platform: `tiktok_get_videos`, `youtube_get_videos` or `instagram_get_reels` with the handle (1 credit each). Read three things: whether they cover new products or tools ("I tried", "new app", unboxings), whether they take paid posts (`is_ad`, or "ad" and "sponsored" in `text`), and how often they post. Someone who posts every few days can hit a date; someone who posts monthly may not.
3. **Check the audience where the launch depends on it.** For a paid creator or one the launch leans on, [vet creator](../../influencers/references/vet-creator.md), which checks where the audience is. Skip it for gifted posts to small creators.
4. **Set the dates.** Per creator: product in hand by T-21 (T-30 for YouTube, where a review takes longer to make), the brief by T-10, the post live between launch day and T+3, and a code or link per creator so the user can count what each one brought.
5. **Write the brief.** Run [campaign brief](../../influencers/references/campaign-brief.md) with the launch date, the embargo, the one message, the launch offer and the disclosure rule.
6. **Deliver** a table: creator, platform, followers, median views, a recent post about a product (URL), posting cadence, deal (gift, paid, affiliate), ship date, go-live date, tracking code, contact from the find creators table. The user or the host reaches out; nothing is sent from here.

## Judgment

- A gifted creator chooses whether and when to post. Only a paid or agreed post is a launch-week commitment; plan the gifted ones as a bonus.
- Median views, not followers, predict reach. Take them from the recent posts in step 2.
- A creator who has never shown a product will not start with the user's. Step 2 is the cheapest filter in this playbook.
- Paid posts need the platform's disclosure (its paid-partnership label or a clear "ad"). The brief says so; a hidden ad costs the creator and the brand.
- An embargo with creators is a request, not a contract. Do not send anything that cannot leak before launch day.
- The reaction report finds the creators' posts and the comments under them, not the sales. The tracking code per creator is the only way to count what each one brought.
