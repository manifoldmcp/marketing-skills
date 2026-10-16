# Post formats

Which formats, hooks, lengths and endings earn engagement, first from the large studies, then measured in the user's own niche, which is the number that matters. Checked October 2026.

## What the studies say

Formats move every quarter, and the studies disagree because they measure different things (reach against engagement rate, same-author against pooled, people against pages). Quote the source and date with any number.

| Format | Evidence | Read it as |
|---|---|---|
| Document carousel (PDF) | Lead over photo posts fell from +21% to +9% in mid-March 2026, same author (MagicPost). Highest engagement rate on company pages, 7.0% (Socialinsider, 2025 data). Reach multiplier 1.45 for people, 1.40 for pages (AuthoredUp, 2025) | Still a good format, no longer a cheat code |
| Photo or multi-image | Multi-image 6.45% and image 5.3% engagement on pages (Socialinsider); 1.18 to 1.21 reach multiplier (AuthoredUp) | The baseline the others are compared with |
| Native video | 6.0% engagement on pages, but page video views down 36% year on year (Socialinsider); 1.05 to 1.10 multiplier (AuthoredUp); LinkedIn says watch time rose 36% (2024) and suggests two videos in 2 to 5 posts a week | Worth it where the person is good on camera; not a reach shortcut |
| Text only | 0.88 multiplier for people, 0.42 for pages (AuthoredUp); 4.5% on pages (Socialinsider) | Fine for a person with a strong first line; weak for a page |
| Poll | Fell from +9% to −22% against photo posts in mid-May 2026 (MagicPost); 4.2% on pages | Avoid unless the answers are the point |
| Link post | 3.25% on pages (Socialinsider); −19% to −21% reach same-author (MagicPost) | See [the feed](feed.md#links-hashtags-timing) |
| Article or newsletter | About 0.4 times normal feed reach (van der Blom), but cited by AI engines more than posts and delivered to subscribers | A different job: see [profiles and pages](profiles-and-pages.md#newsletters-and-articles) |

- **Length:** AuthoredUp puts the best reach at 800 to 1,000 characters and finds posts above a 10th-grade reading level get over 35% less reach (vendor data, method not published). LinkedIn says longer, substantive updates hold interest; no study shows a length rule that beats substance.
- **The first line carries the post.** LinkedIn shows two or three lines before "see more", and the first line becomes the post's URL. Quote real first lines when reporting what works; never describe them in the abstract.
- **Substance beats structure.** Since July 2026 posts with a concrete detail and lived experience run 15% above their author's average and posts with neither 23% below; none of 17 formatting features MagicPost tested moved reach by more than 6%.

## Measuring the niche

The studies say what works on average. The niche's own top posts say what works for these buyers. Default: four or five searches in the words the ICP uses, `linkedin_search_posts` with `since: "month"` (add `since: "year"` in a quiet niche), three pages each, for 80 posts or more: about 15 credits, plus 10 for the user's own last 10 posts with `linkedin_get_post`.

Code each post from its text:

- **Length:** under 500 characters, 500 to 1,300, 1,300 to 2,000, cut (longer, known only as long).
- **Hook, the first line:** a number or result, a contrarian claim, a story opening, a question, a how-to promise, news.
- **Structure:** list or numbered steps, story, one-liner, framework, lessons, before-and-after.
- **Ending:** question to readers, link, "comment X" giveaway, none.
- **Substance:** a concrete detail (number, name, case) and lived experience, each yes or no. This is now the strongest signal, so code it.
- **Other:** a link in the text, people tagged, hashtags, a real video (non-null `transcript`).

The media type cannot be read from the tools (see [reading the data](data.md#field-traps)); add it only for posts the host or the user opened.

Compare each feature's group on median likes and comments and, since authors differ in size, median engagement per 1,000 followers. Leave giveaways and suspect posts out of the medians, and compare only groups of 10 or more.

## Delivering it

A table: feature, group, posts, median likes, median comments, median per 1,000 followers, and an example (URL and real first line). Under it, three to five rules for this niche, each with its evidence ("posts of 800 to 1,300 characters with a concrete number in the first line earn twice the niche median per 1,000 followers, 14 posts"), and where the user's own posts sit against the medians.

- These are correlations from a sample that favours posts that already did well. Big authors write in their own style; per-follower numbers reduce that, not remove it. Say so.
- Rules do not travel: a recruiting audience and a developer audience reward different posts.
- Many authors in a niche avoiding links is itself a finding.
- The tools see engagement, not what converts. When the user asks which format brings customers, say the tools cannot answer that and ask what their CRM shows.
