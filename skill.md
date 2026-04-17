---
name: daily-morning-briefing
description: Daily 9AM morning briefing — Slack, Gmail, Asana, and Google Drive recap sent as a Slack DM
---

<!--
  ╔══════════════════════════════════════════════════════════╗
  ║           DAILY MORNING BRIEFING — SKILL FILE           ║
  ║                                                          ║
  ║  Before using, replace all placeholders marked with      ║
  ║  [BRACKETS]. See SETUP.md for detailed instructions.     ║
  ╚══════════════════════════════════════════════════════════╝
-->

You are generating [YOUR_NAME]'s daily morning briefing. Compile a comprehensive recap of everything that happened the previous business day and send it as a Slack DM.

## Placeholders to Replace

| Placeholder           | What to put here                                      |
|-----------------------|-------------------------------------------------------|
| `[YOUR_NAME]`         | Your first name (e.g. Siamac)                        |
| `[SLACK_USER_ID]`     | Your Slack user ID (e.g. U09RHP7TDC6) — see SETUP.md |
| `[YOUR_EMAIL]`        | Your work email (e.g. you@company.com)               |
| `[YOUR_TIMEZONE]`     | Your timezone abbreviation (e.g. EDT, PDT, GMT)      |
| `[CHANNEL_1]`         | A key Slack channel to monitor (e.g. #engineering)   |
| `[CHANNEL_2]`         | Another key channel (e.g. #product)                  |

---

## Data Sources to Pull (all in parallel where possible)

### 1. Slack
Search for:
- Messages sent by [YOUR_NAME]: `from:<@[SLACK_USER_ID]> after:YYYY-MM-DD before:YYYY-MM-DD`
- Messages mentioning [YOUR_NAME]: `<@[SLACK_USER_ID]> after:YYYY-MM-DD before:YYYY-MM-DD`
- Key channel activity: `in:[CHANNEL_1] after:YYYY-MM-DD before:YYYY-MM-DD`
- Key channel activity: `in:[CHANNEL_2] after:YYYY-MM-DD before:YYYY-MM-DD`

Use `slack_search_public_and_private` with date filters for the previous business day.

### 2. Gmail
Search for emails sent and received by [YOUR_EMAIL] from the previous business day.
Use query: `after:YYYY/MM/DD before:YYYY/MM/DD`
Flag any unread threads that appear to need a reply.
Pull calendar invites to populate today's calendar section.

### 3. Asana
Pull the current task list using `get_my_tasks` with `completed_since: "now"`.
Categorize tasks as:
- **Overdue** — due_on is before today
- **Due today** — due_on is today's date
- **Upcoming** — due_on is in the future (next 7 days)
- **No date** — flag the most important open items only (limit to 5)

### 4. Google Drive
Search for Docs and Sheets modified by [YOUR_EMAIL] the previous day.
Use `list_recent_files` with `orderBy: lastModifiedByMe`.
For each document owned by [YOUR_EMAIL] and modified yesterday, use `read_file_content` to extract:
- Key decisions made
- Action items noted
- Meeting notes or summaries

---

## Output Format

Send a single well-formatted Slack DM using `slack_send_message` to `[SLACK_USER_ID]`.

```
:sunny: *Good morning, [YOUR_NAME]! Here's your briefing for [TODAY'S DAY], [DATE]*
_(recap of [YESTERDAY'S DAY], [DATE])_
━━━━━━━━━━━━━━━━━━━━━━━━

:mega: *WHAT HAPPENED YESTERDAY*
━━━━━━━━━━━━━━━━━━━━━━━━
[TOPIC BULLETS — see formatting rules below]

:date: *TODAY'S CALENDAR*
━━━━━━━━━━━━━━━━━━━━━━━━
[MEETINGS from calendar invites in Gmail]

:clipboard: *ASANA: TODAY'S FOCUS*
━━━━━━━━━━━━━━━━━━━━━━━━
:red_circle: Overdue · :large_yellow_circle: Due today · :large_green_circle: Upcoming
[TASK LIST]

━━━━━━━━━━━━━━━━━━━━━━━━
Have a great day! :rocket: *Sent by* @Claude
```

### Topic Bullet Format

Group everything by **topic** (not by source). Each bullet combines everything relevant from Slack, Gmail, and Drive into one entry.

Aim for **4–7 topics**. For each:
- What happened / what was discussed or decided
- Where things stand now
- Any action needed (flagged inline with `:warning:`)

**Example bullets:**
```
• :rotating_light: *Vendor X — Support Issue* — Escalated billing discrepancy to account team.
  Still waiting on response. :warning: *Follow up needed* if no reply by EOD.

• :bar_chart: *Weekly Review Prep* — Updated slides, shared with manager.
  Noted in Drive doc: need to add Q2 projections before Thursday.

• :google: *Partner Sync Rescheduled* — Moved to next Tuesday 2pm [YOUR_TIMEZONE].
  New invite confirmed in email.
```

---

## Key Principles

- **One mention per item** — each piece of information appears once, in the most relevant topic. Never repeat across sections.
- **Action items inline** — flag `:warning:` within the topic, not in a separate section.
- **Skip weekends** — if today is Monday, recap Friday. Otherwise skip Sat/Sun.
- **Don't send if empty** — if there is no meaningful activity to report, do not send.
- **Scannable over comprehensive** — this is read first thing in the morning. Prioritize decisions and actions over raw summaries.
- **Ignore automated noise** — skip automated system emails, noreply notifications, and bot messages unless they contain meaningful signal.
