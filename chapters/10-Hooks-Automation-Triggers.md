# Chapter 10: Hooks — Automation Triggers

Hooks are automatic actions that run at specific points in Claude's workflow. When Claude reads a file, writes code, or runs a command, hooks can intercept and act.

This is where Claude Code becomes truly automated. Instead of manually running formatters, linters, or validators, hooks do it automatically.

## Hook Events

Hooks can trigger on these events:

| Event | When It Fires |
|-------|--------------|
| `PreToolUse` | Before Claude uses any tool |
| `PostToolUse` | After Claude uses any tool |
| `Notification` | When Claude sends a notification |
| `Stop` | When Claude's response completes |

The most useful are `PreToolUse` (validation before action) and `PostToolUse` (automation after action).

## Configuration

Hooks live in your settings files:

**User settings:** `~/.claude/settings.json`  
**Project settings:** `.claude/settings.json`  
**Local settings:** `.claude/settings.local.json`

## Basic Hook Structure

```json
{
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
      }
    ]
  }
}
```

This hook runs Black (Python formatter) on every Python file Claude writes.

## Matcher Patterns

The `matcher` field determines which tool calls trigger the hook:

| Pattern | Matches |
|---------|---------|
| `Write` | All file writes |
| `Write(*.py)` | Python file writes |
| `Write(src/*.ts)` | TypeScript files in src/ |
| `Bash` | All bash commands |
| `Bash(npm *)` | npm commands |
| `*` | All tools |
| `mcp__github__*` | All GitHub MCP tools |

## Environment Variables in Hooks

Hooks receive context through environment variables:

| Variable | Contains |
|----------|----------|
| `$CLAUDE_FILE_PATH` | Path to the affected file |
| `$CLAUDE_PROJECT_DIR` | Project root directory |
| `$CLAUDE_TOOL_NAME` | Name of the tool used |

## Practical Hook Examples

### Auto-Format Python with Black

```json
{
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
      }
    ]
  }
}
```

### Auto-Format JavaScript with Prettier

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.js)",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
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

### Run ESLint After JavaScript Changes

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.js)",
        "hooks": [
          {
            "type": "command",
            "command": "eslint --fix $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### Run Tests After Source Changes

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(src/*.ts)",
        "hooks": [
          {
            "type": "command",
            "command": "npm test -- --findRelatedTests $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### Log All File Operations

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*)",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$(date): Wrote $CLAUDE_FILE_PATH\" >> ~/.claude/file-log.txt"
          }
        ]
      }
    ]
  }
}
```

### Protect Sensitive Files

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write(.env*)",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'BLOCKED: Cannot write to .env files' && exit 1"
          }
        ]
      }
    ]
  }
}
```

A `PreToolUse` hook that exits with non-zero status blocks the action.

## Advanced: Auto-Approval with Hooks

Hooks can auto-approve certain operations:

```python
#!/usr/bin/env python3
# ~/.claude/scripts/auto-approve-docs.py
import json
import sys

input_data = json.load(sys.stdin)
tool_name = input_data.get("tool_name", "")
tool_input = input_data.get("tool_input", {})

if tool_name == "Read":
    file_path = tool_input.get("file_path", "")
    if file_path.endswith((".md", ".txt", ".json")):
        output = {
            "decision": "approve",
            "reason": "Documentation file auto-approved",
            "suppressOutput": True
        }
        print(json.dumps(output))
        sys.exit(0)

sys.exit(0)
```

Configure it:
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read(*)",
        "hooks": [
          {
            "type": "command",
            "command": "python3 ~/.claude/scripts/auto-approve-docs.py"
          }
        ]
      }
    ]
  }
}
```

## Hooks with MCP Tools

Hooks work with MCP tools using the pattern `mcp__servername__toolname`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "mcp__github__create_issue",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Issue created' | /usr/bin/notify-send 'Claude'"
          }
        ]
      }
    ]
  }
}
```

## Hook Security

**Important warnings:**

Hooks execute arbitrary shell commands with your user permissions. Malicious or poorly written hooks can:
- Delete files
- Expose secrets
- Damage your system

Safety practices:
- Quote shell variables: Use `"$VAR"` not `$VAR`
- Validate inputs in complex hooks
- Use absolute paths for scripts
- Skip sensitive files (.env, secrets, keys)
- Test hooks carefully before relying on them

## Reviewing Hook Changes

When hooks in settings files change, Claude requires review:

```
/hooks
```

This shows pending hook changes. You must approve them before they take effect. This prevents session hijacking through modified hooks.

## Hook Execution Details

**Timeout:** 60 seconds by default (configurable)  
**Parallelization:** All matching hooks run in parallel  
**Deduplication:** Identical hook commands are deduplicated  

## Combining Hooks

Multiple hooks can match the same event:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          { "type": "command", "command": "black $CLAUDE_FILE_PATH" },
          { "type": "command", "command": "isort $CLAUDE_FILE_PATH" },
          { "type": "command", "command": "mypy $CLAUDE_FILE_PATH" }
        ]
      }
    ]
  }
}
```

All three run in parallel after any Python file is written.

## The Power of Automation

Hooks remove manual steps:
- No more running formatters manually
- No more forgetting to lint
- Automatic test runs catch issues immediately
- File protection prevents accidents

Start with simple hooks (formatting), then add more as you identify repetitive steps in your workflow.
