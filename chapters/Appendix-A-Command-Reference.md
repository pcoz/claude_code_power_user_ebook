# Appendix A: Command Reference

## CLI Flags

### Starting Claude

```bash
claude                    # Interactive session in current directory
claude "prompt"           # Start with initial prompt
claude -p "prompt"        # Headless mode (non-interactive)
claude --help             # Show all options
claude --version          # Show version
```

### Session Options

```bash
claude --model <model>           # Use specific model (opus, sonnet, haiku)
claude --system-prompt "..."     # Override system prompt
claude --append-system-prompt    # Add to (not replace) system prompt
claude --max-turns <n>           # Limit conversation turns
claude --cwd <path>              # Set working directory
claude --add-dir <path>          # Add directory to context
```

### Permissions

```bash
claude --allowedTools "Read,Write,Bash(*)"   # Allow specific tools
claude --deniedTools "Bash(rm *)"            # Deny specific tools
claude --permission-mode manual              # Require approval for all
claude --permission-mode acceptEdits         # Auto-approve edits
claude --permission-mode acceptAll           # Auto-approve everything
```

### Output

```bash
claude -p "..." --output-format text         # Plain text (default)
claude -p "..." --output-format json         # JSON output
claude -p "..." --output-format stream-json  # Streaming JSON
claude --verbose                             # Show detailed logs
```

### MCP and Agents

```bash
claude --mcp-debug                           # Debug MCP connections
claude --agents '{...}'                      # Define subagents (JSON)
```

## In-Session Slash Commands

### General

| Command | Description |
|---------|-------------|
| `/help` | Show all available commands |
| `/exit` | End the session |
| `/clear` | Clear conversation history |
| `/compact` | Summarize and compress context |

### Configuration

| Command | Description |
|---------|-------------|
| `/model <name>` | Switch model (opus, sonnet, haiku) |
| `/model` | Show current model |
| `/cost` | Show token usage and costs |
| `/config` | Open configuration |

### Context

| Command | Description |
|---------|-------------|
| `/add-dir <path>` | Add directory to context |
| `@filename` | Reference a file in prompt |

### Tools and Integrations

| Command | Description |
|---------|-------------|
| `/mcp` | Configure MCP servers |
| `/agents` | Manage subagent definitions |
| `/hooks` | Review and approve hook changes |

### Feedback

| Command | Description |
|---------|-------------|
| `/bug` | Report an issue to Anthropic |

### Custom Commands

Your custom commands appear as:
- `/project:command-name` (from `.claude/commands/`)
- `/user:command-name` (from `~/.claude/commands/`)

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Escape` | Stop Claude's current operation |
| `Escape Escape` | Show message history for navigation |
| `Ctrl+C` | Exit Claude Code entirely |
| `Ctrl+R` | Search prompt history |
| `Ctrl+V` | Paste image from clipboard |
| `Tab` | Command completion |
| `Shift+drag` | Reference file when dragging |

## Shell Commands

Prefix with `!` to run shell commands directly:

```
! ls -la
! git status
! npm test
```

This bypasses Claude's processing for quick operations.

## Environment Variables

| Variable | Purpose |
|----------|---------|
| `ANTHROPIC_API_KEY` | API key for authentication |
| `CLAUDE_MODEL` | Default model |
| `CLAUDE_CODE_DEBUG` | Enable debug logging |

## Configuration Files

### User-Level

```
~/.claude/
+-- settings.json      # Global settings
+-- CLAUDE.md          # Global context
+-- commands/          # Personal commands
+-- agents/            # Personal subagents
+-- skills/            # Personal skills
```

### Project-Level

```
.claude/
+-- settings.json      # Project settings (git-tracked)
+-- settings.local.json # Local settings (git-ignored)
+-- commands/          # Project commands
+-- agents/            # Project subagents
+-- skills/            # Project skills

CLAUDE.md              # Project context (root directory)
.mcp.json              # MCP server config
```

## Settings Structure

```json
{
  "model": "claude-sonnet-4-20250514",
  "maxTokens": 4096,
  "permissions": {
    "allowedTools": ["Read", "Write", "Bash(git *)"],
    "deny": ["Read(./.env)", "Write(./production.*)"]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [{ "type": "command", "command": "black $file" }]
      }
    ]
  }
}
```

## MCP Configuration

```json
{
  "mcpServers": {
    "server-name": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "package-name"],
      "env": {
        "API_KEY": "your-key"
      }
    }
  }
}
```

## Tool Names for Permissions

| Tool | Description |
|------|-------------|
| `Read` | Read files |
| `Write` | Create new files |
| `Edit` | Modify existing files |
| `Bash` | Run shell commands |
| `Grep` | Search file contents |
| `Glob` | Find files by pattern |
| `Task` | Spawn subagents |
| `mcp__server__tool` | MCP tools (pattern: `mcp__<server>__<tool>`) |

### Permission Patterns

```
Read                    # All reads
Read(*.py)             # Python files only
Read(src/*)            # Files in src/
Bash                   # All bash commands
Bash(git *)            # Git commands only
Bash(npm run *)        # npm run commands
mcp__github__*         # All GitHub MCP tools
```

## Model Names

| Alias | Full Model ID |
|-------|---------------|
| `opus` | `claude-opus-4-5-20251101` |
| `sonnet` | `claude-sonnet-4-5-20250929` |
| `haiku` | `claude-haiku-4-5-20251001` |

## Exit Codes (Headless Mode)

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General error |
| 2 | Authentication failure |
| 3 | Permission denied |
| 4 | Timeout |

> **See Also:**
> - [Slash Commands and Custom Commands](06-Slash-Commands-and-Custom-Commands.md) for creating custom commands
> - [Settings and Configuration](07-Settings-and-Configuration.md) for configuration details
> - [Headless Mode and the SDK](13-Headless-Mode-and-the-SDK.md) for CLI automation

---

**Next:** [Appendix B: CLAUDE.md Templates](Appendix-B-CLAUDE-md-Templates.md) — Ready-to-use templates for your projects.
