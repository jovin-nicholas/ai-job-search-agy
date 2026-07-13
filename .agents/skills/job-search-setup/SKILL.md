---
name: job-search-setup
version: 1.0.0
description: >
  Handles onboarding setup, profile configuration, and candidate profile resets.
  Triggers on: setup, /setup, onboard, update profile, reset, /reset, clear candidate data.
---

# Job Search Setup & Maintenance

This skill manages onboarding, candidate profile updates, and framework resets.

---

## 1. Onboarding & Setup Workflow (/setup)

When the user asks to setup, onboard, or run `/setup`:
1. **Determine Path**:
   - Check the `documents/` folder. If it contains files (in `cv/`, `linkedin/`, `diplomas/`, or `references/`), recommend **Path A (Read documents folder)**.
   - If empty, offer **Path B (Single CV import)** or **Path C (Interactive interview mode)**.
2. **Process Inputs**:
   - **Path A**: Read all CVs, LinkedIn PDFs/text, and diplomas in the folder. Parse them, cross-reference dates and titles, check for inconsistencies, and merge additions/resolutions.
   - **Path B**: Parse the pasted CV, extract education and experience, and ask follow-up questions.
   - **Path C**: Interactively ask for contact information, education, experience, technical skills, behavioral traits, target sectors, and deal-breakers.
3. **Generate/Update Profile Files**:
   - Replace placeholders in `CLAUDE.md`.
   - Update candidate profile documents under `.claude/skills/job-application-assistant/` (specifically `01-candidate-profile.md`, `02-behavioral-profile.md`, `04-job-evaluation.md`, `05-cv-templates.md`, `07-interview-prep.md`).
   - Generate job search queries in `.claude/skills/job-scraper/search-queries.md`.

---

## 2. Reset Profile Workflow (/reset)

When the user asks to reset, clear data, or run `/reset`:
1. **Identify Scope**: Parse whether the user wants to clear `profile` data, delete `documents` files, or clear `all`.
2. **Report Wiped Content**: List the specific files and folders that will be affected and clarify what will remain unchanged.
3. **Request Confirmation**: Prompt: *"This cannot be undone. Type RESET (all caps) to confirm, or anything else to cancel."*
4. **Execute Wipe**:
   - **profile**: Revert `01-candidate-profile.md` and `02-behavioral-profile.md` to blank templates. Clear profile statements in `05-cv-templates.md` and STAR examples in `07-interview-prep.md`.
   - **documents**: Run bash commands to clean subfolders:
     ```bash
     rm -f documents/cv/*
     rm -f documents/linkedin/*
     rm -f documents/diplomas/*
     rm -f documents/references/*
     rm -rf documents/applications/*/
     ```
