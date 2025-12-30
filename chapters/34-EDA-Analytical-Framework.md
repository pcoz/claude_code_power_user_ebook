# Chapter 34: The EDA Analytical Framework

Any situation you encounter can be decomposed into three dimensions: Energy, Logic, and Control. This isn't an arbitrary framework—it reflects something fundamental about how systems exist.

Understanding these dimensions transforms how you analyze problems, design solutions, and make decisions.

---

## Why Three Dimensions?

Consider what it takes for anything to exist as a system—a bounded, persistent, active thing in the world.

**It must be something.** There must be a pattern, a structure, a "what it is" that distinguishes it from everything else. This is Logic.

**It must happen.** A pattern that sits still is just a blueprint. Something must animate it, push it into actuality, make it unfold through time. This is Energy.

**It must remain itself.** A system that expands without limit dissipates into everything and nothing. Something must hold it within bounds, maintain identity, keep it coherent over time. This is Control.

These are conditions of existence. Anything that exists as a system must have all three.

---

## Energy: What Makes It Move

Energy is the animating force—what makes a system move rather than stay still.

**Types of energy:**

- **Biological**: Encoded drives toward survival and reproduction. A tree reaching for light doesn't choose to grow; the energy is built into what it is. You cannot negotiate with biological energy.

- **Purposive**: Conscious intention toward chosen goals. A CEO pursuing market share. This energy involves strategy, decision, allocation of attention. It can be persuaded, incentivized, redirected by argument.

- **Emergent**: Force arising from many individual energies combining. A crowd becoming a movement. No one person creates a movement's momentum. It emerges from interaction, feeds on itself.

**When analyzing energy, ask:**
- What type of force animates this?
- How intense is it?
- What direction is it pushing?
- What would strengthen or weaken it?

---

## Logic: What It Is

Logic means the rules that govern how something works—and there are fundamentally different types.

**Types of logic:**

- **Process logic**: Inputs → transformations → outputs. A factory. What counts as "working" is whether inputs reliably become outputs.

- **Growth logic**: Encoded instructions responding to environment, developing over time. A tree, an organization. What counts as "working" is whether it thrives and develops.

- **Exchange logic**: Prices adjust based on supply/demand, resources flow toward returns. A market. What counts as "working" is whether equilibrium emerges.

- **Precedential logic**: Prior decisions bind future ones, principles extend by analogy. Legal reasoning. What counts as "working" is whether reasoning follows from authoritative sources.

**When analyzing logic, ask:**
- What type of logic governs this system?
- What specific version of that logic applies here?
- What counts as valid, correct, or well-formed?
- Is the reasoning sound?

---

## Control: What Holds It Together

Control means self-regulation—what keeps a system within bounds.

**Types of control:**

- **Homeostatic**: Automatic mechanisms maintaining equilibrium. Body temperature regulation. Operates below awareness, continuous, self-correcting.

- **Structural**: Rules that bind decisions before they're made. A constitution constraining a government. Works through pre-commitment.

- **Allocation**: Limits on resource flow forcing trade-offs. A budget. Shapes decisions by making costs visible and choices necessary.

- **Identity**: Boundaries maintained by shared understanding. "That's not who we are." Works through recognition of what belongs and what doesn't.

**When analyzing control, ask:**
- What types of self-regulation operate here?
- What role does each play? What does it maintain?
- Where is control tight and where loose?
- What could shift the control boundaries?

---

## Two Levels: Principle and Application

Each dimension operates at two levels:

**Principle level**: What type of thing is this? What kind of energy, logic, or control typically governs situations like this?

**Application level**: How does this particular instance express the type? What specific circumstances shape this case?

Both levels are necessary:
- Principle gives you orientation—what dynamics typically govern this type of situation
- Application gives you location—where you stand within that map

---

## The Framework in Action

**Example: A Job Interview**

*Energy:*
- Candidate: Driven by goal of getting the job (or advancing career)
- Interviewer: Driven by goal of finding the right person
- The energies overlap but aren't identical—creates tension

