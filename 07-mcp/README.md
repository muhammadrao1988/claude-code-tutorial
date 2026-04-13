# MCP Servers

## What is it?

MCP (Model Context Protocol) servers connect Claude to external tools and services. Out of the box, Claude can read files, run commands, and search code. MCP servers extend that -- they let Claude talk to databases, APIs, browsers, calendars, and anything else that has an MCP server.

Think of MCP servers as plugins. Each one gives Claude a new ability it didn't have before.

## Why use it?

Claude can't access your database, your project management tool, or your browser by default. MCP servers bridge that gap. Instead of copying data from Notion into Claude, you connect a Notion MCP server and Claude reads it directly.

## How to set it up

There are two ways to add MCP servers:

### Method 1: CLI command (quickest)

```bash
claude mcp add --transport http <name> <url>
```

For local servers that run as a process:

```bash
claude mcp add --transport stdio <name> -- <command> <args>
```

### Method 2: Config file (for teams)

Create `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "some-mcp-server"],
      "env": {
        "API_KEY": "${MY_API_KEY}"
      }
    }
  }
}
```

For remote HTTP servers:

```json
{
  "mcpServers": {
    "server-name": {
      "type": "http",
      "url": "https://api.example.com/mcp",
      "headers": {
        "Authorization": "Bearer ${API_TOKEN}"
      }
    }
  }
}
```

### Manage servers

```bash
claude mcp list              # see all servers
claude mcp get <name>        # see server details
claude mcp remove <name>     # remove a server
```

Inside Claude Code, type `/mcp` to check status and authenticate servers.

## Example 1: For Everyone

You want Claude to check and manage your **Google Calendar** -- see upcoming events, create new ones, find free time.

Google Calendar has an MCP server that Claude Code can connect to:

```bash
claude mcp add --transport http google-calendar https://mcp.google.com/calendar
```

Then inside Claude Code, run `/mcp` and authenticate with your Google account.

Once connected, you can say things like:

- "What's on my calendar this week?"
- "Schedule a meeting with the team on Thursday at 2pm"
- "Find a free 30-minute slot tomorrow afternoon"

Claude reads your calendar directly instead of you having to copy-paste event details.

**Without MCP:** You open Google Calendar, read the events, type them into Claude, and ask for suggestions.

**With MCP:** You say "what's on my calendar?" and Claude reads it directly.

### Other useful MCP servers for non-tech users

| Server | What it does |
|--------|-------------|
| Notion | Read and write Notion pages and databases |
| Gmail | Read, search, and draft emails |
| Slack | Read channels and send messages |
| Filesystem | Give Claude access to specific folders |

## Example 2: For Developers

You want Claude to **query your PostgreSQL database** directly while working on your app -- check schemas, run queries, debug data issues.

Add the Postgres MCP server:

```bash
claude mcp add --transport stdio postgres -- npx -y @modelcontextprotocol/server-postgres
```

Or add it to your `.mcp.json` so the whole team can use it:

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_URL": "${DATABASE_URL}"
      }
    }
  }
}
```

Set your database URL in your environment:

```bash
export DATABASE_URL="postgresql://user:pass@localhost:5432/mydb"
```

Now you can say things like:

- "Show me the schema for the users table"
- "Find all orders from the last 7 days with status 'pending'"
- "What indexes exist on the products table?"

Claude queries the database directly and gives you answers without you writing SQL manually.

### Other useful MCP servers for developers

| Server | What it does |
|--------|-------------|
| Playwright | Automate and test web apps in a browser |
| GitHub | Read issues, PRs, and repo data |
| Sentry | Pull error reports and stack traces |
| Context7 | Fetch up-to-date library documentation |

## Try it yourself

1. Pick an MCP server from the examples above
2. Install it using the CLI command
3. Run `/mcp` inside Claude Code to verify it's connected
4. Ask Claude something that uses the server

See [`try-it-yourself.md`](try-it-yourself.md) for a step-by-step exercise.

## Tips

- Use `${VAR}` syntax in `.mcp.json` for secrets. Never hardcode API keys.
- Options like `--transport` and `--env` must come BEFORE the server name in CLI commands.
- Use `--` to separate the server name from the command when adding stdio servers.
- Project servers in `.mcp.json` require approval the first time a teammate uses them (security).
- Use `/mcp` inside Claude Code to check server status and authenticate OAuth servers.
- Set `MCP_TIMEOUT=10000` as an env var if a server is slow to start.
