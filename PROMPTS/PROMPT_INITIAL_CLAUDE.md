<!--
  Copie fidèle du prompt de mission rédigé dans la conversation source (message 6),
  suivie des deux messages d'accompagnement (messages 8 et 10). NE PAS MODIFIER.
-->

> **Source :** conversation de conception MEMORIA, message 6 — reproduit à l'identique ([archive](../DOCUMENTATION/00_CONVERSATION_SOURCE.md)).
> **Correspondance des documents demandés au §8** : voir le tableau du [`README.md`](../README.md).

# MISSION : CONSTRUIRE MEMORIA — PLATEFORME SAAS PHOTOGRAPHIQUE

Tu es un architecte logiciel senior, développeur React/TypeScript, expert Firebase, sécurité multi-tenant et UX/UI premium.

Tu dois concevoir et développer MEMORIA, une plateforme SaaS de gestion et de partage privé de photographies d'événements.

## 1. Vision du produit

MEMORIA transforme les photographies de mariages, anniversaires, cérémonies et événements en expériences numériques immersives.

Identité visuelle :
- Ciel étoilé.
- Constellations interactives.
- Bleu nuit profond.
- Or subtil.
- Ambiance élégante, émotionnelle et premium.

Slogan :
"Every moment deserves a place in the stars."

La plateforme possède deux publics principaux :
- Photographes et studios qui gèrent leurs événements.
- Invités qui consultent leurs albums personnels et les galeries collectives.

Le produit doit être multi-photographes dès sa conception.

## 2. Stack technique imposée

Frontend :
- React.
- TypeScript.
- Vite.
- Tailwind CSS.
- React Router.
- Framer Motion.
- Lucide Icons.

Backend :
- Firebase Authentication.
- Cloud Firestore.
- Firebase Storage.
- Firebase Hosting.
- Firebase App Check.
- Cloud Functions uniquement lorsque nécessaire.

Tests :
- Vitest.
- React Testing Library.
- Playwright.
- Firebase Emulator Suite.

Privilégier une architecture modulaire, maintenable et économique.

## 3. Architecture obligatoire

Créer une séparation claire entre :
- Site public.
- Dashboard photographe.
- Gestion des événements.
- Gestion des photos.
- Gestion des invités.
- Albums personnels.
- Galerie collective.
- Expérience immersive.
- Administration globale.

Utiliser une architecture multi-tenant avec workspaces.

Chaque événement doit être associé à un workspace.

Chaque utilisateur invité doit être identifié par son UID Firebase Authentication.

Ne jamais faire confiance à un email ou à un identifiant transmis uniquement par le navigateur.

## 4. Fonctionnalités du MVP

### Authentification
- Google Sign-In.
- Inscription photographe.
- Gestion des sessions.
- Gestion des permissions.
- Protection des routes.

### Workspaces
- Création automatique du workspace propriétaire.
- Gestion des collaborateurs.
- Rôles et permissions.
- Isolation stricte des données.

### Événements
- Création.
- Modification.
- Suppression contrôlée.
- Publication.
- Archivage.
- Personnalisation.

### Photographies
- Importation multiple.
- Glisser-déposer.
- Optimisation locale.
- Miniatures.
- Version d'affichage.
- Original facultatif.
- Détection des doublons.
- Gestion des erreurs.
- Attribution aux invités.

### Invités
- Ajout individuel.
- Importation CSV.
- Invitations email.
- Vérification de l'identité Google.
- Révocation des accès.

### Albums
- Album individuel par invité.
- Galerie collective.
- Favoris.
- Visionneuse.
- Téléchargement.
- Pagination.

### Expérience immersive
- Page événement étoilée.
- Constellations.
- Navigation interactive.
- Galerie photographique premium.
- Responsive mobile.

## 5. Sécurité non négociable

- Aucune galerie publique par défaut.
- Aucune lecture non autorisée de photographies.
- Aucun accès croisé entre workspaces.
- Aucun accès à l'album personnel d'un autre invité.
- Vérification des permissions côté backend.
- Firestore Security Rules strictes.
- Storage Security Rules strictes.
- App Check.
- Protection contre les modifications frauduleuses des rôles.
- Protection contre les invitations falsifiées.
- Liens de téléchargement temporaires après vérification des autorisations.
- Tests négatifs pour tous les scénarios de violation.

Ne jamais utiliser des URL de téléchargement permanentes comme mécanisme d'autorisation.

Ne jamais utiliser une simple URL cachée pour protéger un album.

## 6. Maîtrise des coûts

L'application doit être conçue pour minimiser :
- Lectures Firestore.
- Écritures Firestore.
- Transferts Storage.
- Taille des fichiers.
- Appels Cloud Functions.

Prévoir :
- Pagination.
- Lazy loading.
- Compression.
- Miniatures.
- Cache adapté.
- Compteurs agrégés.
- Quotas par workspace.
- Statistiques de stockage et de trafic.

Ne pas créer une Cloud Function pour chaque opération si elle peut être effectuée de manière sécurisée autrement.

Aucun environnement de développement ne doit pouvoir modifier accidentellement la production.

## 7. Règles de développement

Tu dois travailler progressivement.

Avant chaque phase :
1. Analyser l'architecture existante.
2. Identifier les dépendances.
3. Présenter le plan de réalisation.
4. Identifier les risques.
5. Développer uniquement le périmètre prévu.
6. Exécuter les tests.
7. Vérifier le build.
8. Documenter les fichiers modifiés.

