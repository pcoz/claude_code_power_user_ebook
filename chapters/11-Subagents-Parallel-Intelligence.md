# Chapter 11: Subagents — Parallel Intelligence

Subagents are Claude's secret weapon. They're separate Claude instances that the main agent can spawn to handle focused subtasks—each with its own context window, specialized knowledge, and tools.

This is how you scale beyond a single conversation.

## Why Subagents Matter

Every Claude conversation has a context limit. As conversations grow, Claude must summarize or forget earlier content. For complex tasks, this limits what's achievable.

Subagents solve this by:

1. **Isolating context** — Each subagent gets a fresh context window focused on its task
2. **Running in parallel** — Multiple subagents work simultaneously
3. **Specializing knowledge** — Each subagent can have different expertise and tools
4. **Preserving quality** — Focused context means better results per task

## The Built-in Subagent

Even without configuration, Claude can spawn subagents using the Task tool:

"Explore the codebase using 4 tasks in parallel. Each should explore a different directory."

Claude spawns four subagents, each analyzing a portion of your codebase, then synthesizes results.

## Defining Custom Subagents

Create subagent definitions in `.claude/agents/` (project) or `~/.claude/agents/` (global).

### File-Based Definition

`.claude/agents/code-reviewer.md`:

```markdown
---
name: code-reviewer
description: Expert code reviewer. Use for detailed code analysis and review.
model: sonnet
tools:
  - Read
  - Grep
  - Glob
---

You are a senior code reviewer focused on:

## Expertise
- Code quality and readability
- Security vulnerabilities
- Performance issues
- Best practices adherence

## Review Process
1. Understand the context and purpose
2. Check for logic errors and edge cases
3. Verify error handling
4. Assess test coverage needs
5. Identify security concerns
6. Suggest improvements

## Output Format
For each issue:
- Location (file:line)
- Severity (Critical/High/Medium/Low)
- Description
- Suggested fix
```

### CLI-Based Definition

```bash
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer. Use proactively after code changes.",
    "prompt": "You are a senior code reviewer. Focus on code quality, security, and best practices.",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  }
}'
```

## Subagent Properties

| Property | Purpose |
|----------|---------|
| `name` | Identifier for the subagent |
| `description` | When to use this subagent (Claude reads this to decide) |
| `prompt` | System prompt for the subagent |
| `model` | Which model to use (opus, sonnet, haiku) |
| `tools` | Allowed tools for this subagent |

## Practical Subagent Examples

### Security Auditor

`.claude/agents/security-auditor.md`:

```markdown
---
name: security-auditor
description: Use for security analysis, vulnerability assessment, and security code review.
model: opus
tools:
  - Read
  - Grep
  - Glob
---

You are a security specialist focused on:

## Focus Areas
- SQL injection vulnerabilities
- XSS and CSRF issues
- Authentication and authorization flaws
- Secrets in code
- Input validation gaps
- Dependency vulnerabilities

## Methodology
1. Identify sensitive operations (auth, data access, user input)
2. Trace data flow for taint analysis
3. Check for common vulnerability patterns
4. Review dependency security
5. Assess access control implementation

## Report Format
- Vulnerability name
- OWASP category
- Affected files and lines
- Proof of concept (if applicable)
- Remediation steps
```

### Test Writer

`.claude/agents/test-writer.md`:

```markdown
---
name: test-writer
description: Use to generate comprehensive tests for code.
model: sonnet
tools:
  - Read
  - Write
  - Bash
---

You are a test engineering expert.

## Testing Philosophy
- Tests should be readable as documentation
- Each test should test one thing
- Use descriptive test names
- Prefer integration tests for important flows
- Mock external dependencies, not internal logic

## Test Categories
1. Unit tests for pure functions
2. Integration tests for API endpoints
3. Edge case tests for boundary conditions
4. Error handling tests for failure modes

## When Generating Tests
1. Read and understand the code
2. Identify testable units
3. Write tests with arrange/act/assert pattern
4. Run tests to verify they pass
5. Add edge cases based on code analysis
```

### Database Expert

`.claude/agents/db-expert.md`:

```markdown
---
name: database-expert
description: Use for database queries, schema design, optimization, and migrations.
model: sonnet
tools:
  - Read
  - Grep
  - mcp__postgres__*
---

You are a database specialist.

## Expertise
- Query optimization
- Schema design and normalization
- Index strategy
- Migration planning
- Performance tuning

## Response Format
1. Analysis summary
2. Specific recommendations
3. Implementation steps
4. Performance impact assessment
5. Risk evaluation
```

## Invoking Subagents

Claude automatically invokes subagents when tasks match their descriptions.

You can also invoke explicitly:

"Use the security-auditor agent to review the authentication module."

"Have the test-writer create tests for src/utils/validation.ts"

## Parallel Subagents

The real power is parallel execution:

"Use four subagents in parallel:
1. code-reviewer: Review src/api/
2. security-auditor: Check for vulnerabilities
3. test-writer: Generate missing tests
4. database-expert: Review the migration files"

Each subagent works independently with its own context, then Claude synthesizes results.

## Multi-Agent Workflows

### Product Development Pipeline

Define agents for different roles:

- **product-manager**: Focuses on user needs and acceptance criteria
- **architect**: Designs technical approach
- **developer**: Implements the solution
- **qa-engineer**: Validates and tests

Orchestrate them:

"For this new feature:
1. product-manager: Define requirements and acceptance criteria
2. architect: Create technical design based on requirements
3. developer: Implement according to design
4. qa-engineer: Validate against acceptance criteria"

### Large-Scale Refactoring

For a function used in 75 files:

"I need to refactor the `oldFunction` to `newFunction`. 
1. First agent: Find all usages with grep
2. For each file: Spawn a subagent to make the change
3. Final agent: Verify all changes compile and tests pass"

### Incident Analysis

For debugging across multiple services:

"Analyze this outage:
1. Agent for service-a: Review logs and identify timeline
2. Agent for service-b: Review logs and identify timeline  
3. Agent for service-c: Review logs and identify timeline
4. Synthesize the three timelines into a single incident report"

## Subagent Best Practices

**Clear descriptions** — The description field is how Claude decides when to use the subagent. Make it specific.

**Appropriate tools** — Limit tools to what the subagent needs. A reviewer doesn't need Write access.

**Model selection** — Use opus for complex reasoning, sonnet for most tasks, haiku for simple lookups.

**Focused scope** — Each subagent should have one clear purpose.

**Output format** — Specify how the subagent should structure its response.

## The Orchestration Pattern

```
Main Agent
+-- Subagent A (focused task)
+-- Subagent B (focused task)
+-- Subagent C (focused task)
+-- Synthesis (combine results)
```

The main agent:
1. Decomposes the problem
2. Spawns appropriate subagents
3. Collects results
4. Synthesizes into final answer

Each subagent:
1. Receives focused task
2. Uses full context for that task
3. Returns structured result

## Context Preservation

The key insight: by giving each subagent its own context window, you preserve quality.

A single agent trying to review 20 files would forget details of early files by the time it finished. Twenty subagents, each reviewing one file, maintain full context for their portion.

This is how you handle large codebases with AI.

> **See Also:**
> - [Multi-Agent Orchestration](23-Multi-Agent-Orchestration.md) for advanced orchestration patterns
> - [The Model Selector](08-The-Model-Selector.md) for choosing the right model for each subagent
> - [Large Codebase Navigation](24-Large-Codebase-Navigation.md) for strategies at scale

---

**Next:** [Chapter 12: Skills — Automatic Expertise](12-Skills-Automatic-Expertise.md) — Give Claude automatic expertise that activates when relevant.
