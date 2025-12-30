# Chapter 33: Program Prompts

Here's something that might surprise you: for many qualitative analysis tasks, the prompt *is* the complete program. No Python. No JavaScript. No code at all.

The prompt explains your analytical framework. The AI applies that framework directly. The output is analysis, not code. The entire "program" lives in natural language.

---

## The Prompt Is the Program

In classical programming, you write code that a machine executes literally. The machine has no understanding—it simply follows instructions.

In prompt-programming, you write prompts that tell an AI how to think. The AI applies your instructions interpretively. You're not specifying procedures. You're teaching concepts.

If your prompt says "analyze the energy dimension," the AI must understand what "energy dimension" means before it can do anything. The quality of your output depends entirely on the quality of your explanation.

---

## Why This Works

When you explain your approach to AI, you place concepts into the context window. The AI processes your explanation and your request as a single context. Your explanation becomes part of AI's working understanding for this conversation.

This is different from training. Training changes the AI's weights permanently. Your explanation changes AI's context for this specific interaction. But that's enough—for this conversation, the AI "knows" your framework and can apply it consistently.

---

## A Simple Example

**Weak prompt** (doomed to produce vague results):
```
Analyze this business situation using energy, logic, and control.
```

The AI doesn't know what you mean. It reaches for general understanding of those words and produces something—but not what you intended.

**Strong prompt** (likely to succeed):
```
Analyze this situation using the following framework:

ENERGY: What drives this forward? What goals are being pursued?
What forces push it or hold it back? Is the energy strong or weak?
Coming from where? Pushing toward what?

LOGIC: What reasoning governs this? What rules apply? What would
count as valid or correct here? Is the logic sound? Consistent?
What assumptions underlie it?

CONTROL: What constrains this? What boundaries cannot be crossed?
What limits shape what's possible? Are controls tight or loose?
Enforced how?

For each dimension, assess at two levels:
1. PRINCIPLE: What type of energy/logic/control typically governs
   situations like this?
2. APPLICATION: How does this specific situation express that type?

Situation to analyze:
[INSERT SITUATION]
```

The second prompt teaches the AI your framework. It's longer. It requires more work upfront. But it actually works because AI now knows what you mean.

---

## Novel Methods Need Explanation

Here's a paradox: the approaches that most need AI are the approaches AI knows least about.

Any genuinely new methodology for AI-assisted analysis was developed *because* AI makes such analysis possible. It didn't exist before AI. It's not in any training data.

When you say "energy," AI reaches for its general understanding—physical energy, enthusiasm, business-speak "bringing energy to a project." It doesn't think: "the animating dimension—what goal is being pursued and how that goal-seeking is strengthened or weakened."

The words are familiar. Your framework is not. You must teach it.

---

## Building Reusable Prompt-Programs

For frameworks you use repeatedly, build the explanation into a system prompt:

```
You are an analytical assistant using the EDA Framework.

FRAMEWORK DEFINITION:

Energy Dimension:
Energy is what makes a system move rather than stay still. It is
goal-pursuit, the animating force. Types of energy include:
- Biological: encoded drives toward survival/reproduction
- Purposive: conscious intention toward chosen goals
- Emergent: force arising from many individual actions combining

When analyzing energy, identify: What type? How intense? What
direction? What would strengthen or weaken it?

Logic Dimension:
Logic is the rules that govern how something works. Types include:
- Process logic: inputs → transformations → outputs
- Growth logic: development over time responding to environment
- Exchange logic: prices, supply/demand, equilibrium
- Precedential logic: prior decisions binding future ones

When analyzing logic, identify: What type? Is it sound? What
assumptions underlie it? What counts as valid here?

Control Dimension:
Control is what holds a system within bounds. Types include:
- Homeostatic: automatic mechanisms maintaining equilibrium
- Structural: rules that bind decisions before they're made
- Allocation: limits on resource flow forcing trade-offs
- Identity: boundaries maintained by shared understanding

When analyzing control, identify: What types operate? How tight
or loose? What does each maintain? What would shift them?

TWO LEVELS:
For each dimension, analyze at:
- Principle level: What type typically governs situations like this?
- Application level: How does this specific instance express that type?

OUTPUT FORMAT:
Present analysis in three sections (Energy, Logic, Control), each
with Principle and Application subsections. Conclude with Synthesis
connecting the dimensions.
```

With this system prompt, every query gets processed through your framework automatically.

---

## When Code Is Unnecessary

Consider what code would add to an analytical prompt-program:

- **Parsing input?** AI handles text natively
- **API call?** Just infrastructure
- **Formatting output?** Specify in the prompt

