# Chapter 37: Divide and Conquer

Sometimes Claude gets stuck. You describe a problem, Claude attempts a fix, the fix doesn't work, Claude tries another approach, that breaks something else, Claude fixes that, which reintroduces the original problem. Around and around.

This isn't failure. It's a signal that the problem is too large for the current context. The solution is divide and conquer: break the problem into smaller pieces until each piece is small enough to solve cleanly.

---

## Recognizing the Loop

You're in a fix-break-fix loop when:

- Claude's "fix" reintroduces a problem from two attempts ago
- Each solution creates a new problem of similar complexity
- The same files keep getting modified back and forth
- Claude starts apologizing and trying "one more approach"
- You've been at the same issue for 20+ minutes with no progress

This happens because the problem has too many interacting parts. Claude can see each part individually but loses track of how they affect each other. The context window fills with failed attempts, making it harder to see clearly.

---

## The Traditional Cost Problem

In traditional development, when you hit a wall, you have limited options:

1. **Keep debugging** — Hours of stepping through code
2. **Rewrite the component** — Days of work, high risk
3. **Work around it** — Technical debt accumulates
4. **Get help** — Wait for someone with more expertise

All of these are expensive. So developers push through, trying to fix the existing code even when starting fresh would be faster.

---

## The AI Advantage: Cheap Rewrites

With AI, rewriting is cheap. You can say:

```
Stop. Let's try a different approach.

Write a complete, self-contained module that handles [specific thing].
It should:
- Take [these inputs]
- Return [these outputs]
- Not depend on [the problematic code]

Make it work in isolation first. We'll integrate it after.
```

This takes minutes, not days. If the new module doesn't work, you've lost little. If it does, you've bypassed the problem entirely.

---

## The Divide and Conquer Pattern

### Step 1: Stop the Loop

When you notice circular fixes, stop immediately. Don't let Claude try "one more thing." The loop will continue.

```
Stop. We're going in circles. Let's step back and try a different approach.
```

### Step 2: Identify the Core Problem

What specifically isn't working? Strip away everything else.

```
The actual problem is: [specific thing] doesn't [specific behavior].
Everything else is working. Let's focus only on this.
```

### Step 3: Extract to Isolated Module

Ask Claude to write a complete, standalone solution:

```
Write a new module called [name] that:
- Takes: [exact inputs]
- Returns: [exact outputs]
- Handles: [edge cases]

Don't modify any existing code yet. Just write this module
so it works perfectly in isolation.

Include a test that proves it works.
```

### Step 4: Test in Isolation

Run the isolated module. Does it work? If not, you now have a smaller problem to debug. If yes, proceed to integration.

### Step 5: Integrate

Only after the module works in isolation:

```
Now integrate this module into the main code.
Replace [the problematic code] with a call to this module.
```

### Step 6: Repeat If Needed

If integration fails, the integration itself is now the problem—a smaller problem than before. Apply the same pattern: isolate, solve, integrate.

---

## Example: The Authentication Loop

**The Problem:**

You're adding authentication to an Express app. Claude implements it, but:
- Login works, but session doesn't persist
- Fix session, now logout breaks
- Fix logout, now login fails for some users
- Fix that edge case, session persistence breaks again

**The Loop:**

```
Claude: Fixed the session persistence by changing the cookie settings.
You: Now logout doesn't clear the session.
Claude: I see, let me adjust the logout handler...
You: That broke login for users with existing sessions.
Claude: Ah, I need to handle that case...
[20 minutes later, back to session persistence issues]
```

**The Solution:**

```
Stop. We're going in circles.

Write me a complete, standalone authentication module with these specs:

File: auth/authModule.js

Exports:
- createSession(userId) → returns session token
- validateSession(token) → returns userId or null
- destroySession(token) → returns boolean

Requirements:
- Sessions expire after 24 hours
- Use a simple in-memory store for now
- No dependencies on Express or our existing code

Include a test file that proves all three functions work.
```

Claude writes a clean 50-line module. You run the tests. They pass.

```
Now create an Express middleware that uses this module:

- POST /login calls createSession after password verification
- All protected routes call validateSession
- POST /logout calls destroySession

Keep the authModule unchanged. It works. Only write the Express integration.
```

Integration works first try because the hard part (session logic) is already solved and tested.

---

## Example: The Data Processing Loop

**The Problem:**

You're building a CSV processor that:
- Reads large files
- Validates each row
- Transforms data
- Writes to database

