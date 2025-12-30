# Chapter 24: Large Codebase Navigation

Small projects fit in Claude's context window easily. Large codebases—tens of thousands of files—require strategy.

## The Context Limit

Claude can't read your entire codebase at once. With a large project, you need to:

1. Give Claude the right context for each task
2. Let Claude explore and discover relevant code
3. Use tools that search rather than load everything

## Agentic Search

Claude Code uses **agentic search**—it explores your codebase dynamically using tools like grep, find, and glob. Just like a human developer would.

```
Find where user authentication is implemented in this codebase.
```

Claude doesn't need the whole codebase. It searches:
```bash
grep -r "authenticate" --include="*.py"
grep -r "login" --include="*.ts"
find . -name "*auth*" -type f
```

Then reads the relevant files.

## Starting in a New Codebase

When you're new to a large project:

```
I just joined this project. Give me an overview:
- What does this application do?
- What's the tech stack?
- What's the directory structure?
- Where are the main entry points?
```

Claude explores and summarizes.

## Finding Specific Code

### By Functionality

```
Find the code that handles payment processing.
```

### By Pattern

```
Find all files that use the database connection.
```

### By String

```
Find where "ERROR_INVALID_TOKEN" is defined and used.
```

### By Symptom

```
Users report slow page loads on the dashboard.
Find the dashboard code and identify potential bottlenecks.
```

## Understanding Code Flow

```
Trace what happens when a user clicks "Submit Order":
1. Start from the frontend button handler
2. Follow through the API call
3. Track the backend processing
4. Show how the database is updated
```

Claude follows the flow, reading files as needed.

## Using Subagents for Exploration

For very large codebases, use parallel subagents:

```
Explore this codebase using 4 subagents:
1. Explore the frontend (src/client/)
2. Explore the backend (src/server/)
3. Explore the shared utilities (src/shared/)
4. Explore the test structure (tests/)

Each should summarize what they find.
Then synthesize into an overall picture.
```

## Building a Mental Map

Create documentation as you explore:

```
Create a CODEBASE.md file documenting:
- High-level architecture
- Key directories and their purposes
- Important files and what they do
- Data flow between components
- External dependencies
```

This helps both you and future Claude sessions.

## Common Patterns in Large Codebases

### Monorepos

```
This is a monorepo with multiple packages.
List all packages and their dependencies on each other.
```

### Microservices

```
Identify all the services in this project.
For each, tell me: name, purpose, how it communicates with others.
```

### Legacy Code

```
This is a legacy codebase with mixed patterns.
Identify the different coding styles/eras present.
Note which parts are well-tested vs untested.
```

## Efficient Context Management

### Using @mentions

Reference specific files:
```
Look at @src/auth/login.ts and @src/api/users.ts.
How do they work together?
```

### Using /add-dir

Add directories to persistent context:
```
/add-dir src/core
```

Now Claude always has access to core modules.

### Using /compact

When context gets full:
```
/compact
```

Claude summarizes the conversation, freeing space.

## The CLAUDE.md Strategy

For large codebases, CLAUDE.md is essential:

```markdown
# MyLargeCorp Application

## Quick Navigation
- Frontend: src/client/ (React)
- Backend: src/server/ (Node/Express)
- Database: src/db/ (PostgreSQL/Prisma)
- Shared types: src/types/

## Key Files
- src/server/index.ts: Main entry point
- src/server/routes/: API route handlers
- src/client/App.tsx: Root React component

## Architecture
[Brief description of how components interact]

## Common Tasks
- Adding a new API endpoint: see src/server/routes/
- Adding a new page: see src/client/pages/
- Database changes: create migration in prisma/
```

## When You're Lost

If Claude seems confused:

```
Let's step back. What files have you read in this session?
What do you understand about the codebase so far?
```

Sometimes starting fresh with targeted exploration works better than continuing a confused conversation.

## Performance Tips

**Be specific.** "Find the auth code" is faster than "tell me about security."

**Start narrow.** Explore one directory, then expand.

**Use grep.** Text search is faster than reading files.

**Summarize as you go.** Build understanding incrementally.

**Document findings.** Update CLAUDE.md with discoveries.

## Tools for Large Codebases

### Tree view
```
Show me the directory structure 2 levels deep.
```

### Code statistics
```
Count lines of code by file type.
What's the largest file?
```

### Dependency analysis
```
What packages does this project depend on?
Are there any outdated or vulnerable dependencies?
```

### Test coverage
```
Where are tests located?
Which parts of the codebase have tests?
Which parts don't?
```

Large codebases require patience and strategy. You won't understand everything in one session. Build understanding incrementally, document as you go, and let Claude's search capabilities do the heavy lifting.
