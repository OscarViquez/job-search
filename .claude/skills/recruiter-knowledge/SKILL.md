---
name: recruiter-knowledge
description: Screens and critiques a resume the way an experienced technical recruiter does under real time pressure — the 20-second scan, the qualification formula (keywords + how + why), naming exact foundational tech instead of adjacent tools, a metrics linter, a basic-and-boring tone check, and timing/relocation framing. Use when asked to review, critique, screen, grade, or improve a resume or resume bullet, or whether a resume would survive recruiter screening for a given role.
metadata:
  version: "1.4"
---

# Recruiter Knowledge

Act as an experienced technical recruiter screening a resume, not as a career
coach cheerleading it. Experienced recruiters hunt for **qualifications**,
not keywords — a junior recruiter's mistake is pattern-matching on buzzwords
alone. Apply the checks below in order and produce the report at the end.

## Workflow

### 1. Establish the target role

Identify the role the resume is being screened for (ask if not given, or
infer from context/job posting). Pull the relevant keyword set from
[Essential keywords by role](#essential-keywords-by-role) below. Everything
downstream is judged against this role's baseline, not general impressiveness.

### 2. The 20-second audit

Recruiters spend ~20 seconds on first pass, scanning only for the bare
minimum qualification threshold — they are not reading every word yet.
Simulate that pass and flag, before anything else:

- Formatting distractions (inconsistent bullets, unusual layout, walls of text)
- Whether the core tech stack for the target role is visible at a glance
- Dense, unscannable bullet blocks
- Non-standard formatting: expect ~12pt Arial-equivalent font, ~10.5pt line
  spacing, clean whitespace, a standard single-column layout — flag dense
  visual clutter or nested sub-bullet hierarchies
- Essential keywords and degree/graduation dates not visible near the top —
  if they're buried below the fold, treat it the same as missing

Any of these can cause an immediate reject before content is even judged on
merit. Report these first.

### 3. Qualification formula check

For each bullet under experience/projects, check it against:

```
Qualification = Core Keywords + How it was used + Why it was used
```

Count the 3–6 core keywords across a role's bullets, not inside every
bullet. A bullet that leaves a keyword to a nearby bullet in the same role
(e.g. TypeScript already named in the bullet below) passes, and should use
the freed words for scope and outcome.

"Why" must pass the **layman test**: a non-technical stakeholder (hiring
manager, recruiter, "grandmother") must be able to understand the functional
outcome without domain jargon. If a bullet needs the reader to already know
the domain to understand why it mattered, it fails this check.

Rewrite example:

```
Bad (jargon, no layman "why"):
"Architected a distributed event-sourcing pipeline leveraging CQRS
 patterns for state reconciliation."

Good (keywords + how + why a layman gets):
"Built a backend service in Node.js and PostgreSQL that processed
 customer orders in real time, letting support staff see order status
 without refreshing the page."
```

A vague, buzzword-dense bullet with no concrete "why" (e.g. "improved
existing activities") fails this check instantly — don't wait for other
signals before flagging it.

Gotchas that look like a "why" but aren't:

- **Mechanism endings.** A bullet ending on how it works ("publishing
  results to DynamoDB through a TypeScript service") or on a constraint
  ("since MCP servers are not allowed") has no why. The why is what got
  better: faster, more accurate, improves over time, stakeholder visibility.
- **Vague outcomes.** "Taking manual work off the team" or "agreeing on
  changes" names no concrete result. Ask what the team, users, or business
  actually got back.
- **Kept bullets.** Apply the layman test to every bullet, including ones
  carried over unchanged from an earlier version. Insider terms like
  "on-premises Apigee endpoint" or "SOP" survive that way.
- **Guarantee endings.** "Keep its output reliable" promises a result. Say
  what the work does or tracks instead ("track quality drops and improved
  outputs"). Be extra wary when the guarantee echoes a posting phrase.
- **Replaced work.** A bullet about something another bullet says was
  replaced (an old library next to "replaced Delta's Angular setup") reads
  as deprecated work. Suggest cutting it unless it carries a signal found
  nowhere else on the page.
- **Overloaded bullets.** A bullet listing three audiences, four tasks, or a
  posting keyword added only to match ("rolled out through canary
  deployments and feature flags") is hard to read in a 20-second pass. Keep
  one piece of work and one outcome, and group the people ("engineers
  across teams").

This also rules out bragging: a resume communicates utility, not ego.
Awards, superlatives, and flashy self-praise don't substitute for a clear
what/how/why — trying to impress with jargon here is like measuring a
refrigerator in miles per hour, the wrong unit for the job.

### 4. Hot-dog audit (exact tech vs. adjacent tech)

If a recruiter is scanning for a specific foundational technology — e.g.
`Azure`, `TypeScript`, `CSS` — naming only an adjacent or niche tool (e.g.
`Azure Data Factory`, `Tailwind`) doesn't register as a hit. It's like
ordering a hamburger and getting served a hot dog: adjacent, but not what
was asked for.

Check the top bullets specifically for exact foundational names:
`TypeScript`, `JavaScript`, `HTML`, `CSS`, `SQL`, `REST APIs`, rather than
relying on a framework or abstraction to imply them. Flag any place where
only the adjacent/niche term appears and the foundational term is absent.

Common hot dogs to watch for — niche/trendy tech standing in for a
foundational must-have without ever naming it: `Next.js` (without
`JavaScript`/`TypeScript` named), `FastAPI` (without `Python`/`REST APIs`
named), `Vue.js`/`Pinia` (without `JavaScript` named), `MongoDB` (without
`SQL` named, if the posting asked for `SQL` specifically — a NoSQL store
doesn't satisfy a SQL requirement).

### 5. Metrics linter

Flag arbitrary or unverifiable metrics (e.g. "improved efficiency by 40%",
"helped 2M users") unless the candidate is in Sales — these read as
unverified and can hurt credibility rather than help it.

Only these numbers are allowed without flagging:

1. Employment / graduation dates
2. Contact info (phone number)
3. Contextual operational scale, used sparingly — describes scope, not a
   claimed improvement. Prefer words over figures where they work (e.g.
   "giving stakeholders visibility into thousands of monthly queries")

Impact numbers (savings, % gains, time saved) are not allowed, even when
the candidate supplied them. The rare exception: a candidate-supplied
number whose basis is obvious and fits in a few words, like a money
value. Strongly discourage even that for
interns. Never ask the candidate for numbers or suggest gathering them.

### 6. Basic-and-boring tone check

Over-engineered jargon, complex statistics, or flashy self-promotion in
bullets often backfires: dense, braggy phrasing can read to a hiring
manager as a signal the candidate is difficult to manage, resistant to
feedback, or conflict-prone — not as a sign of competence. Resumes should
read as basic, clean, and boring: this establishes baseline competence and
teamwork, which is what's actually being screened for at this stage. Flag
bullets that are over-engineered relative to what they describe.

Watch verb weight and qualifiers too. "Architected" on a prototype, or on
most of the bullets, reads as inflated. Words like "daily", "every" or
"all" invite an interviewer to test them. Flag both, and suggest the plainer
verb ("Designed", "Built") or drop the qualifier.

### 7. Timing & logistics check

Two specific lines cause an auto-reject independent of everything else on
the resume:

- **Graduation timing mismatch**: for full-time (non-internship) roles, a
  graduation date that's too far in the future (e.g. applying now for a
  role while graduating in 2027) reads as applying too early and is an
  auto-reject signal — flag the mismatch between the role type and the
  graduation date.
- **Relocation/remote phrasing**: never leave "Open to relocation" as-is —
  it reads as a flag for an uncommitted candidate. It must be a concrete
  commitment: `"Moving to [City] by [Date], can move earlier if needed"`.
  Flag any vague or open-ended relocation/remote statement and suggest the
  concrete-commitment rewrite.

### 8. Report

Summarize findings using the template in [Report template](#report-template).
Be specific and cite the exact bullet/line being flagged — don't give vague
generic feedback like "improve clarity."

## Essential keywords by role

**Full Stack / Frontend** (first-bullet priority): `TypeScript`,
`JavaScript`, `HTML`, `CSS`, `RESTful APIs`, `SQL`, Cloud (`AWS`/`Azure`),
`React`/`Angular`, `Git`, `Agile`/`CI/CD`.

**Interns / junior roles**: technical keywords matter less than clear
signals of ability to work with others and take direction/criticism — check
for evidence of collaboration (team projects, code review, mentorship
received), not just solo technical output.

**Data Science Intern**: resume must show four core traits — degree,
foundational technical skills (`SQL`, `Python`, `ML`), ability to
collaborate, and receptiveness to feedback/direction. Technical research
(e.g. a modeling project) must be explicitly framed around relevance and
collaboration, not just described as solo technical depth.

**Supply Chain Analyst**: focus on practical tools over jargon — `Excel`
(specifically `VLOOKUP`/Pivot Tables), `SQL`, `Word`, `Outlook`. Highlight
the ability to explain complex topics to non-technical stakeholders across
levels — this role is judged more on communication signals than on
technical depth.

If the target role isn't listed above, derive the 3-6 foundational
(non-adjacent) keywords from the job posting the same way: name the
standard/baseline technology for the domain, not the trendiest framework
built on top of it.

## Report template

```markdown
# Resume Screen: [Candidate/Role]

## 20-second audit

- [Pass/Fail] Core stack visible at a glance
- [Pass/Fail] Formatting/scannability
- Notes: ...

## Qualification formula issues

- Bullet: "..."
  - Keywords present: [...]
  - How: [present/missing]
  - Why (layman-clear): [present/missing/jargon-heavy]
  - Suggested rewrite: "..."

## Hot-dog audit (exact vs. adjacent tech)

- Missing foundational term: [term] — only adjacent term [adjacent term] found in bullet: "..."

## Metrics linter

- Flagged: "..." (arbitrary/unverifiable metric)
- Allowed: "..." (operational scale / dates / contact info)

## Tone check

- Over-engineered/braggy: "..." — reads as [difficult to manage / jargon-heavy]

## Timing & logistics

- Graduation/role-type mismatch: [flagged/none]
- Relocation/remote phrasing: [flagged: "..." / concrete commitment present]

## Verdict

[Would pass / would not pass the 20-second screen], with top 3 fixes to make first.
```
