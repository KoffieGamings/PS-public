---
name: journal-closing
description: Closes finished operational days by copying their captures into the Journal of Progression and creating the journal MoC for each day. Use right after a new changeover has been logged, or when the user asks to close a day.
---

# Journal Closing

## Purpose

Turn every finished operational day into one page in the Journal of Progression: a journal MoC (JMoC) plus a copy of the captures that belong to that day.

The Chain of Events stays the untouched raw record. The journal is the organized layer on top of it.

## When to use

- Right after the daily briefing has added a new line to `/changeover-log.md`.
- When the user asks to close a day.

One run closes every operational day that is finished but not yet in the journal. Running it again is always safe: it never makes duplicates.

## Key terms

- **Operational day:** runs from one line in `/changeover-log.md` (its start) to the next line (its end).
- **Closed day:** an operational day whose end is in the log. The day that starts at the last line is still running and is never closed.
- **Operational day ID:** the start as `YYYY-MM-DD_HHMM`, for example `2026-11-15_0530`.
- **JMoC filename:** `JMoC-YYYYMMDD-HHMM-<weekday>.md`, using the start date, start time and English weekday in lowercase, for example `JMoC-20261115-0530-sunday.md`.

## Reads and writes

| | Path |
| --- | --- |
| Reads | `/changeover-log.md`, `/chain-of-events/`, `/reports/daily/`, `/templates/MoC-template.md`, `/config.md` |
| Writes | `/journal-of-progression/` only |

All paths are relative to the repository root.

## Steps

### 1. Find the days to close

1. Read the lines under `## Log` in `/changeover-log.md`, oldest first. Lines marked `(fallback)` count like any other line.
2. Every two neighbouring lines form one closed day: the first is its start, the second is its end.
3. Skip a day if its JMoC already exists in `/journal-of-progression/`.
4. If nothing is left, tell the user there is nothing to close and stop.

Handle the remaining days one by one, oldest first.

### 2. Copy the captures

1. A capture is a file in `/chain-of-events/` named `i-YYYYMMDD-HHMMSS…md`. Files starting with `MoC-` are not captures.
2. A capture belongs to the day if its timestamp is at or after the start and before the end.
3. Copy each of these captures to `/journal-of-progression/` with `j-` instead of `i-` at the start of the name. Keep the rest of the name and the content exactly the same.
4. If the `j-` copy already exists, leave it as it is.

### 3. Find the reports

- **Debrief report:** the file in `/reports/daily/daily-debrief/` whose `operational_day` equals the operational day ID.
- **Briefing report:** the file in `/reports/daily/daily-briefing/` whose filename timestamp falls inside the day.

A day can miss one or both. That is fine.

### 4. Create the JMoC

1. Copy `/templates/MoC-template.md` to `/journal-of-progression/<JMoC filename>`.
2. Keep the template's headings, order and language exactly as they are.
3. Fill it in as described below, in the language specified in `/config.md`.

| Part of the template | What to write |
| --- | --- |
| Title | Weekday and date of the day's start. |
| Intro sentence | Link to the newest earlier JMoC, for example `[[JMoC-20261114-0600-saturday\|gisteren]]`, and one short remark about that day. Leave the link out if there is no earlier JMoC. |
| Dagoverzicht | One or two sentences on the main activity or focus of the day. |
| Dagelijkse notities | One wikilink per `j-` copy of this day, oldest first. |
| Bereikt | The items under `## Gedaan` in the debrief report, as checked items (`- [x]`). |
| Nice to Address (today) | Unplanned achievements, only if a report or capture mentions them. |
| Dagelijkse rapporten | Wikilinks to the debrief and briefing report of this day, with their path, for example `[[reports/daily/daily-debrief/ddb-20261115-223015]]`. |
| Planning volgende dag: Need to Address | The items under `## Geparkeerd voor morgen` in the debrief report, as open items (`- [ ]`). |
| Planning volgende dag: Afspraken | Appointments with time and people, only if a report or capture mentions them. |
| Planning volgende dag: Nice to Address | Optional items, only if a report or capture mentions them. |
| Outro sentence | Link to the next JMoC, for example `[[JMoC-20261116-0545-monday\|morgen]]`. Its name follows from the end of this day, which is the start of the next one. |

Copy task and item texts word for word from the source. If a part has no source, leave it empty under its heading.

### 5. Check and report

Check that every `j-` copy and JMoC exists, then tell the user:

- which days were closed;
- how many captures were copied per day;
- which days had no debrief report.

## Rules

- Never change, move or delete anything in `/chain-of-events/`, `/changeover-log.md`, `/reports/` or `/intentions/`.
- Never overwrite an existing JMoC or `j-` copy.
- Never invent tasks, achievements, appointments, people or times.
- Never close the running day.
- Never translate or rewrite captures or reports.
