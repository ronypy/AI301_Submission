# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68

**Verdict output**

```
Maintainer active: pass — last default-branch commit 2026-09-16T21:42:18Z, 3 days before today (2026-09-19)
Repo not abandoned-by-policy: pass — repo.archived: false; README carries no unmaintained/deprecated banner
Repo in active use: pass — open_issues_count: 71 (>=5)
Contribution policy allows AI-assisted work: pass — docs/CONTRIBUTING.md covers workflow/CI/branch naming; no statement on AI-generated code anywhere in the repo (silence passes)
Scope is newcomer-sized: pass — body names the exact function (KeywordSearcher.index()), the cause (tokenized empty corpus passed straight to BM25Okapi, unlike search() which already guards against it), and the fix target (remove the covering test's xfail marker) — a single bounded function fix
Not already claimed: pass — assignees: none; comments: 0
Labeled for newcomers: pass — labels: bug, good first issue, rag, tier-1
Maintainer responsive: unclear — 0 comments on the issue, no maintainer reply to grade
Clear instructions available: pass — docs/CONTRIBUTING.md lists make lint/typecheck/test-unit and npm test commands
Recent similar issue got merged: fail — repo has 0 pull requests in its entire history (GET /pulls?state=all returned empty); closed good-first-issue #43 has no linked PR

Verdict: accept
```

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
  "checks": [
    {"name": "Maintainer active", "grade": "pass", "evidence": "Last default-branch commit 2026-09-16T21:42:18Z, 3 days before today (2026-09-19)"},
    {"name": "Repo not abandoned-by-policy", "grade": "pass", "evidence": "repo.archived: false; no deprecation banner in README"},
    {"name": "Repo in active use", "grade": "pass", "evidence": "open_issues_count: 71 (>=5)"},
    {"name": "Contribution policy allows AI-assisted work", "grade": "pass", "evidence": "no AI policy stated anywhere in docs/CONTRIBUTING.md (silence passes)"},
    {"name": "Scope is newcomer-sized", "grade": "pass", "evidence": "Body names KeywordSearcher.index(), the ZeroDivisionError cause, and the xfail marker to remove — single bounded function fix"},
    {"name": "Not already claimed", "grade": "pass", "evidence": "assignees: none; comments: 0"},
    {"name": "Labeled for newcomers", "grade": "pass", "evidence": "labels: bug, good first issue, rag, tier-1"},
    {"name": "Maintainer responsive", "grade": "unclear", "evidence": "0 comments on the issue; no maintainer reply to grade"},
    {"name": "Clear instructions available", "grade": "pass", "evidence": "docs/CONTRIBUTING.md lists make lint/typecheck/test-unit and npm test commands"},
    {"name": "Recent similar issue got merged", "grade": "fail", "evidence": "repo has 0 pull requests in its entire history"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Only one run occurred. From `eval-run.txt`:

`agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

Issue `issue-12` (category: policy). Gold label: `reject`. My rubric's verdict: `reject` (agree).

The bundle's repo-facts block quotes the contribution policy directly: *"Meaningful human
interaction is the whole point of BookWyrm. We do not accept AI-generated code or
documentation. If you are unsure how something in BookWyrm works, please ask for help –
we are keen to help other humans to understand and contribute to the project."* Every
other family passes clean — the gold note itself says so: *"passes every liveness, scope,
and claim check; the repo's contributing docs ban AI-generated code and documentation
outright."* My rubric's "Contribution policy allows AI-assisted work" check fails exactly
this case by design: its pass condition only fails on "an outright ban on AI-generated
code or documentation with no carve-out for assisted/reviewed use," and this policy is
that ban verbatim, with no disclosure-and-review carve-out offered. Because that check is
`required`, the single failure rejects the issue regardless of every other check passing.

**Check rationale**

Quoted as written in `rubric.md`:

> Contribution policy allows AI-assisted work | repo-facts block: contribution policy /
> CONTRIBUTING.md statement on generative AI | Fail only if the policy states an outright
> ban on AI-generated code or documentation with no carve-out for assisted/reviewed use
> ("we do not accept AI-generated code or documentation"). Pass if the policy is silent,
> allows AI tools outright, or allows AI-assisted contributions on the condition the
> contributor understands, reviews, or tests the change (a ban on *fully* AI-generated
> work with assisted use still allowed also passes). If no contribution-policy evidence is
> present in the bundle, treat as pass | required

I wrote it this lenient on purpose. `references/evidence-guide.md` says most real policies
are conditions (disclosure, human review, testing) rather than bans, and my own workflow in
this course is AI-assisted, not AI-generated. A check that rejected on any AI-related
language at all would reject issues I am actually allowed to take; the check only fires on
an explicit, uncarved-out ban, which is the one case that actually rules me out.

**Trade-offs**

The lenient wording gives up catching soft discouragement. A policy that stops short of an
outright ban — "we'd prefer you not lean on AI tools, though we won't refuse a PR" — passes
under this check exactly as silence would, even though a real maintainer behind that
wording might be unhappy to see AI-assisted work land. The check only reads written policy
text, not maintainer sentiment expressed in threads, so it would also miss a maintainer who
closes AI-flavored PRs on sight without ever writing that rule down. I accept that miss:
the eval set's one `policy`-category item (`issue-12`) is the clean, explicit-ban case the
check is built to catch, and it caught it — full 20/20 agreement on this run, with
`categories: ... policy 1/1`, confirms nothing else regressed when I wrote the pass
condition this wide.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Issue #68 is a Python bug in the RAG search module (`KeywordSearcher.index()` /
   `BM25Okapi`), which matches my stated strengths (Python, SQL) and has no front-end
   component, which I wanted to avoid. It's a single guard-clause fix plus removing an
   `xfail` marker on an existing test — small enough to finish without the issue eating
   the rest of the week.
2. The verdict correctly flagged that the fix is fully bounded (one named function, one
   named failure mode, one test to un-mark) and that nothing blocks taking it — no
   assignee, no comments, no linked PR. What the rubric couldn't weigh is fit: it ranked
   #68 behind #72 and #69 on Python-relevance grounds alone, but I picked #68 anyway
   because it was the cleanest scope match to my actual comfort level for a first PR in
   an unfamiliar repo, over a marginal "closer to my strength" edge on the other two.
3. Low anticipated difficulty in claiming it: zero comments and no assignee mean nobody
   else has touched it, and the repo's house rule means even a stray classmate claim
   wouldn't block me. The main risk is that this is a brand-new course repo with no
   merged-PR history yet (`Recent similar issue got merged` failed for all four
   candidates), so I have no track record to gauge how quickly a maintainer will review
   once I open the PR.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
