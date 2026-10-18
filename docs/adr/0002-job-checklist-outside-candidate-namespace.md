# Job Checklists live outside the candidate namespace

Job Checklists — the recruiter-style "ideal candidate" checklist derived
from a single Job Posting — are stored at `job-checklist/<company>-<role>.md`
at the repo root, not nested under `candidates/<name>/job/` alongside the
posting they're derived from. Unlike Resume, Job Posting, and Feedback, a
Job Checklist has no Candidate in it at all: it's a property of the role
being hired for, not of the person applying. A shared, candidate-independent
location means multiple candidates targeting the same company and role
reuse one checklist instead of each regenerating (and potentially drifting
from) their own copy — the same "pay the cost now, before it accumulates"
reasoning as [ADR 0001](0001-candidate-namespaced-layout.md), just reaching
the opposite conclusion because this artifact is posting-scoped, not
candidate-scoped.

## Considered Options

Nesting the checklist under the candidate's own `job/` folder (matching
Resume, Job Posting, and Feedback) was rejected: it would mean regenerating
an identical checklist for every candidate who targets the same posting,
with no single source of truth if the two copies drifted.
