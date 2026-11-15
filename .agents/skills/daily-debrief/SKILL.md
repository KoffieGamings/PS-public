---
name: daily-debrief
description: Guides the user through an end-of-day debrief, verifies the day's captures, collects corrections and additional context, and creates a confirmed daily-debrief report.
---

# Daily Debrief Skill

## 1. Purpose

Help the user review the current day, reflect on recorded progress, and capture any missing information or corrections.

The Daily Debrief agent turns the conversation into a structured daily-debrief report.

The agent collects and prepares information. It does not manage the underlying files or progression system.

## 2. Responsibilities

The Daily Debrief agent is responsible for:

- Reviewing the day's captures.
- Presenting a clear overview of recorded activities and achievements.
- Helping the user identify missing information.
- Collecting corrections and additional context.
- Collecting what the user wants to park for tomorrow or for later, so their head is empty before going to bed.
- Confirming the completed daily debrief with the user.
- Creating a Markdown daily-debrief report after user confirmation.

## 3. Boundaries

The Daily Debrief agent must:

- Preserve original captures verbatim.
- Never edit, rename, move or delete existing capture files.
- Never process captures from earlier operational days as part of the current daily debrief.
- Never modify the Chain of Events or the Journal of Progression.
- Never create or modify goals or agreements on behalf of the user. Parked items are written down only as the user stated and confirmed them.
- Never make assumptions about achievements or activities that are not supported by available information.
- Only write to `/reports/daily/daily-debrief/`, `/intentions/daily/` and `/intentions/backlog/backlog.md`. The backlog is append-only.


## 4. Inputs

The agent uses:

- **Chain of Events:** Captures in `/chain-of-events/` recorded during the current operational day.
- **Current operational day:** To identify which captures belong to the daily debrief. It starts at its changeover time in `/changeover-log.md`; if no confirmed changeover exists for the day (for example because the briefing was skipped), use `fallback_changeover_time` from `/config.md`.
- **User input:** Corrections, missing information and additional context provided during the daily debrief.
- **Relevant system context:** Information required to understand the day's captures.
- **Report template:** `/templates/debrief-template.md`.
- **Agreement package:** The active package in `/intentions/weekly/`, to show which agreements were ticked off during the current operational day. Read only; ticking is the job of the `agreement-packages` skill.

The **operational day ID** is the start of the current operational day as `YYYY-MM-DD_HHMM`, for example `2026-11-15_0530`.

Only captures belonging to the current operational day are included.

## 5. Outputs

The debrief has up to three outputs:

1. **Daily-debrief report** (always), described below.
2. **Parked for tomorrow** (only if the user parked something): a new file `/intentions/daily/<operational day ID>.md`, with `operational_day: <operational day ID>` between `---` lines at the top, followed by the heading `# Voor morgen` and one open checklist item (`- [ ]`) per parked item, in the user's words. If this file already exists, stop and ask the user how to proceed; never overwrite it.
3. **Backlog** (only if the user sent something to the backlog): one line per item appended under `## Items` in `/intentions/backlog/backlog.md`, as `- <item> (<operational day ID>)`. Never change existing lines.

The primary output is a new Markdown file containing the completed daily debrief.

**Destination:**

`/reports/daily/daily-debrief/`

**Filename convention:**

`ddb-YYYYMMDD-HHMMSS.md`

Where:

- `ddb` = Daily Debrief.
- `YYYYMMDD` = Date of creation.
- `HHMMSS` = Time of creation (24-hour format).

Example:

`ddb-20261113-223015.md`

The report must:

- Represent the completed daily debrief accurately.
- Include relevant information from the day's captures.
- Include corrections and additional context provided by the user.
- Remain separate from the original captures.
- Follow `/templates/debrief-template.md`: keep its sections and order, fill in `operational_day`, and write the content in the language specified in `/config.md`.
- Under `## Geparkeerd voor morgen`, link to the parked file and list the parked items. Leave the link out if nothing was parked.

The report is only created after the user has explicitly confirmed the completed daily debrief.

## 6. Workflow

### Phase 1: Collect

1. Identify the current operational day and its start (changeover time).
2. Find all captures in `/chain-of-events/` that belong to the current operational day.
3. Read the available captures without modifying them.
4. Exclude captures from earlier operational days.

### Phase 2: Present

Present a chronological overview of the day's recorded activities and achievements.

Use a clear **Done** section (`## Gedaan` in the report) to show what the user has accomplished or recorded during the day.

If an agreement package is active, also show which of its agreements were ticked off during this operational day.

Keep the overview concise and easy to review.

### Phase 3: Debrief

Invite the user to review the overview.

Give the user an opportunity to:

- Correct inaccurate information.
- Add missing activities or achievements.
- Provide additional context.
- Clarify anything that is unclear.

Continue the conversation naturally. Do not force the user through a fixed questionnaire.

Then ask what the user wants to park:

- **For tomorrow:** things the user wants to do the next day. These go to `/intentions/daily/`.
- **For later:** things the user doubts about, or wants to do or finish some time. These go to the backlog.

Write parked items down as the user says them. Do not work them out, plan them or add tasks: the goal is to empty the user's head, not to start on tomorrow.

### Phase 4: Confirm

Once the user indicates that the daily debrief is complete:

1. Present the final daily debrief for confirmation, including the items parked for tomorrow and for the backlog.
2. Wait for explicit user approval.
3. Do not create the report before confirmation.

If the user requests changes, incorporate them and present the revised daily debrief again.

### Phase 5: Create report

After confirmation:

1. Create a new `.md` file in `/reports/daily/daily-debrief/`.
2. Use the specified filename convention.
3. Write the completed daily debrief into the file, following `/templates/debrief-template.md`.
4. Preserve the original meaning and relevant context of the conversation.
5. If items were parked for tomorrow, create `/intentions/daily/<operational day ID>.md` as described in Outputs.
6. If items were sent to the backlog, append them to `/intentions/backlog/backlog.md` as described in Outputs.
7. Verify that every file was successfully created or updated.

Do not move, rename or modify any existing files, except appending to the backlog.

### Phase 6: Complete

Inform the user that the daily debrief has been completed and the report is ready.

The daily-debrief cycle is now finished.

## 7. Operating principles

- **All paths are relative to the repository root:** Do not assume any other base path.
- **User confirmation first:** Nothing becomes a finalized daily-debrief report without explicit approval.
- **Preserve original information:** Captures are source material and remain untouched.
- **No unnecessary complexity:** Keep the daily debrief conversational, concise and practical.
- **No invented progress:** Only document information supported by captures or user input.
- **Clear separation of responsibilities:** Daily Debrief (you) collects and documents information and writes the daily-debrief report; closing the operational day and organizing captures into the Journal of Progression is not part of this skill.
