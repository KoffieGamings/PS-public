# Changeover log

The source of truth for the definitive daily changeovers: the moment each operational day starts.

- One line per confirmed daily briefing (wake-up time minus 2 hours).
- One line per calendar day without a confirmed briefing, using `fallback_changeover_time` from `config.md`, marked `(fallback)`.
- Written only by the daily-briefing skill, after the user confirms the briefing.
- Oldest entry first, newest entry last.
- A logged changeover is final: never edit or delete existing lines.
- Format: `- YYYY-MM-DD HH:MM` or `- YYYY-MM-DD HH:MM (fallback)` (local time, 24-hour).

## Log

