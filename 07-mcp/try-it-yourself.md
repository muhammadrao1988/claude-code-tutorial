# Try it yourself: MCP Servers

## Exercise

Connect Claude to a local filesystem server so it can access a specific folder.

### Step 1: Add the filesystem MCP server

```bash
claude mcp add --transport stdio filesystem -- npx -y @modelcontextprotocol/server-filesystem /path/to/your/documents
```

Replace `/path/to/your/documents` with an actual folder on your machine (e.g., `~/Documents`).

### Step 2: Verify it's connected

Start Claude Code and run:

```
/mcp
```

You should see `filesystem` listed as connected.

### Step 3: Use it

Ask Claude:

```
List all files in my documents folder using the filesystem server
```

Claude will use the MCP server to read the directory instead of using regular bash commands.

### Step 4: Try a remote server (optional)

If you have a Notion, Gmail, or other account, try connecting a remote server:

```bash
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

Then authenticate with `/mcp` inside Claude Code.

### Step 5: Clean up

To remove a server you no longer need:

```bash
claude mcp remove filesystem
```
