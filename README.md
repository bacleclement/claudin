# Claudin

> **[Version francaise](README.fr.md)**

**Free, open-source LinkedIn outreach. Replace Waalaxy with Claude.**

Claudin is a set of [Claude Code](https://claude.ai/code) skills that automate your LinkedIn outreach:
- Search for your ideal prospects
- Send connection requests (with personalized notes)
- Auto-message people who accept
- Track everything in a simple dashboard

No subscription. No SaaS. No data leaves your machine. Just Claude + your browser.

> Waalaxy charges $80/month. Claudin does the same thing for free.

## How It Works

```
/claudin-setup     →  5-min wizard: describe your business, pick targets, review AI-drafted messages
/claudin           →  runs the outreach pipeline: search → connect → message
/claudin-dashboard →  see your pipeline stats
```

Claudin uses Claude in Chrome to control your LinkedIn browser tab — the same way you would, just faster.

## Prerequisites

1. **Claude Code app** (desktop or web at [claude.ai/code](https://claude.ai/code)) — **not** the CLI terminal
2. **Claude in Chrome extension** — install from Chrome Web Store, connect it to your Claude Code session
3. **LinkedIn account** (free or Premium — Claudin adapts to your plan)

> **Important:** Claudin requires the Claude Code **app** (desktop or web), not the terminal CLI. Browser control only works through the app.

## Quick Start

```
1. Clone the repo:       git clone https://github.com/bacleclement/claudin.git
2. Open Claude Code app (desktop or claude.ai/code)
3. Open the claudin folder as your project
4. Run:                  /claudin-setup
5. Start outreaching:    /claudin
```

The setup wizard asks you 7 questions (in French or English), drafts your messages using AI, and configures everything. Takes 5 minutes.

## Features

### Multi-campaign support
Target different audiences with different messages. A SaaS founder might have one campaign for CTOs and another for VPs of Sales — each with tailored search keywords and messages.

### AI-assisted targeting
Describe your business and ideal customer. Claudin suggests LinkedIn search keywords based on your description. Pick the ones that fit.

### Smart rate limiting
- Respects LinkedIn's limits (100 connections/week, 50 messages/day)
- Random delays between actions (15-30s) to avoid detection
- Adapts to free vs Premium plans (personalized notes budget)
- Stops immediately if LinkedIn shows a warning

### Automatic scheduling
Set it to run every morning, evening, or twice a day. Claudin checks for new acceptances, sends follow-up messages, and finds new prospects — on autopilot.

### Full tracking
Every contact is tracked: when they were found, invited, connected, and messaged. Daily reports in JSON and Markdown. Visual dashboard in your terminal.

## Dashboard

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
  "SaaS for CTOs"      — 12 invited | 4 connected | 3 messaged
  "Consulting for VPs"  — 12 invited | 3 connected | 2 messaged
```

## Claudin vs Waalaxy

| Feature | Waalaxy | Claudin |
|---|---|---|
| Price | $56-80/month | Free |
| Open source | No | Yes |
| Your data | On their servers | On your machine |
| Connection requests | Yes | Yes |
| Personalized notes | Yes (Premium) | Yes |
| Auto-messaging | Yes | Yes |
| Multi-campaign | Yes | Yes |
| Sequences (multi-step) | Yes | No (v1 — connect + 1 message) |
| Works without PC | Yes (cloud) | No (needs Chrome open) |
| AI-drafted messages | No | Yes (Claude writes your messages) |
| LinkedIn detection risk | Medium | Low (human-speed delays) |

## FAQ

**Will this get my LinkedIn account banned?**
Claudin mimics human behavior: random delays between actions, respects weekly limits, stops on warnings. The risk is lower than most automation tools because it operates at human speed. That said, all LinkedIn automation carries some risk. Use responsibly.

**Do I need LinkedIn Premium?**
No. Claudin works with free accounts. With Premium you get unlimited personalized notes on connection requests, which improves acceptance rates.

**Does it work in languages other than English/French?**
The setup wizard is in French or English, but your messages and search keywords can be in any language.

**Can I run it from the terminal (CLI)?**
No. Claudin requires the Claude Code **app** (desktop or web). Browser control is not available in the terminal CLI.

**Can I run it headless / in the cloud?**
Not currently. Claudin needs Chrome open with the extension connected. Your computer needs to be on when Claudin runs.

## License

MIT

## Contributing

Issues and PRs welcome. If you've built a useful campaign config, share it — it helps everyone.
