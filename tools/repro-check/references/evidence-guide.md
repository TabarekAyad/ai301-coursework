# Evidence guide: where proof lives in a reproduction package

A package has three parts read in order: the issue context (what was reported), the claim comment (intent to reproduce), and the repro report (the attempt itself). This guide maps each rubric check to the exact place in the package where its evidence lives.

## Environment

**Where it lives:** repro report body, near the top or in a dedicated "Environment" section.

In an eval bundle: look in the repro report section for OS, runtime or language version, and any dependency versions. Compare against the issue's stated target.

In live mode: the draft file or comment the student submitted; the issue body for what version the reporter used.

**What good looks like:** OS and the version of the tool under test are named, plus any packages the issue could be sensitive to. `yq v4.53.3, macOS 15.5, Homebrew` passes. A version range like "recent pandas" or a comment like "tested on my laptop" does not. When the student's version differs from the reporter's, the deviation is called out and its meaning for the bug is explained. When the issue is clearly not version-sensitive, a single runtime version is enough.

**What fails:** no environment section at all; OS named but tool version missing; version listed but it is a known-different one with no acknowledgment.

## Steps

**Where it lives:** repro report body, in the section describing what the student actually did.

In an eval bundle: the repro report section following the environment record.

In live mode: the draft comment.

**What good looks like:** each step is an atomic action a stranger could take on their own machine — a command to run, a file to create with its contents, or a navigation step in a UI. A minimal self-contained script or runnable code snippet counts as steps. The starting state is implied or stated. A stranger reading the steps should reach the trigger without contacting the author.

**What fails:** steps that reference files in a private or unshared repository; steps described as goals ("install the dependencies") with no specifics; steps that require the author's local environment or credentials; steps whose outputs the author does not show.

## Behavior shown

**Where it lives:** repro report body, after the steps — the artifact section, output block, or screenshot description.

In an eval bundle: the output excerpts, tracebacks, logs, or other artifacts in the repro report. Read them against the issue's "actual behavior" description.

In live mode: the artifacts in the student's draft, compared to what the issue describes.

**What good looks like:** the artifact shows the same error class, message text, or behavioral outcome the issue names. A panic in the issue → a panic in the artifact (not a graceful error). A wrong-output bug → the wrong output shown side-by-side with the expected output. An honest cannot-reproduce with real artifacts showing what happened instead also counts as good.

**What fails:** an artifact that shows a different failure (a syntax error where a panic was expected, a graceful validation error where a crash was reported) presented as confirming the issue; an artifact that shows the tool ran but does not show the buggy behavior; no artifact at all; only the issue's own description quoted back without new evidence.

## Honesty

**Where it lives:** repro report body, especially the analysis or summary section; cross-check against the artifacts shown.

In an eval bundle: read the report's conclusions against its artifacts. Does what is claimed match what is shown?

In live mode: check that the draft's narrative matches its own evidence.

**What good looks like:** claims are proportional to evidence. "I ran the command and got this error" is backed by the error block shown. "I could not reproduce this" is backed by a description of what was tried and what came back instead. An honest cannot-reproduce with a real attempt and an explanation of the environmental difference is a pass.

**What fails:** "guaranteed reproducible" backed by nothing; a root-cause analysis with no supporting artifact; expected behavior stated backwards from what the artifacts actually show; a confident diagnosis that the artifacts contradict; "I verified this" with no verification shown.

## Comms

**Where it lives:** the claim comment body; the repo-facts "contribution policy" line for disclosure requirements.

In an eval bundle: the claim comment section, and the contribution policy line in repo facts.

In live mode: the student's draft claim comment, and CONTRIBUTING.md / any AI policy file for the live repo.

**What good looks like:** the claim comment names something specific about this issue — a symptom, the file named in the bug report, the version range — so it could not be posted unchanged on a different issue. Intent is stated as the next step only: reproduce, investigate, report. If the repo's policy requires AI-use disclosure, both the claim comment and the repro comment contain an explicit disclosure line.

**What fails:** a boilerplate claim interchangeable with any issue ("I'll take this", "assign me"); a claim that promises a fix, a PR, or a timeline before any work is done; a "same as above, can confirm" repro with no independent evidence; AI policy requires disclosure and neither comment discloses.
