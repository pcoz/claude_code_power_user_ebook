# Chapter 13: Headless Mode and the SDK

Claude Code isn't just an interactive tool—it's also a programmatic agent you can embed in scripts, pipelines, and applications. Headless mode and the Agent SDK let you use Claude Code without human interaction.

This is where automation gets serious.

## Headless Mode Basics

Add `-p` (or `--print`) to run Claude non-interactively:

```bash
claude -p "What files are in this directory?"
```

Claude processes the prompt, takes any necessary actions, and prints the result. No approval prompts, no interactive session.

## Output Formats

Control how Claude responds:

```bash
# Plain text (default)
claude -p "Explain this error" 

# JSON output
claude -p "List all TODO comments" --output-format json

# Streaming JSON
claude -p "Refactor this function" --output-format stream-json
```

JSON output is essential for parsing Claude's responses programmatically.

## Permissions in Headless Mode

Since there's no human to approve actions, you must pre-authorize tools:

```bash
# Allow specific tools
claude -p "Fix the linting errors" --allowedTools "Edit,Bash(npm run lint)"

# Allow read-only operations
claude -p "Analyze this codebase" --allowedTools "Read,Grep,Glob"

# Accept edits automatically
claude -p "Format all Python files" --permission-mode acceptEdits

# Accept everything (use carefully)
claude -p "Set up the project" --permission-mode acceptAll
```

## Practical Examples

### Batch File Processing

```bash
#!/bin/bash
# Process all Python files through Claude

for file in src/*.py; do
  claude -p "Review $file for security issues. Output only critical findings." \
    --allowedTools "Read" \
    --output-format json >> security-report.json
done
```

### CI Integration

```yaml
# .github/workflows/ai-review.yml
name: AI Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: AI Review
        run: |
          claude -p "Review the changes in this PR for issues. 
                     Focus on: bugs, security, performance.
                     Output as markdown." \
            --allowedTools "Read,Grep,Glob,Bash(git diff)" \
            > review.md
      - name: Post Review
        uses: actions/github-script@v7
        with:
          script: |
            const review = require('fs').readFileSync('review.md', 'utf8');
            github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: review
            });
```

### Issue Triage

```bash
#!/bin/bash
# Auto-label new GitHub issues

ISSUE_NUM=$1
claude -p "Analyze GitHub issue #$ISSUE_NUM and suggest appropriate labels.
           Use 'gh issue view $ISSUE_NUM' to get details.
           Then use 'gh issue edit $ISSUE_NUM --add-label <labels>' to apply.
           Only use labels that exist in the repo." \
  --allowedTools "Bash(gh *)"
```

### Migration Script

```bash
#!/bin/bash
# Migrate files from framework A to framework B

# Generate task list
claude -p "List all files that use FrameworkA. Output as JSON array of paths." \
  --allowedTools "Grep,Glob" \
  --output-format json > tasks.json

# Process each file
cat tasks.json | jq -r '.[]' | while read file; do
  claude -p "Migrate $file from FrameworkA to FrameworkB. 
             Return 'OK' if successful, 'FAIL' if not." \
    --allowedTools "Read,Edit" \
    --max-turns 10
done
```

## The Agent SDK

For deeper integration, use the Claude Agent SDK in Python or TypeScript.

### Python SDK

```python
from claude_code_sdk import query, ClaudeCodeOptions

options = ClaudeCodeOptions(
    system_prompt="You are a Python code reviewer focused on security",
    cwd="/path/to/project",
    allowed_tools=["Read", "Grep"],
    permission_mode="manual",
    max_turns=5
)

async for message in query("Review main.py for security issues", options=options):
    for block in message.content:
        if block.type == "text":
            print(block.text)
        elif block.type == "tool_use":
            print(f"Using tool: {block.tool_name}")
```

### TypeScript SDK

```typescript
import { query, ClaudeCodeOptions } from '@anthropic-ai/claude-code-sdk';

const options: ClaudeCodeOptions = {
  systemPrompt: "You are a code reviewer",
  cwd: process.cwd(),
  allowedTools: ["Read", "Grep", "Glob"],
  maxTurns: 10
};

for await (const message of query("Analyze this codebase", options)) {
  for (const block of message.content) {
    if (block.type === "text") {
      console.log(block.text);
    }
  }
}
```

