# Appendix B: CLAUDE.md Templates

## Minimal Template

For small projects and scripts:

```markdown
# Project Name

Brief description of what this does.

## How to Run
```bash
python main.py
```

## Status
What works, what doesn't.
```

## Standard Template

For most projects:

```markdown
# Project Name

## Overview
One paragraph describing what this project does and why.

## Current State
- [x] Completed feature
- [ ] In progress feature
- [ ] Planned feature

## Tech Stack
- Language: Python 3.11
- Framework: Flask
- Database: SQLite
- Other: Claude API

## Project Structure
```
project/
├── app.py          # Main application
├── models.py       # Database models
├── routes/         # API routes
├── templates/      # HTML templates
└── static/         # CSS/JS
```

## Key Decisions
- **Database choice**: SQLite for simplicity; migrate to PostgreSQL if needed
- **Auth approach**: Session-based with Flask-Login

## How to Run
```bash
# Install dependencies
pip install -r requirements.txt

# Set environment variables
export ANTHROPIC_API_KEY="your-key"

# Run the application
flask run --debug
```

## Environment Variables
- `ANTHROPIC_API_KEY`: Required for AI features
- `DATABASE_URL`: Optional, defaults to SQLite

## Next Steps
1. Complete user authentication
2. Add export functionality
3. Improve mobile layout
```

## Team Project Template

For collaborative development:

```markdown
# Project Name

## Overview
Description of the project, its goals, and target users.

## Team
- **Lead**: Name (area of responsibility)
- **Backend**: Name
- **Frontend**: Name

## Current Sprint
Sprint 12: User Dashboard Improvements
- [ ] Dashboard widget framework
- [ ] Real-time notifications
- [x] Performance optimization

## Tech Stack
### Backend
- Python 3.11 / FastAPI
- PostgreSQL with SQLAlchemy
- Redis for caching
- Celery for background jobs

### Frontend
- React 18 with TypeScript
- Tailwind CSS
- React Query for data fetching

### Infrastructure
- AWS ECS for hosting
- RDS for database
- CloudFront for CDN

## Architecture
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│     API     │────▶│  Database   │
│   (React)   │     │  (FastAPI)  │     │ (PostgreSQL)│
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │    Redis    │
                    │   (Cache)   │
                    └─────────────┘
```

## Code Standards
- Use TypeScript strict mode
- All functions must have type hints (Python)
- Tests required for new features
- PR reviews required before merge

## Key Files
- `src/api/main.py`: API entry point
- `src/api/routes/`: Route handlers
- `src/client/src/App.tsx`: React entry
- `src/client/src/pages/`: Page components

## Common Tasks

### Adding a new API endpoint
1. Create route in `src/api/routes/`
2. Add schema in `src/api/schemas/`
3. Add tests in `tests/api/`
4. Update OpenAPI docs

### Adding a new page
1. Create component in `src/client/src/pages/`
2. Add route in `src/client/src/routes.tsx`
3. Add navigation link if needed

## Environment Setup
```bash
# Backend
cd src/api
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Frontend
cd src/client
npm install

# Run both
make dev  # Uses docker-compose
```

## Deployment
- `main` branch auto-deploys to staging
- Production deploys via GitHub release tags
- Rollback: `make rollback ENV=prod`

## Contacts
- Slack: #project-name
- On-call: Check PagerDuty rotation
```

## Personal Global CLAUDE.md

For `~/.claude/CLAUDE.md`:

```markdown
# Global Preferences

## About Me
Senior developer working primarily in Python and TypeScript.
I prefer concise code and clear explanations.

## Code Style
- Functional programming patterns when appropriate
- Meaningful names over comments
- Small functions that do one thing
- Tests for complex logic

## Preferences
- Use f-strings in Python (not .format())
- Prefer async/await over callbacks
- Use TypeScript strict mode
- Tailwind CSS for styling

## Tools
- Editor: VS Code
- Terminal: iTerm2 with zsh
- Package managers: npm, pip
- Version control: Git with conventional commits

## Communication
- Be direct and concise
- Show code examples
- Explain tradeoffs when relevant
- Ask clarifying questions if my request is ambiguous

## Don't
- Add excessive comments
- Create unnecessary abstractions
- Use deprecated patterns
- Over-engineer simple solutions
```

## AI-Heavy Project Template

For projects using Claude API extensively:

```markdown
# AI Project Name

## Overview
Application that uses Claude AI for [purpose].

## AI Integration
### Models Used
- `claude-sonnet-4-5-20250929`: Main processing
- `claude-haiku-4-5-20251001`: Quick classifications

### Prompts
Prompts are stored in `prompts/`:
- `prompts/summarize.txt`: Meeting summarization
- `prompts/extract.txt`: Action item extraction
- `prompts/classify.txt`: Ticket classification

### Cost Management
- Average cost per request: ~$0.01
- Daily budget limit: $10
- Caching enabled for identical requests

## API Key Management
- Development: Use personal key in `.env`
- Production: AWS Secrets Manager
- Never commit keys to git

## Rate Limits
- Current tier: 60 RPM
- Implemented: Exponential backoff
- Queue: Redis-based request queue

## Monitoring
- Cost tracking: CloudWatch metrics
- Error rates: Sentry
- Usage dashboard: `/admin/ai-usage`

## Testing AI Features
- Mock responses in tests (don't call real API)
- Golden file tests for prompt changes
- Manual testing checklist in `docs/ai-testing.md`
```

## Usage Tips

1. **Copy the relevant template** as a starting point
2. **Delete sections you don't need** — shorter is better
3. **Update regularly** — stale docs mislead
4. **Include run commands** — always test that they work
5. **Document decisions** — future you will thank present you
