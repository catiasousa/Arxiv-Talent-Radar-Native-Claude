## What this project does

This project creates an arXiv-based sourcing workflow you run directly in Claude. It scans recent papers in a chosen category, evaluates role relevance, enriches authors with public profile context (best-effort), scores fit, and writes qualified candidates to a live tracker.

You can run it on demand for active roles or on a recurring schedule with a linked computer.

## Why arXiv

arXiv provides early signal on people doing current technical work. It is useful across AI/ML and many other technical fields, and it surfaces activity before many traditional sourcing channels update.

This workflow converts that signal into a repeatable sourcing process with structured scoring and tracking.

## The search methods this workflow runs

This workflow processes one arXiv category feed per run and evaluates the most recent N papers you choose.

**Method 1 — Category feed scan.**  
What it does: reads `https://arxiv.org/rss/<category>` for recent papers.  
Why it matters: gives a current stream of domain-specific candidates.

**Method 2 — Role relevance filter.**  
What it does: filters papers by title/abstract overlap with your hiring criteria.  
Why it matters: removes off-topic research before enrichment and scoring.

**Method 3 — Author cross-reference (best-effort).**  
What it does: attempts to map authors to GitHub profiles when possible.  
Why it matters: adds practical engineering/build signal to publication output.

## Setup details

**1) Install the skill**  
Give Claude `skill/SKILL.md` and ask it to save the skill.

**2) Connect access**  
No credentials are required for arXiv RSS. Optionally provide a GitHub token in chat for better cross-reference reliability. Never store tokens in files.

**3) Provide role context**  
Share title, must-haves, seniority, and domain focus.

**4) Choose search inputs**  
Set one arXiv category and recent paper count (for example: `cs.SE`, `cs.CR`, `eess.SP`, `physics.optics`, `math.*`, `stat.*`, `cs.LG`).

**5) Run a small test batch**  
Review tracker output quality and adjust criteria.

**6) Automate if needed**  
Set a recurring schedule for continuous sourcing.

**7) Set cadence**  
Choose a fixed rhythm (for example weekly).

## Customizing

- Change category and paper count by role priority.
- Tune scoring strictness by seniority and role type.
- Add tracker fields for domain-specific signals.
