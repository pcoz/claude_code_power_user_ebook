# Chapter 6: Slash Commands and Custom Commands

Slash commands are shortcuts. Instead of typing out the same complex prompt every time, you define it once and invoke it with a simple `/command`.

This is where Claude Code transforms from a tool you use into a tool that works the way you work.

## Built-in Commands

Type `/` in a Claude Code session to see available commands:

| Command | What It Does |
|---------|-------------|
| `/help` | Show all available commands |
| `/clear` | Clear conversation history |
| `/compact` | Summarize and compress context |
| `/model` | Switch between models |
| `/cost` | Show token usage and costs |
| `/bug` | Report an issue to Anthropic |
| `/agents` | Manage subagent definitions |
| `/hooks` | Review and approve hook changes |
| `/mcp` | Configure MCP servers |
| `/add-dir` | Add directory to context |

The `/help` command shows all available commands, including your custom ones.

## Creating Custom Commands

Custom commands live in markdown files:

**Project-specific commands:** `.claude/commands/`  
**Personal commands (all projects):** `~/.claude/commands/`

Each file becomes a command. The filename (minus `.md`) is the command name.

### Simple Command

Create `.claude/commands/review.md`:

```markdown
Review this code for:
1. Potential bugs or edge cases
2. Performance issues
3. Security vulnerabilities
4. Code style and readability
5. Missing error handling

Be specific and actionable in your feedback.
```

Now `/project:review` is available. When you invoke it, Claude executes this prompt.

### Command with Arguments

Use `$ARGUMENTS` to pass parameters:

Create `.claude/commands/fix-issue.md`:

```markdown
Analyze and fix GitHub issue #$ARGUMENTS.

Follow these steps:
1. Use `gh issue view $ARGUMENTS` to get issue details
2. Understand the problem described
3. Search the codebase for relevant files
4. Implement the fix
5. Write tests to verify the fix
6. Create a descriptive commit message
```

Invoke with: `/project:fix-issue 142`

The `$ARGUMENTS` placeholder gets replaced with `142`.

### Documentation Header

Add a description for better discoverability:

```markdown
---
description: Fix a GitHub issue by number
---

Analyze and fix GitHub issue #$ARGUMENTS.
...
```

This description appears in `/help` output.

## Real-World Command Examples

### Code Review Command

`.claude/commands/review-pr.md`:
```markdown
---
description: Review the current PR for issues
---

Review this pull request thoroughly:

1. Check for logic errors and edge cases
2. Verify error handling is complete
3. Ensure tests cover new functionality
4. Check for security issues (SQL injection, XSS, etc.)
5. Verify code follows our style guide
6. Look for performance concerns

For each issue found, specify:
- File and line number
- What the problem is
- How to fix it

If the code looks good, say so briefly.
```

### Test Generator Command

`.claude/commands/generate-tests.md`:
```markdown
---
description: Generate comprehensive tests for a file
---

Generate tests for $ARGUMENTS:

1. Read the file and understand its functionality
2. Identify all public functions and methods
3. For each function, create tests for:
   - Normal operation with typical inputs
   - Edge cases (empty inputs, nulls, boundaries)
   - Error conditions
4. Use our existing test patterns (check /tests for examples)
5. Add appropriate mocks for external dependencies

Run the tests to verify they pass.
```

Usage: `/project:generate-tests src/utils/validation.ts`

### Commit Command

`.claude/commands/commit.md`:
```markdown
---
description: Create a well-formatted commit
---

Create a git commit for the current changes:

1. Run `git diff --staged` to see what's being committed
2. Analyze the changes to understand what was done
3. Write a commit message following this format:
   - First line: type(scope): brief description (50 chars max)
   - Blank line
   - Body: explain what and why (wrap at 72 chars)
4. Use conventional commit types: feat, fix, docs, style, refactor, test, chore
5. Execute the commit

Do not commit if there are no staged changes.
```

### Debug Command

`.claude/commands/debug.md`:
```markdown
---
description: Debug an error message
---

Debug this error: $ARGUMENTS

1. Parse the error message and stack trace
2. Identify the root cause
3. Search the codebase for the failing code
4. Explain what's happening and why
5. Propose a fix with code
6. If appropriate, implement the fix

If you need more context, ask before making changes.
```

Usage: `/project:debug "TypeError: Cannot read property 'id' of undefined"`

## Personal vs Project Commands

**Personal commands** (`~/.claude/commands/`) are yours alone:
- Preferences for how you like code written
- Shortcuts for your personal workflow
- Commands that don't make sense to share

**Project commands** (`.claude/commands/`) are shared:
- Commit to git so the team has them
- Encode team standards and workflows
- Ensure consistency across developers

## Command Organization

For large command libraries, use prefixes:

```
.claude/commands/
+-- review-code.md
+-- review-security.md
+-- review-perf.md
+-- gen-tests.md
+-- gen-docs.md
+-- fix-issue.md
+-- fix-lint.md
```

These appear as:
- `/project:review-code`
- `/project:review-security`
- `/project:gen-tests`
- etc.

## Combining Commands with Context

Commands can reference files:

```markdown
Review the code in @$ARGUMENTS for issues.
Focus on the patterns established in @src/utils/patterns.ts.
```

The `@` syntax tells Claude to read those files.

## Iterating on Commands

Commands are just markdown files. Edit them as you discover what works:

1. Start with a simple version
2. Use it several times
3. Notice what's missing or wrong
4. Update the command
5. Repeat

The best commands evolve from real usage.

## Pro Tips

**Be specific.** Vague commands get vague results. "Review this code" is worse than the detailed review command above.

**Include output format.** Tell Claude how to structure the response: lists, sections, code blocks.

**Reference project context.** Mention specific files or patterns from your project.

**Provide escape hatches.** "If you need more context, ask before making changes" prevents bad assumptions.

**Chain commands mentally.** You can invoke multiple commands in sequence: `/project:fix-issue 142` then `/project:generate-tests src/changed-file.ts`.

Commands are multipliers. Time spent crafting good commands pays back every time you use them.

> **See Also:**
> - [Code Review Workflows](18-Code-Review-Workflows.md) for systematic review commands
> - [Hooks](10-Hooks-Automation-Triggers.md) for triggering actions automatically
> - [Writing Plugins](36-Writing-Claude-Code-Plugins.md) for packaging commands for distribution

---

**Next:** [Chapter 7: Settings and Configuration](07-Settings-and-Configuration.md) — Customize Claude Code's behavior at every level.
