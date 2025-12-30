# Chapter 36: Writing Claude Code Plugins

Claude Code isn't a closed system. It's designed to be extended. You can add new commands, teach Claude new skills, connect external tools, and package everything for distribution.

This chapter covers the complete extension architecture—from simple commands to full plugins you can share with the world.

---

## The Extension Landscape

Claude Code has multiple extension mechanisms, each with a different purpose:

| Mechanism | What It Does | How It's Invoked |
|-----------|--------------|------------------|
| **Slash Commands** | Custom prompts you invoke explicitly | `/command-name` |
| **Agent Skills** | Knowledge Claude uses automatically | Automatic when relevant |
| **MCP Servers** | External tools and data sources | Via Claude's tool system |
| **Hooks** | Scripts triggered by events | Automatic on events |
| **Plugins** | Packaged bundles of all the above | Via plugin manager |

You can use each mechanism independently, or bundle them together into plugins.

---

## Slash Commands: The Simplest Extension

A slash command is a markdown file that defines a reusable prompt.

### Creating Your First Command

```bash
mkdir -p .claude/commands
```

Create `.claude/commands/explain.md`:

```markdown
---
description: Explain code with diagrams and analogies
allowed-tools: Read, Grep, Glob
---

Explain the following code clearly:

1. Start with a one-sentence summary
2. Draw an ASCII diagram showing the structure or flow
3. Walk through step-by-step how it works
4. Highlight any gotchas or non-obvious behavior

Code to explain: $ARGUMENTS
```

**Usage:**
```
/explain src/auth/middleware.js
```

### Command Features

**Arguments:** Use `$ARGUMENTS` for the full argument string, or `$1`, `$2` for positional arguments.

```markdown
Compare $1 with $2 and explain the differences.
```

**File References:** Use `@` to include file contents:

```markdown
---
description: Review code against our standards
---

Review this code against our coding standards:

Standards document:
@docs/coding-standards.md

Code to review:
$ARGUMENTS
```

**Bash Execution:** Use backticks with `!` prefix to run commands:

```markdown
---
description: Summarize recent git activity
---

Here's the recent git activity:

`!git log --oneline -20`

`!git diff --stat HEAD~5`

Summarize what's been happening in this codebase.
```

### Frontmatter Options

```yaml
---
description: Short description shown in command list
allowed-tools: Read, Grep, Glob, Bash  # Tools Claude can use without asking
model: claude-sonnet-4-20250514         # Specific model for this command
argument-hint: "[file-pattern]"         # Help text for arguments
---
```

### Where Commands Live

| Location | Scope |
|----------|-------|
| `.claude/commands/` | Project-specific (shared via git) |
| `~/.claude/commands/` | Personal (all your projects) |

---

## Agent Skills: Automatic Expertise

Skills are different from commands. You don't invoke them explicitly—Claude automatically uses them when relevant.

### How Skills Work

Claude reads the skill's description and decides when to apply it. A skill for "reviewing pull requests" activates when you ask Claude to review a PR. You don't type `/review-pr`; Claude just knows how to do it.

### Creating a Skill

Create a directory with a `SKILL.md` file:

```bash
mkdir -p .claude/skills/code-review
```

Create `.claude/skills/code-review/SKILL.md`:

```markdown
---
name: code-review
description: Reviews code for quality, security, and maintainability. Use when reviewing pull requests, code changes, or asking about code quality.
---

# Code Review Skill

When reviewing code, follow this process:

## 1. Security Check
- Look for injection vulnerabilities (SQL, XSS, command injection)
- Check authentication and authorization
- Verify input validation
- Look for hardcoded secrets

## 2. Quality Check
- Verify error handling
- Check for edge cases
- Look for code duplication
- Assess naming clarity

## 3. Maintainability Check
- Is the code readable?
- Are there sufficient comments for complex logic?
- Is the structure logical?
- Are dependencies appropriate?

## Output Format
Structure your review as:
- **Critical Issues** (must fix)
- **Suggestions** (should consider)
- **Nitpicks** (minor improvements)
- **Praise** (what's done well)
```

### The Description Is Critical

The `description` field determines when Claude uses the skill. Be specific:

**Good:** "Reviews code for security vulnerabilities, code quality, and maintainability. Use when asked to review PRs, check code quality, or audit security."

**Bad:** "Code review helper"

### Multi-File Skills

For complex skills, split across files:

```
.claude/skills/database-queries/
+-- SKILL.md              # Overview (keep under 500 lines)
+-- schema.md             # Database schema reference
+-- examples.md           # Query examples
+-- scripts/
    +-- validate-query.py # Utility script
```

Reference supporting files from SKILL.md:

```markdown
For database schema, see [schema.md](schema.md).
For query examples, see [examples.md](examples.md).

To validate a query:
python scripts/validate-query.py "SELECT * FROM users"
```

### Skill Frontmatter

```yaml
---
name: my-skill                    # Unique identifier
description: When to use this     # Critical for activation
allowed-tools: Read, Bash         # Tools without permission prompts
model: claude-sonnet-4-20250514   # Optional specific model
---
```

