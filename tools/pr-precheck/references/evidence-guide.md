# Evidence guide: where evidence lives in a PR package

For each evidence family: where it lives in an eval bundle (the
`pkg-*.md` / `calib-*.md` sections, or the same fields in the `.json`
twin), where it lives in live mode (your working copy and GitHub),
and what good looks like. The four families map one to one onto the
eval categories: plan fidelity = silent-drift, test evidence =
not-tested, diff quality = unreviewable, standards and comms =
standards-wall.

## Plan fidelity (harness category: silent-drift)

- **Where it lives (eval):** the plan's scope is in the "Plan
  context" block: the "Plan:" paragraph (its "Scope" / "one bounded
  change" sentence, listed changes, "Not in scope:" line, "Files:"
  line) and any "Known limit", deferral, or deviation sentence. What
  actually changed is the "### Diff" section under "Candidate PR":
  the `--- a/` / `+++ b/` file headers and each `@@` hunk. The
  fidelity claims are in "### Title" and "### Description".
- **Where it lives (live):** your `plan.md` (its scope, files,
  changes, and `## Deviations` section); your branch's diff from
  `git diff main...HEAD` run in the top folder of your fork's clone;
  your draft title and description in `pr_draft.md` (title on the
  first line).
- **What good looks like:** every file header in the diff is in the
  plan's Files line (or is the regression test / changelog for the
  planned change), every hunk does a change the plan lists, every
  change the plan lists has a hunk, and anything else is covered by a
  note in the plan that the description restates. Silent drift in the
  more direction: a renaming pass, a new config option or CLI flag, a
  rewrite of a neighboring function, reworded unrelated help text, a
  hunk in a file the plan never names, under a description saying
  "exactly as planned". Silent drift in the less direction: the plan
  promised a docs update or a second fix and the diff has none, with
  no note, or the description claims it is there. An honest
  shortfall (the plan's deferral note, restated in the description)
  is fidelity, not drift.

## Test evidence (harness category: not-tested)

- **Where it lives (eval):** the "### Test evidence" section of the
  Candidate PR (and any before/after blocks inside the Description),
  read against the plan's "Test plan:" sentence and the "Repro
  evidence" paragraph (its failing command, Actual, Expected, and
  controls). The repo's own test command is in the Repo facts or
  contribution line.
- **Where it lives (live):** `test_evidence.md` next to `plan.md`
  (your unit-2 repro steps re-run on `main` for the before and on
  your branch for the after, plus the output of `make test-unit`,
  `make test-integration`, `make lint`, `make typecheck`); the test
  plan in `plan.md`.
- **What good looks like:** the repro's own failing command, with the
  same inputs, re-run on the branch, with the output shown, next to
  the before result, and the after equal to the plan's expected-after
  (a full URL and exit code 0; both table rows present; a
  byte-identical round-trip). Every failure mode the test plan names
  is shown, not just the first. The repo's checks are named with
  their outcome; a failing check reported honestly with its reason
  is still evidence. Not decisive: "tested locally, works now",
  "verified on my machine for a day", "cargo test passes" alone; a
  run of the single-file control or the GET path when the bug was the
  two-file case or the POST path; a new test that asserts a different
  behavior than the one the issue reports.

## Diff quality (harness category: unreviewable)

- **Where it lives (eval):** the "### Diff" section (every `+` line
  and every hunk) and the "### Commits" list.
- **Where it lives (live):** `git diff main...HEAD` and
  `git log --oneline main..HEAD`; `git diff main...HEAD --stat`
  should list only the files your change needs.
- **What good looks like:** every added line is the fix, its test,
  or a comment explaining the fix; a reviewer sees the change in one
  read. Debris tells: `println!`/`eprintln!`/`print(`/`console.log`/
  `dbg!` or "DEBUG" output added; `+ // old_code(...)` commented-out
  lines or a commented-out first attempt; a function nothing calls,
  `#[allow(dead_code)]`, names like `_unused_*`; a TODO added on an
  untouched line; hunks that only re-indent, re-wrap, re-order
  imports, or remove and re-add identical lines; commits named
  "wip", "fix", "fmt + cleanup", "misc cleanups while debugging".

## Standards and comms (harness category: standards-wall)

- **Where it lives (eval):** the "## Repo facts" block: the "pull
  requests:" line (template sections, required checklist items,
  issue-link form like "closes #xxxx" or "fixes #<n>", required
  changelog/whatsnew entries, tests-with-changes) and the
  "contribution policy" line (AI rules). Compliance lives in the
  Title, the Description (section headings or equivalent content,
  checklist answers, issue link, AI disclosure), and the Diff (the
  changelog or whatsnew file).
- **Where it lives (live):** the Path Review repo's
  `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` (the
  AI-use policy and checks list) in your working copy; compliance in
  `pr_draft.md` (every template section filled, "Closes #<n>", the
  Notes for Reviewers AI disclosure) and in your diff.
- **What good looks like:** every section the template names has real
  content in the description, under its heading or in unmistakable
  equivalent words ("Type of changes: Bug fix"; "Requirements:
  cheatsheets regenerated (no diff); integration test added"); every
  required entry (CHANGELOG.md, whatsnew) is in the diff or marked
  n/a with a reason; the required issue link appears. When the policy
  requires disclosing all AI use (or AI use in PRs/comments), the
  description says which tool helped, how much, and that the author
  reviewed and understands the work, in their own words. A repo with
  no template and no AI rule asks nothing beyond what its
  contribution line states. A visibly ignored ask: a required
  checklist absent, no "closes #", no whatsnew entry when the template
  requires one for bug fixes, no disclosure under a strict AI policy.
  (Whether the description's claims match the diff is plan fidelity,
  above.)
