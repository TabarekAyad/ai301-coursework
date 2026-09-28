# Unit 1 Selection

## Chosen issue

**Link:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47

**Skill verdict:** accept

**Fit reason:** Single bounded documentation task — adding curl examples to `docs/API.md` for all 9 endpoints. Clear acceptance criteria, 1 file, 2–3 h estimate, no design debate. Matches fit profile: strong technical writing background, prefers clear scope over vague feature requests.

---

## Skill run in live mode (check grades)

The skill graded 3 candidate issues from `codepath/pathreview-ai301-fa26-s3`:

| Issue | repo-liveness | scope-bounded | unclaimed | ai-policy | Verdict |
|---|---|---|---|---|---|
| #37 | pass | pass | pass | pass | accept |
| #47 | pass | pass | pass | pass | accept |
| #63 | pass | pass | pass | pass | accept |

All three passed. Issue #47 was chosen as the best personal fit: pure documentation with very clear acceptance criteria, matching my technical writing experience.

---

## Reflection prompts

**What evidence mattered most for your chosen issue?**

The unclaimed check mattered most for narrowing candidates. Many issues in the repo had open PRs already (e.g. #68, #54, #60 were all rejected because open PRs explicitly said "Fixes #68/54/60"). Issue #47 had no open linked PRs, only classmate claim comments which the Path Review house rule says do not block. Once unclaimed passed, the scope-bounded check confirmed it was a single deliverable with clear scope.

**What would make you change your mind about this issue?**

If an open PR targeting issue #47 appeared before posting my claim comment in Unit 2. The issue has classmate interest, so someone could open a PR at any time. I would also reconsider if a maintainer commented that the scope had grown or that the task was already covered elsewhere in the docs.

---

## Write-up fields

### Run history

The rubric was run twice against all 20 scored eval issues.

**Run 1:** 17/20 — below the bar. Three `clear-accept` issues (issue-01, issue-04, issue-19) were incorrectly rejected on `scope-bounded`. The check was too strict: it flagged terse issue bodies and numbered implementation notes as insufficiently scoped.

**Run 2 (saved as eval-run.txt):** 18/20 — PASS. The `scope-bounded` pass condition was revised to clarify that a terse body does not fail if the issue carries a `good first issue` label or was opened by a maintainer/collaborator, and that numbered implementation notes within one deliverable do not make an issue an umbrella. The three previous misses were fixed, though two new misses appeared (issue-15 and issue-20 were graded accept instead of reject).

### Issue analysis

**Scored issue analyzed: issue-15** (zulip/zulip#19589, category: `scope`)

- **Gold label:** reject
- **My rubric's verdict:** accept (failed to reject)
- **Why the rubric read it that way:** Issue-15 has a `good first issue` label, no assignee, and no open linked PRs — so all four checks passed under the updated rubric. The scope-bounded check failed to catch the real problem: 97 comments spanning 2021–2024, two abandoned closed PRs, and a thread showing years of unresolved design debate about how to separate the `command` and `text` fields in a Slack-compatible webhook. The rubric's scope-bounded condition requires "2 or more abandoned PRs OR 2 or more years of inconclusive discussion" — issue-15 has both — but the updated wording added a clause that a good-first-issue label or maintainer/collaborator opener offsets a terse body, and the skill appears to have over-applied this positive signal, letting the label offset the design-debate signal instead of reading them independently. The label is a necessary-but-not-sufficient condition; it does not cancel a 97-comment unresolved debate.

### Check rationale

**Check quoted from the uploaded rubric.md:**

> `scope-bounded` | issue body and comment thread | issue describes a single, bounded task: not a tracking list or umbrella explicitly calling for separate sub-issues, not a feature with unresolved design debate (no maintainer-accepted direction after 2 or more abandoned PRs or 2 or more years of inconclusive discussion), and not a pure support or usage question; a terse body does not fail this check if the issue carries a good-first-issue label or was opened by a maintainer or collaborator; numbered implementation notes or a list of files within one deliverable do not make an issue an umbrella | required

**Why this check is designed this way:** The most common way a first contribution fails is that the issue turns out to be larger or less defined than it looked. This check guards against the three failure modes seen in the eval set: tracking lists (issue-05, issue-10), years of design debate with no settled spec (issue-15, issue-20), and support questions. The extra clauses about terse bodies and numbered notes were added after Run 1 to stop the check from rejecting valid issues that were brief or had inline implementation suggestions.

### Trade-offs

The main trade-off is in the `scope-bounded` check. Making it strict enough to reject umbrella issues and long-debated features also causes it to reject terse but valid issues (like issue-04's one-liner with a `good first issue` label). Loosening it to pass those terse issues caused it to pass issue-15 and issue-20, which have years of unresolved debate behind a friendly label. The rubric currently has no way to apply positive signals (label, opener role) and negative signals (debate length, abandoned PRs) independently — a good-first-issue label partially overrides the debate signal instead of sitting beside it. Fixing this would require separating the conditions more explicitly, at the cost of a more complex check that is harder to apply consistently.
