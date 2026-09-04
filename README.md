# arXiv Talent Radar — Claude Edition

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A Claude-native fork of [Arxiv-Talent-Radar-with-Claude-n8n](PASTE-EXACT-URL-HERE), same sourcing pipeline, rebuilt to run entirely inside Claude instead of n8n + Airtable.

## What this project does

This project reads newly posted arXiv papers and turns promising authors into scored, trackable candidates through Claude only, with no n8n and no Airtable required.

It reads a category feed, evaluates paper relevance against your role, cross-references the first author on GitHub as a best-effort lead, and writes qualified candidates to a live tracker with a personalized outreach hook based on real paper content.

You can run it on demand whenever you need fresh sourcing, or run it on a schedule if you use a linked computer.

## What arXiv is and why it is one of the most underused sourcing tools in AI hiring

arXiv is where AI researchers and engineers publish their newest technical work in real time. It is not a social feed and not a hiring platform. It is the earliest public signal of who is actively pushing the field forward through original research and implementation.

Why it is uniquely powerful: arXiv captures current momentum before it appears on resumes, LinkedIn updates, or conference talks. If someone is consistently publishing relevant work in your target area, that is direct evidence of active domain depth, technical focus, and recent execution.

Three types of talent signals you find here: first authors who often drive core execution on a paper, senior or last authors who often indicate research leadership, and repeat contributors who show sustained output in a narrow technical domain over time.

What this guide builds: a Claude-native sourcing workflow that runs on demand or on schedule, reads new arXiv papers from selected categories, evaluates role fit, cross-references likely GitHub profiles, scores promising candidates, and saves them in a live editable tracker with outreach hooks grounded in actual paper content.

## Workflow logic explained

This workflow reads one arXiv category feed per run, with `cs.LG` as the default.

For each paper, Claude reads the title, abstract, and author list and applies strict role-fit judgment.

Relevant papers move forward to first-author GitHub cross-reference as a best-effort identity lead.

Qualified candidates are saved to a live tracker that separates verified facts from inferred signals.

## Who this is for

This project is ideal for teams recruiting ML and AI research or research-adjacent engineering talent who want current publication signals and a repeatable sourcing process.

This project is not a fit if your role has no meaningful research overlap, or if policy constraints prevent external API use or automated profile analysis.

## How discovery works

You choose one arXiv category per run, for example `cs.LG`, `cs.CL`, `cs.CV`, or `stat.ML`.

The feed endpoint is:

`https://arxiv.org/rss/<category>`

Each run processes the most recent N papers you specify, with 20 as a practical default if you do not set one.

## Accounts and access

You need a GitHub account and a Claude account with Cowork or Artifacts enabled. arXiv itself does not require an account.

An optional GitHub personal access token can improve API reliability and rate limits when cross-referencing authors.

Never store tokens in repo files or committed content.

## Quickstart

### Install mode
Open Claude, attach or paste `skill/SKILL.md`, and ask Claude to save it as a skill.

After saving, provide role context, arXiv category, and paper count whenever you want a sourcing run.

### One-off mode
Paste `skill/SKILL.md` into Claude and ask it to run once for your role without saving as a reusable skill.

## How the workflow operates

1. You provide role context, category, and number of recent papers.
2. Claude fetches and parses the arXiv feed.
3. Claude evaluates relevance and filters strictly.
4. Claude cross-references first authors on GitHub.
5. Claude scores candidates and updates a live tracker.
6. You review, contact, and track outreach in one place.

## Tracker fields

### Workflow-populated fields

`full_name`, `github_url`, `source`, `source_role`, `date_sourced`, `fit_score`, `priority_action`, `key_strengths`, `key_gaps`, `outreach_hook`, `profile_summary`, `paper_title`, `paper_url`, `arxiv_category`, `institution`, `author_position`, `career_stage`, `co_authors`

### Manual outreach fields

`contacted`, `contact_date`, `replied`, `reply_sentiment`, `do_not_contact`, `notes`

## Security notes

This repository is a public template and should include logic only, never live secrets or personal data.

Safe to publish includes workflow instructions, endpoint patterns, field mappings, and example queries.

Never commit tokens, API keys, `.env` files, private keys, or real candidate data.

Gitleaks is configured in `.github/workflows/gitleaks.yml`.

Run local checks before pushing.

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

## License

MIT License, Copyright (c) 2026 Catia Sousa.
