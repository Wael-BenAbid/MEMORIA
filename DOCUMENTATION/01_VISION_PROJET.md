# MEMORIA — Vision du projet

> *Every moment deserves a place in the stars.*

> Synthèse fidèle de la [conversation source](00_CONVERSATION_SOURCE.md). Ce document ne contient aucune décision nouvelle : les propositions et corrections sont dans le [rapport d'analyse](RAPPORT_ANALYSE_CLAUDE.md).

## 1. L'origine

Wael est photographe de mariages et d'événements, et développeur. Son idée de départ (message 1) : après un mariage, une fois les photos retouchées, les déposer sur un site qui crée un « book » **dans les étoiles du ciel** ; chaque personne se connecte avec son Gmail et y trouve son book.

## 2. Le concept

MEMORIA ne doit pas être perçu comme un service de stockage d'images. C'est une plateforme qui transforme les souvenirs d'un événement en une expérience personnelle, privée et émotionnelle. **Le photographe ne livre plus seulement des fichiers : il livre un univers de souvenirs** (cahier §22).

L'intuition formulée dans la conversation : le produit différenciant n'est pas une galerie photo, c'est le sentiment, pour chaque invité, de recevoir un cadeau numérique personnalisé après un événement important de sa vie.

| Dans la métaphore | Dans le produit |
|---|---|
| Un ciel étoilé | L'univers visuel d'un événement |
| Une constellation | La navigation de l'événement |
| Une étoile | Un espace : mon album, la galerie collective, mes favoris, la timeline, les messages |

Le message 2 proposait d'abord « un invité = une étoile » ; le cahier des charges a retenu « une étoile = un espace ». Ce point reste à confirmer (question Q-05 du rapport).

## 3. Les quatre décisions fondatrices

Prises par Wael au message 3, en réponse au questionnaire du message 2.

| Question | Options proposées | **Choix** |
|---|---|---|
| À qui proposer la plateforme ? | Mon activité seule · Moi et les autres photographes (SaaS) · Plateforme ouverte à tous | **Moi et les autres photographes : SaaS** |
| Comment les invités retrouvent-ils leurs photos ? | Album personnel · Galerie complète · Les deux | **Les deux : album personnel et galerie collective** |
| Quel univers visuel ? | Ciel étoilé · Magazine haut de gamme · Les deux selon l'événement | **Ciel étoilé, constellations, étoiles interactives** |
| Quelle plateforme ? | Site web responsive · Site installable · Web + application native | **Site web responsive, mobile et ordinateur** |

Conséquence tirée par la conversation : **une seule plateforme multi-photographes**, avec une séparation stricte des données de chaque photographe et de chaque événement, plutôt qu'un site par client.

## 4. Pour qui

| Public | Ce qu'il attend |
|---|---|
| Photographes indépendants, studios, vidéastes | Livrer un événement sans créer un site par client ; gagner du temps ; une image de marque |
| Mariés et organisateurs | Voir toute la galerie autorisée, choisir des favoris, partager, télécharger |
| Invités | Retrouver **leurs** photos simplement, sur leur téléphone, avec leur compte Google |

Événements visés : mariages, fiançailles, anniversaires, baptêmes, événements professionnels, soirées et cérémonies, séances privées.

## 5. L'expérience de l'invité

1. **Entrer dans son univers** : une page d'accueil immersive, le nom de l'événement, la date, un ciel étoilé animé.
2. **Découvrir son espace** : la constellation ouvre son album, la galerie collective, ses favoris.
3. **Parcourir son album** : une visionneuse plein écran, une navigation fluide.
4. **Garder ses souvenirs** : téléchargement d'une photo ou d'une sélection, selon les droits accordés.

L'accès passe toujours par la connexion Google et la vérification de l'invitation. Un QR code ouvre la page de l'événement, jamais les photos directement.

## 6. L'outil du photographe

Créer un événement, importer les photos retouchées, les organiser par catégories, saisir ou importer les invités, attribuer les photos, régler les accès, publier, inviter. Un tableau de bord suit les événements, les photos, les invités, les consultations, les téléchargements et le stockage.

L'idée directrice : tout part d'un seul événement. Le photographe importe ses photos, sélectionne celles d'un invité, et l'album de cet invité existe.

## 7. L'identité visuelle

Bleu nuit profond, noir spatial, or doux, blanc lunaire, lumières discrètes, dégradés subtils. Ambiance élégante, émotionnelle, premium.

Deux garde-fous posés par la conversation : éviter un design chargé ou des animations qui gênent la consultation des photos ; le design étoilé « doit être exceptionnel, mais ne doit jamais compromettre les performances ou l'accessibilité ».

## 8. Les principes

Les cinq recommandations du message 4, à prendre « dès le départ » :

1. **Multi-tenant dès la première version** : chaque photographe a son workspace, sans mélange de données.
2. **Sécurité avant le design** : tester les autorisations des albums personnels avant de développer les animations.
3. **Pas de reconnaissance faciale au MVP** : l'attribution manuelle suffit à valider le produit.
4. **Optimisation des photos dès l'importation** : ne jamais servir systématiquement les originaux.
5. **Mobile-first** : la majorité des invités consulteront leurs souvenirs sur leur téléphone.

Et les principes du cahier §1.3 : confidentialité par défaut, gestion des accès par événement, interface en français au lancement avec le multilingue préparé, pas de compte à créer par événement puisque les invités utilisent leur compte Google.

Priorité affichée du prompt de mission : **la confidentialité des photographies et la fiabilité des albums personnels**.

## 9. Le premier objectif

« Un mariage entièrement fonctionnel » : 1 photographe, 1 mariage, 10 invités, 100 photographies, 10 albums personnels, 1 galerie collective.

Test de réussite : chaque invité se connecte avec son Gmail, retrouve ses photos et ne peut pas consulter celles des autres sans autorisation.

Ensuite seulement vient l'univers étoilé, « qui fera la différence ».

## 10. Le modèle économique

Hypothèses à tester sur le marché tunisien (cahier §19) :

| Offre | Prix indicatif | Contenu |
|---|---|---|
| Starter | 39 DT par événement | Galerie collective |
| Premium | 79 DT par événement | Albums individuels, QR code, personnalisation |
| Studio | 149 DT par événement | Plusieurs événements, branding, statistiques |

À prévoir : un essai gratuit, des limites de stockage, du stockage supplémentaire en option, des offres annuelles pour les studios. La facturation en ligne viendra dans une phase ultérieure.

## 11. Ce qui n'est pas dans la première version

Hors périmètre du MVP (cahier §20) : applications Android et iOS natives, reconnaissance faciale, livres imprimés, paiement en ligne, marketplace de photographes, montage vidéo, réseau social public, messagerie instantanée.

Idées évoquées au message 2, gardées en réserve :

| Idée | Description |
|---|---|
| « Find Me » | L'invité envoie un selfie, le système retrouve ses photos. Données biométriques : consentement explicite et validation juridique nécessaires |
| Photo du jour | Une photo mise en avant chaque jour |
| Message des mariés | Une vidéo ou un texte de remerciement |
| Livre de souvenirs | Les invités laissent un message |
| Diaporama « cinéma » | Les photos défilent avec des transitions |
| Album souvenir en PDF | Un album personnalisé à télécharger |
| Album familial | Un album partagé par la famille proche |
| Style « magazine » | L'autre univers visuel proposé, non retenu |

Autres noms envisagés : STELLARA, EVERORA, ASTRIA. Le nom retenu est MEMORIA.

## 12. Chronologie de la conception

| Message | Auteur | Contenu |
|---|---|---|
| 1 | Wael | L'idée : un book dans les étoiles, accessible avec son Gmail |
| 2 | ChatGPT | Le concept, les fonctionnalités, une première architecture, un modèle économique, quatre phases, des noms, et un questionnaire de quatre questions |
| 3 | Wael | Les quatre choix ; demande du cahier des charges |
| 4 | ChatGPT | Le cahier des charges V1.0 (22 sections), cinq recommandations, le premier objectif |
| 5 | Wael | Accord pour l'architecture technique |
| 6 | ChatGPT | L'architecture : arborescence, modèle de données révisé, sécurité, images, routes, environnements, backlog en 7 phases, prompt de mission |
| 7 | Wael | Comment transmettre la conversation à Claude |
| 8 | ChatGPT | Organisation des documents, prompt d'accompagnement, fichier `CLAUDE.md`, ordre des sept étapes |
| 9 | Wael | Un lien partagé suffit-il ? |
| 10 | ChatGPT | Oui ; le message qui a lancé l'analyse |
