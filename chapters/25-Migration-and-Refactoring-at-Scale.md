# Chapter 25: Migration and Refactoring at Scale

Renaming a function used in 3 files is easy. Renaming one used in 300 files is where Claude Code shines.

## The Challenge of Scale

Large refactoring is tedious and error-prone when done manually:
- Find all usages (miss some? bugs)
- Update each one (typo? bugs)
- Test everything (forget a case? bugs)

Claude Code automates the tedium while you maintain control.

## Simple Rename Refactoring

```
Rename the function `getUserData` to `fetchUserProfile` across the entire codebase.
Update all imports and usages.
Show me what files will change before making changes.
```

Claude:
1. Searches for all usages
2. Shows you the list
3. Waits for approval
4. Makes all changes

## API Migration

Migrating from one library to another:

```
Migrate from moment.js to date-fns:
1. Find all moment imports
2. For each file, convert moment calls to date-fns equivalents
3. Update package.json
4. Remove moment from dependencies

Show me the migration plan before starting.
```

## Database Schema Migration

```
We're changing the user table:
- Rename 'userName' to 'username'
- Split 'fullName' into 'firstName' and 'lastName'

1. Create the database migration
2. Update all code that references these fields
3. Update any API responses
4. Update frontend forms and displays
```

## Large-Scale Pattern Changes

Convert callback-based code to async/await:

```
Find all callback-style async code in src/api/.
Convert to async/await pattern.
Maintain the same behavior.
Test after each file.
```

## The Parallel Approach

For massive refactoring, use headless mode with parallelization:

```bash
#!/bin/bash
# Migrate 300 files from React class components to functional components

# Get list of files to migrate
claude -p "List all React class components in src/" \
  --allowedTools "Grep,Glob" \
  --output-format json > components.json

# Migrate each file
cat components.json | jq -r '.[]' | while read file; do
  claude -p "Convert $file from class component to functional component with hooks. Maintain the same props interface and behavior." \
    --allowedTools "Read,Edit" \
    --max-turns 5 &
done

wait
echo "Migration complete"
```

## Worktree Strategy for Safety

For risky refactoring:

```bash
# Create a separate worktree for the migration
git worktree add ../project-migration migration/big-refactor

cd ../project-migration
claude
# Do the refactoring in isolation
```

If it goes wrong, delete the worktree. Main branch is untouched.

## Incremental Migration

Sometimes you can't migrate everything at once:

```
We're migrating from Redux to Zustand incrementally.
Create an adapter layer that allows both to coexist.
Migrate the auth store first as a pilot.
```

Then in future sessions:
```
Continue the Redux to Zustand migration.
Which stores are left?
Migrate the next one.
```

## Automated Testing During Migration

```
After each file migration:
1. Run the tests for that module
2. If tests fail, stop and report
3. If tests pass, continue to next file

Track: migrated files, skipped files, failed files
```

## Finding What to Migrate

### Deprecated API Usage

```
Find all usage of deprecated React lifecycle methods:
- componentWillMount
- componentWillReceiveProps
- componentWillUpdate

Show file, line number, and what it should become.
```

### Security Vulnerabilities

```
Find all instances of:
- eval() usage
- innerHTML assignment
- SQL string concatenation
- exec() with user input

Create a prioritized list for remediation.
```

### Performance Issues

```
Find patterns that suggest N+1 query problems:
- Database calls inside loops
- Missing includes/joins in ORM queries
- Multiple sequential fetches that could be batched
```

## Creating Migration Guides

Document the migration for the team:

```
Create a migration guide for the moment.js to date-fns change:
- Why we're migrating
- What changes are needed
- Common patterns and their equivalents
- How to test after migrating
- FAQ for common issues
```

## Rollback Planning

Always have a way back:

```
Before starting the migration:
1. Create a git tag: pre-migration-DATE
2. Document the current behavior
3. Create rollback scripts if needed
4. Test the rollback procedure
```

## Migration Checklist Command

Create a reusable command:

`.claude/commands/migrate.md`:
```markdown
---
description: Run a structured migration
---

Migration: $ARGUMENTS

Phase 1 - Analysis:
1. Find all code affected by this change
2. Categorize by complexity (simple/medium/complex)
3. Identify test coverage for affected code
4. Report findings before proceeding

Phase 2 - Planning:
1. Propose migration order (start with well-tested code)
2. Identify dependencies between changes
3. Create rollback checkpoints
4. Estimate effort

Phase 3 - Execution (after approval):
1. Migrate simple cases first
2. Run tests after each file
3. Commit in logical groups
4. Document any manual steps needed

Phase 4 - Verification:
1. Run full test suite
2. Verify no regressions
3. Update documentation
4. Create completion report
```

## The Safety Mindset

Large refactoring is high-risk. Protect yourself:

**Small commits.** Each logical change gets a commit. Easy to bisect if something breaks.

**Continuous testing.** Run tests after each change, not just at the end.

**Feature flags.** Deploy behind flags when possible. Roll back instantly if needed.

**Staged rollout.** Migrate one module first. Prove it works. Then continue.

**Documentation.** Future you (and your team) will want to know what changed and why.

Claude Code makes large refactoring possible. Your job is to make it safe.

> **See Also:**
> - [Large Codebase Navigation](24-Large-Codebase-Navigation.md) for understanding what to change
> - [Git Worktrees for Parallel Development](15-Git-Worktrees-for-Parallel-Development.md) for safe experimentation
> - [CI/CD Integration](26-CICD-Integration.md) for automated testing during migration

---

**Next:** [Chapter 26: CI/CD Integration](26-CICD-Integration.md) — Automate your development pipeline.
