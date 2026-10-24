# Manifold: the free marketing data MCP server for AI agents

**Free every month.** One sign-in gives Claude, ChatGPT or any MCP client live data from Google search, AI answers, Search Console, Google Analytics, Google Maps, Reddit, TikTok, Instagram, YouTube, Facebook, LinkedIn, the ad libraries, the app stores, Amazon, Google Shopping, and companies and people. There is no account to open with each platform: no Reddit API app, no LinkedIn login, no SEO subscription.

```text
https://mcp.manifoldmcp.com/mcp
```

A remote MCP server over Streamable HTTP, with OAuth sign-in or a Manifold API key as a bearer token. [Manifold](https://www.manifoldmcp.com) hosts the server, and it is not open source. This repository holds the plugin manifests and the marketing skills that run on it, under the MIT license. Agents can read [llms.txt](https://www.manifoldmcp.com/llms.txt) for when to use it, what is free and its limits.

## Connect

| Client | How |
|--------|-----|
| Claude | Settings, then Connectors: add a custom connector, paste the URL and sign in. |
| Claude Code | `claude mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp`, then `/mcp` to sign in. |
| ChatGPT | At chatgpt.com/plugins, click Add, then Create custom MCP server, and paste the URL. Plus, Pro, Business, Enterprise and Edu, on the web. |
| Codex | `codex mcp add manifold --url https://mcp.manifoldmcp.com/mcp`, then `codex mcp login manifold` to sign in. |
| Cursor | [Add to Cursor](https://cursor.com/install-mcp?name=manifold&config=eyJ1cmwiOiJodHRwczovL21jcC5tYW5pZm9sZG1jcC5jb20vbWNwIn0%3D), or paste the URL in the MCP section of Cursor Settings. |
| Gemini CLI | `gemini mcp add --transport http manifold https://mcp.manifoldmcp.com/mcp`, then `/mcp auth manifold` to sign in. |
| Grok | Settings, then Connectors: add a connector, paste the URL and sign in. |
| Grok Bot | Paste to the bot: `Add the Manifold MCP server as a plugin: https://mcp.manifoldmcp.com/mcp.` |
| OpenCode | Run `opencode mcp add`, choose Remote and paste the URL. |
| Any other MCP client | Add the URL as a remote MCP server. |

Signing in creates a workspace with free credits. The steps for each client are at https://www.manifoldmcp.com/docs/clients. To add the marketing skills as well, install the plugin: see [Marketing skills](#marketing-skills).

## What your agent can look up

- **[Google search](https://www.manifoldmcp.com/mcp/seo)**: keyword ideas, volumes and difficulty, rankings, results pages, backlinks, competitors, traffic estimates and a technical crawl.
- **[AI answers](https://www.manifoldmcp.com/mcp/ai-visibility)**: what ChatGPT, Claude, Gemini, Perplexity and Google's AI answers say for a prompt, which brands they name and which pages they cite, and whether a site is ready for them.
- **Search Console and Google Analytics**: your own [Google Search Console](https://www.manifoldmcp.com/mcp/search-console), [Bing Webmaster Tools](https://www.manifoldmcp.com/mcp/bing-webmaster-tools) and [Google Analytics](https://www.manifoldmcp.com/mcp/google-analytics) data once you connect them, and sitemap and URL submission.
- **[Google Maps](https://www.manifoldmcp.com/mcp/google-maps)**: who ranks for a local search, filtered lists of businesses, and full Business Profiles.
- **[Reddit](https://www.manifoldmcp.com/mcp/reddit)**: posts, comments and communities, searched across Reddit or watched in one subreddit.
- **Social**: accounts, posts, comments and transcripts on [TikTok](https://www.manifoldmcp.com/mcp/tiktok), [Instagram](https://www.manifoldmcp.com/mcp/instagram), [YouTube](https://www.manifoldmcp.com/mcp/youtube), [Facebook](https://www.manifoldmcp.com/mcp/facebook) and [LinkedIn](https://www.manifoldmcp.com/mcp/linkedin).
- **Ad libraries**: the ads running on Meta, Google, TikTok and LinkedIn, by keyword, brand or advertiser.
- **Stores**: [Google Play](https://www.manifoldmcp.com/mcp/google-play) and [App Store](https://www.manifoldmcp.com/mcp/app-store) listings, reviews and charts, and [Amazon](https://www.manifoldmcp.com/mcp/amazon) and [Google Shopping](https://www.manifoldmcp.com/mcp/google-shopping) results, products, sellers and prices.
- **[Companies and people](https://www.manifoldmcp.com/mcp/apollo)**: company and people search, and verified work emails.

Every tool, with what it costs, is at https://www.manifoldmcp.com/docs/tools.

## Free every month

- Every workspace gets free credits each month, with no card.
- The Search Console, Bing Webmaster Tools and Google Analytics tools, the on-page snapshot and the AI readiness check cost nothing on any plan.
- Every tool states its price before it runs. Your agent can check a price without paying it, and cap what a call may spend.
- After the free credits, a monthly plan buys more. Prices are at https://www.manifoldmcp.com/pricing.

## What it does not do

- It looks things up. It does not post, comment, message, send email or buy anything. The one thing it changes elsewhere is your own Search Console or Bing, when you ask it to submit a sitemap or URLs.
- It reads public data and your own Search Console, Bing and Google Analytics. It does not connect your ad accounts, CRM or inbox.
- It is hosted by Manifold and is not open source. Manifold MCP is not related to Manifold Markets.

## Marketing skills

**Marketing skills for AI agents that look at the data before they answer.** Built by [Manifold](https://www.manifoldmcp.com).

Ask an agent what works in AI search, on TikTok or in cold email, and it answers from what it learned a year or more ago. These skills give it what changed since: dated, sourced findings on each channel, what the evidence no longer supports, and Manifold's own measurements. Each skill covers one area of marketing, and calls the Manifold MCP server for live search, AI answer, social, ad library and company data when the work needs numbers.

Claude Code, Codex and Gemini CLI install the skills and the connector together. Cursor, Cline and other agents that read [Agent Skills](https://agentskills.io) get the skills and connect the server themselves. The skills are MIT-licensed; the data is free every month, with [credits](#credits) to buy for more.

Questions, bugs and skill requests go to [issues](https://github.com/manifoldmcp/marketing-skills/issues).

## Quick start

```sh
npx skills add manifoldmcp/marketing-skills
```

[npx skills](https://github.com/vercel-labs/skills) installs every skill in Claude Code, Codex, Cursor and the other agents it supports, or only the ones you name with `--skill`. It does not add the connector: add the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp` with the steps for your client at https://www.manifoldmcp.com/docs/clients. Every skill still helps without the connector, and tells you how to connect when the work needs live data.

Then run `/manifold-get-started`. It writes down what your business sells and to whom, drafting from your site so you only fill the gaps, then picks the skills for your goal in the order to use them.

In Claude Code, Codex and Gemini CLI, the plugin installs the skills and the connector together. In Claude Code:

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold@manifold
```

Then run `/mcp` to sign in to Manifold. The other agents are under [Install](#install).

## Ask it like this

You do not need to know a skill's name. Describe the job and the matching skill loads:

| You ask | The skill that runs |
|---------|---------------------|
| "Does ChatGPT recommend us when people ask for a CRM? Do listicles still work?" | `ai-search` |
| "Find TikTok creators in home fitness under 100k followers" | `creators` |
| "What is our main competitor running on Meta right now?" | `paid-ads` |
| "Heads of marketing at Series A fintechs, with verified emails" | `outbound` |
| "What do people on Reddit hate about project management tools?" | `reddit` |

Or run one directly by name, such as `/seo`. If another plugin uses the same name, add the prefix: `/manifold:seo`.

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

**Cursor and other agents.** Use `npx skills add` and add the connector yourself; see [Quick start](#quick-start).

**Cline.** See [llms-install.md](llms-install.md) for the server configuration and sign-in (OAuth or API key).

## The skills

One skill per area of marketing, named for the area. The library is flat, so install all of it or only what you need; the groups below are for browsing.

### Start here

| Skill | What it does |
|-------|--------------|
| [`manifold-get-started`](skills/manifold-get-started/) | Writes down once what every skill needs to know about your business (what you sell, who buys, competitors, positioning, customer language, goals) in `.agents/product-marketing.md`, then picks the skills for your goal in the order to use them. |

### SEO

| Skill | What it does |
|-------|--------------|
| [`seo`](skills/seo/) | Google rankings now: what the 2025-2026 updates rewarded and punished, keywords, content, briefs, quick wins, refreshes, comparison pages, rank tracking and Search Console. |
| [`technical-seo`](skills/technical-seo/) | Crawling, indexing, traffic drops and migrations, including the false drops that come from Google's own reporting changes. |

### AI search

| Skill | What it does |
|-------|--------------|
| [`ai-search`](skills/ai-search/) | What gets a brand named and cited in ChatGPT, Claude, Gemini, Perplexity and Google's AI answers now, what tested as doing nothing, and how to measure it without fooling yourself. |

### Links and PR

| Skill | What it does |
|-------|--------------|
| [`link-building`](skills/link-building/) | Which backlinks still move rankings, where to find them, unlinked mentions, lost links, and the right contact at any site. |
| [`digital-pr`](skills/digital-pr/) | What journalists and podcast hosts say they want now, the stories that earn coverage, and who to pitch. |

### Social media

| Skill | What it does |
|-------|--------------|
| [`tiktok`](skills/tiktok/) | What TikTok rewards now, account audits, hooks, trends, comments and why a video took off. |
| [`instagram`](skills/instagram/) | How Instagram ranks Reels and posts now, account audits, hooks, trends and comments. |
| [`youtube`](skills/youtube/) | What YouTube recommends now, channel audits, video ideas, comments, and YouTube as a source for AI answers. |
| [`linkedin`](skills/linkedin/) | LinkedIn's feed now for people and company pages, formats, commenting, topic leaders and buyer posts. |
| [`facebook`](skills/facebook/) | Facebook pages and groups now, page audits, comments and group mining. |
| [`x-twitter`](skills/x-twitter/) | How X ranks posts now, and account audits. |

### Shops, apps and local

| Skill | What it does |
|-------|--------------|
| [`local-seo`](skills/local-seo/) | Google Maps and Business Profiles now: what moves the local pack, reviews, rank checks across a town, local competitors and local prospect lists. |
| [`amazon`](skills/amazon/) | Amazon search and listings now: keywords and real Amazon search volume, competitor listings, pricing and the Buy Box, and what Rufus changed. |
| [`google-shopping`](skills/google-shopping/) | Google Shopping, product feeds and AI shopping answers: who wins a product search, price checks across stores, feed audits and product reviews. |
| [`app-store`](skills/app-store/) | App Store and Google Play search: store keywords, listings, competitor apps, reviews and top charts. |

### Communities

| Skill | What it does |
|-------|--------------|
| [`reddit`](skills/reddit/) | Reddit without getting banned: communities, threads, pain points, and Reddit's changing place in Google and AI answers. |

### Creators

| Skill | What it does |
|-------|--------------|
| [`creators`](skills/creators/) | Influencer, UGC and affiliate work across platforms: rates, fake-engagement signals, vetting, briefs, disclosure rules and brand fans. |

### Paid ads

| Skill | What it does |
|-------|--------------|
| [`paid-ads`](skills/paid-ads/) | Ad platforms after the 2025-2026 automation changes, competitors' ads in the ad libraries, keywords, brand bidding, ads in AI answers and creative briefs. |

### Outbound

| Skill | What it does |
|-------|--------------|
| [`outbound`](skills/outbound/) | Cold email that still lands under the 2024-2025 sender rules: lead lists, verification, buying signals, job changes, account research and first lines. |

### Customer and market research

| Skill | What it does |
|-------|--------------|
| [`voice-of-customer`](skills/voice-of-customer/) | Pain points, objections, personas and positioning from what customers actually say. |
| [`market-research`](skills/market-research/) | Market size, market maps, demand checks and entering a new market, with the limits of each signal. |

### Competitor research

| Skill | What it does |
|-------|--------------|
| [`competitor-intel`](skills/competitor-intel/) | Who the competitors are, teardowns, monitoring, messaging, complaints and battlecards, and how far to trust traffic estimates. |

### Growth and launch

| Skill | What it does |
|-------|--------------|
| [`growth-strategy`](skills/growth-strategy/) | Which channels to bet on now, growth and go-to-market plans, launches, content calendars, ideas and repurposing. |

### Monitoring

| Skill | What it does |
|-------|--------------|
| [`brand-monitoring`](skills/brand-monitoring/) | Brand mentions and awareness: share of search, what listening tools can and cannot see, and AI answers as an awareness surface. |

### Agencies

| Skill | What it does |
|-------|--------------|
| [`agency`](skills/agency/) | Client onboarding, pitches built on a prospect's real data, and client and weekly reports that hold up. |

## Credits

Every tool call costs Manifold credits (1 to 2.5 US cents a credit, by plan), and the skills tell the agent what calls cost so it can say before the first paid one. Give a budget and the agent caps every call with it. Prices are at https://www.manifoldmcp.com/pricing.

## What the plugin runs and sends

The plugin is Markdown skills, two manifests and one connector entry. It runs no code on your machine, installs no packages and has no hooks.

- **Where calls go.** The skills tell your agent which tools to call on the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`, the only server the plugin adds. You sign in to it with your Manifold account; the plugin holds no credentials.
- **What a call sends.** Only the parameters the call needs: domains, URLs, keywords, prompts to ask AI engines, social handles and post URLs, subreddit names, company and person filters, the names and domains used to look up a work email, and email addresses to verify.
- **Where the server gets its data.** DataForSEO for search, keyword, backlink, site audit and AI answer data; ScrapeCreators for public Reddit, social platform and ad library data; Icypeas, Findymail and Hunter for company data, professional contacts and email verification. Two tools fetch the site you name directly. These providers receive the parameters of a call, never your name, email address or account.
- **What the server keeps.** Your credit balance, the charge for each call, task results for 30 days and cached results. The full policy is at https://www.manifoldmcp.com/privacy.
- **What it never does.** Send email, post, comment, message, buy, or change anything on another service. The skills hand you the results to act on.

## Contributing

A release step builds this repository from Manifold's private repository, so a pull request here would be overwritten. To report a problem, improve a skill or suggest a new one, [open an issue](https://github.com/manifoldmcp/marketing-skills/issues).

## License

MIT. See [LICENSE](LICENSE).
