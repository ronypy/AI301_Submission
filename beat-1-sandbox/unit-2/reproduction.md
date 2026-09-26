# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

ronypy

---

## Posted upstream

**Claim comment**

<!--[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]
-->
https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68#issuecomment-5849171682

Hi,
I would like to contribute to this issue #68. It is my First contribution attempt. 

As the issue describes, on main (f89c06f), KeywordSearcher.index([]) passes the empty corpus straight to BM25Okapi, and rank-bm25's _initialize raises ZeroDivisionError. That happens before search() is ever reached, and search() already returns [] when no index is built. The covering test is tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index, marked xfail(strict=True) for H-01.

Next I'll post a repro report (environment, steps, traceback, and a one-chunk control run) against rag/retriever/keyword_search.py before touching any code.


**Reproduction comment**
<!--
[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]
-->

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68#issuecomment-5849973602

I ran the reproduction commands and checked the output below.

On `main` at `f89c06f`, calling `KeywordSearcher().index([])` raises
`ZeroDivisionError: division by zero` from inside `rank_bm25` (`BM25Okapi`), and the process exits with code 1.
This is the failure the issue describes.

## Environment

| Item | Value |
|---|---|
| OS | Windows 11 Pro, 10.0.26200 (`ver`: `Microsoft Windows [Version 10.0.26200.9457]`) |
| Shell | Windows PowerShell 5.1 |
| Python | 3.11.9 (in a fresh `.venv`) |
| rank-bm25 | 0.2.2 |
| structlog | 26.1.0 |
| pytest | 9.1.1 |

Run these in PowerShell, starting from any empty folder:

```powershell
git clone https://github.com/codepath/pathreview-ai301-fa26-s1.git
cd pathreview-ai301-fa26-s1

python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -e ".[dev]"               # the install step `make setup` uses
python -c 'from rag.retriever.keyword_search import KeywordSearcher; searcher = KeywordSearcher(); searcher.index([])'
```

Optional: run the issue's covering test with its `xfail` marker ignored:

```powershell
pytest tests/unit/test_keyword_search.py -q --runxfail
```

## Expected behavior

`index([])` should build an empty index without raising. A later `search(...)` should then return `[]`, which is what
`search()` already does when no index exists (`tests/unit/test_keyword_search.py::test_empty_index` asserts this).


## Actual behavior

`index([])` passes an empty tokenized corpus to `BM25Okapi`. `BM25Okapi` computes `num_doc / self.corpus_size` with
`corpus_size == 0` and raises `ZeroDivisionError`. The script exits with error.

### Output (exact, from the `python -c` command above)

```text
(.venv) PS C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week2\pathreview_clone\pathreview-ai301-fa26-s1> python -c 'from rag.retriever.keyword_search import KeywordSearcher; searcher = KeywordSearcher(); searcher.index([])'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week2\pathreview_clone\pathreview-ai301-fa26-s1\rag\retriever\keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week2\pathreview_clone\pathreview-ai301-fa26-s1\.venv\Lib\site-packages\rank_bm25.py", line 83, in __init__
    super().__init__(corpus, tokenizer)
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week2\pathreview_clone\pathreview-ai301-fa26-s1\.venv\Lib\site-packages\rank_bm25.py", line 27, in __init__
    nd = self._initialize(corpus)
         ^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week2\pathreview_clone\pathreview-ai301-fa26-s1\.venv\Lib\site-packages\rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
                 ~~~~~~~~^~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero
```

### Covering test (`pytest tests/unit/test_keyword_search.py -q --runxfail`)

```text
(.venv) PS C:\Users\USER\Desktop\101\CodePath\Codepath_AI301\Week2\pathreview_clone\pathreview-ai301-fa26-s1> pytest tests/unit/test_keyword_search.py --runxfail -q
........F........                                                                                                      [100%]
========================================================= FAILURES ==========================================================
___________________________________________ TestKeywordSearcher.test_empty_index ____________________________________________

self = <tests.unit.test_keyword_search.TestKeywordSearcher object at 0x00000239F6E81C90>
searcher = <rag.retriever.keyword_search.KeywordSearcher object at 0x00000239F6E878D0>

    @pytest.mark.xfail(
        strict=True,
        reason="issue #68 (manifest H-01): BM25 keyword search raises ZeroDivisionError on an empty index",
    )
    def test_empty_index(self, searcher):
        """Test searching on empty index."""
>       searcher.index([])

tests\unit\test_keyword_search.py:140:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
rag\retriever\keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
.venv\Lib\site-packages\rank_bm25.py:83: in __init__
    super().__init__(corpus, tokenizer)
.venv\Lib\site-packages\rank_bm25.py:27: in __init__
    nd = self._initialize(corpus)
         ^^^^^^^^^^^^^^^^^^^^^^^^
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <rank_bm25.BM25Okapi object at 0x00000239F6E87B10>, corpus = []

    def _initialize(self, corpus):
        nd = {}  # word -> number of documents with word
        num_doc = 0
        for document in corpus:
            self.doc_len.append(len(document))
            num_doc += len(document)

            frequencies = {}
            for word in document:
                if word not in frequencies:
                    frequencies[word] = 0
                frequencies[word] += 1
            self.doc_freqs.append(frequencies)

            for word, freq in frequencies.items():
                try:
                    nd[word]+=1
                except KeyError:
                    nd[word] = 1

            self.corpus_size += 1

>       self.avgdl = num_doc / self.corpus_size
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^
E       ZeroDivisionError: division by zero

.venv\Lib\site-packages\rank_bm25.py:52: ZeroDivisionError
================================================== short test summary info ==================================================
FAILED tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index - ZeroDivisionError: division by zero
1 failed, 16 passed in 1.05s
```
Without `--runxfail`, the suite reports `16 passed, 1 xfailed`. The failure is hidden by
`@pytest.mark.xfail(strict=True, reason="issue #68 (manifest H-01) ...")` at `tests/unit/test_keyword_search.py:134`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1 (2026-09-26T18:03:49Z, rubric.md `sha256:dfb1ca8275e0f26b`):
   `agreement: 17/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`
