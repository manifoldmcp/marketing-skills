# Install Manifold in Cline

Configure the Manifold MCP server in Cline to access marketing data tools (SEO, AI search visibility, leads, Reddit, social platforms and ad libraries) through the marketing skills.

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

Every tool call costs Manifold credits (100 credits equal one US dollar). The marketing skills in this repository show the cost before making paid calls. Install the skills with:

```bash
npx skills add manifoldmcp/marketing-skills
```

Start with `/manifold-get-started`: it asks about your goal and picks the skills to run. Run any skill by its name, such as `/audit-technical-seo` or `/find-tiktok-creators`, or let it load automatically when a request matches. If another plugin has a skill with the same name, use `/manifold:audit-technical-seo`.

## What the server provides

Read-only marketing data tools:

- Search and keyword data (rankings, traffic estimates, SERP features)
- AI answer visibility (ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews)
- Link building (backlinks, referring domains, domain ratings)
- Leads (company search, people search, email finding and verification)
- Social platforms (Reddit, TikTok, Instagram, YouTube, LinkedIn, Facebook)
- Ad libraries (Meta, TikTok, LinkedIn, Google ads)

Nothing is sent, posted or bought for you. Every skill ends with a table you act on.