Ne jamais supprimer une fonctionnalité existante sans justification.

Ne jamais remplacer du code fonctionnel par des données fictives pour faire passer un test.

Ne jamais prétendre qu'une fonctionnalité est terminée sans vérification.

Ne pas introduire de secrets dans le frontend ou dans Git.

## 8. Documentation obligatoire

Créer :
- README.md
- ARCHITECTURE.md
- DATABASE_SCHEMA.md
- SECURITY.md
- FIREBASE_COSTS.md
- DEVELOPMENT_ROADMAP.md
- TESTING.md

Ajouter les diagrammes Mermaid :
- Architecture globale.
- Relations des collections.
- Authentification.
- Création d'événement.
- Importation des photos.
- Attribution des albums.
- Contrôle des permissions.
- Parcours invité.

## 9. Première mission

NE DÉVELOPPE PAS TOUTE L'APPLICATION IMMÉDIATEMENT.

Commence par :

A. Examiner le dossier et son contenu.

B. Vérifier Node.js, npm et les dépendances disponibles.

C. Vérifier si un projet Firebase existe déjà.

D. Préparer l'architecture technique et l'arborescence.

E. Définir les interfaces TypeScript et le schéma Firestore.

F. Définir la stratégie de sécurité multi-tenant.

G. Préparer Firebase Emulator Suite.

H. Préparer les tests de sécurité.

I. Présenter un rapport contenant :
- Architecture proposée.
- Arborescence.
- Modèle de données.
- Dépendances.
- Risques.
- Coûts potentiels.
- Plan de développement par phases.

Attends ma validation avant de commencer le développement complet.

## 10. Résultat attendu

Un produit professionnel, sécurisé, rapide, responsive et commercialisable.

La priorité est la confidentialité des photographies et la fiabilité des albums personnels.

Le design étoilé doit être exceptionnel, mais ne doit jamais compromettre les performances ou l'accessibilité.

Tu dois agir comme un véritable ingénieur logiciel, pas comme un simple générateur de maquettes.

---

# Annexe A — Prompt « dossier DOCUMENTATION » (message 8)

> Reproduit à l'identique.

Tu vas travailler sur mon nouveau projet professionnel : MEMORIA.

Je t'ai fourni plusieurs documents dans le dossier DOCUMENTATION.

IMPORTANT : avant toute intervention, prends le temps de lire intégralement et attentivement tous les documents suivants :

- DOCUMENTATION/01_VISION_PROJET.md
- DOCUMENTATION/02_CAHIER_DES_CHARGES.md
- DOCUMENTATION/03_ARCHITECTURE_TECHNIQUE.md
- DOCUMENTATION/04_ROADMAP_DEVELOPPEMENT.md
- PROMPTS/PROMPT_INITIAL_CLAUDE.md

Ne commence pas à coder immédiatement.

### ÉTAPE 1 — Compréhension

Lis tous les documents et comprends :
- La vision commerciale de MEMORIA.
- Les besoins des photographes.
- Les besoins des invités.
- Le fonctionnement des albums individuels et collectifs.
- L'expérience immersive du ciel étoilé.
- L'architecture multi-photographes.
- La sécurité Firebase.
- Les contraintes de coûts.

### ÉTAPE 2 — Analyse technique

Examine les choix techniques et identifie :
- Les incohérences éventuelles.
- Les fonctionnalités manquantes.
- Les risques de sécurité.
- Les risques de coûts Firebase.
- Les problèmes de performances.
- Les améliorations nécessaires.

Ne modifie pas les décisions importantes sans me les expliquer.

### ÉTAPE 3 — Préparation du projet

Prépare :
- L'architecture finale.
- L'arborescence complète.
- Le schéma Firestore.
- Les règles Firestore et Storage.
- Le système d'authentification.
- Le système de permissions.
- Le plan de tests.

### ÉTAPE 4 — Rapport

Crée un rapport détaillé dans :

DOCUMENTATION/RAPPORT_ANALYSE_CLAUDE.md

Présente les décisions techniques, les risques et les corrections proposées.

### ÉTAPE 5 — Validation

Attends ma validation avant le développement.

Ne génère pas toute l'application en une seule fois.

Nous travaillerons phase par phase, avec tests et validation après chaque étape.

Tu es responsable de la qualité architecturale, de la sécurité, de la maintenabilité et de la performance de MEMORIA.

Je veux un véritable produit SaaS professionnel, pas une simple démonstration visuelle.

---

# Annexe B — Prompt « lien de conversation partagé » (message 10)

> Reproduit à l'identique. C'est ce message qui a lancé la présente analyse.

Voici le lien de ma conversation ChatGPT qui contient toute la conception de mon projet MEMORIA :

[COLLER LE LIEN ICI]

Je veux que tu lises attentivement l'intégralité de cette conversation, depuis le début jusqu'à la fin.

Elle contient :
- La vision du projet.
- Les fonctionnalités souhaitées.
- Le cahier des charges.
- L'architecture technique.
- Les choix technologiques.
- Les décisions concernant les photographes et les invités.
- Le concept du ciel étoilé et des albums privés.

Ne te contente pas d'un résumé rapide.

Considère cette conversation comme la référence principale du projet.

Après lecture, prépare une analyse technique complète et crée les documents de référence dans le projet.

Ne commence pas le développement avant de m'avoir présenté ton analyse et obtenu ma validation.
