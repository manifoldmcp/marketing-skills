# Find creators across platforms

Most creators worth sponsoring post on more than one platform, and a list built per platform counts them two or three times with numbers that cannot be compared. This playbook runs each platform's own creator search, merges the results into one row per person, and puts their engagement on one scale. It ends in one table.

## Inputs to settle first

- **Niche**: three to five keywords and hashtags the niche uses ("home espresso", "latte art", "espressotips"). Ask for two.
- **Platforms**: default TikTok, Instagram and YouTube. Drop a platform the user's buyers do not use.
- **Size**: default 10K to 250K followers, per the [floors](../SKILL.md#floors). Pass the same band to each platform playbook; YouTube's is set in median views.
- **Market**: the country and language the user sells in.
- **Count**: default 20 people in the final table.
- **Budget**: the three platform playbooks cost about 80 + 75 + 102 = 257 credits at their defaults (take the current figures from each), plus 20 x 2 = 40 for the cross-platform lookups in step 2: about 300 credits. Dropping a platform saves its share. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Run each platform's search.** Follow [TikTok creators](../../tiktok/references/find-creators.md), [Instagram creators](../../instagram/references/find-creators.md) and [YouTube creators](../../youtube/references/find-creators.md) with the same niche, size and market. Take from each its shortlist: handle, profile URL, followers, median views, engagement, bio, `website`, last post date and sponsored posts. Do not repeat their steps here.
2. **Match people across platforms.** For the top 20 by engagement, look for the same person on the other platforms: `tiktok_get_profile`, `instagram_get_profile` or `youtube_get_channel` with the same handle (1 credit each, about 40 in all). Count it a match only with a second signal: the same `website` (on YouTube a site shows in the `bio`, since `website` is null there), a bio that names the other handle, or the same name with the same niche. A shared handle alone is not proof; handles are claimed by strangers. When a match turns up a platform the creator is active on, pull its listing (`tiktok_get_videos`, `instagram_get_reels` or `youtube_get_videos`, 1 credit a page) and work out median views there too.
3. **Put engagement on one scale.** For each person and platform, take the view rate and engagement rate as the [router](../SKILL.md#engagement) sets out, then express each as a multiple of the median of the candidates on that platform. A creator at 2.0 on TikTok and 0.6 on YouTube is strong on TikTok and weak on YouTube, whatever the raw numbers say.
4. **Cut to the list.** Apply the [floors](../SKILL.md#floors): active, niche fit, sponsored share. Rank by the best platform's engagement multiple first and the combined followers second, and keep the count the user asked for.
5. **Merge contacts.** Keep one contact per person: the platform playbooks already took a business email from the bio or ran the contact steps on the creator's site. Prefer a management or business address over a personal one. Only when step 2 turned up a site none of them checked, run the [contact steps](../../link-building/SKILL.md#contact-steps) on it with `limit: 10`.
6. **Deliver** one table, one row per person: name, platforms with handle and link, followers per platform, combined followers, best platform, median views there, view rate and engagement multiple per platform, sponsored share, last post, one post URL that shows the niche fit, country signal, contact and where it came from. Mark the platform-only rows (no match found) so the user knows they were checked.

## Judgment

- Pay for the platform where a creator's median views are, not where their follower count is. Most creators are strong on one platform and carry an old following on the others.
- The engagement multiple is relative to this candidate set. In a narrow niche with fewer than ten candidates on a platform, the median is noisy; say so and show the raw rates.
- Search on every platform is ranked and never complete. A creator the user expected and did not find is a reason for a second pass with more keywords, not proof they are absent; searches are cached for 6 hours, so repeats are cheap.
- Instagram search is by hashtag only, so Instagram creators who do not tag their posts are under-found. Say so when Instagram is the user's main platform.
- A list is not a vetting. Before any fee, run [vet a creator](vet-creator.md) on the shortlist.
