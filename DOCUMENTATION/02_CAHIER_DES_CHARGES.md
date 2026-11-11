<!--
  Copie fidèle du cahier des charges V1.0 rédigé dans la conversation source (message 4).
  NE PAS MODIFIER ce texte : il fait foi. Les écarts, précisions et corrections proposés sont
  consignés dans RAPPORT_ANALYSE_CLAUDE.md et, une fois validés, dans les documents 03 à 08.
-->

> **Source :** conversation de conception MEMORIA, message 4 — reproduit à l'identique ([archive](00_CONVERSATION_SOURCE.md)).
> **Lecture conseillée avec :** [`RAPPORT_ANALYSE_CLAUDE.md`](RAPPORT_ANALYSE_CLAUDE.md), qui liste les points de ce cahier à préciser ou à amender (en particulier le §12, remplacé par le modèle de [`05_DATABASE_SCHEMA.md`](05_DATABASE_SCHEMA.md) après validation).

# MEMORIA — Cahier des charges fonctionnel et technique

**Version :** 1.0  
**Date :** 11 novembre 2026  
**Type :** Plateforme SaaS multi-photographes  
**Slogan :** Every moment deserves a place in the stars.

---

# 1. Présentation générale

## 1.1. Description du projet

MEMORIA est une plateforme web destinée aux photographes et vidéastes professionnels, permettant de créer, gérer et partager des albums photographiques privés et personnalisés pour des événements.

Les événements concernés sont notamment :

- Mariages.
- Fiançailles.
- Anniversaires.
- Baptêmes.
- Événements professionnels.
- Soirées et cérémonies.
- Séances photographiques privées.

La particularité de MEMORIA est son expérience immersive inspirée du ciel étoilé.

Chaque événement possède son propre univers visuel. Chaque invité peut accéder à ses photographies personnelles et, selon les autorisations, à la galerie collective.

La plateforme doit également être commercialisable auprès d'autres photographes sous forme de SaaS.

## 1.2. Objectifs

1. Faciliter la livraison des photographies après un événement.
2. Automatiser la création des albums.
3. Donner à chaque invité un espace personnel.
4. Proposer une galerie collective contrôlée par le photographe.
5. Offrir une expérience visuelle différenciante.
6. Permettre aux photographes de gérer plusieurs événements.
7. Préparer une architecture commerciale multi-photographes.
8. Maîtriser les coûts d'hébergement, de stockage et de transfert des photos.

## 1.3. Principes fondamentaux

- Une seule plateforme pour tous les photographes.
- Isolation stricte des données entre photographes.
- Confidentialité par défaut.
- Albums personnels et galerie collective.
- Gestion des accès par événement.
- Design responsive.
- Interface en français au lancement, avec préparation du multilingue.
- Aucune obligation de créer un compte pour chaque événement côté photographe : les invités utilisent leur propre compte Google.

# 2. Utilisateurs et rôles

## 2.1. Les types d'utilisateurs

### A. Super administrateur de la plateforme

Responsable de MEMORIA.

Fonctionnalités :
- Gérer les photographes inscrits.
- Consulter les statistiques globales.
- Suspendre ou réactiver un compte.
- Gérer les offres commerciales.
- Surveiller les volumes de stockage.
- Consulter les journaux techniques et de sécurité.
- Gérer les signalements.

Il ne doit pas avoir un accès automatique et illimité aux albums privés des clients. Tout accès exceptionnel doit être justifié et journalisé.

### B. Photographe propriétaire

Utilisateur qui crée son espace professionnel.

Fonctionnalités :
- Gérer son profil et son identité visuelle.
- Créer plusieurs événements.
- Importer et organiser des photos.
- Créer des albums individuels.
- Gérer les invités.
- Envoyer des invitations.
- Générer des QR codes.
- Gérer les autorisations.
- Consulter les statistiques.
- Télécharger les rapports.

### C. Collaborateur du photographe

Option à prévoir dans l'architecture.

Permissions possibles :
- Gestion des événements.
- Importation de photos.
- Organisation des albums.
- Gestion des invités.

Le propriétaire doit pouvoir attribuer les permissions.

### D. Mariés / organisateurs

Accès facultatif à un espace dédié.

Fonctionnalités :
- Consulter la galerie complète autorisée.
- Consulter les albums.
- Sélectionner des favoris.
- Partager des souvenirs.
- Télécharger les photos.
- Envoyer un message au photographe.

### E. Invité

