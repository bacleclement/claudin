# I Replaced My $99/month Waalaxy with Claude — Here's the Free Open-Source Alternative

*How I built a LinkedIn outreach tool in one afternoon that does everything Waalaxy does — for free.*

---

## The Problem

If you've ever tried LinkedIn outreach at scale, you know the drill: Waalaxy, Lemlist, Phantombuster. They all charge $39 to $139/month for what is essentially:

1. Search for people
2. Send connection requests
3. Message them when they accept

That's it. Three steps. And you're paying $1,200/year for it.

![Waalaxy pricing — $39 to $139/month](images/waalaxy-pricing.jpg)

I thought: Claude can control a browser. Claude can read LinkedIn profiles. Claude can write personalized messages. Why am I paying for this?

So I built **Claudin** — a free, open-source set of Claude Code skills that replaces Waalaxy entirely.

---

## What Claudin Does

Claudin runs inside the Claude Code app (desktop or web) and controls your Chrome browser through the Claude in Chrome extension. It does exactly three things:

1. **Search** — finds relevant profiles on LinkedIn based on your ICP
2. **Connect** — sends connection requests with AI-drafted personalized notes
3. **Message** — detects when someone accepts and auto-sends a follow-up message

No sequences (that's v2). No email finder (yet). Just the core loop that generates 80% of results.

---

## Setup: 5 Minutes

### Prerequisites
- Claude Code app (desktop or [claude.ai/code](https://claude.ai/code)) — NOT the terminal CLI
- Claude in Chrome extension (Chrome Web Store)
- LinkedIn account (free or Premium)

### Install

```
git clone https://github.com/bacleclement/claudin.git
cd claudin
```

Open the folder in Claude Code app, then run:

```
/claudin-setup
```

The setup wizard asks you 7 questions:

**Step 0** — Language: French or English? The whole wizard adapts.

**Step 1** — LinkedIn plan: Free or Premium? This changes the connection strategy (free = limited personalized notes).

**Step 2** — "Describe your business in one sentence."
> I typed: "I'm building an AI assistant for trade fair buyers — snap a booth photo, get a structured supplier profile."

**Step 3** — "Who do you want to reach?"
> "Sourcing managers, buyers, and category managers at French retail companies"

**Step 4** — Claude suggests LinkedIn search keywords based on your answers:
- [x] "acheteur sourcing foire"
- [x] "category manager retail"
- [x] "sourcing manager import Lille"
- [x] "responsable achats international"
- [ ] "procurement manager France"

You just check the ones that make sense.

**Step 5** — Claude drafts your messages. You review and edit:

> Connection note: "Bonjour {prenom}, je developpe un outil IA pour les acheteurs en foire : photo d'un stand -> fiche fournisseur auto. Curieux d'avoir votre avis !"

> Follow-up: "Merci pour la connexion ! Je developpe un assistant IA pour les acheteurs en foire — photo d'un stand -> fiche fournisseur structuree. Seriez-vous dispo pour un echange de 15 min ?"

**Step 6** — Schedule: every morning at 9am? Every evening? Manual only?

Done. Config saved. You're ready.

---

## First Run: Watch It Work

```
/claudin
```

### Step 1 — Search

Claudin opens LinkedIn in your Chrome browser, runs your search keywords, and scans the results:

![LinkedIn search results](images/search-results.jpg)

It reads each profile: role, company, location. Skips irrelevant ones (recruiters, salespeople). Adds relevant buyers to the tracker.

### Step 2 — Connect

For each new profile found, Claudin clicks "Connect" and adds your personalized note:

[SCREENSHOT: Claude Code showing connection request being sent]

It respects LinkedIn limits: max 15 per run, max 100 per week. Random delays between each action (15-30 seconds) to avoid detection.

### Step 3 — Check Acceptances & Message

Claudin navigates to your connections page, checks for new acceptances:

![Recent connections showing acceptances](images/connections-accepted.jpg)

When someone accepted, it clicks "Message" and sends your follow-up:

![Follow-up message sent to Pierre Foulon](images/message-sent.jpg)

All automatic. All tracked.

---

## Results: Day 1

After one afternoon:

```
Claudin — 2026-04-06

Found: 15 profiles
Invites: 15 (15/100 this week)
Acceptances: 2
Messages: 2
Pipeline: 0 found -> 13 invited -> 0 connected -> 2 messaged
```

Two Castorama buyers accepted and got my follow-up message within hours. One is a Director-level contact. That's a $0 cost for what Waalaxy charges $99/month to do.

---

## Claudin vs Waalaxy

| Feature | Waalaxy | Claudin |
|---|---|---|
| Price | $39-139/month | **Free** |
| Open source | No | **Yes** |
| Your data | Their servers | **Your machine** |
| Connection requests | Yes | Yes |
| Personalized notes | Yes | Yes + **AI-drafted** |
| Auto-messaging | Yes | Yes |
| Multi-campaign | Yes | Yes |
| Sequences | Yes | No (v1) |
| Works offline | Yes (cloud) | No (needs Chrome) |
| AI message drafting | No | **Yes** |

The one thing Waalaxy does better: cloud execution. You don't need your computer on. For everything else, Claudin matches or beats it — and it's free.

---

## How It Works Under the Hood

Claudin is a set of three Claude Code skills (markdown files that tell Claude what to do):

- `/claudin-setup` — interactive config wizard
- `/claudin` — the outreach pipeline
- `/claudin-dashboard` — stats and tracking

It uses the **Claude in Chrome** extension to control your browser — clicking buttons, typing messages, reading profiles. Same thing you'd do manually, just automated.

All contacts are tracked in a local `tracker.json` file. Daily reports in JSON and Markdown. Everything stays on your machine.

---

## Get Started

```
git clone https://github.com/bacleclement/claudin.git
```

Open in Claude Code app. Run `/claudin-setup`. Start outreaching.

GitHub: [github.com/bacleclement/claudin](https://github.com/bacleclement/claudin)

Star the repo if this saved you $99/month.

---

*Built by [Clement Bacle](https://linkedin.com/in/clementbacle) — also building [Gotchi](https://github.com/bacleclement/gotchi), an AI sourcing assistant for trade fair buyers.*
