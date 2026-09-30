# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

---

## Your identity upstream

**GitHub username**

TabarekAyad

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47#issuecomment-5903828337

Looking into this as my first contribution. `docs/API.md` documents nine endpoints but none of them include an example request — no curl command, no sample body, no indication of what headers are required. Without that, a developer who just finished setup has no quick way to verify the API is actually responding before they start writing code against it.

Here's what I'll do: follow `docs/SETUP.md` to get the stack running locally, then check the doc on the current `main` commit to confirm there are no existing examples, and call each of the nine endpoints with curl to capture what a real working request and response looks like. I'll include my OS, relevant version numbers, and the commit I tested when I post the repro report here.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47#issuecomment-5904076109

Reproduced. Here's what I found setting up and calling each endpoint.

**Environment:** Windows 11 Home (build 26200, x86-64), Python 3.11.9 venv, curl 8.21.0, Docker Desktop 27.1.1, repo commit `2f4e82f` on `main` (fork of codepath/pathreview-ai301-fa26-s3).

**Setup:** Followed `docs/SETUP.md` — copied `.env.example` to `.env`, brought up Docker services with `docker compose up -d`, created a venv, ran `pip install -e ".[dev]"`, applied migrations with `alembic upgrade head` (migrations 001 and 002), and seeded the database. Started the API with `uvicorn api.main:app --host 127.0.0.1 --port 8000`. Skipped the frontend `npm install` since this issue is API-only.

**The doc gap, confirmed on commit `2f4e82f`:**

```
$ git grep -c "curl" docs/API.md
(no output — exit 1, zero matches)
$ grep -c "^\`" docs/API.md
9
```

`docs/API.md` lists nine endpoints with no example invocations, no code blocks, and no curl commands anywhere in the file.

**All nine endpoints, as they answer today** (JWTs shortened, list response trimmed to first item):

```
$ curl http://localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},...}}
[HTTP 503]

$ curl -X POST http://localhost:8000/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"repro47@example.com","password":"repro47-testpass"}'
{"access_token":"<token>","token_type":"bearer"}
[HTTP 200]

$ curl -X POST http://localhost:8000/auth/login \
  -d "username=user1@example.com&password=password1"
{"access_token":"<token>","token_type":"bearer"}
[HTTP 200]

$ curl -X POST http://localhost:8000/profiles \
  -H "Authorization: Bearer <token>" \
  -F "github_username=TabarekAyad" \
  -F "portfolio_url=https://example.com"
{"id":"0d96e7d4-...","user_id":"efdb5b37-...","github_username":"TabarekAyad","portfolio_url":"https://example.com","created_at":"2026-09-30T08:20:24.973536Z","resume_filename":null}
[HTTP 200]

$ curl http://localhost:8000/profiles/0d96e7d4-1b42-4cd8-b080-7d1cf82c3fc3 \
  -H "Authorization: Bearer <token>"
{"id":"0d96e7d4-...","github_username":"TabarekAyad","portfolio_url":"https://example.com",...}
[HTTP 200]

$ curl -X POST http://localhost:8000/reviews \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"profile_id":"0d96e7d4-1b42-4cd8-b080-7d1cf82c3fc3"}'
{"id":"b2e9814b-...","profile_id":"0d96e7d4-...","status":"pending",...}
[HTTP 200]

$ curl http://localhost:8000/reviews/b2e9814b-aab4-4802-bbbe-56b8042df15c \
  -H "Authorization: Bearer <token>"
{"id":"b2e9814b-...","status":"complete","sections":[...],"overall_score":0.81,...}
[HTTP 200]

$ curl "http://localhost:8000/reviews?page=1&page_size=5" \
  -H "Authorization: Bearer <token>"
{"items":[...],"total":4,"page":1,"page_size":5}
[HTTP 200]

$ curl -X DELETE http://localhost:8000/profiles/0d96e7d4-1b42-4cd8-b080-7d1cf82c3fc3 \
  -H "Authorization: Bearer <token>"
[HTTP 204]
```

