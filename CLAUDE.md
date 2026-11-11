# MEMORIA — Contexte permanent

Ce fichier est lu au début de chaque session de développement. Il doit rester court et à jour.

## Vision

MEMORIA est une plateforme SaaS de livraison et de partage privé de photographies d'événements. Un photographe crée un événement, importe ses photos et les attribue à ses invités ; chaque invité se connecte avec Google et retrouve son album personnel et, s'il y est autorisé, la galerie collective, dans un univers de ciel étoilé. Slogan : *Every moment deserves a place in the stars.*

Priorité absolue : **la confidentialité des photographies et la fiabilité des albums personnels.**

## État actuel

- **Phase : analyse terminée, en attente de validation par Wael.** Aucun code applicatif n'existe.
- Prochaine étape après validation : phase 1 de `DOCUMENTATION/04_ROADMAP_DEVELOPPEMENT.md`.
- **Ne pas commencer le développement tant que le rapport d'analyse n'est pas validé.**

## Sources de vérité, par ordre de priorité

1. Les décisions validées par Wael dans la conversation de travail (à reporter ci-dessous).
2. `DOCUMENTATION/00_CONVERSATION_SOURCE.md`, `02_CAHIER_DES_CHARGES.md`, `PROMPTS/PROMPT_INITIAL_CLAUDE.md` : la référence d'origine. **Ne jamais les modifier.**
3. `DOCUMENTATION/03` à `08` et `RAPPORT_ANALYSE_CLAUDE.md` : architecture et corrections proposées.

En cas de contradiction entre ces documents, le signaler à Wael au lieu de trancher seul.

## Décisions fondatrices (actées)

- SaaS multi-photographes : une seule plateforme, un workspace par photographe, isolation stricte.
- Album personnel par invité **et** galerie collective.
- Univers visuel : ciel étoilé et constellations.
- Site web responsive ; pas d'application native, pas de PWA au MVP.
- Connexion par Google uniquement. Attribution manuelle. Pas de reconnaissance faciale en V1.
- Interface en français, multilingue préparé.

## Décisions d'architecture (proposées — à valider)

Détail et justification : `DOCUMENTATION/RAPPORT_ANALYSE_CLAUDE.md` §8.

| # | Décision proposée | Statut |
|---|---|---|
| D-01 | Fichiers lus directement par le SDK (`getBlob`) sous règles Storage ; aucune URL de téléchargement | À valider |
| D-02 | `assignedGuestIds` (≤ 200) sur le document de la photo | À valider |
| D-03 | Fiche `guests/{guestId}` créée par le photographe, rattachée à l'UID par la fonction `claimInvitations` | À valider |
| D-04 | Pas de collection `albums` : l'album est une requête | À valider |
| D-05 | Workspace créé « en attente », activé par le super administrateur | À valider |
| D-06 | Région Firebase en Europe | À valider |
| D-07 | Trois fichiers par photo ; originaux en option par événement | À valider |
| D-08 | Aperçu public minimal sans photo ; adresse d'événement à suffixe aléatoire | À valider |
| D-09 | Règles et tests négatifs dans chaque phase | À valider |
| D-10 | Lien et QR code d'abord, emails ensuite | À valider |
| D-11 | Placement des fonctions flottantes du MVP | À valider |
| D-12 | Bibliothèques ajoutées : TanStack Query, Zod, React Hook Form, i18next, exifr, fflate, qrcode | À valider |

## Stack

React · TypeScript · Vite · Tailwind CSS · React Router · Framer Motion (`motion`) · Lucide
Firebase : Authentication (Google), Firestore, Storage, Hosting, App Check, Functions seulement lorsque nécessaire
Tests : Vitest · React Testing Library · Playwright · Firebase Emulator Suite · `@firebase/rules-unit-testing`

## Contraintes de sécurité non négociables

- Tout est refusé par défaut dans `firestore.rules` et `storage.rules`.
- Aucune galerie publique, aucune lecture anonyme de photo.
- Aucun accès croisé entre workspaces ; aucun accès à l'album d'un autre invité.
- **Ne jamais appeler `getDownloadURL`.** Ne jamais protéger un contenu par une URL difficile à deviner.
- Les règles Storage lisent **2 documents Firestore au plus** : ne pas ajouter une troisième lecture.
- Aucun rôle dans un document que son titulaire peut modifier.
- L'email d'un invité se lit dans le jeton vérifié, jamais dans une requête.
- Les routes et l'interface ne protègent rien : seules les règles comptent.
- Toute règle ajoutée a un test positif et un test négatif.
- Aucun secret dans le frontend ni dans Git. Les tests n'utilisent que le projet `demo-memoria`.

## Règles de développement

Issues du prompt de mission §7 :

1. Travailler phase par phase. Avant chaque phase : analyser l'existant, identifier les dépendances, présenter le plan et les risques.
2. Développer uniquement le périmètre prévu.
3. Exécuter les tests et vérifier le build avant d'annoncer qu'une phase est terminée.
4. Documenter les fichiers modifiés ; tenir ce fichier à jour.
5. Ne jamais supprimer une fonctionnalité existante sans justification.
6. Ne jamais remplacer du code fonctionnel par des données fictives pour faire passer un test.
7. Ne jamais prétendre qu'une fonctionnalité est terminée sans vérification.
8. Attendre la validation de Wael entre deux phases.
9. Ne pas modifier une décision importante sans l'expliquer.

## Conventions

- Documentation et interface en français. Code, noms de fichiers et identifiants en anglais.
- Une fonctionnalité = un dossier dans `src/features/` avec ses composants, hooks, services et types.
- Un seul point d'initialisation de Firebase (`src/services/firebase`) et un seul point de lecture des fichiers privés (`src/services/storage`).
- Pagination par curseur partout ; aucune lecture de collection entière.
- Maîtrise des coûts : pas de Cloud Function dans le chemin de lecture des photos ; ne jamais servir un original par défaut.

## Commandes

Le projet n'est pas encore initialisé. Commandes prévues en phase 1 :

```text
npm install · npm run dev · npm run build · npm run test · npm run test:rules · npm run test:e2e · npm run seed
```

## Environnement

- Vérifié : Node.js 22, npm 10, OpenJDK 21.
- Environnement cloud : réseau limité. Le registre npm est accessible ; `storage.googleapis.com` (téléchargement des émulateurs) doit être autorisé avant la phase 1.
- Aucun projet Firebase n'est encore associé au dépôt.