## Parallel Execution

Run multiple Claude instances simultaneously:

```bash
#!/bin/bash
# Analyze 4 directories in parallel

directories=("src/api" "src/models" "src/utils" "src/components")

for dir in "${directories[@]}"; do
  claude -p "Analyze code quality in $dir. Output summary as JSON." \
    --allowedTools "Read,Grep,Glob" \
    --output-format json > "reports/${dir//\//-}.json" &
done

wait  # Wait for all background jobs
echo "All analyses complete"
```

## Subagents in Headless Mode

Define subagents for specialized tasks:

```bash
claude -p "Review this PR for security and code quality" \
  --agents '{
    "security-scanner": {
      "description": "Scans for security vulnerabilities",
      "prompt": "You are a security expert. Focus on OWASP top 10.",
      "tools": ["Read", "Grep"],
      "model": "opus"
    },
    "style-checker": {
      "description": "Checks code style and formatting",
      "prompt": "You are a code style expert.",
      "tools": ["Read"],
      "model": "haiku"
    }
  }' \
  --allowedTools "Read,Grep,Glob"
```

## Building Custom Tools

The SDK lets you build custom agentic tools:

```python
from claude_code_sdk import ClaudeSDKClient, ClaudeCodeOptions

async def custom_code_analyzer(directory: str):
    options = ClaudeCodeOptions(
        system_prompt="""You are a code analyzer.
        First explore the directory structure.
        Then analyze each file for patterns.
        Finally, produce a comprehensive report.""",
        cwd=directory,
        allowed_tools=["Read", "Grep", "Glob", "Bash(wc *)"],
        max_turns=20
    )
    
    async with ClaudeSDKClient(options=options) as client:
        await client.query("Analyze this codebase and produce a report")
        async for response in client.receive_response():
            yield response
```

## Error Handling

Handle failures gracefully:

```bash
result=$(claude -p "Fix the bug in utils.py" \
  --allowedTools "Read,Edit" \
  --max-turns 5 \
  2>&1)

if echo "$result" | grep -q "FAIL\|Error\|error"; then
  echo "Claude failed to fix the bug"
  echo "$result" >> error-log.txt
else
  echo "Bug fixed successfully"
fi
```

## Common Patterns

### Pre-Commit Hook

```bash
#!/bin/bash
# .git/hooks/pre-commit

# Get staged files
staged=$(git diff --cached --name-only --diff-filter=ACM)

if [ -n "$staged" ]; then
  claude -p "Review these staged files for obvious issues: $staged
             If there are critical problems, output 'BLOCK: reason'.
             Otherwise output 'OK'." \
    --allowedTools "Read" \
    --output-format json > /tmp/review.json
    
  if grep -q "BLOCK" /tmp/review.json; then
    echo "Commit blocked by AI review:"
    cat /tmp/review.json
    exit 1
  fi
fi
```

### Documentation Generator

```bash
claude -p "Generate API documentation for all endpoints in src/api/.
           Output as markdown suitable for README." \
  --allowedTools "Read,Grep,Glob" \
  > docs/API.md
```

### Test Generator Pipeline

```bash
# Find files without tests
claude -p "List source files in src/ that don't have corresponding test files" \
  --allowedTools "Glob" \
  --output-format json > untested.json

# Generate tests for each
cat untested.json | jq -r '.[]' | while read file; do
  claude -p "Generate comprehensive tests for $file" \
    --allowedTools "Read,Write" \
    --max-turns 10
done
```

## Rate Limits and Costs

Headless mode uses the same token budget as interactive mode. For high-volume automation:

- Batch requests when possible
- Use `haiku` for simple tasks
- Set `--max-turns` to prevent runaway costs
- Monitor usage with `--verbose`

## The Automation Mindset

Headless mode transforms Claude from a tool you use into a worker you deploy:

- **CI/CD**: Automated code review, documentation, testing
- **Batch processing**: Mass refactoring, migrations, analysis
- **Scheduled tasks**: Daily reports, maintenance, cleanup
- **Custom tools**: Build your own AI-powered utilities

The patterns from interactive use translate directly to automation. Start with scripts, evolve to pipelines, build toward integrated workflows.
