---
name: job-checklist
description: Reads a job posting and distills it into a short, recruiter-style checklist (~8-12 items) of what an ideal candidate for that role should have — the concrete artifact an experienced recruiter keeps at hand before ever looking at a resume. Use before screening a resume against a posting that doesn't have a checklist yet, or when asked to build/update a candidate checklist or "ideal candidate" profile for a job posting.
metadata:
  version: "1.0"
---

# Job Checklist

Produces the artifact an experienced recruiter builds mentally (or on paper)
before ever looking at a resume: a short, scannable checklist of what an
ideal candidate for one specific Job Posting should have. This skill never
reads or compares against a resume — see `recruiter-knowledge` for the skill
that screens an actual resume, using a checklist like this one as its
concrete keyword set for the role.

## Workflow

### 1. Get the job posting

Look under `candidates/<candidate>/job/**/<company>-<role>.md`, or ask for
one if not given. You need the company name, role title, and the full
requirements/preferred-qualifications text — don't work from a paraphrase or
a generic role profile.

### 2. Check for an existing checklist

Look under `job-checklist/<company-slug>-<role-slug>.md`. A Job Checklist
belongs to the posting, not to any one candidate — if two candidates target
the same company and role, reuse the existing file instead of building a
duplicate. Only rebuild it if the posting's requirements have materially
changed since the existing checklist was written, and say so if you do.

### 3. Apply the hamburger/hot-dog filter

For each requirement, name the foundational technology actually being asked
for — not an adjacent or niche tool that merely implies it (same principle
as `recruiter-knowledge`'s hot-dog audit). Examples:

- `Node.js` → `JavaScript/TypeScript`
- `Azure Data Factory` → `Azure`
- `FastAPI` → `Python`

Only give the niche/trendy tool its own checklist line if the posting names
nothing more foundational underneath it.

### 4. Group into ~8-12 checklist lines

Group related must-haves into one scannable line, the way a recruiter would
say it out loud — not a one-keyword-per-row table. Typical groups (drop
whatever doesn't apply to this posting; add a line for anything distinct
that doesn't fit, e.g. a security clearance or certification):

- Degree/education, if the posting requires one
- Core language(s), grouped (e.g. `TypeScript, JavaScript, HTML, CSS`)
- Named protocol/domain concept stated explicitly (`REST API`, `GraphQL`)
- Data/cloud, grouped with "any of" phrasing where the posting accepts
  alternatives (e.g. `SQL (Any), Cloud (AWS, GCP, Azure)`)
- Secondary/back-end stack, grouped (e.g. `Back End (Python, C#/.NET, Java)`)
- Named framework(s) (e.g. `React or React Native`)
- VCS/tooling (`Git or GitHub`)
- Process/methodology (`CI/CD, Agile, DevOps`)
- Extra Credit: differentiators the posting calls "nice to have," not
  required (AI tools, scale/impact signals, cross-functional experience)

### 5. Write the checklist file

```markdown
Qualifications for a [Role] @ [Company]

- [ ] [group 1]
- [ ] [group 2]
      ...
- [ ] Extra Credit: [differentiator], [differentiator]
```

Save to `job-checklist/<company-slug>-<role-slug>.md` at the repo root — not
inside `candidates/`. A Job Checklist is a property of the role being hired
for, not of any one candidate applying to it (see
[ADR 0002](../../../docs/adr/0002-job-checklist-outside-candidate-namespace.md)).

### 6. Report

Tell the user the file path you saved (or reused). Call out any requirement
you merged or dropped, with why — e.g. "posting named 'Vue.js' but no
foundational JS requirement; merged into the JavaScript line since that's
the hamburger a recruiter is actually scanning for."
