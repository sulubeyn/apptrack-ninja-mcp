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
| Auth | OAuth 2.1 sign-in (recommended), or an API key as `Authorization: Bearer ninja_…` |
| Plan | AppTrack Ninja Pro |
| Rate limits | 600 reads and 30 searches per minute |

## Before you start

You need an [AppTrack Ninja](https://apptrack.ninja) account on the **Pro** plan. That is
all: with sign-in there is no key to create or paste.

## Connect with sign-in (recommended)

Add the server URL to your client. The first time it connects, the client opens
apptrack.ninja in your browser; sign in and approve the connection. Your client then
refreshes its access on its own.

Every connection you approve is listed under **Account → MCP / IDE**
([apptrack.ninja/account?tab=mcp](https://apptrack.ninja/account?tab=mcp)), where you can
disconnect it at any time.

### Claude (claude.ai, Desktop and mobile)

1. Open **Settings → Connectors** and choose **Add custom connector**.
2. Name it `AppTrack Ninja` and enter the URL `https://api.apptrack.ninja/mcp`.
3. Click **Connect** and approve on apptrack.ninja.

Connectors added on claude.ai are also available in Claude Desktop and the mobile apps.

### Claude Code

```bash
claude mcp add --transport http --scope user apptrack https://api.apptrack.ninja/mcp
```

Then run `/mcp` in Claude Code, pick **apptrack** and sign in.

### Cursor

[Install in Cursor](https://cursor.com/en/install-mcp?name=apptrack&config=eyJ1cmwiOiJodHRwczovL2FwaS5hcHB0cmFjay5uaW5qYS9tY3AifQ%3D%3D),
or open this link:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=apptrack&config=eyJ1cmwiOiJodHRwczovL2FwaS5hcHB0cmFjay5uaW5qYS9tY3AifQ==
```

Or add it by hand to `~/.cursor/mcp.json`, or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "apptrack": {
      "url": "https://api.apptrack.ninja/mcp"
    }
  }
}
```

Cursor shows a **Connect** button next to the server; click it to sign in.

### VS Code (GitHub Copilot)

[Install in VS Code](https://vscode.dev/redirect/mcp/install?name=apptrack&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.apptrack.ninja%2Fmcp%22%7D),
or open this link:

```text
vscode:mcp/install?%7B%22name%22%3A%22apptrack%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.apptrack.ninja%2Fmcp%22%7D
```

Or add it by hand to `.vscode/mcp.json` in your workspace:

```json
{
  "servers": {
    "apptrack": {
      "type": "http",
      "url": "https://api.apptrack.ninja/mcp"
    }
  }
}
```

VS Code asks you to sign in when the server starts.

### Windsurf

`~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "apptrack": {
      "serverUrl": "https://api.apptrack.ninja/mcp"
    }
  }
}
```

Refresh the MCP servers in Windsurf and sign in when asked.

### Codex

```bash
codex mcp add apptrack --url https://api.apptrack.ninja/mcp
```

```bash
codex mcp login apptrack
```

### Gemini CLI

```bash
gemini mcp add --scope user --transport http apptrack https://api.apptrack.ninja/mcp
```

Then run `/mcp auth apptrack` in Gemini CLI to sign in.

### Other clients

Any client that supports remote MCP servers with OAuth can connect with just the URL
`https://api.apptrack.ninja/mcp`. The server answers unauthenticated requests with
`401` and a `WWW-Authenticate` header, publishes its protected-resource and
authorization-server metadata under `/.well-known/`, and supports dynamic client
registration and PKCE, so clients find the sign-in flow on their own.

## Using an API key

Use a key when your client cannot do OAuth sign-in, or for scripts and CI.

1. Sign in at [apptrack.ninja](https://apptrack.ninja) on the Pro plan.
2. Open **Account → MCP / IDE** ([direct link](https://apptrack.ninja/account?tab=mcp)).
3. Create a key. It is shown once; keys start with `ninja_`.

Send it with every request as `Authorization: Bearer ninja_your_key`. Gateways that
reserve `Authorization` can send the key in `X-Api-Key` instead.

Claude Code:

```bash
claude mcp add --transport http --scope user apptrack https://api.apptrack.ninja/mcp --header "Authorization: Bearer ninja_your_key"
```

Cursor (`~/.cursor/mcp.json`):

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

VS Code (`.vscode/mcp.json`):

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

## Tools

All tools are read-only except `add_tracked_app` and `add_tracked_keywords`, which change
your ASO tracker.

| Tool | Access | What it does |
|---|---|---|
| `search_apps` | Read | Full-text App Store search |
| `find_apps` | Read | Filtered app search: genre, price, rating, sort order |
| `get_app` | Read | App detail: description, screenshots, version history |
| `get_app_market` | Read | Price, rating and version in one storefront |
| `trending_apps` | Read | Top free, paid and grossing charts for a storefront |
| `get_publisher` | Read | Publisher identity and their apps |
| `list_tracked_apps` | Read | Apps in your ASO tracker |
| `get_tracked_keywords` | Read | Tracked keywords with rank, popularity and difficulty |
| `get_keyword_history` | Read | Daily rank history for one tracked keyword |
| `whoami` | Read | Your plan and remaining calls this minute |
| `add_tracked_app` | **Write** | Add an app to your ASO tracker |
| `add_tracked_keywords` | **Write** | Track keywords for an app |

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
