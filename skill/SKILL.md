# arXiv Talent Radar

Replaces an n8n + Claude + GitHub + Airtable arXiv sourcing workflow with a Claude-native pipeline: read a category feed, evaluate role fit paper by paper, cross-reference the first author on GitHub as a best-effort lead, score, and keep a live editable tracker.

## 1. Gather inputs

Ask for (or infer from context already given):
- Role context: title, must-haves, nice-to-haves, seniority
- arXiv category (default `cs.LG` if not specified — other common ones: `cs.CL`, `cs.CV`, `stat.ML`)
- Number of recent papers to process (default 20)

## 2. Fetch the arXiv feed

Fetch `https://arxiv.org/rss/<category>` with WebFetch. This works reliably from the cloud sandbox (unlike GitHub's API — see step 4). Take the most recent N entries requested.

For each entry, also fetch the abstract page (`https://arxiv.org/abs/<id>`) if the RSS summary doesn't give you enough to judge relevance — author list and full abstract usually live there.

## 3. Filter for role relevance

For each paper, read the title, abstract, and author list. Apply strict judgment against the role context as most papers on a feed will not be relevant. Only papers with genuine topical overlap with the role move forward. Discard the rest without further processing; don't enrich or score papers that fail this filter.

## 4. Cross-reference the first author on GitHub (best-effort)

For each paper that passes the filter, try to find the first author's GitHub profile (search by name, check for a linked GitHub in their institutional page if surfaced, or a matching username pattern). This is a hedge, not a guarantee — arXiv author names don't map cleanly to GitHub logins. Treat anything found here as an inferred signal, not a verified fact, and say so in the tracker.

GitHub access note: direct calls to `api.github.com` from the cloud sandbox are blocked by a proxy restriction regardless of any token. Use WebFetch for all GitHub lookups here, not Bash/curl. WebFetch hits GitHub's API unauthenticated on a shared IP and can 403 under load; retry once or twice before giving up on that lookup. If the user has linked their computer, GitHub calls can be run from there instead for higher, token-backed rate limits.

## 5. Exclude already-tracked / do-not-contact

Before scoring, check the existing tracker (if one exists for this role) for this author by full name. Skip anyone already marked `do_not_contact` or already present with an unchanged paper. This avoids duplicate entries across repeated runs.

## 6. Score

For each remaining candidate, produce:
- `fit_score` (0–100)
- `key_strengths` (from the paper and any GitHub signal)
- `key_gaps`
- `priority_action` (concrete next step)
- `profile_summary` (2–3 sentences)
- `outreach_hook` — a personalized line grounded in the actual paper content (cite the specific contribution, not a generic compliment)

Only candidates above a reasonable bar (use judgment relative to the role; there's no universal cutoff for research candidates the way there is for a downloads/stars threshold) get written to the tracker.

## 7. Create or update the live tracker

Use the Artifact tool with the `db` capability — this is the Airtable replacement. One tracker per role, titled `<Role> — arXiv Talent Radar`. If a tracker for this role already exists, update it (add new candidates, don't duplicate existing ones by full name) rather than creating a new one.

Tracker doc ID: slugified full name (arXiv doesn't give a stable username the way GitHub does).

Workflow-populated fields: `full_name`, `github_url`, `source`, `source_role`, `date_sourced`, `fit_score`, `priority_action`, `key_strengths`, `key_gaps`, `outreach_hook`, `profile_summary`, `paper_title`, `paper_url`, `arxiv_category`, `institution`, `author_position`, `career_stage`, `co_authors`.

Manual outreach fields (left for the user to fill in via the tracker UI, never overwritten by a re-run): `contacted`, `contact_date`, `replied`, `reply_sentiment`, `do_not_contact`, `notes`.

## 8. Report back

Summarize: how many papers were processed, how many passed the relevance filter, how many were scored and added to the tracker, and a link to the tracker. Flag any GitHub cross-references that are low-confidence guesses so the user knows to verify before outreach.

## Notes

- This skill can run on demand or on a schedule (weekly is a reasonable default if the user wants a recurring scan of a category).
- Never store a GitHub token in the tracker, in this skill file, or anywhere written to disk. If the user provides one for a session, use it only for that session's API calls.
- `author_position` and `career_stage` are inferred, not verified. Label them as such in the tracker/profile summary so outreach messages don't overstate certainty.
