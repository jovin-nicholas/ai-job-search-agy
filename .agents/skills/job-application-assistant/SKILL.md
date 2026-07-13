---
name: job-application-assistant
version: 1.0.0
description: >
  Assists with job applications: evaluating job postings, tailoring CVs, writing cover letters,
  tracking application status, and preparing for interviews.
  Triggers on keywords and commands: /apply, /interview, /outcome, evaluate job posting,
  tailor cv, cover letter, resume, interview prep, job fit.
---

# Job Application Assistant

This skill guides the agent in conducting job evaluations, tailoring LaTeX application documents, and preparing for interviews.

## Core Reference Files

The skill uses profile parameters defined in the following relative paths:
- `.claude/skills/job-application-assistant/01-candidate-profile.md`
- `.claude/skills/job-application-assistant/02-behavioral-profile.md`
- `.claude/skills/job-application-assistant/03-writing-style.md`
- `.claude/skills/job-application-assistant/04-job-evaluation.md`
- `.claude/skills/job-application-assistant/05-cv-templates.md`
- `.claude/skills/job-application-assistant/06-cover-letter-templates.md`
- `.claude/skills/job-application-assistant/07-interview-prep.md`

---

## Workflows

### 1. Apply to a Job (/apply)

When the user provides a job posting (URL or text) and wants to apply:
1. **Evaluate Fit**: Score the posting against `01-candidate-profile.md` and `04-job-evaluation.md`. Present technical, experience, behavioral, and overall scores. If available, run `python salary_lookup.py "<Company Name>" --json`. Ask for approval to draft.
2. **Draft Documents**: If approved, generate:
   - CV in `cv/main_<company>.tex` (English only, moderncv banking template)
   - Cover letter in `cover_letters/cover_<company>_<role>.tex` (matches language of posting)
3. **Spawning Reviewer**: Use `invoke_subagent` to spawn a `self` subagent acting as a Hiring Manager reviewer. Pass the CV and Cover Letter drafts inline in the subagent prompt.
4. **Revise & Compile**: Apply reviewer feedback. Compile CV using `lualatex` and Cover Letter using `xelatex` via terminal commands.
5. **Inspect & Verify**: Check pages (CV = 2 pages, Cover Letter = 1 page). Verify text layer extracts cleanly using `pdftotext -layout <file>.pdf`. Present final pass/fail verification checklist to the user.

### 2. Prepare for Interview (/interview)

When the user asks to prepare for an interview:
1. Load `07-interview-prep.md`.
2. Generate STAR interview answers for high-probability questions.
3. Draft roleplay scenarios and potential questions to ask.

### 3. Record Outcome (/outcome)

When updating application statuses:
1. Update `job_search_tracker.csv`.
2. Save outcome checklist and feedback notes to `documents/applications/<company>_<role>/outcome.md`.
3. Suggest running setup/calibration if multiple applications are resolved.