**Expected:** each endpoint entry in `docs/API.md` includes an example curl command showing the required method, headers, and body shape.

**Actual:** zero curl examples and zero code blocks in the file. A few things I had to look up from `api/routes/` before I could call the endpoints correctly: `/auth/login` takes an OAuth2 form body with a `username` field (not `email`), `/profiles` takes multipart form fields (not JSON), and `/health` returns 503 on a fresh setup due to health-check bugs unrelated to this issue.

**Also noted (not part of this issue):** `/health` reports postgres and redis as unhealthy on a fresh setup — the uvicorn logs show two known bugs in the health check code. The API itself functions correctly.

## Eval iterations

**Run history**

The rubric required two rounds of revision before reaching the final 20/20 run.

Round 1 (targeted re-runs, not saved): The initial `honest-outcome` check required that every claim in the repro report be backed by a shown artifact. pkg-03 (BurntSushi/ripgrep#2779) was rejected on this check because the report includes a control-run statement — "Dropping `-r '$1'` from the same command reports 1, 4, 7, 10 correctly" — without showing that output. The gold label is `accept`. The fix: the primary buggy-path artifact is required; secondary control-run or expected-path statements do not need their own output shown, as long as the main artifact is present.

The initial `conventions` check required that both comments disclose AI use when the repo's policy requires it. pkg-07 (processing/p5.js#7168) was rejected because only the claim comment disclosed, not the repro comment. The gold label is `accept`. The p5.js policy requires disclosure, not per-comment disclosure — one disclosure in the claim comment satisfies it. The fix: "at minimum the claim comment discloses explicitly."

Final run (saved as eval-run.txt): 20/20, all five categories at ceiling (clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4). PASS.

**Package analysis**

Package: pkg-03 (BurntSushi/ripgrep#2779, category: clear-accept). Gold label: accept. My rubric's initial verdict: reject, on `honest-outcome`.

The repro report shows the wrong output (line numbers 1, 2, 3, 4 instead of 1, 4, 7, 10) and then says "Dropping `-r '$1'` from the same command reports 1, 4, 7, 10 correctly." The initial check required every claim to be backed by a shown artifact, and that control-run statement has no output block. The primary buggy-path artifact is present and matches the issue; the control-run statement is supporting context, not the main claim. The check was over-strict: it treated a secondary statement as if it were a primary reproduction claim, then failed the package for not proving what was only asserted in passing. After revising `honest-outcome` to require an artifact only for the primary claim, pkg-03 passed correctly.

**Check rationale**

Quoted from `rubric.md` exactly as it reads now:

> | honest-outcome | repro report body and shown artifacts | the primary claim about whether the bug was reproduced is backed by a shown artifact; an evidenced cannot-reproduce (describes what was tried and what came back) counts as pass; a confident reproduction claim backed by zero shown artifacts fails; secondary control-run claims or statements about expected-path behavior do not need their own output shown, as long as the main buggy-path artifact is present | required |

This check draws the line at the primary claim because the most common way a repro report fails is that it confidently states "reproduced" with no output shown. Secondary statements (control runs, expected-path verification, "removing flag X restores correct behavior") are context, not claims; requiring output for them would cause every report with an explanatory aside to fail. The cannot-reproduce branch passes because a report that honestly describes what was tried and what came back is more useful to maintainers than a fabricated confirmation. The secondary-claims carve-out was added specifically after pkg-03 failed on the initial version of this check.

**Trade-offs**

The main trade-off is in `honest-outcome`. Requiring an artifact for every statement (primary and secondary) catches fabricated reports but also rejects accurate reports that include interpretive context without re-pasting the output — pkg-03's control run is the live case. Dropping the artifact requirement entirely would pass every report that sounds confident, which is no check at all. The current boundary relies on correctly identifying which claim is "primary." A report that buries its main claim inside a secondary-sounding sentence could slip through. I re-ran pkg-03 with `--only pkg-03` after each revision to confirm the fix held, and ran the full 20 at the end to verify no previously-agreeing package flipped.

---

Related paths: `eval-run.txt` in this directory; your skill's files in `tools/repro-check/`.
