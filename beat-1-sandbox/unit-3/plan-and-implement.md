# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

ronypy

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68#issuecomment-5986344820

I'm building and testing based on my reproduction of the issue and now planning the fix on my own fork from my own repro. It adds two things to what they do: a re-index-to-empty regression test, and a note on an empty-text edge case (below). 

**Cause:** `KeywordSearcher.index()` hands an empty tokenized corpus straight to `BM25Okapi`, which divides by `corpus_size == 0`. `search()` already returns `[]` when there is no index (`test_index_not_called_returns_empty` passes), so the fault is only in `index()`.

**Change** (branch `fix/68-keyword-search-empty-index` on my fork):
- In `rag/retriever/keyword_search.py`, `index()` returns early on an empty `chunks` list, setting `self.chunks = []` and `self.bm25 = None` so an empty re-index does not keep a stale model. The existing early return in `search()` covers the rest.
- In `tests/unit/test_keyword_search.py`, remove the `xfail(strict=True)` marker on `test_empty_index` (per CONTRIBUTING's seeded-bug note), and add one test: index one chunk, re-index with `[]`, and `search()` returns `[]`.
- Out of scope: `search()`, the tokenizer, `rank_bm25` itself, and other callers.

**Test:** re-run my repro command. On the branch it should exit 0 with no traceback, and `search("python")` after `index([])` should print `[]`. `pytest tests/unit/test_keyword_search.py -q` goes from `16 passed, 1 xfailed` (re-run on `f89c06f` today, without `--runxfail`) to `18 passed`. A one-chunk control should still return a scored result.

---

## Your branch

**Branch**

fix/68-keyword-search-empty-index

**Evidence**

Fork: https://github.com/ronypy/pathreview-ai301-fa26-s1/tree/fix/68-keyword-search-empty-index.
Windows 11, Git Bash, Python 3.11.9 in the repo's `.venv`, rank-bm25 0.2.2.

Before: branch at `f89c06f` (same as `main`), no changes.

```text
$ python -c 'from rag.retriever.keyword_search import KeywordSearcher; searcher = KeywordSearcher(); searcher.index([])'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week3\ai301-unit3-starter\pathreview-ai301-fa26-s1\rag\retriever\keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week3\ai301-unit3-starter\pathreview-ai301-fa26-s1\.venv\Lib\site-packages\rank_bm25.py", line 83, in __init__
    super().__init__(corpus, tokenizer)
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week3\ai301-unit3-starter\pathreview-ai301-fa26-s1\.venv\Lib\site-packages\rank_bm25.py", line 27, in __init__
    nd = self._initialize(corpus)
         ^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week3\ai301-unit3-starter\pathreview-ai301-fa26-s1\.venv\Lib\site-packages\rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
                 ~~~~~~~~^~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero
exit code: 1

$ python -c 'from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search("python", top_k=10))'
...
                 ~~~~~~~~^~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero
exit code: 1

$ pytest tests/unit/test_keyword_search.py -q
........x........                                                        [100%]
16 passed, 1 xfailed in 0.63s

$ pytest tests/unit/test_keyword_search.py -q --runxfail
=========================== short test summary info ===========================
FAILED tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index
1 failed, 16 passed in 0.61s
```

After: the same branch with the fix (commit `5db4d09`,
`fix(rag): handle an empty corpus in KeywordSearcher.index`).

```text
$ python -c 'from rag.retriever.keyword_search import KeywordSearcher; searcher = KeywordSearcher(); searcher.index([])'
2026-10-04 21:12:50 [info     ] keyword_index_built            chunk_count=0
exit code: 0

$ python -c 'from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search("python", top_k=10))'
2026-10-04 21:12:51 [info     ] keyword_index_built            chunk_count=0
2026-10-04 21:12:51 [warning  ] keyword_search_empty_index
[]
exit code: 0

$ python -c 'from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([{"id": 1, "text": "python programming"}]); print(s.search("python", top_k=10))'
2026-10-04 21:12:52 [info     ] keyword_index_built            chunk_count=1
2026-10-04 21:12:52 [info     ] keyword_search_complete        query_len=1 results_count=1
[{'id': 1, 'text': 'python programming', 'bm25_score': -0.2746530721670274}]
exit code: 0

$ pytest tests/unit/test_keyword_search.py -q
..................                                                       [100%]
18 passed in 0.56s

$ ruff check .
All checks passed!
$ black --check .
All done! ✨ 🍰 ✨
110 files would be left unchanged.
$ mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
$ pytest tests/unit -m unit -q
377 passed, 52 xfailed, 2 warnings in 14.65s
```

The one-chunk command is the control: non-empty indexing still returns a scored result after the fix.
The 52 remaining xfails belong to other issues' seeded bugs and are not part of this change.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, 20 packages: crashed before grading any package. On Windows, Python piped the prompt
   to the grader using the cp1252 encoding (`UnicodeEncodeError: 'charmap' codec can't encode
   characters`), so no score was produced. I re-ran with `PYTHONUTF8=1` and made no changes to
   the rubric, evidence guide, or procedure.
2. Full run, 20 packages: **19/20** (`categories: clear-accept 6/7  scope-creep 4/4
   thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`), `agreement: 19/20 scored items
   (bar: 18/20: PASS)`. This is the run saved in `eval-run.txt`.

**Package analysis**

pkg-14 (zellij-org/zellij#5174, category `clear-accept`). Gold label: `accept`. My rubric said
`reject`, with the note `failed: executable, honest-certainty`.

Why my rubric read it that way: the plan's Files section says "the client attach/reattach path
in `zellij-server` (session connection handling) and `zellij-client`'s terminal query issuance;
exact functions to be pinned in the PR after tracing the query issuance with debug logs". My
`executable` check fails a plan that "names no file or code site ... or leaves the choice between
approaches to build time", and only excuses "a bounded open question (with how it will be
resolved)". The grader read "exact functions to be pinned in the PR" as no code site named. It
also read the deferred Windows variant ("I cannot test Windows; the fix site may be shared") as
an unbacked claim, which failed `honest-certainty`.

The gold note says it is "honestly scoped-down ... defers the untestable Windows variant and says
so; arguable on the deferral, ready as scoped". The plan does commit to one approach (drain OSC
responses in the reattach handshake), names the crate and the path, and says how the function will
be pinned (debug logs it already has working). So it meets my "bounded open question" exception,
but my rubric words that exception too weakly for the grader to apply it at the crate/path level.

**Check rationale**

> | executable | The plan's change/approach and files sections. | Passes if a stranger could start work today without asking the author: the plan names the file(s) or the specific function/site to change and commits to one approach. Fails if it names no file or code site, says "somewhere", "investigate", "profile and see", "not sure which layer", "whichever is easier", or leaves the choice between approaches to build time. Stating a bounded open question (with how it will be resolved) does not fail this check. | required |

Why it reads that way: in the class activity, the sample rubric had no check for "could a
stranger start this?", and calib-02 ("poke around the editor code this weekend") is exactly that
failure. I wrote the condition around the outcome (could someone start work today) and listed
the hedge phrases that the unbuildable packages actually use (pkg-10 "profile and optimize",
pkg-17 "gocui? tcell? not sure", pkg-18 "whichever is easier"). That way the grader matches
concrete wording instead of judging how detailed the plan looks. I rejected a "names a specific
function" requirement, because terse clear accepts like calib-01 name a file and a callback, not a
function signature. I added the "bounded open question" exception so honest plans that admit one
unknown are not punished for it.

**Trade-offs**

The `executable` check gives up pkg-14. Its phrase list catches every unbuildable package
(`unbuildable 3/3`), but it also caught a clear accept that names a crate and a path but defers
the exact function. I accept that miss rather than loosening the check further. If "a named
crate/path is enough" were allowed, pkg-18's "recover() 'somewhere'" plan, which also names a
general area, could pass, and `unbuildable` is a 3-package category where each miss costs a lot.
Because I made no change after the 19/20 run, no other package's result moved. If I loosen it
later, I will re-run with `--only pkg-14,pkg-10,pkg-17,pkg-18`, using the three unbuildable
packages as canaries.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