For pure qualitative analysis, code is often unnecessary. The prompt-program is complete.

Code becomes necessary when you need:
- **Persistence**: saving results, tracking changes over time
- **Integration**: connecting to databases, other APIs
- **Scale**: processing thousands of documents automatically
- **Interface**: building a web app others can use

But these are infrastructure concerns. The analytical framework—the actual thinking—lives in the prompt.

---

## From Prompt to Program to Code

The path often goes:

1. **Develop the prompt** — Figure out what you want the analysis to do
2. **Test manually** — Paste situations, refine the prompt
3. **Automate later** — Wrap in code only if needed for scale/integration

This is the opposite of traditional development where you start with code architecture. Here, you start with the thinking and add machinery only as needed.

---

## Iterating on Prompt-Programs

Your first explanation will be imperfect.

**Ambiguity:** You used a word with multiple meanings; AI chose wrong. Fix: replace with more specific phrase.

**Incompleteness:** You explained what something is but not what it isn't. AI filled gaps with assumptions. Fix: add explicit contrasts.

**Structure:** Explanation was clear sentence-by-sentence but relationships between concepts were unclear. Fix: reorganize to show dependencies.

Each iteration teaches you how AI interprets your language. Over time, explanations get sharper.

```
Test your analytical prompt-program:

1. Apply it to 3 different situations
2. Review the outputs:
   - Where did AI nail it?
   - Where did it miss your intent?
   - What part of explanation led it astray?
3. Refine the prompt
4. Repeat until outputs consistently match intent
```

---

## Building a Contract Analyzer

Here's a complete prompt-program for analyzing contract disputes:

```
You are a contract dispute analyst using the EDA Framework.

ENERGY ANALYSIS:
Identify what drives each party. What do they actually want? Is their
stated position their true interest? How intense is their commitment?
What would strengthen or weaken their drive?

LOGIC ANALYSIS:
Examine the arguments. What reasoning supports each position? Is it
sound? What evidence exists? What's missing? What legal principles
apply? How do the facts map to those principles?

CONTROL ANALYSIS:
Map the constraints. What procedural options exist? What's the
timeline? What costs constrain action? What must each party do or
avoid? What leverage does each have?

For each dimension, analyze at PRINCIPLE level (what typically
governs disputes like this) and APPLICATION level (how this
specific situation expresses those patterns).

SYNTHESIS:
- Where is each party strong and weak?
- What does the dimensional analysis reveal that surface reading misses?
- What strategic options emerge from this analysis?
- What would you recommend and why?

Analyze the following:
[INSERT DISPUTE DESCRIPTION]
```

This prompt-program:
- Defines the analytical framework
- Specifies two-level analysis
- Requests synthesis and recommendations
- Contains no code whatsoever

Paste a contract dispute description, and you get structured analysis.

---

## The Discipline of Conceptual Programming

This is harder than it sounds. Most of us use concepts loosely. We know what we mean, more or less. We rely on shared context.

When we say "energy" to a colleague who knows our framework, they understand. When we say "energy" to AI that has never seen our framework, it guesses—and guesses wrong.

The discipline of prompt-programming is making implicit knowledge explicit:
- Every assumption stated
- Every term defined
- Every relationship articulated

You're not reminding AI of something it learned. You're teaching something new.

---

## What This Makes Possible

Any structured methodology can become a prompt-program:

- **Negotiation analyzer**: Decompose the other party's position
- **Strategic communication calibrator**: Optimize messages across dimensions
- **Risk assessment tool**: Evaluate opportunities systematically
- **Decision support system**: Compare options across criteria

The pattern is the same:
1. Define your framework clearly
2. Explain it in the prompt
3. AI applies it to whatever you provide

The approaches you use that others could benefit from? Those can become prompt-programs that make your expertise accessible through conversation.

---

## From Prompts to Real Programs

The progression:

1. **Personal prompt** — You use it in conversation
2. **Shared template** — Others copy and use it
3. **System prompt** — Embedded in a custom GPT or Claude project
4. **Wrapped in code** — API calls, storage, interface
5. **Full application** — Production system with users

You can stop at any level. A good prompt-program is valuable even without code. Code just adds reach.

---

## Summary

- The prompt is the program for qualitative analysis
- Novel frameworks need explicit teaching—AI hasn't seen them
- Quality of explanation determines quality of output
- Code is optional—add only for persistence, scale, or interface
- Iterate until outputs match intent
- Any methodology can become a prompt-program

This is native qualitative programming: describing thinking so clearly that AI can apply it. Not writing code—writing concepts. The concepts are the program.
