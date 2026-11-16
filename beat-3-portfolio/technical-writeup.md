# Adding a runnable MCOD example to PySAD, and what my own checks caught

## Overview

PySAD is an open source Python library for detecting anomalies in data streams. It ships `MCOD`, a distance-based detector that scores each new point against a sliding window, but none of its 59 example scripts used it, and `docs/examples.rst` never mentioned it. Issue [#645](https://github.com/selimfirat/pysad/issues/645) asked for an example. I claimed it, the maintainer assigned it to me, and I wrote `examples/example_mcod.py` plus a matching docs section. I opened [PR #656](https://github.com/selimfirat/pysad/pull/656). As of today it is still open with no review. All CI jobs that ran against it passed, apart from the pre-commit.ci status, which errored while cloning.

## What happened

**Proving the gap.** Nothing was broken, so the "reproduction" was an absence. On master at `f529f6f`:

```text
$ grep -l -E "MCOD|ExactStorm|CPOD|NETS|ApproxStorm" examples/*.py
(no output)
$ pytest -m examples -k mcod
=========================== 2524 deselected in 5.65s ===========================
```

PySAD's `tests/test_examples.py` runs every `example_*.py` automatically, so the test I needed was the new file itself. With zero tests selected before and one after, that became my pass/fail check.

**Picking the window size.** `MCOD` calibrates its radius from the first `calibration_size` (256) points when `max_radius=None`. The default window is smaller than that, so I tried four window sizes on `cardio.mat` before writing anything:

```text
window_size  128 -> AUROC 0.728
window_size  256 -> AUROC 0.790
window_size  512 -> AUROC 0.843
window_size 1000 -> AUROC 0.879
```

Each run took under a second. I chose 512, the smallest size that keeps the 256 calibration points comfortably inside the window. I did not choose 1000, even though it scores higher, because the example should show the mechanism rather than chase a number. The script that came out of this:

```python
# The score is the fraction of window instances that are not neighbors of the instance.
# With max_radius=None, the first calibration_size (256) instances set the radius.
model = MCOD(window_size=512)
```

After the change, `python examples/example_mcod.py` printed `AUROC: 0.843`, and `pytest -m examples -k mcod -v` gave `test_example_runs[example_mcod.py] PASSED` (1 passed, 2524 deselected). The docs section quotes the same `AUROC: 0.843`.

**The check that caught me.** I run a pre-submission gate on my branch: a ship-gate that runs the repo's own checks, then my `pr-precheck` rubric against my PR description. My first gate run failed on a path typo in my manifest (`.venv/Scripts/pre-commit` is not a command on Windows cmd), which I fixed. The second run passed every repo check and blocked on my description. This is the entry from my `gate-log.md`, dated 2026-11-09 21:32:

```text
- ai-disclosure [fail]: CONTRIBUTING AI rule: 'Put the text that an AI tool writes in a quote block
  or a code block. Add your own comment that tells why the AI text is relevant'; the description
  has no AI-use statement, no quote-block marking and no read-and-understood statement.
```

PySAD's contributing guide has that rule, and my first description ignored it. I rewrote the description so that it opens with a statement that I used Claude Code, that I read the script and MCOD's docstring to check the comments, and that I ran the example and the checks myself. The 21:41 gate run then passed with all eight checks green. This was a change to my own work, caused by my own tool catching a real miss, and the second version is the one that counted.

**Environment problems I fixed before they hid real results.** Windows git checked out the repo with CRLF line endings, so pre-commit's mixed-line-ending hook rewrote 440 files on untouched master. I set `core.autocrlf false` and re-checked out; the content diff ignoring line endings was 0. `CLAUDE.md` is a symlink upstream, which Windows checked out as a plain file, so I set `core.symlinks true`. Without both fixes, pre-commit would have failed on code I never touched. After them, all 13 hooks passed on master and on my branch.

**What landed.** The issue thread shows the maintainer's reply to my claim: "Thanks @ronypy! I assigned this issue to you." On the PR, `all-checks`, `examples`, `docs`, and the test matrix on Ubuntu, macOS and Windows all passed. `pytest` gave 2461 passed, 1 skipped, 1 xfailed, the same as master. Nothing has landed beyond that: no review, no merge.

## Limitations and what's next

The PR is open and unreviewed, so I cannot say the example is accepted. If the maintainer wants a different window size or a different dataset, I will change the script and report the new AUROC in the thread.

The example covers `MCOD` only. PySAD's other distance-based detectors (`ExactStorm`, `ApproxStorm`, `CPOD`, `NETS`) still have no example. I kept those out on purpose, since the issue asked for one detector and one PR should do one thing. If the PR merges, I will suggest a follow-up issue instead of widening this one.

The 0.843 depends on a window size I chose by trying four values on one dataset. It shows that the example works, not that 512 is the best setting for MCOD in general.

Two checks did not pass cleanly, and neither is a code problem. `example_creditcard.py` fails because its dataset download breaks on my connection, and it failed on master too (`http.client.IncompleteRead` there, a SHA-256 mismatch on the branch). The pre-commit.ci status on the PR shows "error during clone". I excluded the first with a dated reason in my `ship-manifest.md`, and I have not yet found out why the second happened, so I will ask the maintainer if it is still there on the next push.

My own gate has a gap that this PR showed. It checks the PR description I drafted locally, not the one live on GitHub, so the two can drift apart. Next I will make the gate compare the draft with the live description before it passes.
