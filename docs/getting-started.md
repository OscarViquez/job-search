# Getting started

This tutorial follows Jane Doe, a frontend engineer applying to a
(made-up) Frontend Engineer job at Acme. Use your own name and a real
posting as you go. It takes about 15 minutes.

## Before you start

- Install [Claude Code](https://claude.com/product/claude-code).
- Clone this repo, open the folder in a terminal, and run `claude`.
- Find one job posting you want to apply to, and keep its full text handy.
  A link alone often won't load (see [Step 2](#step-2-add-the-job-posting)).

## Step 1: Add yourself

Choose a folder name for yourself: lowercase, with hyphens, like `jane-doe`.
You'll type part of it in every command, and `jane` is enough.

**If you have a resume,** paste it into a new file as plain Markdown:

```text
candidates/jane-doe/resume/resume-v1.md
```

The exact formatting doesn't matter. Claude only needs the text: contact
line, education, and each job with its bullets. The resumes this repo
builds follow the format from
[How to Get a Job](https://www.headlessheadhunter.org/how-to-get-a-job),
the guide by Lee Korelitz, the Headless Headhunter.

**If you don't have one,** or it's out of date, skip to
[Step 6](#step-6-build-a-resume-for-the-job). `/create-resume` sets up your
folder and builds one with you.

**Optional, but worth it:** create `candidates/jane-doe/history/work-history.md`
and write down everything you've done, in your own words. Messy notes are
fine. The more Claude knows, the fewer questions it asks, and the less it
has to guess. There's a template at
`.claude/skills/create-resume/assets/work-history-template.md`.

## Step 2: Add the job posting

Every review and tailored resume is judged against a real job description,
so add the posting before anything else. Without one, you only get a
general screen.

**Recommended: save it as a file.** Copy the whole posting, including the
requirements and "nice to have" sections, into a new file named after the
company and role:

```text
candidates/jane-doe/job/frontend/acme-frontend-engineer.md
```

The middle folder groups similar roles (`frontend`, `ai`, `healthcare`,
...), so name it whatever makes sense to you. Then refer to the posting by a
short name: `/review-resume jane acme`. This keeps the long posting out of
your chat, which saves tokens and leaves more room in the conversation for
the actual work. The recruiter agents read the file on their own.

Other ways, if they're easier:

- **Give the link.** `/review-resume jane https://example.com/jobs/acme-frontend-engineer`.
  Claude saves the page to your `job/` folder for you. Many job sites
  (LinkedIn, Workday, and most applicant tracking systems) need a login or
  block automated reading. If the link doesn't load, save the file yourself.
- **Paste it into the chat.** This works, but the whole posting then stays
  in the conversation for the rest of the session. Use it for short
  postings, or start a fresh session afterward.

Either way, the posting ends up saved word for word in your `job/` folder,
and from then on a short name is enough.

## Step 3: Review your resume against the job

```text
/review-resume jane acme
```

What happens:

1. It builds a **Job Checklist**: the 8-12 things an ideal candidate for
   this job has. The checklist is saved in `job-checklist/` and reused next
   time.
2. It tells you the plan in one line, then starts 3 recruiter agents at once.
   Each one looks at your resume from a different angle: the first 20-second
   pass, the quality of each bullet, and whether every claim holds up.
3. It merges their findings into one short report:

```markdown
# Resume Screen: Jane Doe — Acme Frontend Engineer

## Verdict

Would not survive the 20-second recruiter screen for this role.
Single highest-leverage fix: name TypeScript in the first bullet.
Reviewed by: 3 recruiters (first pass, bullets, credibility)

## Top fixes

1. Name TypeScript in your first Acme bullet — the posting requires it and
   it only appears in your skills list.
2. ...
```

The report is saved in `candidates/jane-doe/feedback/` with today's date, so
you can compare reports over time.

## Step 4: Fix it

```text
/update-resume jane
```

1. Claude lists the fixes from your latest report and asks which ones to
   work on. Pick as many as you like.
2. For each bullet, you get 2-3 options with word counts, a recommended pick
   with the reason, and any claims you need to confirm:

```markdown
### Bullet: "Worked on the checkout page using React."

1. "Built the checkout page in React and TypeScript, using REST APIs to
   show live prices, helping shoppers finish orders without errors." (21 words)
2. ...

Pick: 1, because it names the required keywords and ends on what
shoppers got.
Confirm with the candidate: "without errors" was inferred. Is that true?
```

3. Pick an option, ask for changes, or explain what really happened. Your
   explanation is the best input you can give. The strongest bullets come
   from you telling Claude what actually got better.
4. When you're done, Claude saves `resume-v2.md`. `resume-v1.md` is left
   untouched.

## Step 5: Review again

```text
/review-resume jane acme v2
```

This time "acme" is enough, because the posting is already saved. Repeat
steps 4 and 5 until the verdict says the resume would survive.

## Step 6: Build a resume for the job

Once your base resume is solid, make a copy aimed at this one job:

```text
/create-resume jane acme
```

1. **Fit plan:** Claude explains the job in plain words. It shows which of
   your experience matches each checklist item, and asks about any gaps.
   It never fills a gap for you. If you have the experience, tell it. If
   not, the gap stays a gap.
2. **Drafting:** one recruiter agent per job or project drafts bullet
   options, all at the same time.
3. **Picking:** you choose bullets one entry at a time, as in step 4.
4. **Saved:** `candidates/jane-doe/resume/curated-resumes/resume-v2-acme-frontend-engineer.md`.

Finish with `/review-resume jane acme <that file>` to check the tailored
copy.

## Tips

- **Talk to it.** At any point you can say "that's not what happened, it was
  actually..." Corrections are carried into every later recruiter run.
- **Edit by hand anytime.** Claude re-reads your file before writing and
  tells you if something changed.
- **One resume per job.** Tailored copies work best when each one targets a
  single posting.
- **Not a tech job?** The recruiters notice and judge against the posting's
  own signals (certifications, availability, and so on) instead of tech
  keywords.

## Troubleshooting

| Problem | Fix |
| - | - |
| `/review-resume` doesn't appear | Run `claude` from the repo root, where `.claude/` lives. |
| "Which candidate?" every time | Type part of your folder name first: `/review-resume jane ...` |
| The posting link won't load | Save the posting text as a file in your `job/` folder, then refer to it by name. |
| A suggestion claims something you didn't do | Say so. It's listed under "Confirm with the candidate" for exactly this reason. |

Next: [the workflow for every job you apply to](workflow.md).
