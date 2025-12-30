# Chapter 3: Your First Session

Let's actually use Claude Code. Not a complex project—just enough to feel the rhythm of agentic development.

## Starting a Session

Create a new folder for this experiment:

```bash
mkdir hello-claude
cd hello-claude
```

Start Claude:

```bash
claude
```

You're now in an interactive session. Claude can see this directory (which is empty) and is waiting for your input.

## Your First Build

Type something like:

```
Create a Python script that asks for my name, then prints a personalized greeting that includes today's date.
```

Watch what happens.

Claude will propose creating a file:

```
I'll create a greeting script for you.

Create file: greeting.py
[shows the code]

Do you want me to create this file? (y/n)
```

This is the **supervision model** in action. Claude proposes; you approve.

Type `y`.

Claude creates the file. You'll see confirmation.

## Running Your Code

Now ask Claude to run it:

```
Run the script
```

Claude executes:

```
Running: python greeting.py

What is your name?
```

Type your name. The script runs, prints a greeting with today's date.

## Making Changes

Now say:

```
Make it also tell me what day of the week it is, and add an encouraging message for Mondays.
```

Claude proposes edits. You see exactly what will change. Approve with `y`.

The file updates. Run it again to verify.

## The Rhythm

Notice the pattern:

1. **You describe** what you want
2. **Claude proposes** specific actions (create file, edit file, run command)
3. **You see** exactly what Claude wants to do
4. **You approve** or reject
5. **Claude executes**

This happens for every file creation, every edit, every command. Nothing happens without your explicit approval.

You can also:
- Type `n` to reject an action
- Ask Claude to explain before you decide
- Ask for something different

## What Claude Can See

In your session, try:

```
What files are in this directory?
```

Claude lists them—because it can actually check. It's not guessing.

Try:

```
Read greeting.py and explain what each line does
```

Claude reads the actual file and explains it.

This is **information symmetry**. Claude sees what you see.

## Keyboard Shortcuts

Learn these immediately:

| Shortcut | Action |
|----------|--------|
| `Escape` | Stop Claude (not Ctrl+C, which exits entirely) |
| `Escape Escape` | Show message history to jump back |
| `Ctrl+V` | Paste images (not Cmd+V on Mac) |
| `Shift+drag` | Reference files when dragging into terminal |
| `Ctrl+R` | Search prompt history |

## Built-in Commands

Type `/help` to see available commands:

| Command | What It Does |
|---------|-------------|
| `/help` | Show all commands |
| `/clear` | Clear conversation history |
| `/compact` | Summarize and compress context |
| `/model` | Switch models (opus, sonnet, haiku) |
| `/cost` | Show token usage |
| `/exit` | End the session |

## Using Shell Commands

You can run shell commands directly by prefixing with `!`:

```
! ls -la
```

This bypasses Claude's conversational processing—useful for quick checks.

## Adding Context

Reference files with `@`:

```
Look at @greeting.py and add error handling
```

Drag files into the terminal to add them to context (hold Shift when dragging to reference rather than open).

Paste images directly (Ctrl+V, not Cmd+V) for visual context.

## Ending a Session

When you're done:

```
/exit
```

Or press `Ctrl+C`.

Your files remain. The session ends, but the work persists.

## What Just Happened

In five minutes, you:

- Started a Claude Code session
- Created a working Python script through conversation
- Ran it and saw results
- Modified it with natural language
- Experienced the propose-approve-execute cycle

This is the fundamental pattern. Everything else in this book builds on it.

## Tips for Beginners

**Be specific.** "Create a script that..." works better than "I need something for..."

**Iterate.** Your first description won't be perfect. Adjust and refine.

**Ask questions.** "Why did you do it that way?" "What does this line do?"

**Review proposals.** Don't blindly approve. Skim the important parts.

**Use Claude to learn.** Ask it to explain what it's doing as it goes.

The learning curve is short. Within a few sessions, the interaction becomes natural.

## What's Different

If you've used ChatGPT or Claude in a browser:

- No copying and pasting code
- No switching between windows
- No explaining file contents—Claude reads them
- No describing errors—Claude sees them
- Changes happen instantly, verifiably

This is what "agentic" means in practice. Claude operates in your environment, not just in conversation.

Ready for something more substantial? Next, let's understand [the permission model](04-The-Permission-Model.md) that keeps you in control, then learn how to [configure Claude for your specific workflow](05-CLAUDE-md-Your-Projects-Brain.md).
