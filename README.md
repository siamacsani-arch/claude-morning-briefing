# 🌅 Daily Morning Briefing — Claude + Cowork

An automated daily briefing that runs every weekday morning and sends a personalized Slack DM summarizing everything that happened the previous business day — across Slack, Gmail, Google Drive, and Asana — so you walk into work already oriented.

Built with [Anthropic Cowork](https://claude.ai) and the Claude Agent SDK.

---

## What It Does

Every weekday at 9am, Claude automatically:

1. **Scans your Slack** — finds messages you sent, threads you were mentioned in, and key channel activity
2. **Reads your Gmail** — surfaces important emails, flags anything needing a reply, and pulls calendar invites for today
3. **Checks your Google Drive** — finds Docs and Sheets you edited the prior day and extracts key decisions, action items, and meeting notes
4. **Reviews your Asana** — categorizes your tasks as overdue, due today, or upcoming

Then sends it all as a single, clean Slack DM grouped by **topic** (not by source), so it reads like a smart briefing — not a data dump.

### Example Output

```
☀️ Good morning, [Name]! Here's your briefing for Friday, April 17
(recap of Thursday, April 16)
━━━━━━━━━━━━━━━━━━━━━━━━

📣 WHAT HAPPENED YESTERDAY
━━━━━━━━━━━━━━━━━━━━━━━━
• 📊 Paid Squad Deck — Slides finalized and ready for sync with team.
  Minor changes ongoing. Deck shared with Lee Anne ahead of Part 2 call.

• 🗓️ Google Meetings Rescheduled — YouTube Accelerator Program moved
  to Tue Apr 21, 2–3pm EDT. Havenly / Google Sync moved to Thu Apr 23.

• ⚠️ Criteo Follow-up Needed — No reply from Mallory since Apr 8.
  EUS call never scheduled. ID tag drop flagged Apr 15, no response.
  Reply needed by EOD Monday.

📅 TODAY'S CALENDAR
━━━━━━━━━━━━━━━━━━━━━━━━
• 4:00pm EDT — Product ↔ Growth Touchbase

📋 ASANA: TODAY'S FOCUS
━━━━━━━━━━━━━━━━━━━━━━━━
🔴 Overdue · 🟡 Due today · 🟢 Upcoming
...
━━━━━━━━━━━━━━━━━━━━━━━━
Have a great day! 🚀 Sent by @Claude
```

---

## Requirements

- [Anthropic Cowork](https://claude.ai) (desktop app)
- The following plugins/MCPs connected in Cowork:
  - **Slack** — to read messages and send the DM
  - **Gmail** — to read email threads and calendar invites
  - **Google Drive** — to read Docs and Sheets you edited
  - **Asana** — to pull your task list

---

## Setup

See [SETUP.md](./SETUP.md) for full step-by-step instructions.

**Quick version:**
1. Install Cowork and connect the four plugins above
2. Copy `skill.md` into your Cowork project's skill folder
3. Personalize the placeholders (your name, Slack user ID, email, channels)
4. Schedule it to run at 9am Monday–Friday using Cowork's scheduler

---

## File Structure

```
daily-morning-briefing/
├── README.md        ← You are here
├── skill.md         ← The briefing task (customize this)
└── SETUP.md         ← Step-by-step setup guide
```

---

## Customization

The briefing is designed to be personalized. In `skill.md`, you can:

- Change which Slack channels are scanned (e.g. `#engineering`, `#product`)
- Adjust the send time and days
- Add or remove data sources
- Change the Slack DM recipient
- Modify the output format and tone

---

## How It Works

Cowork runs the `skill.md` file on a schedule using Claude as the agent. Claude pulls data from all connected sources in parallel, synthesizes everything by topic (not by tool), and sends the result as a Slack DM. The briefing skips weekends automatically — on Mondays it recaps Friday instead.

---

## Contributing

PRs welcome. Common improvements people add:
- Google Calendar integration (once available as an MCP)
- Zoom/meeting transcript summaries
- Custom action item tracking across days
- Brand/team-specific channel configurations

---

*Built by Siamac Sani @ Havenly. Powered by Claude.*