Fonctionnalités :
- Se connecter avec Google.
- Retrouver les événements auxquels il est invité.
- Accéder à son album personnel.
- Accéder à la galerie collective si autorisé.
- Consulter les photos en plein écran.
- Télécharger les photos autorisées.
- Ajouter des favoris.
- Partager les photos selon les permissions.

# 3. Architecture fonctionnelle

## 3.1. Parcours général

### Parcours photographe

1. Inscription sur MEMORIA.
2. Connexion à son espace professionnel.
3. Création d'un événement.
4. Ajout des informations de l'événement.
5. Importation des photos retouchées.
6. Organisation par catégories.
7. Création des albums invités.
8. Attribution des photos.
9. Configuration des accès.
10. Génération de la page immersive.
11. Invitation des participants.
12. Publication de l'événement.

### Parcours invité

1. Réception d'une invitation.
2. Ouverture du lien ou scan du QR code.
3. Connexion avec Google.
4. Vérification de l'adresse email invitée.
5. Affichage de l'événement.
6. Découverte de son univers étoilé.
7. Accès à son album personnel.
8. Consultation de la galerie collective autorisée.
9. Téléchargement des photos.

## 3.2. Structure de navigation

### Espace public

- Accueil.
- Présentation de MEMORIA.
- Fonctionnement.
- Démonstration.
- Tarifs.
- Inscription photographe.
- Connexion.

### Espace photographe

- Dashboard.
- Mes événements.
- Créer un événement.
- Galerie.
- Albums.
- Invités.
- Invitations.
- Statistiques.
- Paramètres.
- Profil professionnel.

### Espace invité

- Mes événements.
- Mes albums.
- Galerie collective.
- Mes favoris.
- Mes téléchargements.
- Mon profil.

# 4. Gestion des événements

## 4.1. Création d'un événement

Le photographe doit pouvoir créer un événement avec :

- Nom de l'événement.
- Type d'événement.
- Nom des personnes concernées.
- Date.
- Lieu.
- Description.
- Image de couverture.
- Photographe responsable.
- Couleurs et personnalisation.
- Statut de publication.

Types de statuts :

- Brouillon.
- En préparation.
- Publié.
- Archivé.
- Expiré.

## 4.2. Personnalisation

Chaque événement doit pouvoir disposer de :

- Son propre titre.
- Son propre univers étoilé.
- Une couverture personnalisée.
- Une musique facultative, uniquement si les droits sont disponibles.
- Un message d'accueil.
- Une date affichée.
- Un logo ou une signature du photographe.
- Un thème de couleurs.

Exemple :

Événement : Mariage de Sarah & Ahmed  
Date : 15 juin 2027  
Univers : Golden Stars  
Message : Welcome to our universe.

## 4.3. Publication

Le photographe doit pouvoir :

- Prévisualiser l'événement.
- Tester les permissions.
- Publier l'événement.
- Désactiver temporairement l'accès.
- Archiver l'événement.

Un événement non publié ne doit jamais être accessible aux invités.

# 5. Gestion des photographies

## 5.1. Importation

Le système doit accepter :

- JPG / JPEG.
- PNG.
- WebP.

Les formats RAW et les vidéos ne sont pas prioritaires pour le MVP.

Fonctionnalités :
- Importation multiple.
- Glisser-déposer.
- Barre de progression.
- Annulation d'un import.
- Détection des doublons.
- Aperçu avant publication.
- Gestion des erreurs.
- Reprise des importations interrompues lorsque possible.

## 5.2. Organisation

Chaque photographie doit pouvoir avoir :

- Un identifiant unique.
- Un événement associé.
- Un nom de fichier.
- Une miniature.
- Un chemin de stockage.
- Une catégorie.
- Une date de prise de vue.
- Une date d'importation.
- Un statut de publication.
- Une liste d'invités autorisés.
- Un indicateur de visibilité collective.

Catégories proposées :

- Préparatifs.
- Cérémonie.
- Couple.
- Famille.
- Amis.
- Réception.
- Gâteau.
- Soirée.
- Danse.
- Moments spontanés.

## 5.3. Traitement des images

Le système doit générer automatiquement :

1. Une miniature pour les aperçus.
2. Une version optimisée pour la galerie.
3. Une version haute résolution pour le téléchargement.

Le fichier original doit être conservé si l'offre et les paramètres de conservation le permettent.

Les images doivent être chargées progressivement avec pagination ou infinite scroll.

Les métadonnées EXIF sensibles, notamment les coordonnées GPS, doivent être supprimées des versions publiques.

# 6. Albums personnels et galerie collective

