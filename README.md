# skills

Agent Skills for working the way we actually work: tasks in ClickUp, merge requests in
GitLab, a development step that ends at something you can look at, and a diagnostic pass
over a project's stands. Seven skills in the [Agent Skills](https://agentskills.io) format,
installed with `npx skills`, usable from Claude Code, Cursor and every other agent the
installer supports.

## Install

```sh
npx skills add git@github.com:TechTechWizard/skills.git -g -a claude-code -s '*'
```

`-g` installs for the user rather than for one project; `-a` names the agent (omit it to
be asked, `'*'` for every agent on the machine); `-s '*'` takes all seven, or name the ones
you want. The repository is private, so two things have to be true before the first
command works: you have been granted access, and your SSH key is set up with GitHub
(`ssh -T git@github.com` should greet you by name). The short form
`TechTechWizard/skills` also works, but it clones over HTTPS and needs a stored GitHub
credential, which most machines do not have — use the SSH address.

Update later with `npx skills update`, remove with `npx skills remove`.

## What is here

| Skill | You say | What happens |
|---|---|---|
| `clickup` | «прочитай задачу», a task id or link, «заведи таску», «багрепорт» | The card, comments and linked tasks read into a statement in your own words; or a task or bug report written to your team's convention and read back to verify. Needs the `clickup` CLI, which the skill offers to install on first use. |
| `quick-edit` | «поправь», «почини», «переименуй» | A small change you are watching, to the team's conventions, and back only what the diff does not say. |
| `implement-task` | «реализуй задачу», «запускай в работу» | A prepared task run on its own: standards, code, two rounds of self-review, verification, commits, handover report. |
| `commit` | «закоммить» | One commit by the convention. With no details given, it proposes a message and waits. |
| `review-mr` | «сделай ревью», a merge-request link | Somebody else's merge request read against the checklist, with the findings posted as comments. Never edits the code. |
| `create-mr` | «создай MR», «на ревью» | A committed branch pushed and turned into a merge request. |
| `project-healthcheck` | «хелсчек», «проверь стенд» | A read-only diagnostic pass over a project's stands, driven by `<project>/.claude/healthcheck.md`. |

The line that matters is between `quick-edit` and `implement-task`. The first is for a
change you are looking at right now; the second is for work someone else will review, and
it costs accordingly — an intake, two review cycles, a report. Asking the heavy one for a
typo is how people conclude that skills are in the way.

`review-mr` is called that, and not `code-review`, because Claude Code ships a built-in
skill named `code-review` and a personal skill with the same name hides it from the list —
and both `review-mr` and `implement-task` call the built-in one to hunt bugs.

## Your team's rules do not travel in here

None of these skills ships anybody's standards. `quick-edit`, `implement-task`, `commit`
and `review-mr` look the team's code standards up; `clickup` looks the task and bug
conventions up. Both use the same slot, in this order: the project's own
`<project>/.claude/standards/<topic>.md`, then your `~/.claude/standards/<topic>.md`, then
a company knowledge base over MCP if one is configured, then a thin general-practice
fallback that ships with the skill. Code topics are `general`, `laravel`, `frontend`,
`css`, `code-review`, `commit`; the tracker topics are `task` and `bug`.

A missing standard never stops the work: the skill says which source answered, once, and
carries on.

To point the slot at documents you already have, link them one topic at a time:

```sh
mkdir -p ~/.claude/standards
ln -s ~/Work/coding-standards/docs/quality-standards.md    ~/.claude/standards/general.md
ln -s ~/Work/coding-standards/docs/code-review.md          ~/.claude/standards/code-review.md
ln -s ~/Work/coding-standards/docs/work-with-repository.md ~/.claude/standards/commit.md
ln -s ~/Work/<your-docs>/task-convention.md                ~/.claude/standards/task.md
ln -s ~/Work/<your-docs>/bug-convention.md                 ~/.claude/standards/bug.md
```

One link per topic rather than one link for the whole directory: the topics usually come
from more than one repository, and the filenames rarely match the topic names. For a
project whose rules differ, commit `<project>/.claude/standards/<topic>.md` into the
project — then everyone on it gets the same answer, including someone with nothing set up.

## What the skills expect

Claude Code or another supported agent, and nothing else mandatory. Each skill degrades
rather than refuses:

- `clickup` needs the `clickup` CLI from
  [claude-work-tools](https://github.com/TechTechWizard/claude-work-tools) and a personal
  ClickUp API token; the skill explains and installs the CLI once you agree, the token only
  you can create.
- `create-mr` and `review-mr` need `glab`, authenticated against your GitLab.
- `implement-task` uses the built-in `code-review`, `security-review`, `simplify` and `run`
  skills when they are there, and says in the handover which reviewers actually ran.
- `project-healthcheck` needs a `healthcheck.md` the project writes about itself.

## How this repository is laid out

```
skills/<name>/          one directory per skill: SKILL.md, protocols/, references/
shared/                 the canonical copy of references several skills carry
scripts/sync-shared.sh  copies shared/ into every skill that carries the file; --check
evals/                  eval cases, run with `claude plugin eval .`
```

The Agent Skills format makes every skill self-contained — an installer copies the skill's
directory and nothing else — so four references (`standards.md`, `review-checklist.md`,
`commit-convention.md`, `verify-template.md`) exist once per skill that needs them. Edit
the copy in `shared/`, run `scripts/sync-shared.sh`, and the check refuses a commit where
the copies have drifted.

**Do not edit an installed skill in place.** `npx skills update` overwrites it without
asking. Disagree by writing your own skill under another name, or by putting a project
standard into the slot above.

## Reporting something

Open an issue, or send a pull request. There is one maintainer and no promise about how
fast a change lands, which is worth knowing before you wait on one.
