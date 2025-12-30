# Chapter 38: Requirements Through Conversation

You have an idea. It's clear in your head—sort of. You know what you want—mostly. The details are fuzzy, but you'll figure them out as you go.

This is where most projects go wrong.

Vague requirements produce vague software. Claude will build something, but it won't be what you actually needed. You'll discover the gaps through painful iteration.

There's a better way: get Claude to interrogate you first.

---

## The Problem with "Just Build It"

When you say "build me an expense tracker," Claude will build an expense tracker. But whose expense tracker?

- Personal or business?
- Single user or multi-user?
- What categories?
- What reports?
- What integrations?
- Mobile or desktop or both?
- What happens to the data?

Claude will make assumptions for every unanswered question. Some assumptions will be wrong. You'll discover this after the code is written.

---

## Flip the Dynamic

Instead of describing your idea and hoping Claude fills in the gaps correctly, ask Claude to interview you:

```
I have an idea for a project. Before we build anything, I want you to
interview me to understand exactly what I need. Ask me questions one
at a time to clarify:

- What problem I'm solving
- Who will use it
- What it must do vs. nice-to-have
- How it should work
- What constraints exist
- What success looks like

Don't write any code yet. Just ask questions until you fully understand
what to build.

My idea: I want to build an expense tracker.
```

Now Claude asks questions. You answer. Claude asks follow-ups. The requirements emerge through conversation.

---

## Why This Works

**You don't know what you don't know.** Claude's questions surface considerations you hadn't thought of. "How should it handle receipts?" Oh right, receipts. "What happens if you're offline?" Hadn't considered that.

**Talking is easier than specifying.** Writing a requirements document is hard. Answering questions is easy. The conversation extracts the same information with less effort.

**Assumptions become explicit.** When Claude asks "should categories be fixed or user-defined?" you make a conscious choice. No hidden assumptions.

**The requirements document writes itself.** At the end, ask Claude to summarize what it learned. You now have documentation.

---

## The Interview Framework

A good requirements interview covers:

### 1. The Problem
- What pain are you solving?
- What happens if this doesn't get built?
- How are you handling this today?

### 2. The Users
- Who will use this?
- How technical are they?
- How often will they use it?
- What devices/contexts?

### 3. Core Functionality
- What must it absolutely do?
- What would be nice but isn't essential?
- What should it explicitly NOT do?

### 4. Data and State
- What information needs to be stored?
- How long should it persist?
- Who can see what?
- What happens to old data?

### 5. Integration
- What does it need to connect to?
- What format should data be in?
- Are there existing systems to work with?

### 6. Constraints
- Timeline?
- Budget?
- Technical limitations?
- Regulatory requirements?

### 7. Success Criteria
- How will you know it's working?
- What does "done" look like?
- What metrics matter?

---

## Worked Example: The Expense Tracker

Here's an actual interview:

**You:**
```
I have an idea for a project. Interview me to understand what I need
before we build anything.

My idea: I want to build an expense tracker.
```

**Claude:**
```
I'd be happy to help you think through this expense tracker. Let me
start with the basics.

What problem is this solving for you? Are you tracking expenses for
personal budgeting, business expense reports, tax purposes, or
something else?
```

**You:**
```
It's for my small consulting business. I need to track what I spend
on client projects so I can bill them back or write it off.
```

**Claude:**
```
Got it—business expense tracking with client attribution. A few
follow-up questions:

1. How many clients do you typically have active at once?
2. Are you the only one who will use this, or do you have employees
   or a bookkeeper who needs access?
```

**You:**
```
Usually 3-5 active clients. Just me using it, but my accountant needs
to see the data at tax time.
```

**Claude:**
```
Single user with export capability for your accountant. Now about the
expenses themselves:

What types of expenses do you track? For example:
- Travel (flights, hotels, meals)
- Software subscriptions
- Hardware purchases
- Subcontractors
- Something else?

And do you need to attach receipts or just log the amounts?
```

**You:**
```
Mostly travel, meals with clients, software subscriptions, and
sometimes equipment. Yes, I need to keep receipt photos—my accountant
requires them.
```

**Claude:**
```
Receipt photos are required. How do you typically capture these today?
Phone camera? Scanner? Do you need the app to work on mobile for
capturing receipts on the go?
```

**You:**
```
Phone camera usually. Yeah, mobile would be essential for logging
expenses when I'm traveling.
```

**Claude:**
```
Mobile-first with camera integration. Let me understand the workflow:

When you incur an expense, what information do you need to capture?
- Amount
- Date
- Category
- Client/project
- Receipt photo
- Anything else? Notes? Payment method?
```

**You:**
```
All of those. Also whether it's billable to the client or just a
business write-off.
```

