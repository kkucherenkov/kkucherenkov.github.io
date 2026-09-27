---
title: "Shipyard"
weight: 6
tagline: "A development process for working with AI agents, packaged as a Claude Code plugin and a repository template."
status: "Plugin and template, MIT license"
links:
  - name: "shipyard"
    url: "https://github.com/kkucherenkov/shipyard"
  - name: "project-skeleton"
    url: "https://github.com/kkucherenkov/project-skeleton"
stack:
  - Claude Code
  - POSIX shell
  - GitHub Actions
---

## What it's for

The rules a project follows when agents write code tend to get copied from
repository to repository and drift apart. Shipyard splits them into two
pieces: a Claude Code plugin with skills and a repository template with the
process files. A fix to a skill reaches every project through
`/plugin update`.

## The shipyard plugin

Six skills, each firing at its own moment:

- **task-stack** keeps the task stack: before any task bigger than a one-line
  fix, when a task blocks and when one ships.
- **lane-discipline** keeps parallel agents from colliding.
- **ci-gates** tells a green pull request from one that only looks green and
  finds required checks that never ran.
- **issue-bookkeeping** reconciles the issue tracker with what reached main.
- **deploy-verify** puts a release on a host and checks which version the host
  runs.
- **release-audit** audits the product before a release.

The `/bootstrap` command sets up a fresh repository from the template once:
it fills the placeholders, writes the first ADR and opens the first task.

Skills read project facts from named sections of `CLAUDE.md`. When a section
is missing, the skill stops and says so instead of running on an empty
default.

## The project-skeleton template

A GitHub template that gives a project its process on the first commit and
nothing else: no framework, no package manager, no linter.

- `specs/tasks/` is the task stack, one file per task, with no shared index for
  branches to collide on.
- `.claude/CLAUDE.md` holds a five-rule working agreement and the sections the
  skills read.
- A Conventional Commits check for PR titles in POSIX shell, with its own
  tests.
- `docs/adr/` keeps architecture decision records.

Monorepos get a separate plugin, `shipyard-monorepo`. It's optional: shipyard
runs without it.

## Getting started

```sh
gh repo create my-project --template kkucherenkov/project-skeleton --private --clone
```

Then, in a Claude Code session:

```text
/plugin marketplace add kkucherenkov/shipyard
/plugin install shipyard
/bootstrap
```
