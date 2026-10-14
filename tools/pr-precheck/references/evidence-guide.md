# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**In an eval bundle:** The plan's scope boundary (in-scope statement,
not-in-scope line, named files) is in the **Plan context** block. Any
deviation notes are in a `## Deviations` subsection of that block.
The diff is in the **Candidate PR → Diff** section; each `---`/`+++`
header names the changed file. The description's fidelity claims are
in the **Candidate PR → Description** section.

**In live mode:** The plan's boundary is in `plan.md`'s Scope and
Files sections; deviation notes are under `## Deviations` in that
same file. The diff is the output of `git diff main...HEAD` (three
dots) run from the working copy of the fork. The description's
fidelity claims are in `pr_draft.md`.

**What good looks like:** Every file that appears in the diff is named
in the plan's Files or Scope section, or is covered by a deviation
note that records what changed and why. The description does not claim
"implements the plan exactly" or "no changes beyond it" when the diff
contains a hunk in a file the plan never named and no deviation note
covers. Silent drift runs in both directions: a diff that does more
than the plan (an extra file, an extra refactor, a new option) and a
diff that does less without any note (a planned file or feature
missing with no recorded reason) both fail. An honestly disclosed
shortfall — recorded in `## Deviations` in `plan.md` and restated in
the description — passes: the disclosure re-ties the mismatch.

## Test evidence (harness category: not-tested)

**In an eval bundle:** The test evidence is in the **Candidate PR →
Test evidence** section. The plan's test scenario is in the
**Plan context** block's Test plan subsection. The repo's required
checks (unit tests, integration tests, lint, typecheck) are named in
the **Repo facts** block under the PR template or contribution guide
entry.

**In live mode:** The test evidence is in `test_evidence.md`. The
plan's test scenario is in `plan.md`'s Test plan section. The repo's
required checks are named in docs/CONTRIBUTING.md and in the PR
template (`.github/PULL_REQUEST_TEMPLATE.md`).

**What good looks like:** The test evidence re-runs the specific
scenario the plan named — the same input, the same command, the same
observable outcome — and shows a before state and an after state. The
before/after may be pasted terminal output, annotated output (e.g.,
`before: prompt appears; after: toast fires`), or a clear statement of
the before and after values. What fails: evidence that exercises only
the unchanged control path without re-running the broken path; evidence
that narrates "tested locally, it works now" or "verified on my machine"
without stating the specific before and after outcomes. The repo's
required checks must appear in the evidence with their outcome (pass,
fail, or explicitly inapplicable); a clear statement like "cargo test
passes (312 tests); fmt and clippy clean" is sufficient — raw pasted
output is not required. A bare "all checks pass" with no stated outcome
context is not sufficient.

## Diff quality (harness category: unreviewable)

**In an eval bundle:** The unified diff is in the **Candidate PR →
Diff** section. The commit list is in the **Candidate PR → Commits**
section.

**In live mode:** The unified diff is the output of
`git diff main...HEAD`. The commit list is `git log main..HEAD`.

**What good looks like:** The diff contains only changes the plan
asked for (or a deviation note covers). The fix itself is directly
visible: a reviewer can read the changed lines and understand what
the fix does without hunting through unrelated changes. Debris that
makes a diff unreviewable: debug `print`/`eprintln`/`console.log`
statements left in, commented-out code blocks (first attempts, dead
functions, TODO-annotated imports), formatting-only re-indentation
hunks unrelated to the fix, and changes to files the plan never named
that no deviation note covers. Draft commit messages ("wip", "fix",
"temp", "misc cleanups while debugging") signal an unfinished cleanup
pass.

## Standards and comms (harness category: standards-wall)

**In an eval bundle:** The repo's required PR template sections and
AI-use disclosure policy are in the **Repo facts** block (under the
pull-requests and contribution-policy entries). The description's
compliance is in the **Candidate PR → Description** section (the
Checklist and any disclosure line).

**In live mode:** The required template sections are in the repo's
`.github/PULL_REQUEST_TEMPLATE.md`. The AI-use disclosure policy is
in `docs/CONTRIBUTING.md`. Compliance is in `pr_draft.md`.

**What good looks like:** Every section the PR template requires
contains real content written for this PR — not the template's own
placeholder text, not a blank, not a literal "N/A" where the template
asks for substance. A section that is genuinely not applicable (e.g.,
"Screenshots / Demo: N/A — this is a docs-only change") passes when
the reason is given. If the repo's stated policy requires disclosing
AI-assisted work, the description includes a disclosure line; absence
of a required disclosure fails regardless of how good the rest of the
PR is. Explicit maintainer direction in the thread (a suggested file,
a confirmed diagnosis, a requested test type) should be acknowledged
in the description or notes; ignoring it entirely is a signal worth
flagging in the summary.