**Claude:**
```
Important distinction—billable vs. write-off.

For reporting, what do you need to see?
- Total by client?
- Total by category?
- Monthly summaries?
- Something specific for your accountant's format?
```

**You:**
```
Total by client is the main one—I need to know what to bill. Category
breakdown would help for tax categories. My accountant just needs a
CSV export with all the details.
```

**Claude:**
```
CSV export for accountant, reports by client and category.

A few more questions:

1. Do you need to generate actual invoices from this, or do you handle
   billing separately?

2. For the CSV export, does your accountant need a specific format or
   just all fields in some reasonable structure?

3. What's your timeline—is this urgent, or a "when it's ready" project?
```

**You:**
```
I bill separately through my invoicing system—this is just for tracking.
Any reasonable CSV format is fine. Not urgent, but I'd like to start
using it next month if possible.
```

**Claude:**
```
Last few questions:

1. Where should the data live? Local on your phone? Cloud synced? Do
   you need to access it from multiple devices?

2. Any budget constraints on this project?

3. Are there any existing tools you've tried that didn't work? What
   was missing?
```

**You:**
```
Cloud synced—I want to access from phone and laptop. No real budget
constraint, it's for my own use. I tried a few apps but they were too
complicated or didn't let me assign to specific clients easily.
```

**Claude:**
```
That's really helpful. Let me summarize what I understand:

## Expense Tracker Requirements

**Purpose:** Track business expenses for client billing and tax write-offs

**User:** Single user (you), with export for accountant

**Core Features:**
- Log expenses with: amount, date, category, client, billable flag, notes, payment method
- Capture receipt photos via phone camera
- Assign expenses to specific clients/projects
- Mark as billable or write-off

**Platforms:**
- Mobile app (primary) for on-the-go capture
- Web access from laptop
- Cloud synced between devices

**Reporting:**
- Total expenses by client (for billing)
- Breakdown by category (for taxes)
- CSV export with all fields

**Not Included:**
- Invoice generation (handled separately)
- Multi-user access
- Complex approval workflows

**Timeline:** Ready to use next month

**Key Differentiator:** Simple client assignment—this is what existing tools got wrong

Does this capture it correctly? Anything to add or change?
```

**You:**
```
That's exactly right. One addition: I'd like to be able to see
unbilled expenses per client so I don't forget to invoice them.
```

**Claude:**
```
Added: Dashboard showing unbilled expenses per client.

Now I have a clear picture. Should I proceed with building this?
```

---

## What Just Happened

In about 10 minutes of conversation:

1. A vague "expense tracker" became a specific application
2. Hidden requirements surfaced (receipt photos, billable flag, client assignment)
3. Explicit decisions were made (mobile-first, cloud sync, no invoicing)
4. Scope was bounded (single user, no approval workflows)
5. Success criteria emerged (simple client assignment)
6. A requirements summary was created

All through answering questions. No requirements document to write from scratch.

---

## Prompts That Trigger Good Interviews

**For new ideas:**
```
I have an idea but it's not fully formed. Interview me to understand
what I actually need. Ask probing questions about the problem, users,
features, and constraints. Don't suggest solutions yet—just understand
the problem space.
```

**For unclear requirements:**
```
I need to build [thing] but I'm not sure about the details. Act as a
business analyst and ask me clarifying questions. Challenge my
assumptions. Help me think through edge cases.
```

**For scope definition:**
```
Help me define the MVP for this project. Ask questions to understand
what's essential vs. nice-to-have. Push back if I'm including too much
for a first version.
```

**For technical decisions:**
```
I need to make some technical choices for this project. Interview me
about my constraints, preferences, and requirements so you can
recommend appropriate technologies.
```

---

## When to Use This

**Before any significant project.** Spend 10-15 minutes on requirements. Save hours of rework.

**When you're stuck.** If you don't know where to start, the interview helps you figure out what you're actually building.

**When stakeholders disagree.** Have Claude interview each person separately, then compare the summaries.

**When inheriting a project.** "Interview me about what this system does. I'll answer based on what I know."

---

## The Summary Step

Always end with:

```
Summarize everything you've learned about this project in a
structured format I can reference later.
```

This gives you:
- A requirements document you didn't have to write
- A reference for future conversations
- Something to share with others
- A baseline to compare the finished product against

---

## Summary

Don't start with "build me a thing." Start with "help me figure out what thing I need."

Claude is excellent at asking the right questions—questions you wouldn't think to answer unprompted. Use that capability.

The interview approach:
1. State your rough idea
2. Ask Claude to interview you
3. Answer honestly, including "I don't know"
4. Let requirements emerge through conversation
5. Get a summary at the end
6. Then build

The 15 minutes spent on requirements saves hours of building the wrong thing.
