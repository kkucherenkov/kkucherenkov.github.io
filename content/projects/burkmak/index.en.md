---
title: "burkmak"
weight: 3
tagline: "A self-hosted read-it-later with Kobo sync and highlight export to Obsidian."
status: "v0.1.0, MIT license"
accent: "#d66f4d"
links:
  - name: "GitHub"
    url: "https://github.com/kkucherenkov/burkmak"
  - name: "Deployment"
    url: "https://github.com/kkucherenkov/burkmak/blob/main/deploy/README.md"
cover: "library.png"
gallery:
  - src: "reader.png"
    caption: "Reader with highlights"
  - src: "save.png"
    caption: "Quick save"
  - src: "library-dark.png"
    caption: "Dark theme"
stack:
  - NestJS 11
  - Prisma 7
  - SQLite FTS5
  - Nuxt 4
  - Flutter 3.41
  - OpenAPI 3.1
  - OPDS
---

## What it's for

Articles saved for later get lost across tabs and bookmarks. burkmak gathers
them into one library on your own server, extracts the text without ads and
sends it to a Kobo e-reader with no firmware hacks. Highlights and notes go to
Obsidian.

## Saving

- **From anywhere.** Save a link from the web app, the mobile app, the Android
  share sheet or a browser bookmarklet.
- **Metadata in the background.** Title, site, excerpt and lead image appear
  without a page reload, over SSE.
- **Text right away.** Extraction runs on save, so the reader, OPDS and Kobo
  get the article ready to read.
- **Organising.** Tags, read states (unread, read, archived) and favourites.
  Every user has a separate library.

## Reading

- **Readability-based reader.** The article without ads or clutter. The copy
  stays even if the original disappears.
- **Images** are cached on your own server.
- **Full-text search** across title, URL and article body on SQLite FTS5.
- **Highlights** in colour, with notes attached.
- **Light and dark themes.**

## Kobo

- **OPDS catalogue** with covers, pagination and search. Articles download as
  EPUB and KEPUB, and a stock Kobo adds the catalogue from its settings.
- **Native sync** over the Kobo store protocol. The e-reader pulls new articles
  when it connects, and reading state flows back: an article finished on the
  Kobo turns read in burkmak, and one archived in the app leaves the device.
  HTTPS is required. The protocol passes a full simulation; the pass on a
  physical device is still ahead.

## Obsidian

- **Markdown export.** The API returns an article as a note with its source,
  metadata, highlights and notes. A repeat export updates the same note.
- **Obsidian plugin** syncs the export into your vault.

## Under the hood

A NestJS backend on SQLite, a Nuxt web client and a Flutter mobile client. A
worker on top of the database runs background jobs and events reach clients
over SSE, so there are no external services: the local stack is two
containers. The API is described in OpenAPI, and the TypeScript and Dart
clients are generated from the spec.

The scaffold shared by Course Shelf, Coriolis and burkmak is covered in a post (in Russian): [«Контракт, от которого код не может уйти»](/posts/spec_first_monorepo/).

## Installation

Every release publishes Docker images for amd64 and arm64. The stack starts
with `docker compose up -d`.
