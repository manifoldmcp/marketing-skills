# Instagram hooks

How the niche's best reels open and how its best captions start: the first spoken line from the transcripts of the top reels, and the first line of each caption, sorted into patterns. It ends in a table of hook patterns, each with real examples and links, that the user can adapt.

## Inputs to settle first

- **Source**: niche hashtags (default: the category tag and two tags that recur in its captions), or a list of accounts the user wants to learn from (competitors, creators they admire).
- **Window**: `since: "month"` by default; `since: "year"` for a small niche.
- **Sample**: 20 reels by default, plus the captions of the top image posts from the same pull. Fewer than 12 reels is too few to see patterns.
- **Budget**: a default run costs about 3 x 2 + 10 + 20 = 36 credits for three hashtags. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Collect the top posts.** From hashtags: `instagram_search_posts` for each tag with `since: "month"`, two pages each (1 credit a page). From accounts: `instagram_get_reels`, two pages each (1 credit a page), sorted by views yourself since the listing runs newest first, and `instagram_get_posts`, one page each (1 credit), for the image posts. Drop rows with `is_ad: true` and reels under the reel engagement [floor](../SKILL.md#floors).
2. **Keep the reels the opening lifted.** For up to 10 authors from the hashtag pull, `instagram_get_reels`, one page each (1 credit), for their median reel views. Keep the 20 reels with the highest multiple of their author's median, not the most views: a big account's ordinary reel says nothing about its hook. From an account source, the listing already gives the median. Also keep the 10 image posts with the most likes plus comments; their only readable hook is the caption.
3. **Read the opening.** `instagram_get_transcript` on each kept reel (1 credit). The first spoken sentence is the spoken hook; the first line of `text` is the caption hook, the part a viewer sees before the caption is cut. `NoData` means no speech: the hook is on screen, which the tools cannot read, so keep the caption line and mark it. A reel over two minutes comes back `InvalidTarget`: keep its caption line only.
4. **Sort into patterns.** Put each hook in one pattern: a question to the viewer, a bold or contrarian claim, a number or list ("5 mistakes..."), a callout of who it is for ("If you rent your flat..."), a story opening, the result first, a mistake or warning, a POV setup, a "save this for later" promise. Count the posts, and take the median views (reels) and the median multiple for each pattern.
5. **Deliver** a table: pattern, what it does in one line, posts using it, median views, median multiple of the author's median, two or three hooks quoted word for word each with the post link, author and views or likes, and whether the hook is spoken or in the caption. If the user asked, add three hooks for their own topic under each of the top three patterns.

## Judgment

- A reel's hook is the first one to three seconds. Transcripts carry no timestamps, so the first sentence is the nearest reading; when it runs long, take its first clause.
- The tools read words, not pictures. Text on screen, the first frame and a carousel's first slide are invisible. If most top reels have no speech, say that the hooks in this niche are visual and the table shows only half of them.
- Image posts are ranked here by raw likes, which favours big accounts. Treat their caption hooks as weaker evidence than the reels'.
- Report patterns with three or more posts first. A pattern seen once is an anecdote.
- For reels with transcripts in several languages, pass `language` to get the one the user needs.
- Adapt the pattern, never the words. Quoted hooks are evidence, not scripts to reuse.
- Hooks from paid Instagram ads belong to the [paid-ads](../../paid-ads/SKILL.md) group's [swipe file](../../paid-ads/references/swipe-file.md).
