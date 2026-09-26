# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
**Where it lives**
In an eval bundle: the `Environment:` block at the top of the repro report,
plus the repo-facts block (which states what version/OS the issue targets,
or what the issue body itself reports). In live mode: the draft's
environment line, checked against the repo's README/CONTRIBUTING (which
version fields they ask for) and the issue thread (what the reporter
originally stated).

**What good looks like**
The tool version, OS, and commit or tag are all named, and the version
either matches what the issue targets or the report explicitly calls out
the difference ("issue reported on 1.18.0, reproducing on 1.20.0"). A
version alone, with no OS or commit, is not sufficient.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->
**Where it lives**
In an eval bundle: the `Steps` section of the repro report, read against
the issue body's own reported command. In live mode: the draft's steps,
checked against the repo's CONTRIBUTING.md or README for the documented
build/run process.
 
**What good looks like**
Steps start from a known, reproducible starting state (a fresh clone or a
named checkout) and end at the issue's exact reported command, using only
commands or files introduced earlier in the same steps — nothing that
assumes local state a stranger wouldn't have. A stranger following the
steps exactly, with nothing else, should land at the same command.
 
When a step creates a helper or test file (a minimal `env.yml`, a
`repro.mjs`), the file does not need to be dumped as a literal block to
pass. It passes if either: (a) its content is shown inline, or (b) it is
described precisely enough — naming every key/section that matters to the
bug — that a stranger could reconstruct exactly what's needed, or (c) it
explicitly points at input already given verbatim in the issue body (e.g.
"repro.mjs contains the issue's two `prettier.format` calls, shape A and
shape B as specified"). Only fail this when the description is vague
("configured it similarly," "set up a typical project," "used my usual
test file") and a stranger genuinely could not tell what to write.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
**Where it lives**
In an eval bundle: the attached log/output block in the repro report,
compared against the issue body's stated symptom (error message, exit
code, panic vs. clean failure). In live mode: whatever the draft attaches
or pastes — a terminal log, a screenshot, a linked CI run — compared
against the issue thread's own description of the failure.

**What good looks like**
The artifact is the exact output of the command named in Steps, and its
failure signature matches the issue's — same error type, same exit code
family. An artifact that shows *some* failure but a different one than the
issue reports is showing an adjacent bug, not this one, and doesn't count
as behavior shown.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->
**Where it lives**
In an eval bundle: the report's opening claim or verdict line, checked
against its own Environment/Steps/Behavior-shown evidence above. In live
mode: the draft comment's claim, checked the same way against what it
actually attaches.

**What good looks like**
The claim states exactly what the evidence supports — including an honest
"could not reproduce," which is a complete and postable answer, not a
failure. A report claiming success without a matching Behavior-shown entry,
or overstating certainty beyond what the log shows, fails this even if
every other section is well-written.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
**Where it lives**
In an eval bundle: the draft comment text meant for the issue thread,
checked against the issue body and, where present in the repo-facts block,
the repo's CONTRIBUTING.md or issue template (including any AI-assistance
disclosure requirement). In live mode: the actual comment about to be
posted, checked against the live issue thread and the repo's own
CONTRIBUTING.md/CODE_OF_CONDUCT for stated norms.

**What good looks like**
The comment names the specific version and behavior rather than "this
bug," states the next step as a report or fix attempt rather than a merge
promise, and discloses AI assistance or first-time-contributor status
where the repo's policy or the fact itself calls for it. Boilerplate
("great project!!", "+1") or a promised timeline reads as generic even if
the report underneath it is solid — this check grades the words meeting
the repo, not the proof itself.
