---
name: review-mr
description: "Review somebody else's merge request: read the diff against the team checklist, hunt bugs with the built-in reviewers, and post the findings as comments on the merge request. Trigger on «сделай ревью», «посмотри MR», «проверь мерж реквест», «отревьюй», review this MR, code review — and on any GitLab merge-request URL, even with no other instruction. Read-only on the code: it describes fixes, it never applies them. Reviewing your own fresh work is part of implement-task instead."
compatibility: "Needs git and `glab` authenticated against your GitLab instance. Written for Claude Code: it invokes the built-in code-review and security-review skills through the Skill tool."
allowed-tools: Read Grep Glob Bash Skill
---

# Code review

You are reading somebody else's work. You do not change it: a reviewer who edits the
code takes the decision away from its author and hides the disagreement.

## 1. The standard

Find the team's review standard and the standard for the touched stack, by the lookup
in [standards.md](references/standards.md). The fallback when nothing closer
exists is [review-checklist.md](references/review-checklist.md).

Say in one line which source answered. A review measured against a bar the author
cannot see is an argument waiting to happen.

## 2. The diff, and only the diff

```sh
glab mr view <iid> --comments
git fetch origin <source-branch>
git diff <target>...<source-branch>
```

Review what changed. Code the author did not touch is not in scope, however wrong it
looks — if it matters, it is a separate task, and the report says so under its own
heading.

## 3. Hunt bugs with the built-in reviewers

Invoke the built-in `code-review` skill through the Skill tool on the merge request's
changes — fetch the source branch first, then pass the branch or commit range as the
target, **always at `high` effort** (`high <branch>`). Never rely on the skill's
sticky last-used level: it carries over from whatever was typed last for unrelated
reasons, and review depth has to be the same on every merge request.

Two rules when using it here:

- **Never pass `--comment` or `--fix`.** Posting belongs to this skill and targets
  GitLab; `--comment` targets GitHub pull requests. And a reviewer does not change code.
- **Its findings are raw input, not the review.** Every one passes the checklist and
  the standard first. A finding that contradicts the standard or the task's stated
  scope is dropped or reframed, not forwarded.

When the diff touches authentication, authorization, parsing of untrusted input, file
uploads or secrets, also run the built-in `security-review` skill on the branch. Same
two rules; its findings land in the security category.

When the merge request implements a Figma mockup, pull the reference through the
official Figma MCP (`get_screenshot`, and `get_design_context` for tokens) and compare
the implementation against it. A visual deviation from the mockup is a finding like
any other.

## 4. Read it yourself, by the checklist

The built-in reviewers are good at correctness and blind to convention: architecture,
the team's idioms and the quality rules are entirely yours. Go through the checklist
categories — implementation, logic, error handling and logging, dependencies and
impact, security and privacy, performance, API usability, testing, readability — and
merge the tools' confirmed findings into the matching ones.

Quality rules worth checking explicitly, because they are invisible to a bug hunter:
unfinished screens carry stubs, unimplemented buttons say so to the user, stubs are
reusable rather than copied, unfinished code carries a clear `TODO:`, and new
functionality on a production product sits behind a feature flag.

## 5. Two artifacts, never one

The **report** goes to the person who asked for the review: summary, every finding,
checklist status. The **comments on the merge request** go to its author and are a much
smaller thing. Never paste the report into GitLab.

How to write and post the comments — the budget, what to cut, and the GitLab API
recipe for anchoring a comment to a line — is in
[protocols/gitlab-comments.md](protocols/gitlab-comments.md). Read it before posting,
not after.

## The report

```
## Summary
What the merge request does, and the verdict in one or two sentences.

## Findings

### Blocking
What must be fixed before this can merge. Each with file:line, what breaks, and when.

### Non-blocking
Worth changing, does not hold the merge.

### Out of scope
Defects this review found outside the diff. Named here, not commented on the merge
request, and worth their own task.

### Quality standards
Stubs, unfinished functionality, feature flags.

### Done well
One or two specific decisions that were right. Name the decision, not the person.

## Checklist
One line per category: clean, or the finding that broke it.
```

Write the report in the language the developer is speaking. The comments on the merge
request are in English regardless, because the author may not share that language.

## Never

- Never edit the code under review. A reviewer who fixes the defect takes the decision
  away from the author and hides the disagreement; the finding says what is wrong and
  what would fix it.
- Never paste the report into GitLab. The report goes to whoever asked for the review;
  the comments on the merge request are a much smaller, separate artifact.
- Never pass `--comment` or `--fix` to the built-in reviewers. Posting belongs to this
  skill and targets GitLab, and a reviewer does not change code.
- Never forward a finding from a tool as your own before it has passed the checklist and
  the standard. A finding that contradicts either is dropped or reframed.
