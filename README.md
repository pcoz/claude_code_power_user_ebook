# Claude Code Power User

## From Terminal to Mastery

*Based on concepts from Vibe Coding with Structure and Style*
*by Edward Chalk*

---

## What This Book Is About

Claude Code is Anthropic's agentic coding tool that lives in your terminal. It understands your codebase, executes commands, modifies files, and helps you build software through natural language conversation. But that description undersells what's actually happening.

What's actually happening is this: **the barrier between having an idea and having working software has collapsed.**

You describe what you want. Claude builds it. You test it, describe what's wrong or what you want different. Claude fixes it. The loop is: describe → get code → test → describe adjustments → get updated code → repeat until done.

This is "vibe coding"—building software by expressing intent rather than writing syntax. It sounds too easy to be real. It is real. It changes everything about how software gets built.

---

## What You'll Learn

This book takes you from installation to mastery across three domains:

### The Tool Itself
- **Installation and authentication** — Getting Claude Code running on your machine
- **The permission model** — Understanding what Claude can and cannot do without asking
- **CLAUDE.md files** — Teaching Claude about your specific project
- **Custom commands and skills** — Extending Claude's capabilities
- **MCP servers** — Connecting Claude to databases, APIs, and external tools
- **Hooks** — Automating actions before and after Claude's work
- **Subagents** — Running multiple Claude instances in parallel
- **The SDK** — Building your own tools powered by Claude

### The Workflows
- **The development loop** — The rhythm of effective vibe coding
- **Test-driven development** — Writing tests first, letting Claude implement
- **Debugging** — Using Claude to find and fix problems
- **Code review** — Systematic review workflows
- **Git worktrees** — Parallel development across branches
- **Large codebases** — Navigation strategies that scale
- **CI/CD integration** — Claude in your deployment pipeline

### The Bigger Picture
- **Traditional APIs + AI APIs** — Combining data sources with intelligent interpretation
- **Qualitative meets quantitative** — When to use classical computing vs. AI
- **Program prompts** — The insight that prompts can be complete programs
- **Analytical frameworks** — Structured approaches to decision-making
- **Decision support systems** — Building tools that help people think better

---

## The Core Insight

Here's what most people miss about AI-assisted development:

**The AI that helps you write code is one thing. The AI inside your application—that runs when users interact with it—is another.**

