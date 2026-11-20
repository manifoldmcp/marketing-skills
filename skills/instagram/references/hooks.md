# Hooks

How the niche's best reels open and how its best captions start, sorted into patterns the user can adapt. Since August 2025 Instagram shows creators a skip rate (viewers gone in the first three seconds), and its own guidance (September 2026) is that a reel must hold from the first second to be recommended: the opening is where most reels fail.

## What the evidence says about openings

- **Speech and a face early.** Speech in the first three seconds lifted 10-second retention by almost 25% against music alone, and a person on screen by about 10%, on 10,110 Facebook Reels (Emplifi, March 2026). The same Meta Reels system; not tested on Instagram itself.
- **Instagram's own advice** (Reels guide, September 2026): a text overlay as the hook, captions on spoken audio for people watching muted, and a hold from the first second. Meta's hook guidance for ads (December 2025) names three types: a promise of value, a statement of what the viewer will see, and a question or invitation.
- **The first caption line is a hook too.** Only two lines show before "more", and since 2025 the caption is also what Instagram search and Google's results read (see [ranking](ranking.md#hashtags-captions-and-search)). Plain keywords in the first line serve both.
- **Unknown:** no study measures which hook pattern wins on Instagram with a method. The table below is evidence from the niche, not a law.

## Method

1. **Collect.** From niche hashtags, `instagram_search_posts` with `since: "month"` (`since: "year"` for a small niche), two pages a tag; or from named accounts, two pages of `instagram_get_reels` each. Keep `media: "video"` for reels, drop `is_ad: true` rows and comment-for-DM or giveaway posts.
2. **Keep the reels the opening lifted.** For up to 10 authors, one page of `instagram_get_reels` (1 credit each) for their median views. Keep the 20 reels with the highest multiple of their author's median, not the most views: a big account's ordinary reel says nothing about its hook. Drop reels whose engagement over views is under half their author's median. Fewer than 12 reels is too few to see patterns.
3. **Read the opening.** `instagram_get_transcript` on each kept reel (1 credit; transcripts carry no timestamps, so take the first sentence, or its first clause when it runs long). The first line of `text` is the caption hook. `NoData` means the hook is on screen, which the tools cannot read: keep the caption line and mark it. If most top reels have no speech, say that the niche's hooks are visual and the table shows half of them.
4. **Sort into patterns:** a question to the viewer, a bold or contrarian claim, a number or list, a callout of who it is for, a story opening, the result first, a mistake or warning, a POV setup, a "save this" promise. Count posts, median views and median multiple per pattern; report patterns with three or more posts first.
5. **Deliver** a table: pattern, what it does, posts, median views, median multiple, two or three hooks quoted word for word with link, author and views, and whether the hook is spoken or in the caption. About 36 credits for three hashtags.

## Rules

- Adapt the pattern, never the words. Quoted hooks are evidence; reusing a creator's script is both copying and, since April 2026, a reach risk under the originality rules.
- Write new hooks for the user only when asked, each on a pattern the table shows, and suggest testing two openings as Trial Reels before committing the grid.
- Image posts ranked by raw likes favour big accounts: treat their caption hooks as weaker evidence.
- Keep Instagram and TikTok tables apart; views compare only within one platform ([tiktok](../../tiktok/SKILL.md) holds TikTok's hooks). Hooks from Instagram ads belong to [paid-ads](../../paid-ads/SKILL.md).
