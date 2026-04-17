# Setup Guide — Daily Morning Briefing

Get your personalized morning briefing running in about 15 minutes.

---

## Step 1 — Install Cowork

Download the [Anthropic Cowork](https://claude.ai) desktop app and sign in with your Anthropic/Claude account.

> Cowork is the desktop tool that runs Claude as an agent on a schedule and connects to your workplace tools.

---

## Step 2 — Connect Your Tools

In Cowork, go to **Plugins / Connectors** and connect all four of the following. Sign in with your **work account** for each (not personal).

| Tool | What it's used for |
|------|-------------------|
| **Slack** | Reading your messages and sending the morning DM |
| **Gmail** | Reading email threads and calendar invites |
| **Google Drive** | Reading Docs/Sheets you edited the prior day |
| **Asana** | Pulling your task list |

> **Tip:** If Google Drive fails to connect, make sure you're selecting your work Google account when the auth window opens. Close and reopen Cowork if it gets stuck after authorizing.

---

## Step 3 — Find Your Slack User ID

You need your Slack user ID (format: `U01ABC1234`) to tell Claude who to DM and whose messages to search.

**How to find it:**
1. Open Slack
2. Click your profile picture → **Profile**
3. Click the **⋯ More** button
4. Click **Copy member ID**

Save this — you'll need it in the next step.

---

## Step 4 — Customize the Skill File

Open `skill.md` and replace every placeholder in the table at the top:

| Placeholder | Replace with |
|---|---|
| `[YOUR_NAME]` | Your first name |
| `[SLACK_USER_ID]` | Your Slack member ID from Step 3 |
| `[YOUR_EMAIL]` | Your work email address |
| `[YOUR_TIMEZONE]` | Your timezone (EDT, PDT, GMT, etc.) |
| `[CHANNEL_1]` | A key Slack channel you want monitored |
| `[CHANNEL_2]` | Another key channel (add more if needed) |

**Example (filled in):**
```
[YOUR_NAME]       → Siamac
[SLACK_USER_ID]   → U09RHP7TDC6
[YOUR_EMAIL]      → siamac.sani@havenly.com
[YOUR_TIMEZONE]   → EDT
[CHANNEL_1]       → #paid-squad
[CHANNEL_2]       → #product-growth
```

---

## Step 5 — Add the Skill to Your Cowork Project

1. In Cowork, open or create a project (e.g. "Morning Briefing")
2. In the project folder, create a `.claude/skills/` directory if it doesn't exist
3. Copy your customized `skill.md` into that folder

The file path should look like:
```
[Your Project Folder]/.claude/skills/daily-morning-briefing/SKILL.md
```

---

## Step 6 — Schedule It

In Cowork, use the **Schedule** feature to set the briefing to run automatically:

- **Schedule:** Monday–Friday at 9:00am
- **Cron expression:** `0 9 * * 1-5`
- **Task file:** point it to your `skill.md`

> **Important:** Cowork scheduled tasks require the app to be **running on your computer**. If your laptop is closed or Cowork isn't open, the task won't fire. The easiest workaround is to set Cowork to launch at login, or leave your laptop open/plugged in overnight.

---

## Step 7 — Test It

Run the briefing manually once to make sure everything is connected:

1. In Cowork, open your project
2. Trigger the skill manually (without waiting for the schedule)
3. Check your Slack DMs — you should receive the briefing within 1–2 minutes

If something is missing (e.g. no Google Drive section), double-check that plugin is connected under Settings → Plugins.

---

## Troubleshooting

**No Slack messages found**
Verify your `[SLACK_USER_ID]` is correct. Test by searching `from:<@YOUR_ID>` directly in Slack — if that returns messages, the skill will too.

**Google Drive not pulling docs**
Make sure you signed into Google Drive with your *work* account, not personal. Reconnect the plugin if needed.

**Briefing didn't send at 9am**
Cowork must be running. Check that the app was open at 9am and the scheduled task is enabled (not paused).

**Getting too much noise / not enough signal**
Edit `skill.md` to adjust which channels are monitored, or add more filtering instructions in the "Key Principles" section.

---

## Customization Ideas

Once it's running, here are popular ways people extend it:

- **Add more Slack channels** — just add more search queries in the Slack section
- **Change the send time** — update the cron expression (e.g. `0 8 * * 1-5` for 8am)
- **Add a weekly summary** — create a separate skill that runs Monday mornings with a weekly rollup
- **Filter by project or topic** — add keywords to the Slack search to focus on specific work streams
- **Include Zoom meeting recaps** — connect the Zoom plugin and add it as a data source

---

## Questions?

Open an issue on this repo or reach out to the team. Happy to help get it working for your setup.
