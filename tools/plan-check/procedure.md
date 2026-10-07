# Procedure: how this skill grades a plan package

## Read order

1. Read the **repo-facts block** first. Note: (a) whether the repo's stated policy requires AI-use disclosure, and (b) any contribution guide constraints (e.g., limited review bandwidth, required templates).
2. Read the **issue description**. Note the specific expected vs. actual behavior the reporter described.
3. Read the **thread highlights**. Note any explicit maintainer direction — a suggested fix location, a stated cause, a request for a specific kind of test, or a "won't fix as described" signal. Record these verbatim; they are what the `thread-convention` check compares against.
4. Read the **repro evidence** carefully. This is the most important read-order step: record (a) what behavior the steps actually produced, (b) what the control runs or control commands showed, and (c) what any timing data, `--debug` output, or ruling-out evidence demonstrated. The repro evidence is the ground truth that `diagnosis-grounded` checks the plan's cause against.
5. Read the **candidate plan** (diagnosis, scope, files, approach, test plan). Note the stated cause, the in-scope and not-in-scope lines, the named files or areas, and the test plan's stated observable outcome.
6. Read the **candidate plan comment**. Note whether it engages the thread's maintainer direction and whether it includes AI disclosure if required.

Read order matters for `diagnosis-grounded`: by reading the repro evidence before the plan, you hold the actual observed behavior in mind when you read the plan's cause, rather than letting the plan's confident framing color your reading of the evidence.

## Evidence gathering

For each check, gather its evidence before executing the check:

- **diagnosis-grounded**: From the repro-evidence block, extract the key observation that pins down the cause (e.g., a control run that rules out an alternative, a timing result that localizes the cost). From the candidate plan, extract the stated cause in one sentence. Record both side by side.
- **scope-bounded**: From the candidate plan, extract the in-scope statement, the not-in-scope line, and the list of named files or areas. Note whether any item in the "changes" or "approach" section goes beyond the issue's described behavior.
- **executable**: From the candidate plan, extract whether at least one file or code area is named and whether at least one concrete action (not just an intention) is described. Note any decision the plan leaves to the build (e.g., "whichever is easier," "somewhere in the stack," "not sure which layer").
- **test-decisive**: From the plan's test plan, extract the stated observable outcome. From the repro evidence, extract the actual artifact the steps produced. Record both so the check can compare them.
- **thread-convention**: From the thread highlights, extract any explicit maintainer direction. From the repo-facts block, extract the AI-use disclosure requirement (or its absence). From the candidate plan comment, extract whether it addresses the maintainer direction and whether it includes a disclosure line.

If a section named above is genuinely absent from the package, record that absence as the evidence for the relevant check and grade it unclear.

## Check execution

Execute checks in this order: `diagnosis-grounded`, `scope-bounded`, `executable`, `test-decisive`, `thread-convention`.

For each check:
1. Read the gathered evidence for that check.
2. Apply the rubric's pass condition exactly as written — judge the thing itself, not the write-up's length or structure.
3. Assign one grade: **pass**, **fail**, or **unclear**.
   - Use **unclear** only when the evidence genuinely does not exist in the package (not when it is thin or short — a terse plan can still pass).
   - Use **fail** when the evidence exists and the pass condition is not met.
4. Record a one-line evidence note: the specific fact or quote that decided the grade.

A check may be graded without re-reading the whole package if its evidence was already gathered in the evidence-gathering step.

## Verdict assembly

1. Collect the grade for each required check.
2. Apply the verdict rule from `rubric.md`: accept if every required check is pass; reject if any required check is fail or unclear.
3. If the verdict is reject, identify the first failing required check (in execution order) as the deciding check.
4. In the output JSON's `evidence` field for the deciding check, quote the specific fact or phrase from the package that caused the fail — not a description of the problem, but the actual evidence.
5. Emit the JSON block as the last element of the output.