Build-time AI (Claude in your terminal) helps you construct software. Runtime AI (Claude's API called from your code) powers intelligent features in the software itself.

This book covers both. You'll learn to use Claude Code to build applications, and you'll learn to build applications that themselves use Claude's intelligence.

The combination is powerful: you can now build software that would have required teams of specialists, and that software can do things that were previously impossible—understanding natural language, making judgments, synthesizing information.

---

## Who This Book Is For

**Developers** who want to build faster without sacrificing quality.

**Non-programmers** who have ideas and want to build them. Vibe coding makes software development accessible to anyone who can describe what they want clearly.

**Technical leaders** evaluating how AI changes software development practices.

**Anyone** who wants to understand what's now possible when the cost of turning ideas into working software approaches zero.

---

## The Philosophy

This book reflects a particular philosophy about AI-assisted development:

**Structure enables freedom.** Good documentation, clear project organization, and explicit requirements make Claude more effective, not less. The best vibe coders aren't the ones who wing it—they're the ones who set up their projects so Claude can understand them deeply.

**Iteration is the process.** Your first attempt won't be perfect. Neither will Claude's. The magic is in the loop: describe, test, refine. Each iteration teaches both you and the AI what you're actually building.

**Tools should amplify thinking.** The goal isn't to outsource thinking to AI. It's to build tools that help humans think better—decision support systems, analytical frameworks, structured approaches that AI can apply consistently.

**Prompts are programs.** For many qualitative tasks, a well-crafted prompt *is* the complete implementation. The prompt explains your framework; the AI applies it. Understanding this unlocks capabilities that have nothing to do with traditional code.

---

## How to Use This Book

**If you're new to Claude Code:** Start with Parts I and II. Get it installed, run through your first sessions, understand the core features.

**If you're already using Claude Code:** Skip to Part III (Power Features) and Part IV (Workflows). These contain the patterns that separate casual users from power users.

**If you want to build AI-powered applications:** Part V covers runtime AI integration—calling Claude's API from your own code.

**If you're interested in the bigger picture:** Part VIII explores analytical frameworks, decision support systems, and the philosophical implications of what's now possible.

**The Mini Labs (Chapter 30)** are designed for hands-on practice. Each takes 15-30 minutes and teaches a specific skill through doing.

---

## Table of Contents

### Part I: Getting Started
1. [Why Claude Code Changes Everything](chapters/01-Why-Claude-Code-Changes-Everything.md)
2. [Installation and Authentication](chapters/02-Installation-and-Authentication.md)
3. [Your First Session](chapters/03-Your-First-Session.md)
4. [The Permission Model](chapters/04-The-Permission-Model.md)

### Part II: Core Features
5. [CLAUDE.md — Your Project's Brain](chapters/05-CLAUDE-md-Your-Projects-Brain.md)
6. [Slash Commands and Custom Commands](chapters/06-Slash-Commands-and-Custom-Commands.md)
7. [Settings and Configuration](chapters/07-Settings-and-Configuration.md)
8. [The Model Selector](chapters/08-The-Model-Selector.md)

### Part III: Power Features
9. [MCP — Model Context Protocol](chapters/09-MCP-Model-Context-Protocol.md)
10. [Hooks — Automation Triggers](chapters/10-Hooks-Automation-Triggers.md)
11. [Subagents — Parallel Intelligence](chapters/11-Subagents-Parallel-Intelligence.md)
12. [Skills — Automatic Expertise](chapters/12-Skills-Automatic-Expertise.md)
13. [Headless Mode and the SDK](chapters/13-Headless-Mode-and-the-SDK.md)

### Part IV: Workflows That Work
14. [The Development Loop](chapters/14-The-Development-Loop.md)
15. [Git Worktrees for Parallel Development](chapters/15-Git-Worktrees-for-Parallel-Development.md)
16. [Test-Driven Development with Claude](chapters/16-Test-Driven-Development-with-Claude.md)
17. [Debugging and Error Recovery](chapters/17-Debugging-and-Error-Recovery.md)
18. [Code Review Workflows](chapters/18-Code-Review-Workflows.md)

### Part V: Building Real Software
19. [Scripts and Automation](chapters/19-Scripts-and-Automation.md)
20. [Full-Stack Applications](chapters/20-Full-Stack-Applications.md)
21. [API Integration Patterns](chapters/21-API-Integration-Patterns.md)
22. [AI-Powered Features at Runtime](chapters/22-AI-Powered-Features-at-Runtime.md)

### Part VI: Advanced Patterns
23. [Multi-Agent Orchestration](chapters/23-Multi-Agent-Orchestration.md)
24. [Large Codebase Navigation](chapters/24-Large-Codebase-Navigation.md)
25. [Migration and Refactoring at Scale](chapters/25-Migration-and-Refactoring-at-Scale.md)
26. [CI/CD Integration](chapters/26-CICD-Integration.md)

### Part VII: Working with Structure
27. [When You Need Documentation](chapters/27-When-You-Need-Documentation.md)
28. [The Document Stack](chapters/28-The-Document-Stack.md)
29. [A Complete Structured Build](chapters/29-A-Complete-Structured-Build.md)

### Part VIII: Analytical Frameworks and Decision Support
30. [Mini Labs — Hands-On Practice](chapters/30-Mini-Labs-Hands-On-Practice.md)
31. [Information Plus Understanding](chapters/31-Information-Plus-Understanding.md)
32. [Weaving AI with Classical Computing](chapters/32-Weaving-AI-With-Classical-Computing.md)
33. [Program Prompts](chapters/33-Program-Prompts.md)
34. [The EDA Analytical Framework](chapters/34-EDA-Analytical-Framework.md)
35. [Building Decision Support Systems](chapters/35-Building-Decision-Support-Systems.md)

### Appendices
- [A. Command Reference](chapters/Appendix-A-Command-Reference.md)
- [B. CLAUDE.md Templates](chapters/Appendix-B-CLAUDE-md-Templates.md)
- [C. MCP Server Catalog](chapters/Appendix-C-MCP-Server-Catalog.md)
- [D. Hook Recipes](chapters/Appendix-D-Hook-Recipes.md)
- [E. Troubleshooting](chapters/Appendix-E-Troubleshooting.md)

---

## About This Book

Whether you're automating tedious tasks, building full-stack applications, or orchestrating multi-agent workflows, this book gives you the patterns that work.

*Current as of December 2025*

---

## License

This work is provided for educational purposes. See individual chapters for specific code examples and their usage.
