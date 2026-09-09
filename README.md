# arXiv Talent Radar — Claude Edition

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

A Claude-native fork of [Arxiv-Talent-Radar-with-Claude-n8n-Airtable](https://github.com/catiasousa/Arxiv-Talent-Radar-with-Claude-n8n-Airtable), same sourcing pipeline, rebuilt to run entirely inside Claude instead of n8n + Airtable.

## What this project does

This project creates an arXiv-based sourcing workflow you run directly in Claude. It scans recent papers in a chosen category, evaluates role relevance, enriches authors with public profile context (best-effort), scores fit, and writes qualified candidates to a live tracker.

You can run it on demand for active roles or on a recurring schedule with a linked computer.

## Why arXiv

arXiv provides early signal on people doing current technical work. It is useful across AI/ML and many other technical fields, and it surfaces activity before many traditional sourcing channels update.

This workflow converts that signal into a repeatable sourcing process with structured scoring and tracking.

## Workflow logic explained

The workflow is designed for repeatability and signal quality, not keyword-only scraping. Claude runs your sourcing criteria end to end in one conversational flow:

- Discovery from recent arXiv category output
- Relevance checks against your role definition
- Best-effort author enrichment with public profile context
- Fit scoring and shortlist decisions
- Live tracker updates for review and outreach

## Who this is for (and not for)

**This is for you if**
- You hire technical or research talent and value publication-based signal.
- You want earlier candidate discovery than traditional sourcing channels.
- You want repeatable shortlist logic with tracker-based follow-up.

**This is not for you if**
- You only hire roles where publication signal has little relevance.
- You cannot use external public web sources in your recruiting workflow.

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

## Accounts you need to create

- **arXiv**: no account required for public RSS/content access.
- **Claude** (claude.ai), with Cowork/Artifacts enabled.
- **GitHub** (optional), for best-effort author cross-referencing and additional context.

## How to get access set up

1. Open Claude and ensure your workspace allows skill usage.
2. Save this repo's `skill/SKILL.md` as a Claude skill.
3. Confirm web access is enabled in your Claude environment.
4. (Optional) Prepare a GitHub token for stronger profile lookups during runs.
5. Do not store tokens in repo files (`.env`, markdown, or config tracked by git).

## Quickstart — two ways to use it

### Option A — Install mode (recommended first)

Use this when setting up the workflow for the first time.

Prompt Claude to install from `skill/SKILL.md`, then provide:
- Target role/title
- arXiv category (or categories run one at a time)
- Recent paper count
- Scoring strictness

### Option B — Run mode (after install)

Use this for repeat sourcing runs.

Tell Claude:
- The role you are hiring for
- Category to scan
- Number of recent papers
- Any constraints (location, seniority, must-have domain signals)

## How the workflow operates

1. Claude reads recent entries from the chosen arXiv category feed.
2. Claude filters for role relevance from title/abstract signals.
3. Claude attempts author enrichment via public profile lookups (best-effort).
4. Claude scores fit and keeps candidates above your threshold.
5. Claude writes/updates rows in the live tracker for outreach workflow use.

## Tracker fields required by this workflow

Use these minimum columns in your tracker:

- Name
- Current role / affiliation
- Location (if available)
- arXiv link
- Category
- Relevance summary
- Score
- Outreach message
- Status
- Notes
- Last updated

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

## Repository structure

```text
.
├── .github/workflows/gitleaks.yml
├── .gitleaks.toml
├── .gitignore
├── LICENSE
├── README.md
└── skill/SKILL.md
```

## Security notes

This repo is a public skill template. It should contain logic only, never live secrets or personal data.

**What is safe to publish:** the skill's instructions, API endpoint patterns, field mappings, and example queries.

**What must not be committed:** GitHub tokens, Claude API keys, `.env` files, private keys, or real candidate data from any run.

Gitleaks runs via `.github/workflows/gitleaks.yml`. Run locally before pushing:

```bash
gitleaks detect --source . --verbose
```

## License

MIT License. See [LICENSE](LICENSE).