C'est le cœur fonctionnel du projet.

## 6.1. Album personnel

Chaque invité possède un album individuel.

Exemple :

- Événement : Mariage de Sarah & Ahmed.
- Invité : Wael.
- Album : Souvenirs de Wael.
- Photos attribuées : 35.

Fonctionnalités :

- Afficher les photos attribuées.
- Visualiser une photographie en plein écran.
- Naviguer entre les photos.
- Télécharger une photo.
- Télécharger une sélection.
- Ajouter aux favoris.
- Afficher la date et la catégorie.

L'album doit être généré automatiquement dès que les photos sont attribuées à l'invité.

## 6.2. Galerie collective

La galerie collective contient les photos sélectionnées par le photographe pour tous les participants autorisés.

Fonctionnalités :

- Affichage en grille.
- Filtres par catégorie.
- Filtres par date.
- Recherche.
- Visionneuse plein écran.
- Favoris.
- Téléchargement selon les droits.

## 6.3. Gestion des permissions

Chaque photographie doit avoir une règle de visibilité :

- `personal` : uniquement les invités explicitement autorisés.
- `collective` : tous les membres autorisés de l'événement.
- `private` : uniquement le photographe et les collaborateurs autorisés.

Une photo personnelle ne doit jamais devenir accessible simplement parce qu'un utilisateur connaît son URL.

## 6.4. Attribution des photos

Le photographe doit disposer d'un outil simple :

1. Sélectionner une photo.
2. Cliquer sur « Attribuer ».
3. Choisir un ou plusieurs invités.
4. Confirmer.
5. Mettre à jour automatiquement leurs albums.

Il doit aussi pouvoir sélectionner plusieurs photos et les attribuer en une seule opération.

La modification ou la suppression d'une attribution doit être prise en compte immédiatement dans les autorisations d'accès.

# 7. Gestion des invités

## 7.1. Ajout des invités

Méthodes :
- Ajout individuel.
- Importation CSV.
- Ajout en masse.
- Génération d'invitations à partir d'une liste.

Informations :

- Nom.
- Prénom.
- Email Google.
- Événement associé.
- Niveau d'accès.
- Statut d'invitation.
- Date d'inscription.

## 7.2. Authentification

Utiliser Firebase Authentication avec Google Sign-In.

Le système doit vérifier que l'email authentifié correspond à un email invité.

Un utilisateur connecté avec un autre Gmail ne doit pas pouvoir accéder à l'album.

Un invité peut participer à plusieurs événements avec le même compte.

## 7.3. Invitations

Statuts :

- En attente.
- Envoyée.
- Acceptée.
- Expirée.
- Révoquée.

L'invitation doit être personnelle, limitée à l'événement concerné et révocable.

## 7.4. QR codes

Chaque événement possède un QR code permettant d'ouvrir sa page d'accueil.

Le QR code ne donne pas directement accès aux photographies.

L'utilisateur doit toujours passer par l'authentification et la vérification de son invitation.

# 8. Expérience visuelle : univers étoilé

## 8.1. Identité visuelle

Direction artistique :

- Bleu nuit profond.
- Noir spatial.
- Or doux.
- Blanc lunaire.
- Lumières discrètes.
- Dégradés subtils.

Éviter un design trop chargé ou des animations qui empêchent la consultation des photos.

## 8.2. Page d'accueil d'un événement

Éléments :

- Ciel étoilé animé.
- Nom de l'événement.
- Date.
- Message de bienvenue.
- Constellation centrale.
- Bouton « Entrer dans mon univers ».

## 8.3. Navigation par constellation

Les étoiles représentent les différents espaces :

- Mon album.
- Galerie collective.
- Mes favoris.
- Timeline.
- Messages.

Au clic sur une étoile, une animation légère ouvre l'espace correspondant.

La navigation classique doit rester disponible pour les utilisateurs qui ne souhaitent pas utiliser l'interface animée.

## 8.4. Animations

Prévoir :

- Étoiles scintillantes.
- Apparition progressive.
- Mouvement lent des étoiles.
- Transitions entre les pages.
- Effets lumineux au survol.
- Animation d'ouverture d'album.

Les animations doivent être légères, accessibles et désactivables via les préférences de mouvement du système.

## 8.5. Galerie photographique

La galerie doit proposer :

- Grille responsive.
- Affichage de haute qualité.
- Visionneuse plein écran.
- Navigation précédente / suivante.
- Zoom.
- Téléchargement.
- Favoris.

