# Manifold skills

Marketing playbooks for AI agents, built on [Manifold](https://www.manifoldmcp.com): one MCP server of read-only marketing data tools (SEO, AI answer visibility, leads, Reddit, six social platforms and the ad libraries), sold on prepaid credits.

Each skill is a group of playbooks for one kind of marketing job. Its `SKILL.md` is a router: it checks that the Manifold connector is there, picks the playbook for the request, and holds the rules the group shares. Each playbook in `references/` names the exact tools to call, what each call costs, the judgment calls, and the table it hands back. Nothing here sends, posts or schedules: the result is yours to act on.

This repository is generated from Manifold's private repository by a release step, so a change made here is overwritten. To report a problem or suggest a playbook, open an issue.

## Install

**Claude Code** (skills and the connector in one plugin):

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold@manifold
```

Then run `/mcp` to sign in to Manifold. The skills run as `/manifold:seo`, `/manifold:ai-search` and so on, or load by themselves when a request matches.

**Codex** (skills and the connector in one plugin):

```sh
codex plugin marketplace add manifoldmcp/marketing-skills
codex plugin add manifold@manifold
codex mcp login manifold
```

The last command signs you in to Manifold. Run `codex plugin marketplace upgrade manifold` to take a new release.

**Cline**:

See [llms-install.md](llms-install.md) for MCP server configuration and authentication (OAuth or API key).

**Other agents** (Cursor, Gemini CLI and the rest that read skills):

```sh
npx skills add manifoldmcp/marketing-skills
```

These hosts get the skills without the connector. Add the Manifold MCP server yourself: its URL is `https://mcp.manifoldmcp.com/mcp`, and the steps for each client are at https://www.manifoldmcp.com/docs/clients. Every skill checks for the connector first and says so if it is missing.

## The skills

**Start here**

- `growth-plan`: a growth plan for your goal (traffic, signups, leads, awareness, first customers), the first 100 customers, go-to-market, a new market, brand awareness.
- `launch`: a launch plan, the communities to post in, creators, press, and a report on the reaction.

**Channels**

- `seo`: strategy, quick wins, traffic drops, content refresh, audits, content plans and briefs, page optimization, comparison pages, migrations, rank checks.
- `ai-search`: GEO and AEO. What ChatGPT, Claude, Gemini, Perplexity and Google's AI answers say for your buyers' questions, citation building, best-of lists AI engines cite, wrong AI answers, AI crawler readiness.
- `link-building`: backlink targets with verified contacts, best-of lists that rank, journalists, lost links, affiliate partners, link building and PR strategy.
- `paid-ads`: competitor ads from the Meta, TikTok, LinkedIn and Google ad libraries, swipe files, creative briefs, PPC keywords, paid strategy.
- `influencers`: creators across TikTok, Instagram and YouTube, vetting, UGC creators, campaign briefs.
- `leads`: market size, lookalike and in-market companies, lead lists with verified emails, account briefs, first lines, list cleaning and enrichment.

**Platforms**

- `reddit`: subreddits, their rules, threads to reply to, pain points, competitor complaints, threads AI engines cite.
- `tiktok`: trends, hooks, viral breakdowns, competitor accounts, creators and their audiences, comment mining.
- `instagram`: Reels trends, hooks, competitor accounts, creators, comment mining.
- `youtube`: video ideas, competitor channels, creators, podcasts to guest on, comment mining, videos AI engines cite.
- `linkedin`: founder and company strategy, topic leaders, post formats, posts to comment on, company page audits, people posting about your problem.
- `facebook`: page audits, group mining, video transcripts, comment mining.

**Research and ongoing**

- `competitors`: teardowns, who your real competitors are, messaging and pricing, positioning, competitive strategy.
- `customers`: pain points across sources, personas, demand checks, market maps.
- `content`: which channels to use, content ideas, calendars, repurposing, X account audits.
- `monitoring`: brand mentions, competitor watch, rank tracking, AI visibility tracking, weekly reports, run on your agent's schedule.
- `agency`: prospect audits, pitch data, client onboarding, monthly client reports.

## Credits

Every tool call costs Manifold credits (100 credits are one US dollar) and every playbook says what a run costs before the first paid call. Give a budget and the agent caps every call with it.

## What the plugin runs and sends

The plugin is Markdown playbooks, two manifests and one connector entry. It runs no code on your machine, installs no packages and has no hooks.

- **Where calls go.** The skills tell your agent which tools to call on the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`, the only server the plugin adds. You sign in to it with your Manifold account; the plugin holds no credentials.
- **What a call sends.** Only the parameters the call needs: domains, URLs, keywords, prompts to ask AI engines, social handles and post URLs, subreddit names, company and person filters, the names and domains used to look up a work email, and email addresses to verify.
- **Where the server gets its data.** DataForSEO for search, keyword, backlink, site audit and AI answer data; ScrapeCreators for public Reddit, social platform and ad library data; Apollo, Hunter, Findymail and Icypeas for company data, professional contacts and email verification. Two tools fetch the site you name directly. These providers receive the parameters of a call, never your name, email address or account.
- **What the server keeps.** Your credit balance, the charge for each call, task results for 30 days and cached results. The full policy is at https://www.manifoldmcp.com/privacy.
- **What it never does.** Send email, post, comment, message, buy, or change anything on another service. Every playbook ends in a table for you to act on.

## License

MIT. See [LICENSE](LICENSE).
