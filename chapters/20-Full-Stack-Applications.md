# Chapter 20: Full-Stack Applications

Beyond scripts, Claude Code can build complete web applications—frontend, backend, database, all working together.

## What Full-Stack Means

A typical web application has:

- **Frontend**: What users see (HTML, CSS, JavaScript)
- **Backend**: Server logic (Python/Node.js/etc.)
- **Database**: Where data lives (SQLite/PostgreSQL/etc.)
- **API**: How frontend and backend communicate

Claude Code handles all of these.

## Start with SQLite

For most projects you build with Claude, SQLite is the right database:

- It's a file (no server to manage)
- Built into Python and most languages
- Handles thousands of users easily
- Zero configuration

Don't reach for PostgreSQL or MySQL until you actually need them.

## A Complete Example

Let's build an expense tracker:

```
Build an expense tracker web application:

Features:
- Add expenses with: amount, category, date, description
- View all expenses in a table
- Filter by date range
- See totals by category
- Simple, clean design

Stack:
- Python with Flask
- SQLite database
- Vanilla HTML/CSS/JavaScript (no React)
- Everything should run with one command

Start with the database schema, then build up.
```

Claude will create multiple files. Approve each one. Watch the structure emerge.

## The Development Sequence

Claude typically builds in this order:

1. **Database schema** — Tables and relationships
2. **Backend models** — Data access layer
3. **API routes** — Endpoints for CRUD operations
4. **Frontend HTML** — Page structure
5. **Frontend JavaScript** — Interactivity
6. **CSS** — Styling

This is logical: each layer builds on the previous.

## Running Your Application

After Claude creates everything:

```
How do I run this application?
```

Usually:
```bash
python app.py
```

Open your browser to `http://localhost:5000` (or whatever port).

## Iterating on Features

Once the basic app works, add features:

```
Add a feature to export expenses to CSV.
The export button should appear above the table.
```

```
Add a pie chart showing spending by category.
Use Chart.js for the visualization.
```

```
Add user authentication so each person has their own expenses.
Use Flask-Login with session-based auth.
Keep it simple - username and password only.
```

## Common Stacks

### Python Stack

```
Build with:
- Flask for web framework
- SQLite for database
- SQLAlchemy for ORM
- Jinja2 for templates
- Vanilla JS for frontend
```

Good for: Quick development, data processing, AI integration.

### Node.js Stack

```
Build with:
- Express for web framework
- SQLite with better-sqlite3
- EJS for templates
- Vanilla JS for frontend
```

Good for: JavaScript everywhere, real-time features.

### Modern Frontend

```
Build with:
- Next.js for full-stack React
- SQLite with Prisma
- Tailwind CSS for styling
- TypeScript throughout
```

Good for: Complex UIs, SPA feel, modern development experience.

## File Structure

A well-organized project:

```
expense-tracker/
+-- app.py              # Main application
+-- models.py           # Database models
+-- routes/
|   +-- expenses.py     # Expense endpoints
|   +-- auth.py         # Auth endpoints
+-- templates/
|   +-- base.html       # Layout template
|   +-- index.html      # Home page
|   +-- expenses.html   # Expenses page
+-- static/
|   +-- css/
|   |   +-- style.css
|   +-- js/
|       +-- app.js
+-- requirements.txt
+-- README.md
```

Ask Claude to organize this way:

```
Organize the project with proper separation:
- Routes in routes/ directory
- Templates in templates/
- Static files in static/
- Models in models.py
```

## Debugging Full-Stack Issues

### Frontend Problems

If something looks wrong, take a screenshot:

```
[paste screenshot]
The layout is broken on mobile. Fix it.
```

Claude sees the visual problem and fixes the CSS.

### Backend Problems

If an API call fails:

```
When I click 'Add Expense', nothing happens.
Check the browser console and server logs.
```

Claude investigates both ends of the request.

### Database Problems

If data isn't saving:

```
Expenses aren't appearing after I add them.
Check the database - are they being saved?
```

Claude queries the database directly:
```bash
sqlite3 expenses.db "SELECT * FROM expenses ORDER BY id DESC LIMIT 5"
```

## Adding Real Features

### Search

```
Add a search box that filters expenses by description.
Search should happen as I type (debounced).
```

### Pagination

```
Add pagination to the expenses list.
Show 20 items per page.
Include page numbers and next/previous.
```

### Dark Mode

```
Add a dark mode toggle.
Remember the preference in localStorage.
Use CSS custom properties for theming.
```

### Mobile Responsiveness

```
Make the application fully responsive.
On mobile:
- Stack form fields vertically
- Make table horizontally scrollable
- Larger touch targets for buttons
```

## Deployment Preparation

Before deploying:

```
Prepare this application for deployment:
1. Use environment variables for configuration
2. Add proper error pages (404, 500)
3. Set secure cookie settings
4. Add CSRF protection
5. Create a production requirements.txt
```

## Testing the Application

```
Create tests for the expense tracker:
1. Unit tests for model functions
2. API tests for each endpoint
3. Test edge cases: empty inputs, invalid dates
4. Use pytest with fixtures for test data
```

## When Things Get Complex

Signs you need more structure:

- Multiple related data models
- User roles and permissions
- Background jobs
- External service integrations
- Team collaboration

At this point, consider:
- Proper project documentation (CLAUDE.md)
- Git workflow with branches
- Migration strategy for database changes
- CI/CD pipeline

Full-stack applications are where Claude Code's ability to work across multiple files truly shines. The AI maintains coherence between frontend and backend, between database schema and API endpoints.

Start simple. Add complexity only when needed. You'll be surprised how far a straightforward architecture can go.

> **See Also:**
> - [A Complete Structured Build](29-A-Complete-Structured-Build.md) for a full example
> - [API Integration Patterns](21-API-Integration-Patterns.md) for connecting to external services

---

**Next:** [Chapter 21: API Integration Patterns](21-API-Integration-Patterns.md) — Connect your applications to the world.
