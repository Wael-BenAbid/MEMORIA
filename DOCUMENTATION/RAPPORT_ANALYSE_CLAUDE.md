# Rapport d'analyse technique — MEMORIA

| | |
|---|---|
| **Date** | 11 novembre 2026 |
| **Auteur** | Claude Code |
| **Statut** | **En attente de validation** — aucun code applicatif n'a été écrit |
| **Objet** | Analyse de la conversation de conception, incohérences, risques, décisions à valider |

Ce rapport est autonome : il peut être transmis tel quel pour relecture (la conversation source le prévoit).
Les détails de chaque sujet sont dans les documents `03` à `08` ; ce rapport renvoie vers eux.

---

## 1. Résumé

La conception issue de la conversation est solide : le produit est clair, les quatre décisions fondatrices sont cohérentes entre elles, les priorités (confidentialité, coûts, mobile) sont les bonnes, et l'ordre « un mariage qui fonctionne avant les étoiles » est le bon.

J'ai relevé **18 incohérences** et **16 manques**. La plupart sont des précisions. **Trois sont structurants** et doivent être tranchés avant d'écrire du code :

1. **L'identité des invités ne peut pas reposer sur l'UID au moment de l'attribution.** Le modèle indexe les membres, les attributions et les albums par UID Firebase. Or un UID n'existe qu'après la première connexion Google de l'invité, alors que le photographe attribue les photos *avant* d'inviter. Il faut une fiche invité créée par le photographe, rattachée à l'UID à la première connexion (→ D-03).
2. **Les deux mécanismes de lecture des photos proposés sont incompatibles entre eux et avec le modèle V2.** Le cahier (§13) veut des règles Storage qui consultent Firestore ; le message 6 veut une fonction qui délivre une URL signée de 5 minutes. Or les règles Storage ne peuvent consulter que **2 documents Firestore par requête** : avec la sous-collection `assignments` du message 6, il en faudrait 3. Et une fonction appelée pour chaque image contredit le principe « pas de Cloud Function pour chaque opération » et empêche la mise en cache (→ D-01, D-02).
3. **Le backlog place la sécurité en phase 7, après les étoiles**, alors que la conversation exige « sécurité avant le design » et « tester les autorisations avant de développer les animations » (→ D-09).

**Ma recommandation centrale** : lecture directe et authentifiée des fichiers, autorisée par les règles Storage à chaque requête, sans aucune URL partageable ; liste des invités attribués portée par le document de la photo ; Cloud Functions réservées à trois besoins réels (rattachement des invitations, envoi des emails, synchronisation de l'état d'un événement). Ce schéma est plus strict que l'URL signée (révocation immédiate, rien à faire fuiter), moins cher (aucun appel de fonction par image) et compatible avec le cache du navigateur.

**Le principal risque du projet n'est pas technique.** C'est la charge de travail du photographe : saisir les adresses Gmail de 250 invités qu'il ne connaît pas, puis reconnaître chacun sur 800 photos. La première démonstration (10 invités, 100 photos) doit mesurer ce temps (→ M-01, Q-03).

---

## 2. Ce qui a été lu

- La conversation partagée complète, 10 messages, extraite de la page et archivée dans [`00_CONVERSATION_SOURCE.md`](00_CONVERSATION_SOURCE.md), y compris le questionnaire interactif du message 2 et le suivi de tâches interactif du message 6 (7 phases), qui n'apparaissent pas dans un simple copier-coller.
- Le cahier des charges V1.0 ([`02_CAHIER_DES_CHARGES.md`](02_CAHIER_DES_CHARGES.md)) et le prompt de mission ([`../PROMPTS/PROMPT_INITIAL_CLAUDE.md`](../PROMPTS/PROMPT_INITIAL_CLAUDE.md)), comparés au texte collé dans la session : identiques.
- La capture d'écran transmise (schéma avec `workspaces`, `invite`, `users`, `teams`, `folders`) : c'est une image d'illustration générique insérée par ChatGPT à côté du modèle de données (source : un article de dev.to sur la conception de bases). **Elle ne décrit pas MEMORIA** et n'est pas prise comme référence.

