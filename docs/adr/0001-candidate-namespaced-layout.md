# Candidate-namespaced layout for resumes, jobs, and feedback

Resumes and job postings previously lived in separate root-level folders
(`resumes/`, `jobs/`) with no notion of who they belonged to. We moved them
under `candidates/<candidate-slug>/{resume,job,feedback}/` instead, because
this repo is expected to eventually track more than one person's job search
(e.g. helping a friend), and retrofitting a per-candidate namespace after
many job postings and feedback reports accumulate would mean rewriting every
path reference in the `recruiter` agent and its skills. Paying that cost now,
while there's only one candidate to move, is cheap; paying it later is not.

We considered keeping type-first root folders (`resumes/`, `jobs/`,
`feedback/`) with a candidate field inside each file instead of in the path.
Rejected: the `recruiter` agent's workflow already depends on
candidate-scoped paths (e.g. checking `feedback/` for prior reports on this
candidate before reviewing a new resume version), so path-based scoping
matches how the data is actually read, not just how it's stored.

We deliberately did not add a `linkedin/` folder or any application-status
tracking alongside this move — those are unused today; see
[CONTEXT.md](../../CONTEXT.md) for the Application concept this would
eventually formalize.
