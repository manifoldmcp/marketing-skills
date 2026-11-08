# Reddit by Manifold

Let your agent research Reddit for you, with no Reddit API app and no scraper to run. It searches posts and comments across Reddit, finds the subreddits and threads where people ask for what you sell, and ranks their pain points, objections and wishes in their own words. Before you post, it explains each subreddit's rules on promotion, karma and AI-written text, so your account does not get banned.

Use it for customer research, positioning and messaging, finding threads worth answering, and spotting what people hate about a competitor.

## Ask it like this

- "What do people on Reddit hate about project management tools?"
- "Find recent threads where someone asks for a CRM for a small agency."
- "Which subreddits should we be in for home espresso, and what do they allow?"
- "Turn the top Reddit complaints about our category into three personas."
- "What words do buyers use when they describe this problem?"

## Install

In Claude, open Customize, then Plugins, find Reddit by Manifold and install it. In Claude Code:

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold-reddit@manifold
```

Then sign in to Manifold: on the plugin's Connectors tab in Claude, or with `/mcp` in Claude Code. Signing in creates a workspace with free credits that come back every month.

## What is inside

- `reddit`: marketing on Reddit, finding subreddits and threads, and what each community allows now.
- `voice-of-customer`: turning posts, comments and reviews into ranked pain points, objections, personas and positioning.
- `manifold-get-started`: writes down what you sell and to whom, so every answer fits your business.

The plugin connects the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`. If you also have the main `manifold` plugin, Claude uses one connection for both. Manifold looks things up; it never posts or comments for you.

More marketing skills are in the main [Manifold plugin](https://github.com/manifoldmcp/marketing-skills). Questions and bugs go to [issues](https://github.com/manifoldmcp/marketing-skills/issues).
