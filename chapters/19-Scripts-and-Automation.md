# Chapter 19: Scripts and Automation

Scripts are Claude Code's sweet spot. Small, focused tools that solve specific problems. This chapter shows you how to build them effectively.

## The Script Mindset

A script is different from a program:

| Script | Program |
|--------|---------|
| Single file | Multiple files |
| Run and done | Persistent state |
| Command-line | User interface |
| Personal tool | Shared application |

Scripts are disposable. Build them fast, use them, move on.

## Quick Script Pattern

Start with the problem:

```
Create a Python script that:
- Reads all markdown files in a directory
- Extracts headings and creates a table of contents
- Saves the TOC as toc.md
```

Claude creates it. Test it. Refine if needed.

## File Processing Scripts

### Batch Renaming

```
Create a script that renames all files in a directory:
- Replace spaces with dashes
- Convert to lowercase
- Remove special characters except dashes and dots
- Preview changes before applying
```

### Log Analysis

```
Create a script that analyzes Apache access logs:
- Count requests per hour
- Find top 10 requested URLs
- Identify error patterns
- Output as formatted report
```

### Data Extraction

```
Create a script that:
- Reads a CSV file
- Filters rows where column 'status' is 'active'
- Extracts columns: name, email, signup_date
- Saves to new CSV with those columns only
```

## System Automation

### Backup Script

```
Create a backup script that:
- Takes a source directory as argument
- Creates timestamped archive: backup-YYYYMMDD-HHMMSS.tar.gz
- Excludes node_modules, .git, and __pycache__
- Keeps only last 5 backups
- Logs all operations
```

### Cleanup Script

```
Create a script that cleans development artifacts:
- Remove all node_modules directories
- Remove all __pycache__ directories
- Remove .pyc and .pyo files
- Remove .DS_Store files
- Show space recovered
- Confirm before deleting
```

### Environment Setup

```
Create a script that sets up a new project:
1. Create directory structure: src/, tests/, docs/
2. Initialize git repository
3. Create .gitignore for Python
4. Create virtual environment
5. Create requirements.txt template
6. Create basic README.md
```

## Web Scraping Scripts

```
Create a script that:
- Takes a URL as argument
- Fetches the page
- Extracts all links
- Filters to same-domain links only
- Saves as links.txt

Use requests and BeautifulSoup.
Handle errors gracefully.
```

## API Interaction Scripts

```
Create a script that:
- Reads a list of GitHub usernames from a file
- For each username, fetches their public repos via GitHub API
- Collects: repo name, stars, language
- Outputs as formatted markdown table

Include rate limiting and error handling.
```

## Text Processing Scripts

### Markdown Cleaner

```
Create a script that cleans markdown files:
- Normalize heading levels (no skipping)
- Fix broken links
- Remove trailing whitespace
- Ensure single blank line between sections
- Ensure file ends with newline
```

### Code Formatter

```
Create a script that extracts code blocks from markdown:
- Find all fenced code blocks
- Group by language
- Save each group to separate file: extracted-python.md, extracted-js.md
- Include the surrounding context for each block
```

## Report Generation

```
Create a script that generates a project summary:
- Count files by extension
- Calculate total lines of code
- Find largest files
- List TODO/FIXME comments
- Output as markdown report with sections
```

## Making Scripts Robust

Ask Claude to add proper error handling:

```
Update the script to:
- Validate all inputs before processing
- Handle file not found gracefully
- Catch and report network errors
- Exit with appropriate status codes
- Include --help flag with usage info
```

## Adding Configuration

For reusable scripts:

```
Update the script to:
- Read configuration from config.json if present
- Allow command-line arguments to override config
- Use sensible defaults if no config exists
- Support --config flag to specify alternate config file
```

## Testing Scripts

Before trusting a script:

```
Create test data for the script:
- Sample input files with edge cases
- Expected output for each case
- Include: empty files, unicode, very large files
```

Then:

```
Run the script on all test cases.
Compare actual output to expected.
Report any differences.
```

## Documenting Scripts

```
Add documentation to the script:
- Module docstring explaining purpose
- Function docstrings for each function
- Inline comments for complex logic
- Example usage in comments at top
```

## The Script Library

Over time, build a collection:

```
~/scripts/
+-- file-utils/
|   +-- batch-rename.py
|   +-- find-duplicates.py
|   +-- organize-downloads.py
+-- dev-tools/
|   +-- project-setup.py
|   +-- cleanup-artifacts.py
|   +-- generate-report.py
+-- data-processing/
|   +-- csv-filter.py
|   +-- json-transform.py
|   +-- log-analyzer.py
+-- web/
    +-- scrape-links.py
    +-- api-fetch.py
```

Each script is:
- Self-contained
- Well-documented
- Tested on sample data
- Ready to use

## From Script to Tool

When a script proves valuable, enhance it:

1. Add proper CLI argument parsing (argparse/click)
2. Add logging instead of print statements
3. Add configuration file support
4. Add tests
5. Create a README
6. Consider packaging for distribution

But don't over-engineer. Many scripts work perfectly as quick, simple tools.

## Script Best Practices

**Start small.** Get basic functionality working first.

**Test on copies.** Never run file-modifying scripts on original data first.

**Add dry-run mode.** Show what would happen without doing it.

**Be idempotent.** Running twice should give same result as running once.

**Log everything.** Future you will thank present you.

**Handle Ctrl+C.** Clean up gracefully on interrupt.

Scripts are the building blocks of automation. Master them, and you'll solve problems in minutes that used to take hours.

> **See Also:**
> - [Headless Mode and the SDK](13-Headless-Mode-and-the-SDK.md) for running scripts programmatically
> - [Mini Labs](30-Mini-Labs-Hands-On-Practice.md) for hands-on practice building scripts

---

**Next:** [Chapter 20: Full-Stack Applications](20-Full-Stack-Applications.md) — Build complete applications with frontend, backend, and database.
