---
name: agreement-packages
description: Creates a weekly agreement package (afsprakenpakket) and ticks off agreements the user has kept. Use when the user wants to set agreements for a period, or when a saved capture or a message says an agreement was kept.
---

# Agreement Packages

## Purpose

An agreement package is a small set of agreements the user wants to build into daily life for a fixed period, usually one week. Each new package builds on the previous one.

This skill does two things:

1. **Create** a new package.
2. **Tick off** an agreement the user has kept.

## Reads and writes

| | Path |
| --- | --- |
| Reads | `/intentions/weekly/`, `/changeover-log.md`, `/config.md`, `/chain-of-events/` |
| Writes | `/intentions/weekly/` only |

All paths are relative to the repository root.

## The package file

One file per package: `/intentions/weekly/ap-YYYYMMDD.md`, where `YYYYMMDD` is the first day of the package.

```
---
start: 2026-11-16
end: 2026-11-22
---

# Afsprakenpakket 16 t/m 22 november 2026

## Afspraken

- A1: Om 23:00 in bed
- A2: Om 07:30 uit bed

## Afgevinkt

- 2026-11-16_0530 | A1 | [[i-20261116-230500-in_bed]]
```

- `start` and `end` are calendar dates; both days are part of the package.
- Every agreement has a short code: `A1`, `A2`, and so on.
- Every line under `## Afgevinkt` is one tick: operational day ID, agreement code, and the capture that proves it. Write `-` instead of the capture if there is none.
- Headings and agreements are written in the language specified in `/config.md`.

The **active package** is the one whose `start` and `end` contain today's date.

## Workflow 1: Create a package

1. Ask for the period. Default: seven days, starting today.
2. If an earlier package exists, show its agreements as a starting point. The user decides what stays, changes or goes.
3. Collect the agreements in the user's own words. A package usually has 3 to 6 agreements. Mention it if there are fewer or more, but let the user decide.
4. Present the complete file for review and wait for explicit approval.
5. After approval, create the file with an empty `## Afgevinkt` section.

Never overwrite an existing package. If the new period overlaps an existing package, stop and ask the user how to proceed.

## Workflow 2: Tick off an agreement

Input: a capture that was just saved, or a message from the user, saying an agreement was kept.

1. Find the active package. If there is none, tell the user and stop.
2. Decide which agreement it is. If it is not clear, ask. Never guess.
3. Determine the operational day ID: the start of the current operational day, from the last line in `/changeover-log.md`, as `YYYY-MM-DD_HHMM`. If today has no line yet, use `fallback_changeover_time` from `/config.md`.
4. If this agreement already has a line for this operational day, tell the user it was already ticked and stop.
5. Add one line at the end of `## Afgevinkt`.
6. Confirm in one short sentence which agreement was ticked.

## Rules

- Only add lines under `## Afgevinkt`. Never change or remove existing lines or agreements.
- Never tick an agreement the user did not clearly say they kept.
- Do not record agreements that were not kept, and do not comment on them.
- Never change a capture. Saving captures is the job of the `captures` skill.
