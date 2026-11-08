# SEO by Manifold

Do SEO with your agent using live Google data, without an SEO subscription. It looks up keyword volumes and difficulty, who ranks on page one, backlinks and referring domains, and traffic estimates for any site. It crawls your site for broken links, redirects and indexing problems, and reads your own Google Search Console and Google Analytics. Then it finds the pages one push from page one and tells you what to fix and what to write next.

It covers local SEO too: who shows in the Google Maps pack for your town, what their Business Profiles do better and how to catch up.

## Ask it like this

- "Find keywords for a page about project management software for agencies."
- "Which of our pages rank 4 to 15 and what would move them up?"
- "Why did our organic traffic drop last month?"
- "Run a technical SEO audit of our site and rank the fixes."
- "Why does the dentist across town beat us in the map pack?"

## Install

In Claude, open Customize, then Plugins, find SEO by Manifold and install it. In Claude Code:

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold-seo@manifold
```

Then sign in to Manifold: on the plugin's Connectors tab in Claude, or with `/mcp` in Claude Code. Connect Search Console and Google Analytics from the Manifold app when you want your own numbers. Signing in creates a workspace with free credits that come back every month.

## What is inside

- `seo`: keyword research, content plans, quick wins and what the latest Google updates rewarded.
- `technical-seo`: crawling, indexing, traffic drops and site moves.
- `local-seo`: Google Maps, the local pack and Business Profiles.
- `manifold-get-started`: writes down what you sell and to whom, so every answer fits your business.

The plugin connects the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`. If you also have the main `manifold` plugin, Claude uses one connection for both. Manifold looks things up; it never edits your site.

More marketing skills are in the main [Manifold plugin](https://github.com/manifoldmcp/marketing-skills). Questions and bugs go to [issues](https://github.com/manifoldmcp/marketing-skills/issues).
