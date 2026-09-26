# Campaign brief

The brief is what the user sends each creator before they post: what to make, what to say and not say, how to disclose it, and what the user may do with it afterwards. Good briefs give the message and the evidence and leave the words to the creator, because creators know what their audience watches. This playbook grounds the brief in what already works in the niche and ends in a one-page document.

## Inputs to settle first

- **Goal and KPI**: sales through a code, sign-ups through a link, awareness, or content to reuse. It decides the CTA and the tracking.
- **Product and offer**: what the creator receives, the one message the post must land, and the code or link.
- **Creators**: the shortlist from [find creators](find-creators.md) or [UGC creators](ugc-creators.md), and their tier. A UGC brief covers videos for the user's own channels; an influencer brief covers posts on the creator's.
- **Deliverables**: platforms, formats, lengths, number of posts, draft and live dates. Default: one TikTok or Reel of 30 to 60 seconds, one draft round, live within three weeks.
- **Usage rights**: organic reposts only, or paid ads from the user's account or the creator's handle (whitelisting, Spark Ads), and for how long. Default: organic reposts for 90 days; paid use costs extra and is named in the brief.
- **Market**: the country, for the disclosure rules that apply.
- **Budget**: a default run costs about 2 + 2 + 5 + 3 = 12 credits: two pages of TikTok search, two of Instagram hashtag search, five transcripts and comments on three posts. The [hooks](../../tiktok/references/hooks.md) playbook states its own cost if the user wants it run fresh. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find the niche's sponsored posts.** `tiktok_search_videos` with the niche keyword and `sort: "popular"`, and `instagram_search_posts` with the niche hashtag, two pages each (1 credit a page). Keep the rows with `is_ad: true` or a sponsored caption, and add competitors' names as queries if they sponsor creators. These show what paid posts in the niche look like when they get watched.
2. **Hooks that work in the niche.** Take the hook patterns and example videos from [TikTok hooks](../../tiktok/references/hooks.md). Then `tiktok_get_transcript` or `instagram_get_transcript` on the five most viewed sponsored posts from step 1 (1 credit each): how they open, at what point the product comes in, whether it is a story, a list or a demo, and how they ask for the click.
3. **What the audience asks.** `tiktok_get_comments` or `instagram_get_comments` on three of those posts (1 credit a page). Collect the questions and objections ("does it work on curly hair", "is it worth the price", "link?"). The brief asks creators to answer the top three on camera.
4. **Write the do and don't.** Do: say it in your own words, open with your own hook, show the product in use, answer the audience's questions from step 3, one clear call to action with the code. Don't: read a script, claim what the user cannot prove (health outcomes, earnings, "best"), name or mock competitors, show before and after images where the platform bans them, promise a price that may change.
5. **Disclosure.** Every paid or gifted post turns on the platform's paid partnership label (TikTok's content disclosure setting, Instagram's paid partnership label, YouTube's paid promotion box) and says it is an ad at the start: "ad" or "sponsored" at the front of the caption, and said out loud in the video. In the US the FTC requires the disclosure to be clear and hard to miss, so a tag buried at the end of the caption does not count; the UK and the EU have their own rules. The creator discloses; the user makes it a condition in the brief.
6. **Deliver** the brief as one page: the brand in two lines; the goal; the one message and at most three talking points; a deliverables table (platform, format, length, count, draft due, live date); hooks that work in the niche, each with an example link; the audience's questions to answer; do and don't; disclosure; usage rights and exclusivity; the approval process (one draft, one round of changes); tracking (the creator's code and link); and a line for fee and payment terms that the user fills in. Never send it; hand it to the user.

## Judgment

- Brief the message, not the script. Scripted posts sound like ads and do worse than the creator's own median, which is the number the user will judge them by.
- One message per post. A creator asked to land five features lands none.
- Hooks that work organically are the reference for creator posts. Ad hooks from the `paid-ads` group's creative brief fit UGC videos the user will run as ads; say which one this brief is.
- Usage rights and exclusivity cost money. State them before the creator quotes, not after the post is live.
- The tools read public posts. They cannot see a post's paid boost, its saves or its sales; the tracking line in the brief is how the user will measure it.
