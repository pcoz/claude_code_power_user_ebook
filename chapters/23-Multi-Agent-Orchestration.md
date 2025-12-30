# Chapter 23: Multi-Agent Orchestration

The most powerful Claude Code pattern: multiple specialized agents working together on complex tasks. This is how you tackle problems too large for a single context window.

## The Orchestration Concept

Instead of one agent doing everything:

```
Main Agent
+-- Research Agent --> Gathers information
+-- Implementation Agent --> Writes code
+-- Review Agent --> Checks quality
+-- Test Agent --> Verifies correctness
```

Each agent has its own context window, specialized expertise, and focused task.

## Why Orchestration Works

**Context preservation.** A single agent reviewing 50 files loses detail by the end. Fifty agents reviewing one file each maintain full context.

**Specialization.** A security expert agent thinks differently than a performance expert agent. Specialized prompts yield specialized insights.

**Parallelization.** Multiple agents work simultaneously. Complex tasks complete faster.

## Basic Orchestration Pattern

```
Coordinate these agents to implement the new payment feature:

1. research-agent: Analyze our current payment code and document patterns
2. architect-agent: Design the new payment flow based on patterns
3. implementation-agent: Implement following the design
4. security-agent: Review implementation for vulnerabilities
5. test-agent: Generate comprehensive tests

Each agent should complete before the next begins.
Synthesize final results.
```

Claude orchestrates, delegating to subagents.

## Parallel Orchestration

For independent tasks:

```
Run these analyses in parallel:

1. security-agent: Audit authentication code
2. performance-agent: Profile database queries
3. dependency-agent: Check for outdated packages
4. coverage-agent: Identify untested code

Synthesize all findings into a single health report.
```

Four agents work simultaneously, each with full context for their task.

## Defining Specialist Agents

### Product Manager Agent

```markdown
# .claude/agents/product-manager.md
---
name: product-manager
description: Use for defining requirements and acceptance criteria
model: sonnet
tools:
  - Read
  - Grep
---

You are an experienced product manager.

Focus on:
- User needs and pain points
- Clear acceptance criteria
- Edge cases and error states
- User-facing behavior, not implementation

Output format:
- Feature description (1-2 sentences)
- User stories (As a... I want... So that...)
- Acceptance criteria (Given/When/Then)
- Out of scope items
```

### Architect Agent

```markdown
# .claude/agents/architect.md
---
name: architect
description: Use for technical design and system architecture
model: opus
tools:
  - Read
  - Grep
  - Glob
---

You are a senior software architect.

Focus on:
- System design and component interaction
- Data flow and state management
- Scalability and performance considerations
- Technical trade-offs

Output format:
- High-level design overview
- Component breakdown
- Interface definitions
- Technical decisions with rationale
```

### Implementation Agent

```markdown
# .claude/agents/implementer.md
---
name: implementer
description: Use for writing production code
model: sonnet
tools:
  - Read
  - Write
  - Edit
  - Bash
---

You are a senior developer.

Focus on:
- Clean, readable code
- Following existing patterns
- Proper error handling
- Inline documentation for complex logic

Before coding:
- Review existing patterns in the codebase
- Follow the project's style guide
- Match existing conventions
```

### Code Reviewer Agent

```markdown
# .claude/agents/reviewer.md
---
name: reviewer
description: Use for detailed code review
model: opus
tools:
  - Read
  - Grep
  - Glob
---

You are a senior code reviewer.

Review for:
1. Correctness - Does it do what it should?
2. Security - Any vulnerabilities?
3. Performance - Any obvious bottlenecks?
4. Maintainability - Is it readable and well-structured?
5. Testing - Is it testable? Are tests needed?

Output format:
- Summary (1-2 sentences)
- Critical issues (must fix)
- Suggestions (should consider)
- Praise (what's done well)
```

## Orchestration Command

Create a command to run the full pipeline:

```markdown
# .claude/commands/build-feature.md
---
description: Full-cycle feature development with multiple agents
---

For feature: $ARGUMENTS

Execute this pipeline:

## Phase 1: Requirements
Use product-manager agent to define:
- Clear requirements
- Acceptance criteria
- Edge cases

## Phase 2: Design  
Use architect agent to:
- Create technical design
- Define interfaces
- Document decisions

## Phase 3: Implementation
Use implementer agent to:
- Write the code following the design
- Follow existing patterns
- Handle errors appropriately

## Phase 4: Review
Use reviewer agent to:
- Review all new code
- Identify issues
- Suggest improvements

## Phase 5: Testing
Use test-agent to:
- Generate comprehensive tests
- Run tests and verify they pass

## Synthesis
Compile:
- What was built
- How it works
- Any remaining concerns
- Next steps
```

