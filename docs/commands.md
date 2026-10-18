# Commands

All arguments are optional and can go in any order. Claude matches each one
against your files. If something is missing, it picks the obvious default.
If a word matches more than one thing, it asks.

## Argument types

| Type | You type | Matches |
| - | - | - |
| Candidate | `jane` or `jane-doe` | a folder in `candidates/`. If there's only one candidate, you can leave this out. |
| Posting | `acme`, a file path, a URL, or pasted text | a file under `candidates/<you>/job/`. URLs and pasted text are saved there first. |
| Version | `v2`, or a file path | `resume/resume-v2.md`, or any resume file, such as a tailored copy. The default is your newest `resume-vN.md`. |
| Source | `from: resume` or `from: history` | what `/create-resume` builds from (see below). |
| Change | `"quoted text"` or a report path | what `/update-resume` should work on. |

## /review-resume

```text
/review-resume [candidate] [posting ...] [version]
```

Screens a resume and saves one report per posting.

- **One posting:** 3 recruiters, one each for the first pass, the bullets,
  and credibility.
- **2-5 postings:** 1 recruiter per posting, plus a ranking by fit.
- **No posting:** Claude lists your saved postings to pick from, or a
  general screen.

```text
/review-resume jane acme
/review-resume jane acme globex initech
/review-resume jane acme v3
/review-resume jane acme candidates/jane-doe/resume/curated-resumes/resume-v3-acme-frontend-engineer.md
```

## /update-resume

```text
/update-resume [candidate] [version] [report or change]
```

Rewrites bullets with you and saves the next version.

- With no change given, it uses the fixes from your newest report for that
  resume.
- It runs 1 recruiter per group of bullets, up to 5.
- It saves to `resume-v(N+1).md`. Your current version isn't touched.

```text
/update-resume jane
/update-resume jane v2 "make the checkout bullet shorter"
/update-resume jane "add my new open-source project"
/update-resume jane candidates/jane-doe/feedback/resume-screen-acme-frontend-engineer-2026-10-17.md
```

## /create-resume

```text
/create-resume [candidate] [posting] [from: resume | history]
```

Builds a resume for one posting.

- `from: resume`: starts from your newest base resume.
- `from: history`: draws on everything in `history/`, plus your LinkedIn
  and resume.
- Default: `history` if that folder has files, otherwise `resume`.
- A new candidate name sets up the folder and asks about your experience.

```text
/create-resume jane acme
/create-resume jane https://example.com/jobs/123 from: history
/create-resume john-smith acme
```

The first time a name is used, it sets up `candidates/john-smith/`.

## Asking in plain words

You don't have to use the commands. "Check my resume for the Acme job" or
"make me a resume for this posting: <link>" start the same workflows.
