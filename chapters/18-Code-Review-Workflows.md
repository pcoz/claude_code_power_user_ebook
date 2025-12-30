# Chapter 18: Code Review Workflows

Claude Code excels at code review. It can analyze code for bugs, security issues, performance problems, and style violations—then help you fix what it finds.

This chapter covers practical code review workflows.

## Quick Review

For a fast review of recent changes:

```
Review the changes I've made since the last commit.
Focus on bugs and obvious issues.
```

Claude runs `git diff`, analyzes the changes, and reports findings.

## Structured Review

For thorough reviews, be specific about what to check:

```
Review src/auth/login.ts for:
1. Security vulnerabilities (XSS, injection, auth bypass)
2. Error handling completeness
3. Edge cases that might fail
4. Performance issues
5. Code style and readability

For each issue, tell me:
- Line number
- Severity (Critical/High/Medium/Low)
- What's wrong
- How to fix it
```

## Pull Request Review

When reviewing a PR:

```
Review the changes in this PR. Use 'git diff main..HEAD' to see them.

Check for:
- Bugs and logic errors
- Security issues
- Breaking changes
- Missing tests
- Documentation needs

Summarize your findings as a PR comment.
```

## Creating a Review Command

Save time with a custom command.

`.claude/commands/review-pr.md`:
```markdown
---
description: Comprehensive PR review
---

Review the current branch against main:

1. Run `git diff main..HEAD` to see all changes
2. For each changed file, analyze:
   - Logic correctness
   - Error handling
   - Security implications
   - Test coverage
   - Performance impact

3. Summarize findings by severity:
   - 🔴 Critical: Must fix before merge
   - 🟠 High: Should fix before merge
   - 🟡 Medium: Consider fixing
   - 🟢 Low: Nice to have

4. Provide an overall recommendation:
   - ✅ Approve
   - ⚠️ Approve with comments
   - ❌ Request changes
```

Use with `/project:review-pr`.

## Security-Focused Review

For security-sensitive code:

```
Perform a security audit of src/api/:

1. Check for OWASP Top 10 vulnerabilities
2. Review authentication and authorization
3. Look for input validation gaps
4. Check for secrets in code
5. Analyze dependency security
6. Review error messages for info leakage

For each finding, include:
- Vulnerability type
- Affected code location
- Proof of concept (if applicable)
- Remediation steps
```

## Using Subagents for Review

For large PRs, use parallel subagents:

```
Review this PR using specialized subagents:

1. security-reviewer: Check for security issues
2. performance-reviewer: Analyze performance impact
3. style-reviewer: Check code style and conventions
4. test-reviewer: Evaluate test coverage

Run them in parallel and synthesize findings.
```

Define these subagents in `.claude/agents/`:

**security-reviewer.md:**
```markdown
---
name: security-reviewer
description: Security-focused code review
model: opus
tools: [Read, Grep]
---

You are a security expert. Focus on:
- Injection vulnerabilities
- Authentication flaws
- Authorization bypasses
- Data exposure risks
- Cryptographic issues
```

## Automated Review with Hooks

Run reviews automatically on commits:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash(git commit*)",
        "hooks": [
          {
            "type": "command",
            "command": "claude -p 'Quick review of last commit' --allowedTools 'Read,Grep' --output-format json >> .claude/reviews.json"
          }
        ]
      }
    ]
  }
}
```

## CI Integration

Add Claude review to your CI pipeline:

```yaml
# .github/workflows/ai-review.yml
name: AI Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
          
      - name: Setup Claude
        run: npm install -g @anthropic-ai/claude-code
        
      - name: Run AI Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Review changes between ${{ github.event.pull_request.base.sha }} and ${{ github.sha }}.
                     Output as GitHub PR review comment markdown.
                     Focus on bugs, security, and breaking changes." \
            --allowedTools "Read,Grep,Glob,Bash(git diff*)" \
            > review.md
            
      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = fs.readFileSync('review.md', 'utf8');
            github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: review
            });
```

## Reviewing Specific Patterns

Ask Claude to look for specific issues:

```
Search the codebase for:
1. Console.log statements that should be removed
2. TODO comments older than 30 days
3. Functions longer than 50 lines
4. Files with no tests
5. Deprecated API usage

Report findings grouped by category.
```

## Interactive Review Session

For complex reviews, work interactively:

```
Let's review the authentication module together.
Start by showing me the main entry points.
```

Claude shows the code. You ask questions:

```
What happens if the token is expired?
```

```
Show me everywhere we check permissions.
```

```
Is there any path where a user could access admin functions?
```

This conversational approach catches issues that automated scans miss.

## Fixing Review Findings

After identifying issues, fix them:

```
Fix all the security issues you found in the review.
Create a separate commit for each fix.
```

Claude implements fixes, you approve each one.

## Review Documentation

Generate a review report:

```
Create a code review report in markdown format:

1. Summary of changes reviewed
2. Findings by category
3. Recommendations
4. Files that need attention
5. Suggested follow-up work

Save as docs/review-report-YYYY-MM-DD.md
```

## Best Practices

**Be specific about what to look for.** "Review this code" is vague. "Check for SQL injection in database queries" is actionable.

**Use the right model.** Opus for security reviews (needs deep reasoning), Sonnet for style reviews (faster, cheaper).

**Review in context.** Give Claude access to related files, not just the changed ones.

**Iterate on findings.** Ask follow-up questions about unclear issues.

**Track findings.** Keep a log of common issues to improve your review commands over time.

Code review is where Claude Code provides tremendous value. Invest in building review workflows that match your team's standards.

> **See Also:**
> - [Slash Commands](06-Slash-Commands-and-Custom-Commands.md) for creating review commands
> - [Subagents](11-Subagents-Parallel-Intelligence.md) for parallel specialized reviews
> - [CI/CD Integration](26-CICD-Integration.md) for automated review pipelines

---

**Next:** [Chapter 19: Scripts and Automation](19-Scripts-and-Automation.md) — Build reusable automation scripts.
