# Chapter 12: Skills — Automatic Expertise

Skills are knowledge packages that Claude applies automatically when relevant. Unlike commands (which you invoke explicitly), skills activate based on context—Claude reads their descriptions and decides when to use them.

Think of skills as expertise that Claude acquires for your project.

## Skills vs Commands

**Commands** are explicit shortcuts:
- You type `/project:review` to invoke
- Run when called
- Good for repeatable workflows

**Skills** are automatic expertise:
- Claude detects when they're relevant
- Apply without invocation
- Good for persistent knowledge

## Skill Structure

Skills live in `.claude/skills/` (project) or `~/.claude/skills/` (global).

Each skill is a directory containing a `SKILL.md` file and optional supporting files:

```
.claude/skills/
+-- testing/
    +-- SKILL.md
    +-- examples/
    |   +-- test-example.ts
    +-- patterns.md
```

## Basic Skill Definition

`.claude/skills/testing/SKILL.md`:

```markdown
---
name: testing-patterns
description: Apply when writing tests, reviewing tests, or discussing test strategy
---

# Testing Patterns Skill

When the task involves testing:

## Our Test Stack
- Jest for unit tests
- React Testing Library for components
- Playwright for E2E
- MSW for API mocking

## Test File Conventions
- Test files: `*.test.ts` alongside source
- E2E tests: `e2e/*.spec.ts`
- Test utilities: `src/test-utils/`

## Required Patterns
- Use `describe` blocks for grouping
- Use `it` with behavior descriptions
- Follow arrange/act/assert structure
- Mock external deps, not internal modules

## Example
See examples/test-example.ts for reference patterns.
```

## How Claude Discovers Skills

When you give Claude a task:

1. Claude reviews available skill descriptions
2. If a description matches the task context, Claude loads that skill
3. Skill instructions become part of Claude's approach

This happens transparently—you don't explicitly invoke skills.

## Practical Skill Examples

### Code Style Skill

`.claude/skills/code-style/SKILL.md`:

```markdown
---
name: code-style
description: Apply to all code writing and review in this project
---

# Code Style

## TypeScript
- Strict mode enabled
- Prefer interfaces over types for objects
- Use `const` by default, `let` when needed
- No `any` without justification comment

## React
- Functional components only
- Custom hooks in `src/hooks/`
- Props interfaces above component
- Destructure props in function signature

## Naming
- camelCase for functions and variables
- PascalCase for components and types
- SCREAMING_CASE for constants
- kebab-case for file names

## Files
- One component per file
- Max 200 lines per file (refactor if larger)
- Index files only for public exports
```

### API Design Skill

`.claude/skills/api-design/SKILL.md`:

```markdown
---
name: api-design
description: Apply when creating or modifying API endpoints
---

# API Design Standards

## Endpoints
- RESTful resource naming
- Use plural nouns: /users, /orders
- Nest for relationships: /users/:id/orders

## Responses
- Always return JSON
- Success: { data: ... }
- Error: { error: { code, message, details? } }
- Include appropriate status codes

## Validation
- Validate all inputs with Zod
- Return 400 for validation failures
- Include field-level error messages

## Authentication
- JWT in Authorization header
- 401 for missing/invalid token
- 403 for insufficient permissions

## Versioning
- URL prefix: /api/v1/
- Breaking changes require new version
```

### Git Workflow Skill

`.claude/skills/git-workflow/SKILL.md`:

```markdown
---
name: git-workflow
description: Apply when making commits, creating branches, or doing git operations
---

# Git Workflow

## Branch Naming
- feature/description-here
- fix/bug-description
- refactor/what-changed
- docs/what-documented

## Commit Messages
Format: type(scope): description

Types:
- feat: new feature
- fix: bug fix
- docs: documentation
- style: formatting
- refactor: code restructuring
- test: adding tests
- chore: maintenance

Examples:
- feat(auth): add password reset flow
- fix(cart): correct quantity calculation
- docs(api): update endpoint documentation

## Pull Requests
- Reference issue number: "Fixes #123"
- Include test evidence
- Request review from code owners
```

### Framework-Specific Skills

`.claude/skills/nextjs/SKILL.md`:

```markdown
---
name: nextjs-patterns
description: Apply when working with Next.js code in this project
---

# Next.js 14 Patterns

## App Router
- All routes in app/ directory
- page.tsx for routes
- layout.tsx for shared layouts
- loading.tsx for suspense boundaries
- error.tsx for error handling

## Server Components (Default)
- Fetch data directly in components
- No useState/useEffect
- 'use server' for server actions

## Client Components
- Add 'use client' directive
- For interactivity and browser APIs
- Keep as small as possible

## Data Fetching
- Prefer server components
- Use fetch with caching options
- Revalidate with tags when needed

## Styling
- Tailwind CSS for all styling
- Component-specific in same file
- Global styles in app/globals.css
```

## Multi-File Skills

Skills can include supporting files:

```
.claude/skills/database/
+-- SKILL.md
+-- migrations-guide.md
+-- query-patterns.sql
+-- schema-conventions.md
```

Reference them in SKILL.md:

```markdown
---
name: database-patterns
description: Apply when working with database code, migrations, or queries
---

# Database Patterns

See the following guides for detailed patterns:
- migrations-guide.md: How to write migrations
- query-patterns.sql: Common query patterns
- schema-conventions.md: Naming and structure rules
```

## Skills vs CLAUDE.md

**Use CLAUDE.md for:**
- Always-true project context
- Current state and active work
- High-level architecture

**Use Skills for:**
- Domain-specific expertise
- Detailed patterns and examples
- Knowledge Claude should access conditionally

Skills are modular chunks of what could be in CLAUDE.md. Instead of one massive file, skills let Claude access specific expertise only when needed—improving context efficiency.

## Skill Activation

Claude activates skills based on natural language matching:

| Skill Description | Activates When You Say |
|------------------|----------------------|
| "Apply when writing tests" | "Write tests for this function" |
| "Apply for API design" | "Create an endpoint for user registration" |
| "Apply for git operations" | "Commit these changes" |

Write clear, specific descriptions for reliable activation.

## Official Anthropic Skills

Anthropic provides skills for common document types:

- PDF handling
- Word document (docx) processing
- PowerPoint creation
- Excel manipulation

These are pre-installed and activate automatically when working with those file types.

## Creating Effective Skills

1. **Identify repeated patterns** — What do you explain to Claude over and over?

2. **Write the skill** — Capture that knowledge in SKILL.md

3. **Include examples** — Concrete examples beat abstract rules

4. **Test activation** — Verify the skill triggers when expected

5. **Iterate** — Refine based on Claude's behavior

## The Power of Accumulated Expertise

Over time, your skill library becomes a knowledge base:

```
.claude/skills/
+-- code-style/
+-- testing/
+-- api-design/
+-- git-workflow/
+-- nextjs/
+-- database/
+-- error-handling/
+-- security/
```

Every task benefits from relevant accumulated expertise, automatically applied. Claude doesn't just help you code—it helps you code your way.

> **See Also:**
> - [CLAUDE.md](05-CLAUDE-md-Your-Projects-Brain.md) for always-on project context
> - [Slash Commands](06-Slash-Commands-and-Custom-Commands.md) for explicit command shortcuts
> - [Writing Plugins](36-Writing-Claude-Code-Plugins.md) for packaging skills for distribution

---

**Next:** [Chapter 13: Headless Mode and the SDK](13-Headless-Mode-and-the-SDK.md) — Run Claude without interaction and build your own tools.
