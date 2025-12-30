# Appendix E: Troubleshooting

Common problems and their solutions.

## Installation Issues

### "command not found: claude"
The npm global bin directory isn't in your PATH.

**Find the directory:**
```bash
npm bin -g
```

**Add to PATH (add to ~/.bashrc or ~/.zshrc):**
```bash
export PATH="$PATH:$(npm bin -g)"
```

**Reload shell:**
```bash
source ~/.bashrc  # or source ~/.zshrc
```

### "EACCES permission denied" during npm install
npm doesn't have permission to install globally.

**Option 1: Use sudo (Mac/Linux):**
```bash
sudo npm install -g @anthropic-ai/claude-code
```

**Option 2: Fix npm permissions:**
```bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
npm install -g @anthropic-ai/claude-code
```

### "node: command not found"
Node.js isn't installed.

**macOS:**
```bash
brew install node
```

**Ubuntu/Debian:**
```bash
sudo apt update && sudo apt install nodejs npm
```

**Windows:**
Download from nodejs.org

### Node version too old
Claude Code requires Node.js 18+.

**Check version:**
```bash
node --version
```

**Update Node.js:**
```bash
# Using nvm (recommended)
nvm install 20
nvm use 20

# Or reinstall from nodejs.org
```

## Authentication Issues

### "Not authenticated"
Run authentication:
```bash
claude auth login
```

Or check status:
```bash
claude auth status
```

### "Subscription required"
Claude Code requires Claude Pro, Max, Team, or API credits.

**Check your subscription** at claude.ai/settings.

### API key not working
Verify the key is set correctly:
```bash
echo $ANTHROPIC_API_KEY
```

**Set if missing:**
```bash
export ANTHROPIC_API_KEY="sk-ant-your-key"
```

**Add to shell profile for persistence:**
```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-your-key"' >> ~/.bashrc
```

## Session Issues

### Claude seems confused or inconsistent
Context may be overloaded or contradictory.

**Try:**
```
/clear
```

Or start a fresh session:
```bash
exit
claude
```

### Claude keeps making the same mistake
Rephrase the problem differently:
```
Let's try a different approach. Instead of X, let's do Y.
```

Or provide explicit constraints:
```
Do NOT use library X. Use library Y instead.
```

### Session freezes or times out
- Check internet connection
- Try a simpler request to test
- Restart with: `Ctrl+C` then `claude`

### Context window full
```
/compact
```

This summarizes the conversation, freeing space.

## File Operation Issues

### "Permission denied" on file operations
Check file permissions:
```bash
ls -la filename
```

**Fix permissions:**
```bash
chmod 644 filename  # Read/write for owner
```

### Changes not appearing
- Verify the file path is correct
- Check if you're in the right directory
- Look for the file with `ls` or `find`

### Wrong file modified
Always verify file paths before approving. When ambiguous:
```
Which file exactly will this change? Show me the full path.
```

## Code Execution Issues

### "ModuleNotFoundError" in Python
Install the missing package:
```bash
pip install package-name
```

Or in a virtual environment:
```bash
python -m venv venv
source venv/bin/activate
pip install package-name
```

### "Cannot find module" in Node.js
Install the missing package:
```bash
npm install package-name
```

### Script runs but does nothing
Add debugging:
```
Add print statements to debug why the script isn't working.
```

Or ask Claude to explain:
```
Walk me through what this script does step by step.
```

## API Issues (When calling Claude's API)

### "401 Unauthorized"
API key is wrong or missing.
```bash
echo $ANTHROPIC_API_KEY  # Check if set
```

### "429 Too Many Requests"
Rate limit hit. Wait and retry, or add delays:
```
Add rate limiting with 1 second delay between requests.
```

### "400 Bad Request"
Usually a malformed request. Check:
- Message format is correct
- Model name is valid
- Max tokens is within limits

### Connection errors
- Check internet connectivity
- API might be temporarily down
- Try again in a few minutes

## MCP Issues

### MCP server not connecting
**Enable debug mode:**
```bash
claude --mcp-debug
```

**Common causes:**
- Server not installed: `npm install -g package-name`
- Wrong command path
- Missing environment variables

### "Tool not found"
The MCP server might not be configured correctly.

**List configured servers:**
```bash
claude mcp list
```

**Verify the server is running** by checking debug output.

## Hooks Issues

### Hooks not running
1. Check hook configuration syntax
2. Verify matcher pattern matches the tool
3. Review with `/hooks` and approve changes

### Hook blocking unexpectedly
Check PreToolUse hooks that might be returning non-zero exit codes.

### Hook command failing
Test the command manually:
```bash
black /path/to/file.py
```

If it fails manually, fix the underlying issue first.

## Performance Issues

### Claude is slow
- Large context (many files) slows responses
- Use `/compact` to reduce context
- Be more specific to reduce searching

### High token usage
- Review what files Claude is reading
- Use targeted requests instead of broad exploration
- Consider using haiku for simple tasks

## Getting More Help

### Check logs
Claude Code logs to `~/.claude/logs/`

### Report bugs
Use `/bug` in a session to report issues to Anthropic.

### Community resources
- Anthropic Discord: Claude Developers channel
- GitHub Issues: github.com/anthropics/claude-code
- Documentation: docs.anthropic.com

### Debug mode
For detailed diagnostics:
```bash
CLAUDE_CODE_DEBUG=1 claude
```

## Quick Diagnostic Checklist

When something isn't working:

1. ☐ Is Claude Code installed? `claude --version`
2. ☐ Are you authenticated? `claude auth status`
3. ☐ Is your subscription active?
4. ☐ Is your internet connection working?
5. ☐ Are you in the right directory? `pwd`
6. ☐ Do you have file permissions? `ls -la`
7. ☐ Is the syntax correct? (for code issues)
8. ☐ Are dependencies installed?
9. ☐ Have you tried `/clear` or restarting?
10. ☐ Can you reproduce with a minimal example?
