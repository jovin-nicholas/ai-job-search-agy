# Project Guidelines: AI Job Search

This workspace is structured to manage job search activities, scraper tools, CVs, cover letters, and interview preparation. 


## General Principles
1. **Zero-Duplication for Customizations**: The core workflow instructions are maintained in the reference markdown files (`.claude/skills/job-application-assistant/*.md`). Do not duplicate these files or instructions. Reference them as the single source of truth.
2. **Honest Evaluation**: Never fabricate experience, skills, or certifications. If a job posting lists a required skill that is a gap, acknowledge it honestly or highlight adjacent competencies.
3. **Data Integrity**: Track all applications in `job_search_tracker.csv` and keep state in `job_scraper/seen_jobs.json`. Do not break their structural schemas.
## Document Formatting & Templates
* **CV (`cv/main_<company>.tex`)**:
  - Must always be in **English**.
  - Restrict strictly to **exactly 1 page** (matching the formatting structure, packages, and sizes defined in the default template).
  - Draw source content from your master candidate content repository and tailor it to the target job description.
  - Compile using **`lualatex`**.
* **Cover Letter (`cover_letters/cover_<company>_<role>.tex`)**:
  - Must **match the language of the job posting** (e.g., Danish posting -> Danish letter).
  - Restrict strictly to **exactly 1 page**.
  - Compile using **`xelatex`** (since `cover.cls` uses `fontspec`).
  - Bullet list fonts must match the Raleway-Medium body text font.

## Verification Checklist
Before presenting compiled PDFs (CV and cover letter) to the user, the agent must verify:
- [ ] Visual PDF inspection (CV = 1 page, Cover Letter = 1 page).
- [ ] No orphaned titles/headings.
- [ ] Text extraction check (`pdftotext -layout <file>.pdf`) to ensure email, phone, and readable content are visible to ATS parsers without garbage encodings.
