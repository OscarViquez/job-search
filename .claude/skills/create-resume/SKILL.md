---
name: create-resume
description: Build a new resume tailored to a job posting, using several recruiter agents in parallel to draft bullet options from the candidate's current resume or their full work history, then assemble it only after the candidate approves each pick. Use when the user wants a resume for a specific job, a tailored version of their resume, or a first resume built from their projects and experience, even if they just say "make me a resume for this role" or paste a job link. Not for screening an existing resume (use review-resume) or editing the base resume (use update-resume).
argument-hint: "[candidate] [posting name, path, or URL] [from: resume | history]"
compatibility: Designed for Claude Code. Needs the recruiter subagent in .claude/agents/recruiter.md and the job-checklist skill.
metadata:
  version: "1.0"
---

# Create Resume

Arguments: `$ARGUMENTS`

Build a new resume for one job posting, with the candidate's approval at
every step. Recruiter agents draft the options. The candidate picks them.
Nothing is written to the resume file until the candidate has approved it.

## Asking the candidate

Ask every question with the AskUserQuestion tool, never as a list in chat
for the candidate to answer by typing.

- Up to 4 questions per call. For more, send several calls in a row (7
  bullets means two calls).
- A pick (bullet options, a posting, a plan): 2 to 4 options. Give the
  label a short name ("B, 28 words") and put the full text, word for word,
  in the description. List the recommended option first and add
  "(Recommended)" to its label.
- A fact in the candidate's own words: still ask with the tool. Offer the
  likely answers as options, and the candidate types anything else under
  "Other".
- Use multiSelect when several answers can be true ("Which agents are in
  daily use?").
- Keep the question text short, with only the context needed to answer.
  Show long material (a fit plan, a report) in chat first, then ask.

## Progress

- [ ] 1. Resolve the candidate, posting and source
- [ ] 2. Job Checklist ready
- [ ] 3. Fit plan approved
- [ ] 4. Recruiters drafted options in parallel
- [ ] 5. Candidate picked every bullet
- [ ] 6. Resume written and reported

## 1. Resolve the inputs

Arguments come in any order.

- **Candidate:** a folder under `candidates/` (partial match is fine). **New
  person:** create `candidates/<first-last>/` with `resume/`, `job/`,
  `feedback/` and `history/`. Then ask them to paste their resume or tell you
  about their work, and save what they give you to
  `history/work-history.md`, following
  [the work history template](assets/work-history-template.md).
- **Posting:** a path, a word matched against `candidates/<slug>/job/**`, a
  URL, or pasted text. Save URLs and pasted text verbatim to
  `job/<role-family>/<company>-<role>.md` first. Ask if nothing matches, or
  if several files match. One resume targets one posting. If a URL needs a login, gets blocked, or comes back without the
  requirements, don't work from a partial page. Ask the user to save the
  full job description as a file in `job/` and give its name (pasting into
  the chat also works, but uses more context).
  No posting given: ask for one before anything else, since the whole
  resume is built around it.
- **Source:**
  - `from: resume` uses the latest `resume/resume-vN.md`.
  - `from: history` uses everything in `history/`, plus the latest
    `linkedin/` file and resume.
  - Default: history if `history/` has files, otherwise resume.
  - Older resume versions are leads to confirm, never facts on their own.

## 2. Job Checklist

Reuse `job-checklist/<company>-<role>.md`, or run the `job-checklist` skill
now to build it.

## 3. Fit plan (approval gate)

Before any drafting, show the candidate:

1. **The job in plain words:** 2-3 sentences on what the role is really
   about.
2. **The fit:** each checklist item next to the experience from the source
   that backs it, quoting the candidate's own words. Mark any item with no
   evidence as a gap.
3. **The proposed structure:** which roles and projects to include, in what
   order, and which to cut because they don't fit this role type.

For each gap, ask whether the candidate has that experience. Never fill a
gap yourself. Get the plan approved (or corrected) before step 4.

## 4. Spawn recruiters in parallel

One recruiter per role or project entry in the approved plan, up to 5.
Group small entries together if there are more. Send all Agent calls in one
message (`subagent_type: recruiter`).

Brief for each:

```text
Rewrite mode. Candidate: <slug>. Posting: <path>. Job Checklist: <path>.
Entry: <title at company, dates>. Other entries on this resume: <one line each, so you can spread keywords>.
Source material for this entry (the only facts you may use): <quoted text from the source files, with paths>.
Candidate's approved wording for this work (base resume and other tailored copies): <quoted bullets, with paths, or "none">. Offer it as one option per bullet.
Checklist items this entry should cover: <items>.
Candidate context and past corrections: <everything the candidate said in steps 1-3>.
Write 3 to 5 bullets for this entry. For each bullet, give 2-3 options,
a pick, and a "Confirm with the candidate" line. Don't write any files.
```

## 5. Candidate picks (approval gate)

- Recount each option with `echo "<option>" | wc -w` before showing it.
  The agents' own counts can be wrong.
- Before showing anything, check every option against the
  [format rules](#format-rules) and the candidate's corrections. Fix
  breaches or drop the option.
- When the candidate has approved wording for a bullet in another tailored
  copy, include it as an option even if the recruiter left it out. The
  candidate usually picks it, or edits it, over fresh drafts.
- Present one entry at a time with AskUserQuestion (see
  [Asking the candidate](#asking-the-candidate)): one question per bullet,
  each option holding the full bullet and its word count, and the
  recommended pick first with a one-line reason in its description. Offer
  "Cut this bullet" as an option when cutting is a real choice. Then ask the
  claims the candidate needs to confirm as their own questions.
- If the candidate corrects a fact, re-check every remaining option against
  that correction. Re-spawn a recruiter for that entry if needed, passing
  the correction along.

## 6. Write and report

- **Candidate has a base resume:** write
  `resume/curated-resumes/resume-vN-<posting-slug>.md`, where N is the base
  version. If the candidate's folder uses another curated folder name (e.g.
  `curated-resumes-collection/`), use that one.
- **No base resume yet:** write `resume/resume-v1.md`. That becomes their
  base.
- Never overwrite an existing file. Add `-2` instead.

Copy the header (name, contact line, citizenship and location, education)
from the base resume, or ask for it if there's no base. Then report the
path. Offer `/review-resume <candidate> <posting> <new file path>` as the
next step.

If the candidate later shares their own final edits to the bullets,
re-read the file, apply the edits word for word, and compare them with the
picked options. Any new pattern (a cut qualifier, a softer verb, a
grouped list) is a correction for the recruiter agent's rules.

## Format rules

Follow [the resume template](assets/resume-template.md). Where it differs
from these rules (dates, bullet minimum), the rules win:

- No headline, title line, or summary/objective section.
- No bold "Keyword:" labels at the start of bullets. Start each bullet with
  a plain action verb, never a lead-in phrase ("As a founding engineer on
  X, architected..."). Don't tell recruiters a lead-in is allowed.
- Each bullet is keyword + how + why, and the why is the outcome. Aim for
  about 20-30 words.
- Never use "so" or semicolons in a bullet.
- No numbers except dates and contact info. Show scale in words ("thousands
  of monthly queries").
- Dates use full month names joined by "to": "October 2022 to March 2026",
  "March 2026 to Current".
- Name the foundational tech with the specific tool in parentheses:
  "SQL (PostgreSQL)".
- Under a project heading, put only its URL or an award, never a tech line.
- Each entry has 2 to 8 bullets. Don't split a bullet just to reach 3.
- Use the job titles the candidate chose, even when they differ from the
  official title.
