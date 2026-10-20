---
name: review-resume
description: Screen a candidate's resume against one or more job postings using several recruiter agents in parallel, then merge their findings into one verdict and a short ranked fix list saved under the candidate's feedback folder. Use when the user wants to know whether a resume would survive recruiter screening, how well it fits a job, or what to fix before applying, even if they just paste a job link or say "check my resume for this role." Not for rewriting bullets (use update-resume) or building a new resume (use create-resume).
argument-hint: "[candidate] [posting name, path, or URL ...] [resume version or path]"
compatibility: Designed for Claude Code. Needs the recruiter subagent in .claude/agents/recruiter.md and the job-checklist skill.
metadata:
  version: "1.0"
---

# Review Resume

Arguments: `$ARGUMENTS`

Fan out recruiter agents to screen one resume, merge what they find into one
report, and save it. You coordinate. The recruiter agents do the screening.

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

## 1. Resolve the inputs

Arguments can come in any order. Match each token against the repo:

- **Candidate:** a folder name under `candidates/` (partial match is fine:
  "jane" → `jane-doe`). If there's one candidate, use it. If
  there are several and none was named, ask with the folder names as options.
- **Resume:** `v6` or `resume-v6` → `candidates/<slug>/resume/resume-v6.md`.
  A path to a file (e.g. a tailored copy under `resume/curated-resumes*/`)
  is used as-is. Default: the highest-numbered `resume-vN.md` at the top of
  `resume/`.
- **Postings:** a path, or a word matched against file names under
  `candidates/<slug>/job/**` ("acme" → `.../acme-frontend-engineer.md`). A
  URL: fetch it and save the posting text verbatim to
  `candidates/<slug>/job/<role-family>/<company>-<role>.md` first. Pasted
  posting text: save it the same way. If a URL needs a login, gets blocked, or comes back without the
  requirements, don't work from a partial page. Ask the user to save the
  full job description as a file in `job/` and give its name (pasting into
  the chat also works, but uses more context). If nothing matches, or a word matches
  several files, ask, listing the matches as options. Never guess, and never
  cover several postings in one report.
- No posting at all: list the candidate's saved postings and ask which to
  use, with "Add a new job description" and "General screen, no posting"
  as the last options. If they have no saved postings, ask them to save
  one as a file in `job/` first.

Before spawning anything, tell the user in one line what will be reviewed:
"Reviewing resume-v2 for jane-doe against Acme Frontend Engineer with 3 recruiters."

## 2. Make sure every posting has a Job Checklist

For each posting, look for `job-checklist/<company>-<role>.md`. If one is
missing, run the `job-checklist` skill yourself now, before spawning
recruiters. Every recruiter must read the same checklist, and parallel
agents would otherwise race to write it.

## 3. Spawn the recruiters in parallel

Send all Agent calls in one message (`subagent_type: recruiter`) to run them
at the same time.

- **One posting:** 3 recruiters, one per focus below.
- **2 to 5 postings:** 1 recruiter per posting, each doing a full screen.
- **More than 5:** ask the user to pick up to 5.

| Focus | Covers |
| - | - |
| First pass | 20-second audit, which checklist items show in the top third of the page, formatting, timing and relocation lines |
| Bullets | qualification formula per bullet, outcome over mechanism, plain words (including bullets kept as-is), hot-dog audit, length, repeated words |
| Credibility | claims the resume can't back up, inferred outcomes, the metrics linter, overstated leadership, ownership signals, tone |

Give every recruiter this brief, filled in:

```text
Screen mode. Candidate: <slug>. Resume: <path>. Posting: <path>.
Job Checklist: <path> (already exists, don't rebuild it).
Focus: <focus and what it covers, or "full screen">.
Candidate context and past corrections: <notes from this conversation, or "none">.
<If the field isn't tech, e.g. healthcare: "Skip tech-specific advice. Judge against the checklist and the field's signals: certifications, patient contact, compliance, availability.">
The caller saves the report. Don't write any files. Return your report text,
with your verdict and up to 5 ranked fixes, each citing the exact line.
```

## 4. Merge into one report per posting

- **Verdict:** "would not survive" if any recruiter found a must-have from
  the checklist missing or buried. Otherwise use the majority read.
- **Fixes:** merge duplicates and rank by leverage, then keep at most 5
  (3 is typical). If two recruiters disagree, pick one and give the reason
  in one line.
- Drop any fix that breaks the repo rules in `.claude/agents/recruiter.md`:
  adding a headline or summary, asking for numbers, using "so" or
  semicolons, opening a bullet with a lead-in instead of an action verb,
  or changing the job titles the candidate chose.

Use the recruiter agent's Screen report format, with this one line added
under the verdict: `Reviewed by: <N> recruiters (<focuses>)`.

## 5. Save and show

Save each report to
`candidates/<slug>/feedback/resume-screen-<posting-slug>-<YYYY-MM-DD>.md`
(add `-2`, `-3` if that file already exists, and never overwrite). For a
general screen, use `resume-screen-general-<date>.md`.

Show the user the verdict and fixes, and the path where it was saved. For
several postings, end with a short ranking of best fit to worst fit, one
line each. Then offer the next step: `/update-resume <candidate>` to work
through the fixes.

## Gotchas

- Don't edit the resume during a review, even for an obvious typo. Report
  it as a fix instead.
- Re-read past reports in `feedback/` for the same posting, and tell the
  recruiters which fixes were already raised, so they don't repeat them as
  new.
- If the user named a tailored copy, review that file, not the base resume.
