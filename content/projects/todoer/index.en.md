---
title: "todoer"
weight: 4
tagline: "A self-hosted task manager that works offline on every client: web, CLI and Flutter."
status: "In development: sync and the CLI work"
links:
  - name: "GitHub"
    url: "https://github.com/kkucherenkov/todoer"
stack:
  - NestJS 11
  - Prisma
  - Postgres 18
  - SQLite
  - OpenAPI
---

## What it's for

Personal tasks, projects, priorities and tags in the spirit of Todoist and
Things, on your own server. The defining requirement is offline: every client
holds a full replica and answers "what's on today" without a network.

## What works

- **Sync.** One endpoint, `POST /api/v1/sync`, takes operations and returns
  changes. Conflicts resolve per field, last write wins. Tombstones get pruned,
  and a client with a stale cursor receives a fresh snapshot.
- **CLI.** `todoer add`, `list` and `outbox` work against a local SQLite
  replica. Each operation enters a queue first and keeps its id across retries,
  so running `add` twice doesn't create a duplicate.
- **For scripts and agents.** `--json` output and exit codes you can branch
  on: exit code 5 means the server was unreachable and the operation waits in
  the queue.

## Planned

- Projects and tags from quick-add, recurring tasks.
- Views: list, kanban, calendar.
- The web client and the Flutter app.
- A habit tracker in version two.

## Under the hood

NestJS 11, Prisma and Postgres 18. There is no REST CRUD: every write goes
through the sync protocol. The contract lives in OpenAPI, and
`express-openapi-validator` enforces it in both directions. The repository
started from the [project-skeleton](../shipyard/) template.
