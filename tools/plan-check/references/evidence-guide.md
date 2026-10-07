# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**In an eval bundle:** The plan's stated cause is in the Candidate plan's Diagnosis or Summary section. The grounding evidence is in the **Repro evidence** block — specifically: the steps and their outputs, any control runs (same command with one variable changed), timing data, `--debug` or profiling output, and any result that rules out an alternative cause.

**In live mode:** The plan's cause is in `plan.md`'s Diagnosis section. The grounding evidence is in the student's posted repro comment on the issue thread (their week-2 proof).

**What good looks like:** The stated cause names a mechanism that the repro evidence actually showed — for example, "after a push, the view's model is not refreshed" is grounded when the repro shows the color stays stale until the view is rebuilt. A cause is not grounded when a control run in the repro rules it out: if the same behavior appears with the proposed cause absent (e.g., the slowness appears with no pager involved, ruling out a pager key-binding fix), that contradiction is the evidence that fails `diagnosis-grounded`.

## Scope

**In an eval bundle:** The in-scope statement and not-in-scope line are in the Candidate plan's Scope section (or the Change section if no Scope heading exists). Named files are in the Files or Changes section.

**In live mode:** Same sections in `plan.md`.

**What good looks like:** A bounded scope names exactly one fix and excludes at least one adjacent area by name (e.g., "Not in scope: the syntax highlighting pipeline"). Scope creep appears when the Changes or Approach section lists work the issue never described: migrations, refactors, new options, UI reworks, or changes to files outside the one-line fix. A not-in-scope line alone is not enough — the Changes section must match it.

## Executability

**In an eval bundle:** Files or code areas are named in the Candidate plan's Files, Changes, or Approach section. The approach or ordered steps are in the Approach or Changes section. Deferred decisions appear as phrases like "whichever is easier," "not sure which layer," "somewhere in the stack," or "TBD."

**In live mode:** Same sections in `plan.md`.

**What good looks like:** A stranger could open the named file and know what to change. The plan names at least one file or function, and describes at least one concrete action (not just an intention). A plan that says "investigate the input stack" without naming a layer, or "fix upstream or vendored, whichever is easier," fails executability because the key decision is deferred.

## Test plan

**In an eval bundle:** The test plan is in the Candidate plan's Test plan section. The repro evidence's actual observable artifact (the output, the color, the timing, the exit code) is in the Repro evidence block.

**In live mode:** The test plan is in `plan.md`'s Test plan section. The repro artifact is in the student's posted repro comment.

**What good looks like:** A decisive test plan names a specific observable outcome that maps onto the repro's actual artifact — for example, "at step 3 the color must flip without leaving the view" maps onto the repro showing the color stays stale. Vague outcomes like "should feel faster," "run the test suite," or "nothing else should feel broken" are not decisive because they do not name what to observe for the fix itself. A good test plan also re-runs the repro's steps (or a direct analog) rather than switching to a different scenario.

## Honesty

**In an eval bundle:** Risks and unknowns are in the Candidate plan's Risks, Unknowns, or Notes section (if present). Mid-build deviations are in a Deviations section (if the plan was updated after the build).

**In live mode:** Same sections in `plan.md`.

**What good looks like:** Stated unknowns name a specific thing the contributor does not yet know and says how they will resolve it (or explicitly defers it). False confidence appears when every decision is stated as settled but the approach section shows underspecified choices. A deviation recorded under `## Deviations` is honest; a deviation that exists only in the diff is not.

## Comms

**In an eval bundle:** The candidate plan comment is in the **Candidate plan comment** section. The thread highlights are in the **Thread highlights** section. The AI-use disclosure policy and contribution guide are in the **Repo facts** block.

**In live mode:** The draft plan comment is in `comment.md`. The live thread is on the issue's GitHub page. The AI policy and contribution guide are in the repo's CONTRIBUTING.md or README (also visible via the evidence guide's repo-facts pointer).

**What good looks like:** A thread-aware comment names or responds to the most specific explicit maintainer direction in the thread (e.g., a suggested file, a confirmed diagnosis, a request for a specific kind of test) rather than ignoring it or posting generic boilerplate. If the repo's stated policy requires disclosing AI-assisted work, the comment includes a disclosure line; absence of a required disclosure is a fail regardless of how good the rest of the comment is.
