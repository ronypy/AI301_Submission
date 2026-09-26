# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->
I'm a first-time contributor to most repos I touch here — I say so, plainly, the first time I comment on an issue. I'm not promising a merge; I'm showing up with a repro report and, sometimes, a fix attempt. Readers should expect someone careful and specific, not someone selling enthusiasm.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

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

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
- A merge or fix promise before I've written the repro report.
- A specific date or "by tomorrow" / "by tonight" — I don't control review
  timelines.
- "+1", "any update?", or other content-free bumps on someone else's issue.
- Generic enthusiasm with no version or behavior named ("amazing project!!",
  "love this repo!!").
- A claim of reproduction with no attached log, steps, or environment.