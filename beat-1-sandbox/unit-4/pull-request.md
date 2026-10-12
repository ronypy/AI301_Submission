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

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/144

**Branch**

fix/68-keyword-search-empty-index

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run 1: crashed before any package was graded. On Windows the harness wrote the prompt
   to the `claude` subprocess in cp1252 and hit
   `UnicodeEncodeError: 'charmap' codec can't encode characters`. No score; I re-ran with
   `PYTHONUTF8=1 PYTHONIOENCODING=utf-8`.
2. Full run 2: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.
   The one miss was `pkg-05  clear-accept  accept  reject  NO  failed: decisive-evidence`.
3. Partial run (`--only pkg-05,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration`),
   after revising `decisive-evidence`: `agreement: 5/5 scored items`, and calib-04 still
   `reject` (agrees). Partial runs set no bar.
4. Full run 3, the confirming run saved with `--save-run eval-run.txt`:
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

Final score: 20/20, matching the agreement line in the `eval-run.txt` I committed.

**Package analysis**

`pkg-05` (nushell/nushell#18848, category `clear-accept`). Gold label: `accept`. On full run 2
my rubric decided `reject`, failing `decisive-evidence`. On the final run it decided `accept`.

Why it rejected first: the plan's test plan names three things: "re-run the issue's script,
expect both `atuin` rows; re-run with two same-name same-key bindings, expect one row plus a
warning; cargo test on the config crate." The test evidence shows the first one in full, with
before/after tables going from one `atuin` row to two. It covers the warning only in prose:
"Same-key redefine prints the one-time warning." My check then said "When the plan's test plan
names more than one failure mode or repro, each must be shown", so the grader counted the
unshown warning as a missing failure mode and held the PR.

That reading was wrong for this package. The warning is a new side behavior the fix adds, not
a second way the reported bug breaks. The bug (ctrl-r reverting to `history_menu` because
same-name bindings collapse) is shown failing before and fixed after, which is what the gold
note credits: "repro table before/after shown". After I split "failure modes of the reported
bug" from "secondary items", the rubric read pkg-05 as `accept`.

**Check rationale**

From `tools/pr-precheck/rubric.md`, exactly as it reads now:

> | decisive-evidence | The Test evidence section (and any before/after in the Description), read against the plan's Test plan and the repro evidence's failing step(s) and Expected line. | Passes if the evidence shows the plan's repro (the failing step on the changed code path, with the same inputs that broke) re-run on the branch, with the observable after-result shown (output, exit code, rendered result, table) and the before-result shown or quoted from the repro, and the after matches the plan's expected-after. When the repro evidence or test plan names more than one failure mode of the reported bug (repro 1, repro 2, a second command that broke), each must be shown with output. Secondary items in the test plan that are not the reported failure (controls, an added warning or side behavior, the suite run) may be stated in prose and do not need shown output. Fails if the evidence is only an assertion ("tested locally", "works now", "verified on my machine", "tests pass", "cargo test passes") with no observable before/after for the reported behavior; or if it exercises a path the fix does not change (the control case, a different method or input than the repro) instead of the repro; or if any failure mode of the reported bug that the plan's test plan names is not shown with output. A new unit test alone does not substitute for the re-run repro unless the plan's test plan says it does. | required |

Why it reads that way: the first version said "When the plan's test plan names more than one
failure mode or repro, each must be shown", and its fail clause said "any failure mode the
plan's test plan names is not shown". I wrote that to catch calib-04, where the plan names
"both failure modes by name" (`-j 9223372036854775807` and `-j 200000 -x echo`) and the
evidence re-runs only repro 1. But "names" was broad enough to cover every item in a test plan,
so it also rejected pkg-05 for a warning shown only in prose.

I didn't drop the "every failure mode" rule, because that would let calib-04 through. I also
didn't stop at "show the first repro", for the same reason. Instead I scoped the rule to
failure modes "of the reported bug" and listed what does not need output: "controls, an added
warning or side behavior, the suite run". The procedure got the same split: each test-plan
observable is tagged `FAILURE-MODE` or `SECONDARY`, and only `FAILURE-MODE` needs shown output.
The "only an assertion" and "a path the fix does not change" clauses are what hold pkg-04,
pkg-10 ("verified working on my machine"), pkg-07 (single-file control only) and pkg-14 (GET
instead of the POST repro).

**Trade-offs**

Loosening `decisive-evidence` risked letting not-tested packages through, so before the
confirming run I re-ran canaries with `--only`. I picked every `not-tested` package (pkg-04,
pkg-07, pkg-10, pkg-14), plus calib-04 via `--include-calibration`, because calib-04 is the
"two failure modes, one shown" case the loosened wording could flip. All five still came back
`reject`. pkg-05 flipped to `accept`, and the confirming full run kept
`not-tested 4/4` and every other category intact (20/20).

What the check gives up: a PR whose secondary behavior is broken can pass if its evidence
describes that behavior in a sentence instead of showing it. With pkg-05, if the same-key
warning didn't actually fire, my rubric would still accept, because it trusts a prose claim for
anything tagged secondary. I accept that miss. A false "it prints a warning" is a different
problem from "the bug isn't proven fixed", and catching it would mean failing honest,
well-evidenced PRs like pkg-05 for not pasting output for every side effect.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
