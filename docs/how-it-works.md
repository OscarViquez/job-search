# How it works

## The pieces

```text
/review-resume  /update-resume  /create-resume      .claude/skills/<command>/SKILL.md
        │               │               │            the workflow: inputs, approval steps, saving
        └───────────────┼───────────────┘
                        ▼
              recruiter agents (2-5 in parallel)     .claude/agents/recruiter.md
                        │                            the writing and screening rules
          ┌─────────────┴─────────────┐
          ▼                           ▼
   recruiter-knowledge          job-checklist        .claude/skills/<name>/SKILL.md
   how a recruiter screens      what an ideal candidate
                                for one posting looks like
```

- **Commands** coordinate. They find your files, ask you to approve each
  step, merge what the recruiters return, and save the results.
- **Recruiter agents** do the screening and drafting. Each one runs on its
  own with a fresh context, which is why the commands pass your
  explanations and corrections into every run.
- **Skills** hold the recruiter's knowledge. Both are loaded into every
  recruiter agent.

## Why several recruiters?

One recruiter reading a resume once tends to fix on whatever it notices
first. Splitting the work gives each recruiter a narrower job:

- **Review, one posting:** three focuses.
  - First pass: what a recruiter sees in 20 seconds.
  - Bullets: is each one clear, specific, and ending on a result?
  - Credibility: can every claim be backed up in an interview?
- **Review, several postings:** one recruiter per job, run side by side.
- **Create and update:** one recruiter per job or project, drafting in
  parallel, so a full resume doesn't take five rounds in a row.

Each command uses at most 5 recruiters per run.

## The rules recruiters follow

These come from real screening practice and from corrections made while
using this repo.

| Rule | Why |
| - | - |
| The 20-second scan comes first | Recruiters skim for the minimum before reading anything. Required keywords near the top matter most. |
| Bullet = keyword + how + why | A keyword alone is a buzzword. A bullet shows you used it and what it achieved. |
| The "why" is the result, not how it works | "Helping teams build pages faster" beats "published to a database through a service". |
| Plain words | A recruiter outside your field must understand the result. |
| Exact tech names | If the posting asks for TypeScript, naming only a framework built on it may not count. |
| No numbers, with rare exceptions | Unverifiable metrics hurt credibility. Scale is shown in words ("thousands of monthly users"). |
| No headline or summary section | The bullets and the section order do that job. |
| Only claims you support | Nothing is added just because the posting mentions it. Guessed results are flagged for you to confirm. |
| About 20-30 words per bullet | Short bullets survive a skim. |
| No "so" and no semicolons in bullets | Use "to", "letting", "helping", or a comma instead. |

## Customizing

- **Change a writing rule:** edit the bullet rules in
  `.claude/agents/recruiter.md`. Every command picks up the change.
- **Change how screening works:** edit
  `.claude/skills/recruiter-knowledge/SKILL.md`. It also lists essential
  keywords by role, so you can add your own field there.
- **Change a workflow** (approval steps, file names, number of recruiters):
  edit that command's `.claude/skills/<command>/SKILL.md`.
- **Use a different model:** change `model:` in `.claude/agents/recruiter.md`.
  It's `sonnet` by default, since it's run several times per command.

Changes take effect the next time you run a command, with no restart needed.

## Design decisions

The `docs/adr/` folder records why the repo is laid out the way it is. For
example, why Job Checklists live outside candidate folders: two candidates
targeting the same job share one checklist. `CONTEXT.md` defines the terms
used everywhere: Candidate, Resume, Tailored Resume, Work History, Job
Posting, Job Checklist, Recruiter, and Screen Report.
