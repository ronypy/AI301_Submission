# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | Report's "Environment:" line (tool version, OS, build toolchain, commit/tag) | States tool version, OS, and commit or tag — matching whatever the repo's issue template or CONTRIBUTING.md asks for. A version alone with no OS/commit fails | required |
| Steps are minimal and re-runnable | Report's "Steps" section | Steps start from a stated, reproducible starting point (fresh clone/checkout, or a file created within the steps) and end at the issue's exact reported command. A helper/test file created mid-steps does not need to be shown as a literal block — a precise structural description, or a pointer to input already given verbatim in the issue, is sufficient. Only fail on genuinely vague descriptions ("configured it similarly," "my usual test file") that leave a stranger unable to tell what to write | required |
| Expected vs. actual both stated | Report's expected/actual lines | Both an expected outcome and an actual outcome are written explicitly — not just "it crashed" but what should have happened instead | required |
| Actual behavior shown, not asserted | Report's attached log/output block | A raw terminal log or output block is present, produced by the exact command in the Steps section — not a paraphrase or summary of what happened | required |
| Behavior matches the issue's claim | Report's actual output vs. issue body's reported symptom (error message, exit code, panic vs. non-panic) | Same failure mode as the issue describes (e.g., issue reports exit 101 panic, report also shows exit 101 panic) — a different exit code or error type fails, even if something did go wrong | required |
| Honest under a negative result | Report's overall claim vs. its own evidence | If reproduction did NOT succeed, the report says so plainly ("could not reproduce on X") rather than claiming success the evidence doesn't support. A confident claim unsupported by the log/steps above fails regardless of polish | required |
| AI-assistance disclosed when required | Repo-facts block's contribution/AI policy language vs. the candidate claim comment | If the repo's stated policy explicitly requires disclosing AI usage — naming the tool and the extent of assistance — the claim comment does so. If the repo's policy is silent on AI, or only states general responsibility for reviewing AI-assisted contributions without requiring a disclosure statement, this check passes automatically regardless of the comment's content | required |
| Comment draft follows voice rules | Draft comment text meant for the issue thread | No promised timelines ("I'll fix this by tomorrow"); names the specific version and behavior rather than "this bug"; states next step as a report/attempt, not a merge promise; discloses first-time contributor status if true | required |
| No filler or generic enthusiasm | Draft comment text | Free of generic phrases ("amazing project!!", "+1", "any update?") that carry no version- or behavior-specific content | preferred |


## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept if every `required` check passes — this includes a plain, evidence-matched "could not reproduce" report; readiness is about proof quality, not about confirming the bug. Reject if any `required` check fails or is `unclear`.
 
`unclear` counts as fail on every required check — a report or draft that doesn't give enough to judge a check is not ready to post.
 
`preferred` checks never change the verdict. Among ready reports, use them only to flag polish worth cleaning up before posting.
 