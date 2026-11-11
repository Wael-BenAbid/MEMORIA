# MEMORIA — Stratégie de tests (TESTING)

> **Statut : plan, en attente de validation.** Aucun test n'existe encore : ils seront écrits à partir de la phase 1, **dans chaque phase** (décision D-09 du [rapport d'analyse](RAPPORT_ANALYSE_CLAUDE.md)).

## 1. Niveaux de test

| Niveau | Outil | Ce qu'il prouve | Où |
|---|---|---|---|
| Unitaire | Vitest | Fonctions pures : normalisation d'email, calcul d'empreinte, validation CSV, machine d'états, formules de coût | `tests/unit/` |
| Composants | Vitest + React Testing Library | Formulaires, sélection multiple, visionneuse, états d'erreur, accessibilité de base | à côté des composants |
| **Sécurité (règles)** | `@firebase/rules-unit-testing` + émulateurs Firestore et Storage | Chaque autorisation et chaque refus des règles | `tests/security/` |
| Intégration | Vitest + émulateurs | Fonctions (`claimInvitations`, `syncEventAccess`), import complet d'une photo, attribution | `tests/integration/` |
| Bout en bout | Playwright + émulateurs | Parcours photographe et parcours invité dans un vrai navigateur, sur écran mobile et ordinateur | `tests/e2e/` |
| Performance | Playwright, mesures du navigateur | Galeries de 100 et 1 000 photos, import par lots, réseau mobile lent | `tests/e2e/perf/` |

Tous les tests tournent sur les émulateurs, avec l'identifiant de projet `demo-memoria`. Aucun test ne peut toucher la production.

## 2. Tests de sécurité

Chaque ligne est un test automatisé. « Refus » signifie que la requête directe au service échoue, pas seulement que l'interface masque un bouton. Pour les photos, chaque scénario est vérifié **deux fois** : sur le document Firestore et sur les fichiers Storage.

### 2.1. Les sept scénarios obligatoires du cahier §17.2

| # | Scénario | Attendu |
|---|---|---|
| S-01 | L'invité A lit une photo personnelle attribuée seulement à B (document, vignette, affichage, original) | Refus |
| S-02 | Un invité lit un événement dont il n'est pas membre (document, photos, fichiers) | Refus |
| S-03 | Un photographe lit ou modifie un événement, une photo ou un invité d'un autre workspace | Refus |
| S-04 | Un utilisateur non connecté lit ou télécharge une photo | Refus |
| S-05 | Un invité révoqué relit une photo qu'il voyait auparavant, par le chemin exact du fichier | Refus |
| S-06 | Un invité lit une photo `private`, même si son `guestId` figure dans `assignedGuestIds` | Refus |
| S-07 | Un collaborateur sans `manageGuests` modifie les invités ; sans `uploadPhotos` importe ; supprime un événement | Refus |

### 2.2. Scénarios complémentaires

| # | Scénario | Attendu |
|---|---|---|
| **Accès légitimes** | | |
| S-10 | L'invité A lit une photo personnelle publiée qui lui est attribuée | Autorisé |
| S-11 | Un invité autorisé lit une photo collective publiée | Autorisé |
| S-12 | Le propriétaire et un collaborateur lisent toutes les photos de leur événement | Autorisé |
| S-13 | Un invité liste « son album » avec la requête prévue | Autorisé, et uniquement ses photos |
| **Albums et galerie** | | |
| S-14 | Un invité liste toutes les photos de l'événement sans filtre | Refus |
| S-15 | Un invité liste l'album d'un autre en changeant le `guestId` de la requête | Refus |
| S-16 | Un invité sans accès à la galerie lit une photo collective qui ne lui est pas attribuée | Refus |
| S-17 | Un invité lit une photo non publiée qui lui est attribuée | Refus |
| S-18 | Un invité sans droit de téléchargement lit le fichier `original` | Refus |
| S-19 | Après retrait d'une attribution, l'invité relit la photo | Refus |
| **État de l'événement** | | |
| S-20 | Un invité lit les photos d'un événement en brouillon ou en préparation | Refus |
| S-21 | Un invité lit les photos d'un événement suspendu, puis archivé | Refus |
| S-22 | Rattachement demandé pour un événement non publié | Refus |
| **Identité et rôles** | | |
| S-23 | Un invité crée ou modifie sa propre appartenance `members/{uid}` (rôle, `guestId`, statut) | Refus |
| S-24 | Un utilisateur crée une appartenance dans un événement où il n'est pas invité | Refus |
| S-25 | Un utilisateur s'ajoute comme membre d'un workspace, ou se donne le rôle propriétaire | Refus |
| S-26 | Un collaborateur modifie ses propres permissions | Refus |
| S-27 | Un photographe modifie `plan` ou `status` de son workspace | Refus |
| S-28 | Un utilisateur écrit dans `users/{uid}` d'un autre, ou y ajoute un champ de rôle | Refus |
| S-29 | Rattachement avec un compte dont l'email diffère de celui de la fiche | Refus |
| S-30 | Rattachement d'une fiche déjà rattachée à un autre compte | Refus |
| S-31 | Rattachement d'une invitation expirée ou révoquée | Refus |
| S-32 | Rattachement avec la même adresse écrite autrement (majuscules, points, `+alias`) | Autorisé |
| **Isolation** | | |
| S-33 | Un photographe envoie un fichier dans le chemin Storage d'un autre workspace | Refus |
| S-34 | Modification de `workspaceId` sur un événement ou une photo | Refus |
| S-35 | Un invité lit la liste des invités, ou la fiche d'un autre invité | Refus |
| S-36 | Un invité lit le journal d'audit ou les compteurs du workspace | Refus |
| **Fichiers** | | |
| S-37 | Liste du contenu d'un dossier Storage, par n'importe qui | Refus |
| S-38 | Envoi d'un fichier sans document de photo en `uploading` | Refus |
| S-39 | Remplacement d'un fichier d'une photo `ready` | Refus |
| S-40 | Envoi d'un fichier de type non autorisé ou dépassant la taille maximale | Refus |
| S-41 | Import par un workspace `pending` ou `suspended` | Refus |
| **Divers** | | |
| S-42 | Un invité augmente un compteur d'un montant hors bornes, ou le diminue | Refus |
| S-43 | Lecture de `eventPublic/{slug}` sans connexion | Autorisé, champs publics seulement |
| S-44 | Le super administrateur lit un événement, une photo ou un invité | Refus |
| S-45 | Les règles Storage ne dépassent jamais 2 lectures Firestore, quel que soit le cas | Vérifié |

