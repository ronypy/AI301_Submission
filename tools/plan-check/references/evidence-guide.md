# Evidence guide: where evidence lives in a plan package

For each evidence family: where to find it in an eval bundle (the
`pkg-*.md` / `calib-*.md` sections, or the same fields in the `.json`
twin), where to find it live, and what good looks like.

## Diagnosis and grounding

- **Where it lives (eval):** the cause is in the "Candidate plan"
  section, under "Diagnosis" or "Cause", or the first sentence of the
  Summary saying why the bug happens. The behavior it must explain is
  in the "Repro evidence" block: the numbered steps, the "Control"
  runs, and the "Expected" / "Actual" lines. Confident cause claims
  from other people are in "Thread highlights" and the issue body.
- **Where it lives (live):** the plan's cause in the draft `plan.md`;
  the repro in the student's posted repro comment on the issue (or the
  house repro pack quoted in the drafts).
- **What good looks like:** the stated cause explains every repro
  step, and no control run contradicts it. A control that changes one
  variable is the key test: if the bug persists with the blamed part
  removed (e.g. 26 s with no pager at all) or disappears with the
  blamed part untouched (e.g. bindings unchanged but colors off fixes
  it), the diagnosis is wrong, however confident the thread is. The
  change must act on that cause, not document a workaround for it.

## Scope

- **Where it lives (eval):** "Scope" / "In scope" / "Not in scope"
  lines, the "Changes" or "Approach" list, and the "Files" list of the
  candidate plan; the issue title and body define what was asked.
- **Where it lives (live):** the same sections of `plan.md`, read
  against the issue body.
- **What good looks like:** one bounded change to the code behind the
  reported behavior, plus its regression test. Every listed item is
  needed to fix what the issue reports. A drive-by rewrite adds
  anything the issue did not ask for: refactors, migrations,
  dependency upgrades, new options/settings, UI rework, CI matrix
  changes, "while I'm in here" fixes. Naming larger work as explicitly
  deferred is good scoping, not scope creep.

## Executability

- **Where it lives (eval):** "Changes", "Approach", "Change", and
  "Files" in the candidate plan.
- **Where it lives (live):** the same sections of `plan.md`.
- **What good looks like:** a named file (e.g.
  `pkg/gui/controllers/sync_controller.go`) or function/site, and one
  chosen approach in order. A stranger could open the file and start.
  Red flags: "somewhere", "investigate", "profile and optimize", "not
  sure which layer", "upstream or vendored, whichever is easier",
  "maybe also". A bounded open question with how it will be settled
  is fine.

## Test plan

- **Where it lives (eval):** "Test plan" / "Test" in the candidate
  plan, read against the repro's numbered steps and "Expected" line.
- **Where it lives (live):** the test plan in `plan.md`, read against
  the posted repro comment.
- **What good looks like:** it re-runs the repro's failing step and
  names the observable result that proves the fix (a color flips at
  step 3, exit code 0, a match is printed, the seek completes
  immediately), and/or adds a regression test asserting that result.
  A vague plan names no observable for the fix itself: "run the full
  test suite", "nothing regresses", "should feel fast", "nothing else
  should feel broken", or checks only an internal detail rather than
  the reported behavior.

## Honesty

- **Where it lives (eval):** "Risk", "Unknowns", "Open question"
  lines in the plan; certainty words in the plan and comment ("I
  traced", "verified", "this fixes"); for a built plan, a
  "Deviations" section.
- **Where it lives (live):** the same in `plan.md` and the draft
  comment; after the build, the `## Deviations` section of `plan.md`
  and any follow-up comment on the issue.
- **What good looks like:** every certainty claim is backed by
  something in the repro evidence; things not yet checked are named as
  unchecked (e.g. "verified gzip, xz, zstd; will check the other three
  before the PR"). False confidence is claiming to have traced or
  verified something the package does not show, or stating as fact
  what the repro contradicts. A deviation recorded in `plan.md` is
  honest; one that exists only in the diff is not.

## Comms

- **Where it lives (eval):** the "Candidate plan comment" section,
  read against "Thread highlights" (look for OWNER / MEMBER /
  COLLABORATOR comments and linked PRs) and the "Repo facts" block's
  "contribution policy" line (AI rules) and "bug reports" line.
- **Where it lives (live):** the draft `comment.md`, read against the
  live issue thread and the repo's CONTRIBUTING.md, AI_POLICY.md (or
  equivalent), and issue templates.
- **What good looks like:** thread-aware: when a maintainer has named
  a culprit, chosen a fix option, posted a test build, or asked for
  testing, the comment says it follows that direction or explains why
  it diverges; open PRs and prior art are acknowledged, not raced.
  Policy-aware: if the repo requires disclosure of all AI use (or in
  comments), the comment states the tool/extent and that a human
  reviewed it; if the policy only asks for disclosure in PRs, or has
  no AI rule, no disclosure is required in the comment. Boilerplate
  is a comment that could be pasted on any issue: no version, no
  repro reference, no mention of what the thread already said.
