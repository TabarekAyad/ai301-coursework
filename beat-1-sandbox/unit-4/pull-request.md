# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/<!-- replace with actual PR number after opening -->

**Branch**

`docs/47-api-curl-examples`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1: 14/20 scored items (bar: 18/20: FAIL)  
Run 2: 20/20 scored items (bar: 18/20: PASS)

**Package analysis**

`pkg-04` — category: `not-tested`, gold: reject, my verdict: reject, agree: yes.

The plan's test plan named a specific scenario: re-run `bat Cargo.toml` under pwsh
with less-Windows, expect paged output WITH colors, matching the `--pager="less -R"`
control. The test evidence reads: "Tested locally on Windows 11 with less-Windows:
colors work now in paging mode. `cargo test` passes."

The rubric's `test-decisive` pass condition requires that the evidence "re-runs the
specific scenario the plan's test plan named … and shows a before state and an after
state." This evidence only narrates the outcome ("colors work now") — it names no
before value (e.g., the raw `ESC[38;5;...m` sequences the repro documented) and no
explicit after value from the plan's named command. "cargo test passes" is a bare
statement with no outcome context. Both failures together satisfy the `not-tested`
reject pattern: the evidence narrates the result rather than re-running the plan's own
scenario with named before and after values.

**Check rationale**

Quoted from `rubric.md` as it reads now:

> `test-decisive` | The test-evidence section's before/after runs read against the
> plan's test plan and the repro's original steps; the repo's own required checks
> (unit tests, lint, typecheck, integration) read against the test evidence | The test
> evidence re-runs the specific scenario the plan's test plan named — not just a
> control or an adjacent path that was already working — and shows a before state and
> an after state by any means: pasted terminal output, annotated output, or a clear
> statement of the before and after values. Evidence that only narrates "the fix
> works", "tested locally", or "verified on my machine" without naming specific before
> and after outcomes for the plan's own scenario fails. The repo's required checks
> appear in the evidence with their outcome (passing, failing, or explicitly
> inapplicable); raw pasted output from checks is not required, but a bare "checks
> pass" with no stated outcome context fails | required

The phrase "by any means: pasted terminal output, annotated output, or a clear
statement of the before and after values" was added in a revision. The first draft
required raw pasted terminal output and bare check names to match — it failed all
clear-accept packages whose evidence used annotated or comment-style before/after
blocks (e.g., `-- before: { code = 1 }` inside a fenced block) and packages that
stated check outcomes like "cargo test passes (312 tests)" without raw output. That
made the check too strict for format rather than for substance, knocking the first
eval run to 14/20. The revision clarified that the check grades whether the evidence
shows the scenario, not whether it shows raw terminal text, and added that stated
outcomes with context (not bare "checks pass") satisfy the repo-checks sub-condition.
The phrase "not just a control or an adjacent path that was already working" was
retained from the original to guard against evidence that only re-tests the working
path and skips the broken one.

**Trade-offs**

The "by any means" relaxation accepts annotated and described before/after values in
addition to pasted terminal output. The risk is that a sufficiently fluent narrative
could pass `test-decisive` without the author ever having run the repro — the check
cannot distinguish "I ran it and wrote it up clearly" from "I wrote up what I expected
would happen." To verify the relaxation did not change correct-reject outcomes, I
re-ran the four `not-tested` packages (pkg-04, pkg-07, pkg-10, pkg-14) individually
using `--only` before the confirming full run. All four still rejected: the
distinguishing factor is whether named before-and-after values appear at all, which
remains the check. Packages that only narrate ("colors work now", "verified locally")
still fail regardless of how fluently they narrate.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
