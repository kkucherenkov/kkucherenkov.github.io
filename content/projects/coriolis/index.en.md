---
title: "Coriolis Companion"
weight: 2
tagline: "Character sheets, ship sheets, dice and GM tools for the Coriolis: The Third Horizon tabletop RPG."
status: "Beta, v1.1, MIT license"
accent: "#d4a95c"
links:
  - name: "GitHub"
    url: "https://github.com/kkucherenkov/coriolis_app"
cover: "character-sheet.png"
gallery:
  - src: "dashboard.png"
    caption: "Bridge: tonight's session at a glance"
  - src: "game-session.png"
    caption: "Game session: Darkness Points and members"
  - src: "npc-editor.png"
    caption: "NPC editor"
  - src: "ship-sheet.png"
    caption: "Ship sheet"
  - src: "shared-character.png"
    caption: "Public share link"
stack:
  - NestJS 11
  - Prisma 7
  - Postgres 18
  - Redis
  - Centrifugo
  - Nuxt 4
  - Flutter 3.41
  - OpenAPI 3.1
  - AsyncAPI 3.0
---

## What it's for

Coriolis: The Third Horizon is a science-fiction RPG from Free League with an
Arabian Nights flavour. The app replaces paper sheets and keeps the table in
one shared state: every member of a session sees the same rolls, Darkness
Points and chat. A fan project, not affiliated with the publisher.

## For players

- **Character sheet** true to the Coriolis rules: icons, concept, upbringing,
  attributes from 1 to 5, general and advanced skills, talents, gear,
  conditions, encumbrance, critical injuries.
- **Dice.** A D6 pool with pushed rolls: sixes are successes, and ones on a
  push hand the GM Darkness Points.
- **Public share link** to a read-only sheet, with a separate toggle for GM
  notes. The link can be revoked.
- **JSON export** of sheets, ships and history.
- **Offline.** Reading works everywhere. The web client queues edits in a
  ServiceWorker, the mobile app in a Hive cache, and both queues sync when the
  connection returns.

## For game masters

- **Ship sheets**: 20 ship classes, modules, crew positions, hull, energy,
  signature, armor, problems.
- **Game sessions**: a single invite link, member roles, an archive and a
  Darkness Point tracker with history.
- **NPCs and encounters**: pre-generated and custom NPCs, an encounter builder
  with initiative, rounds and turns.
- **A real-time table.** Rolls, Darkness Points and chat reach every member
  through Centrifugo. Chat covers session-wide messages, private whispers,
  blocking and system messages.

## Under the hood

Three clients on one backend: a Nuxt SPA, a Flutter app for iOS and Android
and a NestJS backend with Postgres, Redis and Centrifugo. The API is described
in OpenAPI and AsyncAPI, the TypeScript and Dart clients are generated from the
spec, and CI fails on drift. The interface ships in English, Russian, Ukrainian
and Greek, with key parity checked in CI. Telemetry flows through
OpenTelemetry into Grafana, errors into Sentry.

## Installation

The local stack runs on Docker Compose. There is no public instance yet:
production hosting and a mobile cold-start profile remain before v1.0.
