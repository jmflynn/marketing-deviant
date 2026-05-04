---
name: morning-brief
description: Automated weekday morning briefing skill for Cowork scheduled tasks. Triggers every weekday at 7am to deliver a scannable, under-400-word email brief covering the day's time-blocked calendar events and relevant email/conversation context from the last 48 hours scoped to meeting attendees. Use this skill whenever the scheduled morning-brief task fires, or when the user asks to "run my morning brief", "send my daily briefing", or "what's on my calendar today with context".
compatibility: Requires Google Calendar MCP, Gmail MCP, and recent_chats tool access.
---

# Morning Brief Skill

Delivers a single, scannable weekday briefing by 7am that surfaces calendar events for the day and relevant context — pulled from email threads and recent Claude conversations — so the user walks into their day prepared.

---

## Schedule Behavior

- **Runs**: Monday–Friday only. If today is Saturday or Sunday, do nothing and exit silently.
- **Scope**: Today's date only (the calendar day the task fires).
- **Time**: Scheduled for 7:00am local time via Cowork.

---

## Step-by-Step Execution

### Step 1 — Check the day

Before doing anything else, check today's day of the week. If it's Saturday or Sunday, stop. Do not send an email. Do not log anything.

### Step 2 — Pull today's calendar events

Use Google Calendar MCP to fetch all events for today.

**Filter rules:**
- Include only **time-blocked events** (events with a specific start and end time)
- **Exclude all-day events** (events with no time component)
- **Exclude declined events**
- Sort by start time ascending

If there are no qualifying events after filtering → skip to the **No Meetings** path below.

For each event, collect:
- Event title
- Start and end time
- Attendee list (names and/or email addresses)

### Step 3 — Scan email for attendee-scoped threads

For each meeting on the calendar, use Gmail MCP to search for email threads from the **last 48 hours** where any of the meeting's attendees appear as sender or recipient.

Search strategy per meeting:
- Build a search query using attendee emails: `from:(email1 OR email2) OR to:(email1 OR email2) newer_than:2d`
- Pull the subject line and a brief summary of the thread content (2–3 sentences max per thread)
- Flag only threads that are substantively relevant — skip automated/system emails, calendar notifications, and mailing list blasts

### Step 4 — Scan recent Claude conversations

Use `recent_chats` to look back at Claude conversations from the **last 48 hours**.

For each meeting, check if any recent conversation touches on:
- The meeting topic (inferred from event title)
- Any of the attendees by name
- Related projects, documents, or decisions

Pull a 1–2 sentence summary of any relevant conversation found.

### Step 5 — Compose the brief

Write a single flowing brief. Target: **under 400 words**. Tone: direct, professional, scannable. No fluff, no filler.

**Structure:**

```
Good morning. Here's your day.

[DAY, DATE] — [X] meetings

──────────────────────────

[TIME] — [Meeting Title]
With: [Attendee names]

[1–3 sentences of relevant context from email or conversations. 
If none found, write: "No recent context found."]

──────────────────────────

[Repeat for each meeting in chronological order]

──────────────────────────

That's your day. Go get it.
```

**Content rules:**
- Lead with the most actionable context, not background
- If a thread or conversation is relevant to multiple meetings, mention it under the first relevant meeting only
- Do not invent or infer context — only surface what was actually found
- Keep each meeting block to 4–6 lines max

### Step 6 — No Meetings path

If there are no time-blocked events today, compose a minimal email:

```
Good morning.

No meetings on the calendar today. Clear runway.
```

### Step 7 — Send the email

Use Gmail MCP to send the brief as an email:
- **To**: the user's own email address (the authenticated Gmail account)
- **Subject**: `Morning Brief — [Day, Month Date]` (e.g., "Morning Brief — Monday, May 5")
- **Body**: plain text (no HTML required)

---

## Quality Checks Before Sending

- [ ] Word count is under 400
- [ ] Only weekday execution (Saturday/Sunday = no send)
- [ ] No all-day events included
- [ ] Email threads scoped to attendees only — no inbox-wide sweep
- [ ] Context is factual (pulled from real sources, not inferred)
- [ ] Subject line includes correct date

---

## Error Handling

- If Google Calendar MCP fails: send email with subject "Morning Brief — [Date]" and body: "Could not retrieve calendar data this morning. Check your calendar manually."
- If Gmail scan fails: proceed with the brief but note under each meeting: "Email context unavailable this morning."
- If recent_chats fails: proceed without conversation context — do not block the send.
