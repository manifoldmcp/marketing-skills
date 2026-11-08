# Getting named and cited

What engines cite, how being cited differs from being recommended, and how to get onto the sources that shape answers. Checked October 2026.

## Mentioned and cited are different outcomes

- **A mention** is the brand named in the answer. **A citation** is a link to a page the answer drew on. Buyers act on the mention; the citation is how the engine got there.
- They come apart more often than not. In every month of January to July 2026, fewer than half of the 100 most-mentioned brands were among the 100 most-cited domains, on every engine Semrush tracks; ChatGPT overlapped least (29 to 36 of 100). Ford was mentioned on all four engines but ranked 1,055th for citations on ChatGPT.
- **What predicts being mentioned is being talked about elsewhere.** Across 75K brands, branded web mentions correlated with AI Overview visibility at ρ 0.66, against 0.33 for domain rating and 0.22 for backlink count (Ahrefs, 2025; correlation, large brands). YouTube mentions correlated most strongly of any signal (ρ about 0.74, Ahrefs, 2026). For ChatGPT, SE Ranking found referring domains the strongest predictor, so links are not irrelevant there.
- **A brand cited but not recommended** is usually citing itself: its own page answers a question, the engine borrows the facts and names someone else. **A brand recommended but never cited** is being learned about on third-party pages; those pages are the work.
- AI recommendations raise direct and branded visits within a week, not only clicks on citations (Similarweb panel, two large brands, June 2026). Referral sessions in analytics undercount AI's influence.

## What gets cited

- **Across engines, the most-cited domains are Reddit, YouTube, LinkedIn, Wikipedia, Forbes, G2, Yelp, Facebook, Medium and TechRadar** (Peec AI, 30M sources, March 2026, before ChatGPT's August Reddit drop). Only Reddit and YouTube are top five on every engine. ChatGPT leans editorial, Google's surfaces lean Facebook and Yelp, Perplexity leans review sites. Verticals override all of this: in travel, Booking and Airbnb beat Reddit.
- **Rank sources by the citations in the user's own answers, never by domain rank.** The engines have already said which pages they trust for these prompts. A source cited by two or more engines, or for two or more prompts, is a target; one cited once is noise unless it is the only source for a prompt that matters.
- **Citation sets churn.** About half of cited domains rotate week to week on Google's surfaces and about three-quarters on ChatGPT (SISTRIX, 82,619 prompts over 17 weeks); 40% to 60% change month to month (Profound). URLs churn faster than domains. A domain that keeps reappearing is worth more than any single URL.

## Freshness

- 65% to 78% of pages cited by ChatGPT, Gemini and Perplexity were updated within a year, but only 42% were published within a year (Seer Interactive, 47,097 citations, four brands, March to June 2026). The freshness engines reward comes from substantive updates to existing pages, not new pages.
- Changing the date without changing the page is a risk on Google (see [Google's AI answers](google-ai-overviews.md)). Update the facts, the examples and the numbers, then the date.
- How long a change takes to show in answers has never been measured in a controlled study. Practitioner estimates: Perplexity in days, ChatGPT in one to four weeks, Google's AI answers from a day to two months. Re-measure two to four weeks after a change, and say the timing is unknown.

## Routes by source type

Read each cited URL's domain and title, and `seo_get_page` (free) when the type is unclear.

- **Editorial article or guide:** a contact target. The ask is to make the page more complete or more correct for its readers, with evidence the user supplies (data, a quote, an updated fact). Never offer payment for an undisclosed mention.
- **Best-of list on a publisher:** ask for inclusion with a specific angle (a use case, a price point, what the user does best). Many are affiliate pages and the editor may ask for a fee; flag those and let the user decide. After GPT-5.6, ChatGPT cites listicles half as often as before (15.8% to 7.8% of citations, Peec AI, August 2026; one travel dataset showed the opposite), while Gemini and Perplexity showed no reported drop. Lists still matter, but less on ChatGPT and per vertical.
- **The user's own "best X" or comparison page:** on Google these now tend to help competitors (see [Google's AI answers](google-ai-overviews.md)). An honest comparison page that answers a real buyer question is fine; a self-ranking listicle written for AI citations is not worth making.
- **Reddit thread:** on Perplexity and Google's surfaces, still a top source; on ChatGPT, retrieved but rarely shown since August 2026. Take part only where the subreddit's rules allow, with a reply that adds what the thread lacks (a newer option, a correction, a number). Never ask for upvotes or use a second account. A locked or archived thread cannot take a reply; a newer thread on the same question can. `reddit_get_post` shows `locked` and `archived`; `reddit_get_comments` shows who the thread recommends.
- **YouTube video:** a top source on every engine and the fastest-growing in AI Overviews. Either get featured (an active creator, a review, comparison or list format, the user missing; the ask is an update, a pinned correction or a place in the next video, sponsored if there is budget, and a paid mention must be disclosed on YouTube) or make the better video when the cited one is old, from a dead channel, or a competitor's own. `youtube_get_transcript` shows whether a video names the user.
- **LinkedIn:** third most-cited domain across engines in Peec's March 2026 data, mostly for professional and B2B questions. Posts and articles from the founder and team are the user's own sources there.
- **Review site or directory** (G2, Capterra, Trustpilot, Yelp and the like): no Manifold tool reads them, except a Google Business Profile, which `maps_get_place` reads (rating, rating breakdown, review topics). Claim the profile, complete it, ask customers for reviews. Matters most on Perplexity and for local businesses on Google.
- **Wikipedia:** weighs heavily in AI Mode. Never suggest editing a page about the user's own company; a missing or wrong page is fixed by the coverage Wikipedia's editors would cite.
- **A competitor's own page:** cannot be joined. The answer is the user's own page on the same question.
- **For a shop:** ChatGPT's product picks now come mostly from merchant feeds. The feed's titles, attributes and availability are the AI search work there.