Claude's implementation keeps crashing on large files, and fixes for memory issues break the validation, fixes for validation break the transforms...

**The Solution:**

Break into four independent modules:

```
Module 1: CSV Reader
Write a module that reads a CSV file in chunks.
- Input: file path, chunk size
- Output: yields arrays of row objects
- Test: read a 10MB file without memory issues

Module 2: Row Validator
Write a module that validates a single row.
- Input: row object, schema definition
- Output: { valid: boolean, errors: string[] }
- Test: validate 10 sample rows with known good/bad data

Module 3: Data Transformer
Write a module that transforms a validated row.
- Input: validated row
- Output: transformed object ready for database
- Test: transform 10 sample rows

Module 4: Database Writer
Write a module that writes a batch of rows.
- Input: array of transformed objects
- Output: { written: number, failed: number }
- Test: write 100 rows to test database
```

Each module is simple enough to get right. Then:

```
Now write a pipeline that connects these four modules:
- Reader yields chunks
- Each row goes through Validator
- Valid rows go through Transformer
- Transformed rows batch to Writer

The modules are tested and working. Only write the orchestration.
```

---

## Example: The UI Component Loop

**The Problem:**

A React component with complex state keeps breaking:
- Fix the initial render, break the update
- Fix the update, break the reset
- Fix the reset, break the initial render

**The Solution:**

```
Stop. Let's separate concerns.

Module 1: State Logic (no React)
Write a pure JavaScript class that manages this state:
- constructor(initialData)
- update(changes) → new state
- reset() → initial state
- validate() → { valid, errors }

No React, no hooks, no rendering. Just state logic.
Test it with plain JavaScript.

Module 2: React Wrapper
Now write a React hook that wraps this class:
- useMyState(initialData)
- Returns: { state, update, reset, errors }

The class does the hard work. The hook just connects it to React.
```

---

## How Small Is Small Enough?

Keep dividing until:

- The module does **one thing**
- It has **clear inputs and outputs**
- You can **test it in isolation**
- Claude gets it **right on the first or second try**

If Claude struggles with a module, it's still too big. Split again.

---

## The Integration Sandwich

Once you have working modules, integration follows a pattern:

```
+-----------------------------+
|     Orchestration Layer     |  <-- Write last
+-----------------------------+
|  Module A  |  Module B  |   |  <-- Write and test first
+-----------------------------+
|     Shared Types/Interfaces |  <-- Define first
+-----------------------------+
```

1. **Define interfaces first** — What data flows between modules?
2. **Build modules in isolation** — Each one works alone
3. **Write orchestration last** — Just wiring, no logic

If orchestration has logic, it's doing too much. Extract another module.

---

## When to Use This Pattern

**Use divide and conquer when:**

- You've been stuck for more than 15-20 minutes
- Fixes keep breaking other things
- The problem involves multiple interacting concerns
- Claude's attempts are getting longer and more complicated

**Don't use it for:**

- Simple bugs (typos, off-by-one errors)
- Problems you haven't actually tried to solve yet
- Issues where you don't understand the requirements

---

## The Mindset Shift

Traditional development trains you to fix existing code. Rewriting feels like defeat.

With AI, rewriting is a tool. A good tool. Often the fastest path forward.

When you're stuck:
- Don't ask "how do I fix this code?"
- Ask "what's the smallest piece I can extract and rewrite cleanly?"

The answer is usually smaller than you think. A 10-line function. A single validation step. One database query.

Extract it. Rewrite it. Test it. Integrate it.

Progress resumes.

---

## Summary

When Claude goes in circles:

1. **Stop** — Don't let it try one more fix
2. **Identify** — What's the core problem?
3. **Extract** — Write it as an isolated module
4. **Test** — Make sure it works alone
5. **Integrate** — Connect it to the rest
6. **Repeat** — Split further if needed

This works because:
- Smaller problems are easier to solve
- Isolated modules are easier to test
- AI makes rewriting cheap
- Fresh context beats polluted context

The loop isn't failure. It's information. The problem is too big. Make it smaller.

> **See Also:**
> - [The Development Loop](14-The-Development-Loop.md) for the overall workflow
> - [Debugging and Error Recovery](17-Debugging-and-Error-Recovery.md) for systematic debugging approaches
> - [Test-Driven Development](16-Test-Driven-Development-with-Claude.md) for testing isolated modules