---

## MCP Servers: External Tools

MCP (Model Context Protocol) connects Claude to external systems—databases, APIs, monitoring tools, anything with an MCP server.

### Adding an MCP Server

**Remote HTTP server:**
```bash
claude mcp add --transport http sentry https://mcp.sentry.io/
```

**Local stdio server:**
```bash
claude mcp add --transport stdio postgres \
  --env DATABASE_URL=postgresql://localhost/mydb \
  -- npx -y @modelcontextprotocol/server-postgres
```

### Managing MCP Servers

```bash
claude mcp list              # See all servers
claude mcp get postgres      # Get details
claude mcp remove postgres   # Remove server
```

Inside Claude Code, use `/mcp` to manage servers interactively.

### Popular MCP Servers

| Server | Purpose |
|--------|---------|
| `@modelcontextprotocol/server-postgres` | Query PostgreSQL databases |
| `@modelcontextprotocol/server-filesystem` | Extended file operations |
| `@modelcontextprotocol/server-github` | GitHub integration |
| `airtable-mcp-server` | Airtable access |
| Custom servers | Build your own |

### Building a Custom MCP Server

MCP servers can be built in any language. Here's a minimal Python example:

```python
# my_server.py
import json
import sys

def handle_request(request):
    method = request.get("method")

    if method == "tools/list":
        return {
            "tools": [{
                "name": "get_weather",
                "description": "Get current weather for a city",
                "inputSchema": {
                    "type": "object",
                    "properties": {
                        "city": {"type": "string"}
                    },
                    "required": ["city"]
                }
            }]
        }

    if method == "tools/call":
        tool_name = request["params"]["name"]
        args = request["params"]["arguments"]

        if tool_name == "get_weather":
            # Your implementation here
            return {"content": [{"type": "text", "text": f"Weather in {args['city']}: Sunny, 72°F"}]}

    return {"error": "Unknown method"}

# MCP stdio protocol
for line in sys.stdin:
    request = json.loads(line)
    response = handle_request(request)
    response["id"] = request.get("id")
    print(json.dumps(response))
    sys.stdout.flush()
```

Register it:
```bash
claude mcp add --transport stdio weather -- python my_server.py
```

---

## Hooks: Event-Driven Automation

Hooks run scripts when specific events occur in Claude Code.

### Hook Configuration

Create `.claude/hooks/hooks.json`:

```json
{
  "hooks": [
    {
      "event": "on_tool_call",
      "script": "./hooks/lint-on-save.sh",
      "match": {
        "tool": "Write",
        "pattern": "**/*.py"
      }
    },
    {
      "event": "on_session_start",
      "script": "./hooks/setup.sh"
    }
  ]
}
```

### Available Events

| Event | When It Fires |
|-------|---------------|
| `on_session_start` | Claude Code session begins |
| `on_session_end` | Session ends |
| `on_tool_call` | Before a tool is called |
| `on_tool_result` | After a tool returns |
| `on_permission_request` | Before asking for permission |

### Example: Auto-Format Python

`.claude/hooks/hooks.json`:
```json
{
  "hooks": [{
    "event": "on_tool_result",
    "script": "./hooks/format-python.sh",
    "match": {
      "tool": "Write",
      "pattern": "**/*.py"
    }
  }]
}
```

`.claude/hooks/format-python.sh`:
```bash
#!/bin/bash
FILE="$CLAUDE_TOOL_ARG_FILE_PATH"
if [[ -f "$FILE" ]]; then
    black "$FILE" 2>/dev/null || true
    isort "$FILE" 2>/dev/null || true
fi
```

---

## Plugins: Bundling Everything

Plugins package commands, skills, hooks, and MCP servers into a distributable unit.

### Plugin Structure

```
my-plugin/
+-- .claude-plugin/
|   +-- plugin.json          # Required: Plugin manifest
+-- commands/                 # Slash commands
|   +-- hello.md
|   +-- deploy.md
+-- skills/                   # Agent skills
|   +-- code-review/
|       +-- SKILL.md
+-- hooks/                    # Event hooks
|   +-- hooks.json
+-- agents/                   # Custom agents
|   +-- reviewer/
|       +-- AGENT.md
+-- .mcp.json                # MCP server configuration
+-- .lsp.json                # LSP server configuration
```

### The Plugin Manifest

`.claude-plugin/plugin.json`:

```json
{
  "name": "my-awesome-plugin",
  "description": "Adds code review and deployment capabilities",
  "version": "1.0.0",
  "author": {
    "name": "Your Name",
    "email": "you@example.com"
  },
  "homepage": "https://github.com/you/my-awesome-plugin",
  "repository": "https://github.com/you/my-awesome-plugin",
  "license": "MIT"
}
```

### Plugin Namespacing

When installed as a plugin, commands are namespaced:

- Standalone command: `/hello`
- Plugin command: `/my-awesome-plugin:hello`

This prevents conflicts between plugins.

### Testing Your Plugin Locally

