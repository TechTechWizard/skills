# Where the standards come from

Every skill in this set that touches code reads the team's standard for what it is about
to touch. This file says where to look, what to open and what to do when there is nothing
to find. Read it once per session, not once per step. Filling the slot is a human job and
is described in the README of the skills repository.

## Where to look

**The slot is a directory, and what is in it is the standard.** There is no list of
expected names anywhere in this skill, deliberately: the listing is the truth, and a
direction the team documented yesterday shows up by someone putting a file in the folder,
not by a skill learning its name first.

Two folders, in this order:

1. **`<project>/.claude/standards/`** — the project's own, committed into the repository.
   A client project with its own code guide overrides everything else, which is the point
   of looking here first.
2. **`~/.claude/standards/`** — the developer's own.

For the same subject the project's folder wins and the lookup stops there. Subjects the
project says nothing about still come from the developer's folder: a project that ships
its own CSS guide has not thereby cancelled the team's commit convention.

What lies inside is up to the person who set it up. It can be a document, a symbolic link
to a document, or a symbolic link to a whole directory of documents in a checkout they
already have. All three read the same way — open the file, or list the directory and open
what is in it.

When neither folder answers for the stack being touched, and a company knowledge base is
configured over MCP, search that. If a fallback ships with this skill, it comes last; it
is general practice rather than anyone's house rules, and its only job is that a developer
with nothing set up still works to a stated bar instead of to nothing.

## What to open

Read only what the work actually touches, and decide that from the names you see. A name
that matches the stack being edited, a name that matches the activity being performed —
that is the whole of the rule. Everything else in the folder belongs to work this session
is not doing.

A directory entry is read the same way: list it, open the documents whose names match, and
leave the rest. Documentation repositories nest their documents (`common/`, `nextjs/`,
`react-native/`) and the nesting says what a document is for, so a mobile document is not
opened for a web change. An `index.md` in such a repository is usually an introduction
rather than a table of contents — opening it costs a read and answers nothing, and having
opened it is not having read the standard.

**Reading means opening the document before the change, not searching it after.** A
search returns the words already in your head, so it can only confirm what you remembered
the standard to say; the rule the standard exists to give you is the one you did not know
to search for, and it sits in a paragraph no grep returns. Open the document that matches
the work — or the section of it that covers what is being touched — read it, and then
edit. A search across a whole slot entry is worse than a search inside one document: it
runs through every subdirectory at once, so a mobile rule matches on a web change and
arrives without the heading that would have said whose rule it is.

Never paste a whole document into context. Read the part that applies, of a document you
have opened.

## Say which source answered

One line, out loud, once: "standards from the project", "standard from
`~/.claude/standards/frontend/common-стиль-кода.md`", "nothing in the slot for this stack,
working to the skill's fallback". Put the same line in the handover report when the skill
writes one, because a reader judging the result needs to know which bar it was measured
against.

Naming the file rather than the folder matters, and when the slot entry is a directory the
file is the document inside it: `~/.claude/standards/frontend/nextjs/<document>.md`, not
`~/.claude/standards/frontend/`. The line is a claim that a document was opened, and a
reader checks it against the files the session actually opened; a folder that exists
proves nothing about what was read, and a checkout nobody has pulled answers with last
year's rules while looking entirely current.

## When there is nothing to find

Nothing stops. Say in one line that the slot holds nothing for this stack and carry on to
the fallback, or to plain good practice if there is no fallback. A missing standard never
becomes a question to the developer either: they know what they put in their own folder.

## When a standard contradicts the task

The task wins, always. A standard is how the team writes code; the task is what the
developer decided this change is. Record the contradiction in the report — that is a
decision for a person, not for the session — and never edit the task statement to fit a
rule.
