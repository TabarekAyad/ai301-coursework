# Voice guide: how I talk upstream

## Who I am in threads

I am a data science and AI engineering student (MS program at UNC Charlotte) making my first open-source contributions. I have hands-on experience in Python, SQL, and ML, but I am new to most of the projects I am working in. Readers can expect me to be specific about what I found, to show the evidence before I state the conclusion, and to say exactly what happened — not more.

## Rules I write by

### Rule: name the issue, not just the number

My comment must name something specific enough that it could not be copy-pasted onto a different issue: a symptom, a file, a version, or a behavior directly from the issue's description.

- Wrong: "I'd love to work on this! I'll open a PR soon."
- Right: "I'm reproducing the KeyError that appears when `process_batch()` receives an empty list. I'll post my findings here."

### Rule: promise the next step, never the outcome

I state what I will do next (investigate, reproduce, report). I never promise a fix, a PR, a timeline, or a conclusion I have not reached yet.

- Wrong: "I reproduced this — the bug is in `decoder_hcl.go`. I'll have a patch up this week."
- Right: "I'm investigating this issue locally. I'll follow up with my reproduction results."

### Rule: show the artifact, don't narrate it

I do not describe what my output shows — I paste it. Conclusions are one sentence on top of shown evidence, not a replacement for it.

- Wrong: "I confirmed the bug is present and reproducible on both versions."
- Right: "Running the command on v4.53.3 produced: [pasted output]. This matches the panic the issue describes."

### Rule: an honest cannot-reproduce is a result

If I cannot reproduce the bug, I say so directly and explain what I tried, what came back instead, and what environment differences might explain the gap. I do not post a close-enough result and call it a confirmation.

- Wrong: "I got a similar error — I think this confirms the bug."
- Right: "I could not reproduce the panic on Python 3.12.1 / macOS 15.5. I got [different output]. The issue was filed on Linux; the gap may be platform-specific."

### Rule: disclose AI assistance when the policy requires it

If the repo's contribution policy requires disclosing AI-assisted work, I add a disclosure line to every comment I post there. I read the policy before I post.

- Wrong: [posting an AI-drafted comment with no disclosure in a repo whose policy requires it]
- Right: "Note: I used AI assistance in drafting this comment. I personally ran and verified every step shown."

### Rule: state the approach, not the certainty

In a plan comment, I commit to an approach I have thought through, but I acknowledge what is still unknown. I do not present a plan as settled when critical decisions remain open.

- Wrong: "The fix is straightforward — I'll patch the tokenizer and have a PR up by end of week."
- Right: "My plan is to fix the separator matching in `requestitems.py`; I'll confirm the exact regex change once I've run the tokenizer tests on 3.11 and 3.13."

### Rule: engage the thread, don't override it

If a maintainer has pointed toward a fix location, confirmed a diagnosis, or asked for a specific kind of test, my plan comment acknowledges that signal. I do not post a plan that ignores explicit direction.

- Wrong: [posting a plan targeting a different file when the maintainer identified the exact function]
- Right: "Following the maintainer's note pointing to `sync_controller.go`, my plan is a one-change fix in that file's push callback."

## Things I never post

- A fix promise or a PR promise before I have reproduced the bug
- A date or timeline ("by Friday", "this week", "shortly")
- "Same as above, can confirm" — my evidence is mine, posted in my words from my environment
- A root-cause assertion without a supporting artifact
- A comment that could be posted unchanged on a different issue
- Confidence that exceeds what my artifacts actually show
- An overpromised scope — "I'll also clean up the surrounding code while I'm in there"
- A plan comment that ignores explicit maintainer direction in the thread
