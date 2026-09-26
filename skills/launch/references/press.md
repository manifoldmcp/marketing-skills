# Launch press

Launch coverage goes to reporters who had the story before the day and a reason to care. The list itself comes from the link-building group's journalists playbook; this playbook finds the angle that got similar launches covered, sorts the list into who hears first, and sets the dates around the embargo.

## Inputs to settle first

- **News**: what is new beyond "we launched": a new category, numbers, a known customer, funding, a founder with a story. Without one, trade press and newsletters are realistic and national press is not; say so.
- **Launch date and time zone**: the embargo lifts then.
- **Embargo**: whether the user will brief reporters before launch under embargo. Default: yes, for launch-day coverage.
- **Assets**: a press kit (the release, screenshots, founder photo and bio, pricing, a customer who will talk) and a demo.
- **Competitors**: two or three whose launches got coverage.
- **Market**: `location` and `language` if not the United States and English.
- **Budget**: about 3 credits here plus the [journalists](../../link-building/references/journalists.md) run (about 365 credits for 15 publications at its default): about 370 in all. Say so before starting; pass `max_credits` if the user gave a budget.

## Steps

1. **Find which launch news got covered.** `seo_get_serp` for "<competitor> launches", "<competitor> announces" and "<category> launch" (1 credit each). Keep the articles from publications, not the vendors' own pages. They show which outlets cover launches in the category, and the angle that got each one covered: the funding, the feature, the numbers, the founder.
2. **Build the list.** Run [journalists](../../link-building/references/journalists.md) with the launch as the story, the angle from step 1 and the competitors. Take its table: reporter, publication, the article that shows the beat, email and verification status.
3. **Tier the list.** Sort the reporters into four tiers:
   - Exclusive: at most one outlet, the one that matters most to the ICP, offered the story first at T-14 under embargo.
   - Embargoed briefing: 5 to 10 reporters whose last article is closest to the story, briefed at T-7 with the embargo time and time zone.
   - Launch day: trade press, newsletters and bloggers, pitched on the morning with the news live.
   - Follow-up: reporters who did not reply, pitched again at T+7 with results (signups, what people said from the [reaction report](reaction-report.md), a customer story).
4. **Check the press kit.** List what each tier needs and what is missing. No tool builds it; it is the user's checklist.
5. **Deliver** two tables. The list: reporter, publication, tier, the article that shows the beat, the angle for this reporter, pitch date, embargo (yes or no), email, verification status. The timeline: date (T-14, T-7, launch day, T+7), who is pitched, what they receive. Nothing is sent from here; the user or the host's email tool sends.

## Judgment

- An exclusive gets one outlet's full attention and can cost the others. Offer it only when that outlet reaches the ICP better than the rest combined.
- An embargo holds when the reporter agrees to it before seeing the details. Ask first, and send the details after the yes. A reporter who breaks an embargo leaves the list.
- Pitch the story the reporter's last article shows they care about, not the product's features.
- Most launches are not national news. Trade press, newsletters and podcasts are the realistic tier; podcasts to guest on are in [podcast guesting](../../youtube/references/podcast-guesting.md).
- A launch-day pitch to a stranger rarely lands the same day. The T+7 follow-up with results is the second chance, and numbers make it stronger.
- Coverage after T+30 is no longer launch press: a [PR strategy](../../link-building/references/pr-strategy.md) takes over.
