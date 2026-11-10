# Contribution: Add an example for MCOD

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Md Rakibul Hasan
**GitHub Username:** ronypy
**Issue:** https://github.com/selimfirat/pysad/issues/645
**Pull Request:** https://github.com/selimfirat/pysad/pull/656

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Installed issue-scout over my Unit 1 skill and wrote its search strategy (2026-11-09)**

My first install was broken: the skill files sat one folder too deep, in `~/.claude/skills/issue-select/skill/`, so Claude Code never loaded the skill. I rebuilt it as a flat `~/.claude/skills/issue-scout/` (staff `SKILL.md`, `scope.md`, `search-strategy.md` plus my own `rubric.md` and evidence guide, checked byte for byte against a backup), pasted my fit profile into the new scope, and wrote four `gh search issues` queries and six ranking filters. While testing the queries, `sql OR database` in one query returned 0 results, so I split it into separate `sql` and `database` queries. I never ran a full scan or a direct grade; I picked my issue from a manual `gh search` instead (see Why I Chose This Issue).

**Unit 6: Voice guard, wild scopes, and a repo memory file for pysad (2026-11-09)**

I had three drifting copies of my voice guide (repro-check, plan-check, pr-precheck); I merged them into one at `~/.claude/voice-guard/voice-guide.md`, kept the two rules only the pr-precheck copy had, added an Upstream section (claims, questions, pings), and symlinked all three skills to it. The hook went into `settings.json`, and the wiring test was stopped as expected ("no body the guard can read"). The guard log shows only that test: I posted my claim on the GitHub website, so the guard never checked it, which is the gap the guard's own docs warn about. The memory file `CLAUDE.local.md` in my clone distills CONTRIBUTING, AGENTS.md and the examples subsystem; once the clone's `CLAUDE.md` symlink was restored, I dropped the `@AGENTS.md` import because `CLAUDE.md` already loads it.

**Unit 7: Ship-gate and manifest; the gate stopped me twice before it passed (2026-11-09)**

Claude Code's auto-mode safety check blocked the agent from running `install.py` (it edits my global settings), so I ran the install myself. My manifest has three must-pass checks from pysad's contributing guide (pre-commit, `pytest`, `pytest -m examples`) and two dated exclusions: the benchmark (no detector change) and `example_creditcard.py`, whose dataset download failed on master too. First stop: `'.venv' is not recognized`, because the gate runs commands through Windows `cmd.exe` and my `.venv/Scripts/...` paths used forward slashes; I rewrote them as `.venv\Scripts\...exe`. Second stop: my pr-precheck rejected the description on ai-disclosure, so I put the AI-drafted text in a quote block with my own paragraph, and the gate passed.

**Unit 8: PR monitor and playbook (TODO: fill after installing and running the monitor)**

TODO before submitting: install the PR monitor (Week 8 steps 1-3), write the playbook, run `monitor.py` on PR #656, and replace this paragraph with what it read, what it drafted, and what you did with the draft. As of 2026-11-09 the PR has only the bot's welcome comment and no review.

---

## Why I Chose This Issue

My fit profile says Python and SQL are my strengths and that I want to avoid front-end work. I searched GitHub with `gh search issues` for open, unassigned Python issues labeled "good first issue" or "help wanted" and updated in the last month. Most of the results came from repos only weeks old with one active person, so I checked repo health by hand: stars, last push, and whether recent PRs from outside contributors were merged. Two strong picks, pavlov99/json-rpc #135 and mila-iqia/cluv #278, already had open PRs, so I dropped them. selimfirat/pysad, a streaming anomaly detection library, had 318 stars, a push the same day, a CONTRIBUTING promise to reply "within a few days", an outside contributor's PR (#650) merged that day, and a written AI policy that allows AI-assisted work with rules. Issue #645 was unclaimed, had zero comments, and spelled out the whole deliverable: the file name, the script to model it on, and the docs file. It is Python and data/ML work with no UI, and I wanted to learn how a real library's examples, docs and CI fit together.

