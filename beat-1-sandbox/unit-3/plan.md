# Plan: issue #68, `KeywordSearcher.index([])` raises `ZeroDivisionError`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68
Builds on my repro comment on that issue (2026-09-26), reproduced on `main` at `f89c06f`.

## Diagnosis

`KeywordSearcher.index()` in `rag/retriever/keyword_search.py` passes its tokenized corpus
straight to `BM25Okapi` with no check for an empty list. `BM25Okapi.__init__` computes
`self.avgdl = num_doc / self.corpus_size`, and with `corpus_size == 0` that raises.

The repro evidence I rely on (my week-2 report, Windows 11, Python 3.11.9, rank-bm25 0.2.2).
The final `exit code: 1` line is from my local `report.md` run; the posted comment states the
exit code in prose:

```text
  File "...\rag\retriever\keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
  ...
  File "...\.venv\Lib\site-packages\rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero
exit code: 1
```

The crash happens inside `index()`, before `search()` is ever called. The repro also shows the
other 16 tests in `test_keyword_search.py` pass, including `test_index_not_called_returns_empty`:
`search()` already returns `[]` when `self.bm25` is `None` or `self.chunks` is empty. So the
fault is only that `index()` hands an empty corpus to the library; nothing in `search()` or in
`rank_bm25` needs to change.

## Scope

In scope:

- An empty-corpus guard at the top of `KeywordSearcher.index()`.
- Removing the `@pytest.mark.xfail(strict=True, reason="issue #68 (manifest H-01) ...")` marker
  from `tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index`, as
  `docs/CONTRIBUTING.md` ("Working on a seeded bug: remove its xfail marker") requires.
- One regression test in the same file for re-indexing to empty (see Changes, step 3).

Not in scope:

- `search()`, `_tokenize()`, and the rest of the module: they already behave correctly.
- Patching or wrapping `rank_bm25`, or changing the dependency version.
- The hybrid retriever and other callers of `KeywordSearcher`.
- Corpora whose chunks all have empty text (see Risks and unknowns): a separate failure that this
  issue does not report.

## Changes

All in `rag/retriever/keyword_search.py` and `tests/unit/test_keyword_search.py`, on branch
`fix/68-keyword-search-empty-index` in my fork.

1. In `KeywordSearcher.index()`, before tokenizing: if `chunks` is empty, set
   `self.chunks = []` and `self.bm25 = None`, log `keyword_index_built` with `chunk_count=0`,
   and return. Clearing `self.bm25` matters: if `index()` was called earlier with real chunks,
   an empty re-index must not leave the old BM25 model behind. `search()`'s existing early
   return then handles the empty case, so there is no second code path.
2. Delete the `xfail` decorator (lines 134-137) on `test_empty_index`. Without this the fixed
   test reports `XPASS(strict)` and CI fails.
3. Add `test_reindex_with_empty_clears_previous_index`: index one chunk, then `index([])`, then
   assert `search("python")` returns `[]`. This pins the stale-index behavior from step 1.
4. Run `make check && make test-unit` before pushing.

## Test plan

Before/after, using my week-2 repro command unchanged, from the repo root with the venv active:

```powershell
python -c 'from rag.retriever.keyword_search import KeywordSearcher; searcher = KeywordSearcher(); searcher.index([])'
echo "exit code: $LASTEXITCODE"
```

- Before (on `main`, already recorded): `ZeroDivisionError: division by zero`, `exit code: 1`.
- After (on the branch): no traceback, `exit code: 0`.

Then the observable behavior the issue asks for:

```powershell
python -c 'from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search("python", top_k=10))'
```

- After: prints `[]` and exits 0.

Covering tests:

- `pytest tests/unit/test_keyword_search.py -q` (without `--runxfail`). Before: my posted repro
  ran with `--runxfail` and got `1 failed, 16 passed`. I re-ran it without the flag on `main` at
  `f89c06f` while planning (2026-10-04) and got `16 passed, 1 xfailed in 1.36s`. After, with the
  marker removed and the new test added: `18 passed`, with no `xfail`/`XPASS`.
- Control: index one chunk and search `"python"`: still returns one result with a
  `bm25_score`. This confirms non-empty indexing is unchanged.
- `make check && make test-unit` is green.

## Prior art on the thread

Two open PRs already target this issue with the same early-return approach: #78
(Tiyatrotist, opened 2026-09-27) and #89 (yulijasso, opened 2026-10-05). No maintainer has
given direction on the thread. Per the course house rules, I build my own fix on my fork from
my own repro. My comment acknowledges both PRs and says I'm not racing them. My change adds the
re-index-to-empty regression test (Changes, step 3) and the empty-text note below. If either PR
merges first, I'll offer that test against it.

## Risks and unknowns

- **All-empty-text corpus (checked, left out of scope).** While planning I ran
  `index([{"id": 1, "text": ""}])` on `main`. It also raises, at a different place, because a
  non-empty list of empty documents gives an empty vocabulary:

  ```text
    File "...\rank_bm25.py", line 101, in _calc_idf
      self.average_idf = idf_sum / len(self.idf)
  ZeroDivisionError: division by zero
  ```
 My guard checks only for an empty
  `chunks` list, so this case still raises after the fix. I'll mention it in my comment and not fix it
  in this change unless a maintainer asks for it in this issue.
- **Callers relying on the old exception.** I have not yet searched for callers that catch
  `ZeroDivisionError` from `index()`. Before the PR I will `grep -rn "ZeroDivisionError\|\.index("`
  under `rag/` and `api/`, and note what I find in the PR description.
- **Python version.** My repro ran on Python 3.11.9. The guard uses nothing version-specific, and
  CI will confirm the project's configured versions.

## Deviations

Nothing changed; the plan held. The commit on `fix/68-keyword-search-empty-index` (`5db4d09`)
makes exactly the planned changes: the empty-list early return in `KeywordSearcher.index()`, the
`xfail` marker removed from `test_empty_index`, and the new
`test_reindex_with_empty_clears_previous_index`. The test plan came out as predicted: the repro
command exits 0, `search("python")` after `index([])` prints `[]`, the one-chunk control still
returns a scored result, and `test_keyword_search.py` went from `16 passed, 1 xfailed` to
`18 passed`.

Two items from Risks and unknowns are now settled:

- **Callers relying on the old exception:** `grep -rn "ZeroDivisionError" rag/ api/` finds nothing.
  The only other user of `KeywordSearcher` is `rag/retriever/hybrid.py`, which takes an instance
  and never calls `index()`, so no caller depends on the old exception.
- **All-empty-text corpus:** still out of scope as planned. `index([{"id": 1, "text": ""}])` still
  raises; it is raised as an open question in my plan comment.

Checks: `ruff check .`, `black --check .`, and `mypy` all pass (the same as on `main`).
`pytest tests/unit -m unit` gives `377 passed, 52 xfailed`. The remaining xfails belong to other
issues' seeded bugs and are untouched.
