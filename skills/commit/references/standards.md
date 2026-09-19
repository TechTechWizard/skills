# Where the standards come from

Every skill in this set that touches code reads the team's standard for the stack it is
about to touch. This file says where to look and what to do when there is nothing to
find. Read it once per session, not once per step. Setting the slot up is a human job
and is described in the README of the skills repository.

## Lookup order

Take the first that exists and stop.

1. **`<project>/.claude/standards/<topic>.md`** — the project's own. A client project
   with its own code guide overrides everything else, which is the point of looking here
   first.
2. **`~/.claude/standards/<topic>.md`** — the developer's own copy of the team
   standards, usually a symbolic link per topic into a checkout they already have.
3. **A company knowledge base over MCP**, when one is configured — search it for the
   standard of the touched stack and for the general quality standard.
4. **The fallback shipped with the skill**, listed below. It is general practice rather than
   anyone's house rules, so that a developer with none of the above still works to a
   stated bar instead of to nothing. It is deliberately thin: a team's actual standards
   belong in the slot above, not in a package everyone installs.

## Topics and fallback

`<topic>` is the stack or the activity. Read only the ones the work actually touches.

| Topic | Covers | Fallback next to this file |
|---|---|---|
| `general` | The quality bar that holds whatever the stack is | — |
| `laravel`, `frontend`, `css` | The stack being touched | — |
| `code-review` | What a reviewer looks for | `review-checklist.md` |
| `commit` | How a commit message is formed | `commit-convention.md` |

A topic with no fallback and nothing in the slot is simply unanswered — say so and carry
on.

## What to do when a step above is missing

Nothing, out loud, once. Say in one line which source answered — "standards from the
project", "no standard found, working to the skill's fallback" — and carry on. A
missing standard never stops the work, and it never becomes a question to the developer
either: they know what they have installed.

Put the same line in the handover report when the skill writes one, because a reader
judging the result needs to know which bar it was measured against.

## When a standard contradicts the task

The task wins, always. A standard is how the team writes code; the task is what the
developer decided this change is. Record the contradiction in the report — that is a
decision for a person, not for the session — and never edit the task statement to fit a
rule.
