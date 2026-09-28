# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-liveness | "archived:" field and "last 5 default-branch commits" under Repo facts | repo is not archived AND at least 1 of the 5 most recent default-branch commits was authored by a human (non-`[bot]` username) within 365 days of the capture date | required |
| scope-bounded | issue body and comment thread | issue describes a single, bounded task: not a tracking list or umbrella explicitly calling for separate sub-issues, not a feature with unresolved design debate (no maintainer-accepted direction after 2 or more abandoned PRs or 2 or more years of inconclusive discussion), and not a pure support or usage question; a terse body does not fail this check if the issue carries a good-first-issue label or was opened by a maintainer or collaborator; numbered implementation notes or a list of files within one deliverable do not make an issue an umbrella | required |
| unclaimed | "assignees:" and "linked PRs:" under Repo facts, plus any claim comments in the Comments section | no current assignee AND no open linked PRs; stale claims (claim comment with no maintainer acknowledgment, or claims auto-expired by a bot with no current follow-up and no open linked PR) do not block | required |
| ai-policy | "contribution policy" under Repo facts | no outright ban on AI-generated or AI-assisted contributions; conditions such as disclosure, personal understanding, testing, or human review are not bans and pass; silence passes | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. If a check grade is unclear (evidence genuinely absent), treat it as fail: a first issue you cannot verify is not a first issue to take.
