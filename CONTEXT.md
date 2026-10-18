# Job Search

Helps one or more candidates get their resumes past recruiter screening:
their resumes, the job postings they target, and recruiter-style feedback on
how well a resume fits a posting. For folder layout and commands, see
`README.md` and `docs/`.

## Language

**Candidate**:
A person whose job search this repo supports. Each candidate's material is
kept apart from every other candidate's, and stays private to the machine
it's on.
_Avoid_: User, applicant.

**Resume**:
A candidate's résumé, kept as numbered versions over time. A new version is
added rather than an old one changed.
_Avoid_: CV.

**Base Resume**:
The candidate's newest Resume version. It's the candidate's own document
and the starting point for every Tailored Resume.
_Avoid_: Master resume, main resume.

**Tailored Resume**:
A copy of a Base Resume aimed at exactly one Job Posting. It records which
Base Resume version it came from. When a bullet changes in the Base Resume,
the same bullet changes in its Tailored Resumes.
_Avoid_: Curated resume (the folder name, not the concept), custom resume.

**Work History**:
Everything a Candidate has done, in their own words. It's the fact source
for building a Resume. Older Resume versions are leads to confirm, not facts.
_Avoid_: Career story, background.

**LinkedIn Profile**:
Versioned drafts of a Candidate's LinkedIn sections. They're written in
first person with fewer tool names than a Resume, and they may be quoted
as source material.

**Job Posting**:
A job listing copied verbatim from the web, grouped by role family. Never
paraphrased.
_Avoid_: Job listing, job (ambiguous with Application).

**Job Checklist**:
A short, recruiter-style list of what an ideal candidate for one Job Posting
has, derived from that posting alone, with no Resume or Candidate involved.
Each posting has one checklist, shared by every Candidate who targets it.
_Avoid_: Qualification, skills list.

**Qualification**:
One resume bullet's proof of a skill: keywords + how they were used + why
(the outcome). A smaller unit than a Job Checklist item.
_Avoid_: Requirement (that's what a Job Checklist item states).

**Recruiter**:
An agent that screens or drafts the way an experienced technical recruiter
would. Several Recruiters can work on one request at the same time, each
with its own Focus or its own Job Posting.

**Focus**:
The part of a screen one Recruiter goes deep on: First pass (the 20-second
scan), Bullets (the Qualification in each bullet), or Credibility (whether
every claim can be backed up).

**Screen Report**:
A dated written verdict on how one Resume holds up against one Job Posting
(or against the role in general), with a short ranked fix list. When several
Recruiters screen together, their findings are merged into one Screen
Report.
_Avoid_: Feedback (it also means comments from a real recruiter or
interviewer, which aren't modeled), review.

**Application** *(not yet modeled)*:
The lifecycle of pursuing one Job Posting (interested, applied,
interviewing, offer, rejected) with dates. If introduced, record the design
decision as a new ADR in `docs/adr/`.

## Relationships

- A **Candidate** has many **Resume** versions. The newest is the **Base
  Resume**.
- A **Tailored Resume** comes from one **Base Resume** version and targets
  one **Job Posting**.
- A **Job Posting** has one **Job Checklist**, shared across **Candidates**.
- A **Screen Report** judges one **Resume** against one **Job Posting**.
  It's written by one or more **Recruiters**.
- A **Resume** is built from the **Candidate**'s **Work History**, earlier
  **Resume** versions, and **LinkedIn Profile**. Only the Candidate's own
  statements count as facts.
