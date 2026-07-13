---
name: job-scraper
version: 1.0.0
description: >
  Finds, scrapes, and ranks new job postings matching your profile.
  Triggers on commands: /scrape, /rank, scrape jobs, find jobs, rank jobs, new jobs.
---

# Job Scraper & Ranking Triage

This skill discovers new job matches and ranks them against the candidate's profile to create a ranked shortlist.

---

## Workflows

### 1. Scrape for Jobs (/scrape)

When the user asks to scrape or find new job matches:
1. **Load State**: Read `job_scraper/seen_jobs.json` and `job_search_tracker.csv`.
2. **Identify Installed Portals**: Discover all installed CLI tools under `.agents/skills/*/SKILL.md`. Read their usage parameters (e.g. `linkedin-search`, `freehire-search`).
3. **Run CLI Searches**: 
   - Translate queries in `search-queries.md` to CLI flags.
   - Execute the CLI tools in parallel using background tasks.
   - Fall back to `WebSearch` if `bun` is missing or a portal doesn't have a CLI.
4. **Quick Triage & Save**:
   - Filter out already applied/seen positions.
   - Do a rapid high/medium/low match assessment.
   - Save matches to `job_scraper/seen_jobs.json`.
   - Present results in a neat Markdown table.

### 2. Rank Candidates (/rank)

When the user asks to triage, shortlist, or run `/rank`:
1. **Load State**: Read `job_scraper/seen_jobs.json` and find jobs with status `new`.
2. **Parallel Scoring**: 
   - Spawn multiple subagents using the `invoke_subagent` tool with `TypeName: self` or `research`.
   - Distribute the jobs to subagents (approx. 5 jobs per agent).
   - Subagents fetch the posting URLs via `read_url_content` and score them against the profile rubric (Technical, Experience, Behavioral, Career alignment).
3. **Aggregate & Shortlist**:
   - Compute overall weighted scores (Technical 30%, Experience 25%, Behavioral 15%, Career 30%).
   - Apply location and commute vetoes.
   - Sort by overall score and deadline urgency.
4. **Update State**: Save results with status `ranked`, `rank_score`, and `rank_verdict` to `job_scraper/seen_jobs.json`.
5. **Present Shortlist**: Render the ranked table and highlights. Prompt the user to start `/apply` on their top choices.
