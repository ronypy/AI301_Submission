---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is
this pull request ready to submit?** A PR package is a candidate pull
request (its title, description, commit list, unified diff, and test
evidence) read against two things: the accepted plan it claims to
implement (including that plan's scope, test plan, deviation notes,
and the repro evidence it built on) and the issue that plan belongs
to (the issue body, its thread, and the repo's stated PR template and
contribution policy).

Do not answer any other question. Do not review code style for its
own sake, judge whether the plan itself was a good idea, suggest a
better fix, or grade more than one package per run. The plan was
already accepted; your job is whether this PR faithfully delivers it,
proves it, is clean enough to review, and meets the repo's stated
asks. Never answer from gut feel: execute the components in this
directory.

## Inputs and modes

Decide the mode first, from what you were given.

**Eval mode** applies when you are handed a package bundle (a
markdown file headed `# Eval package: <id>`, or its `.json` twin).
The bundle is the whole world: the "Repo facts" block, the "Issue"
section, the "Thread highlights", the "Plan context" (accepted plan
plus its repro evidence), and the "Candidate PR" (Title,
Description, Commits, Diff, Test evidence). Use ONLY the bundle text
as evidence. Do not fetch anything, do not open the `source:` issue
on GitHub, do not read any other file except this tool's own
components. Eval mode always grades a complete package: every check
in `rubric.md`, the full verdict rule. Use the bundle id
(e.g. `pkg-07`) as the JSON `item`.

**Live mode** applies when a student asks you to grade their own
draft PR before opening it. Run from the top folder of their fork's
working copy, on their fix branch. Gather exactly these inputs:

- **The plan**: `plan.md` in the working-copy root, including its
  `## Deviations` section if present. A house-chain student uses the
  house plan and the house repro pack the TF handed out instead; the
  same checks grade the same things there.
- **The diff**: run `git diff main...HEAD` (three dots) from the
  working copy. That output is everything the branch changes relative
  to the default branch, and it is the only diff you grade. Also run
  `git log --oneline main..HEAD` for the commit list. Uncommitted
  changes are not part of the PR; ignore them and say so if
  `git status` shows any.
- **The draft PR title and description**: `pr_draft.md` in the
  working-copy root; the first line is the title, everything under
  it is the description.
- **The test evidence**: `test_evidence.md` in the working-copy root
  (repro before/after and the repo's checks output).
- **The issue**: the issue URL the student names. Read the issue
  body and its thread (via `gh issue view <n> --comments` or the web)
  for maintainer direction.
- **The repo's stated asks**: the PR template at
  `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in
  the working copy (or the repo on GitHub if they are absent
  locally).

If any of these inputs is missing in live mode, do not invent it:
grade the checks that need it `unclear` and name the missing file in
the evidence line. Use the issue URL (or the PR URL, if one exists)
as the JSON `item`.

## The scope seam (live mode only)

In live mode, read `scope.md` in this directory before anything
else. If its `Repo:` line still carries the bracketed placeholder
(`<ORG>/<PATH-REVIEW-REPO>` or anything in angle brackets), stop
without grading and tell the student to fill the `Repo:` line in
`scope.md` with their section's Path Review repo; never guess a
scope. Otherwise, confirm the working copy's upstream (`git remote
-v`) and the issue URL both belong to the scoped repo; if either does
not, refuse to grade and say which one is out of scope. Apply the
scope's house rules as reading rules: the PR comes from a
`fix/<issue-number>-<slug>` branch on the student's fork, one PR per
issue, the repo's template always used, and a classmate's PR on the
same issue does not block this one.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` in this directory
and hold the outgoing text (the PR title and the description from
`pr_draft.md`) against each of its rules and its "Things I never
post" list. Report every rule the draft breaks in the summary, above
the JSON block, quoting the rule's name and the offending phrase. A
broken voice rule never changes the verdict on its own, because the
rubric has no check that reads the voice guide: voice is the
student's personal standard, not a readiness gate.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order and use each for one job:

1. `rubric.md` defines the checks (name, evidence, pass condition,
   weight) and the verdict rule. It decides WHAT is graded and how
   grades combine. Use the check names exactly as written there in
   the JSON.
2. `references/evidence-guide.md` is the map: WHERE each evidence
   family lives in a bundle and in live mode, and what good looks
   like there.
3. `procedure.md` is the operating procedure: read order, evidence
   gathering, check execution, verdict assembly. Execute it as
   written, step by step.

When the procedure is silent on something you need, do not improvise
a step around it: grade with what the written steps produced, and
report the gap in the summary as "procedure gap: <what was missing>".

If `rubric.md` has no filled check rows (only an empty table or
comments) or no verdict rule, or `procedure.md` has no written steps
under its headings, refuse to grade: say which file is still a
template and emit no verdict. Instruction comments (`<!-- -->`) are
not content.

## Verdict and output

The verdict is binary: `accept` means ready to submit; `reject`
means hold. There is no third verdict, no score, and no "accept with
changes"; reservations go in a check's evidence line.

Before the JSON block, write a short readable summary: one line per
check (name, grade, the deciding fact), the deciding check for the
verdict, any procedure gaps, and (live mode only) any voice-guide
rules broken. Then end the reply with this fenced JSON block,
filled in, with one `checks` entry per rubric check in rubric order.
The block must be valid JSON and the last thing in your reply;
nothing follows it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote from the package that decided it. "Looks fine" or "seems
  good" is not evidence.
- **Grade the thing, not the polish.** Read the diff against the
  plan, the evidence against the test plan, the description against
  the diff, the PR against the repo's stated asks. A terse PR that
  meets every pass condition is ready; a long, confident, perfectly
  formatted PR whose diff does something the plan never named is not.
  The description's claims are claims, not evidence; the diff and the
  test output are evidence.
- **The rubric decides, not the run.** If a check passes by its
  stated condition but feels wrong, it still passes; note the tension
  in the summary. Do not add checks the rubric does not have.
- **The procedure decides how, not the run.** Follow `procedure.md`
  as written and report its gaps instead of inventing steps.
- **Unclear is a fail.** Use `unclear` only when the part of the
  package a check needs is genuinely absent. The rubric's verdict
  rule counts `unclear` as a failing grade; where any rule is silent,
  an unverifiable claim is a failing one. A PR you cannot verify from
  the package is not ready to submit.
- **Honest shortfalls are not failures.** A PR that delivers less
  than everything, but records the shortfall in the plan's deviation
  notes and restates it in the description, can be ready. Only
  unannounced differences count against it.
