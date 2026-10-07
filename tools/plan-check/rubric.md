# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `diagnosis-grounded` | The plan's stated cause read against the repro evidence's observed behavior, control runs, and any evidence that rules out alternative causes (e.g., timing matrices, control commands, `--debug` output in the repro-evidence block) | The stated cause is consistent with all evidence the repro pins down; it does not contradict a control run or ignore a result that rules it out | required |
| `scope-bounded` | The plan's in-scope and not-in-scope statements (the scope section or equivalent), plus the list of files or areas named, read against the issue's described behavior | The change is a single bounded fix; it does not bundle unrelated refactors, migrations, or redesigns the issue never asked for; the not-in-scope line exists and excludes at least one adjacent area | required |
| `executable` | The plan's files/areas named, its approach or ordered steps, and any deferred decisions, read as if a stranger were starting from zero | A stranger could start the work from the plan alone: at least one file or code area is named and at least one concrete action is described. Deferring the exact function name within a named file/area is acceptable when the reason is given (e.g., "will be pinned after tracing with debug logs already working"). Fails only when no file, layer, or approach is named, or when a core decision — which file, which layer, which mechanism — is left entirely open with no grounding | required |
| `test-decisive` | The plan's test plan read against the repro evidence's actual steps and observed artifact | The test plan names a specific observable outcome (a value, a behavior, an exit code, a timing, a rendering change) that directly corresponds to the bug the repro showed; "run the tests" or "should feel better" without a named observable is not decisive | required |
| `thread-convention` | The candidate plan comment read against the thread highlights (any explicit maintainer direction) and the repo-facts block (AI-use disclosure policy, contribution policy) | The plan comment does not ignore explicit maintainer direction present in the thread; if the repo's stated policy requires AI-use disclosure, the comment includes it | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Unclear counts as fail: a plan whose evidence cannot be verified from the package is not ready to build from.
