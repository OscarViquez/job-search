# Job Search

Get your resume past the recruiter screen, using Claude Code.

The knowledge in this repo comes from two places: what I've learned
firsthand about hiring and resume reviews, and the advice of Lee Korelitz,
the Headless Headhunter, a recruiter who reviews resumes on
[YouTube](https://www.youtube.com/@headlessheadhunter) and
[Twitch](https://www.twitch.tv/headlessheadhunter)
Thank you, Lee.

The resume format used here, for me and for everyone I help, is his. Read
his guide, [How to Get a Job](https://www.headlessheadhunter.org/how-to-get-a-job),
for the format and the reasoning behind it. His reviews shaped how these
recruiter agents think, from the 20-second scan to plain, outcome-focused
bullets.

This repo turns Claude Code into a small team of technical recruiters. Give
it a job posting and your resume, and several recruiter agents screen it the
way a real recruiter does. That's the 20-second scan first, then the details.
They tell you what to fix, help you rewrite it, and build a version tailored
to each job you apply for.

You stay in control. Every bullet is offered as 2-3 options for you to pick
from, nothing is written until you approve it, and your existing resume
files are never overwritten.

## What you can do

| Command          | Use it to                                                                                    |
| ---------------- | -------------------------------------------------------------------------------------------- |
| `/review-resume` | See whether your resume would survive the recruiter screen for a job, and get the top fixes. |
| `/update-resume` | Work through those fixes, or your own changes, and save a new resume version.                |
| `/create-resume` | Build a resume tailored to one job, from your current resume or your full work history.      |

```text
/review-resume jane acme
/update-resume jane
/create-resume jane https://example.com/jobs/frontend-engineer
```

Arguments are optional and can go in any order. If something is missing or
unclear, Claude asks you with a short list to pick from.

## Quick start

1. Install [Claude Code](https://claude.com/product/claude-code) and clone this repo.
2. Open the repo folder in a terminal and run `claude`.
3. Save the job description you're applying to as a file, e.g.
   `candidates/<your-name>/job/frontend/acme-frontend-engineer.md`. Every
   review is judged against it. A file keeps the long posting out of your
   chat, which saves tokens. A link works too, but many job sites block
   automated reading.
4. Pick one:
   - **You have a resume:** save it as Markdown at
     `candidates/<your-name>/resume/resume-v1.md` (e.g. `jane-doe`), then run
     `/review-resume <your-name> acme`.
   - **Starting from scratch:** run `/create-resume <your-name> acme`. It
     sets up the rest of your folder and asks about your experience.

New to this? The [tutorial](docs/getting-started.md) walks through a full
first run in about 15 minutes.

## Documentation

- [Getting started](docs/getting-started.md): a step-by-step first run,
  from adding yourself to a tailored resume.
- [Workflow](docs/workflow.md): the loop to repeat for every job you apply
  to.
- [Commands](docs/commands.md): every argument, with examples.
- [How it works](docs/how-it-works.md): the recruiter agents, the rules
  they follow, and how to customize them.

## Your files

```text
candidates/<your-name>/
  resume/              resume-v1.md, resume-v2.md, ... (your base resume)
    curated-resumes/   one tailored copy per job
  history/             everything you've done, in your own words (optional)
  job/                 job postings you're targeting, copied as-is
  feedback/            dated screen reports
job-checklist/         what an ideal candidate looks like for each posting
```

## Privacy

Your candidate folder holds your contact details and work history. If you
fork this repo publicly, add `candidates/` to `.gitignore` or keep your fork
private.
