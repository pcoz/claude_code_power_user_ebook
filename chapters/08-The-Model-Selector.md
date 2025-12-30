# Chapter 8: The Model Selector

Claude Code gives you access to multiple Claude models. Each has different strengths, speeds, and costs. Choosing the right model for the task makes your work faster and cheaper.

## Available Models

| Model | Alias | Best For |
|-------|-------|----------|
| Claude Opus 4.5 | `opus` | Complex reasoning, architecture, difficult bugs |
| Claude Sonnet 4.5 | `sonnet` | General development (recommended default) |
| Claude Haiku 4.5 | `haiku` | Quick tasks, simple edits, exploration |

## Switching Models

### In Session

```
/model opus
```

Or:
```
/model sonnet
/model haiku
```

### At Launch

```bash
claude --model opus
```

### In Settings

```json
{
  "model": "claude-sonnet-4-20250514"
}
```

## Model Selection Strategy

### Use Haiku For:

- Quick file exploration
- Simple text changes
- Generating boilerplate
- Reading and summarizing
- Fast iteration on small changes

```
/model haiku
List all TODO comments in the codebase
```

Haiku is fast and cheap. Use it for simple tasks where speed matters more than depth.

### Use Sonnet For:

- General development work
- Writing new features
- Debugging typical issues
- Code review
- Most daily tasks

```
/model sonnet
Implement user authentication with JWT tokens
```

Sonnet is the sweet spot—capable enough for real work, fast enough for iteration.

### Use Opus For:

- Complex architectural decisions
- Difficult debugging sessions
- Multi-file refactoring
- Understanding large codebases
- Tasks requiring deep reasoning

```
/model opus
This codebase has a subtle race condition. Analyze the async flow 
and identify where it occurs.
```

Opus is slower and more expensive, but significantly more capable for hard problems.

## Thinking Modes

Regardless of model, you can trigger extended thinking with specific phrases:

| Phrase | Thinking Budget |
|--------|-----------------|
| "think" | Standard extended thinking |
| "think hard" | More computation |
| "think harder" | Even more |
| "ultrathink" | Maximum thinking budget |

Example:
```
Think hard about this architecture. We need to handle 10,000 
concurrent connections with minimal latency.
```

Extended thinking gives Claude more time to reason before responding.

## Cost Awareness

Check your usage:
```
/cost
```

Shows token consumption and estimated costs for the session.

### Cost Optimization

1. **Start with haiku** for exploration
2. **Switch to sonnet** when you know what to build
3. **Use opus** only for genuinely difficult problems
4. **Use /compact** to reduce context when it grows large

## Subagent Model Selection

When defining subagents, specify appropriate models:

```json
{
  "agents": {
    "quick-lookup": {
      "description": "Fast lookups and simple queries",
      "model": "haiku",
      "tools": ["Read", "Grep"]
    },
    "code-reviewer": {
      "description": "Detailed code review",
      "model": "sonnet",
      "tools": ["Read", "Grep", "Glob"]
    },
    "architect": {
      "description": "Complex architectural decisions",
      "model": "opus",
      "tools": ["Read", "Grep", "Glob", "Bash"]
    }
  }
}
```

This balances capability and cost per task.

## Dynamic Model Switching

A common pattern: start cheap, escalate as needed.

```
/model haiku
What files handle user authentication?

/model sonnet
Implement password reset functionality in those files

/model opus
Review the security implications of this implementation
```

Each phase uses the appropriate model.

## Model Capabilities Comparison

### Haiku
- ✅ Fast responses
- ✅ Low cost
- ✅ Good for straightforward tasks
- ❌ May miss nuance
- ❌ Less thorough analysis
- ❌ Weaker at complex reasoning

### Sonnet
- ✅ Balanced performance
- ✅ Good reasoning ability
- ✅ Reliable code quality
- ✅ Reasonable cost
- ❌ May struggle with very complex problems
- ❌ Sometimes needs guidance on architecture

### Opus
- ✅ Strongest reasoning
- ✅ Best for complex problems
- ✅ Most thorough analysis
- ✅ Handles ambiguity well
- ❌ Slower responses
- ❌ Higher cost
- ❌ May overthink simple tasks

## Recommended Defaults

### For Learning
Start with **Sonnet**. It's capable enough to be helpful, responsive enough to not be frustrating.

### For Daily Development
**Sonnet** as default, **Haiku** for quick tasks, **Opus** when stuck.

### For Production/Critical Work
**Opus** for review and architecture decisions. Don't economize on important code.

### For High-Volume Automation
**Haiku** for batch processing. Cost adds up at scale.

## The Model Ladder

Think of it as a ladder:

```
Problem Difficulty
       ^
       |  +-----------+
  Hard |  |   Opus    |  Complex reasoning
       |  +-----------+
       |  +-----------+
Medium |  |  Sonnet   |  General development
       |  +-----------+
       |  +-----------+
  Easy |  |   Haiku   |  Quick tasks
       |  +-----------+
```

Start at the bottom. Climb only when needed.

## Session Example

```
$ claude --model haiku

> What's the project structure?
[Fast response explaining structure]

> /model sonnet

> Add input validation to the user registration form
[Implements validation]

> The validation isn't catching edge cases with unicode usernames

> /model opus

> think hard about unicode username validation edge cases
[Thorough analysis and robust implementation]
```

Match the model to the moment. Your work gets done faster, and your costs stay reasonable.

> **See Also:** [Subagents](11-Subagents-Parallel-Intelligence.md) for running multiple specialized agents with different models.

---

**Next:** [Chapter 9: MCP — Model Context Protocol](09-MCP-Model-Context-Protocol.md) — Connect Claude to databases, APIs, and external services.
