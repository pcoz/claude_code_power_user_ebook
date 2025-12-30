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

## The Qualitative Revolution

For decades, software has been spectacular at one thing: **measuring**. Counting, calculating, comparing, storing. Classical computing handles quantitative work with perfect accuracy at incredible speed.

But classical computing has always been terrible at **judging**. Is this customer satisfied? Is this argument persuasive? Is this opportunity worth pursuing? These are qualitative questions—they require interpretation, context, nuance. Computers couldn't answer them. They could only check whether something matched a rule that a human wrote in advance.

This created what we call "the squeeze": organizations forced qualitative reality through quantitative bottlenecks. A loan officer's judgment ("this person seems reliable despite the rough patch") became a credit score formula. A hiring manager's assessment ("great culture fit, high potential") became a checklist. Rich human judgment compressed into yes/no rules.

**AI changes the boundary.** AI can accept nuance directly. You can say: "This applicant's credit score is low, but look at their situation—medical bills tanked their score, they've paid everything on time since, income has doubled." AI can weigh this. It can reason about context and form a judgment.

This means applications can now combine both:
- **Classical computing** for measurement: precise calculations, reliable storage, fast retrieval
- **AI** for judgment: interpretation, synthesis, nuanced evaluation

The combination is powerful. Consider:

| Traditional App | AI-Enhanced App |
|-----------------|-----------------|
| Shows stock price dropped 5% | Explains whether this drop is noise or signal given market context |
| Displays customer feedback | Synthesizes sentiment patterns across thousands of reviews |
| Lists job candidates by score | Assesses fit considering factors no checkbox captures |
| Alerts when metric exceeds threshold | Alerts only when the deviation actually matters |

This isn't about replacing human judgment. It's about building tools that support human judgment with structured analysis that was previously impossible to automate.

The book teaches you to build these applications: software that measures precisely where precision matters, judges thoughtfully where judgment matters, and weaves both together with clean handoffs between the quantitative and qualitative domains.

---

## What You'll Learn

This book takes you from installation to mastery across three domains:

### The Tool Itself
- **[Installation and authentication](chapters/02-Installation-and-Authentication.md)** — Getting Claude Code running on your machine
- **[The permission model](chapters/04-The-Permission-Model.md)** — Understanding what Claude can and cannot do without asking
- **[CLAUDE.md files](chapters/05-CLAUDE-md-Your-Projects-Brain.md)** — Teaching Claude about your specific project
- **[Custom commands](chapters/06-Slash-Commands-and-Custom-Commands.md)** and **[skills](chapters/12-Skills-Automatic-Expertise.md)** — Extending Claude's capabilities
- **[MCP servers](chapters/09-MCP-Model-Context-Protocol.md)** — Connecting Claude to databases, APIs, and external tools
- **[Hooks](chapters/10-Hooks-Automation-Triggers.md)** — Automating actions before and after Claude's work
- **[Subagents](chapters/11-Subagents-Parallel-Intelligence.md)** — Running multiple Claude instances in parallel
- **[Plugins](chapters/36-Writing-Claude-Code-Plugins.md)** — Packaging and distributing your extensions
- **[The SDK](chapters/13-Headless-Mode-and-the-SDK.md)** — Building your own tools powered by Claude

### The Workflows
- **[The development loop](chapters/14-The-Development-Loop.md)** — The rhythm of effective vibe coding
- **[Test-driven development](chapters/16-Test-Driven-Development-with-Claude.md)** — Writing tests first, letting Claude implement
- **[Debugging](chapters/17-Debugging-and-Error-Recovery.md)** — Using Claude to find and fix problems
- **[Code review](chapters/18-Code-Review-Workflows.md)** — Systematic review workflows
- **[Git worktrees](chapters/15-Git-Worktrees-for-Parallel-Development.md)** — Parallel development across branches
- **[Large codebases](chapters/24-Large-Codebase-Navigation.md)** — Navigation strategies that scale
- **[CI/CD integration](chapters/26-CICD-Integration.md)** — Claude in your deployment pipeline

### The Bigger Picture
- **[Traditional APIs + AI APIs](chapters/31-Information-Plus-Understanding.md)** — Combining data sources with intelligent interpretation
- **[Qualitative meets quantitative](chapters/32-Weaving-AI-With-Classical-Computing.md)** — When to use classical computing vs. AI
- **[Program prompts](chapters/33-Program-Prompts.md)** — The insight that prompts can be complete programs
- **[Analytical frameworks](chapters/34-EDA-Analytical-Framework.md)** — Structured approaches to decision-making
- **[Decision support systems](chapters/35-Building-Decision-Support-Systems.md)** — Building tools that help people think better