L'expérience doit être particulièrement soignée sur smartphone.

# 9. Dashboard photographe

## 9.1. Tableau de bord

Informations principales :

- Nombre d'événements.
- Nombre de photographies importées.
- Nombre d'invités.
- Albums créés.
- Invitations acceptées.
- Nombre de consultations.
- Nombre de téléchargements.
- Stockage utilisé.

## 9.2. Gestion des événements

Chaque événement doit présenter :

- Nom.
- Date.
- Couverture.
- Nombre de photos.
- Nombre d'invités.
- Statut.
- Dernière activité.

## 9.3. Statistiques

Statistiques par événement :

- Photos les plus consultées.
- Albums les plus consultés.
- Nombre de téléchargements.
- Nombre d'invités actifs.
- Évolution des visites.
- Volume de stockage.

Les statistiques doivent être calculées à partir d'événements agrégés, et non par lecture répétée de toutes les photographies.

# 10. Notifications

Notifications prévues :

- Invitation envoyée.
- Album disponible.
- Nouvelles photos publiées.
- Événement archivé.
- Invitation révoquée.

Canaux :

- Email.
- Notifications dans l'application.

Les emails doivent être envoyés par un service adapté avec limitation de débit, gestion des erreurs et possibilité de désinscription des notifications non essentielles.

# 11. Architecture technique

## 11.1. Stack recommandée

### Frontend

- React.
- Vite.
- TypeScript.
- Tailwind CSS.
- React Router.
- Framer Motion pour les animations.
- Lucide Icons.

### Backend et infrastructure

- Firebase Authentication.
- Cloud Firestore.
- Firebase Storage.
- Firebase Hosting.
- Firebase App Check.
- Cloud Functions uniquement lorsque nécessaire.

### Outils complémentaires

- Git et GitHub.
- Vitest.
- React Testing Library.
- Playwright.
- Firebase Emulator Suite.

L'architecture doit rester simple et limiter les services payants pendant la phase de lancement.

## 11.2. Architecture générale

```text
MEMORIA
│
├── Public Website
│   ├── Landing Page
│   ├── Pricing
│   └── Photographer Registration
│
├── Photographer Dashboard
│   ├── Events
│   ├── Photos
│   ├── Albums
│   ├── Guests
│   └── Analytics
│
├── Guest Experience
│   ├── My Events
│   ├── My Album
│   ├── Collective Gallery
│   └── Favorites
│
└── Firebase
    ├── Authentication
    ├── Firestore
    ├── Storage
    ├── Hosting
    └── Optional Functions
```

# 12. Modèle de données Firestore

Proposition initiale :

```text
users/
  {uid}
    displayName
    email
    photoURL
    role
    createdAt
    status

workspaces/
  {workspaceId}
    ownerUid
    name
    branding
    plan
    storageUsed
    createdAt

workspaces/{workspaceId}/members/
  {uid}
    role
    permissions
    status

events/
  {eventId}
    workspaceId
    title
    type
    date
    location
    description
    coverPath
    theme
    status
    publishedAt
    createdAt

events/{eventId}/members/
  {uid}
    email
    name
    role
    status

events/{eventId}/invitations/
  {invitationId}
    email
    status
    invitedAt
    expiresAt

events/{eventId}/photos/
  {photoId}
    originalPath
    displayPath
    thumbnailPath
    category
    visibility
    assignedUids
    published
    createdAt

events/{eventId}/albums/
  {albumId}
    guestUid
    title
    photoIds
    createdAt

events/{eventId}/favorites/
  {favoriteId}
    guestUid
    photoId
    createdAt

events/{eventId}/analytics/
  {periodId}
    views
    downloads
    activeGuests
```

Les collections et index devront être adaptés aux requêtes réellement utilisées.

Les albums ne doivent pas être la seule source d'autorisation : les règles d'accès doivent également contrôler les métadonnées de chaque photo.

# 13. Architecture Firebase Storage

Structure proposée :

```text
storage/
  workspaces/
    {workspaceId}/
      branding/
      events/
        {eventId}/
          cover/
          photos/
            {photoId}/
              original.webp
              display.webp
              thumbnail.webp
```

Principes :

- Aucun fichier public par défaut.
- Lecture authentifiée et autorisée.
- Écriture réservée aux photographes et collaborateurs autorisés.
- Validation des types MIME.
- Limitation des tailles.
- Suppression des fichiers inutilisés.
- Gestion des versions et des suppressions.

Les règles Storage doivent vérifier l'appartenance à l'événement et la visibilité de la photo en consultant ses métadonnées Firestore.

