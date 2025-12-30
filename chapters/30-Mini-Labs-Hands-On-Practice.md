# Chapter 30: Mini Labs — Hands-On Practice

Theory without practice is philosophy. Practice without theory is chaos. This chapter gives you both—structured exercises that let you experience vibe coding's rhythm firsthand.

Each lab takes 15-30 minutes. By the end, you'll have built real tools and internalized the pattern that makes everything else in this book work.

---

## The Pattern

Every lab follows the same rhythm:

1. **Notice** a small annoyance in your digital life
2. **Describe** it clearly to Claude
3. **Get** working code
4. **Test** it
5. **Describe** what went wrong or what you want different
6. **Get** updated code
7. **Repeat** until done

That's all vibe coding is. Everything else—the documents, the architecture patterns, the frameworks—exists to help you apply this pattern to bigger problems.

---

## Lab 1: Photo Organizer

**The Problem:**
You have folders of photos with names like `IMG_2847.jpg`, `DSC_0034.jpg`, and `Screenshot 2024-03-15.png`. You want them renamed by date: `2024-03-15_001.jpg`.

**Your First Prompt:**
```
Write a Python script that renames photos based on when they were taken.
The script should:
- Look at all image files in a folder I specify
- Read the date from EXIF data (or file modified date as fallback)
- Rename to format: YYYY-MM-DD_NNN.jpg
- Number sequentially if multiple photos on same day
- Skip files that are already in this format
```

**Expected Iterations:**
- First run: Missing library error → Claude tells you to install PIL/Pillow
- Second run: Some photos skipped → Ask for fallback to file modification date
- Third run: Files overwritten → Ask for duplicate handling

**What You Learn:**
- The AI knows about image metadata libraries
- Iteration is normal, not failure
- You make design decisions; AI handles implementation

---

## Lab 2: Text Cleaner for Messy Notes

**The Problem:**
Your text files are chaos—inconsistent bullets, random headings, extra blank lines.

**Your First Prompt:**
```
Write a Python script that cleans up messy text files.
Process all .txt files in a folder I specify.

Rules:
- Convert all bullet points to "- " (dash space)
- Standardize headings as # Heading (markdown style)
- Remove extra blank lines (max one blank between paragraphs)
- Keep numbered lists as-is
```

**Expected Iterations:**
- First run: Headings wrong → Refine what counts as a heading
- Second run: Some bullets missed → Specify all bullet variants

**What You Learn:**
- Defining "good" requires decisions from you
- Scripts can transform files without APIs or internet
- The AI doesn't have opinions about formatting—you do

---

## Lab 3: Download Folder Tidy

**The Problem:**
Your Downloads folder is chaos—hundreds of files of every type.

**Your First Prompt:**
```
Write a Python script that organizes my Downloads folder.

Move files to subfolders by type:
- Images: jpg, jpeg, png, gif, webp, svg
- Documents: pdf, doc, docx, txt, md
- Archives: zip, tar, gz, 7z, rar
- Videos: mp4, mov, avi, mkv
- Installers: exe, dmg, msi, deb
- Other: everything else

Create folders if they don't exist.
Print how many files were moved to each folder.
```

