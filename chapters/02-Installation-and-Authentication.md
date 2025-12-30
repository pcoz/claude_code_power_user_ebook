# Chapter 2: Installation and Authentication

Before you can use Claude Code, you need two things: the right subscription plan and the software itself.

## Subscription Requirements

Claude Code requires one of the following:

| Plan | Monthly Cost | Notes |
|------|-------------|-------|
| Claude Pro | $20 | Includes CLI access with usage limits |
| Claude Max | $100 or $200 | Higher limits for heavy usage |
| Claude Team | Per-seat pricing | For organizations |
| API Credits | Pay-as-you-go | Direct API access |

If you only have the free Claude tier, you'll need to upgrade. The Pro plan is sufficient for learning and moderate usage.

**Recommendation**: If you're using Claude Code consistently, the subscription is worth it. You get predictable costs without worrying about per-token charges.

## Installation

### macOS

Open Terminal and install Node.js if you haven't:

```bash
brew install node
```

If you don't have Homebrew, install Node.js from nodejs.org instead.

Install Claude Code:

```bash
npm install -g @anthropic-ai/claude-code
```

Verify:

```bash
claude --version
```

### Windows

**Option 1: Windows Terminal with PowerShell**

Install Node.js from nodejs.org (download the Windows installer).

Open PowerShell or Windows Terminal:

```powershell
npm install -g @anthropic-ai/claude-code
```

**Option 2: WSL (Windows Subsystem for Linux)**

This gives you a Linux environment inside Windows, which many developers prefer.

Enable WSL (run in PowerShell as Administrator):

```powershell
wsl --install
```

Restart your computer, then open Ubuntu from the Start menu:

```bash
sudo apt update
sudo apt install nodejs npm
npm install -g @anthropic-ai/claude-code
```

### Linux

Open your terminal:

```bash
sudo apt update
sudo apt install nodejs npm
npm install -g @anthropic-ai/claude-code
```

Or use your distribution's package manager (dnf, pacman, etc.).

## Authentication

The first time you run `claude`, it will ask you to authenticate:

```bash
claude
```

Follow the prompts:

1. A browser window opens
2. Log into your Anthropic account
3. Authorize Claude Code
4. Return to terminal

Once authenticated, you're ready to use Claude Code.

### API Key Alternative

If you prefer API access over a subscription:

```bash
export ANTHROPIC_API_KEY="sk-ant-your-key-here"
```

Add this to your shell profile (`~/.bashrc`, `~/.zshrc`) for persistence.

## Verifying Installation

Navigate to any folder and run:

```bash
claude
```

You should see Claude's interface. Type a simple message like "hello" to confirm it responds.

Type `/exit` or press `Ctrl+C` to end the session.

## The VS Code Extension

Claude Code also offers a VS Code extension for those who prefer IDE integration:

1. Open VS Code
2. Go to Extensions (Cmd+Shift+X or Ctrl+Shift+X)
3. Search for "Claude Code"
4. Install the extension

The extension provides:
- Sidebar panel for Claude interactions
- Inline diff viewing
- Multiple parallel sessions
- Integration with your editor

You can use the terminal CLI, VS Code extension, or both.

## Configuration Locations

After installation, Claude Code creates configuration directories:

```
~/.claude/
+-- settings.json      # User settings
+-- CLAUDE.md          # Global instructions
+-- commands/          # Personal slash commands
+-- agents/            # Subagent definitions
+-- skills/            # Personal skills
```

We'll explore these throughout the book.

## Troubleshooting

### "command not found: claude"

The npm global bin directory isn't in your PATH.

Find it:
```bash
npm bin -g
```

Add to PATH in your shell profile:
```bash
export PATH="$PATH:$(npm bin -g)"
```

### "Authentication failed"

Ensure you have an active Pro, Max, or Team subscription, or a valid API key set.

### Permission errors on npm install

Mac/Linux: Try with sudo:
```bash
sudo npm install -g @anthropic-ai/claude-code
```

Windows: Run terminal as Administrator.

### Behind a corporate proxy

Set proxy environment variables:
```bash
export HTTP_PROXY="http://proxy.company.com:8080"
export HTTPS_PROXY="http://proxy.company.com:8080"
```

## Keeping Updated

Claude Code updates frequently. Update with:

```bash
npm update -g @anthropic-ai/claude-code
```

Check your version:
```bash
claude --version
```

## What's Next

With Claude Code installed and authenticated, you're ready for your first session. Let's build something.
