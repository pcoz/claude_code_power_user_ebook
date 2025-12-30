# Chapter 22: AI-Powered Features at Runtime

Here's something that might twist your brain: you can call an AI from your code.

The AI that helps you write code (Claude in your terminal) is one thing. The AI inside your application—that runs when users interact with it—is another.

## Build-Time vs Runtime AI

**Build-time AI**: Claude helps you write the code. You're in the terminal, having a conversation, building software.

**Runtime AI**: Your code calls Claude's API when the application runs. Users interact with AI features you've built.

You need runtime AI when your application needs to:
- Analyze text users provide
- Classify or categorize content
- Generate responses or content
- Make judgment calls on dynamic data

## Getting API Access

To call Claude from your code, you need an API key:

1. Go to console.anthropic.com
2. Create an account or sign in
3. Navigate to API Keys
4. Create a new key

Set it as an environment variable:

```bash
export ANTHROPIC_API_KEY="sk-ant-your-key-here"
```

## Your First API Call

```
Create a Python script that:
1. Takes a sentence as input
2. Sends it to Claude's API for sentiment analysis
3. Prints whether the sentiment is positive, negative, or neutral

My API key is in ANTHROPIC_API_KEY.
```

Claude creates:

```python
import anthropic
import os

client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

sentence = input("Enter a sentence: ")

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=100,
    messages=[
        {"role": "user", "content": f"Analyze the sentiment of this sentence and respond with only 'positive', 'negative', or 'neutral': {sentence}"}
    ]
)

print(f"Sentiment: {message.content[0].text}")
```

Run it, type a sentence, see the analysis.

## Practical Runtime AI Features

### Content Classification

```
Build a Flask endpoint that classifies support tickets:
- Receives ticket text via POST
- Calls Claude to classify as: billing, technical, account, other
- Returns JSON with classification and confidence
```

### Smart Search

```
Create a search feature that:
- Takes a natural language query
- Calls Claude to understand intent and extract keywords
- Queries the database based on extracted criteria
- Returns results ranked by relevance
```

### Content Generation

```
Build an email composer:
- User provides: recipient, purpose, key points
- Claude generates a professional email draft
- User can edit before sending
```

### Data Extraction

```
Create a receipt processor:
- Takes an image of a receipt
- Sends to Claude's vision API
- Extracts: merchant, date, items, total
- Returns structured JSON
```

## Prompt Engineering for Runtime

Runtime prompts need to be robust:

```
Create a prompt template for classifying support tickets.
It should:
- Handle edge cases gracefully
- Return consistent JSON format
- Work even with messy user input
- Include examples for the model
```

Example prompt template:

```python
CLASSIFICATION_PROMPT = """
Classify this support ticket into exactly one category.

Categories:
- billing: Payment issues, invoices, refunds
- technical: Bugs, errors, how-to questions  
- account: Login, password, profile changes
- other: Everything else

Respond with JSON only: {"category": "...", "confidence": 0.0-1.0}

Examples:
"I can't log in" -> {"category": "account", "confidence": 0.95}
"Charge me twice" -> {"category": "billing", "confidence": 0.9}

Ticket: {ticket_text}
"""
```

## Handling API Responses

Parse Claude's responses safely:

```
Update the classifier to:
- Parse JSON response safely (handle malformed JSON)
- Validate the response has required fields
- Fall back to "other" category if uncertain
- Log unusual responses for review
```

## Cost Management

API calls cost money. Be smart:

```
Add cost controls to the classification service:
- Cache identical requests
- Limit max tokens in response
- Track usage per user
- Set daily spending limits
- Use haiku for simple classifications
```

## Model Selection

Different models for different tasks:

| Task | Recommended Model |
|------|------------------|
| Simple classification | claude-haiku |
| Content generation | claude-sonnet |
| Complex reasoning | claude-opus |

```python
# Quick classification - use haiku (fast, cheap)
model = "claude-haiku-4-5-20251001"

# Content generation - use sonnet (balanced)
model = "claude-sonnet-4-5-20250929"

# Complex analysis - use opus (best quality)
model = "claude-opus-4-5-20251101"
```

## Async API Calls

For web applications, use async:

```
Convert the classifier to async:
- Use httpx or aiohttp for async HTTP
- Handle multiple classifications concurrently
- Add timeout handling
- Return results as they complete
```

## Streaming Responses

For long-form generation, stream the response:

```
Build a chat endpoint that streams Claude's response.
As Claude generates text, send it to the client in chunks.
Show text appearing word-by-word in the UI.
```

## Error Handling for Production

```
Add production-ready error handling:
- Retry on transient failures (with exponential backoff)
- Circuit breaker for sustained failures
- Graceful degradation when API is down
- Logging for debugging
- Metrics for monitoring
```

## Testing AI Features

```
Create tests for the classification service:
- Mock the Claude API for unit tests
- Test with known inputs and expected outputs
- Test error handling paths
- Include integration test against real API
```

## The Power of Runtime AI

With API access to Claude, you can build:

- **Classifiers**: Categorize anything
- **Analyzers**: Extract insights from text
- **Generators**: Create content on demand
- **Assistants**: Answer questions about your data
- **Translators**: Convert between languages or formats

Your application becomes intelligent. Not just following rules, but understanding intent and generating appropriate responses.

This is where the tools in this book come together: Claude Code helps you build software that itself uses Claude's intelligence at runtime.
