# Chapter 7: Settings and Configuration

Claude Code is highly configurable. Understanding the settings hierarchy lets you customize behavior at the right level—global defaults, project standards, or local overrides.

## The Settings Hierarchy

Settings are loaded from multiple locations, with later ones overriding earlier:

1. **User settings**: `~/.claude/settings.json`
   - Your personal defaults across all projects

2. **Project settings**: `.claude/settings.json`
   - Shared with team, checked into git

3. **Local project settings**: `.claude/settings.local.json`
   - Personal overrides, git-ignored

## Basic Settings Structure

```json
{
  "model": "claude-sonnet-4-20250514",
  "maxTokens": 4096,
  "permissions": {
    "allowedTools": ["Read", "Write", "Edit", "Bash(git *)"],
    "deny": ["Read(./.env)", "Write(./.env)"]
  }
}
```

## Common Settings

### Model Selection

```json
{
  "model": "claude-sonnet-4-20250514"
}
```

Options:
- `claude-opus-4-5-20251101` — Most capable, slower, higher cost
- `claude-sonnet-4-5-20250929` — Balanced (recommended default)
- `claude-haiku-4-5-20251001` — Fastest, lowest cost

### Token Limits

```json
{
  "maxTokens": 4096,
  "maxTurns": 50
}
```

Limits response length and conversation depth.

### Permissions

```json
{
  "permissions": {
    "allowedTools": ["Read", "Write", "Bash(npm *)"],
    "deny": [
      "Read(./.env*)",
      "Write(./production.*)",
      "Bash(rm -rf *)"
    ]
  }
}
```

### Environment Variables

```json
{
  "env": {
    "NODE_ENV": "development",
    "DEBUG": "true"
  }
}
```

Passed to shell commands Claude executes.

## Project Settings for Teams

Create `.claude/settings.json` in your project root:

```json
{
  "model": "claude-sonnet-4-20250514",
  "permissions": {
    "allowedTools": [
      "Read",
      "Write", 
      "Edit",
      "Bash(npm *)",
      "Bash(git *)",
      "Bash(python *)"
    ],
    "deny": [
      "Read(./.env*)",
      "Write(./.env*)",
      "Read(./secrets/*)",
      "Write(./secrets/*)"
    ]
  }
}
```

Commit this to git. Everyone on the team gets consistent settings.

## Local Overrides

For personal preferences that shouldn't affect the team, use `.claude/settings.local.json`:

```json
{
  "model": "claude-opus-4-5-20251101",
  "maxTokens": 8192
}
```

Add `.claude/settings.local.json` to `.gitignore`.

## User-Level Defaults

Set your personal defaults in `~/.claude/settings.json`:

```json
{
  "model": "claude-sonnet-4-20250514",
  "permissions": {
    "deny": [
      "Bash(sudo *)",
      "Bash(rm -rf *)"
    ]
  }
}
```

These apply to all projects unless overridden.

## Complete Settings Example

```json
{
  "model": "claude-sonnet-4-20250514",
  "maxTokens": 4096,
  "maxTurns": 100,
  
  "permissions": {
    "allowedTools": [
      "Read",
      "Write",
      "Edit",
      "Grep",
      "Glob",
      "Bash(npm *)",
      "Bash(git *)",
      "Bash(python *)",
      "Bash(node *)",
      "mcp__github__*"
    ],
    "deny": [
      "Read(./.env*)",
      "Write(./.env*)",
      "Bash(rm -rf *)",
      "Bash(sudo *)"
    ]
  },
  
  "env": {
    "NODE_ENV": "development"
  },
  
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "black $CLAUDE_FILE_PATH"
          }
        ]
      },
      {
        "matcher": "Write(*.ts)",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

## MCP Configuration

MCP servers can be configured in settings or in a separate `.mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "~/Documents"]
    }
  }
}
```

Environment variable substitution (`${VAR}`) lets you avoid hardcoding secrets.

## Viewing Current Configuration

In a Claude session:

```
/config
```

Or from the command line:

```bash
claude config show
```

## Configuration Best Practices

### 1. Start Minimal

Begin with few settings. Add as you discover needs.

### 2. Team Standards in Project Settings

Put shared standards in `.claude/settings.json`:
- Common tool permissions
- Hooks for formatting and linting
- MCP servers the team uses

### 3. Personal Preferences in Local Settings

Put individual preferences in `.claude/settings.local.json`:
- Model preference (some prefer opus)
- Additional tool permissions
- Personal MCP servers

### 4. Security in User Settings

Put security baselines in `~/.claude/settings.json`:
- Always-denied dangerous commands
- Environment file protections
- Baseline permission restrictions

### 5. Keep Secrets Out

Never put API keys or secrets directly in settings files. Use:
- Environment variables: `${GITHUB_TOKEN}`
- Separate secret management
- `.env` files that are git-ignored

## Directory Structure

A well-organized Claude Code setup:

```
~/.claude/
├── settings.json          # Global defaults
├── CLAUDE.md              # Global context
├── commands/              # Personal commands
├── agents/                # Personal subagents
└── skills/                # Personal skills

project/
├── .claude/
│   ├── settings.json      # Team settings (git-tracked)
│   ├── settings.local.json # Personal overrides (git-ignored)
│   ├── commands/          # Project commands
│   ├── agents/            # Project subagents
│   └── skills/            # Project skills
├── CLAUDE.md              # Project context
├── .mcp.json              # MCP configuration
└── .gitignore             # Include settings.local.json
```

## Resetting Configuration

If settings get confused:

```bash
# View current config
claude config show

# Reset to defaults (preserves commands/skills)
claude config reset

# Nuclear option: remove all user config
rm -rf ~/.claude
```

Then reconfigure from scratch.

## Environment-Specific Settings

For different environments, use environment variables:

```json
{
  "model": "${CLAUDE_MODEL:-claude-sonnet-4-20250514}",
  "env": {
    "API_URL": "${API_URL:-http://localhost:3000}"
  }
}
```

Then set variables per environment:

```bash
# Development
export CLAUDE_MODEL="claude-haiku-4-5-20251001"

# Production debugging
export CLAUDE_MODEL="claude-opus-4-5-20251101"
```

Settings are powerful but can be complex. Start simple, add configuration as you need it, and keep security settings at the user level where they can't be overridden.
