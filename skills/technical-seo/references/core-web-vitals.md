# Core Web Vitals and page experience

How much speed and page experience matter for Google rankings, how to read them, and when they are worth fixing. Checked October 2026.

## The metrics

| Metric | Measures | Good | Poor |
|---|---|---|---|
| LCP, Largest Contentful Paint | Loading | 2.5 s or less | over 4 s |
| INP, Interaction to Next Paint | Responsiveness | 200 ms or less | over 500 ms |
| CLS, Cumulative Layout Shift | Visual stability | 0.1 or less | over 0.25 |

- A page passes when all three are good at the **75th percentile of real Chrome visits**, mobile and desktop separately. INP replaced FID in March 2024; Google says stable metrics change at most once a year; we found no change to the three or their thresholds announced in 2025 or 2026.
- **Field data, not lab data.** Ranking uses the Chrome UX Report (real visits). Lighthouse and lab scores are diagnostics. A URL with too little Chrome traffic has no field data of its own and is judged by its group or origin.
- **How common passing is:** 48% of sites pass all three on mobile and 56% on desktop (HTTP Archive Web Almanac 2025, CrUX, July 2025). LCP is the usual failure (62% good on mobile); INP passes on 77% of mobile sites and 97% of desktop; CLS on 81% mobile.

## How much it counts

- Google's page experience guidance (updated September 2026): Core Web Vitals are used by ranking systems, but there is "no single signal" for page experience, good scores do not guarantee rankings, Google will rank the most relevant page even with a poor experience, and chasing a perfect score for SEO "may not be the best use of your time".
- So: a relevance and content problem is never solved by speed work. Speed is worth fixing for rankings where pages are **poor** (not merely "needs improvement") on templates that earn traffic, in a market where competitors are close on everything else, and for conversions, where the case is stronger than for rankings.
- No controlled study since INP's arrival has isolated its ranking effect that we found. Correlation studies cannot separate fast sites from well-funded ones. Say that when a client asks for a number.

## Reading it

- **Search Console's Core Web Vitals report** groups similar URLs and shows field data per group: the place to find which template fails. It needs enough traffic; small sites often see "not enough data".
- **PageSpeed Insights** shows field data for a URL and its origin above the lab test. Quote the field section.
- **Manifold has no Core Web Vitals or Lighthouse data.** The crawl's `high_loading_time` and `has_render_blocking_resources` are the crawler's own timing and an HTML pattern, not field data ([audit](audit.md#what-the-crawl-does-not-measure)). Never report them as Core Web Vitals.
- **Usual fixes by metric:** LCP, the hero image (size, format, priority hint, no lazy-loading above the fold) and server response time; INP, long JavaScript tasks from tag managers, chat widgets and hydration; CLS, images and ads without reserved dimensions, late-loading fonts and banners.
