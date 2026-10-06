# Marketing Skills

**Marketing skills for AI agents that look at the data before they answer.** Built by [Manifold](https://www.manifoldmcp.com).

Ask an agent why your traffic fell, who your competitors pay to reach or which creators fit your niche, and it usually answers from memory. These skills give it the numbers instead. Each one does a single marketing job, calls the Manifold MCP server for live search, AI answer, social, ad library and company data, and hands you a table you can act on.

Claude Code, Codex and Gemini CLI install the skills and the connector together. Cursor, Cline and other agents that read [Agent Skills](https://agentskills.io) get the skills and connect the server themselves. The skills are MIT-licensed; the data runs on prepaid [credits](#credits).

Questions, bugs and skill requests go to [issues](https://github.com/manifoldmcp/marketing-skills/issues).

## Quick start

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold@manifold
```

In Claude Code, run `/mcp` to sign in to Manifold, then `/manifold-get-started`. It writes down what your business sells and to whom, drafting from your site so you only fill the gaps, then picks three to five skills for your goal in the order to run them. Other agents are under [Install](#install).

## What a run looks like

```
"Why did our signups from Google drop in March?"
        │
        ▼
 diagnose-traffic-drop ──reads──▶ .agents/product-marketing.md
        │
        ▼
 Manifold tools: Search Console, rankings, SERPs, page crawl
        │  (cost stated before the first paid call)
        ▼
 A table: which pages lost what, the likely cause, what to fix first
```

Every skill follows the same pattern:

- **Context first.** `manifold-get-started` writes what you sell, who buys it, your competitors, positioning and goals to `.agents/product-marketing.md` once, and every skill reads it before it asks you anything. It works for B2B software, shops, local services, apps, creators, agencies, marketplaces and nonprofits. The path and sections match other marketing skill libraries, so one file serves them all.
- **Exact tools, known cost.** The skill names the Manifold tools to call and what each costs, says the total before it spends anything, and caps every call at your budget.
- **Judgment, written down.** How to read the results: which numbers to trust, what to filter out, when a signal is too thin to act on. Where a job differs by platform, the skill's `references/` folder holds the detail.
- **A table at the end.** Nothing here sends, posts, buys or schedules. You decide what to do with the result.

Plans hand off to the skills that do the work: `create-seo-plan` leads into `find-seo-quick-wins` and `write-seo-brief`, and `find-pain-points` pulls from `find-reddit-pain-points` and the comment skills. Each skill's **Related skills** section lists its neighbours.

## Ask it like this

You do not need to know a skill's name. Describe the job and the matching skill loads:

| You ask | The skill that runs |
|---------|---------------------|
| "Does ChatGPT recommend us when people ask for a CRM?" | `check-ai-visibility` |
| "Find TikTok creators in home fitness under 100k followers" | `find-tiktok-creators` |
| "What is our main competitor running on Meta right now?" | `research-meta-ads` |
| "Heads of marketing at Series A fintechs, with verified emails" | `build-lead-list` |
| "What do people on Reddit hate about project management tools?" | `find-reddit-pain-points` |

Or run one directly by name, such as `/audit-technical-seo`. If another plugin uses the same name, add the prefix: `/manifold:audit-technical-seo`.

## Install

**Claude Code.** The plugin carries the skills and the connector; see [Quick start](#quick-start).

**Codex.** The plugin carries the skills and the connector:

```sh
codex plugin marketplace add manifoldmcp/marketing-skills
codex plugin add manifold@manifold
codex mcp login manifold
```

The last command signs you in. Ask "Where should I start with Manifold?" to pick skills, and run `codex plugin marketplace upgrade manifold` for a new release.

**Gemini CLI.** The extension carries the skills and the connector:

```sh
gemini extensions install https://github.com/manifoldmcp/marketing-skills
```

Run `/mcp auth manifold` in Gemini CLI to sign in, and `gemini extensions update manifold` for a new release.

**Cursor and other agents.** [npx skills](https://github.com/vercel-labs/skills) installs every skill, or only the ones you name with `--skill`:

```sh
npx skills add manifoldmcp/marketing-skills
```

These hosts get the skills without the connector. Add the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp` with the steps for your client at https://www.manifoldmcp.com/docs/clients. Every skill checks for the connector first and tells you if it is missing.

**Cline.** See [llms-install.md](llms-install.md) for the server configuration and sign-in (OAuth or API key).

## The skills

Names start with the verb: `create-` a plan, or `find-`, `audit-`, `check-`, `mine-`, `write-`, `monitor-`. The library is flat, so install all of it or only what you need; the groups below are for browsing.

### Start here

| Skill | What it does |
|-------|--------------|
| [`manifold-get-started`](skills/manifold-get-started/) | Writes down once what every skill needs to know about your business (what you sell, who buys, competitors, positioning, customer language, goals) in `.agents/product-marketing.md`, then picks three to five skills for your goal in the order to run them. |

### SEO

| Skill | What it does |
|-------|--------------|
| [`create-seo-plan`](skills/create-seo-plan/) | A 90-day SEO plan from where the site stands, its competitors and the keyword gaps. |
| [`audit-technical-seo`](skills/audit-technical-seo/) | What stops Google crawling and indexing the pages that matter. |
| [`diagnose-traffic-drop`](skills/diagnose-traffic-drop/) | Why organic traffic fell, and what to fix first. |
| [`create-migration-plan`](skills/create-migration-plan/) | Move domains, platforms or URLs without losing rankings. |
| [`find-seo-quick-wins`](skills/find-seo-quick-wins/) | Pages near page one that a small change moves up. |
| [`refresh-content`](skills/refresh-content/) | Pages that decayed or sit just off page one, and what to update. |
| [`create-seo-content-plan`](skills/create-seo-content-plan/) | Topic clusters and an order to publish them in. |
| [`write-seo-brief`](skills/write-seo-brief/) | A brief for one page that can win its keyword. |
| [`optimize-page`](skills/optimize-page/) | What one page is missing against the pages that beat it. |
| [`plan-comparison-pages`](skills/plan-comparison-pages/) | "X vs Y" and alternatives pages worth writing. |
| [`fix-keyword-cannibalization`](skills/fix-keyword-cannibalization/) | Pages of yours that compete for one query, and which should win. |

### AI search

| Skill | What it does |
|-------|--------------|
| [`create-ai-search-plan`](skills/create-ai-search-plan/) | A plan to get named and cited by ChatGPT, Claude, Gemini, Perplexity and Google's AI answers. |
| [`check-ai-visibility`](skills/check-ai-visibility/) | Which brands AI answers name and cite for your buyers' questions, once or on a schedule. |
| [`build-ai-citations`](skills/build-ai-citations/) | The sources AI engines cite for competitors and not for you, Reddit threads and YouTube videos included. |
| [`check-ai-overviews`](skills/check-ai-overviews/) | Which of your keywords show a Google AI Overview, and whether it cites you. |
| [`fix-wrong-ai-answers`](skills/fix-wrong-ai-answers/) | Find and fix what AI answers get wrong about you. |
| [`check-ai-crawler-access`](skills/check-ai-crawler-access/) | Whether AI crawlers can reach and read your site. |
| [`find-best-of-lists`](skills/find-best-of-lists/) | The best-of lists that rank on Google and that AI engines cite, and how to get on them. |

### Links and PR

| Skill | What it does |
|-------|--------------|
| [`create-link-building-plan`](skills/create-link-building-plan/) | A link building plan from your links, your competitors' and the gaps. |
| [`find-backlink-targets`](skills/find-backlink-targets/) | Sites worth a backlink, with a verified contact at each. |
| [`create-digital-pr-plan`](skills/create-digital-pr-plan/) | The stories that earn coverage in your space, in a 90-day plan. |
| [`find-journalists`](skills/find-journalists/) | The journalists who cover your story, with a contact for each. |
| [`find-podcasts`](skills/find-podcasts/) | Podcasts to guest on, with a contact for each. |
| [`find-unlinked-mentions`](skills/find-unlinked-mentions/) | Pages that name you without a link. |
| [`reclaim-lost-links`](skills/reclaim-lost-links/) | Links you lost, and which are worth winning back. |
| [`find-affiliate-partners`](skills/find-affiliate-partners/) | Sites that already promote products like yours. |

### Social media

| Skill | What it does |
|-------|--------------|
| [`create-tiktok-plan`](skills/create-tiktok-plan/), [`create-instagram-plan`](skills/create-instagram-plan/), [`create-youtube-plan`](skills/create-youtube-plan/), [`create-linkedin-plan`](skills/create-linkedin-plan/), [`create-facebook-plan`](skills/create-facebook-plan/) | A 90-day plan for one platform. |
| [`audit-tiktok-account`](skills/audit-tiktok-account/), [`audit-instagram-account`](skills/audit-instagram-account/), [`audit-youtube-channel`](skills/audit-youtube-channel/), [`audit-facebook-page`](skills/audit-facebook-page/), [`audit-linkedin-page`](skills/audit-linkedin-page/), [`audit-x-account`](skills/audit-x-account/) | Your own or a competitor's account. |
| [`find-tiktok-hooks`](skills/find-tiktok-hooks/), [`find-instagram-hooks`](skills/find-instagram-hooks/) | The opening lines that hold viewers in your niche. |
| [`find-tiktok-trends`](skills/find-tiktok-trends/), [`find-reels-trends`](skills/find-reels-trends/) | The trends your niche can use now. |
| [`analyze-viral-tiktok`](skills/analyze-viral-tiktok/), [`analyze-viral-reel`](skills/analyze-viral-reel/), [`analyze-viral-youtube-video`](skills/analyze-viral-youtube-video/) | Why one video took off. |
| [`find-content-ideas`](skills/find-content-ideas/) | Post ideas from real demand, across channels. |
| [`find-youtube-video-ideas`](skills/find-youtube-video-ideas/) | YouTube videos worth making. |
| [`find-linkedin-post-formats`](skills/find-linkedin-post-formats/) | The LinkedIn formats that win in your niche. |
| [`create-content-calendar`](skills/create-content-calendar/) | What to post, where and when. |
| [`repurpose-content`](skills/repurpose-content/) | One piece of content turned into posts for each channel. |

### Communities

| Skill | What it does |
|-------|--------------|
| [`create-reddit-plan`](skills/create-reddit-plan/) | A plan to show up on Reddit without getting banned. |
| [`find-subreddits`](skills/find-subreddits/) | The subreddits your buyers use, and their rules. |
| [`find-reddit-threads`](skills/find-reddit-threads/) | Reddit threads worth a reply. |
| [`find-linkedin-posts-to-comment`](skills/find-linkedin-posts-to-comment/) | LinkedIn posts worth a comment today. |
| [`find-linkedin-buyer-posts`](skills/find-linkedin-buyer-posts/) | People on LinkedIn posting about the problem you solve. |
| [`find-linkedin-topic-leaders`](skills/find-linkedin-topic-leaders/) | Who leads the conversation on your topic. |

### Influencers

| Skill | What it does |
|-------|--------------|
| [`create-influencer-plan`](skills/create-influencer-plan/) | An influencer plan from what competitors run and what works in your niche. |
| [`find-creators`](skills/find-creators/) | Creators across TikTok, Instagram and YouTube, in one table. |
| [`find-tiktok-creators`](skills/find-tiktok-creators/), [`find-instagram-creators`](skills/find-instagram-creators/), [`find-youtube-creators`](skills/find-youtube-creators/) | Creators on one platform. |
| [`find-ugc-creators`](skills/find-ugc-creators/) | Creators who make review and unboxing videos for ads. |
| [`find-brand-fans`](skills/find-brand-fans/) | Creators already posting about you. |
| [`vet-creator`](skills/vet-creator/) | Whether one creator is worth the fee, and where their audience lives. |
| [`write-creator-brief`](skills/write-creator-brief/) | The campaign brief a creator works from. |

### Paid ads

| Skill | What it does |
|-------|--------------|
| [`create-paid-ads-plan`](skills/create-paid-ads-plan/) | Which platforms to run ads on, and with what budget. |
| [`research-meta-ads`](skills/research-meta-ads/), [`research-google-ads`](skills/research-google-ads/), [`research-tiktok-ads`](skills/research-tiktok-ads/), [`research-linkedin-ads`](skills/research-linkedin-ads/) | One competitor's ads, or the best ads across your category, on one platform. |
| [`write-ad-brief`](skills/write-ad-brief/) | A brief for new ad creative. |
| [`find-google-ads-keywords`](skills/find-google-ads-keywords/) | The search keywords worth bidding on. |
| [`check-google-brand-bidding`](skills/check-google-brand-bidding/) | Who bids on your brand name. |

### Outbound

| Skill | What it does |
|-------|--------------|
| [`create-outbound-plan`](skills/create-outbound-plan/) | An outbound plan from your ICP, the market and your send capacity. |
| [`size-market`](skills/size-market/) | How many companies and buyers fit your ICP. |
| [`find-lookalike-companies`](skills/find-lookalike-companies/) | Companies like your best customers. |
| [`build-lead-list`](skills/build-lead-list/) | A list of buyers with verified emails. |
| [`find-buying-signals`](skills/find-buying-signals/) | Companies showing a reason to buy now. |
| [`track-job-changes`](skills/track-job-changes/) | Past champions who moved to a new company. |
| [`research-account`](skills/research-account/) | One account, researched before a call. |
| [`write-first-lines`](skills/write-first-lines/) | A personal first line for each lead. |
| [`clean-email-list`](skills/clean-email-list/) | Verify and clean a list you already have. |
| [`enrich-lead-list`](skills/enrich-lead-list/) | Fill in the missing fields of a list. |

### Customer research

| Skill | What it does |
|-------|--------------|
| [`find-pain-points`](skills/find-pain-points/) | What buyers complain about, across every source. |
| [`find-reddit-pain-points`](skills/find-reddit-pain-points/) | Pain points from Reddit threads. |
| [`mine-facebook-groups`](skills/mine-facebook-groups/) | Questions and complaints in Facebook groups. |
| [`mine-tiktok-comments`](skills/mine-tiktok-comments/), [`mine-instagram-comments`](skills/mine-instagram-comments/), [`mine-youtube-comments`](skills/mine-youtube-comments/), [`mine-facebook-comments`](skills/mine-facebook-comments/) | What people say under posts on one platform. |
| [`build-personas`](skills/build-personas/) | Personas built from evidence. |
| [`find-objections`](skills/find-objections/) | The doubts that stop people buying. |
| [`find-competitor-complaints`](skills/find-competitor-complaints/) | What competitors' customers complain about, and why they switch. |
| [`check-demand`](skills/check-demand/) | Whether people want the thing, before you build or launch it. |
| [`map-market`](skills/map-market/) | The players in a market and where each one wins. |

### Competitor research

| Skill | What it does |
|-------|--------------|
| [`create-competitor-plan`](skills/create-competitor-plan/) | A plan to win against your competitors. |
| [`find-competitors`](skills/find-competitors/) | Who your real competitors are. |
| [`tear-down-competitor`](skills/tear-down-competitor/) | One competitor across search, ads, social and AI answers. |
| [`compare-messaging`](skills/compare-messaging/) | How competitors describe and price themselves. |
| [`find-positioning`](skills/find-positioning/) | Where you can stand apart. |
| [`write-battlecard`](skills/write-battlecard/) | A sales battlecard for one competitor. |

### Growth and launch

| Skill | What it does |
|-------|--------------|
| [`create-growth-plan`](skills/create-growth-plan/) | A growth plan for your goal (traffic, signups, leads, awareness, first customers). |
| [`pick-channels`](skills/pick-channels/) | Which channels to be on at all. |
| [`find-first-customers`](skills/find-first-customers/) | Where to find your first 100 customers. |
| [`create-gtm-plan`](skills/create-gtm-plan/) | A go-to-market plan. |
| [`create-market-entry-plan`](skills/create-market-entry-plan/) | Whether and how to enter a new country or segment. |
| [`measure-brand-awareness`](skills/measure-brand-awareness/) | Your awareness against competitors, and what moves it. |
| [`create-launch-plan`](skills/create-launch-plan/) | A launch plan, communities, creators, press, and a report on the reaction. |

### Monitoring

| Skill | What it does |
|-------|--------------|
| [`monitor-brand-mentions`](skills/monitor-brand-mentions/) | New mentions of your brand, on your agent's schedule. |
| [`monitor-competitors`](skills/monitor-competitors/) | What competitors changed this week. |
| [`track-rankings`](skills/track-rankings/) | Where you rank, once or on a schedule. |
| [`monitor-search-console`](skills/monitor-search-console/) | Drops in your own Search Console clicks and impressions. |
| [`write-weekly-report`](skills/write-weekly-report/) | One weekly report across search, AI answers, social and mentions. |

### Agencies

| Skill | What it does |
|-------|--------------|
| [`prepare-client-pitch`](skills/prepare-client-pitch/) | A prospect audit and the data for a pitch. |
| [`onboard-client`](skills/onboard-client/) | A new client's baseline and first plan. |
| [`write-client-report`](skills/write-client-report/) | The monthly client report. |

## Credits

Every tool call costs Manifold credits (100 credits are one US dollar), and every skill says what a run costs before the first paid call. Give a budget and the agent caps every call with it. Prices are at https://www.manifoldmcp.com/pricing.

## What the plugin runs and sends

The plugin is Markdown skills, two manifests and one connector entry. It runs no code on your machine, installs no packages and has no hooks.

- **Where calls go.** The skills tell your agent which tools to call on the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`, the only server the plugin adds. You sign in to it with your Manifold account; the plugin holds no credentials.
- **What a call sends.** Only the parameters the call needs: domains, URLs, keywords, prompts to ask AI engines, social handles and post URLs, subreddit names, company and person filters, the names and domains used to look up a work email, and email addresses to verify.
- **Where the server gets its data.** DataForSEO for search, keyword, backlink, site audit and AI answer data; ScrapeCreators for public Reddit, social platform and ad library data; Apollo, Hunter, Findymail and Icypeas for company data, professional contacts and email verification. Two tools fetch the site you name directly. These providers receive the parameters of a call, never your name, email address or account.
- **What the server keeps.** Your credit balance, the charge for each call, task results for 30 days and cached results. The full policy is at https://www.manifoldmcp.com/privacy.
- **What it never does.** Send email, post, comment, message, buy, or change anything on another service. Every skill ends in a table for you to act on.

## Contributing

A release step builds this repository from Manifold's private repository, so a pull request here would be overwritten. To report a problem, improve a skill or suggest a new one, [open an issue](https://github.com/manifoldmcp/marketing-skills/issues).

## License

MIT. See [LICENSE](LICENSE).
