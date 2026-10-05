# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2; paste your filled week-2 voice-guide.md here, whole. It is not
re-authored and it is not graded as new work this week.

Then reread it with the plan comment in mind. Your claim and repro
comments promised and reported; a plan comment commits you to an
approach in front of the people who maintain the code. If your rules
do not cover that register (for example: how you state an approach you
are not certain of, or how you respond when a maintainer already
suggested a direction), extend the guide with what it needs. Extending
is allowed and encouraged; starting over is not required.

Live mode reads this file before your plan comment goes out and
reports any rule your draft breaks. Eval mode ignores it entirely,
because your voice is yours and carries no gold labels.
-->

## Who I am in threads

<!-- Paste your week-2 section here. -->
I'm a first-time contributor to most repos I touch here — I say so, plainly, the first time I comment on an issue. I'm not promising a merge; I'm showing up with a repro report and, sometimes, a fix attempt. Readers should expect someone careful and specific, not someone selling enthusiasm.


## Rules I write by

<!-- Paste your week-2 rules here, wrong/right pairs and all. Add any
rule the plan-comment register needs that your week-2 comments did
not. -->
### Rule: No promised timelines

I don't commit to a delivery date I don't control. Maintainers review on
their own schedule, and a promised date I miss costs more trust than no
date at all.

- Wrong: "I will fix this by tomorrow, promise!!"
- Right: "Repro report on the way."

### Rule: Name the version and the behavior

A comment has to be identifiable as being about *this* issue and nothing
else. "This bug" could be any of fifty open issues; the version and the
specific behavior can't be.

- Wrong: "I can fix this bug, please assign me."
- Right: "v1.20.0 ignores `--style`, exactly as described. Picking this up."

### Rule: Say it like I would out loud

If I wouldn't say it to someone's face, I don't type it into a comment box.
No exclamation-point enthusiasm standing in for content, no "amazing
project!!" filler.

- Wrong: "Very interested in this amazing project!! Please assign!!"
- Right: "Picking this up: v1.20.0 ignores `--style`, exactly as described."

### Rule: Report first, fix second

My first comment on an issue is a claim plus evidence, not a claim plus a
finished patch I haven't opened yet. I say what I did and what's next, in
that order.

- Wrong: "Fixed! PR incoming."
- Right: "Reproduced on v1.20.0, log attached. Repro report on the way; I'll
  attempt a fix next."

### Rule: Flag that I'm new, once

I say I'm a first-time contributor the first time it's true in a thread,
then I stop mentioning it — it's context once, not a running disclaimer.

- Wrong: (three comments in a row each opening with "as a beginner...")
- Right: "This is my first contribution, so flagging that." (stated once,
  then dropped)


## Things I never post

<!-- Paste your week-2 list here; extend it if planning tempts you
toward new ones (overpromised timelines are the classic). -->
- A merge or fix promise before I've written the repro report.
- A specific date or "by tomorrow" / "by tonight" — I don't control review
  timelines.
- "+1", "any update?", or other content-free bumps on someone else's issue.
- Generic enthusiasm with no version or behavior named ("amazing project!!",
  "love this repo!!").
- A claim of reproduction with no attached log, steps, or environment.