## 3. État des lieux (points A, B, C de la première mission)

| Vérification | Résultat |
|---|---|
| Contenu du dépôt | Vide à l'ouverture de la session : aucun commit, aucun fichier |
| Node.js / npm | Node 22.23.4, npm 10.9.9 — compatibles avec Vite 8 et firebase-tools 15 |
| Java (émulateurs) | OpenJDK 21 — compatible |
| firebase-tools | Non installé ; sera une dépendance de développement du projet |
| Projet Firebase | Aucun dans le dépôt (`firebase.json`, `.firebaserc` absents). Existence d'un projet dans ta console : inconnue (→ Q-01) |
| Navigateur de test | Chromium et Playwright disponibles dans l'environnement |
| Réseau de l'environnement cloud | Limité. Le registre npm est accessible. L'émulateur Firestore se télécharge depuis `storage.googleapis.com`, **non autorisé pour l'instant** : il faudra l'autoriser au début de la phase 1 |

Versions stables constatées sur le registre npm le 11/11/2026 : voir [`03_ARCHITECTURE_TECHNIQUE.md` §3](03_ARCHITECTURE_TECHNIQUE.md#3-stack-et-versions).

## 4. Décisions actées par la conversation (non remises en cause)

| Sujet | Décision | Source |
|---|---|---|
| Clientèle | Toi et les autres photographes : SaaS multi-photographes dès la conception | Message 3 |
| Accès aux photos | Album personnel **et** galerie collective | Message 3 |
| Univers visuel | Ciel étoilé, constellations, étoiles interactives (pas le style « magazine ») | Message 3 |
| Plateforme | Site web responsive mobile + ordinateur (pas d'application native, pas d'installation) | Message 3 |
| Nom et slogan | MEMORIA — *Every moment deserves a place in the stars.* | Messages 2 et 4 |
| Architecture | Monolithe modulaire React + Firebase, une seule plateforme, isolation stricte par workspace | Messages 4 et 6 |
| Stack | React, TypeScript, Vite, Tailwind, React Router, Framer Motion, Lucide ; Firebase Auth, Firestore, Storage, Hosting, App Check, Functions si nécessaire ; Vitest, RTL, Playwright, Emulator Suite | Cahier §11, prompt §2 |
| Authentification | Google Sign-In ; l'email authentifié doit correspondre à un email invité | Cahier §7.2 |
| Attribution | Manuelle au MVP ; pas de reconnaissance faciale en V1 | Cahier §14.4, §20 |
| Images | Trois versions par photo, optimisation locale avant envoi, jamais d'original servi par défaut | Cahier §5.3, message 6 §4 |
| Confidentialité | Rien de public par défaut, pas d'URL permanente ni d'URL « cachée » comme protection | Prompt §5 |
| Environnements | Démo sur émulateurs, production dans un projet séparé | Message 6 §6 |
| Méthode | Phase par phase, tests et validation à chaque étape | Prompt §7, message 8 |
| Premier jalon | 1 photographe, 1 mariage, 10 invités, 100 photos, 10 albums, 1 galerie collective | Message 4 |
| Hors MVP | Applications natives, reconnaissance faciale, livres imprimés, paiement en ligne, marketplace, montage vidéo, réseau social public, messagerie instantanée | Cahier §20 |

---

## 5. Incohérences relevées

| # | Sujet | Constat | Résolution proposée |
|---|---|---|---|
| I-01 | Deux modèles de données | Le cahier §12 (« proposition initiale ») utilise `assignedUids` sur la photo et `photoIds` dans l'album ; le message 6 les supprime au profit d'une sous-collection `assignments/{uid}`. | Modèle consolidé V3 dans [`05_DATABASE_SCHEMA.md`](05_DATABASE_SCHEMA.md) (D-02, D-04). |
| I-02 | **Identité des invités** | Membres, attributions et albums sont indexés par UID, qui n'existe pas tant que l'invité ne s'est pas connecté. L'attribution précède l'invitation dans le parcours du cahier §3.1. | Fiche `guests/{guestId}` créée par le photographe ; rattachement à l'UID à la première connexion (D-03). |
| I-03 | **Lecture des fichiers** | Cahier §13 : règles Storage consultant Firestore. Message 6 : fonction + URL signée 5 min. Prompt §6 : pas de fonction par opération. Les trois ne tiennent pas ensemble. | Lecture directe sous règles Storage (D-01). |
| I-04 | **Place de la sécurité** | Backlog : « Sécurité et tests » en phase 7. Message 4 : « sécurité avant le design ». Prompt §9 : préparer les tests de sécurité dès la première mission. | Règles et tests négatifs écrits dans chaque phase (D-09). |
| I-05 | Trois feuilles de route | Message 2 (4 phases, IA avant commercialisation), cahier §18 (4 phases, pro avant IA), messages 6 et 8 (7 phases de développement). | Feuille de route unique dans [`04_ROADMAP_DEVELOPPEMENT.md`](04_ROADMAP_DEVELOPPEMENT.md). |
| I-06 | Périmètre du MVP | Favoris, personnalisation et collaborateurs sont « MVP » dans le prompt §4, mais en phase 2 ou 3 dans le cahier §18. QR codes : phase 2 dans le cahier. | Tableau de placement dans la feuille de route (D-11). |
| I-07 | `original.webp` | Le cahier §13 nomme l'original `original.webp`. Un original est le JPEG exporté par le photographe ; le convertir l'altère. | `original.<extension d'origine>`. |
| I-08 | « Trois versions » | Cahier §5.3 : miniature, galerie, haute résolution **et** original conservé (donc quatre fichiers ?). Message 6 : Original, Display, Thumbnail. | Trois fichiers ; nature de la version téléchargeable à choisir (D-07). |
| I-09 | Métaphore des étoiles | Message 2 : une étoile = un invité. Cahier §8.3 : une étoile = un espace (Mon album, Galerie…). | Cahier retenu ; précision à valider (Q-05). |
| I-10 | PWA | Tu as choisi « site web responsive », pas « installable ». Le message 2 §5 écrit pourtant « Application Web / PWA ». | Pas de PWA au MVP. |
| I-11 | Rôles dans `users` | `users.role` (V1) et `platformRole` (V2) sont dans un document que l'utilisateur peut modifier, ce qui contredit la « protection contre les modifications frauduleuses des rôles ». De plus un même compte peut être photographe ici et invité ailleurs. | Aucun rôle global : les rôles découlent des appartenances. Super administrateur par *custom claim* posé hors application. |
| I-12 | Messages | « Messages » est une étoile de navigation et les mariés peuvent « envoyer un message au photographe », mais la messagerie instantanée est hors périmètre. | À définir (Q-06). |
| I-13 | Jeton d'invitation | `tokenHash` suggère un lien secret par invité. Mais le QR code est commun et l'accès repose sur l'email vérifié. Le jeton n'est donc pas le vrai contrôle. | La preuve est l'email vérifié du compte Google. Pas de jeton au MVP. |
| I-14 | Navigation invité | « Mes téléchargements » et la « Recherche » de la galerie n'ont pas de définition (rechercher quoi ? il n'y a pas de texte sur les photos). | À définir ou reporter (Q-06). |
| I-15 | Albums | Une collection `albums` distincte rend possible le doublon que le cahier §17.4 interdit. | Un album par invité par construction : il est porté par la fiche invité (D-04). |
| I-16 | Statuts d'événement | « Désactiver temporairement l'accès » n'a pas de statut ; la différence Brouillon / En préparation et les effets d'Archivé / Expiré ne sont pas définis. | Machine d'états dans [`05_DATABASE_SCHEMA.md` §6](05_DATABASE_SCHEMA.md#6-machine-détats-dun-événement) (Q-04). |
| I-17 | Coût « limité au lancement » | Cloud Storage for Firebase exige désormais le forfait Blaze (paiement à l'usage) dès la création du bucket. Il n'existe plus de démarrage sans moyen de paiement. | Fait à acter ; alertes de budget dès le premier jour ([`07_FIREBASE_COSTS.md`](07_FIREBASE_COSTS.md)). |
| I-18 | Offres | Prix « par événement », mais l'offre Studio inclut « plusieurs événements ». L'offre Starter exclut l'album personnel, cœur du produit. | Question commerciale (Q-09), sans effet sur le MVP. |

## 6. Manques

| # | Manque | Proposition |
|---|---|---|
| M-01 | **Comment le photographe obtient-il les Gmail des invités, et qui les reconnaît sur les photos ?** | Mesurer sur la démonstration. Pistes : demande d'accès par QR code approuvée par le photographe ou les mariés ; délégation de l'attribution aux mariés ; groupes d'invités pour l'attribution en lot (Q-03). |
| M-02 | Les navigateurs intégrés de Messenger, Instagram et Facebook bloquent la connexion Google. | Détecter ces navigateurs et afficher « Ouvrir dans Chrome / Safari ». À tester sur de vrais téléphones. |
| M-03 | Invités sans compte Google. | Hors MVP ; connexion par lien email envisageable plus tard (Q-08). |
| M-04 | Ce qui est visible **avant** connexion sur la page d'un événement. | Aperçu public minimal, sans photo (D-08). |
| M-05 | Ce que voient les mariés. | À trancher (Q-02). |
| M-06 | Suppression fiable d'un événement (milliers de fichiers), durée de conservation, sauvegardes. | Archivage au MVP ; purge par script d'administration, puis fonction planifiée en phase pro. |
| M-07 | Quota de stockage vérifiable et abus à l'inscription ouverte (n'importe qui pourrait remplir un bucket facturé). | Activation manuelle des photographes au MVP (D-05) ; compteur de confiance en phase pro. |
| M-08 | Service d'email et nom de domaine. | À choisir avant la phase 5 (D-10, Q-01). |
| M-09 | Région Firebase, **irréversible**. | À choisir avant de créer le projet de production (D-06). |
| M-10 | Cadre juridique : loi tunisienne n° 2004-63 et INPDP (déclaration des traitements, hébergement hors de Tunisie), invités résidant en Europe (RGPD), mineurs, droit à l'image, partage des responsabilités photographe / MEMORIA. | Validation par un juriste avant toute commercialisation. Je ne suis pas juriste : ce point est signalé, pas traité. |
| M-11 | Normalisation des adresses Gmail (majuscules, points, `+alias`, `googlemail.com`). | Clé d'email normalisée, calculée des deux côtés ([`06_SECURITY.md` §6](06_SECURITY.md#6-rattachement-dune-invitation-à-un-compte)). |
| M-12 | Pipeline local : orientation EXIF, profils de couleur, encodage WebP selon le navigateur, mémoire. | Traitement en *Web Worker*, repli JPEG, concurrence limitée ([`03` §8](03_ARCHITECTURE_TECHNIQUE.md#8-pipeline-image)). |
| M-13 | Multilingue : l'arabe (écriture de droite à gauche) est-il visé ? | Clés de traduction dès le départ ; mise en page compatible RTL (Q-10). |
| M-14 | Intégration continue. | GitHub Actions : lint, types, tests unitaires, tests de règles, build. |
| M-15 | Journal des changements de permissions : où, écrit par qui. | `workspaces/{id}/auditLogs`, en ajout seul. |
| M-16 | Bibliothèques non précisées : requêtes et cache, formulaires, validation, traductions, EXIF, ZIP. | Liste courte à valider (D-12). |

---

## 7. Risques

Probabilité et impact : F = faible, M = moyen, É = élevé.

| # | Risque | P | I | Parade |
|---|---|---|---|---|
| **Produit** | | | | |
| R-01 | L'attribution manuelle prend des heures par mariage ; le photographe ne connaît pas les invités | É | É | Mesure sur la démo ; attribution en lot, groupes, délégation aux mariés ; « Find Me » en phase IA |
| R-02 | Les invités n'arrivent pas à se connecter depuis un lien ouvert dans Messenger ou Instagram | É | É | Détection et message d'aide ; QR code (ouvre le navigateur par défaut) ; test sur appareils réels |
| R-03 | Adresse saisie différente de celle du compte Google de l'invité | M | M | Normalisation ; écran « vous êtes connecté avec X, aucune invitation trouvée » ; correction par le photographe |
| **Sécurité** | | | | |
| R-04 | Erreur dans des règles complexes : fuite d'un album | M | É | Règles refusant tout par défaut ; tests négatifs automatisés à chaque phase ; revue avant chaque mise en production |
| R-05 | Limite de 2 documents Firestore par requête dans les règles Storage | — | É | Conception calée sur cette limite ; prouvée sur émulateur avant tout écran |
| R-06 | Usage par mégarde de `getDownloadURL` (URL permanente qui contourne les règles) | M | É | Interdit par convention et par règle de lint ; un seul composant d'affichage d'image privée |
| R-07 | Escalade de rôle par écriture directe dans Firestore | M | É | Aucun rôle dans un document modifiable par son titulaire ; tests dédiés |
| R-08 | Fuite de la liste des invités (noms, emails) | M | É | Illisible pour les invités ; chaque invité ne lit que sa fiche |
| R-09 | Facture provoquée par un invité autorisé qui télécharge en boucle | F | M | App Check, alertes de budget, tailles plafonnées, coupure d'urgence documentée |
| R-10 | Secret ou configuration de production dans Git | F | É | `.env` ignorés, secrets dans le gestionnaire de secrets, identifiant de projet `demo-` imposé aux tests |
| **Coûts** | | | | |
| R-11 | Le trafic sortant des originaux domine la facture | É | M | Version téléchargeable allégée par défaut, originaux en option (D-07) |
| R-12 | Le stockage s'accumule mariage après mariage | É | M | Durée de conservation par événement, purge, originaux optionnels |
| R-13 | Blaze n'a pas de plafond ; une alerte n'arrête rien | M | É | Alertes à plusieurs seuils ; coupure d'urgence manuelle documentée |
| **Performance** | | | | |
| R-14 | Import de 800 photos : plantage de l'onglet, envoi de plusieurs heures sur une connexion lente | M | M | Worker, 2 à 3 images en parallèle, envoi reprenable, import idempotent |
| R-15 | Galerie de 1 000 photos lente sur un téléphone d'entrée de gamme | M | M | Pagination, liste virtualisée, miniatures d'environ 25 Ko, chargement au défilement |
| R-16 | Ciel étoilé saccadé sur Android modeste | M | M | Un seul `canvas`, nombre d'étoiles adapté à l'appareil, respect de la préférence « réduire les animations » |
| **Technique** | | | | |
| R-17 | Versions majeures très récentes (TypeScript 7, Vite 8, React Router 8, Firebase 13) : API et compatibilités à vérifier | M | F | Lire la documentation de chaque version à l'initialisation ; versions figées par le fichier de verrouillage |
| R-18 | Hypothèses non encore prouvées (voir ci-dessous) | M | É | Phase 1 : prototype de validation avant tout écran |
| **Juridique** | | | | |
| R-19 | Photos de personnes identifiables hébergées hors de Tunisie, sans cadre validé | M | É | Avis juridique avant commercialisation (M-10) |

**Hypothèses à prouver en premier, sur émulateur et sur appareils réels (phase 1) :**

- H1 — Les règles Storage autorisent correctement les trois cas (personnel, collectif, privé) avec 2 lectures Firestore au plus.
- H2 — La requête « mes photos » (`array-contains` sur mon identifiant d'invité) passe les règles Firestore.
- H3 — Une galerie revisitée ne retélécharge pas les images (cache du navigateur avec lecture authentifiée).
- H4 — L'encodage WebP en *Web Worker* fonctionne sur Chrome, Edge, Firefox et Safari récents, avec repli JPEG.
- H5 — La connexion Google par redirection fonctionne sur mobile lorsque le domaine d'authentification est celui de l'application.

Si H1 ou H2 échoue, le repli est le schéma du message 6 (fonction + URL signée), au prix des inconvénients décrits en D-01.

---

## 8. Décisions à valider

Pour chacune : ce que dit la conversation, ce que je recommande, et pourquoi. Tu peux répondre « D-01 à D-12 : d'accord » ou amender point par point.

### D-01 — Lecture des fichiers photo

- **Conversation :** fonction backend qui vérifie les droits et délivre une URL signée de 5 minutes (message 6) ; règles Storage consultant Firestore (cahier §13).
- **Recommandation :** lecture directe par le SDK (`getBlob`) avec le jeton de l'utilisateur ; les règles Storage vérifient les droits **à chaque requête** en lisant 2 documents (l'appartenance, puis la photo). Aucune URL n'existe. Aucune fonction dans le chemin de lecture.
- **Pourquoi :** la révocation est immédiate et il n'y a rien à transférer à un tiers, alors qu'une URL signée reste valable 5 minutes pour quiconque la détient. App Check couvre aussi les octets de l'image. Zéro appel de fonction par image. Les images restent en cache alors qu'une URL signée change à chaque visite et force un nouveau téléchargement, c'est-à-dire le poste de coût principal.
- **Contrepartie :** l'image s'affiche une fois entièrement reçue (pas de rendu progressif) ; négligeable pour des miniatures de 25 Ko et des images d'affichage de 400 Ko. Le bucket doit recevoir une configuration CORS.
- **Détail :** [`06_SECURITY.md` §4 et §5](06_SECURITY.md#4-chaîne-dautorisation-dune-photo).

### D-02 — Où stocker « qui voit cette photo »

- **Conversation :** V1 : tableau `assignedUids` sur la photo. V2 : sous-collection `photos/{id}/assignments/{uid}`, pour éviter les grands tableaux.
- **Recommandation :** tableau `assignedGuestIds` sur le document de la photo, plafonné à 200 entrées.
- **Pourquoi :** l'argument du message 6 (limite de 1 Mio) vise le tableau `photoIds` d'un album, qui peut compter des milliers d'entrées ; il reste valable et `photoIds` est bien supprimé. La liste des personnes d'une photo, elle, compte quelques dizaines d'identifiants. Avec ce tableau, l'album d'un invité est **une seule requête**, et la règle Storage tient en 2 lectures. Avec la sous-collection, il faut 3 lectures (impossible) et l'album coûte deux lectures par photo.
- **Au-delà de 200 personnes :** la photo est par nature collective.

### D-03 — Identité de l'invité et rattachement au compte Google

- **Conversation :** tout est indexé par UID ; invitation avec `tokenHash`.
- **Recommandation :** le photographe crée une fiche `events/{E}/guests/{guestId}` (nom, email). Les photos sont attribuées à ce `guestId`. À la première connexion, une fonction `claimInvitations` compare l'**email vérifié du jeton Google** aux fiches en attente et crée l'appartenance `events/{E}/members/{uid}`. Les contrôles d'accès passent ensuite toujours par l'UID.
- **Pourquoi :** c'est la seule façon d'attribuer avant la connexion. L'album appartient à la personne, pas au compte : si un invité change d'adresse, le photographe corrige la fiche et l'album suit. L'exigence « identifié par son UID, jamais par un email envoyé par le navigateur » est respectée : l'email vient du jeton signé par Google, lu côté serveur.
- **Alternative :** le même rattachement écrit uniquement en règles Firestore, sans fonction. Possible, mais les règles deviennent nettement plus complexes ; je ne le recommande pas.

### D-04 — L'album est une vue, pas une collection

- **Recommandation :** pas de collection `albums` au MVP. L'album d'un invité est le résultat de la requête « photos publiées qui me sont attribuées » ; son titre et son compteur sont sur la fiche invité.
- **Pourquoi :** un seul album par invité par construction (critère « aucune duplication accidentelle d'album »), moins d'écritures, aucune désynchronisation possible entre l'album et les droits. Les albums thématiques ou familiaux pourront être ajoutés plus tard comme groupes.

### D-05 — Inscription des photographes

- **Conversation :** inscription publique.
- **Recommandation :** inscription publique, mais l'espace est créé « en attente » et activé par le super administrateur (toi) au MVP.
- **Pourquoi :** le bucket est facturé à l'usage et un quota fiable demande un compteur côté serveur (phase pro). Tant qu'il n'existe pas, l'activation manuelle évite qu'un inconnu envoie des gigaoctets à tes frais.

### D-06 — Région Firebase (irréversible)

- **Recommandation :** Europe (par exemple `europe-west1`) pour Firestore, Storage et Functions en production.
- **Pourquoi :** la latence depuis la Tunisie est nettement meilleure qu'avec les États-Unis, et les images ne passent pas par un CDN.
- **Contrepartie :** le quota gratuit de Storage (5 Go) ne vaut que pour trois régions américaines. À ce volume, la différence se compte en centimes par mois.

### D-07 — Version téléchargeable

- **Recommandation :** trois fichiers par photo : `thumbnail` (≈ 400 px), `display` (≈ 2 048 px), `original` (le JPEG exporté, inchangé, sans ses coordonnées GPS). L'envoi des originaux est une option par événement : désactivée, le téléchargement fournit la version `display`.
- **Pourquoi :** l'original coûte environ 20 fois plus cher à stocker et à servir que la version d'affichage, et son envoi peut durer des heures. Le photographe choisit selon l'offre vendue.
- **Option ultérieure :** une quatrième version « HD » (≈ 4 000 px, 2 à 3 Mo) pour les invités, l'original restant réservé aux mariés.

### D-08 — Page d'un événement avant connexion

- **Recommandation :** un document public minimal par événement (titre, date, thème, signature du photographe ; chaque champ activable), **sans aucune photo** ; l'image de couverture n'apparaît qu'après connexion. L'adresse de l'événement porte un suffixe aléatoire (`sarah-ahmed-x7k2`).
- **Pourquoi :** le QR code est imprimé et circule ; sans cela, n'importe qui pourrait lire les noms et la date d'un mariage, ou deviner l'adresse d'un autre.

### D-09 — La sécurité dans chaque phase

- **Recommandation :** les règles démarrent en « tout interdit » ; chaque phase ajoute ses règles et ses tests négatifs, et ne se termine que lorsqu'ils passent. La phase 7 devient l'audit final (tests de bout en bout, performance, coûts, App Check en mode strict).

### D-10 — Invitations

- **Recommandation :** d'abord le lien et le QR code de l'événement, que le photographe ou les mariés partagent (WhatsApp, impression). L'envoi d'emails arrive ensuite dans la même phase, une fois choisis le service et le nom de domaine.
- **Pourquoi :** l'accès ne dépend pas de l'email reçu mais du compte Google. Le parcours complet peut donc être validé sans infrastructure d'email, et le lien partagé correspond aux usages réels.

### D-11 — Périmètre du MVP sur les points flottants

| Fonction | Placement proposé |
|---|---|
| Collaborateurs | Modèle et règles prêts et testés dès le MVP ; écrans de gestion en phase pro |
| Favoris | MVP (simple, attendu dans les critères du prompt) |
| QR code de l'événement | MVP (une image générée localement ; nécessaire à D-10) |
| Personnalisation | MVP : titre, date, message, couverture. Thèmes et univers : phase « Expérience » |
| Statistiques | MVP : compteurs simples (photos, invités, stockage). Statistiques détaillées : phase pro |
| Notifications, rapports, abonnements, quotas | Phase pro |

### D-12 — Bibliothèques ajoutées à la stack

À ajouter à la liste de la conversation, toutes légères et courantes : **TanStack Query** (cache et pagination des lectures, pour réduire les lectures Firestore), **Zod** (validation des formulaires, du CSV et des documents), **React Hook Form**, **i18next** (français au lancement, multilingue préparé), **exifr** (date de prise de vue, orientation), **fflate** (ZIP d'une sélection, dans le navigateur), **qrcode** (génération locale du QR code). Framer Motion est aujourd'hui distribué sous le nom `motion` : c'est la même bibliothèque.

---

## 9. Questions ouvertes

| # | Question | Pourquoi elle compte |
|---|---|---|
| Q-01 | As-tu déjà un projet Firebase et un nom de domaine pour MEMORIA ? | Conditionne la création du projet, le domaine d'authentification et l'envoi d'emails |
| Q-02 | Les mariés voient-ils l'album personnel de chaque invité, ou seulement la galerie collective et leur propre album ? | Règle d'accès du rôle « mariés » |
| Q-03 | Qui saisit les invités et attribue les photos : toi seul, ou aussi les mariés ? Veux-tu qu'un invité non listé puisse demander l'accès depuis le QR code ? | Risque produit principal (R-01) |
| Q-04 | Un événement archivé reste-t-il visible par les invités ? Quelle durée de conservation par défaut ? | Machine d'états, coûts de stockage |
| Q-05 | Dans la constellation, les étoiles sont les espaces (cahier). Veux-tu en plus une étoile « à moi » par invité ? Les étoiles des autres invités resteraient anonymes. | Design de la phase « Expérience », confidentialité de la liste des invités |
| Q-06 | Que recouvrent « Messages », « Mes téléchargements » et la « Recherche » ? Proposition : message d'accueil des mariés en lecture seule ; pas d'historique des téléchargements au MVP ; filtres par catégorie et par date à la place de la recherche. | Périmètre |
| Q-07 | Quel budget mensuel maximal pour Firebase pendant le lancement ? | Seuils d'alerte |
| Q-08 | Faut-il prévoir les invités sans compte Google ? | Parcours de connexion |
| Q-09 | L'offre Starter sans album personnel et le prix « par événement » de l'offre Studio sont-ils voulus ? | Verrouillage des fonctions par offre (phase pro) |
| Q-10 | L'arabe est-il une langue visée après le français ? | Mise en page de droite à gauche à anticiper |

---

## 10. Après validation

1. J'intègre tes réponses dans les documents `03` à `08` et dans `CLAUDE.md` (les décisions passent de « proposées » à « validées »).
2. Je démarre la **phase 1** de la [feuille de route](04_ROADMAP_DEVELOPPEMENT.md) : initialisation du projet, émulateurs, règles « tout interdit », harnais de tests de sécurité, et prototype de validation des hypothèses H1 à H5.
3. Je te présente le résultat de la phase 1 avant de commencer la phase 2.

De ton côté, la phase 1 ne demande rien : elle tourne entièrement sur les émulateurs. Le projet Firebase de production, la région, le domaine et le service d'email ne sont nécessaires qu'à partir du premier déploiement.
