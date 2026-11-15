---
name: setup
description: Sets up the Personal Progress System the first time it is used in a new repository. Use when /config.md does not exist, or when the user asks to install or set up the system.
---

# Setup

## Purpose

Prepare a fresh copy of the system for its user, step by step, so that the daily cycle works from the next morning on.

Go through the steps in order. Explain each step in one or two short sentences, in plain words: the user may be new to tech and AI. Ask before you change anything.

## Reads and writes

| | Path |
| --- | --- |
| Reads | the whole repository |
| Writes | `/config.md`, the agent's own skills folder (step 3), git settings (step 4) |

## Steps

### 1. Personal settings

1. If `/config.md` already exists, show it and ask whether anything should change. Otherwise copy `/config.example.md` to `/config.md`.
2. Ask the user:
   - which language the system should use with them (for example `NL` or `EN`);
   - at what time their operational day normally starts if they skip the briefing (`fallback_changeover_time`, 24-hour). Explain: the day changes over two hours before they usually wake up.
3. Write the answers into `/config.md`. Remove the first line ("Copy this file …"), since it only belongs in the example.

### 2. Check the folders

Make sure these exist; create any that are missing, with an empty `.gitkeep` file inside:

- `/chain-of-events/`
- `/journal-of-progression/`
- `/intentions/daily/`, `/intentions/weekly/`, `/intentions/backlog/`
- `/reports/daily/daily-briefing/`, `/reports/daily/daily-debrief/`

Also check that `/changeover-log.md` and `/intentions/backlog/backlog.md` exist. If one is missing, tell the user: it should come with the system.

### 3. Make the skills available to the agent

The skills live in `/.agents/skills/`. Some agents read skills from that folder automatically; others look somewhere else.

- Find out where you, the agent, load skills from. If it is `/.agents/skills/`, nothing needs to happen.
- If it is another folder, for example `/.claude/skills/`, make that folder point to `/.agents/skills/` with a link, so there is only one copy to keep up to date. If links are not possible, copy the skills and tell the user they need to copy them again after every update.
- If you cannot load skills at all, that is fine too: you can always read a skill's `SKILL.md` and follow it when needed.

### 4. Where updates come from

1. Ask from which repository the user wants to receive system updates. The default is the public version of the system the user started from.
2. Add it as a git remote named `upstream`, for example `git remote add upstream <url>`.
3. Explain: "Updates come in with `git pull upstream main`. They never change your settings or your notes."

Skip this step if `upstream` already exists.

### 5. Daily schedule

Ask at what time the user wants:

- the **briefing** in the morning (followed by `journal-closing`);
- the **debrief** in the evening.

If you can create scheduled tasks, create one for each, with a prompt that starts the skill by name. If you cannot, tell the user they can start them themselves by writing "briefing" or "debrief".

### 6. Save and test

1. Commit the changes with the message `chore: set up the system` and push.
2. Offer a test capture: the user sends one short note, and you run the `captures` skill. Show where it landed.
3. Finish with a short summary: what was set up, and what happens tomorrow morning.

## Rules

- Never delete or overwrite the user's notes, reports or logs.
- Never put personal settings or notes in the upstream repository.
- If a step fails, explain what went wrong in plain words and continue with the next step if possible.