Deux autres contrôles, hors émulateur :

- Une règle de lint interdit `getDownloadURL` dans tout le code.
- Une revue manuelle des règles précède chaque mise en production.

## 3. Tests fonctionnels (cahier §17.1)

| Domaine | Cas principaux |
|---|---|
| Authentification | Connexion, déconnexion, redirection selon les appartenances, message « aucune invitation » |
| Création d'événement | Champs obligatoires, statuts, publication, pause, archivage |
| Importation | Types acceptés et refusés, doublon ignoré, annulation, reprise après interruption, erreur réseau |
| Attribution | Une photo à plusieurs invités, plusieurs photos en une fois, retrait, plafond de 200 |
| Invitations | Ajout unitaire, import CSV (lignes invalides, doublons), statuts, révocation |
| Permissions | Chaque permission de collaborateur, écran par écran |
| Favoris | Ajout, retrait, pas de doublon, liste |
| Téléchargements | Photo seule, sélection en ZIP, refus sans droit |

## 4. Jeu de données de démonstration

Le script `npm run seed` recrée dans les émulateurs le premier jalon de la conversation :

- 1 photographe et son workspace actif ;
- 1 mariage publié (« Mariage de Sarah & Ahmed ») ;
- 10 invités, dont les mariés, plus 1 invité révoqué et 1 compte non invité ;
- 100 photos de test : personnelles, collectives et privées, dont certaines attribuées à plusieurs invités ;
- 1 second workspace avec 1 événement, pour les tests d'isolation ;
- 1 collaborateur aux permissions limitées.

**Test de réussite du jalon** (message 4) : chaque invité se connecte, retrouve ses photos et ne peut pas consulter celles des autres sans autorisation. Ce test est rejoué de bout en bout par Playwright pour les 10 invités.

## 5. Tests de performance (cahier §17.3)

| Cas | Mesure | Cible provisoire |
|---|---|---|
| Galerie de 100 photos | Temps avant la première vignette, fluidité du défilement | Première vignette en moins de 2 s en 4G |
| Galerie de 1 000 photos | Mémoire, fluidité, nombre de requêtes | Défilement fluide ; pas plus de 60 vignettes chargées à la fois |
| Nouvelle visite d'une galerie | Octets retéléchargés | Aucun pour les images déjà vues |
| Import de 100, puis 800 photos | Durée, mémoire, reprise | Onglet stable ; reprise sans doublon |
| Connexions simultanées | 10 invités en parallèle sur le même événement | Aucune erreur |
| Réseau mobile lent | Parcours invité complet en 3G simulée | Utilisable ; ciel étoilé allégé |
| Appareil modeste | Processeur bridé | Animations réduites automatiquement |

## 6. Critères de qualité (cahier §17.4)

- Aucun accès non autorisé : toute la section 2 passe.
- Aucun fichier manquant après un import réussi : une photo `ready` a tous ses fichiers.
- Aucun album en double : un album par invité, par construction.
- Affichage correct sur smartphone, tablette et ordinateur.
- Chaque erreur prévisible a un message clair et une issue.
- Les règles Firestore et Storage sont testées automatiquement.

## 7. Définition de « terminé » pour une phase

1. Le périmètre prévu est développé, sans plus.
2. Les règles ajoutées ont leurs tests, positifs et négatifs.
3. Vérification des types, lint, tests et build passent.
4. Le parcours est vérifié à la main dans le navigateur, sur écran mobile.
5. Les documents touchés et `CLAUDE.md` sont à jour.
6. Un compte rendu liste ce qui a été fait, vérifié, et ce qui reste.
