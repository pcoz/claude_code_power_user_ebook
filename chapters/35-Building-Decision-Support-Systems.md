# Chapter 35: Building Decision Support Systems

This is where everything converges. Structured planning, AI integration, analytical frameworks—all combining into software that helps people make better decisions.

---

## What We're Building

A decision support system that:
- Accepts a decision context (what you're deciding, what options you see)
- Analyzes each option across dimensions (energy, logic, control)
- Compares options and surfaces trade-offs
- Identifies when synthesis might produce better options
- Helps generate and evaluate new options
- Produces documentation supporting the decision

This is more than analysis. It's a system that encodes structured thinking and applies it intelligently.

---

## The Core Insight: Best as Balance

"Best" requires two things:
1. No catastrophic weakness on any dimension
2. Strongest overall balance among options that pass that test

An option that's brilliant on logic but destroys your control position isn't viable. Eliminate those first. Then among remaining options, select the strongest balance given what matters most.

But notice: you can't determine "best" until you know what matters. "Best" is meaningless without weighting dimensions.

---

## The Decision Pipeline

**Step 1: Describe Context**
What are you deciding? What matters most here? What constraints exist?

**Step 2: List Options**
What alternatives are you considering? Name and describe each.

**Step 3: Analyze Each Option**
For each option, assess:
- Energy outcome: How does it affect goal-pursuit dynamics?
- Logic outcome: How does it affect the rational terrain?
- Control outcome: How does it affect constraining frameworks?

Rate each: Positive, Mixed, Negative, Uncertain, Risky

**Step 4: Compare Across Options**
Look at the pattern. Which options stand out? What trade-offs exist? Is there synthesis potential—different options strong on different dimensions?

**Step 5: Synthesize (If Needed)**
When all options are flawed, can you combine strengths? Take the logic strength from Option A, the control strength from Option B, and create Option C.

**Step 6: Document**
Record the analysis, the comparison, the reasoning, the recommendation.

---

## Building with Claude Code

Start with the executive brief:

```
I want to build a decision support system.

Users should be able to:
1. Describe a decision they face (context, what matters most)
2. Enter the options they're considering
3. See each option analyzed across Energy, Logic, Control dimensions
4. View a comparison matrix showing all options
5. Explore synthesis when stuck between flawed options
6. Export a summary document

Build as a web application with:
- Flask backend with Claude API integration
- Simple HTML/JavaScript frontend
- Three AI functions: analyze, compare, synthesize

Start with an executive brief for this project.
```

---

## The Three AI Functions

### Function 1: Analyze

Evaluates a single option against the framework:

```python
ANALYZE_PROMPT = """
Analyze this option using the following framework:

ENERGY: How does this option affect goal-pursuit dynamics?
Does it strengthen our drive? Weaken theirs? What's the
energy outcome?

LOGIC: How does this option affect the rational terrain?
Does it strengthen our arguments? What reasoning implications?

CONTROL: How does this option affect constraints?
Does it invoke rules that favor us? Shift procedural posture?

For each dimension, provide:
- Assessment (1-2 sentences)
- Rating: Positive / Mixed / Negative / Uncertain / Risky

Decision Context: {context}
What matters most: {priorities}
Option to analyze: {option_name} - {option_description}
"""
```

### Function 2: Compare

Looks across all analyzed options:

```python
COMPARE_PROMPT = """
You have analyzed these options for a decision:

{options_with_analyses}

Compare them and identify:

1. STANDOUTS: Any options that are clearly strong or weak overall?

2. TRADE-OFFS: What are the key trade-offs between options?
   (e.g., "Option A is strong on logic but weak on control")

3. SYNTHESIS POTENTIAL: Are different options strong on different
   dimensions? If so, combining strengths might be possible.

4. RECOMMENDATION: Based on the stated priorities ({priorities}),
   which option(s) seem strongest and why?
"""
```

### Function 3: Synthesize

Proposes new options by combining strengths:

```python
SYNTHESIZE_PROMPT = """
The user wants to explore combining strengths from these options:

{options_with_strengths}

For a successful synthesis:
1. Identify the MECHANISM that creates each strength
2. Assess whether mechanisms are SEPARABLE from weaknesses
3. Check if mechanisms can COEXIST in one approach
4. If viable, describe the SYNTHETIC OPTION concretely

Be specific. If synthesis won't work, explain why (same underlying
problem, mechanisms conflict, etc.)
"""
```

---

## The User Experience

**Screen 1: Context**
```
Decision Title: [____________]
Background: [____________]
What matters most: [____________]
```

**Screen 2: Options**
Add options one by one:
```
Option Name: [____________]
Description: [____________]
[+ Add Another Option]
```

**Screen 3: Analysis**
For each option, show:
```
OPTION A: Aggressive Response

Energy: Mixed
- Shows commitment but may escalate other party's resistance

Logic: Positive
- Surfaces our arguments clearly, creates record

Control: Negative
- Invites escalation, may lose procedural advantage

[Analyze Next Option]
```

**Screen 4: Comparison Matrix**

| Option | Energy | Logic | Control |
|--------|--------|-------|---------|
| A | 🟡 Mixed | 🟢 Positive | 🔴 Negative |
| B | 🔴 Negative | 🟡 Mixed | 🟢 Positive |
| C | 🟢 Positive | 🟢 Positive | 🟡 Mixed |

Below: AI's comparison analysis and recommendations.

**Screen 5: Synthesis (Optional)**
```
Explore combining:
☑ Logic strength from Option A
☑ Control strength from Option B

[Generate Synthesis]

PROPOSED OPTION D:
Substantive letter with resolution path.
Present arguments clearly (A's strength) while
proposing structured negotiation (B's strength).
```

**Screen 6: Export**
Generate a document with complete analysis, suitable for sharing or records.

---

## Building It

```
Let's build this step by step.

First, create the Flask backend with:
- POST /api/context - save decision context
- POST /api/options - add an option
- POST /api/analyze - analyze one option (calls Claude)
- POST /api/compare - compare all options (calls Claude)
- POST /api/synthesize - generate synthesis (calls Claude)
- GET /api/export - generate summary document

Use SQLite for persistence.
Read ANTHROPIC_API_KEY from environment.
```

Then:

```
Now create the frontend. Build as a multi-step wizard:

Step 1: Context form
Step 2: Options list with add/edit/delete
Step 3: Analysis cards (one per option, lazy-loaded)
Step 4: Comparison matrix with AI analysis
Step 5: Synthesis explorer
Step 6: Export button

Use vanilla JavaScript. Keep it simple.
```

---

## Testing with a Real Decision

Before calling it done, test with something real.

Enter a decision you're actually facing. List your real options. Run the analysis.

- Does the dimensional breakdown clarify anything you hadn't seen?
- Does the comparison reveal patterns?
- Does synthesis suggest options you hadn't considered?

The real test isn't whether it produces output—it's whether the output helps you think better.

---

## What You've Built

- **Structured process encoded in software**: The framework guides users through analysis step by step

- **AI-powered intelligence at three levels**: Not just displaying data—analyzing, comparing, generating

- **Visual clarity**: Trade-offs become obvious in the matrix

- **Generative capability**: Proposes new options when users are stuck

- **Exportable artifacts**: Documentation supporting decisions

---

## The Larger Lesson

This demonstrates something beyond building a specific tool.

The EDA framework is a way of thinking about decisions. Traditionally, such frameworks live in books and training programs. They require people to learn the methodology, remember it, apply it correctly.

By encoding the framework in software—with AI handling the analytical work—the methodology becomes accessible to anyone who can fill out a form.

The system doesn't replace human judgment. It structures and supports it. It catches things humans forget. It surfaces patterns humans miss. It generates options humans didn't think of.

This is what vibe coding plus AI enables: not just automation of tasks, but amplification of structured thought.

---

## What Else Could You Build?

Any structured methodology can become a decision support system:

- **Strategic planning**: SWOT analysis, competitive positioning
- **Risk assessment**: Identify, evaluate, prioritize risks
- **Project evaluation**: Score proposals against criteria
- **Hiring decisions**: Evaluate candidates systematically
- **Investment analysis**: Compare opportunities across factors

The pattern is the same:
1. Define the framework
2. Build AI prompts that apply it
3. Create interface for input and visualization
4. Let Claude Code assemble the pieces

What methodologies do you use that others could benefit from?

Those are things you can build now.

> **See Also:**
> - [EDA Analytical Framework](34-EDA-Analytical-Framework.md) for the framework used in this example
> - [Full-Stack Applications](20-Full-Stack-Applications.md) for building complete web applications
> - [A Complete Structured Build](29-A-Complete-Structured-Build.md) for structured development process

---

**Next:** [Chapter 36: Writing Claude Code Plugins](36-Writing-Claude-Code-Plugins.md) — Extend Claude Code with custom commands, skills, and more.
