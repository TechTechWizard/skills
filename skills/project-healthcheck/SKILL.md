---
name: project-healthcheck
description: "Regular project healthcheck: API state, frontend state, database diagnostics, usage statistics. Universal — reads per-project config from <project>/.claude/healthcheck.md. Use when user asks for a healthcheck, project status check, 'хелсчек', 'состояние проекта', 'проверь стенд', 'статистика использования', 'usage report'."
allowed-tools: Bash Read Write Grep Glob WebFetch Skill
---

# Project Healthcheck

## Role

You run a read-only diagnostic pass over a project's stands and produce one report with
a verdict per area. You change nothing: no writes to databases, no mutating API calls,
no config edits. A healthcheck that modified something is a failed healthcheck.

## Config first

Every project describes itself in `<project>/.claude/healthcheck.md`: stand URLs, key
endpoints, how to reach the database, which usage metrics matter, and known quirks that
are not defects. **Read it before anything else.**

- No config file → offer to generate one: interview the project's `CLAUDE.md` and
  existing protocols, fill what is derivable, leave explicit `TODO:` markers for what
  is not, and show it to whoever asked for the run before the first pass.
- A config `TODO` blocks only its own section — run the sections that are configured,
  list skipped ones in «Не проверено и почему».

## Protocols

**IMPORTANT:** When a section runs, ALWAYS read its protocol file first and follow it
exactly. Do NOT improvise your own checks or format.

| Protocol | Covers | File |
|----------|--------|------|
| API check | endpoints up, auth, latency, error shape | protocols/api-check.md |
| Frontend check | page loads, console, network, critical flow | protocols/frontend-check.md |
| DB check | size, connections, bloat, locks, indexes | protocols/db-check.md |
| App errors & queues | Telescope/Sentry exceptions, failed jobs, Horizon | protocols/app-errors.md |
| Usage report | product usage statistics, trends | protocols/usage-report.md |

A full healthcheck = API + Frontend + DB + App errors. Usage report runs when asked
for, or when the config marks it as part of the regular pass — it is reporting, not
health.

## Report

One file per run, under whatever reports directory the project already uses; a project
with no such convention gets `<project>/.claude/healthchecks/healthcheck-YYYY-MM-DD/report.md`.
Evidence (screenshots, saved responses, query outputs) lives next to it.

Shape, in this order:

1. Verdict line per area: `API: OK | Frontend: WARN | DB: OK | Usage: —`
2. Per-area tables from the protocols (metric, value, OK/Warning/Critical).
3. **«Не проверено и почему»** — mandatory; a missing gap list is an incomplete report.
4. Problems found, each with a recommendation; a problem without a failure scenario is
   an observation, not a problem.
5. Closing status line: `СТАТУС: ГОТОВО` / `СТАТУС: БЛОКЕР — <что именно>`.

Verdict aggregation: WARN if any area has a Warning, FAIL if any has a Critical or an
area configured for the run could not be checked at all.

## Scheduling

This skill is the payload; scheduling is not its business. Regular runs go through the
`schedule` skill, where the harness offers one, or a hire of the `tester` role
with this skill named in the task text.
