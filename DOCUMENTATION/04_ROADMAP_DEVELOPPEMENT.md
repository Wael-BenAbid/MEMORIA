# MEMORIA — Feuille de route de développement (DEVELOPMENT_ROADMAP)

> **Statut : proposé, en attente de validation.** Aucune phase n'a commencé.

## 1. D'où vient cette feuille de route

La conversation contient trois plans, qui ne se recouvrent pas exactement :

| Source | Découpage |
|---|---|
| Message 2 | 4 phases produit : MVP, expérience immersive, intelligence artificielle, commercialisation |
| Cahier §18 | 4 phases produit : MVP fonctionnel, expérience MEMORIA, gestion professionnelle, intelligence artificielle |
| Messages 6 et 8 | 7 phases de développement, de l'initialisation aux tests |

Cette feuille de route les réunit : **les phases produit sont celles du cahier §18** (document de référence, plus récent que le message 2) et **les sept phases de développement des messages 6 et 8 réalisent les deux premières**.

| Phase produit (cahier §18) | Phases de développement |
|---|---|
| 1 — MVP fonctionnel | 1 à 5 |
| 2 — Expérience MEMORIA | 6 |
| Mise en production | 7 |
| 3 — Gestion professionnelle | Après le lancement |
| 4 — Intelligence artificielle | Sous réserve de validation juridique, de coût et de demande |

**Un seul changement par rapport au backlog de la conversation :** la sécurité n'attend pas la phase 7. Chaque phase écrit ses règles et ses tests négatifs (décision D-09). La phase 7 devient l'audit final et le lancement.

**Ordre impératif** (message 6) : ne pas commencer par les étoiles. D'abord le parcours complet photographe → importation → attribution → connexion de l'invité → album privé.

## 2. Phases de développement

Chaque phase suit la méthode du prompt de mission §7 : analyse, plan, risques, développement du seul périmètre prévu, tests, build, compte rendu, **puis validation avant la phase suivante**.

### Phase 1 — Initialisation et architecture

| | |
|---|---|
| **Tâches (backlog)** | Créer le projet React + TypeScript + Vite · Configurer Tailwind et les composants UI · Configurer Firebase et les émulateurs · Créer les types et services de base |
| **Ajouts** | Règles Firestore et Storage « tout interdit » · Harnais de tests de sécurité · Jeu de données de démonstration · Intégration continue · **Prototype de validation des hypothèses H1 à H5** |
| **Sécurité** | S-04, S-37, S-45 ; structure des tests pour tous les suivants |
| **Terminé quand** | `npm run dev`, `build`, `test`, `test:rules` fonctionnent ; les hypothèses H1 à H3 sont prouvées sur émulateur ; un écran minimal affiche une image privée lue sous règles |

Le prototype compte : il confirme, avant d'écrire le moindre écran, que les règles Storage tiennent en 2 lectures et que la requête « mon album » passe les règles. S'il échoue, on bascule sur le repli (fonction + URL signée) sans rien avoir à défaire.

### Phase 2 — Authentification et workspace

| | |
|---|---|
| **Tâches (backlog)** | Connexion Google · Inscription photographe · Création du workspace · Gestion des rôles et permissions · Protection des routes |
| **Précisions** | Workspace créé « en attente », activé par le super administrateur · Page d'administration minimale · Détection des navigateurs intégrés · Modèle des collaborateurs prêt, sans écran de gestion |
| **Sécurité** | S-25 à S-28, S-41, S-44 |
| **Terminé quand** | Un photographe s'inscrit, est activé, arrive sur un dashboard vide ; un inconnu ne peut rien lire ni écrire |

### Phase 3 — Gestion des événements

| | |
|---|---|
| **Tâches (backlog)** | Créer et modifier un événement · Gérer les statuts · Gérer les membres · Personnaliser l'événement |
| **Précisions** | Machine d'états · Titre, type, date, lieu, description, couverture, message d'accueil · Aperçu public et adresse à suffixe aléatoire · Liste des événements avec compteurs |
| **Sécurité** | S-03, S-34, S-43 |
| **Terminé quand** | Un photographe crée, modifie, publie, suspend et archive un événement ; un autre workspace ne le voit pas |

### Phase 4 — Photographies

| | |
|---|---|
| **Tâches (backlog)** | Importation multiple · Optimisation des images · Stockage privé · Miniatures et affichage · Attribution des photos |
| **Précisions** | Pipeline en Web Worker · Empreinte, doublons, reprise · Galerie administrative paginée · Catégories · Sélection multiple et attribution en lot · Visibilité et publication · Compteurs de stockage |
| **Sécurité** | S-06, S-12, S-33, S-38 à S-40 |
| **Terminé quand** | 100 photos importées, classées, attribuées ; une interruption d'import se reprend sans doublon ; aucun fichier n'est lisible hors du personnel |

L'attribution dépend des fiches invités : la saisie minimale des invités est donc avancée ici, et la phase 5 la complète.

### Phase 5 — Invités et albums

