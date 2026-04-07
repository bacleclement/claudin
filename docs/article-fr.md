# J'ai vire Waalaxy et je prospecte sur LinkedIn avec Claude — pour 0 euro

Waalaxy : 99 euros/mois. Lemlist : 69 euros. Phantombuster : 56 euros.

Pour faire quoi ? Chercher des gens sur LinkedIn, leur envoyer une demande de connexion, et un message quand ils acceptent.

Trois etapes. Plus de 1 000 euros par an.

J'ai regarde Claude Code ouvert sur mon ecran. Claude controle un navigateur. Claude lit des profils. Claude ecrit des messages. Le calcul etait vite fait.

J'ai cree **Claudin**. Open-source, gratuit, et ca m'a pris un apres-midi.

---

## Jour 1 : les chiffres

```
Profils trouves :  15
Invitations :      15
Acceptations :     2
Messages envoyes : 2
```

Deux acheteurs d'une grosse enseigne de bricolage ont accepte dans la journee. L'un est directeur. Message de suivi envoye automatiquement.

Cout : 0 euro. Waalaxy facture 99 euros/mois pour ca.

Le dashboard en temps reel :

```
Pipeline :
  Trouves :   ||||||||||||||| 15
  Invites :   ||||||||||||| 13
  Connectes : || 2
  Messages :  || 2

Taux d'acceptation : 13%
Quota semaine : 15/100 invitations
```

---

## Le setup : 5 minutes, une seule fois

Vous clonez le repo, vous ouvrez dans Claude Code, et vous lancez `/claudin-setup`.

L'assistant vous pose 7 questions. En francais ou en anglais, au choix.

Il veut savoir :
- Ce que vous faites
- Qui vous ciblez
- Votre plan LinkedIn (gratuit ou Premium)

A partir de vos reponses, Claude genere des mots-cles de recherche LinkedIn et vous les propose. Vous cochez ceux qui collent :

> - [x] acheteur sourcing foire
> - [x] category manager retail
> - [x] sourcing manager import
> - [ ] procurement manager
> - [x] responsable achats international

Ensuite il redige vos messages. Note de connexion, message de suivi — tout est pre-ecrit par l'IA, adapte a votre cible. Vous relisez, vous ajustez si besoin, vous validez.

Dernier choix : quand voulez-vous que ca tourne ? Chaque matin ? Chaque soir ? Manuellement ?

C'est pret.

---

## Le flow quotidien

Lancez `/claudin` (ou laissez le scheduler le faire).

**Etape 1 — Recherche.** Claude ouvre LinkedIn dans votre Chrome, tape vos mots-cles, scanne les resultats. Il lit les profils, filtre les non-pertinents (recruteurs, RH, commerciaux), et garde les bons. Tout est ajoute au tracker.

**Etape 2 — Connexion.** Pour chaque prospect, Claude clique "Se connecter", ajoute votre note personnalisee, et passe au suivant. 15-30 secondes entre chaque action. Pas plus de 15 par session, 100 par semaine. LinkedIn ne voit rien.

**Etape 3 — Message.** Claude check vos connexions recentes. Quelqu'un a accepte ? Il ouvre la conversation et envoie votre message de suivi. Automatique.

Lancez `/claudin-dashboard` pour voir le pipeline :

```
15 trouves -> 13 invites -> 2 connectes -> 2 messages

Top entreprises : Castorama (5) | Leroy Merlin (1) | Blancheporte (1)
```

---

## Ce qu'il vous faut

1. **L'app Claude Code** — desktop ou [claude.ai/code](https://claude.ai/code). Pas le terminal, l'app.
2. **L'extension Claude in Chrome** — Chrome Web Store. C'est ce qui permet a Claude de piloter votre navigateur.
3. **Un compte LinkedIn** — gratuit ou Premium.

```
git clone https://github.com/bacleclement/claudin.git
```

Ouvrez dans Claude Code. `/claudin-setup`. 5 minutes. C'est tout.

---

## Les limites

Je prefere etre transparent :

**Votre PC doit etre allume.** Waalaxy tourne dans le cloud. Claudin a besoin de Chrome ouvert. C'est le seul vrai avantage de Waalaxy sur Claudin.

**Un seul message apres acceptation.** Pas de sequence "relance J+3, J+7". Prevu pour la v2 — mais honnetement, 80% des reponses viennent du premier message.

**Le risque LinkedIn existe.** Toute automation comporte un risque. Claudin le minimise : delais aleatoires, quotas respectes, arret auto en cas d'alerte. Mais le zero risque n'existe pas. A vous de juger.

---

## Pourquoi j'ai fait ca

Je construis un produit pour les acheteurs en foire (Gotchi). J'avais besoin de beta-testeurs. J'ai regarde les outils de prospection LinkedIn. 99 euros/mois pour faire des trucs que Claude fait deja.

Un apres-midi plus tard, Claudin marchait. Deux acheteurs m'ont repondu le jour meme.

C'est open-source. Si ca vous sert, tant mieux.

---

**Le repo :** [github.com/bacleclement/claudin](https://github.com/bacleclement/claudin)

Une etoile si ca vous economise 99 euros/mois.

*Clement Bacle — [LinkedIn](https://linkedin.com/in/clementbacle)*
