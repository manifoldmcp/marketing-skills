# Find YouTube creators

A shortlist of YouTube channels in the user's niche that are active and watched: found from what ranks for the niche's searches, sized by the median views their recent videos get rather than by subscribers, with sponsorship evidence and a contact where the channel gives one.

## Inputs to settle first

- **Niche**: the topics the right creators cover, as four to six searches ("<category> tutorial", "<competitor> review", "best <category>", the audience's jobs such as "how to run payroll").
- **Purpose**: a sponsored integration, a dedicated review, an affiliate deal or a long-term partnership. It decides whether sponsorship evidence matters.
- **Size band**: default channels whose median recent video gets 2,000 to 100,000 views. Below that a sponsorship reaches too few people; above it prices usually outgrow a first test.
- **Market and language**: the language the videos should be in. YouTube publishes no audience location here.
- **Count**: default a shortlist of 15.
- **Budget**: a default run costs about 6 x 2 + 30 + 30 + 15 x 2 = 102 credits. Contacts through a creator's own website add about 8 to 10 each. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Search the niche.** `youtube_search_videos` with `since: "year"`, two pages each (1 credit a page), for the four to six searches. Group the rows by channel (`author`). Keep channels with at least two relevant videos in the results: a channel that covers the niche, not one that hit it once. Stop at about 30.
2. **Size them.** `youtube_get_channel` on each (1 credit): subscribers, video count, `bio`, and `location` where the channel gives it.
3. **Filter by views and recency.** `youtube_get_videos` with the default sort (1 credit each): the median views of the latest 10, the median as a share of subscribers, uploads in the last 90 days and the date of the last upload. Apply the [channel health](../SKILL.md#channel-health) rules and the size band. For a sponsorship, also drop channels without an upload in the last 30 days: they may not publish the user's slot on time.
4. **Check engagement and sponsors.** For the 15 best, `youtube_get_video` on two recent videos (1 credit each): likes and comments per 1,000 views, and `is_ad`, which is true on a video that declares a paid promotion. A channel with declared promotions already takes sponsors; titles that review competitors show the channel covers the category and may have a deal with one.
5. **Find the contact.** Read the `bio`: most creators who take sponsors list a business address or a site. For a creator with their own site and no address in the bio, run the [contact steps](../../link-building/SKILL.md#contact-steps) on that domain with `limit: 10`.
6. **Deliver** a table: channel, URL, subscribers, median views of the latest 10, median as a share of subscribers, uploads in the last 90 days, last upload, likes and comments per 1,000 views, the one or two most relevant videos (URL, views), sponsors or competitors seen, contact and its source, and one line on fit.

## Judgment

- Price and pick by median views, never by subscribers. A 400,000-subscriber channel with a 6,000-view median is a 6,000-view channel.
- Relevance beats size. A channel of 8,000 subscribers whose every video is about the user's category beats a general tech channel ten times its size.
- YouTube publishes no audience location or age here; only TikTok has an audience split. Use the language of the videos and the `location` of the channel as weak signals, and ask the creator for their analytics before paying.
- Search surfaces what YouTube ranks, which favours channels already winning. Small, excellent channels can be missing; a second pass with narrower searches finds more.
- A channel that reviewed a competitor is a good fit and a risk: it may have an exclusive deal. Ask before assuming.
- To vet one creator in depth, or to run creators across TikTok, Instagram and YouTube together, use the [influencers](../../influencers/SKILL.md) group: [find creators](../../influencers/references/find-creators.md), [vet a creator](../../influencers/references/vet-creator.md) and [campaign brief](../../influencers/references/campaign-brief.md).
