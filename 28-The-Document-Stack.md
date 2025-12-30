# Chapter 28: The Document Stack

Professional software development uses a hierarchy of documents. Each serves a specific purpose at a different level of detail.

You don't need all of these. But understanding the stack lets you choose which documents help your project.

## The Hierarchy

From highest to lowest level:

### Executive Brief
**What**: One page. What you're building and why.  
**Who reads it**: Stakeholders who need the "what," not the "how."  
**When to write**: Complex projects with external stakeholders.

### Business Requirements Document (BRD)
**What**: What the system should do from a user perspective. Features, workflows, success criteria.  
**Who reads it**: Product managers, designers, senior developers.  
**When to write**: Projects with many features to track.

### Technical Specification
**What**: How the system will be built. Architecture, components, data models, APIs.  
**Who reads it**: Developers, Claude.  
**When to write**: Multiple components need to work together.

### Work Breakdown Structure (WBS)
**What**: The project broken into tasks. What needs to be done, in what order.  
**Who reads it**: You, your team.  
**When to write**: Projects large enough to need a plan.

### CLAUDE.md
**What**: Briefing document for Claude sessions. Current state, decisions, context.  
**Who reads it**: Claude, you.  
**When to write**: Any project spanning multiple sessions.

## Creating Documents with Claude

You don't have to write these yourself. Claude helps:

```
Help me write an executive brief for the expense tracker.
Include: problem statement, proposed solution, target users, success metrics.
Keep it to one page.
```

```
Let's develop business requirements.
What features does an expense tracker need?
For each feature, what's the user story and acceptance criteria?
```

```
Create a technical specification for the expense tracker.
Cover: architecture, data model, API endpoints, authentication approach.
```

Claude knows what these documents should contain. You provide specifics; Claude provides structure.

## Templates

### Executive Brief Template

```markdown
# [Project Name] - Executive Brief

## Problem
What problem are we solving? For whom? Why does it matter?

## Solution
What are we building? High-level description.

## Key Features
- Feature 1: Brief description
- Feature 2: Brief description
- Feature 3: Brief description

## Success Metrics
How will we know if this succeeds?

## Timeline
High-level milestones and dates.

## Resources Required
What do we need to build this?
```

### Technical Specification Template

```markdown
# [Project Name] - Technical Specification

## Overview
What is being built and why.

## Architecture
High-level system design. Components and their relationships.

## Data Model
Database schema. Entity relationships.

## API Design
Endpoints. Request/response formats. Authentication.

## Technology Choices
- Language: [why]
- Framework: [why]
- Database: [why]

## Security Considerations
Authentication, authorization, data protection.

## Deployment
How will this be deployed and operated?

## Open Questions
What still needs to be decided?
```

### CLAUDE.md Template

```markdown
# [Project Name]

## Overview
One paragraph description.

## Current State
- [x] Completed feature
- [ ] In progress feature
- [ ] Planned feature

## Tech Stack
- Backend: [language/framework]
- Frontend: [framework]
- Database: [database]
- Other: [anything else]

## Key Decisions
- Decision 1: Rationale
- Decision 2: Rationale

## Project Structure
- src/: Source code
  - api/: Backend routes
  - components/: Frontend components
- tests/: Test files
- docs/: Documentation

## How to Run
```bash
# Commands to start the project
```

## Next Steps
1. Next task
2. Following task
```

## When to Use Each Document

| Project Size | Documents Needed |
|--------------|------------------|
| Script/utility | CLAUDE.md (optional) |
| Personal project | CLAUDE.md |
| Team project | CLAUDE.md + Tech Spec |
| Complex product | Full stack |

## The Portability Advantage

When your project is documented:

**You're not dependent on conversation history.** Start fresh sessions with documents.

**You can hand off to others.** New team member? Give them the docs.

**You can return after months.** Documentation remembers what you forgot.

**Claude performs better.** Clear context = clear results.

The documents ARE the project. Conversations are just how you create and modify them.

## Keeping Documents Synchronized

Documents drift from reality. Counter this:

**Update at session end.** After significant work, update relevant docs.

**Review periodically.** Monthly check: do docs match code?

**Include doc updates in PRs.** Code changes should include doc changes.

**Let Claude help.** "Update CLAUDE.md to reflect the changes we made."

## The Minimum Useful Documentation

If you write nothing else, write this:

```markdown
# [Project Name]

What this is and what it does.

## Status
What works. What doesn't. What's next.

## How to Run
Exact commands.

## Key Files
Where to find important code.
```

Update it every session. It's five minutes that saves hours.
