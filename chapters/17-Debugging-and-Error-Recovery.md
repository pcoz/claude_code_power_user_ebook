# Chapter 17: Debugging and Error Recovery

Things break. Claude Code turns debugging from frustration into conversation.

## The Debugging Advantage

With traditional AI chat: You see an error, copy it, paste it, explain context, get suggestions, copy code, paste it, try again.

With Claude Code: Claude sees the error directly, examines the code, understands the context, and can test fixes immediately.

## Basic Error Debugging

When something fails:

```
Run npm test
```

Claude sees the error output directly. No copying needed.

```
Fix the failing test
```

Claude analyzes the error, identifies the problem, proposes a fix, and you can verify immediately.

## Sharing Error Context

When you encounter an error outside Claude:

```
I'm seeing this error when I run the app:

TypeError: Cannot read property 'id' of undefined
    at getUserData (/src/services/user.ts:45:23)
    at processRequest (/src/handlers/api.ts:78:12)

Diagnose and fix it.
```

Claude traces through the stack, examines the files, and identifies the issue.

## The Diagnostic Pattern

For complex bugs, use systematic diagnosis:

```
Debug this issue systematically:
1. What does the error message tell us?
2. What file and line is the problem?
3. What conditions could cause this?
4. How can we verify each possibility?
```

Claude walks through the analysis, not just guessing at fixes.

## Adding Instrumentation

When the cause isn't obvious:

```
Add logging to the authentication flow so we can see:
- When a request arrives
- What user data is extracted
- Each step of token validation
- Where it fails
```

Run again:

```
Run the login test and show me the logs
```

The logs reveal where things go wrong.

## Interactive Debugging Sessions

```
Let's debug this interactively. I'll describe what happens, 
you suggest what to check next.

When I click the Submit button, nothing happens. No error 
in the console. What should I check first?
```

Claude guides you through the debugging process.

## Visual Debugging

For UI issues, share screenshots:

1. Take screenshot (Cmd+Shift+4 on Mac, to clipboard with Ctrl+Shift+4)
2. Paste in Claude (Ctrl+V)

```
[pasted screenshot]

The dropdown menu is appearing behind the modal. 
Fix the z-index issue.
```

Claude sees exactly what you see.

## Database Debugging

```
The user query is returning empty results but there's data 
in the database. Debug this.

Show me:
1. The actual SQL being generated
2. The database contents for the test user
3. Where the mismatch occurs
```

## Network Debugging

```
The API call to /users is failing. Debug:
1. What request is being sent?
2. What headers are included?
3. What response comes back?
4. Where does the handling fail?
```

## The Infinite Loop Problem

Sometimes Claude gets stuck trying the same fix repeatedly.

**Signs:**
- Same error appears multiple times
- Claude keeps suggesting similar solutions
- No progress after several attempts

**Solutions:**

```
Stop. Let's step back.

We've tried fixing the validation three times. 
Explain what you think is happening and why 
the previous fixes didn't work.
```

Or:

```
Let's try a completely different approach. 
What other ways could we implement this feature?
```

Or start a fresh session with lessons learned.

## The "Why" Question

Don't just accept fixes. Understand them:

```
That fixed it. But why was it broken? 
What caused the original issue?
```

Understanding prevents future bugs.

## Regression Debugging

When something that worked stops working:

```
The login feature was working yesterday but is broken today.

1. Check git history for recent changes to auth files
2. Identify what changed
3. Determine which change caused the regression
```

Claude uses git to find what changed.

## Environment Issues

```
This works on my machine but fails in production/CI/Docker.

Compare:
1. Node versions
2. Environment variables
3. Package versions
4. File permissions
```

Claude investigates environmental differences.

## Memory/Performance Debugging

```
The app slows down after running for a while. 
Potential memory leak.

1. Add memory usage logging
2. Identify what grows over time
3. Find the leak source
```

## Error Recovery Patterns

### Git Recovery

```
# Undo uncommitted changes
git checkout -- .

# Undo last commit
git reset --soft HEAD~1

# Emergency: reset to remote state
git fetch origin
git reset --hard origin/main
```

Claude can help with these:

```
I need to undo the last 3 commits but keep the changes 
as uncommitted files. Help me do this safely.
```

### File Recovery

Claude Code creates checkpoints:

```
/checkpoint list
/checkpoint restore <id>
```

### Database Recovery

```
I accidentally deleted data. Help me recover:
1. Check if there's a recent backup
2. If not, check database logs for the DELETE statement
3. Construct an INSERT to restore the data
```

## Debugging Subagent

Create a specialized debugging agent:

```markdown
# .claude/agents/debugger.md

---
name: debugger
description: Use for complex debugging that requires deep analysis
model: opus
tools:
  - Read
  - Grep
  - Glob
  - Bash
---

You are an expert debugger. For each issue:

1. Gather evidence (logs, stack traces, state)
2. Form hypotheses about the cause
3. Test each hypothesis systematically
4. Identify root cause, not just symptoms
5. Propose fix with explanation
6. Suggest prevention measures
```

Invoke it:

```
Use the debugger agent to analyze why webhooks 
are being processed multiple times.
```

## Prevention

After fixing a bug:

```
How can we prevent this type of bug in the future?
Should we add:
- Validation?
- Tests?
- Type checks?
- Documentation?
```

The best debugging is the debugging you don't have to do.

## Debugging Checklist

When stuck:

1. ☐ Can you reproduce the bug consistently?
2. ☐ Do you have the full error message and stack trace?
3. ☐ Have you added logging to trace execution?
4. ☐ Have you checked recent changes (git diff)?
5. ☐ Have you verified the data/state at each step?
6. ☐ Have you tried a minimal reproduction?
7. ☐ Have you considered environmental differences?
8. ☐ Have you asked Claude to explain what the code does?

Work through the list systematically. Most bugs reveal themselves by step 5.

> **See Also:**
> - [Divide and Conquer](37-Divide-and-Conquer.md) for breaking down complex bugs
> - [The Development Loop](14-The-Development-Loop.md) for the recovery pattern
> - [Subagents](11-Subagents-Parallel-Intelligence.md) for creating specialized debugging agents

---

**Next:** [Chapter 18: Code Review Workflows](18-Code-Review-Workflows.md) — Systematic code review with AI assistance.
