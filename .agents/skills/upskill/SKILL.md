---
name: upskill
version: 1.0.0
description: >
  Compares tracked job postings against the candidate profile to identify skill gaps and generate
  a prioritized learning plan with study resources.
  Triggers on commands and keywords: /upskill, upskill, skill gaps, what should I learn, learning plan.
---

# Upskilling & Skill Gap Analysis

This skill automates detecting hard/soft skill gaps against tracked job postings and creating prioritized learning plans.

---

## Workflow

### 1. Load Profile & Job Gaps
- **Targeted Mode**: Fetch and extract required skills from the job URL.
- **Aggregate Mode**: Load `job_search_tracker.csv` and weight skill gaps based on application fit ratings.
- Differentiate gaps by comparing against `.claude/skills/job-application-assistant/01-candidate-profile.md`.

### 2. Gap Heatmap & Synthesis
- Categorize gaps by type: `[Hard]`, `[Domain]`, `[Soft]`, `[Tooling]`, `[Credential]`.
- Assign priorities: Critical, High, Medium, or Low.
- Present the heatmap table to the user.

### 3. Study Resource Research
- Search the web for top-rated study resources (using `search_web` with queries including the current year).
- Pick 2-3 high-quality resources per gap (courses, books, or documentation).
- Draft a custom "Study Direction" tailored to the candidate's existing background.

### 4. Roadmapping
- Group the learning plan entries by theme (e.g., Cloud & Infrastructure, MLOps, Certifications).
- Outline a suggested study order based on course dependencies, priority, and quick wins.
- Estimate study times and print the total duration.

### 5. Report Export
- Compose the full report and save it locally:
  - **Aggregate**: `upskill/report-YYYY-MM-DD.md`
  - **Targeted**: `upskill/report-YYYY-MM-DD-<company-slug>-<role-slug>.md`
