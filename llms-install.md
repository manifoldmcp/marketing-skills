# Install Manifold in Cline

Manifold is the free marketing data connector for AI agents. Add its MCP server to Cline and one sign-in gives Cline live data from Google search, AI answers, Search Console, Google Analytics, Google Maps, Reddit, TikTok, Instagram, YouTube, Facebook, LinkedIn, the ad libraries, the app stores, Amazon, Google Shopping, and companies and people, without an account with each platform. The marketing skills in this repository run on it.

## Server configuration

Add this server to your MCP settings:

```json
{
  "mcpServers": {
    "manifold": {
      "type": "streamableHttp",
      "url": "https://mcp.manifoldmcp.com/mcp"
    }
  }
}
```

## Authentication

The server requires authentication. Choose one method:

### OAuth (recommended)

Sign in with your Manifold account through OAuth. The MCP client opens the authorization flow when it first connects.

### API key fallback

If OAuth is unavailable, use an API key from your Manifold account settings:

1. Sign in to your Manifold account
2. Generate an API key from account settings
3. Add the key to your environment:

```bash
export MANIFOLD_API_KEY="mk_live_..."
```

Then configure the server with the API key in the authorization header:

```json
{
  "mcpServers": {
    "manifold": {
      "type": "streamableHttp",
      "url": "https://mcp.manifoldmcp.com/mcp",
      "headers": {
        "Authorization": "Bearer ${MANIFOLD_API_KEY}"
      }
    }
  }
}
```

## Usage

Every workspace gets free credits each month, with no card. Tool calls cost Manifold credits (1 to 2.5 US cents a credit, by plan), while the Search Console, Bing Webmaster Tools and Google Analytics tools cost nothing. Every tool states its price before it runs, and the marketing skills in this repository show the cost before making paid calls. Install the skills with:

```bash
npx skills add manifoldmcp/marketing-skills
```

Start with `/manifold-get-started`: it asks about your goal and picks the skills to run. Run any skill by its name, such as `/seo` or `/creators`, or let it load automatically when a request matches. If another plugin has a skill with the same name, use `/manifold:seo`.

## What the server provides

Marketing data tools that look things up:

- Google search: keywords, rankings, results pages, backlinks, competitors, traffic estimates and a technical crawl
- AI answers: what ChatGPT, Claude, Gemini, Perplexity and Google's AI answers say and cite, and whether a site is ready for them
- Your own Google Search Console, Bing Webmaster Tools and Google Analytics data, once connected, and sitemap and URL submission
- Google Maps: local rankings, lists of businesses and Business Profiles
- Reddit, TikTok, Instagram, YouTube, Facebook and LinkedIn: accounts, posts, comments and transcripts
- Ad libraries: the ads running on Meta, Google, TikTok and LinkedIn
- Stores: Google Play and App Store listings, Amazon and Google Shopping products and prices
- Companies and people: company search, people search, and verified work emails

Nothing is posted, emailed or bought for you: the one change it makes elsewhere is a sitemap or URL submission to your own Search Console or Bing, when you ask. Every skill ends with a table you act on.
