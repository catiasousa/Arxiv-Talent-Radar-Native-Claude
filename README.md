## What this project does

This project builds an arXiv sourcing system you run through Claude. It reads recent papers from selected categories, evaluates relevance to your role, cross-references authors to GitHub (best-effort), and writes qualified candidates to a live tracker.

Ask Claude any time you want a fresh batch for a role. With a linked computer and a scheduled task, it can also run unattended on a cadence you choose.

## Why arXiv

Manual research sourcing is slow and hard to scale consistently. arXiv gives early, high-signal evidence of current technical work across multiple domains, not only AI/ML.

This workflow turns that into a repeatable process with structured filtering, scoring, and outreach tracking.

## The search methods this workflow runs

This workflow reads one arXiv category feed per run and processes the most recent N papers you choose.

**Method 1 — Category feed.**  
What it does: reads `https://arxiv.org/rss/<category>`.  
Why it matters: gives a current stream of domain-specific candidates.

**Method 2 — Role relevance filtering.**  
What it does: filters papers by title/abstract overlap with your hiring criteria.  
Why it matters: removes off-target papers before enrichment/scoring.

**Method 3 — Author cross-reference (best-effort).**  
What it does: attempts to map authors to GitHub profiles.  
Why it matters: adds practical build/activity context to publication output.

## Setup details

**1) Install the skill**  
Give Claude `skill/SKILL.md` and ask it to save the skill.

**2) Connect access**  
No credentials are required for arXiv RSS. Optionally provide a GitHub token in chat when prompted for better GitHub cross-reference reliability (never store tokens in files).

**3) Tell Claude the role you're hiring for**  
Provide role title, requirements, and domain context.

**4) Choose your search criteria**  
Provide arXiv category and recent paper count (for example: `cs.LG`, `cs.CL`, `cs.CV`, `cs.SE`, `cs.CR`, `eess.SP`, `physics.optics`, `math.*`, `stat.*`).

**5) Run a test**  
Run a small batch first and review output quality.

**6) Automate it (optional)**  
Set up a recurring schedule if you want continuous sourcing.

**7) Schedule**  
Pick your cadence explicitly (weekly is a practical default for many research roles).

## Customizing

- Change categories and paper count on each run to match hiring priorities.
- Ask Claude to tighten or relax scoring strictness by role/seniority.
- Add tracker fields for domain-specific signals if needed.
