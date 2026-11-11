# MEMORIA

> *Every moment deserves a place in the stars.*

Plateforme SaaS de livraison, de gestion et de partage privé de photographies d'événements. Un photographe crée un événement, importe ses photos et les attribue à ses invités ; chaque invité se connecte avec son compte Google et retrouve son album personnel, et la galerie collective s'il y est autorisé, dans un univers de ciel étoilé.

## État du projet

**Phase d'analyse. Aucun code applicatif n'a été écrit.**

Le dépôt contient les documents de référence tirés de la conversation de conception, et l'analyse technique qui en découle. Le développement commencera après validation du [rapport d'analyse](DOCUMENTATION/RAPPORT_ANALYSE_CLAUDE.md).

## Par où commencer

1. [`DOCUMENTATION/RAPPORT_ANALYSE_CLAUDE.md`](DOCUMENTATION/RAPPORT_ANALYSE_CLAUDE.md) — ce qui a été relevé, les 12 décisions à valider, les questions ouvertes.
2. [`DOCUMENTATION/01_VISION_PROJET.md`](DOCUMENTATION/01_VISION_PROJET.md) — le projet en deux pages.

## Documents

| Document | Contenu | Nature |
|---|---|---|
| [`00_CONVERSATION_SOURCE.md`](DOCUMENTATION/00_CONVERSATION_SOURCE.md) | La conversation de conception, les 10 messages | Archive fidèle — ne pas modifier |
| [`01_VISION_PROJET.md`](DOCUMENTATION/01_VISION_PROJET.md) | Concept, publics, décisions fondatrices, principes | Synthèse |
| [`02_CAHIER_DES_CHARGES.md`](DOCUMENTATION/02_CAHIER_DES_CHARGES.md) | Cahier des charges fonctionnel et technique V1.0 | Copie fidèle — ne pas modifier |
| [`03_ARCHITECTURE_TECHNIQUE.md`](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md) | Stack, arborescence, routes, flux, pipeline image, environnements | Proposé |
| [`04_ROADMAP_DEVELOPPEMENT.md`](DOCUMENTATION/04_ROADMAP_DEVELOPPEMENT.md) | Sept phases de développement, premier jalon, suite | Proposé |
| [`05_DATABASE_SCHEMA.md`](DOCUMENTATION/05_DATABASE_SCHEMA.md) | Collections Firestore, fichiers, index, interfaces TypeScript | Proposé |
| [`06_SECURITY.md`](DOCUMENTATION/06_SECURITY.md) | Menaces, rôles, chaîne d'autorisation, règles, invitations | Proposé |
| [`07_FIREBASE_COSTS.md`](DOCUMENTATION/07_FIREBASE_COSTS.md) | Postes de coût, estimation par mariage, leviers | Estimation |
| [`08_TESTING.md`](DOCUMENTATION/08_TESTING.md) | Niveaux de test, 43 scénarios de sécurité, jeu de démonstration | Plan |
| [`RAPPORT_ANALYSE_CLAUDE.md`](DOCUMENTATION/RAPPORT_ANALYSE_CLAUDE.md) | Incohérences, manques, risques, décisions à valider | **À valider** |
| [`PROMPTS/PROMPT_INITIAL_CLAUDE.md`](PROMPTS/PROMPT_INITIAL_CLAUDE.md) | Prompt de mission et prompts d'accompagnement | Copie fidèle — ne pas modifier |
| [`CLAUDE.md`](CLAUDE.md) | Contexte permanent pour les sessions de développement | Tenu à jour |

### Correspondance avec les documents demandés

La conversation demande ces documents sous deux nommages. Ils sont réunis dans `DOCUMENTATION/`.

| Demandé par le prompt de mission (§8) | Demandé au message 8 | Fichier |
|---|---|---|
| `README.md` | `README.md` | `README.md` |
| — | `01_VISION_PROJET.md` | `DOCUMENTATION/01_VISION_PROJET.md` |
| — | `02_CAHIER_DES_CHARGES.md` | `DOCUMENTATION/02_CAHIER_DES_CHARGES.md` |
| `ARCHITECTURE.md` | `03_ARCHITECTURE_TECHNIQUE.md` | `DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md` |
| `DEVELOPMENT_ROADMAP.md` | `04_ROADMAP_DEVELOPPEMENT.md` | `DOCUMENTATION/04_ROADMAP_DEVELOPPEMENT.md` |
| `DATABASE_SCHEMA.md` | — | `DOCUMENTATION/05_DATABASE_SCHEMA.md` |
| `SECURITY.md` | — | `DOCUMENTATION/06_SECURITY.md` |
| `FIREBASE_COSTS.md` | — | `DOCUMENTATION/07_FIREBASE_COSTS.md` |
| `TESTING.md` | — | `DOCUMENTATION/08_TESTING.md` |
| — | `RAPPORT_ANALYSE_CLAUDE.md` | `DOCUMENTATION/RAPPORT_ANALYSE_CLAUDE.md` |
| — | `PROMPT_INITIAL_CLAUDE.md` | `PROMPTS/PROMPT_INITIAL_CLAUDE.md` |
| — | `CLAUDE.md` | `CLAUDE.md` |

### Diagrammes

| Diagramme demandé | Emplacement |
|---|---|
| Architecture globale | [`03` §2](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#2-vue-densemble) |
| Relations des collections | [`05` §2](DOCUMENTATION/05_DATABASE_SCHEMA.md#2-relations-des-collections) |
| Authentification | [`03` §6.1](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#61-authentification) et [`06` §6](DOCUMENTATION/06_SECURITY.md#6-rattachement-dune-invitation-à-un-compte) |
| Création d'événement | [`03` §6.2](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#62-création-dun-événement) |
| Importation des photos | [`03` §6.3](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#63-importation-des-photos) |
| Attribution des albums | [`03` §6.4](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#64-attribution-des-photos-aux-albums) |
| Contrôle des permissions | [`06` §4](DOCUMENTATION/06_SECURITY.md#4-chaîne-dautorisation-dune-photo) et [`03` §6.5](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#65-contrôle-des-permissions) |
| Parcours invité | [`03` §6.6](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#66-parcours-invité) |

## Stack prévue

React, TypeScript, Vite, Tailwind CSS, React Router, Framer Motion, Lucide · Firebase Authentication, Cloud Firestore, Cloud Storage, Hosting, App Check, Cloud Functions seulement lorsque nécessaire · Vitest, React Testing Library, Playwright, Firebase Emulator Suite.

Versions et ajouts proposés : [`03` §3](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#3-stack-et-versions).

## Commandes

Aucune pour l'instant : le projet n'est pas initialisé. Les commandes prévues sont décrites dans [`03` §11](DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md#11-environnements).
