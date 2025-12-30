# Chapter 27: When You Need Documentation

Most of this book has focused on building quickly. Start a session, describe what you want, iterate until it works.

This works beautifully for scripts and small programs. But as projects grow, undocumented work hits limits.

## Signs You Need Structure

**You've had multiple conversations about the same project and they contradict each other.** Without documentation, each session starts fresh. Claude might make different decisions than before.

**You can't remember why you made certain decisions.** Was there a reason you used SQLite instead of PostgreSQL? Why is the auth module structured that way? Without records, you'll waste time rediscovering rationale.

**Features you add break features you built earlier.** Without understanding of the whole system, changes in one place cause problems elsewhere.

**You spend more time explaining context than building.** If every session starts with a 10-minute explanation, that's 10 minutes wasted.

**The project has grown beyond what fits in your head.** There's no shame in this—it's natural. But it requires adaptation.

## What Documentation Provides

**Coherence.** Documented decisions prevent contradictions across sessions.

**Continuity.** When you return after a week (or month), documentation tells you where things stand.

**Communication.** If others join the project, documentation brings them up to speed.

**Quality.** Thinking through requirements before building catches problems early.

## When You Don't Need Documentation

**For a one-file script**, documentation is overkill. Just build it.

**For a tool only you'll use once**, skip the ceremony.

**For exploration and experimentation**, don't document—discover.

The overhead of documentation only pays off when:
- Projects are large enough to forget
- Projects span multiple sessions
- Multiple components interact
- Others might work on the code

## The Minimum Viable Documentation

If you write nothing else, create a CLAUDE.md:

```markdown
# Project Name

## What This Is
One sentence description of what the project does.

## Current State
- What works
- What's in progress
- What's planned

## Tech Stack
- Language/framework
- Database
- Key libraries

## How to Run
The exact commands to start the project.
```

Update this at the end of each session. Five minutes saves hours of confusion.

## Scaling Documentation

As projects grow, add:

**Architecture documentation.** How do the pieces fit together? What talks to what?

**Decision records.** Why did you choose X over Y? What were the tradeoffs?

**API documentation.** For internal and external APIs.

**Setup instructions.** How does a new person get the project running?

But add these only when you feel the pain of not having them. Don't document preemptively.

## Documentation as Code

Keep documentation close to code:

```
project/
+-- README.md           # Project overview
+-- CLAUDE.md           # AI context file
+-- docs/
|   +-- architecture.md
|   +-- api.md
|   +-- decisions/
|       +-- 001-database-choice.md
|       +-- 002-auth-approach.md
+-- src/
+-- ...
```

Documentation in the repo:
- Stays synchronized with code
- Goes through version control
- Gets reviewed with code changes

## Claude as Documentation Writer

Claude can help create documentation:

```
Create an architecture document that explains:
- System components and their responsibilities
- How data flows between components
- External dependencies
- Deployment topology

Base it on the actual code structure.
```

```
Document the API endpoints:
- List all routes
- Request/response formats
- Authentication requirements
- Example calls

Generate from the actual route handlers.
```

## The Documentation Mindset

Documentation is not bureaucracy. It's memory.

Human memory fades. Claude's context resets between sessions. Documentation persists.

Write documentation for your future self—the person who will return to this code in three months and wonder what's going on.

Keep it current. Outdated documentation is worse than no documentation—it misleads.

Keep it minimal. Document what matters. Skip what's obvious from the code.

## When to Document

**Document decisions when you make them.** "We chose SQLite because..." is easy to write in the moment, hard to reconstruct later.

**Document architecture before building.** Even a sketch helps clarify thinking.

**Document APIs as you build them.** Add documentation alongside the implementation.

**Document bugs and their fixes.** Future debugging benefits from historical context.

## The Real Test

If you can answer "yes" to these questions, you have enough documentation:

1. Can someone new understand what the project does?
2. Can they run it without asking you?
3. Can you return after a month and continue working?
4. Does Claude understand the project from CLAUDE.md?

If yes to all four, you're in good shape. If not, fill the gaps.

Documentation is an investment. The cost is time now. The return is time saved later—often 10x or more.

> **See Also:**
> - [CLAUDE.md: Your Project's Brain](05-CLAUDE-md-Your-Projects-Brain.md) for essential project documentation
> - [The Document Stack](28-The-Document-Stack.md) for comprehensive documentation patterns
> - [A Complete Structured Build](29-A-Complete-Structured-Build.md) for documentation in practice

---

**Next:** [Chapter 28: The Document Stack](28-The-Document-Stack.md) — Layer your documentation for maximum clarity.
