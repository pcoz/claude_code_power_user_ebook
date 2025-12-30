# Chapter 4: The Permission Model

Claude Code can read your files, write new ones, and execute shell commands. That's powerful—and potentially dangerous. The permission model keeps you in control.

## The Default: Ask Before Acting

By default, Claude asks permission before:

- Creating files
- Modifying files
- Running shell commands
- Using MCP tools

You see exactly what Claude wants to do, then approve or reject.

```
I'll create a new file for the user authentication module.

Create file: src/auth/login.ts
-------------------------------
import { hash, compare } from 'bcrypt';
...
-------------------------------

Allow this action? (y/n/e)
```

Options:
- `y` — Yes, proceed
- `n` — No, reject this action
- `e` — Edit the proposed content before accepting

## Permission Modes

You can adjust how much Claude asks:

### Manual Mode (Default)
```bash
claude --permission-mode manual
```
Every action requires approval. Safest option.

### Accept Edits Mode
```bash
claude --permission-mode acceptEdits
```
Auto-approves file reads and writes. Still asks for shell commands.

Good for focused coding sessions where you trust the file operations.

### Accept All Mode
```bash
claude --permission-mode acceptAll
```
Auto-approves everything. Use only when you fully trust the task.

⚠️ **Warning**: This mode lets Claude run any command without asking. Use in isolated environments only.

### Dangerously Skip Permissions
```bash
claude --dangerously-skip-permissions
```
Maximum autonomy. Claude acts without any approval prompts.

Only use for:
- Isolated VMs or containers
- Fully trusted, low-risk tasks
- When you're watching the output closely

## Tool-Specific Permissions

Fine-grained control with `--allowedTools` and `--deniedTools`:

### Allow Specific Tools
```bash
# Only allow reading and searching
claude --allowedTools "Read,Grep,Glob"

# Allow git commands only
claude --allowedTools "Bash(git *)"

# Allow multiple specific patterns
claude --allowedTools "Read,Write,Bash(npm *),Bash(git *)"
```

### Deny Specific Tools
```bash
# Prevent destructive commands
claude --deniedTools "Bash(rm *),Bash(sudo *)"

# Prevent writing to sensitive files
claude --deniedTools "Write(.env*),Write(*.pem)"
```

## Permission Patterns

The pattern syntax:

| Pattern | Matches |
|---------|---------|
| `Read` | All file reads |
| `Read(*.py)` | Python files only |
| `Read(src/*)` | Files in src/ directory |
| `Bash` | All shell commands |
| `Bash(git *)` | Commands starting with "git" |
| `Bash(npm run *)` | npm run commands |
| `mcp__github__*` | All GitHub MCP tools |

### Examples

```bash
# Read-only exploration
claude --allowedTools "Read,Grep,Glob"

# Development with tests
claude --allowedTools "Read,Write,Edit,Bash(npm test),Bash(npm run *)"

# Git operations only
claude --allowedTools "Read,Bash(git *)"

# Full access except destructive commands
claude --deniedTools "Bash(rm -rf *),Bash(sudo *),Write(.env*)"
```

## Settings-Based Permissions

Configure default permissions in settings files:

**~/.claude/settings.json** (global):
```json
{
  "permissions": {
    "allowedTools": ["Read", "Write", "Bash(git *)"],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Write(./secrets/*)",
      "Bash(rm -rf *)"
    ]
  }
}
```

**./claude/settings.json** (project):
```json
{
  "permissions": {
    "allowedTools": ["Read", "Write", "Edit", "Bash(npm *)"],
    "deny": ["Write(./migrations/*)"]
  }
}
```

Project settings override global settings.

## The Approval Flow

When Claude proposes an action:

1. **Review the action** — What tool? What parameters?
2. **Check the content** — For file writes, review the code
3. **Consider the impact** — Safe or risky?
4. **Decide** — Approve, reject, or modify

### Smart Review Habits

You don't need to read every line. Focus on:

- **File paths** — Is it touching the right files?
- **Destructive operations** — Any deletes or overwrites?
- **External calls** — Network requests, API calls?
- **Sensitive data** — Credentials, keys, secrets?

Skim the rest. Trust but verify.

## Bulk Approvals

For repetitive approvals, Claude offers bulk options:

```
Allow all file writes this session? (y/n)
```

Accept when you're confident in the task. The approval persists for the session only.

## Hooks for Auto-Approval

Advanced users can create hooks that auto-approve certain operations:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read(*.md)",
        "hooks": [{
          "type": "command",
          "command": "python ~/.claude/scripts/auto-approve-docs.py"
        }]
      }
    ]
  }
}
```

See [Chapter 10: Hooks](10-Hooks-Automation-Triggers.md) for details.

## Safety Best Practices

1. **Start restrictive** — Begin with manual mode, loosen as you build trust

2. **Use read-only for exploration** — When just browsing code:
   ```bash
   claude --allowedTools "Read,Grep,Glob"
   ```

3. **Protect sensitive files** — Always deny access to:
   - `.env` and environment files
   - Private keys and certificates
   - Production configuration

4. **Isolate risky operations** — Use VMs or containers for:
   - Untrusted code
   - System-level changes
   - `--dangerously-skip-permissions`

5. **Review shell commands carefully** — File operations are usually safe; shell commands can do anything

## The Trust Gradient

Think of permissions as a gradient:

```
Maximum Safety                              Maximum Speed
     |                                           |
     v                                           v
  manual → acceptEdits → acceptAll → skip-permissions
```

Move right as you build confidence in:
- The specific task
- Your environment's isolation
- Your ability to recover from mistakes

Most daily work happens in `manual` or `acceptEdits` mode. Reserve the faster modes for specific situations.

## Recovery

Made a mistake? Options:

- **Git**: `git checkout -- file` or `git reset --hard`
- **Undo in editor**: Most editors have undo history
- **Checkpoints**: Claude Code creates checkpoints you can restore

The permission model is your first line of defense. Git is your second.

---

**Next:** [Chapter 5: CLAUDE.md — Your Project's Brain](05-CLAUDE-md-Your-Projects-Brain.md) — Learn how to give Claude deep context about your project.
