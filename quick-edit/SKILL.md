---
name: quick-edit
description: "A small change the developer is watching: fix a bug in this file, rename it, add the field, adjust the copy. Reads the team standard for the touched stack, keeps the change surgical, runs the tests that already cover it, and answers with what the diff does not say. Trigger on «поправь», «почини», «переименуй», «добавь», «убери», «исправь», fix, rename, tweak — and on any change small enough that nobody will review it separately. NOT for a prepared task from the tracker: that is implement-task, which runs on its own and hands over a report."
compatibility: "Needs git. Written for Claude Code; the standards lookup reads ~/.claude/standards/."
allowed-tools: Read Write Edit Grep Glob Bash
---

# Quick edit

## What this is for

The developer asked for a change and is looking at the result right now. There is no
task to interpret, no report to write, and nobody downstream who needs evidence. What
this skill adds over just editing the file is that the team's conventions still apply
to a two-line change.

**If the request is a prepared task** — a tracker id, a statement with acceptance
criteria, anything that will be reviewed by someone else — stop and use
`implement-task` instead. Say which one you are using in a single clause, so the
developer can redirect you before the work rather than after.

## How

1. **The standard for the touched stack**, by the lookup in
   [standards.md](references/standards.md). One line about which source
   answered, then on with it. Skip this entirely for a change that no standard can
   have an opinion about — a typo in a comment, a version bump.

2. **Surgical.** Touch what the request names and nothing adjacent. Improving code you
   happened to read is the most common way a two-line change becomes a review.

3. **Reuse before writing.** If the codebase already has this, use it. If the language
   or framework already has it, use that.

4. **Tests that already cover it.** Run them when they exist and are cheap to run; say
   what happened. Do not write new tests here — a change that needs new tests is a
   task, and belongs to `implement-task`.

5. **Back to the developer, and nothing they could read off the diff.** Name the file
   and the line so they have somewhere to jump, then only what the diff does not say:
   an edge case the change leaves open, a test you could not run and why, something
   nearby that now looks wrong. No headings, no report, no recap of the change — they
   are about to read it. When there is nothing beyond the diff, one line is the whole
   answer.

   **A conclusion that changes nothing the developer does is either verified or left
   out.** "I read the framework and this case behaves as it did before" costs them a
   paragraph to learn that nothing happened. Either run the thing and report the
   result, or say nothing and let the unchanged behaviour stay unchanged. What survives
   this test is what would make them act differently: stop, look somewhere, decide
   something.

   This replaces a sentence count, which measured the wrong thing: four sentences of
   substance are not ceremony, and "Done, let me know if you want it committed" is
   ceremony in two. The count is kept elsewhere in this set of skills only where it bounds one
   field of a template, never a whole answer.

## Committing

Not part of this. When the developer wants the change committed they will say so, and
the `commit` skill owns the convention.

## Never

- Never improve code you happened to read. The request names what changes; everything
  adjacent stays as it is, however wrong it looks.
- Never write new tests here. A change that needs them is a task, and belongs to
  `implement-task`.
- Never commit. The developer is watching and commits when they are ready, with `commit`.
