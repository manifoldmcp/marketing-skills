# AI Search by Manifold

Find out if AI answers recommend you. Your agent asks ChatGPT, Claude, Gemini, Perplexity and Google AI Overviews the questions your buyers ask, records which brands get named and which pages get cited, and compares you with your competitors. It checks your site for blocked AI crawlers, finds the sources the engines trust in your category, and plans what to change so you get cited. People also call this GEO, AEO, LLM SEO or AI visibility.

It also tracks how often your brand is mentioned across the web and in search against your competitors, so you can see whether the work is paying off.

## Ask it like this

- "Does ChatGPT recommend us when people ask for a CRM for startups?"
- "Which pages do Perplexity and AI Overviews cite for our category?"
- "Can AI crawlers read our site, or are we blocking them?"
- "Make a 90-day plan to get named in AI answers."
- "How does our share of voice compare with our top three competitors?"

## Install

In Claude, open Customize, then Plugins, find AI Search by Manifold and install it. In Claude Code:

```sh
/plugin marketplace add manifoldmcp/marketing-skills
/plugin install manifold-ai-search@manifold
```

Then sign in to Manifold: on the plugin's Connectors tab in Claude, or with `/mcp` in Claude Code. Signing in creates a workspace with free credits that come back every month.

## What is inside

- `ai-search`: how each AI engine picks its sources, what gets a brand cited, and how to measure it with live answers.
- `brand-monitoring`: brand mentions, alerts and share of voice against competitors.
- `manifold-get-started`: writes down what you sell and to whom, so every answer fits your business.

The plugin connects the Manifold MCP server at `https://mcp.manifoldmcp.com/mcp`. If you also have the main `manifold` plugin, Claude uses one connection for both. Manifold looks things up; it never changes your site.

More marketing skills are in the main [Manifold plugin](https://github.com/manifoldmcp/marketing-skills). Questions and bugs go to [issues](https://github.com/manifoldmcp/marketing-skills/issues).
