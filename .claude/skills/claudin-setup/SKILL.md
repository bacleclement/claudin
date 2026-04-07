---
name: claudin-setup
description: Interactive setup wizard for Claudin — configures business context, target campaigns, messages, LinkedIn plan, and schedule
---

# Claudin Setup

Interactive wizard that configures Claudin for the user's outreach needs. Generates config.json with campaigns, messages, and schedule.

## Prerequisites Check

Before starting, verify:
1. User is running Claude Code app — desktop or web (claude.ai/code), NOT the terminal CLI
2. Claude in Chrome extension is installed and connected
3. User is logged into LinkedIn in Chrome

Test the connection:
- Call tabs_context_mcp to check Chrome extension is reachable
- If not connected: "Claudin needs Chrome with the Claude in Chrome extension. Please install it from [link] and make sure Chrome is open with LinkedIn logged in."

## Setup Flow

### Step 0 — Language

Ask in both languages simultaneously:

"Do you speak French or English? Parles-tu francais ou anglais ?"

Options: Francais / English

All subsequent questions use the chosen language. Store as config.language.

### Step 1 — LinkedIn Plan

FR: "As-tu un abonnement LinkedIn Premium ou un compte gratuit ?"
EN: "Do you have LinkedIn Premium or a free account?"

Options: Premium / Gratuit (Free)

This affects the strategy:
- Free: max 5 personalized invites/month, no InMail. Claudin prioritizes sending without notes.
- Premium: unlimited notes, InMail available. Claudin always adds a personalized note.

Store as config.linkedin_plan.

### Step 2 — Business Description

FR: "Decris ton activite en une phrase. Que vends-tu ou que proposes-tu ?"
EN: "Describe your business in one sentence. What do you sell or offer?"

Free text. Store as config.business.description.

### Step 3 — Target Definition (first campaign)

FR: "Qui veux-tu atteindre ? Decris ton client ideal (role, secteur, taille d'entreprise...)"
EN: "Who do you want to reach? Describe your ideal customer (role, industry, company size...)"

Free text. Claude analyzes the answer.

### Step 4 — Search Keywords (AI-assisted)

Based on steps 2 and 3, Claude generates 6-10 LinkedIn search keyword combinations and presents them as multiple-choice checkboxes.

FR: "Voici des recherches LinkedIn que je te propose. Coche celles qui te semblent pertinentes :"
EN: "Here are LinkedIn searches I'd suggest. Check the ones that look relevant:"

Example for a user selling ERP to manufacturers:
- [ ] "directeur usine"
- [ ] "responsable production industrie"
- [ ] "DSI industrie"
- [ ] "IT manager manufacturing"
- [ ] "ERP project manager"
- [ ] "directeur industriel"
- [ ] "responsable supply chain"
- [ ] "COO manufacturing"

User selects which ones to keep. Claude may also ask:

FR: "Des entreprises specifiques a cibler ?"
EN: "Any specific companies to target?"

Free text (optional). E.g. "Decathlon, Leroy Merlin, Castorama"

FR: "Zone geographique ?"
EN: "Geographic area?"

Free text. E.g. "Lille, Hauts-de-France" or "Europe"

Store as first campaign in config.campaigns[].

### Step 5 — Messages (AI-drafted)

Claude drafts a connection note and follow-up message for this campaign, based on the business description and target profile. One per language selected.

Connection note must be under 200 characters (LinkedIn limit).
Follow-up message should be 2-3 sentences, end with a question.

FR: "Voici les messages que je te propose pour cette campagne. Tu peux les modifier :"
EN: "Here are the messages I'd suggest for this campaign. You can edit them:"

Show:
- Connection note (FR and/or EN)
- Follow-up message (FR and/or EN)

User reviews and edits. Confirm before saving.

### Step 6 — Additional Campaigns

FR: "Veux-tu ajouter une autre campagne avec des cibles differentes ?"
EN: "Do you want to add another campaign with different targets?"

If yes: repeat steps 3-5 for the new campaign.
If no: move to step 7.

### Step 7 — Schedule

FR: "Quand veux-tu que Claudin tourne automatiquement ?"
EN: "When should Claudin run automatically?"

Options:
- Chaque matin (9h) / Every morning (9am)
- Chaque soir (19h) / Every evening (7pm)
- Deux fois par jour (9h + 19h) / Twice a day (9am + 7pm)
- Personnalise / Custom (user picks time)
- Manuel seulement / Manual only (no schedule)

If a schedule is selected, create a persistent scheduled task using the scheduled-tasks MCP tool:
- Task ID: "claudin-outreach"
- Prompt: "Run /claudin — full outreach pipeline"
- Cron: based on user choice (e.g. "0 9 * * *" for morning)

Store as config.schedule.

### Step 8 — Confirmation

Show a summary of the full config:

```
Claudin Setup Complete

Language: Francais
LinkedIn: Free plan
Business: [description]

Campaign 1: [name]
  Targets: [roles] at [companies] in [locations]
  Keywords: [list]
  Messages: [preview first 50 chars]

Schedule: Every morning at 9am

Run /claudin to start your first outreach session.
Run /claudin-dashboard to see your pipeline stats.
```

Write config.json to the project root.

## Output

config.json written with all campaigns, messages, and settings.
Scheduled task created if user chose automatic runs.
