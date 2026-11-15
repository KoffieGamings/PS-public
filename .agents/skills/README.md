# Skills overview

A skill is one folder with one `SKILL.md`: plain-language instructions that tell the agent how to do one job. There is no code. To change how the system behaves, you edit the text of a skill.

## The skills

| Skill | Job | When it runs | Writes to |
| --- | --- | --- | --- |
| `captures` | Save one note and link it in the calendar-day MoC | Whenever you capture something | `/chain-of-events/` |
| `daily-briefing` | Start the day: summary of yesterday, plans for today, log the changeover | Every morning | `/reports/daily/daily-briefing/`, `/changeover-log.md` |
| `daily-debrief` | End the day: review what you did, park things for tomorrow or later | Every evening | `/reports/daily/daily-debrief/`, `/intentions/daily/`, `/intentions/backlog/` |
| `journal-closing` | Close finished operational days into the journal | After the briefing | `/journal-of-progression/` |
| `agreement-packages` | Create a weekly agreement package and tick off agreements | When you set agreements, or after a capture says you kept one | `/intentions/weekly/` |

Each skill writes only to its own places. That is what keeps them independent: you can change or replace one skill without breaking the others.

## A day in the system

1. **Morning: `daily-briefing`.** You give your sleep and wake-up time. The briefing shows yesterday's summary and today's plans, and logs the changeover (the moment the new operational day starts).
2. **After the briefing: `journal-closing`.** The day that just ended is copied into the journal.
3. **During the day: `captures`.** Every note lands in the Chain of Events. If a note says you kept an agreement, `agreement-packages` ticks it off.
4. **Evening: `daily-debrief`.** You look back, and park what is on your mind for tomorrow or for later.

## How the skills fit together

The skills never call each other. They only share files:

- `/changeover-log.md` tells every skill where one operational day ends and the next begins.
- `/config.md` holds your settings, such as the language.
- `/templates/` holds the page layouts the skills fill in.

## Changing or adding a skill

- Every `SKILL.md` starts with a `name` and a `description` between `---` lines. The description tells the agent when to use the skill, so keep it accurate.
- Keep one job per skill. If a skill starts doing two things, split it.
- State clearly what the skill reads and what it writes, and keep the writing to its own folder.
- To add a skill, create a new folder here with a `SKILL.md` and add a row to the table above.
- What the system should do is described in `/REQUIREMENTS.md`. Update it when you change behaviour.
