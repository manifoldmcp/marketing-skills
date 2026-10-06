# TikTok audience check

Before the user pays a TikTok creator: is the audience real, does it respond, and is it in the countries the user sells to. It reads the creator's numbers, a sample of comments and followers, and TikTok's audience split by country. It ends in a pass, check or fail per creator, with the reason.

## Inputs to settle first

- **Creators**: one to five TikTok handles, usually the finalists from [find-creators](../../find-creators/SKILL.md).
- **Target market**: the countries the user sells to. Default: the user's own country; ask if unknown.
- **Threshold**: the share of the audience that must sit in the target countries. Default: 50% or more passes, 30% to 50% needs a closer look, under 30% fails. A sponsored video pays back only from viewers who can buy.
- **Budget**: a default run costs about 1 + 2 + 2 + 1 + 26 = 32 credits per creator, 96 for three. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Check the handle.** `tiktok_get_profile` (1 credit): the account exists, its followers, `verified`, bio and `location` where published. Stop on `NoData` and fix the handle: step 5 costs 26 credits and a wrong handle wastes them.
2. **Engagement.** `tiktok_get_videos` with `sort: "latest"`, two pages (1 credit a page): median views, median views over followers, median engagement rate, how widely views swing between videos, and the views of any paid posts (`is_ad: true`) against the median. Hold them to the [floors](../../create-tiktok-plan/references/platforms/tiktok.md#floors).
3. **Comment quality.** `tiktok_get_comments` on two recent videos near the median (1 credit each). A real audience asks questions, answers what was said, and writes in the creator's language. Warning signs: emoji-only comments, generic praise ("nice", "love this") from the same few accounts, comments in languages the creator never uses.
4. **Follower sample.** `tiktok_get_followers`, one page (1 credit): names, bios and each follower's own `followers`. A page of generated names with empty bios and no followers is a warning sign. Many real TikTok users only watch, so this is weak evidence on its own.
5. **Audience split.** `tiktok_get_audience` (26 credits): countries by `share`, largest first. Ignore `count`: it counts followers in the sample, not in the account. Add up the shares of the target countries.
6. **Deliver** a table: creator, followers, median views, views over followers, median engagement rate, comment quality (one line and an example), follower sample (one line), top three countries with their share, the target-market share, verdict (pass, check, fail) and the reason. Pass needs the target share at or above the threshold, the floors met and real comments; one warning sign makes it a check; a target share under 30% or two warning signs make it a fail.

## Judgment

- The split comes from a sample of a few hundred followers. A difference of five points between two creators is noise; read the shape: one country dominant, or spread thin.
- The split describes followers, not viewers. For a creator whose videos routinely get more views than they have followers, most viewers come from TikTok's feed and may sit elsewhere. Say that the split is weaker evidence for them.
- The split is by country only. Age, gender and interests are not in the tools.
- Only TikTok publishes an audience split. For a creator on Instagram or YouTube, the full vetting in [vet-creator](../SKILL.md) uses other signals; if the creator is also on TikTok, run this check on that handle as a proxy.
- An engaged audience in the wrong country is a fail for a sales campaign. It may still suit awareness if the user sells worldwide; say which the verdict assumes.
- The split is cached for 7 days, so checking the same creator again within a week is free.
