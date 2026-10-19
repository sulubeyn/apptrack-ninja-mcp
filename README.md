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
| Auth | `Authorization: Bearer <key>` |
| Plan | AppTrack Ninja Pro |
| Rate limits | 600 reads and 30 searches per minute, per key |

## Get a key

1. Sign in at [apptrack.ninja](https://apptrack.ninja) and start Pro.
2. Open **Account → MCP / IDE** ([direct link](https://apptrack.ninja/account?tab=mcp)).
3. Create a key. It is shown once; keys start with `ninja_`.

## Connect

### Claude Desktop / Cursor

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

### Claude Code

```bash
claude mcp add --transport http apptrack https://api.apptrack.ninja/mcp --header "Authorization: Bearer ninja_your_key"
```

## Tools

| Tool | What it does |
|---|---|
| `search_apps` | Full-text App Store search |
| `find_apps` | Filtered app search: genre, price, rating, sort order |
| `get_app` | App detail: description, screenshots, version history |
| `get_app_market` | Price, rating and version in one storefront |
| `trending_apps` | Top free, paid and grossing charts for a storefront |
| `get_publisher` | Publisher identity and their apps |
| `list_tracked_apps` | Apps in your ASO tracker |
| `add_tracked_app` | Add an app to your ASO tracker (writes) |
| `add_tracked_keywords` | Track keywords for an app (writes) |
| `get_tracked_keywords` | Tracked keywords with rank, popularity and difficulty |
| `get_keyword_history` | Daily rank history for one tracked keyword |
| `whoami` | The plan on this key and remaining calls this minute |

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