2. Run 2 (2026-09-26T18:22:30Z, rubric.md `sha256:0d61cf3ea9ddcb5a`):
   `agreement: 19/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`

Final score: **19/20**. Run 2 is the committed `eval-run.txt`.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#13604, category `disclosure`). My rubric decided **accept**, and
the gold label is **reject**. It is the only disagreement in my final run:
`pkg-20  reject  accept   NO     graded accept`.

On the proof checks the package really is strong, and the gold note agrees ("excellent repro
on every proof check"). The environment line gives `ghostty 1.3.1 (release build, Fedora 42
RPM), GTK backend, GNOME 48 (Wayland)`. The steps start from a one-line config file and end at
the issue's own `CSI ? 996 n` query. The artifact `^[[?997;2n` is the issue's wrong
light-mode answer, and a conditional-theme control answers `^[[?997;1n`. So every check in
the proof families passed.

The package fails on the repo facts: ghostty's policy says "All AI usage in any form must be
disclosed, stating the tool used and the extent of the assistance". The claim comment ("Hi!
I'd like to take this one as a first Ghostty contribution...") says nothing about AI. My
rubric has a check for exactly this, "AI-assistance disclosed when required". Its pass
condition only fires "If the repo's stated policy explicitly requires disclosing AI usage".
It never says whether the package itself should be treated as AI-assisted work. The gold
label assumes it is ("course packages are treated as AI-assisted work"). My check leaves that
open, so the grader could read a comment that never mentions AI as a comment with nothing to
disclose. The package passed, and the verdict rule accepted it. The fix belongs in the
rubric: state that every package is AI-assisted, so a strict policy plus no disclosure
statement is a fail.

**Check rationale**

> | Steps are minimal and re-runnable | Report's "Steps" section | Steps start from a stated, reproducible starting point (fresh clone/checkout, or a file created within the steps) and end at the issue's exact reported command. A helper/test file created mid-steps does not need to be shown as a literal block — a precise structural description, or a pointer to input already given verbatim in the issue, is sufficient. Only fail on genuinely vague descriptions ("configured it similarly," "my usual test file") that leave a stranger unable to tell what to write | required |

In Run 1 this check was stricter. It read "steps a stranger can follow" as "every file the
steps create must be shown". That rejected two gold accepts, both with the same note:
`pkg-05  accept  reject   NO     failed: Steps are minimal and re-runnable` and
`pkg-12  accept  reject   NO     failed: Steps are minimal and re-runnable`. pkg-05 builds a
minimal `env.yml`. pkg-12 runs the issue's two `prettier.format` shapes. Neither pastes the
helper file as a literal block, but a stranger can still write it: the report names every
key that matters, or points at input the issue already gives verbatim. So I kept "start from
a reproducible state and end at the issue's exact command" and dropped the literal-block
requirement. I added the two accepted alternatives, structural description and pointer to
the issue. I also named what still fails ("configured it similarly," "my usual test file")
so the looser wording can't pass a vague setup. The evidence guide's Steps section got the
same three-way rule, (a) inline, (b) precise description, (c) pointer to the issue. That way
the grader reads the same rule in both files.

**Trade-offs**

The revision flipped exactly the two packages it was aimed at: pkg-05 and pkg-12 went from
reject to accept, which matches gold. clear-accept went from `6/8` to `8/8`. I know nothing
else changed because the other 18 scored items got the same verdict in both runs, pkg-20's
wrong accept included. The categories most at risk from a looser steps check are
`unfollowable-comms 3/3` and `wrong-target 4/4`, and both held in Run 2. pkg-18, whose
reproduction lives in a private monorepo with an unshared config, is still rejected: "input
a stranger cannot see" is not a precise description or a pointer to the issue. The case I
accept this check will miss is a report whose structural description *sounds* precise but
leaves out the one key that triggers the bug. The check trusts that the description names
"every key/section that matters", and the grader cannot always tell what matters without
running it. The behavior-matches check is the backstop there: if the key is missing, the
artifact won't show the issue's failure.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
