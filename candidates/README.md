# Candidates

Each person using this repo gets their own folder here, like
`candidates/jane-doe/`. Everything in this folder except this README is
git-ignored, so your resume, contact details and job postings stay on your
machine.

```text
candidates/<your-name>/
  resume/              resume-v1.md, resume-v2.md, ... (your base resume)
    curated-resumes/   one tailored copy per job
  history/             everything you've done, in your own words (optional)
  job/                 job postings you're targeting, copied as-is
  feedback/            dated screen reports
```

To add yourself, either save your resume as
`candidates/<your-name>/resume/resume-v1.md`, or run
`/create-resume <your-name> <job posting URL>` and it sets up the folder for
you.

- [Getting started](../docs/getting-started.md): a step-by-step first run
- [Workflow](../docs/workflow.md): the loop for every job you apply to
- [Commands](../docs/commands.md): every argument, with examples
