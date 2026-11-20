# Earned media in AI answers

Why press coverage now matters as much for what ChatGPT, Claude and Gemini say about a brand as for links. Checked October 2026. Measuring AI answers, and the rest of AI search, is in [ai-search](../../ai-search/SKILL.md).

## What AI engines cite

Muck Rack's Generative Pulse studies are the largest public data on this. Muck Rack sells PR software, and the full reports sit behind a form, so the figures below come from its press releases and the trade coverage of them.

| Edition | Sample | Earned media | Journalism | Other |
|---|---|---|---|---|
| Mid-2025 (first edition) | 1M+ cited links | 89% | 27% | 95% non-paid; journalism about half of citations on recent-news queries |
| December 2025 | 1M+ cited links | 82% | 20% to 30% over any period | 94% of all citations non-paid; press releases about 6% |
| May 2026 | 25M+ links from ChatGPT, Claude and Gemini, 17 industries | 84% | 27% | Paid and advertorial 0.3%; press releases fell, mostly because ChatGPT cites them less |

"Earned" here is broad: journalism, academic research, government sites and other third-party content. Across three editions the earned share held between 82% and 89%.

- **The engines differ.** 56% of ChatGPT's journalism citations were published within the past year, against 36% of Claude's; Claude leans more on academic, technical and trade sources (Muck Rack, March 2026). Fresh coverage moves ChatGPT faster; trade and expert publications matter more for Claude.
- **Paid placement barely shows up.** Advertorial and sponsored content were 0.3% of citations in May 2026. Buying a sponsored article to get into AI answers is the wrong spend.
- **Press releases are a small and shrinking share.** A release on the user's own newsroom page states the facts; it is the coverage of it that gets cited.

## Why it works

- What predicts a brand being named in AI answers is how much it is talked about elsewhere: branded web mentions correlate with AI visibility about twice as strongly as domain rating does (Ahrefs, 75K brands; detail in [ai-search](../../ai-search/SKILL.md)). Coverage is the most controllable way to be talked about.
- **A mention without a link still counts here.** For Google rankings a link matters; for AI answers being named in the article does. Report both.
- Engines cite pages that state facts plainly. A data story whose numbers appear in the coverage with the user's name beside them gives every engine the same sentence to repeat.

## Using it

- **Pitch the outlets the engines already cite for the user's buyer questions.** Run the user's buyer prompts through `aeo_run_ai_answers`, read the `citations[]`, and put the publications cited for two or more prompts, or on two or more engines, at the top of the media list. Those reporters' past articles are proven inputs to the answer.
- **Correct facts at the source.** When an AI answer gets the user wrong and cites an article, the fix is asking that outlet for a correction with evidence; the full method is in [ai-search](../../ai-search/SKILL.md).
- **Measure the effect two to four weeks after coverage runs,** on the same frozen prompt set, against the margin of error. No study has measured how long coverage takes to show in answers; engines that search live can move within days, answers from training data move with model releases.