Il faut tester séparément l'accès personnel, collectif et privé.

# 14. Sécurité et confidentialité

La sécurité est une priorité absolue.

## 14.1. Authentification

- Google Sign-In.
- Session Firebase sécurisée.
- Vérification des invitations.
- Déconnexion.
- Révocation des accès.

## 14.2. Isolation des données

Un photographe ne peut pas consulter les événements d'un autre workspace.

Un invité ne peut pas accéder aux photos d'un événement auquel il n'appartient pas.

Un invité ne peut pas consulter les albums personnels d'un autre invité.

Les permissions doivent être vérifiées côté serveur avec Firestore et Storage Security Rules.

## 14.3. Protection des photos

- Pas d'accès public anonyme.
- Pas de confiance dans les identifiants masqués.
- Contrôle des téléchargements.
- Protection contre l'énumération des fichiers.
- Journalisation des changements de permissions.
- Protection des liens de téléchargement.

Un filigrane facultatif peut être proposé pour les aperçus, sans altérer les originaux.

## 14.4. Confidentialité

Prévoir :

- Consentement et information des personnes concernées.
- Politique de confidentialité.
- Conditions générales d'utilisation.
- Durée de conservation configurable.
- Suppression d'un événement.
- Suppression d'un compte.
- Export des données.
- Procédure de signalement.

La reconnaissance faciale ne fait pas partie de la première version.

# 15. Performance et maîtrise des coûts

La plateforme doit être conçue pour limiter les coûts de Firebase.

Objectifs :

- Chargement progressif.
- Miniatures optimisées.
- Compression avant importation lorsque pertinent.
- Mise en cache adaptée.
- Pagination des galeries.
- Pas de lecture illimitée de collections.
- Pas de téléchargement automatique des originaux.
- Limites de stockage par offre.
- Suivi des volumes transférés.

Les tableaux de bord utilisent des compteurs agrégés.

Le système doit permettre de suivre :

1. Stockage total par photographe.
2. Bande passante consommée.
3. Nombre de photos.
4. Nombre de téléchargements.
5. Nombre de lectures Firestore.
6. Coût estimé des services.

Ne pas promettre un coût Firebase nul : les quotas et tarifs dépendent des offres et de l'utilisation réelle.

# 16. Interface et responsive design

Trois formats prioritaires :

- Smartphone : expérience invité principale.
- Tablette : consultation et gestion.
- Ordinateur : administration photographe.

Le design doit respecter :

- Accessibilité clavier.
- Contrastes suffisants.
- Textes lisibles.
- Images adaptatives.
- Chargement rapide.
- Animations limitées sur les appareils faibles.
- Compatibilité Chrome, Safari, Firefox et Edge récents.

# 17. Tests et qualité

## 17.1. Tests unitaires

- Authentification.
- Création d'événement.
- Importation.
- Attribution des photos.
- Gestion des invitations.
- Permissions.
- Favoris.
- Téléchargements.

## 17.2. Tests de sécurité

Scénarios obligatoires :

1. Un invité A tente d'ouvrir l'album de B.
2. Un invité tente d'accéder à un événement inconnu.
3. Un photographe tente de consulter un autre workspace.
4. Un utilisateur non connecté tente de télécharger une photo.
5. Un utilisateur utilise un ancien lien après révocation de son invitation.
6. Un invité tente d'accéder à une photo privée.
7. Un collaborateur tente une action non autorisée.

Tous ces scénarios doivent être bloqués.

## 17.3. Tests de performance

- Galerie avec 100 photos.
- Galerie avec 1 000 photos.
- Importation de lots de photos.
- Connexions simultanées.
- Chargement sur réseau mobile lent.

## 17.4. Critères de qualité

- Aucun accès non autorisé.
- Aucun fichier manquant après importation réussie.
- Aucune duplication accidentelle d'album.
- Affichage responsive.
- Gestion correcte des erreurs.
- Tests automatisés des règles Firestore et Storage.

# 18. Roadmap de développement

## Phase 1 — MVP fonctionnel

Priorité maximale.

- Authentification.
- Inscription photographe.
- Dashboard.
- Création d'événements.
- Importation de photos.
- Gestion des invités.
- Invitations.
- Attribution manuelle.
- Albums personnels.
- Galerie collective.
- Téléchargements sécurisés.

**Résultat attendu :** un photographe peut livrer un vrai mariage à ses invités.

## Phase 2 — Expérience MEMORIA

