---
name: update-resume
description: Improve a candidate's existing resume by working through screen-report fixes or the user's own change requests, with several recruiter agents drafting bullet options in parallel, then save the approved changes as the next resume version without touching the old one. Use when the user wants to apply review feedback, reword or add bullets, add a new role or project, or "fix" or "update" their resume, even if they only name one bullet. Not for a first screen (use review-resume) or a resume aimed at a new posting (use create-resume).
argument-hint: "[candidate] [resume version or path] [feedback report, or what to change]"
compatibility: Designed for Claude Code. Needs the recruiter subagent in .claude/agents/recruiter.md.
metadata:
  version: "1.0"
---

# Update Resume

Arguments: `$ARGUMENTS`

Turn feedback or a change request into an approved new resume version.
Recruiter agents draft the options, the candidate picks, and the result is
saved as a new file. The old version is never edited.

## 1. Resolve the inputs

Arguments come in any order.

- **Candidate:** a folder under `candidates/` (partial match is fine). Ask
  if it's unclear.
- **Resume:**
  - `v7` → `resume/resume-v7.md`. A path to a tailored copy is also fine.
  - Default: the highest-numbered `resume-vN.md`.
- **What to change**, first match wins:
  1. A path to a report under `feedback/`.
  2. Free text: "reword the dashboard bullet", "add my new project".
  3. Nothing given: use the top fixes from the newest report in
     `feedback/` written for this resume. If there's no such report, offer
     to run `/review-resume` first.

## 2. Pick the changes (approval gate)

List each proposed change in one line, quoting the current bullet exactly as
it stands in the file. Then ask which ones to work on, allowing several
picks. For a new role or project, ask the candidate what they did, what they
built it with, and what got better because of it, in their own words.

## 3. Spawn recruiters in parallel

Group the chosen changes by role. Spawn 1 recruiter per group, up to 5,
sending all Agent calls in one message (`subagent_type: recruiter`).

Brief for each:

```text
Rewrite mode. Candidate: <slug>. Resume: <path>.
Posting and Job Checklist, if this resume targets one: <paths or "none">.
Bullets to rewrite (read the file and quote them as they are now): <list>.
Why each needs changing: <the fix or request, quoted>.
Candidate context and past corrections: <everything the candidate said, plus corrections from earlier runs this session>.
Don't write any files.
```

## 4. Candidate picks (approval gate)

- Recount every option with `echo "<option>" | wc -w` before showing it.
- Check every option against the bullet rules in
  `.claude/agents/recruiter.md` and the candidate's corrections. Fix
  breaches or drop the option.
- Show one bullet at a time: current wording, the options with word counts,
  the recommended pick with a one-line reason, and the claims the candidate
  needs to confirm.
- If the candidate corrects a fact, re-check the remaining options. If
  needed, re-spawn that group's recruiter with the correction included.

## 5. Write the new version

1. Re-read the source resume right before writing. The candidate may have
   edited it by hand during the session. If an approved bullet's original
   line changed, show the difference and ask before going on.
2. Base resume (`resume-vN.md`): write `resume-v(N+1).md` with the approved
   changes, keeping everything else byte for byte. If `resume-v(N+1).md`
   already exists, ask instead of overwriting it.
3. Tailored copy: write a new file next to it with the next version number,
   or ask if the name doesn't follow `resume-vN-<slug>.md`.
4. Never edit the source file itself.

## 6. Keep versions in sync

Check whether any tailored copies under `resume/curated-resumes*/` contain
the old wording of a changed bullet. If so, list the files and ask whether
to carry the new wording over. When reporting, name which files changed and
which don't contain that bullet.

End with the new file's path, and offer `/review-resume <candidate>
<new version>` to check it.
