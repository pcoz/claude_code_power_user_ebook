# Chapter 5: CLAUDE.md — Your Project's Brain

CLAUDE.md is the single most important file for Claude Code power users. It's a special file that Claude automatically reads at the start of every session, giving it deep context about your project without you having to explain anything.

Think of it as a briefing document. Every time Claude starts working in your project, it reads this file first. Everything you put here becomes part of Claude's understanding.

## The Hierarchy

Claude reads CLAUDE.md files from multiple locations, in order:

1. **Global**: `~/.claude/CLAUDE.md` — Applies to all your projects
2. **Project root**: `./CLAUDE.md` — Shared with your team, checked into git
3. **Subdirectories**: Claude reads CLAUDE.md from the current working directory and parent directories

This hierarchy lets you set universal preferences globally while adding project-specific context locally.

## What Goes in CLAUDE.md

The best CLAUDE.md files include:

### Project Context
```markdown
# MyApp

A customer portal for managing subscriptions and billing.
Built for internal use by our support team.

## Tech Stack
- Frontend: Next.js 14 with TypeScript
- Backend: Node.js with Express
- Database: PostgreSQL with Prisma
- Auth: NextAuth.js with Google OAuth
```

### Coding Standards
```markdown
## Coding Standards
- Use TypeScript for all new code
- Follow existing ESLint configuration
- Write tests for all new functions using Jest
- Use functional components with hooks in React
- Prefer named exports over default exports
```

### File Organization
```markdown
## File Organization
- Components in `src/components/`
- API routes in `src/pages/api/`
- Utilities in `src/lib/`
- Tests alongside source files with `.test.ts` extension
```

### Current State
```markdown
## Current State
- User authentication: COMPLETE
- Subscription management: IN PROGRESS
- Billing integration: NOT STARTED

## Active Work
Currently implementing the subscription tier selection UI.
The API endpoints are ready in `src/pages/api/subscriptions/`.
```

### Key Decisions
```markdown
## Key Decisions
- Using Stripe for payments (not PayPal) because of better API
- SQLite for development, PostgreSQL for production
- No Redux—using React Query for server state, Zustand for client state
```

## The Token Economy

CLAUDE.md content uses tokens. Every word counts against your context limit. This means:

**Do include:**
- Information Claude needs frequently
- Patterns you want Claude to follow consistently
- Context that would otherwise require repeated explanation

**Don't include:**
- Documentation Claude could look up
- One-time instructions (just say them in chat)
- Verbose explanations when terse ones work

A good CLAUDE.md is comprehensive but not bloated. Aim for the minimum context needed to get correct first-attempt results.

## A Complete Example

```markdown
# Expense Tracker

Personal expense tracking web application.

## Stack
- Python 3.11 + Flask
- SQLite (expenses.db)
- Vanilla JavaScript frontend
- No external CSS framework

## Structure
- app.py: Main Flask application
- templates/: Jinja2 templates
- static/: CSS and JavaScript
- models.py: SQLAlchemy models

## Conventions
- Use snake_case for Python, camelCase for JavaScript
- All API endpoints return JSON
- Error responses include {"error": "message"}
- Dates in ISO 8601 format (YYYY-MM-DD)

## Current Work
Adding category-based filtering. The Category model
exists but the filter UI needs to be built.

## Running the Project
flask run --debug
```

## Dynamic Content

Some teams include dynamic information:

```markdown
## Recent Changes
- 2024-12-28: Added user preferences API
- 2024-12-27: Fixed timezone handling in reports
- 2024-12-26: Migrated to new auth provider

## Known Issues
- #142: Export times out for large date ranges
- #139: Mobile layout breaks on small screens
```

Update this at the end of each session to maintain continuity.

## Global CLAUDE.md

Your global CLAUDE.md (`~/.claude/CLAUDE.md`) should contain preferences that apply everywhere:

```markdown
# Global Preferences

## Communication Style
- Be concise in explanations
- Show code examples, not just descriptions
- When uncertain, ask before assuming

## Code Style
- Prefer functional programming patterns
- Add comments for non-obvious logic
- Use meaningful variable names

## Tools
- I use VS Code as my editor
- I prefer npm over yarn
- I use zsh with oh-my-zsh
```

## Updating CLAUDE.md

At the end of a productive session:

```
Update CLAUDE.md to reflect what we accomplished today
and what the next steps are.
```

Claude will update the file, keeping future sessions informed.

## Common Mistakes

**Too verbose.** If your CLAUDE.md is 500 lines, you're using too many tokens. Be concise.

**Static content.** If "Current State" hasn't been updated in weeks, it's misleading. Keep it current or remove it.

**Duplicate information.** Don't repeat what's obvious from code or standard for the framework. Claude knows how React works.

**Instructions in the wrong place.** One-time requests belong in chat, not CLAUDE.md. Permanent patterns belong in CLAUDE.md, not chat.

## The Payoff

A well-crafted CLAUDE.md means:
- Fewer corrections (Claude follows your patterns from the start)
- Less context-setting in each session
- Consistency across team members
- Faster onboarding for new contributors who read the file

Five minutes of documentation saves hours of correction. Make CLAUDE.md work for you.
