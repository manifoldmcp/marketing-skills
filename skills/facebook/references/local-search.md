# Facebook in Google and AI answers for local businesses

Where a local business's Facebook page and the town's Facebook groups show up when people search, in Google's results and in AI answers. Checked October 2026.

## In Google's results

- **Facebook ranks for local "best X" searches, mostly through groups.** In six local searches we ran (US and UK, October 2026), Facebook appeared in four: group threads asking for recommendations in Google's discussions and forums block, group threads as ordinary organic results (positions 3 to 7 on two searches), and a business's own page at position 8 with its recommendation score in the snippet. Yelp appeared in three, Reddit in five ([field notes](field-notes.md)). Six searches show the pattern exists, not how common it is.
- **Practical reading:** the town group thread where locals recommend businesses is itself a search result. Being the business members name in it, and answering where rules allow, works twice: in the group and in Google.
- **The page is a listing too.** A complete page (category, hours, phone, address, website) and its recommendations show in Google's snippet. Facebook recommendations cannot be read by the tools; the snippet in `seo_get_serp` sometimes shows them.
- We also saw a facebook.com/fb-answers/ page summarising plumber recommendations rank fifth. How Meta generates these and whether a business can influence them is unknown.

## In AI answers

- **Across engines, Facebook and Yelp are among the ten most-cited domains,** and Google's AI surfaces lean toward them more than ChatGPT does (Peec AI, 30M sources, March 2026). That is across all query types.
- **For local recommendations our test found neither.** On five local prompts (dentist, plumber, bakery, yoga studio, bookshop) across ChatGPT, Gemini and Google AI Mode, none of 15 answers cited facebook.com or yelp.com, and none of ChatGPT's 64 retrieved pages was on either. Gemini cited only Google Maps listings; AI Mode cited business sites, local press and Reddit; ChatGPT cited local guides, directories and business sites. Google showed no AI Overview on any of the five. A small sample (October 2026, 40 credits), but it says the local AI answer is built from the Google Business Profile, the business's own site and local editorial lists, not from the Facebook page.
- **So for a local business,** the Facebook page is not the AI search lever. The Google Business Profile and its reviews, the website, and being named in local guides are; that work is [ai-search](../../ai-search/SKILL.md). Facebook still matters for the groups where locals ask, which feed Google's classic results.

## Checking it for a user

- `seo_get_serp` on three to five of the buyer's local searches (1 credit each, 2 with `ai_overview: true`): note every facebook.com row, its type (group thread, page) and position, and every Yelp and Reddit row.
- For AI answers, `aeo_run_ai_answers` with the same questions on the engines the buyers use; for local, chatgpt, gemini and ai_mode are 6 credits a prompt. Read `citations[]` for the domains. Set `location` to the market. Reading those answers well (noise, margins) is [ai-search](../../ai-search/SKILL.md).
