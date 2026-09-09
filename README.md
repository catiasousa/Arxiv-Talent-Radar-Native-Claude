## What this project does

This project reads newly posted arXiv papers and turns promising authors into scored, trackable candidates through Claude only, with no n8n and no Airtable required.

It scans selected arXiv categories, evaluates paper relevance against your role, cross-references authors on GitHub as a best-effort lead, and writes qualified candidates to a live tracker with a practical outreach workflow.

You can run it on demand whenever you need fresh sourcing, or run it on a schedule if you use a linked computer.

## Why arXiv

arXiv is a high-signal source of current technical work across many domains, not just AI/ML. It helps surface people actively publishing and building in fields such as:

- **Computer science** (systems, security, HCI, networking, software engineering)
- **Mathematics and statistics**
- **Physics and optics/photonics** (including laser-related research)
- **Quantitative biology**
- **Electrical engineering and signal processing**
- **Economics and quantitative social science**

Because papers appear early, arXiv often reveals active contributors before they become visible through traditional hiring channels.

This workflow turns that signal into a repeatable sourcing process: scan recent papers in selected categories, evaluate relevance to your role, cross-reference authors to GitHub (best-effort), score fit, and track qualified candidates in a live tracker.

## Who this is for (and not for)

**This is for you if**

- You hire research, engineering, or research-adjacent talent and want evidence from current technical output.
- You want earlier sourcing signals from publication activity, not only profiles or resumes.
- You want an on-demand (or scheduled) workflow that scores candidates and keeps outreach tracking in one place.

**This is not for you if**

- Your roles have no meaningful overlap with technical publication output.
- You cannot use external APIs or automated profile analysis due to policy constraints.

## The search methods this workflow runs

This workflow processes one arXiv category feed per run and evaluates the most recent N papers you choose.

**Method 1 — Category feed scan.**  
What it does: reads `https://arxiv.org/rss/<category>` for recent papers.  
Why it matters: gives a current stream of domain-specific candidates.

**Method 2 — Role relevance filter.**  
What it does: filters papers by title/abstract overlap with your hiring criteria.  
Why it matters: removes off-topic research before enrichment/scoring.

**Method 3 — Author cross-reference (best-effort).**  
What it does: tries to map authors to GitHub profiles where possible.  
Why it matters: adds practical engineering/build signal to publication output.

## Accounts you need to create

- **arXiv**: no account required for public RSS/content access.
- **Claude** (claude.ai), with Cowork/Artifacts enabled.
- **GitHub** (optional), for best-effort author cross-referencing and additional public activity context.

## Setup details

**1) Install the skill**  
Give Claude `skill/SKILL.md` and ask it to save the skill (see Quickstart, Install mode).

**2) Connect access**  
No credentials are required for arXiv RSS. Optionally provide a GitHub token in chat when prompted for better GitHub cross-reference reliability. Never store tokens in files.

**3) Tell Claude the role you're hiring for**  
Provide role context (title, must-haves, seniority, domain constraints).

**4) Choose your search criteria**  
Provide one or more arXiv categories and a recent paper count.

Examples by domain:
- AI/ML: `cs.LG`, `cs.CL`, `cs.CV`, `stat.ML`
- Systems/software: `cs.SE`, `cs.DC`, `cs.OS`, `cs.NI`
- Security/crypto: `cs.CR`
- Signal processing: `eess.SP`
- Optics/lasers/photonics-related: `physics.optics`
- Quant biology: `q-bio.*` (choose a specific subcategory)
- Math/stats: `math.*`, `stat.*` (choose specific subcategories)

**5) Run a test**  
Ask Claude to run a small batch first and review tracker output quality.

**6) Automate it (optional)**  
Ask Claude to set up a scheduled recurring run if you want continuous sourcing.

**7) Schedule**  
Pick the cadence explicitly (for example weekly for research-heavy hiring).

## Customizing

- Change categories and paper count each run to match hiring priorities.
- Ask Claude to tune scoring strictness by role seniority and domain.
- Add tracker fields for domain-specific signals (e.g., methods, tooling, citation/context notes).
