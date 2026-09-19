# Where the conventions come from

Writing a task or a bug report into the tracker follows the team's own convention, and
this skill does not carry it. This file says where to look and what to do when there is
nothing to find. Read it once per session, not once per step. Setting the slot up is a
human job and is described in the README of the skills repository.

## Lookup order

Take the first that exists and stop.

1. **`<project>/.claude/standards/<topic>.md`** — the project's own. A client project
   with its own way of naming tasks overrides everything else, which is the point of
   looking here first.
2. **`~/.claude/standards/<topic>.md`** — the developer's own copy of the team
   conventions, usually a symbolic link per topic into a checkout they already have.
3. **A company knowledge base over MCP**, when one is configured — search it for the
   task and bug conventions.
4. **The fallback shipped with the skill**, listed below. It is general practice rather than
   anyone's house rules, so that somebody with none of the above still writes to a
   stated shape instead of to none. It is deliberately thin: a team's actual convention
   belongs in the slot above, not in a package everyone installs.

## Topics and fallback

| Topic | Covers | Fallback next to this file |
|---|---|---|
| `task` | How a task is named, and what belongs in its description | `task.md` |
| `bug` | The shape of a bug report | `bug.md` |

Read the one the work actually needs. A task and a bug are written differently, and the
difference lives in these two documents rather than in this skill.

## What to do when a step above is missing

Nothing, out loud, once. Say in one line which source answered — "convention from the
project", "no convention found, working to the skill's fallback" — and carry on. A
missing convention never stops the work, and it never becomes a question to the user
either: they know what they have installed.

Put the same line in the report, because a reader judging the task needs to know which
shape it was written to.

## When a convention contradicts the request

The request wins, always. A convention is how the team writes tasks; the request is what
this particular task is. Say that the two disagree — that is a decision for a person,
not for the session — and never quietly bend the request to fit a rule.
