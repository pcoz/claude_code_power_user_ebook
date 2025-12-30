# Chapter 31: Information Plus Understanding

Traditional APIs give you data. AI APIs give you interpretation. Together, they create something neither could achieve alone.

---

## The Two Kinds of APIs

**Traditional APIs** provide facts:
- Weather: "Tokyo is 18°C with 65% humidity"
- Finance: "AAPL is $187.50, up 2.3%"
- Maps: "Distance is 12.4 km, ETA 23 minutes"
- Sports: "Lakers 108, Celtics 102"

These are measurements. They exist independent of who asks.

**AI APIs** provide understanding:
- "Given this weather, is it a good day for hiking?"
- "Is this stock drop significant or noise?"
- "Based on traffic, should I leave now or wait?"
- "What does this game mean for playoff standings?"

These require interpretation. They depend on context, goals, and judgment.

---

## The Limits of Each

**Traditional APIs alone** can only apply rules you define in advance:

```python
if temperature < 65:
    print("Recommend a jacket")
if rain_chance > 25:
    print("Recommend an umbrella")
```

These rules handle obvious cases. They fail on nuance: "It's 68°F but windy, and you'll be outside for 3 hours at a shaded outdoor venue." No predefined rule covers this.

**AI alone** lacks access to current information. Ask "Should I bring an umbrella today?" and AI has no idea—it doesn't know today's weather. It can reason beautifully about hypotheticals but cannot ground that reasoning in current reality.

---

## The Synthesis

Combine them:

1. **Pull data from traditional API** — get the facts
2. **Send to AI API with context** — get the interpretation
3. **Use the reasoned conclusion** — take action

```
Create a weather advisor that:
1. Gets current weather from Open-Meteo (temperature, conditions, wind, UV)
2. Asks Claude: "Given this weather data and that the user is
   planning a 3-hour outdoor event, what should they prepare for?"
3. Returns Claude's advice
```

**Sample Output:**
```
Weather: 72°F, scattered clouds, 15mph gusts, UV index 7

Claude's Advice: Conditions are pleasant but gusty winds and
high UV warrant preparation. Bring sunscreen (SPF 30+),
sunglasses, and consider weighted anchors for any papers or
lightweight items. The wind may make it feel cooler than 72°F,
so a light layer might be welcome during breaks.
```

No amount of if-then rules could produce this advice. No pure AI model could know today's specific conditions.

---

## Patterns for Combining APIs

### Pattern 1: Data Retrieval → AI Interpretation

```
Build an investment alert system:
1. Fetch portfolio prices from finance API
2. Compare to 30-day moving averages
3. Ask Claude to analyze: "Here's my portfolio with today's prices
   and historical context. What patterns should I notice?
   Are any positions concerning?"
4. Send analysis via email if concerning
```

AI interprets the numbers in context—something a simple threshold alert cannot do.

### Pattern 2: AI Intent → API Action

```
Build a natural language calendar assistant:
1. User says: "Schedule lunch with Sarah next Tuesday"
2. Claude extracts: {action: "create_event", person: "Sarah",
   day: "next Tuesday", type: "lunch", duration: "1 hour"}
3. Use Google Calendar API to create the event
4. Return confirmation
```

AI handles the messy human input. Traditional API handles the precise action.

### Pattern 3: Multi-Source Synthesis

```
Build a trip planning assistant:
1. User provides: destination, dates, interests
2. Pull from APIs: weather forecast, flight prices, hotel availability, local events
3. Send all data to Claude: "Synthesize a trip recommendation
   considering these factors"
4. Return Claude's integrated analysis
```

Traditional APIs each provide a slice. AI synthesizes the whole picture.

### Pattern 4: AI Triage → Appropriate API

```
Build a customer support router:
1. User submits question
2. Claude analyzes: "Is this about billing, technical issue,
   account, or general inquiry?"
3. Based on classification, call appropriate internal API:
   - Billing: Check payment status API
   - Technical: Check system status API
   - Account: Check user profile API
4. Include relevant data in response
```

AI provides judgment about which facts matter. Traditional APIs provide those specific facts.

---

## Real-World Example: Smart Alerts

Traditional alert:
```
if stock_price < threshold:
    send_alert("Stock dropped below $150")
```

This fires on any drop, including normal volatility. Users get alert fatigue.

AI-enhanced alert:
```python
def smart_alert(symbol, price, history):
    # Traditional API: Get price data
    current = get_stock_price(symbol)
    history = get_price_history(symbol, days=30)
    news = get_recent_news(symbol)

    # AI API: Interpret significance
    analysis = claude.analyze(f"""
        Stock: {symbol}
        Current: {current}
        30-day history: {history}
        Recent news: {news}

        Is this price movement significant? Should the investor pay
        attention or is this normal volatility? Be specific about why.
    """)

    if "significant" in analysis.lower() or "attention" in analysis.lower():
        send_alert(symbol, current, analysis)
```

The AI filters noise. Users only get alerts that matter.

---

## The Democratization of Capability

Twenty years ago, building these applications required:
- Negotiating data partnerships
- Building infrastructure for data feeds
- Employing specialists to maintain systems
- Writing thousands of lines of business logic

Today:
- APIs provide the data (often free or cheap)
- AI provides the interpretation
- You describe what you want
- Claude writes the integration code

The playing field has leveled. Individual builders can create applications that combine data and intelligence in ways that were previously available only to well-funded engineering teams.

---

## What You Can Build

**Personal dashboards that interpret, not just display:**
Your morning briefing pulls weather, calendar, news, and portfolio data. AI summarizes what actually matters for your day.

**Smart alerts that understand context:**
Not just "stock dropped 5%" but "this drop looks like noise" or "this might warrant attention given your goals."

**Planning assistants that reason:**
Traditional APIs gather options—flights, hotels, weather, events. AI weighs them against your preferences and constraints to recommend the best combination.

**Analysis tools that synthesize:**
Feed customer feedback from multiple channels. AI identifies themes, sentiment shifts, and emerging concerns across all sources.

---

## Implementation with Claude Code

```
Build a "morning brief" application that:

1. Gets weather from Open-Meteo for my city
2. Lists my calendar events from a JSON file (events.json)
3. Gets top headlines from a free news API
4. Sends all this to Claude with prompt:
   "Synthesize a 3-paragraph morning briefing. What do I need
   to know? What should I prepare for? Keep it concise."
5. Prints the briefing

Include error handling for API failures.
```

Claude builds the entire integration. You describe the synthesis you want.

---

## The Formula

**Information + Understanding = Actionable Intelligence**

- Traditional APIs: The facts
- AI APIs: The interpretation
- Your design: What questions to ask and how to act on answers

This combination creates applications that don't just inform but genuinely help you analyze, decide, and act.

The data is available. The interpretation is available. The integration is now accessible to anyone who can describe what they want to build.

What will you combine?

> **See Also:**
> - [API Integration Patterns](21-API-Integration-Patterns.md) for working with traditional APIs
> - [AI-Powered Features at Runtime](22-AI-Powered-Features-at-Runtime.md) for adding Claude to your applications
> - [Weaving AI with Classical Computing](32-Weaving-AI-With-Classical-Computing.md) for the architectural perspective

---

**Next:** [Chapter 32: Weaving AI with Classical Computing](32-Weaving-AI-With-Classical-Computing.md) — Design systems that combine AI and traditional code effectively.