Honest read of the scout: I did not use it. I installed and configured it, but I never ran `issue-scout: run a scan` or a direct grade on #645, so there is no verdict to report. The manual checks I did (open PRs, outside merges, AI policy) are the ones my rubric encodes, but I applied them by hand. What I would change: run the scan before picking next time, and add a rubric check for "an open PR already references this issue", because that is what knocked out my first two candidates.

My claim comment (2026-11-09; 2026-11-10 00:47 UTC): https://github.com/selimfirat/pysad/issues/645#issuecomment-6755617394. The owner, selimfirat, assigned me 15 minutes later: https://github.com/selimfirat/pysad/issues/645#issuecomment-6755855804.

**Toolkit**

- Tool: issue-scout (installed, with my search strategy and Unit 1 rubric) plus manual `gh search issues` and `gh api` checks. In the end I chose by hand.
- What it produced: the scout produced no scan or grade. The manual search produced about 258 recent candidates; I narrowed them by comment count and repo health to pysad #645, #644 and #643, and picked #645.
- Difficulties: the broken nested install meant the scout wasn't loadable until I fixed it. The raw search was flooded by brand-new single-maintainer repos, which my ranking filters can't see because stars aren't in the search result fields.
- What changed: I rebuilt the skill layout and wrote the search strategy. I would add an "open PR references this issue" check to the rubric and run a real scan before choosing next time.

---

## Understanding the Issue

**Problem Description**

pysad ships MCOD (`pysad/models/mcod.py`), a distance-based streaming detector, but no runnable example shows how to use it. None of the 59 scripts in `examples/` uses MCOD or any other distance-based detector (ExactStorm, ApproxStorm, CPOD, NETS), and `docs/examples.rst` has no section for it.

**Expected Behavior**

An `examples/example_mcod.py` that loads `cardio.mat` with `Data().get_data`, streams it with `ArrayStreamer`, scores each point with `MCOD(...).fit_score_partial`, updates an `AUROCMetric` and prints the AUROC, with short comments on how the score and the radius work. Plus a section in `docs/examples.rst` that includes the script and its real output.

**Current Behavior**

There is no MCOD example. `pytest -m examples -k mcod` selects no test, and the examples page never mentions MCOD.

**Affected Components**

`examples/` (new script), `docs/examples.rst` (new section), and `tests/test_examples.py`, which runs every `examples/example_*.py` automatically, so it needs no change. `pysad/models/mcod.py` is read only, as the source for the two comments.

**Toolkit**

- Tool: Claude Code, reading the issue, `mcod.py`, `AGENTS.md`, `tests/test_examples.py` and merged PR #650 in my clone; I checked its summary against the files myself.
- What it produced: the four answers above, and the finding that #650 (OneFitModel example) is the exact precedent: 2 files, same docs layout.
- Difficulties: none met; the issue itself was precise.
- What changed: nothing.

---

## Reproduction Process

**Environment Setup**

