# Procedure: how this skill grades a plan package

These are the steps for grading a plan package. Follow them in order.
Do not grade anything until the Read order and Evidence gathering
stages are done. `references/evidence-guide.md` says where each fact
lives; this file says when to pull it and what to do with it.

## Read order

1. Read `rubric.md` and list the check names, their weights, and the
   verdict rule. Read `references/evidence-guide.md`.
2. Read the **repro evidence first** (eval mode: the "Repro evidence"
   block; live mode: the student's posted repro comment, or the house
   repro pack as quoted in the drafts). Write down:
   - the failing step(s) and their exact "Actual" result;
   - the "Expected" result;
   - every control run, and in one line what each control rules in
     or out (e.g. "step 3: 26 s with no pager at all -> pager is not
     the cause"; "relative glob works from both cwds -> only absolute
     globs affected").
   Read it before the plan so the plan's diagnosis cannot set the
   frame: the repro is the ground truth the plan must follow.
3. Read the **issue** (title, body, labels, version) and write down
   the one behavior it reports. This is the yardstick for scope.
4. Read the **thread highlights**. Write down each comment from an
   OWNER, MEMBER, or COLLABORATOR that gives direction (names a
   culprit/file, chooses or proposes a fix option, posts a patch or
   test build, asks for testing, says working-as-intended), and every
   open PR or prior art mentioned. Note confident claims from
   non-maintainers separately; they are claims, not evidence.
5. Read the **repo-facts block** (live: CONTRIBUTING.md, AI_POLICY.md,
   issue templates). Write down the contribution policy's AI rule word
   for word, and whether it applies to issue comments, PRs only, or
   all AI usage.
6. Read the **candidate plan**, then the **candidate plan comment**.
   Mark the cause sentence, the in/out scope lines, each listed
   change, the files named, the test plan, and any risk/unknown lines.

## Evidence gathering

For each check, pull exactly these facts and quote them:

- **grounded-cause**: quote the plan's cause sentence. Next to it,
  list each control from step 2 and write "consistent" or
  "contradicts" for each. If the plan's cause was taken from the
  thread, note who said it and whether the repro supports it.
- **fix-targets-cause**: quote the plan's change list. Write whether
  each change alters the mechanism behind the repro's "Actual" line or
  only documents/works around it.
- **bounded-scope**: list every change and file the plan names. Mark
  each "needed for the reported behavior" or "not asked for by the
  issue". Out-of-scope items the plan explicitly defers do not count
  as changes.
- **executable**: quote the files/sites named and the approach chosen.
  Quote any hedging words ("somewhere", "investigate", "not sure",
  "whichever", "maybe also").
- **decisive-test**: quote the test plan. Write down the observable it
  names, and whether it matches the repro's failing step and Expected
  line.
- **honest-certainty**: quote each certainty claim in the plan and
  comment, and the repro fact (if any) that backs it. Quote any stated
  unknowns.
- **thread-engagement**: for each maintainer-direction item from read
  step 4, quote the sentence in the comment that engages it, or write
  "not mentioned".
- **ai-disclosure**: quote the AI policy from read step 5 and the
  disclosure sentence in the comment, or write "no disclosure".
- **preferred checks**: quote the comment's repro reference and any
  date promise or hype.

In eval mode, all of this comes only from the bundle text. In live
mode, fetch issue-side facts from the places the evidence guide names
(issue page, thread, CONTRIBUTING.md / AI_POLICY.md), and take the
candidate side only from the drafts.

## Check execution

1. Run the checks in rubric table order: grounded-cause,
   fix-targets-cause, bounded-scope, executable, decisive-test,
   honest-certainty, thread-engagement, ai-disclosure, then the
   preferred checks.
2. For each check, apply the pass condition word for word to the
   facts gathered for it. Grade `pass` or `fail` and write one line of
   evidence: the quote or fact that decided it.
3. Grade the substance, not the writing: a short plan that meets every
   pass condition passes; a long, confident, well-formatted plan that
   fails a condition fails. Do not reward polish or penalize terseness.
4. A cause, direction, or claim confidently stated in the thread is
   not evidence. Only the repro evidence decides grounded-cause.
5. Grade `unclear` only when the part the check needs does not exist
   in the package (e.g. no test plan, no plan comment). If the part
   exists but is weak, grade `pass` or `fail` on it. Before grading
   `unclear`, look in every location the evidence guide lists for that
   family.
6. Grade every check, even after a required check has failed; the
   student needs the full picture. You may grade a later check from the
   facts already gathered without re-reading the package.
7. If the rubric's condition and your gut disagree, the condition
   wins; note the tension in the summary.

## Verdict assembly

1. Count `unclear` as `fail` (rubric verdict rule).
2. If every `required` check is `pass`, the verdict is `accept`.
   Otherwise the verdict is `reject`.
3. `preferred` checks never change the verdict.
4. In the summary, name the deciding check: for a reject, the first
   failing required check in table order, with its quoted evidence; for
   an accept, the check that came closest to failing.
5. In live mode, add any voice-guide rule the comment breaks to the
   summary (it does not change the verdict).
6. Output the JSON block from SKILL.md last, with one entry per check
   in table order, and nothing after it.
