# Chapter 15: Git Worktrees for Parallel Development

One of the most powerful advanced patterns is running multiple Claude instances simultaneously on different parts of your codebase. Git worktrees make this possible without file conflicts.

## The Problem

You want Claude to work on multiple features in parallel. But if two Claude instances edit the same file, chaos ensues.

The solution: give each Claude instance its own working directory with its own branch.

## What Are Git Worktrees?

Git worktrees let you check out multiple branches simultaneously in different directories. Each worktree:

- Has its own working directory
- Points to a specific branch
- Shares the same git history
- Can be merged back to main

## Setting Up Worktrees

```bash
# From your main project directory
cd my-project

# Create a worktree for feature A
git worktree add ../my-project-feature-a feature/feature-a

# Create a worktree for feature B  
git worktree add ../my-project-feature-b feature/feature-b

# Create a worktree for a bug fix
git worktree add ../my-project-bugfix fix/critical-bug
```

Now you have:
```
~/projects/
├── my-project/           # main branch
├── my-project-feature-a/ # feature/feature-a branch
├── my-project-feature-b/ # feature/feature-b branch
└── my-project-bugfix/    # fix/critical-bug branch
```

## Running Parallel Claude Sessions

Open multiple terminal windows or tabs:

**Terminal 1:**
```bash
cd ~/projects/my-project-feature-a
claude
# "Implement the user authentication feature"
```

**Terminal 2:**
```bash
cd ~/projects/my-project-feature-b
claude
# "Build the payment processing module"
```

**Terminal 3:**
```bash
cd ~/projects/my-project-bugfix
claude
# "Fix the memory leak in the cache system"
```

Each Claude works independently on its own branch.

## The Workflow

### 1. Create Branches and Worktrees

```bash
# Create feature branches
git checkout -b feature/auth
git checkout main
git checkout -b feature/payments
git checkout main
git checkout -b fix/cache-leak
git checkout main

# Create worktrees
git worktree add ../project-auth feature/auth
git worktree add ../project-payments feature/payments
git worktree add ../project-cache fix/cache-leak
```

### 2. Run Parallel Sessions

Launch Claude in each worktree. Give each a focused task.

### 3. Review Changes

As each Claude finishes, review the changes:

```bash
cd ../project-auth
git diff main
git log main..HEAD
```

### 4. Merge Back

```bash
# From main project
cd ~/projects/my-project
git checkout main

# Merge each feature
git merge feature/auth
git merge feature/payments
git merge fix/cache-leak
```

### 5. Cleanup

```bash
# Remove worktrees when done
git worktree remove ../project-auth
git worktree remove ../project-payments
git worktree remove ../project-cache

# Delete branches if desired
git branch -d feature/auth feature/payments fix/cache-leak
```

## Managing Multiple Sessions

### Using tmux

```bash
# Start tmux session
tmux new-session -d -s claude-parallel

# Create windows for each worktree
tmux new-window -t claude-parallel -n auth
tmux send-keys -t claude-parallel:auth "cd ~/projects/project-auth && claude" Enter

tmux new-window -t claude-parallel -n payments
tmux send-keys -t claude-parallel:payments "cd ~/projects/project-payments && claude" Enter

tmux new-window -t claude-parallel -n cache
tmux send-keys -t claude-parallel:cache "cd ~/projects/project-cache && claude" Enter

# Attach to session
tmux attach -t claude-parallel
```

Navigate between windows with `Ctrl+B` then window number.

### Using VS Code

With the Claude Code VS Code extension:

1. Open each worktree as a separate VS Code window
2. Open Claude Code panel in each window
3. Run different tasks in each

## Parallel Subagents Alternative

For simpler parallelization without worktrees, use subagents:

```
I need to work on three areas in parallel:
1. Subagent 1: Refactor the authentication module
2. Subagent 2: Optimize database queries
3. Subagent 3: Add input validation to forms

Use separate subagents for each task. They should not modify the same files.
```

Claude spawns three subagents with isolated contexts.

**When to use worktrees vs subagents:**

| Worktrees | Subagents |
|-----------|-----------|
| Longer-running tasks | Quick parallel tasks |
| Need separate git history | Single commit desired |
| Human review at each step | Automated synthesis |
| Independent features | Related subtasks |

## Automation Script

```bash
#!/bin/bash
# parallel-claude.sh - Run Claude on multiple worktrees

PROJECT_ROOT=$(pwd)
WORKTREE_DIR="${PROJECT_ROOT}-worktrees"

# Create worktree directory
mkdir -p "$WORKTREE_DIR"

# Tasks to run (branch-name:prompt pairs)
tasks=(
  "feature/new-api:Implement the REST API for user profiles"
  "feature/ui-update:Update the dashboard UI with new charts"
  "fix/validation:Add input validation to all form fields"
)

# Create worktrees and launch Claude
for task in "${tasks[@]}"; do
  branch="${task%%:*}"
  prompt="${task#*:}"
  dir_name="${branch//\//-}"
  
  # Create branch if it doesn't exist
  git branch "$branch" 2>/dev/null || true
  
  # Create worktree
  git worktree add "$WORKTREE_DIR/$dir_name" "$branch"
  
  # Launch Claude in background
  (
    cd "$WORKTREE_DIR/$dir_name"
    claude -p "$prompt" \
      --allowedTools "Read,Write,Edit,Bash(npm *),Bash(git *)" \
      --permission-mode acceptEdits \
      > "../$dir_name.log" 2>&1
    echo "Completed: $branch"
  ) &
done

echo "Launched ${#tasks[@]} parallel Claude sessions"
echo "Logs in: $WORKTREE_DIR/*.log"
echo "Run 'wait' to block until all complete"
```

## Handling Conflicts

When parallel work creates conflicts:

```bash
# Try to merge
git merge feature/auth
# If conflicts, either:

# Option 1: Manual resolution
git status  # See conflicting files
# Edit files to resolve
git add .
git commit

# Option 2: Claude-assisted resolution
claude
# "Resolve the merge conflicts in these files:
#  - src/auth/login.ts
#  - src/types/user.ts
# Preserve functionality from both branches."
```

## Best Practices

1. **Scope tasks clearly** — Each worktree should have a distinct focus with minimal file overlap

2. **Keep main updated** — Periodically merge main into feature branches to reduce conflicts

3. **Review before merging** — Each parallel effort should be reviewed independently

4. **Use descriptive branches** — `feature/user-auth` is better than `feature/task1`

5. **Document progress** — Update CLAUDE.md in each worktree with progress

6. **Clean up** — Remove worktrees when done to avoid stale directories

## The Productivity Multiplier

Parallel development transforms how much you can accomplish:

- **Sequential**: Task A (2 hours) → Task B (2 hours) → Task C (2 hours) = 6 hours
- **Parallel**: Tasks A, B, C simultaneously = 2 hours + merge time

For independent features, parallel Claude sessions can 3-5x your velocity. The setup overhead pays for itself on the first use.
