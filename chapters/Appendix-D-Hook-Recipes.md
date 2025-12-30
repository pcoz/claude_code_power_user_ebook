# Appendix D: Hook Recipes

Ready-to-use hook configurations for common tasks.

## Code Formatting

### Python with Black
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          { "type": "command", "command": "black \"$CLAUDE_FILE_PATH\"" }
        ]
      }
    ]
  }
}
```

### Python with Black + isort + mypy
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          { "type": "command", "command": "black \"$CLAUDE_FILE_PATH\"" },
          { "type": "command", "command": "isort \"$CLAUDE_FILE_PATH\"" },
          { "type": "command", "command": "mypy \"$CLAUDE_FILE_PATH\" --ignore-missing-imports" }
        ]
      }
    ]
  }
}
```

### JavaScript/TypeScript with Prettier
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.js)",
        "hooks": [
          { "type": "command", "command": "prettier --write \"$CLAUDE_FILE_PATH\"" }
        ]
      },
      {
        "matcher": "Write(*.ts)",
        "hooks": [
          { "type": "command", "command": "prettier --write \"$CLAUDE_FILE_PATH\"" }
        ]
      },
      {
        "matcher": "Write(*.tsx)",
        "hooks": [
          { "type": "command", "command": "prettier --write \"$CLAUDE_FILE_PATH\"" }
        ]
      }
    ]
  }
}
```

### ESLint Auto-fix
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.js)",
        "hooks": [
          { "type": "command", "command": "eslint --fix \"$CLAUDE_FILE_PATH\"" }
        ]
      }
    ]
  }
}
```

### Go Format
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.go)",
        "hooks": [
          { "type": "command", "command": "gofmt -w \"$CLAUDE_FILE_PATH\"" }
        ]
      }
    ]
  }
}
```

### Rust Format
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.rs)",
        "hooks": [
          { "type": "command", "command": "rustfmt \"$CLAUDE_FILE_PATH\"" }
        ]
      }
    ]
  }
}
```

## Testing

### Run Related Tests After Python Changes
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(src/*.py)",
        "hooks": [
          { "type": "command", "command": "pytest \"${CLAUDE_FILE_PATH%.py}_test.py\" -v 2>/dev/null || true" }
        ]
      }
    ]
  }
}
```

### Run Jest Tests After JS Changes
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(src/*.ts)",
        "hooks": [
          { "type": "command", "command": "npx jest --findRelatedTests \"$CLAUDE_FILE_PATH\" --passWithNoTests" }
        ]
      }
    ]
  }
}
```

## File Protection

### Prevent Writing to Sensitive Files
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write(.env*)",
        "hooks": [
          { "type": "command", "command": "echo 'BLOCKED: Cannot modify .env files' >&2 && exit 1" }
        ]
      },
      {
        "matcher": "Write(**/secrets/*)",
        "hooks": [
          { "type": "command", "command": "echo 'BLOCKED: Cannot modify secrets' >&2 && exit 1" }
        ]
      }
    ]
  }
}
```

### Prevent Destructive Commands
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(rm -rf *)",
        "hooks": [
          { "type": "command", "command": "echo 'BLOCKED: Destructive command' >&2 && exit 1" }
        ]
      },
      {
        "matcher": "Bash(sudo *)",
        "hooks": [
          { "type": "command", "command": "echo 'BLOCKED: sudo not allowed' >&2 && exit 1" }
        ]
      }
    ]
  }
}
```

## Logging and Notifications

### Log All File Operations
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*)",
        "hooks": [
          { "type": "command", "command": "echo \"$(date '+%Y-%m-%d %H:%M:%S') WRITE $CLAUDE_FILE_PATH\" >> ~/.claude/file-operations.log" }
        ]
      },
      {
        "matcher": "Edit(*)",
        "hooks": [
          { "type": "command", "command": "echo \"$(date '+%Y-%m-%d %H:%M:%S') EDIT $CLAUDE_FILE_PATH\" >> ~/.claude/file-operations.log" }
        ]
      }
    ]
  }
}
```

### Desktop Notification on Completion (macOS)
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "*",
        "hooks": [
          { "type": "command", "command": "osascript -e 'display notification \"Task completed\" with title \"Claude Code\"'" }
        ]
      }
    ]
  }
}
```

### Desktop Notification (Linux)
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "*",
        "hooks": [
          { "type": "command", "command": "notify-send 'Claude Code' 'Task completed'" }
        ]
      }
    ]
  }
}
```

## Git Integration

### Auto-Stage After Writes
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*)",
        "hooks": [
          { "type": "command", "command": "git add \"$CLAUDE_FILE_PATH\" 2>/dev/null || true" }
        ]
      }
    ]
  }
}
```

### Verify Clean Working Directory Before Edits
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit(*)",
        "hooks": [
          { "type": "command", "command": "git diff --quiet \"$CLAUDE_FILE_PATH\" 2>/dev/null || (echo 'WARNING: File has uncommitted changes' >&2)" }
        ]
      }
    ]
  }
}
```

## Combined Configuration

A production-ready configuration combining multiple hooks:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write(.env*)",
        "hooks": [
          { "type": "command", "command": "echo 'BLOCKED: Cannot modify .env files' >&2 && exit 1" }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          { "type": "command", "command": "black \"$CLAUDE_FILE_PATH\"" },
          { "type": "command", "command": "isort \"$CLAUDE_FILE_PATH\"" }
        ]
      },
      {
        "matcher": "Write(*.ts)",
        "hooks": [
          { "type": "command", "command": "prettier --write \"$CLAUDE_FILE_PATH\"" },
          { "type": "command", "command": "eslint --fix \"$CLAUDE_FILE_PATH\"" }
        ]
      },
      {
        "matcher": "Write(*)",
        "hooks": [
          { "type": "command", "command": "echo \"$(date '+%Y-%m-%d %H:%M:%S') $CLAUDE_FILE_PATH\" >> ~/.claude/writes.log" }
        ]
      }
    ]
  }
}
```

## Notes

- Hooks run in parallel by default
- Use `exit 1` in PreToolUse hooks to block the operation
- Quote `$CLAUDE_FILE_PATH` to handle spaces in filenames
- Use `2>/dev/null || true` to suppress errors for optional commands
- Test hooks carefully before relying on them
- Review hooks with `/hooks` command before approving changes

> **See Also:**
> - [Hooks: Automation Triggers](10-Hooks-Automation-Triggers.md) for hook fundamentals
> - [Writing Claude Code Plugins](36-Writing-Claude-Code-Plugins.md) for packaging hooks as plugins

---

**Next:** [Appendix E: Troubleshooting](Appendix-E-Troubleshooting.md) — Solutions to common problems.
