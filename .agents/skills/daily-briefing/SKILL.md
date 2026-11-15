---
name: daily-briefing
description: Guides the user through a structured briefing process at the beginning of the operational day, collects relevant information, and creates a confirmed briefing report.
---

# Daily Briefing

## 1. Purpose
Asking the user about their sleep and wake-up time, giving a short summary of the previous operational day and presenting the tasks and intentions for the current operational day.


## 2. Operating Principles

### Seperation of concerns
- Invocation is triggered by the user, after the user recieved a message from the agent that the briefing is ready to be started.
- Never create, delete or change files outside `/reports/daily/daily-briefing/` and `/changeover-log.md`.
- Never change or delete files in `/reports/daily/daily-briefing/` other than the files you created during this briefing session.

### User config
- Language rules: The briefing is conducted in the user's preferred language, as specified in the configuration settings.

### Other important principles
- Daily changeover time = wake-up time - 2 hours. The operational day changes over to the next day at this time.
- `/changeover-log.md` is the source of truth for the definitive changeovers. A changeover that has been logged is final: never edit or delete existing lines.
- If no confirmed changeover exists for a calendar day (for example because the briefing was skipped), use `fallback_changeover_time` from `/config.md` as the changeover time of that day, and record it in the log as a fallback (see Phase 4).
- All paths are relative to the repository root: Do not assume any other base path.


## 3. Sources
- `/config.md` for the user's configuration settings, including `fallback_changeover_time`.
- `/changeover-log.md` for earlier changeover times.
- User input in chat.
- Last daily-debrief report in `/reports/daily/daily-debrief/`. Its `operational_day` field says which operational day it covers.
- Newest file in `/intentions/daily/`: the items the user parked for today during the last debrief.
- Files in `/intentions/weekly/`: the weekly intentions and agreement packages (afsprakenpakketten) that apply this week.
- Captures in `/chain-of-events/`.


## 4. Workflow

### Phase 1: Collect
- Ask the user in chat for their sleep and wake-up time.
- Present the user with their sleep and wake-up time, and ask for confirmation or correction; do not proceed until the user confirms or corrects the sleep and wake-up time.
  - If the user corrects the sleep and wake-up time, ask the user for confirmation or correction again; do not proceed until the user confirms or corrects the sleep and wake-up time.
- Determine the daily changeover time based on the wake-up time; daily changeover time = wake-up time - 2 hours.

- Retrieve the last daily-debrief report from `/reports/daily/daily-debrief/` and extract the information.

- Retrieve the tasks and intentions for the current operational day:
  - The items in the newest file in `/intentions/daily/` (parked during the last debrief).
  - The weekly intentions and agreement packages in `/intentions/weekly/` that apply this week.
  - Read these files only; never change them.

- Retrieve the relevant captures:
  - Determine which captures in `/chain-of-events/` belong to the previous operational day, based on the changeover times in `/changeover-log.md`.
    - Then determine which of those captures have been created after the last daily-debrief report.
  - Determine which captures in `/chain-of-events/` belong to the current operational day, based on the daily changeover time you determined above.
  - Retrieve the captures that belong to the previous operational day, but were created after the last daily-debrief report and extract the information.
  - Retrieve the captures that belong to the current operational day and extract the information.

### Phase 2: Create report
- Create a new `.md` file in `/reports/daily/daily-briefing/` with the specified filename convention.
  - Filename convention: `dbf-YYYYMMDD-HHMMSS.md`
- Write the collected information into the file, including:
  - Sleep and wake-up time.
  - Daily changeover time.
  - Summary of the previous operational day's activities and achievements.
  - Tasks and intentions for the current operational day.
- Create an infographic of the current operational day, including:
  - Wake-up time.
  - List of tasks and intentions for the current operational day.
  - Save the infographic in `/reports/daily/daily-briefing/` with the same filename as the report, but with a `.png` extension.

### Phase 3: Present report
- Present the user in chat with the infographic of the current operational day.
- Present the user with a text-message that includes a summary of the previous operational day, including:
  - Sleep and wake-up time.
  - Daily changeover time.
  - Summary of the previous operational day's activities and achievements.
  - The tasks and intentions for the current operational day.

### Phase 4: Confirm report
- Ask the user to confirm the briefing report; do not proceed until the user confirms or corrects the briefing report; do not finalize the report before confirmation.
  - If the user requests changes, incorporate them and present the revised briefing report again.
- When the user confirms the briefing report, finalize the report and infographic.
  - Finalize the `.md` and `.png` files.
  - Update `/changeover-log.md`, in the format described in that file. Never edit or delete existing lines.
    - First, for every calendar day after the last logged changeover that has no logged changeover (briefing skipped), append a line with that day's date and `fallback_changeover_time`, marked `(fallback)`, oldest first.
    - Then append one line with the confirmed daily changeover time.
- Present the user in chat with the finalized infographic of the current operational day.
- Present the user in chat with a copy of the `.md` file that contains the finalized daily-briefing report.
- Update the `.md` and `.png` files your created in `/reports/daily/daily-briefing/` during Phase 2.
- Send a text-message in chat to the user that the briefingsession of this day is now completed.
