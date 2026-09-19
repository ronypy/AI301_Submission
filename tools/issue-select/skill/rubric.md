# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer active | repo-facts block: date of last commit on default branch | Last commit on default branch was within 90 days of today | required |
| Repo not abandoned-by-policy | repo-facts block: archived flag, README/CONTRIBUTING banners | Repo is not marked "archived" and no README/CONTRIBUTING statement says the project is unmaintained/deprecated | required |
| Repo in active use | repo-facts block: stars, open issue count, or download count — whichever is present | At least one of: stars ≥20, open issues ≥5, or downloads reported at all. If none of these fields are present in repo-facts, treat as pass (absence of data is not evidence of an unused repo) | required |
| Contribution policy allows AI-assisted work | repo-facts block: contribution policy / CONTRIBUTING.md statement on generative AI | Fail only if the policy states an outright ban on AI-generated code or documentation with no carve-out for assisted/reviewed use ("we do not accept AI-generated code or documentation"). Pass if the policy is silent, allows AI tools outright, or allows AI-assisted contributions on the condition the contributor understands, reviews, or tests the change (a ban on *fully* AI-generated work with assisted use still allowed also passes). If no contribution-policy evidence is present in the bundle, treat as pass | required |
| Scope is newcomer-sized | issue body: description, named files/areas, presence of a concrete deliverable; comment thread: signs of unresolved design debate or repeated abandoned attempts | Fail only if the issue is (a) an explicit index/list of many other issues (a "megaissue" or tracking issue), (b) an explicit invitation to pick an arbitrary, unbounded slice of the codebase with no fixed deliverable ("wherever it makes sense," "look around and see what you find"), (c) framed as an open design/architecture discussion with no settled spec — unresolved alternatives, "perhaps we could also...", or a comment thread showing years of debate and abandoned/stalled PR attempts — or (d) a feature request with an unresolved product decision (marked TBD, "alternatives: none identified," no maintainer-endorsed design). Otherwise pass: a concrete, itemized deliverable is newcomer-sized even if it touches several files (e.g. a docs task with a stated content spec, or a bug with named causes and a suggested fix), and a short issue naming one specific, bounded feature or defect is not penalized for lacking exhaustive detail | required |
| Not already claimed | comment thread: explicit claim ("I'll take this," "/claim," etc.) and its date relative to today; assignee field; linked PRs and their state | Fail if an assignee is currently set, an open PR references this issue number, or someone posted an explicit claim within the last 120 days with no sign of abandonment. A claim older than 120 days with no subsequent activity from the claimer — especially one followed by a stale-bot cycle with no reclaim — is a dead claim and does not fail this check. If the comment thread has no claim signal at all, this passes — silence is not a claim | required |
| Labeled for newcomers | issue body: labels list | Issue has a label matching "good first issue," "help wanted," "beginner-friendly," or repo-equivalent | preferred |
| Maintainer responsive | issue body + comment thread: any maintainer reply visible on this issue, or its timestamp relative to issue open date | A maintainer has replied on this specific issue, or replied to it within 30 days if a reply exists at all. If no maintainer-authored comment is visible in the bundle, this is `unclear` and contributes nothing (preferred, not required) | preferred |
| Clear instructions available | issue body + CONTRIBUTING.md (repo-facts block) | CONTRIBUTING.md or README documents a build/test command | preferred |
| Recent similar issue got merged | comment thread / repo-facts | At least 1 of the last 5 closed "good first issue"-labeled issues was closed by a merged PR. If this data isn't in the bundle, treat as `unclear` (no effect) | preferred |



## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->
Accept if every `required` check passes. Reject if any `required` check has clear failing evidence in the bundle.

`unclear` is handled per-check, not globally: for "not already claimed" and "repo in active use," the pass condition is written so that missing evidence resolves to pass (absence of a red flag is not a red flag). For "maintainer active," "repo not abandoned," and "scope," `unclear` still counts as fail — these need positive evidence one way or the other, and the bundle should contain it.

`preferred` checks never affect the verdict, including when `unclear`. Among accepted issues, rank by number of `preferred` checks that clearly passed (unclear does not count for or against ranking).
