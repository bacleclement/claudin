# Claudin

**Prospection LinkedIn gratuite et open-source. Remplace Waalaxy par Claude.**

Claudin est un ensemble de skills [Claude Code](https://claude.ai/code) qui automatisent ta prospection LinkedIn :
- Recherche de prospects pertinents
- Envoi de demandes de connexion (avec notes personnalisees)
- Messages automatiques aux personnes qui acceptent
- Suivi complet dans un dashboard

Pas d'abonnement. Pas de SaaS. Tes donnees restent sur ta machine. Juste Claude + ton navigateur.

> Waalaxy coute 80$/mois. Claudin fait la meme chose gratuitement.

## Comment ca marche

```
/claudin-setup     →  Assistant de config en 5 min : decris ton activite, choisis tes cibles, valide les messages
/claudin           →  Lance le pipeline : recherche → connexion → message
/claudin-dashboard →  Visualise tes stats
```

Claudin utilise Claude in Chrome pour controler ton onglet LinkedIn — comme tu le ferais, mais plus vite.

## Prerequis

1. **Claude Code app** (desktop ou web sur [claude.ai/code](https://claude.ai/code)) — **pas** le terminal CLI
2. **Extension Claude in Chrome** — installer depuis le Chrome Web Store, connecter a ta session Claude Code
3. **Compte LinkedIn** (gratuit ou Premium — Claudin s'adapte)

> **Important :** Claudin necessite l'**app** Claude Code (desktop ou web), pas le terminal CLI. Le controle du navigateur ne fonctionne que via l'app.

## Demarrage rapide

```
1. Clone le repo :         git clone https://github.com/bacleclement/claudin.git
2. Ouvre l'app Claude Code (desktop ou claude.ai/code)
3. Ouvre le dossier claudin comme projet
4. Lance :                 /claudin-setup
5. Demarre la prospection : /claudin
```

L'assistant te pose 7 questions (en francais ou anglais), redige tes messages avec l'IA, et configure tout. 5 minutes.

## Fonctionnalites

### Multi-campagnes
Cible differentes audiences avec differents messages. Un fondateur SaaS peut avoir une campagne pour les CTO et une autre pour les VP Sales — chacune avec ses mots-cles et messages.

### Ciblage assiste par IA
Decris ton activite et ton client ideal. Claudin propose des mots-cles de recherche LinkedIn. Tu choisis ceux qui te conviennent.

### Respect des limites LinkedIn
- Respecte les limites (100 connexions/semaine, 50 messages/jour)
- Delais aleatoires entre les actions (15-30s)
- S'adapte au plan gratuit vs Premium
- S'arrete immediatement en cas d'alerte LinkedIn

### Planification automatique
Programme Claudin pour tourner chaque matin, soir, ou deux fois par jour. Il verifie les acceptations, envoie les messages de suivi, et trouve de nouveaux prospects — en autopilote.

### Suivi complet
Chaque contact est suivi : quand il a ete trouve, invite, connecte, et contacte. Rapports quotidiens en JSON et Markdown. Dashboard visuel dans ton terminal.

## Claudin vs Waalaxy

| Fonctionnalite | Waalaxy | Claudin |
|---|---|---|
| Prix | 56-80$/mois | Gratuit |
| Open source | Non | Oui |
| Tes donnees | Sur leurs serveurs | Sur ta machine |
| Demandes de connexion | Oui | Oui |
| Notes personnalisees | Oui (Premium) | Oui |
| Messages auto | Oui | Oui |
| Multi-campagnes | Oui | Oui |
| Sequences (multi-etapes) | Oui | Non (v1 — connexion + 1 message) |
| Fonctionne sans PC | Oui (cloud) | Non (Chrome doit etre ouvert) |
| Messages rediges par IA | Non | Oui (Claude redige tes messages) |

## Licence

MIT

## Contribuer

Issues et PRs bienvenues. Si tu as cree une config de campagne utile, partage-la.
