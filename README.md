<p align="center"><img src="assets/logo.png" width="96" alt="AppTrack Ninja"></p>

# AppTrack Ninja MCP server

App Store intelligence for AI assistants. Search and inspect any iOS app, read chart
rankings and publisher portfolios, and manage your [AppTrack Ninja](https://apptrack.ninja)
keyword tracker from Claude, Cursor or any MCP client.

This is a **hosted remote server**: there is nothing to install or run. This repository
documents how to connect to it.

| | |
|---|---|
| Endpoint | `https://api.apptrack.ninja/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth sign-in, or `Authorization: Bearer <key>` |
| Plans | Free: one key, 25 calls a day, 10 a minute, 10 results per list · Pro: three keys, 600 reads and 30 searches a minute, full result pages |

## Get a key

Clients with OAuth (Claude, Claude Code, VS Code, Cursor, Windsurf, Codex, Gemini CLI) open the
AppTrack Ninja sign-in on first use; no key needed. For other clients:

1. Sign in at [apptrack.ninja](https://apptrack.ninja) (free plan works; Pro lifts the limits).
2. Open **Account → MCP / IDE** ([direct link](https://apptrack.ninja/account?tab=mcp)).
3. Create a key. It is shown once; keys start with `ninja_`.

## Connect

### Ask your AI agent

Agents that can edit their own configuration (Claude Code, Cursor, Codex, Windsurf) can add
the server themselves. Paste this and replace the key:

```text
Add the AppTrack Ninja MCP server to this tool.
Endpoint: https://api.apptrack.ninja/mcp (Streamable HTTP)
Send this header with every request: Authorization: Bearer ninja_your_key
Setup guide: https://github.com/sulubeyn/apptrack-ninja-mcp
```

### Claude Code

```bash
claude mcp add --transport http apptrack https://api.apptrack.ninja/mcp --header "Authorization: Bearer ninja_your_key"
```

### Cursor

`~/.cursor/mcp.json`, or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "apptrack": {
      "url": "https://api.apptrack.ninja/mcp",
      "headers": { "Authorization": "Bearer ninja_your_key" }
    }
  }
}
```

### VS Code (GitHub Copilot)

`.vscode/mcp.json` in your workspace:

```json
{
  "servers": {
    "apptrack": {
      "type": "http",
      "url": "https://api.apptrack.ninja/mcp",
      "headers": { "Authorization": "Bearer ninja_your_key" }
    }
  }
}
```

### Other clients

Any client that supports remote MCP servers with custom headers:

- URL: `https://api.apptrack.ninja/mcp`
- Transport: Streamable HTTP
- Header: `Authorization: Bearer ninja_your_key`. Gateways that reserve `Authorization`
  can send the key in `X-Api-Key` instead.

Claude Desktop and claude.ai custom connectors: add `https://api.apptrack.ninja/mcp` as a custom
connector and sign in with AppTrack Ninja when asked (OAuth).

## Tools

| Tool | What it does |
|---|---|
| `search_apps` | Plain-language App Store search (developer country, genre, dates, ratings are read from the text) |
| `find_apps` | Filtered catalog: text, storefront, monetization, rating, dates, sort order, cursor paging |
| `get_app` | App detail: title, description, version and screenshots |
| `get_app_market` | Price, rating and version in one storefront |
| `get_app_ranks` | Chart positions today, the 7-day move and the best (Pro: daily series) |
| `get_app_transfers` | When the app moved between publisher accounts |
| `get_rating_history` | Daily rating and rating count (3 days free, 30 with Pro) |
| `get_app_estimates` | 30-day download and revenue estimates (Pro) |
| `trending_apps` | Top free, paid and grossing charts for a storefront |
| `get_publisher` | Publisher identity and their apps |
| `list_tracked_apps` | Apps in your ASO tracker |
| `add_tracked_app` | Add an app to your ASO tracker (writes) |
| `add_tracked_keywords` | Track keywords for an app (writes) |
| `get_tracked_keywords` | Tracked keywords with rank, popularity and difficulty |
| `get_keyword_history` | Daily rank history for one tracked keyword |
| `whoami` | The plan on this key and remaining calls this minute |

On the free plan list tools return up to 10 rows and, once the 25 daily calls are used, answer
with an upgrade note until the next UTC day.

## Example prompts

- "What are the top grossing games in Japan right now?"
- "Show me the apps published by Duolingo and their ratings."
- "Track the keywords 'pdf scanner' and 'scan to pdf' for my app 6736992829, then show
  their current ranks."

## Support

ninja@apptrack.ninja · [apptrack.ninja](https://apptrack.ninja)

## License

The documentation in this repository is MIT licensed. The AppTrack Ninja service is
subject to its [terms of service](https://apptrack.ninja/terms).