Windows 11 Pro, Git Bash and PowerShell, Python 3.11.9, pip upgraded in a venv. I forked `selimfirat/pysad` to `ronypy/pysad`, cloned it with `upstream` pointing at the original, created `.venv` and ran `pip install -e . --group dev`, as the contributing guide says. Base commit: `f529f6f` (docs: add OneFitModel example (#650)).

Two Windows problems made pre-commit fail on untouched master. First, git's `core.autocrlf=true` checked files out with CRLF line endings, so the mixed-line-ending hook rewrote 440 files. I confirmed the content diff ignoring CR was empty, then set `core.autocrlf=false` and `core.eol=lf` and re-checked out. Second, upstream `CLAUDE.md` is a symlink to `AGENTS.md`; Windows checked it out as a one-line text file and the end-of-file hook changed it. I set `core.symlinks=true` and re-checked out. After that, all 13 hooks passed on master. A third problem was the network: `example_creditcard.py` failed in the full examples run on master with `http.client.IncompleteRead` while downloading its large dataset.

**Steps to Reproduce**

1. On master `f529f6f`, run `grep -l -E "MCOD|ExactStorm|CPOD|NETS|ApproxStorm" examples/*.py`: no output, out of 59 example scripts.
2. Run `grep -c MCOD docs/examples.rst`: `0`.
3. Observed result: `pytest -m examples -k mcod` prints `2524 deselected` and exits 5 (no test selected); `python examples/example_mcod.py` fails with "No such file or directory".

**Reproduction Evidence**

- **Commit showing reproduction:** none. The issue is a missing file, so the reproduction is the absence on master `f529f6f`; the fix commit is https://github.com/ronypy/pysad/commit/86e609ad0395de6e1d042d4abf471f2e5ae3f779
- **Screenshots/logs:** `pytest -m examples -k mcod` on master: `=========== 2524 deselected in 5.65s ===========` (exit 5). `Data().get_data("cardio.mat")` loads shape `(1831, 21)` with 176 anomalies.
- **My findings:** the gap is exactly as described. MCOD exists and works on cardio; only the example and its docs are missing. The Windows checkout issues were local and unrelated to the issue.

**Toolkit**

- Tool: Claude Code running the grep and pytest commands in my clone, and my memory file (`CLAUDE.local.md`) for the commands.
- What it produced: the before-evidence above, and a baseline of every check on master (pre-commit, 2461 unit tests passed, 60 of 61 example tests passed).
- Difficulties: the CRLF and symlink problems made master look broken until fixed; the creditcard download failure.
- What changed: the clone's git config (LF endings, real symlinks). I recorded the creditcard failure as a master-side network problem instead of hiding it.

---

## Solution Approach

**Analysis**

There is no bug in the code. The cause is a missing example. pysad keeps one script per public capability in `examples/`, and `tests/test_examples.py` collects every `example_*.py` and runs it under the `examples` marker. `docs/examples.rst` has one section per example: a heading, a short explanation, a `literalinclude` of the script, and an "It prints:" block with the real output. MCOD was added to `pysad/models/` without either piece.

**Proposed Solution**

Add `examples/example_mcod.py` modeled on `examples/example_rs_forest.py`, and a docs section right after the RS-Forest section. I compared window sizes on cardio first (128: 0.728, 256: 0.790, 512: 0.843, 1000: 0.879, all under a second) and chose `window_size=512`, because it is larger than the default `calibration_size` of 256, so the points that set the radius stay inside the window. Deliberately not changing: MCOD itself, any other detector, the tests, `docs/api.rst`, the benchmark results, or examples for ExactStorm, CPOD or NETS (one PR per issue).

**Implementation Plan**

1. Branch `docs/645-mcod-example` from `upstream/master`, following the `<type>/<issue>-<slug>` rule in the contributing guide.
2. Add `examples/example_mcod.py` (19 lines) with two comments: the score is the fraction of window points that are not neighbors, and with `max_radius=None` the first `calibration_size` points set the radius.
3. Add the "Use a distance-based detector" section to `docs/examples.rst` with the `literalinclude` and `AUROC: 0.843`.
4. Decisive check: `pytest -m examples -k mcod` goes from 0 selected to 1 passed, and the script prints `AUROC: 0.843`. Then run pre-commit, `pytest`, `pytest -m examples` and `bash build_docs.sh`.
5. One commit, `docs: add an MCOD example`; PR with `Fixes #645`.

Deviation: the plan named the docs section "Score distances to a sliding window with MCOD"; the built section is titled "Use a distance-based detector", to match the style of the neighboring headings ("Use a random space tree detector"). pr-precheck noted the difference and passed it.

**Toolkit**

- Tool: plan-check, my Unit 3 skill, graded `plan.md` and `plan-comment.md` against issue #645.
- What it produced: verdict reject on one check, ai-disclosure. pysad's policy says to "Put the text that an AI tool writes in a quote block... Add your own comment", and my AI-drafted plan comment had neither. Every other required check passed; decisive-test passed only narrowly.
- Difficulties: the comment was never meant to be posted (Week 7 says not to), but plan-check grades it as if it would be, which is correct for the wild.
- What changed: I put the drafted text in a quote block with a slot for my own comment, and rewrote plan step 4 to state the exact before and after result, so decisive-test is no longer narrow.

---

## Testing Strategy

The decisive check is the reproduction re-run. On master, `pytest -m examples -k mcod` selects 0 tests (exit 5) and `example_mcod.py` does not exist. On my branch, the same command collects `tests/test_examples.py::test_example_runs[example_mcod.py]` and it PASSES, and `python examples/example_mcod.py` prints `AUROC: 0.843`, the same value the docs section shows. The rendered `docs/_build/html/examples.html` contains the new section with that output.

The repo's own checks, master versus branch:

- `pre-commit run --all-files`: all 13 hooks passed on both.
- `pytest -n auto`: 2461 passed, 1 skipped, 1 xfailed on both.
- `pytest -m examples`: master 60 passed, 1 failed; branch 61 passed (including MCOD), 1 failed. The one failure is `example_creditcard.py` both times, from a broken dataset download (`IncompleteRead` on master, a SHA-256 mismatch on the branch), not from code.
- `bash build_docs.sh`: build succeeded, with 1 warning about an unrelated benchmark include file.

On the PR, GitHub CI passed on Ubuntu (Python 3.10, 3.11, 3.13), macOS and Windows, plus the `examples`, `docs` and coverage jobs. `pre-commit.ci` shows "error during clone", an error on that service's side.

**Toolkit**

- Tool: the ship-gate, running my `ship-manifest.md` checks and then pr-precheck.
- What it produced: on the final run, pre-commit ok (12s), unit tests ok (76s), examples ok (140s), and every pr-precheck check passed, including decisive-evidence: "Before: 2524 deselected... after: AUROC: 0.843 and test_example_runs[example_mcod.py] PASSED".
- Difficulties: the full examples run takes about 9 minutes locally and depends on dataset downloads from the network.
- What changed: I excluded only the creditcard example, with a dated reason, and kept every other example as a must-pass check.

---

## Implementation Notes

**2026-11-09**

Picked pysad #645 after json-rpc #135 and cluv #278 turned out to have open PRs. Posted my claim on the issue; the owner assigned me 15 minutes later. Forked to `ronypy/pysad`, cloned it, created the venv, and wrote `CLAUDE.local.md`, excluding it with `.git/info/exclude`.

**2026-11-09**

Baseline checks on master failed until I fixed two Windows checkout issues (CRLF line endings and the `CLAUDE.md` symlink). After that, master was green apart from the creditcard download. Wrote `plan.md`; plan-check rejected it on ai-disclosure, and I revised it.

**2026-11-09**

Compared MCOD window sizes on cardio and chose 512. Wrote the example and the docs section, committed `86e609a docs: add an MCOD example`, and left off the usual tool attribution trailer because pysad's AGENTS.md forbids `Co-Authored-By` lines for tools. Ran all checks and wrote `test_evidence.md` and `pr_draft.md`.

**2026-11-09**

Ship-gate run 1 stopped on the `cmd.exe` path problem; run 2 stopped on ai-disclosure; run 3 passed. Pushed the branch and opened PR #656. The description on GitHub has no AI note: PR #650 was merged without one, and pysad's AGENTS.md asks for no AI mentions in PR text. I record the AI use here instead (see Pull Request).

**Toolkit**

- Tool: Claude Code as the main build tool in my clone, with my memory file and the repo's AGENTS.md loaded.
- What it produced: the 19-line example, the 14-line docs section, the commit, and the working files (plan, evidence, PR draft).
- Difficulties: Windows-only setup problems took longer than the change itself; the auto-mode safety check blocked the agent from editing my global settings, so I ran the installers myself.
- What changed: the clone config, the manifest paths, and running installs myself instead of through the agent.

---

## Pull Request

**PR Link:** https://github.com/selimfirat/pysad/pull/656

**PR Description:** "docs: add an MCOD example", with `Fixes #645`. It says the PR adds `examples/example_mcod.py`, which streams `cardio.mat` through `MCOD(window_size=512)` and prints the AUROC, modeled on `example_rs_forest.py`, with two comments on the score and the radius calibration, plus a section after RS-Forest in `docs/examples.rst`, and no change to MCOD or any other code. A "Before and after" section shows `2524 deselected` on master versus `AUROC: 0.843` and the passing `test_example_runs[example_mcod.py]` on the branch, and lists the pre-commit, pytest, examples and docs results, including the creditcard download failure. pysad's template checklist is filled, with "Every change" ticked.

What AI helped with: Claude Code helped me find the issue, set up the clone, write the example and the docs section, and draft the plan, evidence and PR description. I read the code and MCOD's docstring to check the comments are accurate, and the results above come from the checks run on my machine and in CI. I edited the description down before opening the PR and left the AI note out of it, following pysad's AGENTS.md.

**Maintainer Feedback:**

- 2026-11-09: none yet. The only comment is the bot's welcome message ("Please make sure the change comes with a test"); no reply needed, because `tests/test_examples.py` already runs the new script and the CI `examples` job passed.
- 2026-11-09: nothing to address yet.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate hook (manifest checks plus pr-precheck) before opening the PR.
- What it produced: `ship-gate: PASS` on the third run, with all eight pr-precheck checks passing, then PR #656 with green CI.
- Difficulties: the PR description on GitHub ended up different from the version the gate passed: I removed the quote block and my AI paragraph after the gate run. The gate only checks the description at the moment the PR opens, not later edits.
- What changed: I kept the description as it is (matching #650 and AGENTS.md) and moved the AI statement here. Next time I would decide on the description before running the gate, so the version it passes is the one maintainers read.

---

## Learnings and Reflections

**Technical Skills Gained**

How a mature Python library wires examples into its test suite (`pytest -m examples` discovering every `example_*.py`) and into Sphinx docs with `literalinclude`. Using conventional-commit titles and branch names. How MCOD scores: it uses the fraction of window points outside a radius set from the first 256 points. Debugging Windows git behavior: `core.autocrlf`, `core.symlinks`, and how pre-commit's line-ending hooks react to them. Writing shell commands that work under `cmd.exe` as well as bash.

**Challenges Overcome**

The hardest parts were environmental, not the code. Master looked broken until I traced it to Windows line endings and a symlink checked out as a text file. The ship-gate failed until I learned it runs commands through `cmd.exe`. Separating a real failure from noise mattered too: the creditcard example failed on master as well, so I recorded it with a reason instead of hiding it or treating it as mine.

**What I'd Do Differently Next Time**

Run my scout's scan and grade before choosing, instead of picking by hand and leaving the tool unused. Post comments through Claude Code so the voice guard actually checks them. Settle the PR description before running the ship-gate, so the version it passes is the one I post. Read AGENTS.md and the contributing guide together early, because they disagreed about AI mentions in PRs and I had to choose late.

---

## Resources Used

- pysad contributing guide (setup, checks, PR rules, AI policy): https://pysad.readthedocs.io/en/latest/contributing.html
- pysad `AGENTS.md` (example style, PR metadata rules): https://github.com/selimfirat/pysad/blob/master/AGENTS.md
- Merged PR #650, the template for this change: https://github.com/selimfirat/pysad/pull/650
- The MCOD class docstring in `pysad/models/mcod.py`, the source for the two comments
- The issue thread, where the owner assigned me: https://github.com/selimfirat/pysad/issues/645