*Logic:*
- The logic of interviews: demonstrate capability and fit
- Candidate presents evidence; interviewer evaluates against criteria
- Reasoning patterns: "They handled similar situations, so they can handle this"

*Control:*
- Time constraint (45 minutes)
- Information constraint (limited to what emerges in conversation)
- Legal constraints (questions you cannot ask)
- Policy constraints (salary bands, required qualifications)

**Example: A Contract Dispute**

*Energy:*
- What does each party actually want? (Often not what they claim)
- How committed are they? Desperate or opportunistic?
- What would strengthen or weaken their drive?

*Logic:*
- What reasoning supports each position?
- What evidence exists? What's missing?
- What legal principles apply?

*Control:*
- Procedural options (litigation, mediation, negotiation)
- Timeline constraints
- Cost constraints
- Leverage each party holds

---

## Using EDA with Claude

You can use this framework in conversation:

```
Analyze this situation using the EDA framework:

ENERGY: What drives each party? What do they actually want?
How intense is their commitment? What would change their drive?

LOGIC: What reasoning governs this? What counts as valid here?
Is the logic sound? What evidence supports or undermines it?

CONTROL: What constrains what can happen? What boundaries exist?
What leverage does each party have?

For each dimension, examine:
- PRINCIPLE: What typically governs situations like this?
- APPLICATION: How does this specific situation express those patterns?

Situation:
[DESCRIBE YOUR SITUATION]
```

Claude applies your framework to whatever you provide.

---

## The Thought Exercise

Take any situation you're dealing with:
- A decision you're wrestling with
- A negotiation you're part of
- A conflict you've observed
- An organization you belong to

Ask the three questions:

1. **What is the energy?** What drives this forward? What goals are pursued, by whom?

2. **What is the logic?** What reasoning governs this? What counts as valid here?

3. **What is the control?** What constrains what can happen? Which limits are firm?

Then go deeper: How is each element strengthened or weakened by circumstances?

---

## Which Dimension Dominates?

Different situations have different centers of gravity:

**Energy-dominant**: A startup in hypergrowth. A movement gaining momentum. A person in the grip of ambition. The animating force is what you need to understand.

**Logic-dominant**: A technical architecture decision. A scientific debate. A legal argument. The outcome turns on whose reasoning is sound.

**Control-dominant**: A heavily regulated industry. A resource-constrained project. A relationship with rigid boundaries. What matters most is navigating constraints.

Recognizing which dimension dominates clarifies where to focus.

---

## Analysis → Decision

The framework doesn't just help you understand—it helps you act.

Once you've decomposed a situation:
- You can see which dimensions are strong and weak
- You can identify where intervention would have most impact
- You can recognize trade-offs between dimensions
- You can design actions that address root causes, not symptoms

A contract dispute where you're weak on logic but strong on control? Focus on procedural moves, not arguments.

A negotiation where the other side has high energy but poor control? They may be willing to accept outcomes that preserve their drive toward their goal, even if those outcomes aren't their stated position.

---

## Building Analytical Tools

This framework can power automated analysis:

```
Build a dispute analyzer that:

1. Takes a situation description as input
2. Sends to Claude with EDA framework prompt
3. Returns structured analysis:
   - Energy assessment (type, intensity, direction)
   - Logic assessment (type, soundness, evidence)
   - Control assessment (constraints, leverage, boundaries)
4. Concludes with strategic recommendations

Format output as sections with principle and application
levels for each dimension.
```

The framework becomes a reusable tool for any situation.

---

## Summary

**Energy, Logic, Control**—three dimensions that any system must have.

- Energy: What animates it
- Logic: What it is
- Control: What contains it

**Principle and Application**—two levels for each dimension.

- Principle: What type governs situations like this?
- Application: How does this instance express that type?

**Practical application:**
- Ask the three questions about any situation
- Identify which dimension dominates
- Let analysis inform action

The framework doesn't impose structure on reality. It reveals structure that's already there.
