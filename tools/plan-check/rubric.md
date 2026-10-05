# Rubric: is this plan ready to post and build from?

Every check below reads the plan against the package's own evidence:
the issue, the thread highlights, the repo-facts block, and above all
the repro evidence (including its control runs). Where to find each
piece is in `references/evidence-guide.md`; when to gather it is in
`procedure.md`.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-cause | The plan's stated cause (Diagnosis / Cause line, or the summary sentence that says why the bug happens) read against every step and control run in the repro evidence, especially the "Actual" line and any control that changes one variable (flag off, color off, no pager, relative vs. absolute, other OS). | Passes if the cause the plan states is consistent with every repro step and control: no control run in the repro shows the bug persisting with the blamed component removed or unchanged, or vanishing with the blamed component still present. Fails if any control rules the blamed component out, or if the plan adopts a cause from the thread (or the issue) that the repro evidence contradicts. Fails if the plan states no cause at all. A cause taken from a maintainer or the thread passes only if the repro evidence does not contradict it. | required |
| fix-targets-cause | The plan's change/approach section read against its own stated cause and the repro's "Actual" line. | Passes if the change acts on the mechanism the evidence pins down, so that if made, the repro's failing step would behave as expected. Fails if the change only works around or documents the symptom (docs-only note, user-side workaround, retry/timeout) while the evidence points at a fixable cause in code, unless the plan says why the cause cannot be changed and the thread agrees. | required |
| bounded-scope | The plan's scope / in-scope and not-in-scope lines, its list of changes, and the files named, read against what the issue reports. | Passes if every listed change is needed to fix the reported behavior (plus its regression test and directly related docs). Fails if the plan bundles any work the issue did not ask for: refactors, rewrites, dependency upgrades or migrations, new options or settings, UI rework, CI changes, or fixes to other components "while in the area". One unrequested item is enough to fail. Explicitly deferring larger work (named as out of scope) is fine and does not fail. | required |
| executable | The plan's change/approach and files sections. | Passes if a stranger could start work today without asking the author: the plan names the file(s) or the specific function/site to change and commits to one approach. Fails if it names no file or code site, says "somewhere", "investigate", "profile and see", "not sure which layer", "whichever is easier", or leaves the choice between approaches to build time. Stating a bounded open question (with how it will be resolved) does not fail this check. | required |
| decisive-test | The plan's test plan read against the repro evidence's steps and its "Expected" line. | Passes if the test plan names a specific observable outcome that would differ before and after the fix for the reported behavior: re-running the repro's failing step(s) with a stated expected result (output, color, exit code, timing, match), or a named regression test asserting that result. Fails if the only test is generic ("run the test suite", "nothing regresses", "should feel fast", "make sure it works") or if it checks something other than the reported behavior (e.g. only that a config is registered). | required |
| honest-certainty | The plan's risks/unknowns/open-question lines, and claims of certainty in the plan and comment ("this fixes", "I traced this", "verified"), read against what the repro evidence actually shows. | Passes if every certainty claim is backed by the package evidence, and anything not yet verified is stated as unverified or an open question. Fails if the plan or comment claims something the evidence does not show (e.g. "traced it", "verified" with no evidence) or states as fact what the repro contradicts. | required |
| thread-engagement | The plan comment read against the thread highlights, specifically comments by OWNER / MEMBER / COLLABORATOR, and any linked open PRs or prior art. | Passes if, when a maintainer has given explicit direction in the thread (named the culprit or file, proposed or chose a fix option, posted a patch/test build, asked for testing, said working-as-intended), the comment explicitly engages it: follows it by name, or says why it diverges. Also passes if the thread has no such direction. Fails if the comment proposes a different direction while ignoring the maintainer's stated one, or races an open PR without acknowledging it. | required |
| ai-disclosure | The repo-facts block's contribution policy line read against the plan comment. Treat every package as AI-assisted work. | Passes if the policy has no AI rule that applies to issue comments, or if the comment satisfies what the policy requires (e.g. disclosure of AI use with tool/extent, statement that it is in the author's own words). Fails if the policy requires disclosure of all AI usage (or disclosure in comments) and the comment contains none. A policy that requires disclosure only in pull requests does not require it in the comment. | required |
| repro-referenced | The plan comment. | Passes if the comment points at the reproduction (version, "report above", or the key result) so a maintainer can tie the plan to evidence. | preferred |
| comment-tone | The plan comment. | Passes if the comment makes no delivery-date promises ("this week", "tonight") and no unsupported hype. | preferred |

## Verdict rule

- **accept** if every `required` check grades `pass`.
- **reject** if any `required` check grades `fail` or `unclear`.
- `unclear` counts as `fail`: it is used only when the evidence the
  check needs is genuinely absent from the package (for example, the
  plan states no test plan at all). A plan we cannot verify from the
  package is not ready to build from.
- `preferred` checks are graded and reported but never change the
  verdict.
- The deciding check for a reject is the first failing `required`
  check in table order; quote its evidence in the output.
