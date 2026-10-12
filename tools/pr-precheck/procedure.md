# Procedure: how this tool grades a PR package

Follow these steps in order. Do not grade any check until Read order
and Evidence gathering are done. `references/evidence-guide.md` says
where each fact lives; this file says when to pull it and what to do
with it. Most gathers this week are side-by-side pairings: the diff
against the plan, the evidence against the test plan, the description
against the diff, the PR against the repo's asks.

## Read order

1. Read `rubric.md`. List the check names in table order, their
   weights, and the verdict rule. Read
   `references/evidence-guide.md`.
2. Read the **plan first** (eval: the "Plan context" block; live:
   `plan.md` with its `## Deviations` section, or the house plan).
   Write down, as lists:
   - PLAN-CHANGES: each change the plan commits to (code, test, docs,
     changelog), one line each;
   - PLAN-FILES: every file the plan names;
   - NOT-IN-SCOPE: everything the plan explicitly excludes;
   - NOTES: every deviation, deferral, or "known limit" note, word
     for word;
   - TEST-PLAN: each observable the test plan names (each repro or
     failure mode, each expected-after, each control).
   Read the plan before the PR so the PR's own story cannot set the
   frame: the plan is the yardstick the diff is measured against.
3. Read the **repro evidence** inside the plan context. Write down
   the failing step's exact inputs, its "Actual" result, and its
   "Expected" result, plus each control run and what it shows.
4. Read the **issue** and **thread highlights**. Note any
   OWNER/MEMBER/COLLABORATOR direction (it may justify a plan note).
5. Read the **Repo facts** block (live: `.github/PULL_REQUEST_TEMPLATE.md`
   and `docs/CONTRIBUTING.md`). Write down, as a list, every stated
   PR ask (each template section, each checklist item, each required
   file entry such as changelog/whatsnew, required issue-link form,
   tests-with-changes) and the AI-policy rule word for word.
6. Read the **Diff** (live: `git diff main...HEAD`). Write down
   DIFF-FILES: every changed file, and for each file each hunk in one
   line ("ui.lua: escapes metacharacters in win32 branch"; "fd.rs:
   rewords --hidden help text").
7. Read the **Commits** list.
8. Read the **Title** and **Description** last. Write down every
   factual claim about what the PR changes or contains.
9. Read the **Test evidence** section.

## Evidence gathering

For each check, pull exactly these facts:

- **diff-matches-plan**: build a two-column pairing. For each line in
  DIFF-FILES, write the PLAN-CHANGES item or NOTES entry it belongs
  to, or "UNPLANNED". Treat a regression test for the planned change
  and a changelog/docs entry the plan or repo asks for as belonging
  to the change. Then, for each PLAN-CHANGES item, write the hunk that
  delivers it, or "MISSING" plus the NOTES entry that records its
  absence, or "MISSING, no note". Mark any hunk that does something
  listed in NOT-IN-SCOPE.
- **description-matches-diff**: for each claim from read step 8,
  write the hunk that makes it true, or "NOT IN DIFF". Fidelity
  claims ("exactly as planned", "no changes beyond the plan", "no
  functional changes outside X") are true only if the
  diff-matches-plan pairing has no UNPLANNED row.
- **decisive-evidence**: for each TEST-PLAN observable, quote the
  test-evidence line that shows it (command plus shown output), or
  write "NOT SHOWN". For each quoted run, compare its inputs to the
  repro's failing step: same inputs and code path, or a control /
  unchanged path / different input. Note whether a before result is
  shown or quoted, and whether the after matches the Expected line.
  Tag each TEST-PLAN observable FAILURE-MODE (a repro of the reported
  bug) or SECONDARY (control, added warning/side behavior, suite run).
  Only a FAILURE-MODE observable needs shown output; a SECONDARY one
  stated in prose counts as shown.
- **clean-diff**: scan every `+` line and every hunk. List each
  debug print, commented-out code line, dead/unused function,
  stray TODO/FIXME, and each hunk that is only re-indentation,
  re-formatting, import reordering, or identical re-printed lines.
  Quote the commit messages.
- **template-asks**: for each ask from read step 5, write where it is
  met (description heading or sentence; diff file for a required
  entry) or "NOT MET".
- **ai-disclosure**: quote the AI rule from read step 5, say whether
  it covers PRs/all AI use, and quote the description's disclosure
  sentence or write "no disclosure".
- **repo-checks-run / commit-hygiene**: quote the suite or lint
  command and its outcome from the evidence; quote the commit
  messages.

In eval mode all of this comes only from the bundle. In live mode the
diff and commits come from git, the candidate text from
`pr_draft.md`, the evidence from `test_evidence.md`, the plan from
`plan.md`, and the asks from the repo's template and CONTRIBUTING.md.

## Check execution

1. Run the checks in rubric table order: diff-matches-plan,
   description-matches-diff, decisive-evidence, clean-diff,
   template-asks, ai-disclosure, then the preferred checks.
2. Apply each pass condition word for word to the facts gathered for
   it, and grade `pass` or `fail`. Write one line of evidence: the
   quote, hunk, or missing item that decided it.
3. diff-matches-plan fails on any UNPLANNED row, any "MISSING, no
   note", or any NOT-IN-SCOPE hit. A MISSING item with a NOTES entry
   passes (honest shortfall).
4. description-matches-diff fails on any "NOT IN DIFF" claim.
5. decisive-evidence fails on any "NOT SHOWN" FAILURE-MODE observable, or if the
   only runs shown use control/unchanged-path inputs, or if the
   evidence is bare assertion with no shown output.
6. clean-diff fails if the list from gathering is non-empty for the
   added lines; explanatory comments about the fix do not count.
7. template-asks fails on any "NOT MET" ask; ai-disclosure fails when
   the rule covers PRs or all AI use and there is "no disclosure".
8. Grade the substance, not the writing: a terse PR that meets every
   condition passes; a confident, polished one that fails a condition
   fails. Never take the description's word for what the diff
   contains.
9. Grade `unclear` only when the package part a check needs does not
   exist (e.g. no Test evidence section). If it exists but is weak,
   grade pass or fail. Before grading `unclear`, look in every place
   the evidence guide lists for that family.
10. Grade every check, even after a required one fails. A later check
    may be graded from the facts already gathered without re-reading
    the package.
11. If the condition and your instinct disagree, the condition wins;
    note the tension in the summary.

## Verdict assembly

1. Count `unclear` as `fail`.
2. If every `required` check is `pass`, the verdict is `accept`;
   otherwise `reject`. `preferred` checks never change the verdict.
3. Name the deciding check in the summary: for a reject, the first
   failing `required` check in rubric table order, with its quoted
   evidence; for an accept, the required check that came closest to
   failing and why it still passed.
4. In live mode, after the verdict, list any voice-guide rule the
   title or description breaks (never changes the verdict).
5. Emit the JSON block from SKILL.md last, one `checks` entry per
   rubric check in table order, and nothing after it.
