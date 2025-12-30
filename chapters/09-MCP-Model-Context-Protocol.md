# Chapter 9: MCP — Model Context Protocol

MCP (Model Context Protocol) is how Claude Code connects to the outside world. It's a standardized way to give Claude access to external services, databases, APIs, and tools.

Think of MCP servers as plugins. Each one adds new capabilities—searching the web, querying a database, interacting with GitHub, browsing the filesystem, even controlling a web browser.

## How MCP Works

```
Claude Code ←→ MCP Protocol ←→ MCP Servers ←→ External Services
```

You configure MCP servers. Each server exposes tools. Claude can use those tools during your session.

When you ask Claude to "check what's in my Slack messages" and you have a Slack MCP server configured, Claude calls the server, which calls Slack's API, and returns the results.

## Configuring MCP Servers

### Using the CLI

```bash
# List configured servers
claude mcp list

# Add a new server
claude mcp add <name> <command> [parameters...]

# Remove a server
claude mcp remove <name>
```

### Adding Common Servers

**Filesystem access:**
```bash
claude mcp add filesystem -s user -- npx -y @modelcontextprotocol/server-filesystem ~/Documents ~/Projects
```

**Sequential thinking (helps with complex reasoning):**
```bash
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking
```

**GitHub integration:**
```bash
claude mcp add github -e GITHUB_TOKEN=your-token -- npx -y @modelcontextprotocol/server-github
```

### Direct Configuration

For complex setups, edit the configuration file directly.

**Global config:** `~/.claude.json`  
**Project config:** `.mcp.json`

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "~/Documents"]
    },
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token-here"
      }
    }
  }
}
```

## Popular MCP Servers

### Official Anthropic Servers

| Server | Purpose |
|--------|---------|
| `server-filesystem` | Read/write files outside project |
| `server-github` | GitHub operations |
| `server-sequential-thinking` | Enhanced reasoning |
| `server-puppeteer` | Browser automation |
| `server-memory` | Persistent memory across sessions |

### Community Servers

| Server | Purpose |
|--------|---------|
| `server-postgres` | PostgreSQL database access |
| `server-slack` | Slack messages and channels |
| `server-google-drive` | Google Drive files |
| `server-notion` | Notion pages and databases |
| `server-linear` | Linear issue tracking |
| `mcp-omnisearch` | Multi-engine web search |

Find more at the MCP server registry and community repositories.

## Using MCP Tools

Once configured, Claude automatically knows about available tools. Just ask naturally:

"Search my Google Drive for the Q3 budget spreadsheet"

"Create a GitHub issue for this bug"

"Query the database for all users who signed up this week"

Claude decides which tools to use based on your request.

## Browser Automation with Puppeteer

The Puppeteer MCP server is particularly powerful—it gives Claude a web browser:

```json
{
  "mcpServers": {
    "puppeteer": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-puppeteer"]
    }
  }
}
```

Now Claude can:
- Navigate to websites
- Take screenshots
- Fill out forms
- Click buttons
- Extract data

This enables visual testing, web scraping, and automated testing workflows.

## Database Access

For PostgreSQL:

```json
{
  "mcpServers": {
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
      }
    }
  }
}
```

Now Claude can query your database directly:

"Show me all orders from the last 7 days with total > $100"

"Find users who haven't logged in for 30 days"

## Combining Multiple Servers

The real power comes from combining servers:

```json
{
  "mcpServers": {
    "github": { ... },
    "slack": { ... },
    "postgres": { ... }
  }
}
```

"Check GitHub for open issues, cross-reference with Slack mentions, and show me which ones are from our top 10 customers in the database"

Claude orchestrates across all three services.

## Limiting Tool Access

For safety, you can restrict which tools Claude can use:

```bash
# Allow only specific tools
claude --allowedTools "Read,Write,mcp__github__*"

# Allow all tools from one server
claude --allowedTools "mcp__postgres__*"

# Deny specific tools
claude --deniedTools "mcp__github__delete_repo"
```

## MCP in Settings

Configure default tool permissions in your settings:

```json
{
  "permissions": {
    "allowedTools": ["Read", "Write", "Bash(git *)"],
    "deny": [
      "Write(./.env)",
      "Bash(rm -rf *)"
    ]
  }
}
```

## Debugging MCP

Launch with debug mode:

```bash
claude --mcp-debug
```

This shows detailed logs about MCP server connections and tool calls.

## When to Use MCP vs CLI Tools

**Use MCP when:**
- You need structured API access (GitHub, Slack, databases)
- The service requires authentication handling
- You want Claude to have persistent access across sessions

**Use CLI tools when:**
- You already have working command-line tools
- You want simpler setup
- The tool is a one-off script

Claude is equally capable with both. MCP is more structured; CLI tools are more flexible.

## Building Custom MCP Servers

For internal tools, you can build custom MCP servers. They're just programs that speak the MCP protocol over stdin/stdout.

The basics:
1. Implement the MCP protocol (JSON-RPC)
2. Expose tools with names and schemas
3. Handle tool invocations
4. Return structured results

See the MCP specification and example servers for implementation details.

## The Pattern That Works

The best MCP setups are minimal. Add servers for services you use constantly. Don't add everything just because you can—each server adds overhead.

Start with:
- GitHub (if you use it)
- Your database (if you need direct queries)
- One search tool (web search or docs search)

Add more only when you have a specific need.