```bash
claude --plugin-dir ./my-plugin
```

This loads your plugin for the current session without installing it.

### Installing Plugins

**From GitHub:**
```bash
claude plugin install github:username/repo-name
```

**From local directory:**
```bash
claude plugin install ./my-plugin
```

### Managing Plugins

```bash
claude plugin list              # See installed plugins
claude plugin update plugin-name  # Update a plugin
claude plugin remove plugin-name  # Uninstall
```

---

## Building a Complete Plugin: Example

Let's build a "deployment helper" plugin.

### Step 1: Create Structure

```bash
mkdir -p deploy-helper/.claude-plugin
mkdir -p deploy-helper/commands
mkdir -p deploy-helper/skills/deployment
mkdir -p deploy-helper/hooks
```

### Step 2: Plugin Manifest

`deploy-helper/.claude-plugin/plugin.json`:
```json
{
  "name": "deploy-helper",
  "description": "Deployment automation and safety checks",
  "version": "1.0.0",
  "author": { "name": "Your Team" }
}
```

### Step 3: Commands

`deploy-helper/commands/deploy.md`:
```markdown
---
description: Deploy to staging or production
allowed-tools: Bash, Read
argument-hint: "[staging|production]"
---

# Deployment Command

Environment: $1

## Pre-deployment Checklist

Before deploying, verify:

1. `!git status` — Working directory clean?
2. `!npm test` — Tests passing?
3. `!git log -1 --oneline` — Correct commit?

## Deploy

If all checks pass, run the deployment:

- For staging: `!./scripts/deploy.sh staging`
- For production: `!./scripts/deploy.sh production`

Confirm each step before proceeding.
```

`deploy-helper/commands/rollback.md`:
```markdown
---
description: Rollback to previous deployment
allowed-tools: Bash
---

# Rollback

Show recent deployments:
`!./scripts/list-deployments.sh`

Ask user which deployment to rollback to, then execute:
`!./scripts/rollback.sh <version>`
```

### Step 4: Skill

`deploy-helper/skills/deployment/SKILL.md`:
```markdown
---
name: deployment-safety
description: Ensures safe deployment practices. Use when discussing deployments, releases, or production changes.
---

# Deployment Safety Skill

When discussing deployments, always consider:

## Pre-deployment
- Are all tests passing?
- Has the code been reviewed?
- Is there a rollback plan?
- Are dependent services ready?

## During Deployment
- Monitor error rates
- Watch latency metrics
- Check log output
- Verify health endpoints

## Post-deployment
- Smoke test critical paths
- Monitor for 15 minutes minimum
- Document any issues
- Update deployment log

## Red Flags
Alert on these patterns:
- Deploying on Friday afternoon
- Skipping tests "just this once"
- Large changes without incremental rollout
- Missing rollback scripts
```

### Step 5: Hooks

`deploy-helper/hooks/hooks.json`:
```json
{
  "hooks": [{
    "event": "on_session_start",
    "script": "./hooks/check-deploy-status.sh"
  }]
}
```

`deploy-helper/hooks/check-deploy-status.sh`:
```bash
#!/bin/bash
# Check if there's an active deployment
if [ -f ".deploy-in-progress" ]; then
    echo "WARNING: Deployment in progress since $(cat .deploy-in-progress)"
fi
```

### Step 6: Test It

```bash
claude --plugin-dir ./deploy-helper
```

Try:
```
/deploy-helper:deploy staging
```

### Step 7: Distribute

Push to GitHub, then others can install:
```bash
claude plugin install github:yourname/deploy-helper
```

---

## Best Practices

### Commands vs Skills

| Use Commands When | Use Skills When |
|------------------|-----------------|
| User explicitly invokes | Claude should auto-activate |
| Specific workflow step | General knowledge area |
| Need arguments | Context-dependent behavior |
| Quick one-off tasks | Ongoing expertise |

### Keep Skills Focused

Each skill should do one thing well. Instead of a giant "development" skill, create:
- `code-review` skill
- `testing` skill
- `documentation` skill

### Version Your Plugins

Use semantic versioning in `plugin.json`. When you make breaking changes, bump the major version.

### Test Before Distributing

Always test with `--plugin-dir` before publishing. Check that:
- Commands work with various arguments
- Skills activate appropriately
- Hooks don't break normal workflows
- No hardcoded paths or secrets

### Document Your Plugin

Include a README.md at the plugin root explaining:
- What the plugin does
- How to use each command
- What skills are included
- Any configuration needed

---

## Summary

Claude Code's extension system is layered:

1. **Slash Commands** — Explicit prompts you invoke
2. **Agent Skills** — Knowledge Claude applies automatically
3. **MCP Servers** — External tool integrations
4. **Hooks** — Event-driven automation
5. **Plugins** — Packaged bundles of all the above

Start simple: a single command in `.claude/commands/`. When you have something worth sharing, package it as a plugin.

The extension system means Claude Code can be customized for any workflow, any team, any domain. Your expertise can become reusable tools that make Claude smarter for everyone who installs them.
