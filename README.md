# Manifold skills

Marketing skills for AI agents, built on [Manifold](https://www.manifoldmcp.com): one hosted MCP server of marketing data tools (SEO, AI answer visibility, leads, Reddit, six social platforms and the ad libraries), sold on prepaid credits.

Each skill does one marketing job and is named for it, like `audit-technical-seo`, `find-tiktok-creators` or `find-pain-points`. A skill checks the Manifold connector is there, reads your product context, then names the exact tools to call, what each call costs, the judgment calls, and the table it hands back. Where a job differs by platform, its `references/` folder holds the detail for each one. Nothing here sends, posts or schedules: the result is yours to act on.

This repository is generated from Manifold's private repository by a release step, so a change made here is overwritten. To report a problem or suggest a skill, open an issue.

## Install

**Claude Code** (skills and the connector in one plugin):

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold@manifold
```

Then run `/mcp` to sign in to Manifold, and `/manifold-get-started` to pick the skills for your goal. Run any skill by its name, such as `/audit-technical-seo` or `/find-tiktok-creators`, or let it load by itself when a request matches. If another plugin has a skill with the same name, add the plugin name: `/manifold:audit-technical-seo`.

**Codex** (skills and the connector in one plugin):

```sh
codex plugin marketplace add manifoldmcp/marketing-skills
codex plugin add manifold@manifold
codex mcp login manifold
```

The last command signs you in to Manifold. Then ask "Where should I start with Manifold?" to pick the skills for your goal. Run `codex plugin marketplace upgrade manifold` to take a new release.

**Gemini CLI** (skills and the connector in one extension):

```sh
gemini extensions install https://github.com/manifoldmcp/marketing-skills
```

Then run `/mcp auth manifold` in Gemini CLI to sign in to Manifold. Run `gemini extensions update manifold` to take a new release.

**Cline**:

See [llms-install.md](llms-install.md) for MCP server configuration and authentication (OAuth or API key).

**Other agents** (Cursor and the rest that read skills):

```sh
npx skills add manifoldmcp/marketing-skills
```

These hosts get the skills without the connector. Add the Manifold MCP server yourself: its URL is `https://mcp.manifoldmcp.com/mcp`, and the steps for each client are at https://www.manifoldmcp.com/docs/clients. Every skill checks for the connector first and says so if it is missing.

## The skills

Every skill does one job and its name starts with what it does: `create-` a plan, `find-`, `audit-`, `check-`, `mine-`, `write-`, `monitor-`. The skills are flat: install them all, or only the ones you need. The groups below are for reading only.

**Start here**

- `manifold-get-started`: asks about your goal, stage and budget, and picks three to five skills to run in order. It runs no tools and costs nothing.
- `create-product-context`: write down once what every skill needs to know (product, ideal customer, competitors, positioning, customer language, goals) in `.agents/product-marketing.md`. Every other skill reads it first. The file uses the same path and sections as other marketing skill libraries, so one file serves them all.

**SEO**

- `create-seo-plan`: a 90-day SEO plan from where the site stands, its competitors and the keyword gaps.
- `audit-technical-seo`: what stops Google crawling and indexing the pages that matter.
- `diagnose-traffic-drop`: why organic traffic fell, and what to fix first.
- `create-migration-plan`: move domains, platforms or URLs without losing rankings.
- `find-seo-quick-wins`: pages near page one that a small change moves up.
- `refresh-content`: pages that decayed or sit just off page one, and what to update.
- `create-seo-content-plan`: topic clusters and an order to publish them in.
- `write-seo-brief`: a brief for one page that can win its keyword.
- `optimize-page`: what one page is missing against the pages that beat it.
- `plan-comparison-pages`: "X vs Y" and alternatives pages worth writing.
- `fix-keyword-cannibalization`: pages of yours that compete for one query, and which should win.

**AI search**

- `create-ai-search-plan`: a plan to get named and cited by ChatGPT, Claude, Gemini, Perplexity and Google's AI answers.
- `check-ai-visibility`: which brands AI answers name and cite for your buyers' questions, once or on a schedule.
- `build-ai-citations`: the sources AI engines cite for competitors and not for you, Reddit threads and YouTube videos included.
- `check-ai-overviews`: which of your keywords show a Google AI Overview, and whether it cites you.
- `fix-wrong-ai-answers`: find and fix what AI answers get wrong about you.
- `check-ai-crawler-access`: whether AI crawlers can reach and read your site.
- `find-best-of-lists`: the best-of lists that rank on Google and that AI engines cite, and how to get on them.

**Links and PR**

- `create-link-building-plan`: a link building plan from your links, your competitors' and the gaps.
- `find-backlink-targets`: sites worth a backlink, with a verified contact at each.
- `create-digital-pr-plan`: the stories that earn coverage in your space, in a 90-day plan.
- `find-journalists`: the journalists who cover your story, with a contact for each.
- `find-podcasts`: podcasts to guest on, with a contact for each.
- `find-unlinked-mentions`: pages that name you without a link.
- `reclaim-lost-links`: links you lost, and which are worth winning back.
- `find-affiliate-partners`: sites that already promote products like yours.

**Social media**

- `create-tiktok-plan`, `create-instagram-plan`, `create-youtube-plan`, `create-linkedin-plan`, `create-facebook-plan`: a 90-day plan for one platform.
- `audit-tiktok-account`, `audit-instagram-account`, `audit-youtube-channel`, `audit-facebook-page`, `audit-linkedin-page`, `audit-x-account`: your own or a competitor's account.
- `find-tiktok-hooks`, `find-instagram-hooks`: the opening lines that hold viewers in your niche.
- `find-tiktok-trends`, `find-reels-trends`: the trends your niche can use now.
- `analyze-viral-tiktok`, `analyze-viral-reel`, `analyze-viral-youtube-video`: why one video took off.
- `find-content-ideas`: post ideas from real demand, across channels.
- `find-youtube-video-ideas`: YouTube videos worth making.
- `find-linkedin-post-formats`: the LinkedIn formats that win in your niche.
- `create-content-calendar`: what to post, where and when.
- `repurpose-content`: one piece of content turned into posts for each channel.

**Communities**

- `create-reddit-plan`: a plan to show up on Reddit without getting banned.
- `find-subreddits`: the subreddits your buyers use, and their rules.
- `find-reddit-threads`: Reddit threads worth a reply.
- `find-linkedin-posts-to-comment`: LinkedIn posts worth a comment today.
- `find-linkedin-buyer-posts`: people on LinkedIn posting about the problem you solve.
- `find-linkedin-topic-leaders`: who leads the conversation on your topic.

**Influencers**

- `create-influencer-plan`: an influencer plan from what competitors run and what works in your niche.
- `find-creators`: creators across TikTok, Instagram and YouTube, in one table.
- `find-tiktok-creators`, `find-instagram-creators`, `find-youtube-creators`: creators on one platform.
- `find-ugc-creators`: creators who make review and unboxing videos for ads.
- `find-brand-fans`: creators already posting about you.
- `vet-creator`: whether one creator is worth the fee, and where their audience lives.
- `write-creator-brief`: the campaign brief a creator works from.

**Paid ads**

- `create-paid-ads-plan`: which platforms to run ads on, and with what budget.
- `research-meta-ads`, `research-google-ads`, `research-tiktok-ads`, `research-linkedin-ads`: one competitor's ads, or the best ads across your category, on one platform.
- `write-ad-brief`: a brief for new ad creative.
- `find-google-ads-keywords`: the search keywords worth bidding on.
- `check-google-brand-bidding`: who bids on your brand name.

**Outbound**

- `create-outbound-plan`: an outbound plan from your ICP, the market and your send capacity.
- `size-market`: how many companies and buyers fit your ICP.
- `find-lookalike-companies`: companies like your best customers.
- `build-lead-list`: a list of buyers with verified emails.
- `find-buying-signals`: companies showing a reason to buy now.
- `track-job-changes`: past champions who moved to a new company.
- `research-account`: one account, researched before a call.
- `write-first-lines`: a personal first line for each lead.
- `clean-email-list`: verify and clean a list you already have.
- `enrich-lead-list`: fill in the missing fields of a list.

**Customer research**

- `find-pain-points`: what buyers complain about, across every source.
- `find-reddit-pain-points`: pain points from Reddit threads.
- `mine-facebook-groups`: questions and complaints in Facebook groups.
- `mine-tiktok-comments`, `mine-instagram-comments`, `mine-youtube-comments`, `mine-facebook-comments`: what people say under posts on one platform.
- `build-personas`: personas built from evidence.
- `find-objections`: the doubts that stop people buying.
- `find-competitor-complaints`: what competitors' customers complain about, and why they switch.
- `check-demand`: whether people want the thing, before you build or launch it.
- `map-market`: the players in a market and where each one wins.

**Competitor research**

- `create-competitor-plan`: a plan to win against your competitors.
- `find-competitors`: who your real competitors are.
- `tear-down-competitor`: one competitor across search, ads, social and AI answers.
- `compare-messaging`: how competitors describe and price themselves.
- `find-positioning`: where you can stand apart.
- `write-battlecard`: a sales battlecard for one competitor.

**Growth and launch**

- `create-growth-plan`: a growth plan for your goal (traffic, signups, leads, awareness, first customers).
- `pick-channels`: which channels to be on at all.
- `find-first-customers`: where to find your first 100 customers.
- `create-gtm-plan`: a go-to-market plan.
- `create-market-entry-plan`: whether and how to enter a new country or segment.
- `measure-brand-awareness`: your awareness against competitors, and what moves it.
- `create-launch-plan`: a launch plan, communities, creators, press, and a report on the reaction.

**Monitoring**

- `monitor-brand-mentions`: new mentions of your brand, on your agent's schedule.
- `monitor-competitors`: what competitors changed this week.
- `track-rankings`: where you rank, once or on a schedule.
- `monitor-search-console`: drops in your own Search Console clicks and impressions.
- `write-weekly-report`: one weekly report across search, AI answers, social and mentions.

**Agencies**

- `prepare-client-pitch`: a prospect audit and the data for a pitch.
- `onboard-client`: a new client's baseline and first plan.
- `write-client-report`: the monthly client report.

## Credits

Every tool call costs Manifold credits (100 credits are one US dollar) and every skill says what a run costs before the first paid call. Give a budget and the agent caps every call with it.

## What the plugin runs and sends

The plugin is Markdown skills, two manifests and one connector entry. It runs no code on your machine, installs no packages and has no hooks.

- **Where calls go.** The skills tell your agent which tools to call on the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`, the only server the plugin adds. You sign in to it with your Manifold account; the plugin holds no credentials.
- **What a call sends.** Only the parameters the call needs: domains, URLs, keywords, prompts to ask AI engines, social handles and post URLs, subreddit names, company and person filters, the names and domains used to look up a work email, and email addresses to verify.
- **Where the server gets its data.** DataForSEO for search, keyword, backlink, site audit and AI answer data; ScrapeCreators for public Reddit, social platform and ad library data; Apollo, Hunter, Findymail and Icypeas for company data, professional contacts and email verification. Two tools fetch the site you name directly. These providers receive the parameters of a call, never your name, email address or account.
- **What the server keeps.** Your credit balance, the charge for each call, task results for 30 days and cached results. The full policy is at https://www.manifoldmcp.com/privacy.
- **What it never does.** Send email, post, comment, message, buy, or change anything on another service. Every skill ends in a table for you to act on.

## License

MIT. See [LICENSE](LICENSE).
