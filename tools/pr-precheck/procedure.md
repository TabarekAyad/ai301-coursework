# Procedure: how this tool grades a PR package

## Read order

1. Read the **repo-facts block** (eval) or the repo's PR template and
   `docs/CONTRIBUTING.md` (live). Note: (a) every required PR template
   section by name, (b) whether the repo's stated policy requires
   AI-use disclosure, and (c) any contribution guide constraints.
   Read this first so you hold the repo's stated asks in mind when you
   later read the description.

2. Read the **plan** (`plan.md` in live mode; the plan-context block in
   eval mode). Note: (a) the in-scope statement and the named files or
   areas, (b) the not-in-scope line and what it excludes, (c) any
   `## Deviations` section and what each deviation records, and (d) the
   test plan's stated observable outcome and the steps it names.
   Read the plan before the diff so you hold the declared boundary in
   mind when you read what actually changed.

3. Read the **diff** (`git diff main...HEAD` output in live mode; the
   Diff section in eval mode). For each changed file, note whether the
   plan named it (in scope, a deviation note, or neither). For each
   hunk, note whether it implements something the plan described or is
   unrelated. Scan for debris: debug prints, dead or commented-out
   blocks, formatting-only hunks, draft commit messages.

4. Read the **PR description** (from `pr_draft.md` in live mode; the
   Description section in eval mode). Note every fidelity claim
   ("implements the plan exactly", "no changes beyond it", etc.) and
   every template section and whether it has real content.

5. Read the **test evidence** (`test_evidence.md` in live mode; the
   Test evidence section in eval mode). Note: (a) which repro scenario
   is re-run and what its before/after shows, (b) whether any output is
   shown or only narrated in a comment, and (c) which of the repo's
   required checks appear and with what result.

6. Read the **issue thread highlights** (fetched live, or from the
   Thread highlights block in eval mode). Note any explicit maintainer
   direction (suggested fix location, confirmed diagnosis, requested
   test type). This informs the `standards-met` check's reading of
   whether the description engages the thread.

Read order matters for `plan-fidelity`: by reading the plan before the
diff, you hold the declared scope in mind when you read what actually
changed, rather than letting the description's framing color your
reading of the diff.

## Evidence gathering

Gather evidence for each check before executing it:

- **plan-fidelity**: From the plan, extract the in-scope boundary, the
  named files, and any deviation notes. From the diff, list every
  changed file and characterize each hunk (in-scope, deviation-covered,
  or neither). From the description, extract every fidelity claim.
  Record the boundary, the file list, and the claims side by side.

- **test-decisive**: From the plan's test plan, extract the named
  observable outcome and the repro scenario it re-runs. From the test
  evidence, extract (a) the before/after runs and what each shows, and
  (b) the repo's required checks and their output or absence. Record
  the plan's test scenario and the evidence's actual runs side by side.

- **diff-reviewable**: From the diff and commit list, list any debris
  found (debug prints, dead/commented-out code, formatting-only hunks,
  unrelated file changes) and any draft commit messages. If none are
  found, record "no debris found."

- **standards-met**: From the repo facts, list the required template
  sections and the AI-disclosure requirement. From the description,
  note what each required section contains (real content, placeholder,
  or blank) and whether a disclosure line is present. Record each
  section's status.

If a section named above is genuinely absent from the package, record
that absence as the evidence for the relevant check and grade it
unclear.

## Check execution

Execute checks in this order: `plan-fidelity`, `test-decisive`,
`diff-reviewable`, `standards-met`.

For each check:
1. Read the gathered evidence for that check.
2. Apply the rubric's pass condition exactly as written — judge the
   artifact itself, not the write-up's length or structure.
3. Assign one grade: **pass**, **fail**, or **unclear**.
   - Use **unclear** only when the evidence genuinely does not exist
     in the package (not when it is thin or short).
   - Use **fail** when the evidence exists and the pass condition is
     not met.
   - Use **pass** when the evidence exists and the pass condition is
     met, including when an honestly disclosed deviation re-ties a
     mismatch that would otherwise fail.
4. Record a one-line evidence note: the specific fact or quote that
   decided the grade.

A check may be graded without re-reading the whole package if its
evidence was already gathered in the evidence-gathering step.

## Verdict assembly

1. Collect the grade for each required check.
2. Apply the verdict rule from `rubric.md`: accept if every required
   check is pass; reject if any required check is fail or unclear.
3. If the verdict is reject, identify the first failing required check
   in execution order (plan-fidelity → test-decisive → diff-reviewable
   → standards-met) as the deciding check.
4. In the output JSON's `evidence` field for the deciding check, quote
   the specific fact or phrase from the package that caused the fail —
   not a description of the problem, but the actual evidence.
5. Emit the JSON block as the last element of the output, with nothing
   after it.