| | |
|---|---|
| **Tâches (backlog)** | Importation CSV des invités · Invitations par email · Connexion et vérification Gmail · Albums personnels · Galerie collective |
| **Précisions** | Fonctions `claimInvitations` et `syncEventAccess` · Lien et QR code de l'événement · Page « Mes événements » · Visionneuse plein écran · Favoris · Téléchargement d'une photo et d'une sélection · Révocation · Envoi des emails une fois le service et le domaine choisis |
| **Sécurité** | S-01, S-02, S-05, S-07, S-10, S-11, S-13 à S-24, S-29 à S-32, S-35, S-36, S-42 |
| **Terminé quand** | **Le premier jalon est atteint** (voir §3) |

### Phase 6 — Expérience immersive

| | |
|---|---|
| **Tâches (backlog)** | Landing page étoilée · Univers de chaque événement · Navigation par constellations · Galerie immersive · Responsive mobile |
| **Précisions** | Ciel en `canvas`, adapté à l'appareil · Animations désactivables · Navigation classique toujours disponible · Thèmes par événement · Timeline · Site public (accueil, fonctionnement, tarifs) |
| **Sécurité** | Aucune règle nouvelle ; les tests existants doivent rester verts |
| **Terminé quand** | Le parcours invité complet se fait dans l'univers étoilé, sur un téléphone d'entrée de gamme, sans ralentir la consultation des photos |

### Phase 7 — Audit, performance et lancement

| | |
|---|---|
| **Tâches (backlog)** | Règles Firestore · Règles Storage · Tests d'accès croisés · Tests E2E · Audit des coûts et performances |
| **Précisions** | Relecture complète des règles · Parcours de bout en bout sur mobile et ordinateur · Tests de performance · App Check en mode strict · En-têtes de sécurité · Projet de production, région, domaine · Budgets, alertes, arrêt d'urgence testé · Pages légales · Sauvegardes |
| **Terminé quand** | Les 13 critères du cahier §21 sont vérifiés un par un ; un vrai mariage peut être livré |

## 3. Premier jalon : un mariage entièrement fonctionnel

Défini au message 4 et rappelé au message 6. Atteint en fin de phase 5.

- 1 photographe, 1 mariage, 10 invités, 100 photographies, 10 albums personnels, 1 galerie collective.
- **Test de réussite :** chaque invité se connecte avec son Gmail, retrouve ses photos et ne peut pas consulter celles des autres sans autorisation.
- À faire ensuite, comme le recommande la conversation : tester réellement l'expérience sur ton téléphone, avec de vraies photos et de vraies personnes.
- **À mesurer à cette occasion :** le temps passé à saisir les invités et à attribuer les 100 photos. C'est le principal risque produit ([rapport, R-01](RAPPORT_ANALYSE_CLAUDE.md#7-risques)).

## 4. Placement des fonctions dont la phase variait

| Fonction | Cahier §18 | Prompt §4 | Retenu |
|---|---|---|---|
| Favoris | Phase 2 | MVP | Phase de développement 5 |
| QR code | Phase 2 | — | Phase 5 : simple image, nécessaire au partage |
| Personnalisation | Phase 2 (graphique) | MVP | Phase 3 pour le contenu ; phase 6 pour les thèmes |
| Collaborateurs | Phase 3 | MVP (workspaces) | Modèle et règles dès la phase 2 ; écrans après le lancement |
| Statistiques | Phase 3 (avancées) | — | Compteurs simples dès la phase 3 ; détail après le lancement |
| Timeline | Phase 2 | — | Phase 6 |

## 5. Après le lancement

### Phase produit 3 — Gestion professionnelle (cahier §18)

Collaborateurs (écrans) · Statistiques avancées · Notifications par email et dans l'application · Quotas par offre mesurés côté serveur · Branding professionnel · Rapports · Abonnements et facturation · Purge planifiée et durée de conservation · Journal d'audit infalsifiable · Filigrane optionnel · Procédure de signalement.

Résultat attendu : une plateforme commercialisable auprès d'autres photographes.

### Phase produit 4 — Intelligence artificielle (cahier §18)

Reconnaissance de visages avec consentement (« Find Me ») · Recherche de personnes · Classement automatique · Suggestions de photos · Livres photo.

Conditionnée à la validation juridique, aux coûts et à la demande réelle.

### Idées de la conversation non planifiées

Photo du jour · Message vidéo des mariés · Livre de souvenirs · Diaporama « cinéma » · Album souvenir en PDF · Albums familiaux · Site installable. Voir [`01_VISION_PROJET.md`](01_VISION_PROJET.md).

## 6. Ce dont j'aurai besoin de ta part

| Quand | Quoi |
|---|---|
| Maintenant | Validation du [rapport d'analyse](RAPPORT_ANALYSE_CLAUDE.md) : décisions D-01 à D-12, réponses aux questions |
| Début de la phase 1 | Autoriser le téléchargement des émulateurs dans l'environnement cloud |
| Avant la phase 5 | Choix du service d'email et du nom de domaine, si l'envoi d'emails est voulu au MVP |
| Avant la phase 7 | Création du projet Firebase de production (forfait Blaze), choix de la région, budget d'alerte |
| Avant la commercialisation | Avis juridique sur l'hébergement des photos et les conditions d'utilisation |
