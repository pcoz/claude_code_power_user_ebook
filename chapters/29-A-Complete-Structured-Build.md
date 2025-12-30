# Chapter 29: A Complete Structured Build

Let's put everything together with a complete example—from requirements through deployment, using the document stack and Claude Code best practices.

## The Project

A meeting notes application:
- Upload or paste meeting notes
- Get AI-generated summaries and action items
- Track action item completion over time
- Search across past meetings

This is complex enough to need structure, manageable enough to complete.

## Phase 1: Executive Brief

Start a Claude session:

```
I want to build a meeting notes application.
Help me write a one-page executive brief.

The core idea:
- Upload or paste meeting notes
- AI extracts summaries and action items
- Track action item completion
- Search past meetings
```

Claude drafts the brief. Refine together. Save as `docs/executive-brief.md`.

## Phase 2: Requirements

```
Based on the executive brief, let's develop business requirements.

For each feature area:
1. User stories (As a user, I want...)
2. Acceptance criteria (Given/When/Then)
3. Priority (Must have / Should have / Nice to have)
```

Work through each area: note capture, AI processing, action tracking, search.

Save as `docs/requirements.md`.

## Phase 3: Technical Specification

```
Now let's plan the technical implementation.

I want to keep it simple:
- Python with Flask
- SQLite database
- Claude API for AI features
- Simple HTML/CSS frontend

Create a technical spec covering:
- Architecture
- Data model
- API endpoints
- AI integration approach
```

Claude proposes architecture. Discuss tradeoffs. Save as `docs/tech-spec.md`.

## Phase 4: Work Breakdown

```
Break this project into phases:

Phase 1: Core infrastructure (no AI yet)
Phase 2: AI integration
Phase 3: Action item tracking
Phase 4: Search and polish

For each phase, list specific tasks.
```

Save as `docs/work-breakdown.md`.

## Phase 5: Create CLAUDE.md

```
Create CLAUDE.md for this project.
Include everything a future Claude session needs to continue the work.
```

Example result:

```markdown
# Meeting Notes Application

## Overview
Web app for managing meeting notes with AI-powered summaries
and action item extraction.

## Current State
- [x] Project planning complete
- [ ] Phase 1: Core infrastructure (IN PROGRESS)
- [ ] Phase 2: AI integration
- [ ] Phase 3: Action tracking
- [ ] Phase 4: Search and polish

## Tech Stack
- Backend: Python 3.11, Flask
- Database: SQLite with SQLAlchemy
- AI: Claude API (claude-sonnet)
- Frontend: Vanilla HTML/CSS/JS

## Key Files (once created)
- app.py: Main Flask application
- models.py: Database models
- ai_service.py: Claude API integration
- templates/: Jinja2 templates
- static/: CSS and JavaScript

## Database Schema
- meetings: id, title, raw_notes, summary, created_at
- action_items: id, meeting_id, description, assignee, due_date, completed

## API Endpoints
- GET /meetings - List meetings
- POST /meetings - Create meeting
- GET /meetings/:id - Get meeting details
- POST /meetings/:id/process - Run AI processing
- PATCH /action_items/:id - Update action item

## Running the Project
flask run --debug

## Environment Variables
- ANTHROPIC_API_KEY: Claude API key

## Next Steps
1. Create Flask app skeleton
2. Set up database models
3. Build meeting CRUD
4. Create basic UI
```

## Phase 6: Build

Now build systematically:

```
Let's start Phase 1.
Create the Flask application with SQLite database.
Start with: app.py, models.py, basic templates.
```

Claude builds. You approve. Test that it runs.

```
Add meeting CRUD operations.
Create, read, update, delete meetings.
```

Test. Continue.

```
Create the frontend to add and view meetings.
Simple and functional.
```

Test. At the end of the session:

```
Update CLAUDE.md with our progress.
```

## Phase 7: AI Integration

New session (or continue):

```
Read CLAUDE.md and let's continue with Phase 2: AI integration.
```

Claude reads context, knows where things stand.

```
Create ai_service.py that:
1. Takes meeting notes text
2. Sends to Claude API
3. Extracts: summary, key decisions, action items
4. Returns structured data

Use the API key from ANTHROPIC_API_KEY environment variable.
```

Build, test, integrate with the main app.

## Phase 8: Action Tracking

```
Continue to Phase 3: Action item tracking.

Features needed:
- Display action items extracted from meetings
- Mark items as complete
- Filter by status, assignee, due date
- Dashboard showing overdue items
```

Build each feature incrementally.

## Phase 9: Search and Polish

```
Phase 4: Search and polish.

Add search functionality:
- Full-text search across meeting notes
- Filter by date range
- Sort options

Then polish:
- Improve the UI
- Add loading states
- Handle errors gracefully
- Add helpful empty states
```

## Phase 10: Documentation Update

```
Update all documentation to reflect the completed project:
- CLAUDE.md with final state
- README.md for public consumption
- API documentation
- Setup instructions
```

## The Structure Payoff

What did structured development provide?

**Clear direction.** Each phase had defined goals.

**Continuity.** Multiple sessions built on each other coherently.

**Documentation.** The project is understandable to others (and future you).

**Manageable scope.** Breaking into phases prevented scope creep.

**Quality.** Testing at each phase caught issues early.

## Adapting the Process

For smaller projects, skip phases:
- Script: Just build it, maybe add CLAUDE.md
- Personal tool: CLAUDE.md + build
- Team project: Full process as shown

For larger projects, add phases:
- Design phase with mockups
- Security review phase
- Performance testing phase
- Deployment and monitoring setup

Scale the process to the project. The goal isn't paperwork—it's clarity and coherence.

## The Completed Project

At the end, you have:
- Working software
- Clear documentation
- Maintainable code
- Understanding of decisions made

The structured approach took more upfront time but saved time overall—and produced a better result.

This is the full power of Claude Code: not just generating code quickly, but building real software systematically.

> **See Also:**
> - [The Document Stack](28-The-Document-Stack.md) for understanding each document type
> - [Full-Stack Applications](20-Full-Stack-Applications.md) for technical implementation patterns
> - [The Development Loop](14-The-Development-Loop.md) for the iterative building process

---

**Next:** [Chapter 30: Mini Labs — Hands-On Practice](30-Mini-Labs-Hands-On-Practice.md) — Practice what you've learned with guided exercises.
