---
name: claudin-dashboard
description: Show Claudin pipeline stats — visual summary and JSON output
---

# Claudin Dashboard

Displays outreach pipeline statistics from tracker.json. Both visual (chat) and JSON output.

## Execution

1. Read tracker.json — if missing, tell user to run /claudin-setup then /claudin first.
2. Read config.json for campaign names.
3. Compute stats.
4. Print visual dashboard to chat.
5. Write JSON to reports/dashboard.json.

## Stats Computation

```
For each contact in tracker.json:
  - Group by status: found, invited, connected, messaged
  - Group by campaign
  - Group by company (top 10)
  - Calculate: acceptance_rate = connected / invited
  - Calculate: response_rate = messaged / connected (if messages received — future)
  - Weekly velocity: invites last 7 days, messages last 7 days
  - Time to accept: average days between invitedAt and connectedAt
```

## Visual Dashboard (chat output)

```
=== CLAUDIN DASHBOARD ===

Pipeline:
  Found:     ||||||||||||| 12
  Invited:   |||||||||||||||||||||||| 24
  Connected: ||||||| 7
  Messaged:  ||||| 5
  Total: 48

Acceptance rate: 29% (7/24)

This week:
  Invites:  18/100
  Messages: 5/50

By Campaign:
  "ERP manufacturers"  — 3 found | 12 invited | 4 connected | 3 messaged
  "Retail sourcing"    — 9 found | 12 invited | 3 connected | 2 messaged

Top Companies:
  Castorama (5) | Leroy Merlin (3) | Decathlon (2) | Blancheporte (1)

Last run: 2026-04-06 09:17
Next scheduled: 2026-04-07 09:00
```

## JSON Output (reports/dashboard.json)

```json
{
  "generated_at": "ISO timestamp",
  "pipeline": {
    "found": 0,
    "invited": 0,
    "connected": 0,
    "messaged": 0,
    "total": 0
  },
  "rates": {
    "acceptance_rate": 0.0,
    "acceptance_count": 0,
    "invite_count": 0
  },
  "weekly": {
    "invites": { "used": 0, "max": 100 },
    "messages": { "used": 0, "max": 50 }
  },
  "campaigns": [
    {
      "name": "campaign-name",
      "found": 0,
      "invited": 0,
      "connected": 0,
      "messaged": 0
    }
  ],
  "top_companies": [
    { "name": "Company", "count": 0 }
  ],
  "timing": {
    "avg_days_to_accept": 0,
    "last_run": "ISO timestamp",
    "next_scheduled": "ISO timestamp"
  }
}
```
