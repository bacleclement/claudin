# J'ai remplace mon Waalaxy a 99$/mois par Claude — voici l'alternative gratuite et open-source

*Comment j'ai cree un outil de prospection LinkedIn en un apres-midi qui fait tout ce que fait Waalaxy — gratuitement.*

---

## Le probleme

Si vous avez deja fait de la prospection LinkedIn a grande echelle, vous connaissez les outils : Waalaxy, Lemlist, Phantombuster. Ils facturent tous entre 39 et 139 euros/mois pour essentiellement :

1. Chercher des profils
2. Envoyer des demandes de connexion
3. Envoyer un message quand ils acceptent

Trois etapes. Et vous payez 1 200 euros/an pour ca.

![Waalaxy pricing — 39 a 139 euros/mois](images/waalaxy-pricing.jpg)

Je me suis dit : Claude peut controler un navigateur. Claude peut lire des profils LinkedIn. Claude peut ecrire des messages personnalises. Pourquoi je paye pour ca ?

J'ai donc cree **Claudin** — un ensemble de skills Claude Code, gratuit et open-source, qui remplace Waalaxy.

---

## Ce que fait Claudin

Claudin tourne dans l'app Claude Code (desktop ou web) et controle votre navigateur Chrome via l'extension Claude in Chrome. Il fait exactement trois choses :

1. **Recherche** — trouve des profils pertinents sur LinkedIn selon votre cible
2. **Connexion** — envoie des demandes avec des notes personnalisees redigees par l'IA
3. **Message** — detecte quand quelqu'un accepte et envoie automatiquement un message de suivi

Pas de sequences (c'est pour la v2). Pas de recherche d'emails (a venir). Juste la boucle principale qui genere 80% des resultats.

---

## Installation : 5 minutes

### Pre-requis
- App Claude Code (desktop ou [claude.ai/code](https://claude.ai/code)) — PAS le terminal CLI
- Extension Claude in Chrome (Chrome Web Store)
- Compte LinkedIn (gratuit ou Premium)

### Installation

```
git clone https://github.com/bacleclement/claudin.git
cd claudin
```

Ouvrez le dossier dans l'app Claude Code, puis lancez :

```
/claudin-setup
```

L'assistant de configuration vous pose 7 questions :

**Etape 0** — Langue : francais ou anglais ? Tout l'assistant s'adapte.

**Etape 1** — Plan LinkedIn : gratuit ou Premium ? Cela change la strategie de connexion (gratuit = nombre limite de notes personnalisees).

**Etape 2** — "Decris ton activite en une phrase."
> J'ai tape : "Je developpe un assistant IA pour les acheteurs en foire — photo d'un stand, fiche fournisseur structuree automatiquement."

**Etape 3** — "Qui veux-tu atteindre ?"
> "Les responsables sourcing, acheteurs et chefs de produit dans le retail francais"

**Etape 4** — Claude propose des mots-cles de recherche LinkedIn :
- [x] "acheteur sourcing foire"
- [x] "category manager retail"
- [x] "sourcing manager import Lille"
- [x] "responsable achats international"
- [ ] "procurement manager France"

Vous cochez ceux qui vous semblent pertinents.

**Etape 5** — Claude redige vos messages. Vous relisez et modifiez :

> Note de connexion : "Bonjour {prenom}, je developpe un outil IA pour les acheteurs en foire : photo d'un stand -> fiche fournisseur auto. Curieux d'avoir votre avis !"

> Message de suivi : "Merci pour la connexion ! Je developpe un assistant IA pour les acheteurs en foire — photo d'un stand -> fiche fournisseur structuree. Seriez-vous dispo pour un echange de 15 min ?"

**Etape 6** — Planification : chaque matin a 9h ? Chaque soir ? Manuel uniquement ?

C'est fait. Config sauvegardee. Vous etes pret.

---

## Premier lancement : regardez-le travailler

```
/claudin
```

### Etape 1 — Recherche

Claudin ouvre LinkedIn dans votre navigateur Chrome, lance vos recherches et scanne les resultats :

![Resultats de recherche LinkedIn](images/search-results.jpg)

Il lit chaque profil : poste, entreprise, localisation. Ignore les profils non pertinents (recruteurs, commerciaux). Ajoute les acheteurs pertinents au tracker.

### Etape 2 — Connexion

Pour chaque nouveau profil trouve, Claudin clique sur "Se connecter" et ajoute votre note personnalisee :

[SCREENSHOT: Claude Code envoyant une demande de connexion]

Il respecte les limites LinkedIn : max 15 par session, max 100 par semaine. Delais aleatoires entre chaque action (15-30 secondes) pour eviter la detection.

### Etape 3 — Verification des acceptations et messages

Claudin navigue vers votre page de connexions, verifie les nouvelles acceptations :

![Connexions recentes montrant les acceptations](images/connections-accepted.jpg)

Quand quelqu'un a accepte, il clique sur "Message" et envoie votre suivi :

![Message de suivi envoye a Pierre Foulon](images/message-sent.jpg)

Tout est automatique. Tout est suivi.

---

## Resultats : Jour 1

Apres un apres-midi :

```
Claudin — 2026-04-06

Profils trouves : 15
Invitations : 15 (15/100 cette semaine)
Acceptations : 2
Messages : 2
Pipeline : 0 trouves -> 13 invites -> 0 connectes -> 2 messages envoyes
```

Deux acheteurs Castorama ont accepte et recu mon message de suivi en quelques heures. L'un est Directeur. Cout : 0 euro. Ce que Waalaxy facture 99 euros/mois.

---

## Claudin vs Waalaxy

| Fonctionnalite | Waalaxy | Claudin |
|---|---|---|
| Prix | 39-139 euros/mois | **Gratuit** |
| Open source | Non | **Oui** |
| Vos donnees | Leurs serveurs | **Votre machine** |
| Demandes de connexion | Oui | Oui |
| Notes personnalisees | Oui | Oui + **redigees par IA** |
| Messages auto | Oui | Oui |
| Multi-campagnes | Oui | Oui |
| Sequences | Oui | Non (v1) |
| Fonctionne sans PC | Oui (cloud) | Non (Chrome necessaire) |
| Redaction IA des messages | Non | **Oui** |

Le seul avantage de Waalaxy : l'execution cloud. Votre PC n'a pas besoin d'etre allume. Pour tout le reste, Claudin fait aussi bien ou mieux — et c'est gratuit.

---

## Comment ca marche techniquement

Claudin est compose de trois skills Claude Code (des fichiers markdown qui guident Claude) :

- `/claudin-setup` — assistant de configuration interactif
- `/claudin` — le pipeline de prospection
- `/claudin-dashboard` — stats et suivi

Il utilise l'extension **Claude in Chrome** pour controler votre navigateur — cliquer sur les boutons, taper les messages, lire les profils. La meme chose que vous feriez manuellement, automatisee.

Tous les contacts sont suivis dans un fichier local `tracker.json`. Rapports quotidiens en JSON et Markdown. Tout reste sur votre machine.

---

## Pour demarrer

```
git clone https://github.com/bacleclement/claudin.git
```

Ouvrez dans l'app Claude Code. Lancez `/claudin-setup`. Commencez a prospecter.

GitHub : [github.com/bacleclement/claudin](https://github.com/bacleclement/claudin)

Mettez une etoile si ca vous a economise 99 euros/mois.

---

*Cree par [Clement Bacle](https://linkedin.com/in/clementbacle) — je developpe aussi [Gotchi](https://github.com/bacleclement/gotchi), un assistant IA pour les acheteurs en foire.*