- Univers étoilé.
- Constellations interactives.
- Page événement immersive.
- Animations.
- Favoris.
- Timeline.
- QR codes.
- Personnalisation graphique.

**Résultat attendu :** une expérience différente des galeries photo classiques.

## Phase 3 — Gestion professionnelle

- Collaborateurs.
- Statistiques avancées.
- Notifications.
- Gestion des quotas.
- Branding professionnel.
- Rapports.
- Gestion des abonnements.

**Résultat attendu :** plateforme commercialisable auprès d'autres photographes.

## Phase 4 — Intelligence artificielle

- Reconnaissance de visages avec consentement.
- Recherche de personnes.
- Classement automatique.
- Suggestions de photos.
- Génération de livres photo.

Cette phase dépendra de la validation juridique, des coûts et de la demande réelle.

# 19. Modèle économique

Les offres proposées sont des hypothèses à tester sur le marché tunisien.

| Offre | Prix indicatif | Fonctionnalités |
|---|---:|---|
| Starter | 39 DT / événement | Galerie collective |
| Premium | 79 DT / événement | Albums individuels, QR, personnalisation |
| Studio | 149 DT / événement | Plusieurs événements, branding et statistiques |

Prévoir également :

- Un essai gratuit.
- Des limites de stockage.
- Des options de stockage supplémentaires.
- Des offres annuelles pour les studios.

La facturation devra être intégrée dans une phase ultérieure.

# 20. Hors périmètre du MVP

Pour ne pas ralentir le lancement, les fonctionnalités suivantes sont reportées :

- Application Android native.
- Application iOS native.
- Reconnaissance faciale.
- Génération automatique de livres imprimés.
- Paiement en ligne.
- Marketplace de photographes.
- Montage vidéo.
- Réseau social public.
- Messagerie instantanée.

# 21. Critères de validation finale du MVP

Le MVP est considéré comme terminé lorsque :

- Un photographe peut créer son compte.
- Il peut créer un événement.
- Il peut importer plusieurs centaines de photos.
- Il peut ajouter des invités.
- Il peut attribuer des photos à chacun.
- Chaque invité peut se connecter avec son Gmail.
- Chaque invité peut consulter uniquement ses photos autorisées.
- Les invités peuvent consulter la galerie collective si elle est activée.
- Les téléchargements fonctionnent.
- Les accès peuvent être révoqués.
- Les règles de sécurité passent les tests.
- L'application fonctionne sur smartphone et ordinateur.
- Les coûts de stockage et de transfert sont mesurables.

---

# 22. Vision finale

MEMORIA ne doit pas être perçu comme un simple service de stockage d'images.

C'est une plateforme qui transforme les souvenirs d'un événement en une expérience personnelle, privée et émotionnelle.

Le photographe ne livre plus seulement des fichiers photographiques : il livre un univers de souvenirs.

**MEMORIA — Every moment deserves a place in the stars.**

---

# Annexe — Recommandations accompagnant le cahier des charges

> Texte de ChatGPT placé juste après le document dans le message 4 (numéroté « 23 » à sa suite), reproduit à l'identique.

## 23. Mes recommandations avant le développement

Je recommande de prendre cinq décisions techniques dès le départ :

1. **Multi-tenant dès la première version** : chaque photographe possède son workspace, sans mélange de données.
2. **Sécurité avant le design** : tester les autorisations des albums personnels avant de développer les animations.
3. **Pas de reconnaissance faciale au MVP** : l'attribution manuelle est suffisante pour valider le produit.
4. **Optimisation des photos dès l'importation** : ne jamais servir systématiquement les originaux.
5. **Mobile-first** : la majorité des invités consulteront leurs souvenirs sur leur téléphone.

### Ordre de réalisation recommandé

**Premier objectif** (Priorité 1)

# Un mariage entièrement fonctionnel

Construire un événement de démonstration avec :

- 1 photographe.
- 1 mariage.
- 10 invités.
- 100 photographies.
- 10 albums personnels.
- 1 galerie collective.

---

**Test de réussite :** chaque invité se connecte avec son Gmail, retrouve ses photos et ne peut pas consulter celles des autres sans autorisation.

Cette première démonstration permettra de valider le produit avant d'investir dans les animations complexes et les fonctionnalités SaaS.

**Prochaine étape :** je te propose de transformer ce cahier des charges en architecture technique détaillée, avec l'arborescence complète React, les collections Firebase, les règles Firestore/Storage et un backlog de développement découpé en tâches pour ton agent Claude Code ou Codex.
