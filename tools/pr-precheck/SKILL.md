---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request — its title,
description, diff, commits, and test evidence — read against the plan
it claims to implement and the issue that plan belongs to. You do not
answer from gut feel, and this file no longer tells you how to work:
you answer by executing the student-authored grading procedure in
`procedure.md`, which applies the rubric in `rubric.md` to evidence
gathered per `references/evidence-guide.md`.

## The question

Grade one PR package to decide: is this pull request ready to submit?
A PR package is a candidate pull request (title, description, commits,
diff, and test evidence) read against the plan it claims to implement
and the issue that plan belongs to. The tool never answers a different
question, never grades more than one package per run, and never answers
from gut feel: it executes the components in this directory.

## Inputs and modes

One of:

- **Live mode**: the student's own submission, checked before it goes
  out. Inputs are all of the following, read together:
  - `plan.md` (including any `## Deviations` section recording what
    changed from the original plan and why)
  - The diff on the student's branch: every committed change relative
    to the repo's default branch, produced by `git diff main...HEAD`
    (three dots) run from the working copy of the fork
  - The draft PR title and description from `pr_draft.md`
  - The test evidence from `test_evidence.md`
  - The issue's live thread (fetched via `gh`, the GitHub API, or the
    web) for maintainer direction and context
  - The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
    `docs/CONTRIBUTING.md` for the repo's stated asks and AI-use
    disclosure policy

  A house-chain student reads the house plan and house repro pack
  wherever these steps say `plan.md` and repro evidence. The same
  checks grade the same things. Live mode gathers issue-side evidence
  from the real repo.

- **Eval mode**: a package bundle (a markdown file containing the
  issue context, a repo-facts block, a plan-context block, and the
  candidate PR with its diff and test evidence). Use ONLY the bundle
  text as evidence. Do not fetch anything; the bundle is the whole
  world. Eval mode always grades a complete package: every check,
  full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the repo
the student's PR must target and the house rules that apply there.
Refuse to grade a PR that targets any repo other than the one named in
`scope.md`. If the `Repo:` line still carries an unfilled placeholder
(angle-bracketed text), stop without grading and tell the student to
replace it with their section's Path Review repo; never guess a scope.
In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream, with wrong/right examples. Check the PR title
and description from `pr_draft.md` against those rules and report any
rule the draft breaks in the readable summary, quoting the rule and the
offending text. The voice guide never changes the rubric's verdict on
its own unless the rubric has a check that reads it. In eval mode,
ignore `voice-guide.md` entirely: voice is personal and carries no
gold labels; universal communication-quality checks live in the rubric.

## Component reads

Read `rubric.md`: it defines the checks table (Check, Evidence, Pass
condition, Weight) and the verdict rule that combines check grades into
a final accept or reject. Read `references/evidence-guide.md`: it maps
where each evidence family lives in the package (eval or live), so
every rubric check knows where to look. Then execute `procedure.md`
exactly as written: the procedure decides the read order, how evidence
is gathered for each check, how each check is graded, and how grades
become the verdict.

Follow `procedure.md` as written, the same way an executor follows a
script: exactly, without improvising around gaps. Where the procedure
is silent on a step, note the gap in the readable summary rather than
silently inventing a step; a procedure gap is feedback the student
needs.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop and say so: this tool cannot grade without both a
rubric and a procedure, and that is by design.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict, no "accept with reservations", no
score. Reservations belong in check evidence lines, not in the verdict.

Emit a fenced JSON block as the last element of your reply, and nothing
after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Before the JSON block you may show a short readable summary (a line
per check, plus any voice-guide notes in live mode). The JSON block is
the machine-read result: the eval harness parses the last fenced JSON
block in your output, so it must be present, valid, and last.

## Grading discipline

- **Evidence first**: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish**: a terse complete PR can be
  ready and a beautiful confident one can be hiding drift. Every check
  reads the artifact itself against the plan, the issue, and the
  stated standards, never the formatting or length.
- **The rubric decides, not the run**: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not the run**: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Unclear defaults to fail**: treat `unclear` as the rubric's verdict
  rule directs. Where the rule is silent, an unverifiable claim fails:
  a PR you cannot verify from the package is not ready to submit.
