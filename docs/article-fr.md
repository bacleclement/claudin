# J'ai remplace Waalaxy par Claude — 0 euro au lieu de 99 euros/mois

Waalaxy, Lemlist, Phantombuster. Entre 39 et 139 euros par mois pour automatiser trois actions sur LinkedIn : chercher des profils, envoyer des demandes de connexion, et envoyer un message quand ils acceptent.

Trois etapes. 1 200 euros par an.

Claude peut controler un navigateur. Claude peut lire des profils LinkedIn. Claude peut ecrire des messages personnalises. Alors j'ai cree **Claudin** — un outil open-source qui fait la meme chose, gratuitement.

---

## Mes resultats au bout d'un jour

```
Claudin — Jour 1

Profils trouves :  15
Invitations :      15 (15/100 cette semaine)
Acceptations :     2
Messages envoyes : 2
```

Deux acheteurs d'une grande enseigne de bricolage ont accepte ma demande et recu un message de suivi automatique en quelques heures. L'un est directeur.

Cout total : 0 euro.

Ce que Waalaxy facture 99 euros/mois pour le meme resultat.

Et le pipeline continue de tourner :

```
=== CLAUDIN DASHBOARD ===

Pipeline :
  Trouves :   ||||||||||||||| 15
  Invites :   ||||||||||||| 13
  Connectes : || 2
  Messages :  || 2

Taux d'acceptation : 13% (2/15)
Invitations cette semaine : 15/100
```

---

## Comment ca marche

Claudin est un ensemble de skills pour Claude Code — des fichiers Markdown qui guident Claude dans l'execution de taches. Il utilise l'extension Claude in Chrome pour controler votre navigateur LinkedIn, exactement comme vous le feriez manuellement, mais en automatique.

Le flow est simple :

### 1. Configuration (5 minutes, une seule fois)

Lancez `/claudin-setup`. L'assistant vous pose quelques questions :

- **Langue** — francais ou anglais ? Tout s'adapte.
- **Plan LinkedIn** — gratuit ou Premium ? Claudin ajuste sa strategie (les notes personnalisees sont limitees en gratuit).
- **Votre activite** — decrivez ce que vous faites en une phrase.
- **Votre cible** — decrivez votre client ideal.
- **Mots-cles** — Claude analyse vos reponses et propose 6-8 recherches LinkedIn. Vous cochez celles qui vous semblent pertinentes.
- **Messages** — Claude redige une note de connexion et un message de suivi adaptes a votre cible. Vous relisez et ajustez.
- **Planification** — chaque matin, chaque soir, ou manuel uniquement.

Exemple concret de ce que ca donne :

> **Mon activite :** "Je developpe un assistant IA pour les acheteurs en foire"
>
> **Ma cible :** "Responsables sourcing et chefs de produit dans le retail francais"
>
> **Claude propose :**
> - acheteur sourcing foire
> - category manager retail
> - sourcing manager import
> - responsable achats international
>
> **Note de connexion generee :**
> "Bonjour {prenom}, je developpe un outil IA pour les acheteurs en foire : photo d'un stand, fiche fournisseur auto. Curieux d'avoir votre avis !"

Vous validez. Config sauvegardee. C'est pret.

### 2. Prospection quotidienne (automatique)

Lancez `/claudin` — ou laissez le scheduler le faire pour vous.

**Recherche** — Claudin ouvre LinkedIn dans Chrome, lance vos recherches, et scanne les resultats. Il lit chaque profil : poste, entreprise, localisation. Il ignore les profils non pertinents (recruteurs, commerciaux, RH) et ajoute les bons profils au tracker.

**Connexion** — Pour chaque prospect trouve, Claudin clique sur "Se connecter" et ajoute votre note personnalisee. Delais aleatoires entre chaque action (15-30 secondes) pour mimer un comportement humain. Max 15 par session, 100 par semaine — les limites LinkedIn sont respectees.

**Message** — Claudin verifie votre page de connexions. Quand quelqu'un accepte votre invitation, il envoie automatiquement votre message de suivi. Tout est trace dans un fichier local.

### 3. Suivi

Lancez `/claudin-dashboard` pour voir ou vous en etes :

```
Pipeline :
  15 trouves -> 13 invites -> 2 connectes -> 2 messages envoyes

Par campagne :
  "Retail sourcing"  — 15 invites | 2 connectes | 2 messages

Top entreprises :
  Castorama (5) | Leroy Merlin (1) | Blancheporte (1)
```

Rapports quotidiens generes en JSON et Markdown. Tout reste sur votre machine.

---

## Ce qu'il vous faut

Trois choses :

1. **L'app Claude Code** — desktop ou web sur [claude.ai/code](https://claude.ai/code). Attention : pas le terminal CLI, l'app.
2. **L'extension Claude in Chrome** — disponible sur le Chrome Web Store. C'est elle qui permet a Claude de controler votre navigateur.
3. **Un compte LinkedIn** — gratuit ou Premium. Claudin s'adapte a votre plan.

Ensuite :

```
git clone https://github.com/bacleclement/claudin.git
```

Ouvrez le dossier dans Claude Code. Lancez `/claudin-setup`. 5 minutes de configuration. C'est tout.

---

## Les limites (soyons honnetes)

**Claudin a besoin de votre PC allume avec Chrome ouvert.** Waalaxy tourne dans le cloud — votre PC peut etre eteint. C'est le seul vrai avantage de Waalaxy.

**Pas de sequences multi-messages (pour l'instant).** Claudin fait : connexion, puis un message quand la personne accepte. Pas de "relance J+3, relance J+7". C'est prevu pour la v2, mais honnetement 80% des reponses arrivent au premier message.

**Risque LinkedIn.** Toute automation LinkedIn comporte un risque. Claudin minimise ce risque : delais humains, respect des quotas, arret automatique en cas d'alerte. Mais le risque zero n'existe pas.

---

## Pourquoi j'ai cree ca

Je construis Gotchi, un assistant IA pour les acheteurs en foire. J'avais besoin de contacter des acheteurs pour tester mon produit. J'ai regarde Waalaxy — 99 euros/mois pour envoyer des demandes de connexion et des messages. J'ai regarde mon terminal avec Claude Code deja ouvert. Le choix etait evident.

Un apres-midi plus tard, Claudin existait. Deux acheteurs m'ont repondu le jour meme.

Si ca peut servir a d'autres, c'est open-source.

---

**GitHub :** [github.com/bacleclement/claudin](https://github.com/bacleclement/claudin)

Mettez une etoile si ca vous evite un abonnement a 99 euros/mois.

*Clement Bacle — [LinkedIn](https://linkedin.com/in/clementbacle)*
