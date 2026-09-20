---
name: create-mr
description: "Turn a committed branch into a merge request in GitLab through the glab CLI: push it, write the title and description from the commits, and give back the link. Trigger on «создай MR», «оформи мерж реквест», «запушь и создай MR», «на ревью», create MR, merge request, open a PR. Expects the work to be committed already — committing is the commit skill, and reviewing somebody else's MR is review-mr."
compatibility: "Needs git and `glab` authenticated against your GitLab instance."
metadata:
  source: "git@github.com:TechTechWizard/skills.git"
  update: "npx skills update"
  standard: "https://agentskills.io/specification"
allowed-tools: Read Grep Glob Bash
---

# Create a merge request

## Before anything

```sh
git branch --show-current
git status
git log <target>...HEAD --oneline
git diff <target>...HEAD --stat
```

`<target>` is the project's integration branch — `development` on most, but read the
project's own `CLAUDE.md` or its recent merge requests before assuming.

**Uncommitted changes stop this.** List them and ask: commit them first with the
`commit` skill, or proceed and leave them behind. Never decide that for the developer;
a file they were mid-way through is not something to sweep into a merge request.

## The task reference

From the branch name, `{TASK_ID}-{description}` → the first segment. When the branch
carries no id, ask for the task link rather than inventing a reference.

## Title and description

Title: `{TASK_ID}: {what it does}`, imperative, at most 72 characters.

Description, and nothing beyond these three parts:

```
<one or two sentences: what this merge request is for>

## Summary
- <change>
- <change>

Refs: https://app.clickup.com/t/{TASK_ID}
```

No test plan, no checklist, no list of files. The reviewer reads the diff; the
description exists to tell them what they are looking at and why.

## Create it

```sh
git push -u origin "<branch>"
glab mr create \
  --source-branch "<branch>" \
  --target-branch "<target>" \
  --title "<title>" \
  --description "$(cat <<'BODY'
...
BODY
)"
```

Use a HEREDOC for the body, or the formatting arrives mangled.

Then print the link, the title and the target branch.

## Never

- Never force-push. If the branch has diverged, say so and let the developer decide.
- Never a merge request from an unpushed branch — push first, and if the push fails,
  stop and report the failure rather than creating something that points at nothing.
- Never merge it yourself.
