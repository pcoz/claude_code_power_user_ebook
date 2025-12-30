# Chapter 14: The Development Loop

Effective Claude Code usage follows a rhythm. Understanding this loop helps you work faster and produce better results.

## The Core Loop

```
    +----------------------------------------------------------+
    |                                                          |
    |   DESCRIBE --> REVIEW --> APPROVE --> VERIFY --> REFINE  |
    |       ^                                            |     |
    |       +--------------------------------------------+     |
    |                                                          |
    +----------------------------------------------------------+
```

| Step | What You Do |
|------|-------------|
| **Describe** | Tell Claude what you want |
| **Review** | Read Claude's proposal |
| **Approve** | Accept, reject, or modify |
| **Verify** | Test that it works |
| **Refine** | Adjust as needed |

This loop repeats until the task is complete.

## Scoping Before Building

The most common mistake: jumping straight to building.

Claude is fast. It can generate hundreds of lines in seconds. If those are the wrong hundred lines, you've created confusion.

**Better approach:**

```
Before writing any code, outline what you're going to build 
and how. Include:
- Files you'll create or modify
- Key functions and their purposes
- Any external dependencies
- Testing approach
```

Claude responds with a plan. You review. Catch misunderstandings before they become code.

## Incremental Development

Build in small steps:

```
Step 1: Create the database schema for users
[Review, approve, verify]

Step 2: Add the User model with validation
[Review, approve, verify]

Step 3: Create the registration API endpoint
[Review, approve, verify]

Step 4: Add the login endpoint with JWT
[Review, approve, verify]
```

Each step is small enough to review thoroughly. Errors are caught early.

## The Verification Step

Don't skip verification. After Claude makes changes:

```
Run the tests
```

Or:
```
Start the server and test the endpoint with curl
```

Or:
```
Show me what the output looks like with sample data
```

Claude can verify its own work. Let it.

## Reading Without Reading Everything

As projects grow, you can't review every line. Here's what to focus on:

### Always Check:
- File paths (is it modifying the right files?)
- Destructive operations (deletes, overwrites)
- External calls (APIs, network, database)
- Security-sensitive code (auth, permissions, secrets)

### Skim:
- Import statements
- Boilerplate setup
- Standard patterns you've seen before

### Skip (usually):
- Formatting changes
- Comment additions
- Test boilerplate

Trust, but verify the important parts.

## When Things Break

Things will break. This is normal.

**The Recovery Pattern:**

1. **Stop** — Don't make rapid random changes
2. **Observe** — What's the exact error?
3. **Share** — Tell Claude what happened (it may have seen it already)
4. **Diagnose** — Let Claude analyze
5. **Fix** — One change at a time
6. **Verify** — Did it help?

```
I'm seeing this error: [paste error]

Let's debug this systematically. What are the possible causes?
```

## Adding Logging

When problems are invisible:

```
Add logging to the authentication flow so we can see 
what's happening at each step
```

Run again. Now you can see where things go wrong.

## Screenshots for Visual Problems

Building a UI and something looks wrong?

1. Take a screenshot (Cmd+Shift+4 on Mac, save to clipboard)
2. Paste into Claude (Ctrl+V, not Cmd+V)

```
[paste screenshot]
The button is in the wrong position. It should be 
aligned with the form field above it.
```

Claude sees the visual issue and can fix it.

## The "Actually" Pattern

You'll often approve something, then realize it's not quite right. This is fine.

```
Actually, I want the dates in DD/MM/YYYY format, not MM/DD/YYYY

Actually, make that function async

Actually, use a class instead of separate functions
```

Claude adjusts. There's no penalty for changing your mind.

## Conversation Momentum

Conversations accumulate context. By message 50, Claude has a lot of history about your project.

**Usually good:** Claude understands decisions and patterns.

**Sometimes bad:** Old context becomes confusing or contradictory.

When context gets muddled:

```
/clear
```

Or start a fresh session. Sometimes a clean slate helps.

## Checkpoints

Claude Code creates checkpoints automatically during long sessions. If something goes wrong:

```
/checkpoint list
/checkpoint restore <id>
```

Git also serves as a checkpoint system:

```
git stash
# or
git checkout -- .
```

## The Refinement Mindset

Building software isn't writing code once. It's:

```
Build → Test → Fix → Build → Test → Fix → ...
```

With Claude Code, each iteration is fast. Embrace refinement as the process, not a sign of failure.

## Knowing When You're Done

A task is "done" when it solves the actual problem. Not when it has every feature imaginable.

Before starting, define "done":

```
This feature is complete when:
- Users can reset their password via email
- The reset link expires after 1 hour
- Invalid tokens show a clear error message
- There are tests for the happy path and error cases
```

Stop when you meet those criteria.

## Session Discipline

### Starting a Session

1. Navigate to project directory
2. Start Claude
3. Provide context (or let CLAUDE.md do it)
4. State your goal clearly

### During a Session

- Stay focused on one task
- Verify after each significant change
- Use `/compact` if context grows large
- Take breaks—fresh eyes catch issues

### Ending a Session

1. Verify everything works
2. Commit your changes
3. Update CLAUDE.md if needed
4. Note any TODOs for next session

## The Rhythm

Fast:
```
Quick question → Quick answer → Move on
```

Medium:
```
Describe feature → Plan → Implement → Test → Refine
```

Complex:
```
Explore problem → Understand → Plan → Build incrementally → 
Verify each step → Review complete → Polish
```

Match your rhythm to the task complexity.

## Pro Tips

**Be specific early.** Vague first messages lead to wasted iteration.

**Show examples.** "Like this: [example]" is clearer than lengthy descriptions.

**Reference files.** `@src/auth/login.ts` gives Claude exact context.

**Ask questions.** "Why did you do it that way?" helps you learn.

**Let Claude verify.** "Run the tests and tell me if they pass."

The development loop isn't magic—it's just a faster version of how experienced developers already work. Describe, review, verify, refine. Repeat until done.
