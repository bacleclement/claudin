---
name: claudin
description: LinkedIn outreach pipeline — search, connect, message accepted connections. Reads config.json for campaigns and messages.
---

# Claudin — LinkedIn Outreach Pipeline

Automated LinkedIn outreach: search for relevant profiles, send connection requests, and message people who accept your invitation.

## Prerequisites

- Claude Code app (desktop or web — NOT terminal CLI) with Claude in Chrome extension
- Chrome open with LinkedIn logged in
- config.json exists (run /claudin-setup first)

## Arguments

| Arg | Default | Description |
|---|---|---|
| (none) | full pipeline | Run all steps for all campaigns |
| --campaign "name" | all | Run only the named campaign |
| --only-search | -- | Step 1 only: find new profiles |
| --only-connect | -- | Step 2 only: send connection requests |
| --only-message | -- | Step 3 only: check acceptances and send messages |

## Files

| File | Purpose |
|---|---|
| config.json | User config — campaigns, messages, limits (from /claudin-setup) |
| tracker.json | Contact database — auto-managed, never edit manually |
| reports/YYYY-MM-DD.json | Daily report in JSON |
| reports/YYYY-MM-DD.md | Daily report in Markdown |

## Tracker Schema

Each contact in tracker.json:
```json
{
  "id": "uuid",
  "name": "First Last",
  "firstName": "First",
  "profileUrl": "https://linkedin.com/in/...",
  "role": "Buyer",
  "company": "Castorama",
  "location": "Lille, France",
  "language": "fr",
  "degree": "2e",
  "campaign": "campaign-name",
  "status": "found|invited|connected|messaged|replied|declined",
  "invitedAt": null,
  "connectedAt": null,
  "messagedAt": null,
  "repliedAt": null,
  "response": "",
  "notes": "",
  "recentActivity": "",
  "messageContext": "",
  "ghDiscussionUrl": null
}
```

## Execution

Read config.json and tracker.json at start. If config.json missing, tell user to run /claudin-setup.

---

### Step 0 — Load config and check limits

```
Read config.json
Read tracker.json (create empty if missing)
Calculate:
  - invites_this_week = contacts invited in last 7 days
  - messages_today = contacts messaged today
  - remaining_invites = config.limits.connections_per_week - invites_this_week
  - remaining_messages = config.limits.messages_per_day - messages_today
  - personalized_notes = "unlimited" if config.linkedin_plan == "premium", else track monthly usage
```

If remaining_invites <= 0 -> skip step 2
If remaining_messages <= 0 -> skip step 3

Print current limits status.

---

### Step 1 — Search for relevant profiles

Use Claude in Chrome browser tools.

For each campaign in config.campaigns (or just the --campaign if specified):