**Ask for Enhancements:**
- Dry-run mode to preview changes
- Handling uppercase extensions
- Skip folders (don't move folders into "Other")

**What You Learn:**
- You design the policy; AI implements it
- Safety features (dry-run) can be added after initial version
- The script becomes reusable—run weekly to keep things tidy

---

## Lab 4: API Weather Check

**The Problem:**
You want to check the weather without opening a browser.

**Your First Prompt:**
```
Write a Python script that tells me the current weather.
Use the Open-Meteo API (no API key required).
Accept a city name as a command line argument.
Print temperature, conditions, and humidity.
```

**Expected Output:**
```
$ python weather.py Tokyo
Tokyo: 18°C, Partly cloudy, 65% humidity
```

**What You Learn:**
- APIs give your code access to live data
- Open-Meteo is free—perfect for learning
- The pattern (request → response → display) applies everywhere

---

## Lab 5: Weather + AI Poetry

This lab combines two APIs—data from weather, creativity from Claude.

**Your First Prompt:**
```
Write a Python script that:
1. Gets weather data from Open-Meteo for a city I specify
2. Sends that weather data to Claude's API
3. Asks Claude to write a haiku about the current conditions
4. Prints both the weather data and the haiku

Use ANTHROPIC_API_KEY from environment variables.
```

**Sample Output:**
```
Tokyo: 18°C, Light rain

Gray skies weep softly
Umbrellas bloom like flowers
Petrichor rises
```

**Variations to Try:**
- Ask for a weather report "in the style of a nature documentary"
- Ask for activity recommendations based on conditions
- Compare two cities poetically

**What You Learn:**
- Traditional APIs provide facts; AI APIs provide interpretation
- Information + understanding = powerful applications
- The same mechanics enable very different outputs

---

## Lab 6: The Analyst in a Box (No Code!)

**The Problem:**
You have messy interview notes and need structured analysis.

**Input (paste to Claude):**
```
talked to janet from acme corp, uses our product about 6 months
loves the dashboard "finally can see everything in one place"
onboarding was rough - took 2 weeks to get team trained
would pay more for better reporting
mentioned competitor X has better mobile app
team adoption was mixed at first, now everyone uses it daily
biggest complaint: cant export to excel easily
asked about API, wants to integrate with their CRM
"if you fixed the export thing id recommend you to everyone"
renewal coming up in march, will definitely renew
```

**Your Prompt:**
```
Analyze these customer interview notes.
Produce a structured report with:
- SUMMARY: 2-3 sentence overview
- SENTIMENT: Overall feeling about our product
- WINS: What they love
- PAIN POINTS: What frustrates them
- FEATURE REQUESTS: What they want added
- COMPETITIVE INTEL: Mentions of other products
- RISK FACTORS: What might cause problems
- RECOMMENDED ACTIONS: What we should do next

Use direct quotes where relevant.
```

**What You Learn:**
- Vibe coding without code—the AI is your analyst
- Structure comes from you; synthesis comes from AI
- This scales: 100 interviews take a morning

---

## Lab 7: Hello, Claude (From Code)

**The Problem:**
You want to call Claude from your own application.

**Setup First:**
```bash
pip install anthropic
export ANTHROPIC_API_KEY="sk-ant-your-key"
```

**Your First Prompt:**
```
Write the simplest possible Python script that:
1. Sends "What's 2+2?" to Claude's API
2. Prints Claude's response

Use the anthropic library.
Read API key from ANTHROPIC_API_KEY environment variable.
```

**What You Get:**
```python
import anthropic
import os

client = anthropic.Anthropic()

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=100,
    messages=[{"role": "user", "content": "What's 2+2?"}]
)

print(message.content[0].text)
```

**What You Learn:**
- The same Claude that helps you build code can live inside your code
- API calls are just structured conversation
- This is the foundation of every AI-powered feature

---

## Lab 8: Document Q&A

**The Problem:**
You want to ask questions about a document.

**Your Prompt:**
```
Write a Python script that:
1. Reads a text file specified on command line
2. Takes a question as input
3. Sends both document and question to Claude
4. Prints Claude's answer

Include a system prompt telling Claude to:
- Answer only from the document
- Quote specific passages when relevant
- Say "Not found in document" if the answer isn't there
```

**Test It:**
Save any text document. Run:
```
$ python docqa.py report.txt
Question: What was the main finding?
```

**What You Learn:**
- AI can work with your specific data
- System prompts shape behavior
- This pattern powers every RAG application

---

## Lab 9: Sentiment Classifier

**The Problem:**
You want to automatically classify customer feedback.

**Your Prompt:**
```
Write a Python script that classifies sentiment.
Accept text input.
Return JSON: {"sentiment": "positive/negative/neutral", "confidence": 0.0-1.0}

The prompt to Claude should include examples for consistency.
Handle cases where Claude returns non-JSON gracefully.
```

**What You Learn:**
- AI can return structured data, not just prose
- Examples in prompts improve consistency
- Error handling matters for production code

---

## Lab 10: Build Your Own Tool

Now create something for yourself.

**The Process:**
1. Identify a small annoyance in your workflow
2. Describe it in one paragraph
3. Ask Claude to build it
4. Test and iterate
5. Keep the working script

**Ideas:**
- Rename files by pattern
- Extract data from messy text
- Generate boilerplate from templates
- Summarize long articles
- Convert between formats

**What You Learn:**
- You can build tools for your specific needs
- The investment in getting it right pays off
- Custom automation is now accessible to everyone

---

## The Compound Effect

Each lab teaches a piece:

| Lab | Skill Learned |
|-----|---------------|
| 1-3 | Local file automation |
| 4-5 | Traditional + AI APIs |
| 6 | AI as analyst (no code) |
| 7-9 | Runtime AI integration |
| 10 | Custom tool creation |

Combined, these skills let you build remarkably capable applications. The pattern—describe, test, refine—scales from 15-minute scripts to months-long projects.

Practice the small things. Then build the big things.

> **See Also:**
> - [Scripts and Automation](19-Scripts-and-Automation.md) for building reusable scripts
> - [API Integration Patterns](21-API-Integration-Patterns.md) for connecting to external services
> - [AI-Powered Features at Runtime](22-AI-Powered-Features-at-Runtime.md) for adding AI to your apps

---

**Next:** [Chapter 31: Information Plus Understanding](31-Information-Plus-Understanding.md) — Combine data APIs with AI interpretation.
