# MEMORIA — Coûts Firebase (FIREBASE_COSTS)

> **Statut : estimation, en attente de validation.**
> Les tarifs ci-dessous sont des **ordres de grandeur** tirés des grilles publiques de Firebase et Google Cloud. Ils varient selon la région et changent dans le temps : à revérifier sur <https://firebase.google.com/pricing> avant de créer le projet de production. Conversion indicative : environ 3 DT pour 1 USD.
> Comme le dit le cahier §15 : ne pas promettre un coût nul.

## 1. Le fait à acter

**Cloud Storage for Firebase exige le forfait Blaze** (paiement à l'usage, moyen de paiement enregistré) pour tout nouveau bucket. Il n'est plus possible de lancer MEMORIA sur le forfait gratuit Spark, puisque le produit stocke des photos.

Le forfait Blaze conserve des quotas gratuits, mais **il n'a pas de plafond** : une alerte de budget prévient, elle n'arrête rien (message 6 de la conversation).

## 2. Postes de coût

| Service | Ce qui est facturé | Quota gratuit (indicatif) | Au-delà (indicatif) | Poids pour MEMORIA |
|---|---|---|---|---|
| Storage — stockage | Go stockés par mois | 5 Go, **dans trois régions américaines seulement** | ≈ 0,02 à 0,03 USD par Go et par mois | Moyen, cumulatif |
| Storage — trafic sortant | Go téléchargés | Variable selon la région | ≈ 0,12 USD par Go | **Le poste principal** |
| Storage — opérations | Envois et lectures de fichiers | Quelques dizaines de milliers par jour | Quelques centimes par 10 000 | Négligeable |
| Firestore — lectures | Documents lus, y compris par les règles | 50 000 par jour | ≈ 0,03 à 0,06 USD par 100 000 | Faible |
| Firestore — écritures | Documents écrits | 20 000 par jour | ≈ 0,09 à 0,18 USD par 100 000 | Négligeable |
| Firestore — stockage | Go de documents et d'index | 1 Go | ≈ 0,15 à 0,18 USD par Go et par mois | Négligeable |
| Functions | Appels et temps de calcul | 2 millions d'appels par mois | ≈ 0,40 USD par million | Négligeable avec 3 fonctions |
| Authentication | Utilisateurs actifs par mois | 50 000 | — | Nul au lancement |
| Hosting | Go stockés et transférés | 10 Go stockés, 360 Mo par jour | ≈ 0,15 USD par Go transféré | Faible : l'application pèse peu |
| App Check | Vérifications reCAPTCHA | 10 000 par mois | Forfait au-delà | Nul au lancement |
| Email | Service externe | Selon le service : quelques centaines par jour | Selon le service | Nul à faible |
| Nom de domaine | Achat annuel | — | Selon l'extension | Fixe |

## 3. Un mariage type

Hypothèses : 800 photos ; 150 invités actifs ; vignette 25 Ko, affichage 400 Ko, original 8 Mo ; chaque invité parcourt 150 vignettes, ouvre 40 photos et en télécharge 20 ; les mariés consultent et téléchargent tout.

### Stockage

| | Sans les originaux | Avec les originaux |
|---|---|---|
| Vignettes | 20 Mo | 20 Mo |
| Versions d'affichage | 320 Mo | 320 Mo |
| Originaux | — | 6,4 Go |
| **Total** | **0,34 Go** | **6,7 Go** |
| Coût mensuel | moins de 0,01 USD | ≈ 0,15 USD |

### Trafic sortant du premier mois

| | Sans les originaux | Avec les originaux |
|---|---|---|
| Navigation des invités (150 × ≈ 20 Mo) | 3 Go | 3 Go |
| Consultation des mariés | 0,3 Go | 0,3 Go |
| Téléchargements des invités (150 × 20 photos) | 1,2 Go | 24 Go |
| Téléchargement complet par les mariés | 0,3 Go | 6,4 Go |
| **Total** | **≈ 4,8 Go** | **≈ 34 Go** |
| Coût | ≈ 0,60 USD | ≈ 4 USD |

### Firestore et Functions

- Lectures : listes (≈ 30 000) et contrôles des règles Storage, 2 par fichier (≈ 65 000), soit ≈ 100 000 lectures, quelques centimes, souvent absorbés par le quota quotidien.
- Écritures : import, attribution, invités, soit ≈ 4 000, moins d'un centime.
- Functions : quelques centaines d'appels, gratuit.

### Bilan par mariage

| | Sans les originaux | Avec les originaux |
|---|---|---|
| Premier mois | ≈ 0,7 USD (≈ 2 DT) | ≈ 4,3 USD (≈ 13 DT) |
| Chaque mois de conservation | moins de 0,01 USD | ≈ 0,15 USD (≈ 0,5 DT) |

À rapprocher des prix envisagés (39, 79 et 149 DT par événement, cahier §19) : la marge est confortable, mais **les originaux multiplient le coût par six** et le stockage conservé s'additionne.

| Événements conservés | Sans les originaux | Avec les originaux |
|---|---|---|
| 10 | ≈ 3 Go, quelques centimes par mois | ≈ 67 Go, ≈ 1,5 USD par mois |
| 100 | ≈ 34 Go, ≈ 0,8 USD par mois | ≈ 670 Go, ≈ 15 USD par mois (≈ 45 DT) |

## 4. Leviers, par ordre d'efficacité

1. **Ne pas servir les originaux par défaut** (D-07). Option par événement ; à réserver aux offres qui le justifient.
2. **Cache du navigateur.** Fichiers immuables, en-tête de cache long, pas d'URL changeante (D-01) : revisiter une galerie ne coûte rien.
3. **Vignettes légères et pagination.** 25 Ko par vignette, 40 à 60 par page, chargement au défilement, grille virtualisée.
4. **Durée de conservation.** `retentionUntil` par événement ; purge des originaux d'abord, de l'événement ensuite.
5. **Aucune fonction dans le chemin de lecture.** Trois fonctions au MVP, appelées quelques centaines de fois par mariage.
6. **Lectures Firestore sobres.** Cache applicatif des requêtes, pas d'écoute temps réel sur les galeries, compteurs agrégés pour les tableaux de bord, aucune lecture de collection entière.
7. **Activation manuelle des photographes** (D-05) tant que les quotas par offre ne sont pas mesurés côté serveur.
8. **Compression avant envoi.** Les versions d'affichage sont produites dans le navigateur du photographe : aucun traitement d'image facturé.

## 5. Région

| | Europe (recommandé, D-06) | États-Unis (`us-central1`, `us-east1`, `us-west1`) |
|---|---|---|
| Latence depuis la Tunisie | Faible | Sensiblement plus élevée ; les images ne passent pas par un CDN |
| Quota gratuit de stockage | Non | 5 Go |
| Écart de coût au lancement | Quelques centimes par mois | — |

La région de Firestore et celle du bucket **ne peuvent plus être changées** après création.

## 6. Ce que l'application mesure

Les six indicateurs demandés par le cahier §15 :

| Indicateur | Source | Fiabilité au MVP |
|---|---|---|
| Stockage total par photographe | `workspaces.usage.storageBytes`, somme des tailles déclarées à l'import | Bonne ; déclarative |
| Bande passante consommée | `analytics.bytesServed`, taille des fichiers lus par l'application | Estimation haute : ne voit pas le cache. Total réel dans la console Google Cloud |
| Nombre de photos | `events.counters.photoCount`, `workspaces.usage.photoCount` | Bonne |
| Nombre de téléchargements | `analytics.downloads` | Bonne ; déclarative |
| Lectures Firestore | Console Firebase, globale au projet | Exacte, mais non ventilée par photographe |
| Coût estimé | Formule : compteurs × tarifs, affichée au super administrateur | Indicative ; phase pro |

En phase pro, les compteurs de stockage seront recalculés côté serveur pour servir de base aux quotas et à la facturation.

## 7. Surveillance et arrêt d'urgence

- **Budgets et alertes** dès la création du projet de production, à plusieurs seuils (montants à fixer : Q-07).
- **Tableau de bord d'usage** de Firebase consulté à chaque événement publié pendant le lancement.
- **Arrêt d'urgence**, procédure écrite et testée avant le lancement :
  1. Déployer des règles « tout interdit » sur Storage : le trafic s'arrête en une minute.
  2. Si nécessaire, désactiver la facturation du projet : tous les services payants s'arrêtent.
- Plus tard : arrêt automatique déclenché par l'alerte de budget.

## 8. Points à vérifier à la création du projet

- Tarifs exacts de la région retenue pour Storage, Firestore et Functions.
- Conservation des fichiers supprimés (« suppression réversible ») activée par défaut sur les nouveaux buckets : les fichiers supprimés restent facturés quelques jours.
- Stockage des images de déploiement des Functions : règle de nettoyage à poser.
- Tarifs et quota de reCAPTCHA Enterprise pour App Check.
