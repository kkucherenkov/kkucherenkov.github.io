---
title: "Split Monkey"
weight: 5
tagline: "A typing trainer for split keyboards: speed and accuracy, plus stats per hand, finger and firmware layer."
status: "Runs locally, no public repository yet"
accent: "#d4a72c"
cover: "trainer.png"
stack:
  - Nuxt
  - SQLite
  - TypeScript
  - Docker
---

## What it's for

Trainers like monkeytype measure speed and accuracy, but they don't know which
keyboard you type on. After a move to a split board that isn't enough: you
need to see the imbalance between hands, the slow fingers and what layers cost
you. Split Monkey knows the physical layout and the keymap of the keyboard and
breaks the stats down along them. The target hardware is a Lily58 running QMK
configured through Vial.

## The trainer

- **Modes as in monkeytype:** timed (15, 30, 60 seconds) and word count (10,
  25, 50), with or without punctuation.
- **Speed, accuracy and consistency** follow monkeytype's formulas, so the
  numbers compare.
- **Keyboard under the text.** A full Lily58 sits below the words: the key for
  the next character gets an outline, the pressed one flashes. When a
  character lives on a layer, both the layer key and the character key light
  up.
- **Layout from Vial.** The keymap loads from a `.vil` file. Characters the
  board can't produce never appear in the text.

## Analytics

- **Hand balance** and alternation rate.
- **SFB:** same-finger bigrams and their cost in milliseconds.
- **Fingers and keys:** presses, errors, median latency.
- **Layer cost:** how much slower characters on a layer are than the base
  layer.
- **Run history** with trend charts and a weak-spot profile over recent runs.

## How it measures

The browser sees only what the firmware outputs: layers and mod-taps resolve
inside the keyboard. The trainer rebuilds the physical path of each character
from the loaded keymap, picks the path with fewer presses and accounts for
typing a left-hand capital with the right Shift. The layer breakdown comes
from that path; the other metrics come from the keys actually pressed.
Latencies aggregate by median, so one pause to think doesn't skew the picture.

## Under the hood

Nuxt, SQLite and an analytics core in its own package. The core takes a stream
of events and doesn't depend on the DOM, so the input source can move to
WebHID without rewriting the metrics. It deploys with a single
`docker compose up`. The trainer serves one person: there is no sign-in, and
the port listens on localhost only.
