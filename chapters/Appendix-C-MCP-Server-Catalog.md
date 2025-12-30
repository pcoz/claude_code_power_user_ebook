# Appendix C: MCP Server Catalog

A curated list of useful MCP servers for Claude Code.

## Official Anthropic Servers

### Filesystem
**Purpose**: Read/write files outside your project directory.
```bash
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/Documents ~/Projects
```

### GitHub
**Purpose**: GitHub operations—issues, PRs, repos.
```bash
claude mcp add github -e GITHUB_TOKEN=ghp_xxx -- npx -y @modelcontextprotocol/server-github
```

### Sequential Thinking
**Purpose**: Enhanced reasoning for complex problems.
```bash
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking
```

### Puppeteer
**Purpose**: Browser automation and screenshots.
```bash
claude mcp add puppeteer -- npx -y @modelcontextprotocol/server-puppeteer
```

### Memory
**Purpose**: Persistent memory across sessions.
```bash
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory
```

## Database Servers

### PostgreSQL
```bash
claude mcp add postgres -e DATABASE_URL=postgresql://user:pass@localhost/db -- npx -y @modelcontextprotocol/server-postgres
```

### SQLite
```bash
claude mcp add sqlite -- npx -y @modelcontextprotocol/server-sqlite ./database.db
```

## Communication Servers

### Slack
```bash
claude mcp add slack -e SLACK_TOKEN=xoxb-xxx -- npx -y @modelcontextprotocol/server-slack
```

### Discord
```bash
claude mcp add discord -e DISCORD_TOKEN=xxx -- npx -y mcp-server-discord
```

## Productivity Servers

### Google Drive
```bash
claude mcp add gdrive -- npx -y @anthropic/mcp-server-gdrive
```
Requires OAuth setup.

### Notion
```bash
claude mcp add notion -e NOTION_TOKEN=secret_xxx -- npx -y mcp-server-notion
```

### Linear
```bash
claude mcp add linear -e LINEAR_API_KEY=lin_api_xxx -- npx -y mcp-server-linear
```

## Search Servers

### Web Search (Tavily)
```bash
claude mcp add tavily -e TAVILY_API_KEY=xxx -- npx -y mcp-server-tavily
```

### Brave Search
```bash
claude mcp add brave -e BRAVE_API_KEY=xxx -- npx -y mcp-server-brave
```

### Omnisearch (Multiple engines)
```bash
claude mcp add omnisearch -e TAVILY_API_KEY=xxx -e BRAVE_API_KEY=xxx -- npx -y mcp-omnisearch
```

## Development Servers

### Docker
```bash
claude mcp add docker -- npx -y mcp-server-docker
```

### Kubernetes
```bash
claude mcp add k8s -- npx -y mcp-server-kubernetes
```

### AWS
```bash
claude mcp add aws -- npx -y mcp-server-aws
```
Requires AWS credentials configuration.

## Configuration File Format

Instead of CLI commands, configure in `~/.claude.json`:

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
        "GITHUB_TOKEN": "ghp_your_token_here"
      }
    },
    "postgres": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://localhost/mydb"
      }
    }
  }
}
```

## Project-Specific MCP

For project-specific servers, use `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "project-db": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sqlite", "./data/app.db"]
    }
  }
}
```

This file can be committed to git for team sharing.

## Finding More Servers

### Official Registry
Check Anthropic's MCP server documentation for official servers.

### Community Servers
- GitHub: Search "mcp-server" for community implementations
- npm: Search "@modelcontextprotocol" packages

### Building Custom Servers
MCP servers can be built in any language that supports JSON-RPC over stdin/stdout. See the MCP specification for implementation details.

## Best Practices

**Start minimal.** Add servers as you need them, not preemptively.

**Secure your tokens.** Use environment variables, not hardcoded values.

**Test connections.** Use `claude --mcp-debug` to troubleshoot.

**Monitor costs.** Some servers (like search APIs) have usage costs.

**Keep updated.** MCP servers improve frequently; update with `npm update -g`.
