# Chapter 21: API Integration Patterns

Your software can talk to other software. Weather services, payment processors, social platforms, databases—all accessible through APIs.

Understanding API integration patterns unlocks tremendous power.

## What APIs Are

An API is a conversation protocol between programs:

1. Your code sends a **request**: "What's the weather in Tokyo?"
2. The service sends a **response**: `{"temp": 18, "condition": "cloudy"}`

That's it. Question and answer over the network.

## The Basic Pattern

Every API call follows this structure:

```python
import requests

# Build the request
url = "https://api.example.com/data"
headers = {"Authorization": f"Bearer {api_key}"}
params = {"city": "Tokyo"}

# Make the request
response = requests.get(url, headers=headers, params=params)

# Handle the response
if response.status_code == 200:
    data = response.json()
    print(data["temperature"])
else:
    print(f"Error: {response.status_code}")
```

Claude writes this boilerplate for you. You describe what you want.

## Free APIs for Learning

Start with APIs that don't require authentication:

```
Create a script that fetches current weather from Open-Meteo
(no API key required) for a city I specify.
```

Open-Meteo, JSONPlaceholder, and similar free APIs are perfect for learning.

## Authenticated APIs

Most real APIs require an API key:

```
Create a script that fetches my recent GitHub repositories.
Use the GitHub API with my token in GITHUB_TOKEN environment variable.
```

**Never hardcode API keys.** Use environment variables:

```bash
export GITHUB_TOKEN="ghp_your_token_here"
```

Claude writes code that reads from environment:
```python
import os
token = os.environ.get("GITHUB_TOKEN")
```

## REST API Patterns

### GET — Retrieve data
```
Fetch the list of users from the API.
```

### POST — Create data
```
Create a new issue on GitHub with title and body.
```

### PUT/PATCH — Update data
```
Update the issue status to 'closed'.
```

### DELETE — Remove data
```
Delete the comment with ID 123.
```

## Error Handling

APIs fail. Handle it gracefully:

```
Update the script to handle:
- Network timeouts (retry 3 times)
- Rate limiting (wait and retry)
- Authentication errors (clear message)
- Invalid responses (log and continue)
```

Claude adds try/except blocks and retry logic.

## Rate Limiting

Most APIs limit how fast you can call them:

```
This API allows 100 requests per minute.
Add rate limiting to stay under the limit.
Process the list of 500 items without hitting rate limits.
```

Claude implements delays or uses rate limiting libraries.

## Pagination

Large datasets come in pages:

```
The API returns 20 results per page.
Fetch ALL repositories (there are 150+).
Handle pagination properly.
```

Claude loops through pages until exhausted.

## Combining Multiple APIs

Real power comes from combining APIs:

```
Create a script that:
1. Fetches weather data from Open-Meteo
2. Sends the weather summary to a Slack webhook
3. Runs daily via cron
```

```
Build a tool that:
1. Reads a list of stock symbols
2. Fetches current prices from a finance API
3. Compares to my target prices (from a config file)
4. Sends alerts to my email if targets are hit
```

## Webhooks

Webhooks are reverse APIs—the service calls YOU:

```
Create a Flask endpoint that receives GitHub webhooks.
When a push happens to main branch:
1. Log the commit messages
2. Trigger a build script
```

## API Wrappers

Many APIs have Python/JS libraries:

```
Use the official Stripe Python library to:
1. Create a customer
2. Add a payment method
3. Create a subscription

My Stripe key is in STRIPE_SECRET_KEY.
```

Claude knows popular libraries and uses them appropriately.

## Building Your Own APIs

Create APIs for your own services:

```
Build a Flask API for the expense tracker:

Endpoints:
- GET /api/expenses — list all expenses
- POST /api/expenses — create expense
- GET /api/expenses/:id — get single expense
- PUT /api/expenses/:id — update expense
- DELETE /api/expenses/:id — delete expense

Return JSON. Include proper status codes.
Use authentication via API key in header.
```

## API Documentation

Generate documentation:

```
Create API documentation for the expense tracker:
- List all endpoints
- Request/response examples
- Authentication details
- Error codes

Format as markdown suitable for README.
```

## Testing APIs

Before deploying:

```
Create tests for the expense API:
- Test each endpoint with valid data
- Test with invalid data (expect errors)
- Test authentication (valid/invalid keys)
- Test edge cases (empty lists, large payloads)

Use pytest with requests.
```

## Common Pitfalls

**Exposing secrets.** Never commit API keys. Use environment variables.

**Ignoring errors.** Always check response status codes.

**No rate limiting.** Respect API limits or get banned.

**Missing pagination.** Incomplete data if you don't fetch all pages.

**Synchronous everything.** For many API calls, consider async.

## The Integration Mindset

APIs let you stand on shoulders of giants:

- Need payments? Use Stripe's API
- Need email? Use SendGrid or Mailgun
- Need maps? Use Google Maps or Mapbox
- Need AI? Use Claude's API

Your code becomes an orchestrator, combining capabilities from specialized services into something new.

This is modern software development: composing APIs rather than building everything from scratch.

> **See Also:**
> - [Full-Stack Applications](20-Full-Stack-Applications.md) for building complete apps with APIs
> - [AI-Powered Features at Runtime](22-AI-Powered-Features-at-Runtime.md) for calling AI APIs from your code
> - [Scripts and Automation](19-Scripts-and-Automation.md) for building quick API-calling scripts

---

**Next:** [Chapter 22: AI-Powered Features at Runtime](22-AI-Powered-Features-at-Runtime.md) — Add AI intelligence to your applications.
