---
name: captures
description: Turns an explicitly designated user capture into a proposed atomic Markdown note, requests user approval, and saves it to the Chain of Events in the repository only after confirmation.
---

# Captures

## Purpose

Handle one individual capture submitted by the user.

Prepare an atomic Markdown note, present the complete draft for review, and save it to the Chain of Events in the repository only after the user explicitly approves it.

## Input and activation

- The skill receives the user's original message as its input.
- Do not determine whether an arbitrary message qualifies as a capture. Capture detection and skill invocation are handled outside this skill.
- Each invocation handles exactly one capture.

## Operating Principles

- **All paths are relative to the repository root:** Do not assume any other base path.

## Workflow

### 1. Prepare the draft

- Treat the original user message as the source text.
- Preserve the original wording verbatim.
- Do not independently rewrite, correct spelling or grammar, summarize or restructure the source text.
- Come up with a short, descriptive slug for the filename. The slug must be lowercase, contain only a-z0-9_, be no longer than 40 characters, and contain no spaces or special characters; transliterate accents.

### 2. Present the draft for review

- Send the user a regular written message containing your proposed target filename and the complete proposed Markdown note.
- Make the full content visible so the user can review it.
- Do not create, modify, or otherwise write any repository file at this stage.

### 3. Apply feedback

The user may request edits, provide replacement text, or ask for further adjustments.

- Apply only changes explicitly requested by the user.
- Continue to preserve the source wording wherever the user has not requested a change.
- After every revision, present the complete updated Markdown note and filename.
- Wait for the user's explicit approval before saving.

### 4. Obtain explicit approval

- Only an unambiguous confirmation from the user authorizes file creation.
- Do not treat silence, ambiguous feedback, or a request for further edits as approval.
- Keep the draft pending until approval is received.
- Do not write anything to the repository before approval.

### 5. Save the approved note

After explicit approval:

- Create exactly one new Markdown file in `/chain-of-events/` in the repository.
- Save immediately after approval: the note must be in place within 10 seconds after the user's confirmation.
- Use the approved filename and content exactly as presented in the final draft.
- Do not make additional edits, corrections, formatting changes, or interpretations during saving.
- Never overwrite an existing file. If the proposed filename already exists, stop and ask the user how to proceed. Do not silently change the timestamp or filename.

### 6. Link the capture in the calendar-day MoC

Right after saving, as part of the same 10 seconds:

- The calendar-day MoC is `/chain-of-events/MoC-YYYYMMDD-<weekday>.md`, using the `YYYYMMDD` from the capture filename and the English weekday of that date in lowercase, for example `MoC-20261113-friday.md`.
- If it does not exist yet (first capture of that calendar day), first fill any gap: for every calendar day between the newest existing calendar-day MoC and the capture's date that has no MoC (days without captures), create its MoC with only the back link line and an empty line, oldest first. This keeps the chain unbroken.
- Then create the capture's calendar-day MoC with only:
  - one line linking to the previous calendar day's MoC, with a label in the language specified in `/config.md`, for example `[[MoC-20261112-thursday|vorige dag]]` (no link to the next day: that MoC does not exist yet);
  - an empty line;
  - the list of capture links below.
- Add one line `- [[<capture filename without .md>]]` at the end of the MoC. Keep the list in chronological order and never remove or change existing lines.
- Add nothing else to the MoC.

## Filename convention

Use this exact filename structure:

`i-YYYYMMDD-HHMMSS-slug.md`

Where:

- `i-` is the fixed prefix of every capture filename.
- `YYYYMMDD` is the local calendar date on which the capture was received.
- `HHMMSS` is the local 24-hour time at which the capture was received, including hours, minutes, and seconds.
- `slug` is a short, descriptive identifier for the capture. Rules for the slug: lowercase, a-z0-9_, max 40 characters, no spaces or special characters; transliterate accents; in the language of the capture.
- `.md` is the file extension.

The filename must contain only this prefix, timestamp, date, slug, and extension. Do not add a subject, title, or other descriptive text.

Example:

`i-20261113-090000-op_tijd_wakker.md`


## Destination

The fixed repository destination is:

`/chain-of-events/`

Save every approved capture directly in this directory. Do not choose another destination or infer a different path. All paths are relative to the repository root.

## Scope restrictions

- Do not determine whether an arbitrary message is a capture.
- Do not create files before explicit user confirmation.
- Do not modify existing notes, except adding the capture link to its calendar-day MoC (step 6).
- Do not rename or move captures after saving.
- Do not create or modify any MoC other than the calendar-day MoC of the capture.
- Do not update goals, milestones, or planning documents.
- Do not process, archive, or delete existing files in `/chain-of-events/`.
- Do not perform daily reviews.
- Do not copy captures to `/journal-of-progression/` or assign them to an operational day. That happens when an operational day is closed, not when a capture is saved.
- Do not modify files outside `/chain-of-events/`.

## Completion

- After the file has been successfully created and linked in its calendar-day MoC, provide a concise confirmation with its filename and destination.
- If linking in the MoC fails, retry it the same way as saving, and report clearly if it still fails.
- If file creation fails, retry saving the approved note. Never discard the approved content. If saving still fails, report the failure clearly and keep the approved note available so nothing is lost.
- Do not claim that a capture was saved unless the repository confirms successful file creation.
