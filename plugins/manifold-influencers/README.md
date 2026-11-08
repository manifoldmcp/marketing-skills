# Influencers by Manifold

Find the right influencers and creators on TikTok, Instagram and YouTube without an influencer platform subscription. Ask your agent for creators in your niche and it searches posts, reels and videos, reads profiles and comments, checks engagement against follower count, flags fake followers and bought engagement, suggests a fair rate and writes the outreach brief.

It also works for UGC creators, affiliates and fans who already post about you, and for research on what the top accounts in a niche post, which hooks and formats are working and what is trending on TikTok now.

## Ask it like this

- "Find TikTok creators in home fitness with 10k to 100k followers who post every week."
- "Vet these five Instagram accounts. Are their followers real?"
- "What should we pay a YouTube creator with 40k subscribers for one integration?"
- "Write a brief for a UGC creator making three Spark Ads for our skincare launch."
- "What are the top reels in the specialty coffee niche doing this month?"

## Install

In Claude, open Customize, then Plugins, find Influencers by Manifold and install it. In Claude Code:

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold-influencers@manifold
```

Then sign in to Manifold: on the plugin's Connectors tab in Claude, or with `/mcp` in Claude Code. Signing in creates a workspace with free credits that come back every month.

## What is inside

- `creators`: finding, vetting, pricing and briefing influencers, UGC creators and affiliates, with the disclosure rules and usage rights for paid ads.
- `instagram`, `tiktok` and `youtube`: what each platform rewards now and how to read its data.
- `manifold-get-started`: writes down what you sell and to whom, so every answer fits your business.

The plugin connects the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`. If you also have the main `manifold` plugin, Claude uses one connection for both. Manifold looks things up; it never messages a creator or posts for you.

More marketing skills are in the main [Manifold plugin](https://github.com/manifoldmcp/marketing-skills). Questions and bugs go to [issues](https://github.com/manifoldmcp/marketing-skills/issues).
