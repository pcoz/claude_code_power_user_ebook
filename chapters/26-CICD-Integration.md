# Chapter 26: CI/CD Integration

## What Is CI/CD?

CI/CD stands for Continuous Integration and Continuous Deployment (or Delivery). It's the practice of automating the steps between writing code and shipping it to users.

**Continuous Integration (CI):**
Every time someone pushes code, automated systems:
- Run the test suite
- Check code quality (linting, formatting)
- Build the application
- Report any failures

The goal: catch problems immediately, before they reach other developers or users.

**Continuous Deployment (CD):**
When code passes all checks, automated systems:
- Deploy to staging environments
- Run integration tests
- Deploy to production
- Monitor for issues

The goal: ship changes quickly and reliably, without manual deployment steps.

**Why it matters:**
Without CI/CD, deployment is manual, error-prone, and infrequent. With CI/CD, you can ship multiple times per day with confidence. The automation catches what humans miss.

**Common CI/CD platforms:**
- GitHub Actions (built into GitHub)
- GitLab CI/CD
- Jenkins
- CircleCI
- Azure DevOps

---

## Claude Code in CI/CD Pipelines

Claude Code isn't just for interactive development. It integrates into CI/CD pipelines for automated code review, documentation generation, test creation, and more.

The key insight: anything you can ask Claude to do interactively, you can automate in a pipeline.

## Headless Mode in Pipelines

The `-p` flag runs Claude non-interactively:

```bash
claude -p "Review the code changes for issues" \
  --allowedTools "Read,Grep,Glob" \
  --output-format json
```

This is the foundation for CI/CD integration.

## GitHub Actions Integration

### Automated Code Review

```yaml
# .github/workflows/ai-review.yml
name: AI Code Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
      
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      
      - name: Get changed files
        id: changed
        run: |
          echo "files=$(git diff --name-only origin/main...HEAD | tr '\n' ' ')" >> $GITHUB_OUTPUT
      
      - name: AI Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Review these changed files for bugs, security issues, and code quality: ${{ steps.changed.outputs.files }}
          
          Format your review as markdown with sections:
          - Summary
          - Critical Issues
          - Suggestions
          - Approval Status (Approve/Request Changes)" \
            --allowedTools "Read,Grep,Glob" \
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

### Issue Triage

```yaml
# .github/workflows/issue-triage.yml
name: AI Issue Triage

on:
  issues:
    types: [opened]

jobs:
  triage:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      
      - name: Analyze Issue
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          ISSUE_NUMBER: ${{ github.event.issue.number }}
        run: |
          claude -p "Analyze GitHub issue #$ISSUE_NUMBER using 'gh issue view'.
          
          Determine:
          1. Type: bug, feature, question, documentation
          2. Priority: critical, high, medium, low
          3. Components affected
          4. Suggested labels
          
          Then apply the suggested labels using 'gh issue edit'." \
            --allowedTools "Bash(gh *)"
```

### Documentation Generation

```yaml
# .github/workflows/docs.yml
name: Generate Documentation

on:
  push:
    branches: [main]
    paths: ['src/**']

jobs:
  docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      
      - name: Generate API Docs
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Generate API documentation for all endpoints in src/api/.
          Output as markdown. Include:
          - Endpoint URL and method
          - Request parameters
          - Response format
          - Example usage" \
            --allowedTools "Read,Grep,Glob" \
            > docs/API.md
      
      - name: Commit Documentation
        run: |
          git config user.name "Claude Bot"
          git config user.email "claude@example.com"
          git add docs/
          git diff --staged --quiet || git commit -m "docs: update API documentation"
          git push
```

## Pre-Commit Hooks

### Local Pre-Commit

```bash
#!/bin/bash
# .git/hooks/pre-commit

# Get staged files
staged=$(git diff --cached --name-only --diff-filter=ACM | grep -E '\.(ts|js|py)$')

if [ -n "$staged" ]; then
  echo "Running AI pre-commit check..."
  
  result=$(claude -p "Quick review of these staged files: $staged
  
  Check for:
  - Obvious bugs
  - Security issues (credentials, injection)
  - Console.log/print statements that should be removed
  
  If there are critical issues, respond with 'BLOCK: <reason>'
  Otherwise respond with 'OK'" \
    --allowedTools "Read" \
    --model haiku)
  
  if echo "$result" | grep -q "BLOCK:"; then
    echo "❌ Commit blocked by AI review:"
    echo "$result"
    exit 1
  fi
  
  echo "✅ AI review passed"
fi
```

### With Husky

```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "./scripts/ai-precommit.sh"
    }
  }
}
```

## GitLab CI Integration

```yaml
# .gitlab-ci.yml
ai-review:
  stage: test
  image: node:20
  script:
    - npm install -g @anthropic-ai/claude-code
    - |
      claude -p "Review merge request changes.
      Focus on: bugs, security, performance.
      Format as GitLab markdown." \
        --allowedTools "Read,Grep,Glob,Bash(git diff)" \
        > review.md
    - cat review.md
  artifacts:
    paths:
      - review.md
  only:
    - merge_requests
