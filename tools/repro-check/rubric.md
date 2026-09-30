# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment | repro report body | OS and the version of the tool or library under test are named; relevant dependencies included when the issue is version-sensitive; "my machine" or complete silence fails | required |
| steps | repro report body | every step names a concrete action (a command, file content, or navigation step) that does not require access to the author's private resources; a stranger could start from a clean machine and reach the trigger without guessing what was omitted | required |
| artifact-matches-issue | the artifact(s) in the repro report, read against the issue's description | the artifact shows the same error class, message pattern, or behavior the issue names; OR the report explicitly explains a version or environment deviation and states what it means for the bug; a different failure presented as confirming the reported one fails, even if the two failures share a surface resemblance | required |
| honest-outcome | repro report body and shown artifacts | the primary claim about whether the bug was reproduced is backed by a shown artifact; an evidenced cannot-reproduce (describes what was tried and what came back) counts as pass; a confident reproduction claim backed by zero shown artifacts fails; secondary control-run claims or statements about expected-path behavior do not need their own output shown, as long as the main buggy-path artifact is present | required |
| conventions | repo-facts "contribution policy" line; claim comment body | if the repo's stated policy requires AI-use disclosure, at minimum the claim comment discloses explicitly; a package where neither comment discloses when disclosure is required always fails; claim comment names the specific issue symptom and states intent to investigate or reproduce, never a promised fix or timeline; a boilerplate claim interchangeable with any issue fails; silence on AI policy passes | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. Unclear counts as fail: proof you cannot verify is proof not ready to post.
