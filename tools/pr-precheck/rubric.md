# Rubric: is this pull request ready to submit?

Every check below reads the candidate PR against the package's own
evidence: the accepted plan (its scope, files, changes, test plan,
and any deviation or deferral notes), the repro evidence that plan
built on, the issue and its thread, and the repo's stated asks. Where
each fact lives is in `references/evidence-guide.md`; when to gather
it is in `procedure.md`. The description and the test-evidence prose
are claims; the diff and the shown command output are evidence.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | Every file and hunk in the Diff, read against the plan's scope line, its "Files" list, its listed changes, its "Not in scope" line, and any deviation/deferral note in the plan or restated in the description. | Passes if (a) every changed file and hunk is either one of the plan's listed changes, its regression test, or directly needed docs/changelog for it, or is covered by a deviation note, AND (b) every change the plan lists (code, test, docs) is present in the diff or its absence is recorded in a deviation/deferral note. Fails if the diff adds anything the plan did not name and no note covers: a refactor, renaming pass, new option/setting/flag, unrelated help-text or file edits, a rewrite of a function the plan did not touch, a dependency bump. Fails if a planned change is missing with no note. Fails if the diff adds something the plan's "Not in scope" line excludes. One unannounced item is enough to fail. | required |
| description-matches-diff | Each factual claim in the Description and Title about what the PR changes ("implements the plan exactly", "no changes beyond the plan", "no functional changes outside X", "docs updated", "test added", "changelog entry added"), read against the Diff. | Passes if every such claim is true of the diff. Fails if the description claims fidelity ("exactly as planned", "no other changes") while the diff contains unplanned work, or claims a file, docs update, test, or entry the diff does not contain, or describes a different fix than the diff makes. A description that says less than the diff is graded by diff-matches-plan, not here. | required |
| decisive-evidence | The Test evidence section (and any before/after in the Description), read against the plan's Test plan and the repro evidence's failing step(s) and Expected line. | Passes if the evidence shows the plan's repro (the failing step on the changed code path, with the same inputs that broke) re-run on the branch, with the observable after-result shown (output, exit code, rendered result, table) and the before-result shown or quoted from the repro, and the after matches the plan's expected-after. When the repro evidence or test plan names more than one failure mode of the reported bug (repro 1, repro 2, a second command that broke), each must be shown with output. Secondary items in the test plan that are not the reported failure (controls, an added warning or side behavior, the suite run) may be stated in prose and do not need shown output. Fails if the evidence is only an assertion ("tested locally", "works now", "verified on my machine", "tests pass", "cargo test passes") with no observable before/after for the reported behavior; or if it exercises a path the fix does not change (the control case, a different method or input than the repro) instead of the repro; or if any failure mode of the reported bug that the plan's test plan names is not shown with output. A new unit test alone does not substitute for the re-run repro unless the plan's test plan says it does. | required |
| clean-diff | The Diff's added (`+`) lines and hunks, and the Commits list. | Passes if every added line serves the change. Fails if the diff adds any of: debug output (print/eprintln/console.log/dbg!/"DEBUG" logging), commented-out code or a commented-out earlier attempt, a dead or unused function/variable (including one behind `allow(dead_code)` or named `_unused...`), a TODO/FIXME/XXX left on unrelated code, or a hunk that only re-indents, re-formats, re-orders imports, or re-prints identical lines in code the fix does not otherwise touch. Explanatory comments that describe the fix are not debris. Commit messages like "wip", "fix", "misc cleanups while debugging" support a fail but never decide it alone. | required |
| template-asks | The Repo facts "pull requests" line (template sections, required checklist items, files the template or contributing guide require for this kind of change, e.g. a changelog/whatsnew entry, tests with behavior changes, an issue link like "closes #"), read against the Title, Description, and Diff. | Passes if the repo states no template or asks, or if every stated ask is met: each named template section or checklist item has real content in the description (under that heading or in unmistakable equivalent words), each required file entry is present in the diff or explicitly marked not applicable with a reason, and a required issue-link form appears. Fails if any stated ask is visibly ignored: a required section or checklist missing, a required issue link absent, or a required entry (changelog, whatsnew, test) absent from the diff with no word about it. | required |
| ai-disclosure | The Repo facts "contribution policy" line (AI rules), read against the Description. Treat every package as AI-assisted work. | Passes if the policy states no AI rule, or the AI rule does not cover pull requests, or the description satisfies the rule (e.g. names the AI tool/extent, states the human reviewed and understands the work, says the text is in the author's own words, as the policy asks). Fails if the policy requires disclosure of all AI usage, or disclosure in PRs or comments to maintainers, and the description contains no such disclosure. | required |
| repo-checks-run | The Test evidence, read against the repo's stated test/lint commands (Repo facts, contribution guide). | Passes if the evidence names the repo's suite or check command(s) and their outcome, including an honestly reported failure with its reason. | preferred |
| commit-hygiene | The Commits list. | Passes if each commit message says what it changes (no bare "wip", "fix", "stuff"). | preferred |

## Verdict rule

- **accept** if every `required` check grades `pass`.
- **reject** if any `required` check grades `fail` or `unclear`.
- `unclear` counts as `fail`. Grade `unclear` only when the part a
  check needs is genuinely absent from the package (for example, no
  Test evidence section at all, or no plan context). A PR we cannot
  verify from the package is not ready to submit.
- `preferred` checks are graded and reported but never change the
  verdict.
- **Honest-outcome rule:** a PR that delivers less than the full plan
  is not a failure when the shortfall is recorded in the plan's
  deviation/deferral note and restated in the description. Grade
  diff-matches-plan and description-matches-diff `pass` in that case;
  "less than everything" is never by itself a reason to hold.
- The deciding check for a reject is the first failing `required`
  check in table order; quote its evidence in the summary.

<!--
How the checks cover the four ways a PR gets held (the eval set's
reject categories):

- silent drift: diff-matches-plan catches the diff doing more than
  the plan (drive-by rewrites, new flags/settings, renames) or less
  with no note; description-matches-diff catches the description
  claiming fidelity, docs, or tests the diff contradicts.
- not tested: decisive-evidence catches "tests pass" / "works on my
  machine" with no observable before/after, evidence that exercises
  the unchanged path or the control instead of the repro, and a test
  plan with two failure modes where only one is shown.
- unreviewable: clean-diff catches debug prints, commented-out code,
  dead functions, stray TODOs, and formatting/import churn that bury
  the change.
- standards wall: template-asks catches a required PR template,
  checklist, issue link, or changelog/whatsnew entry visibly ignored;
  ai-disclosure catches a repo policy that requires disclosing AI use
  with no disclosure in the description.
-->