---

## Who This Book Is For

This book serves two distinct audiences, and it's designed so each can read what's relevant to them:

### For Everyone (No Programming Required)

If you want to use Claude Code to automate tasks, process documents, analyze data, or build simple tools—you don't need to be a programmer. Parts I, II, and VIII of this book are written for you. They cover:

- Installing and using Claude Code
- Understanding how it works
- Building scripts and automation through natural language
- Using AI for analysis, decision support, and document processing
- The conceptual frameworks that make AI-assisted work effective

### For Programmers

Parts IV, V, and VI are explicitly technical. They assume familiarity with programming concepts like functions, tests, APIs, version control, and CI/CD pipelines. These chapters teach you how to:

- Integrate Claude into professional development workflows
- Write tests first and let Claude implement (TDD)
- Conduct systematic code reviews
- Build full-stack applications
- Navigate large codebases
- Set up automated pipelines

### The Chapter Guide

Each chapter in the Table of Contents below is marked:

- **(Everyone)** — No programming knowledge required
- **(Programmers)** — Assumes programming experience

Feel free to skip chapters that don't match your background. The book is designed for selective reading.

---

## The Philosophy

This book reflects a particular philosophy about AI-assisted development:

- **Structure enables freedom.** Good documentation, clear project organization, and explicit requirements make Claude more effective, not less. The best vibe coders aren't the ones who wing it—they're the ones who set up their projects so Claude can understand them deeply.

- **Iteration is the process.** Your first attempt won't be perfect. Neither will Claude's. The magic is in the loop: describe, test, refine. Each iteration teaches both you and the AI what you're actually building.

- **Tools should amplify thinking.** The goal isn't to outsource thinking to AI. It's to build tools that help humans think better—decision support systems, analytical frameworks, structured approaches that AI can apply consistently.

- **Prompts are programs.** For many qualitative tasks, a well-crafted prompt *is* the complete implementation. The prompt explains your framework; the AI applies it. Understanding this unlocks capabilities that have nothing to do with traditional code.

---

## How to Use This Book

### If You're Not a Programmer

Start with **Parts I and II** to get Claude Code installed and understand the basics. Then jump to **Part VII** (Working with Structure) and **Part VIII** (Analytical Frameworks) for the conceptual tools that make AI-assisted work effective.

Skip Parts IV, V, and VI—they're written for programmers and assume knowledge you don't need.

**Your reading path:** Chapters 1-9, 11-12, 27-35, 37-38

### If You Are a Programmer

Read everything. The book builds from basics to advanced patterns:

- **Parts I-II:** Foundation (fast if you're technical)
- **Part III:** Power features that multiply your effectiveness
- **Parts IV-VI:** Developer workflows, software building, advanced patterns
- **Part VII-VIII:** Structure and analytical frameworks (valuable for anyone)
- **Part IX:** Extending Claude Code with your own plugins

### Quick Paths

- **New to Claude Code?** Start with Parts I and II. Get it installed, run through your first sessions, understand the core features.

- **Want to build AI-powered applications?** Part V covers runtime AI integration—calling Claude's API from your own code.

- **Interested in analytical frameworks?** Part VIII explores decision support systems and the conceptual foundations of AI-assisted work.

- **Want hands-on practice?** The [Mini Labs (Chapter 30)](chapters/30-Mini-Labs-Hands-On-Practice.md) take 15-30 minutes each and teach specific skills through doing.

---

## Table of Contents

### Part I: Getting Started (Everyone)
1. [Why Claude Code Changes Everything](chapters/01-Why-Claude-Code-Changes-Everything.md) — (Everyone)
2. [Installation and Authentication](chapters/02-Installation-and-Authentication.md) — (Everyone)
3. [Your First Session](chapters/03-Your-First-Session.md) — (Everyone)
4. [The Permission Model](chapters/04-The-Permission-Model.md) — (Everyone)

### Part II: Core Features (Everyone)
5. [CLAUDE.md — Your Project's Brain](chapters/05-CLAUDE-md-Your-Projects-Brain.md) — (Everyone)
6. [Slash Commands and Custom Commands](chapters/06-Slash-Commands-and-Custom-Commands.md) — (Everyone)
7. [Settings and Configuration](chapters/07-Settings-and-Configuration.md) — (Everyone)
8. [The Model Selector](chapters/08-The-Model-Selector.md) — (Everyone)

### Part III: Power Features (Mixed)
9. [MCP — Model Context Protocol](chapters/09-MCP-Model-Context-Protocol.md) — (Everyone)
10. [Hooks — Automation Triggers](chapters/10-Hooks-Automation-Triggers.md) — (Programmers)
11. [Subagents — Parallel Intelligence](chapters/11-Subagents-Parallel-Intelligence.md) — (Everyone)
12. [Skills — Automatic Expertise](chapters/12-Skills-Automatic-Expertise.md) — (Everyone)
13. [Headless Mode and the SDK](chapters/13-Headless-Mode-and-the-SDK.md) — (Programmers)

### Part IV: Developer Workflows (Programmers)
14. [The Development Loop](chapters/14-The-Development-Loop.md) — (Programmers)
15. [Git Worktrees for Parallel Development](chapters/15-Git-Worktrees-for-Parallel-Development.md) — (Programmers)
16. [Test-Driven Development with Claude](chapters/16-Test-Driven-Development-with-Claude.md) — (Programmers)
17. [Debugging and Error Recovery](chapters/17-Debugging-and-Error-Recovery.md) — (Programmers)
18. [Code Review Workflows](chapters/18-Code-Review-Workflows.md) — (Programmers)
37. [Divide and Conquer](chapters/37-Divide-and-Conquer.md) — (Everyone)
38. [Requirements Through Conversation](chapters/38-Requirements-Through-Conversation.md) — (Everyone)

### Part V: Building Software (Programmers)
19. [Scripts and Automation](chapters/19-Scripts-and-Automation.md) — (Programmers)
20. [Full-Stack Applications](chapters/20-Full-Stack-Applications.md) — (Programmers)
21. [API Integration Patterns](chapters/21-API-Integration-Patterns.md) — (Programmers)
22. [AI-Powered Features at Runtime](chapters/22-AI-Powered-Features-at-Runtime.md) — (Programmers)

### Part VI: Advanced Development Patterns (Programmers)
23. [Multi-Agent Orchestration](chapters/23-Multi-Agent-Orchestration.md) — (Programmers)
24. [Large Codebase Navigation](chapters/24-Large-Codebase-Navigation.md) — (Programmers)
25. [Migration and Refactoring at Scale](chapters/25-Migration-and-Refactoring-at-Scale.md) — (Programmers)
26. [CI/CD Integration](chapters/26-CICD-Integration.md) — (Programmers)

### Part VII: Working with Structure (Everyone)
27. [When You Need Documentation](chapters/27-When-You-Need-Documentation.md) — (Everyone)
28. [The Document Stack](chapters/28-The-Document-Stack.md) — (Everyone)
29. [A Complete Structured Build](chapters/29-A-Complete-Structured-Build.md) — (Everyone)

### Part VIII: Analytical Frameworks and Decision Support (Everyone)
30. [Mini Labs — Hands-On Practice](chapters/30-Mini-Labs-Hands-On-Practice.md) — (Mixed)
31. [Information Plus Understanding](chapters/31-Information-Plus-Understanding.md) — (Everyone)
32. [Weaving AI with Classical Computing](chapters/32-Weaving-AI-With-Classical-Computing.md) — (Everyone)
33. [Program Prompts](chapters/33-Program-Prompts.md) — (Everyone)
34. [The EDA Analytical Framework](chapters/34-EDA-Analytical-Framework.md) — (Everyone)
35. [Building Decision Support Systems](chapters/35-Building-Decision-Support-Systems.md) — (Everyone)

### Part IX: Extending Claude Code (Programmers)
36. [Writing Claude Code Plugins](chapters/36-Writing-Claude-Code-Plugins.md) — (Programmers)

### Appendices
- [A. Command Reference](chapters/Appendix-A-Command-Reference.md) — (Everyone)
- [B. CLAUDE.md Templates](chapters/Appendix-B-CLAUDE-md-Templates.md) — (Everyone)
- [C. MCP Server Catalog](chapters/Appendix-C-MCP-Server-Catalog.md) — (Everyone)
- [D. Hook Recipes](chapters/Appendix-D-Hook-Recipes.md) — (Programmers)
- [E. Troubleshooting](chapters/Appendix-E-Troubleshooting.md) — (Everyone)

---

## About This Book

Whether you're automating tedious tasks, building full-stack applications, or orchestrating multi-agent workflows, this book gives you the patterns that work.

*Current as of December 2025*

---

## License

This work is provided for educational purposes. See individual chapters for specific code examples and their usage.