Usage: `/project:build-feature user password reset`

## Dynamic Agent Spawning

For tasks that need adaptive orchestration:

```
This codebase has inconsistent error handling across 50 files.

Strategy:
1. First agent: Identify all files with error handling
2. For each file: Spawn a subagent to refactor error handling
3. Final agent: Verify consistency across all files

Proceed with this parallel refactoring.
```

Claude creates as many subagents as needed.

## Pipeline Patterns

### Linear Pipeline
```
Agent A --> Agent B --> Agent C --> Result
```
Each agent's output feeds the next.

### Fan-Out Fan-In
```
          +--> Agent B --+
Agent A --+--> Agent C --+--> Agent E
          +--> Agent D --+
```
One agent's work is parallelized, then synthesized.

### Iterative Pipeline
```
Agent A <-- feedback <-- Agent B
    |                       ^
    v                       |
  output --> evaluate --> improve
```
Agents iterate until quality threshold is met.

## Real-World Example: Documentation Pipeline

```
Create comprehensive documentation for this codebase:

Phase 1 (Parallel):
- architecture-agent: Document system architecture
- api-agent: Generate API documentation  
- guide-agent: Create getting started guide
- reference-agent: Build function reference

Phase 2 (Sequential):
- editor-agent: Review and improve all docs
- formatter-agent: Ensure consistent formatting

Phase 3:
- Compile into final documentation structure
```

## Cost Management

Multi-agent workflows can be expensive. Manage costs:

```json
{
  "agents": {
    "quick-lookup": { "model": "haiku" },
    "implementation": { "model": "sonnet" },
    "architecture": { "model": "opus" }
  }
}
```

Use cheaper models for simple tasks, expensive models for complex ones.

## Error Handling in Orchestration

```
If any agent fails:
1. Log the failure with context
2. Attempt retry with more specific instructions
3. If still failing, pause and report for human review
4. Continue with other agents if tasks are independent
```

## Monitoring Orchestration

Track what's happening:

```
For this orchestration:
1. Log when each agent starts and completes
2. Report token usage per agent
3. Flag if any agent exceeds expected duration
4. Provide progress updates
```

## The Meta-Agent Pattern

An agent that decides which agents to use:

```markdown
# .claude/agents/coordinator.md
---
name: coordinator
description: Analyzes tasks and delegates to appropriate agents
model: opus
---

You are a project coordinator.

When given a task:
1. Analyze what kind of work is needed
2. Identify which specialist agents should be involved
3. Determine the order of operations
4. Delegate to appropriate agents
5. Synthesize results

Available agents:
- product-manager: Requirements and user stories
- architect: Technical design
- implementer: Code writing
- reviewer: Code review
- security-auditor: Security analysis
- test-writer: Test generation
```

Then just say:

```
@coordinator: Implement the new notification system
```

The coordinator decides how to orchestrate the work.

## Practical Limits

**What works:**
- 3-10 agents collaborating
- Clear boundaries between agent responsibilities
- Well-defined handoff formats
- Tasks that benefit from specialization

**What's challenging:**
- 20+ agents (coordination overhead)
- Highly interdependent tasks
- Agents that need to iterate with each other extensively
- Tasks smaller than agent overhead justifies

## Starting Simple

Begin with two-agent orchestration:

```
Use an analyst agent to understand the problem,
then an implementer agent to build the solution.
```

Add agents as you identify distinct roles that benefit from specialization. Orchestration power grows with practice.

> **See Also:**
> - [Subagents: Parallel Intelligence](11-Subagents-Parallel-Intelligence.md) for subagent fundamentals
> - [Headless Mode and the SDK](13-Headless-Mode-and-the-SDK.md) for programmatic orchestration
> - [Large Codebase Navigation](24-Large-Codebase-Navigation.md) for applying orchestration to large projects

---

**Next:** [Chapter 24: Large Codebase Navigation](24-Large-Codebase-Navigation.md) — Navigate and understand massive codebases.