```

## Jenkins Pipeline

```groovy
// Jenkinsfile
pipeline {
    agent any
    
    environment {
        ANTHROPIC_API_KEY = credentials('anthropic-api-key')
    }
    
    stages {
        stage('AI Code Review') {
            steps {
                sh 'npm install -g @anthropic-ai/claude-code'
                sh '''
                    claude -p "Review all changed files in this build.
                    Output JSON with: {issues: [], suggestions: [], approved: boolean}" \
                      --allowedTools "Read,Grep,Glob" \
                      --output-format json \
                      > review.json
                '''
                
                script {
                    def review = readJSON file: 'review.json'
                    if (!review.approved) {
                        error "AI review found issues: ${review.issues}"
                    }
                }
            }
        }
    }
}
```

## Automated Testing Workflows

### Generate Missing Tests

```yaml
name: Test Coverage Check

on:
  pull_request:

jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Coverage
        run: npm test -- --coverage --json > coverage.json
      
      - name: Check for Missing Tests
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Analyze coverage.json and identify files with <80% coverage.
          For each low-coverage file, suggest specific test cases that would improve coverage.
          Format as a TODO list." \
            --allowedTools "Read" \
            > missing-tests.md
      
      - name: Post Coverage Report
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const report = fs.readFileSync('missing-tests.md', 'utf8');
            if (report.trim()) {
              github.rest.issues.createComment({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: context.issue.number,
                body: '## Test Coverage Suggestions\n\n' + report
              });
            }
```

## Batch Processing in CI

For large-scale operations:

```bash
#!/bin/bash
# scripts/batch-migrate.sh

# Generate task list
claude -p "List all files using the old API pattern" \
  --allowedTools "Grep,Glob" \
  --output-format json > tasks.json

# Process in parallel
cat tasks.json | jq -r '.files[]' | parallel -j 4 '
  claude -p "Migrate {} from old API to new API pattern" \
    --allowedTools "Read,Edit" \
    --permission-mode acceptEdits
'

# Verify all migrations
claude -p "Verify no files still use the old API pattern" \
  --allowedTools "Grep,Glob"
```

## Security Scanning

```yaml
name: AI Security Scan

on:
  push:
    branches: [main]
  schedule:
    - cron: '0 0 * * 1'  # Weekly

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      
      - name: Security Audit
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p "Perform a security audit of this codebase.
          
          Check for:
          - Hardcoded credentials or secrets
          - SQL injection vulnerabilities
          - XSS vulnerabilities
          - Insecure dependencies
          - Authentication/authorization issues
          - Sensitive data exposure
          
          Output a security report with severity ratings." \
            --allowedTools "Read,Grep,Glob,Bash(npm audit)" \
            --model opus \
            > security-report.md
      
      - name: Upload Report
        uses: actions/upload-artifact@v4
        with:
          name: security-report
          path: security-report.md
```

## Best Practices

### 1. Use Appropriate Models

```bash
# Quick checks: haiku
claude -p "..." --model haiku

# Standard reviews: sonnet (default)
claude -p "..."

# Deep analysis: opus
claude -p "..." --model opus
```

### 2. Set Tool Limits

```bash
# Read-only for reviews
--allowedTools "Read,Grep,Glob"

# Controlled bash access
--allowedTools "Read,Grep,Bash(npm test),Bash(git diff)"
```

### 3. Handle Failures

```yaml
- name: AI Task
  continue-on-error: true
  run: |
    claude -p "..." || echo "AI task failed, continuing..."
```

### 4. Cache Dependencies

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: npm-claude-code
```

### 5. Secure API Keys

```yaml
env:
  ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

Never commit API keys. Always use secrets management.

## Rate Limits and Costs

CI/CD can generate significant API usage. Monitor and control:

- Use `--max-turns` to limit conversation length
- Use haiku for high-volume tasks
- Cache results when possible
- Run expensive analyses on schedule, not every commit

## The CI/CD Pattern Library

| Task | Trigger | Model |
|------|---------|-------|
| Code review | PR opened | sonnet |
| Quick lint check | Pre-commit | haiku |
| Security audit | Weekly | opus |
| Documentation | Push to main | sonnet |
| Issue triage | Issue opened | haiku |
| Test generation | Coverage drop | sonnet |

Match the task to the right trigger and model for efficiency.