1. Pick 2-3 search keywords from campaign.search_keywords (rotate daily, don't repeat same search two days in a row)
2. For each keyword:
   a. Navigate to LinkedIn search: https://www.linkedin.com/search/results/people/?keywords={encoded_keyword}&origin=GLOBAL_SEARCH_HEADER
   b. If campaign has target_locations, add geography filter
   c. Screenshot the results page
   d. For each visible profile:
      - Skip if already in tracker (match by name + company, or by profileUrl)
      - Skip if irrelevant role (recruiter, sales, HR, marketing — unless campaign targets these)
      - If relevant: extract name, firstName, role, company, location, profileUrl, degree
      - Detect language: French name/company/location -> "fr", else "en"
      - Add to tracker with status "found", campaign name
   e. Scroll down once for more results
   f. Save tracker after each page

Wait 5-10 seconds between searches.

---

### Step 2 — Connection requests

Guard: Stop if remaining_invites <= 5 (keep buffer)

For each contact in tracker where status = "found" (oldest first):

1. Navigate to their profileUrl (or search by name if URL unknown)
2. Click "Se connecter" / "Connect" button
3. If LinkedIn shows "Ajouter une note" / "Add a note" dialog:
   - If config.linkedin_plan == "premium" OR personalized invites remaining > 0:
     - Get the message from the contact's campaign config
     - Pick connection_note_fr or connection_note_en based on contact.language
     - Replace {prenom}/{firstName} with contact.firstName
     - Type the note and click send
   - Else: click "Envoyer sans note" / "Send without a note"
4. Update tracker: status = "invited", invitedAt = today
5. Wait 15-30 seconds between requests (randomize)
6. Stop after config.limits.connections_per_run invites

Save tracker after each batch of 5.

If LinkedIn shows a CAPTCHA or restriction warning: STOP immediately, report to user, do not continue.

---

### Step 3 — Check acceptances and message

Guard: Stop if remaining_messages <= 5

#### 3a. Detect accepted invitations

1. Navigate to https://www.linkedin.com/mynetwork/invite-connect/connections/
2. Page is sorted by "Ajouts recents" / "Recently added"
3. Screenshot the page
4. For each contact in tracker where status = "invited":
   - If their name appears in the recent connections list: update status = "connected", connectedAt = today
5. Scroll down once and check again
6. Save tracker

#### 3b. Analyze recent activity and send personalized messages

For each contact in tracker where status = "connected" (not yet messaged):

**Analyze their profile activity:**

1. Navigate to their profileUrl
2. Scroll to "Activité" / "Activity" section
3. Scan their last 3-5 posts, reposts, or comments (if any)
4. Extract context: topics they care about, events attended, opinions shared, companies mentioned
5. Judge relevance: does any post relate to the business context in config.business.description? (trade shows, sourcing, suppliers, purchasing, fairs, product discovery, etc.)
6. Save a brief summary in contact.recentActivity (1-2 sentences max), or empty if nothing relevant

**Compose and send personalized message:**

7. Start from the campaign's followup_template_fr or followup_template_en based on contact.language
8. Replace {prenom}/{firstName} with contact.firstName
9. Only personalize if a post is RELEVANT to the product/business context. A post about a trade show, sourcing trip, supplier management, or industry fair = relevant. A post about leadership quotes, team celebrations, or unrelated topics = NOT relevant — do not reference it.
10. If relevant activity found: open with a natural reference — e.g. "J'ai vu votre post sur le salon X — ". This goes BEFORE the template text.
11. If no activity or no relevant activity: use the template as-is, do not force a reference
10. Keep total message under 500 characters, conversational, no sales pitch
11. Click "Message" on their profile page
12. Type the personalized message and click "Envoyer" / "Send"
13. Update tracker: status = "messaged", messagedAt = today, messageContext = what was referenced from their posts
14. Wait 15-30 seconds between messages

Save tracker.

---

#### 3c. Collect responses and post to GitHub Discussions

**Trigger:** Run this step on every pipeline execution. Check LinkedIn messaging for replies from contacts with status = "messaged".

1. Navigate to https://www.linkedin.com/messaging/
2. For each contact in tracker where status = "messaged":
   - Search for their name in the messaging inbox
   - If they replied: read the full conversation thread
   - Extract their response text
   - Update tracker: status = "replied", repliedAt = today, response = summary of their reply

3. If config.feedback is set and the reply contains useful signal (pain points, workflow details, tool usage, feature ideas, opinions on the problem space):

   **Post to GitHub Discussion using `gh api graphql`:**

   ```
   gh api graphql -f query='mutation { createDiscussion(input: {
     repositoryId: "{config.feedback.gh_repo_id}",
     categoryId: "{config.feedback.gh_discussion_category_id}",
     title: "[Interview] {contact.name} — {contact.role} ({contact.company})",
     body: "..."
   }) { discussion { number url } } }'
   ```

   **Discussion body format:**

   ```markdown
   ## Contact
   - **Name:** {contact.name}
   - **LinkedIn:** {contact.profileUrl}
   - **Role:** {contact.role}
   - **Company:** {contact.company}
   - **Location:** {contact.location}
   - **Campaign:** {contact.campaign}

   ## Career Path
   Scrape experience section from their LinkedIn profile. Include role, company, dates.

   ## Conversation — {date}

   ### Our message
   > {the message we sent}

   ### Their response
   > {their full reply}

   ## Key Insights
   - Bullet points summarizing actionable takeaways
   - Pain points, workflow details, tools used, feature requests
   - How this relates to the product

   ## Next Step
   - Recommend action: propose call, send demo, follow up with question, etc.

   ## Source
   Captured by Claudin outreach pipeline — campaign: {contact.campaign}
   ```

4. Update tracker: ghDiscussionUrl = the discussion URL returned by the API
5. Save tracker

**Note:** Only post discussions for replies with actual signal. Polite declines ("merci mais pas intéressé") should NOT be posted — just update tracker status to "declined".

---

### Step 4 — Daily report

Generate both JSON and Markdown reports.

#### JSON report (reports/YYYY-MM-DD.json):

```json
{
  "date": "YYYY-MM-DD",
  "pipeline": {
    "found": 0,
    "invited": 0,
    "connected": 0,
    "messaged": 0,
    "total": 0
  },
  "today": {
    "profiles_found": 0,
    "invites_sent": 0,
    "acceptances": 0,
    "messages_sent": 0
  },
  "limits": {
    "invites_week": { "used": 0, "max": 100 },
    "messages_day": { "used": 0, "max": 50 }
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
  ]
}
```

#### Markdown report (reports/YYYY-MM-DD.md):

```markdown
# Claudin Report — YYYY-MM-DD

## Today
| Metric | Count |
|---|---|
| Profiles found | X |
| Invites sent | X |
| Acceptances | X |
| Messages sent | X |

## Rate Limits
| Limit | Used | Remaining |
|---|---|---|
| Invites (weekly) | X/100 | X |
| Messages (daily) | X/50 | X |

## Pipeline
| Status | Count |
|---|---|
| Found | X |
| Invited | X |
| Connected | X |
| Messaged | X |
| Total | X |

## By Campaign
| Campaign | Found | Invited | Connected | Messaged |
|---|---|---|---|---|
| campaign-1 | X | X | X | X |
```

#### Print summary to chat:

```
Claudin — YYYY-MM-DD

Found: X | Invites: X (Y/100 week) | Acceptances: X | Messages: X (Y/50 day)
Pipeline: X found -> X invited -> X connected -> X messaged
```

---

## Rate Limiting and Safety

- Max connections_per_run per session (default 15)
- Max connections_per_week (default 100, LinkedIn limit)
- Max messages_per_day (default 50)
- 15-30 second random pause between each action
- Never connect or message the same person twice (tracker prevents this)
- Stop immediately if LinkedIn shows CAPTCHA or restriction — report to user
- If account restricted: stop all actions, notify user, suggest waiting 24h
