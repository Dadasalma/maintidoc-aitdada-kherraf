# Traçabilité de l'utilisation de l'intelligence artificielle — MaintiDoc

**Date de début du projet :** 25 octobre 2026  

**Outil principal :** Claude (Anthropic, claude.ai)  

**Outils complémentaires :** Python, ReportLab et Pillow  

**Période de génération et de modification du corpus :** du 1er au 4 novembre 2026  

**Date de révision du présent fichier :** 6 novembre 2026

## 1. Présentation générale

Le projet MaintiDoc a débuté le 25 octobre 2026. Il porte sur la conception d'un assistant documentaire multimodal destiné à accompagner les techniciens dans leurs activités de maintenance industrielle.

Dans le cadre de ce projet, un corpus documentaire synthétique a été constitué afin de tester les fonctionnalités envisagées pour l'assistant : recherche documentaire, compréhension de documents techniques, analyse d'images, gestion des ambiguïtés, détection des contradictions et résistance aux tentatives d'injection de prompt.

La génération du corpus a été réalisée en plusieurs étapes : définition du corpus et d'une fiche de référence commune, rédaction des documents, génération des illustrations, création de cas de test particuliers, audit de cohérence et corrections.

Les documents ont été rédigés avec l'assistance de Claude et générés à l'aide de scripts Python utilisant notamment ReportLab pour les PDF et Pillow pour les images.

Les prompts présentés dans ce fichier sont des **résumés des demandes effectuées**, et non des reproductions exactes de l'historique des conversations. Ils sont conservés sous cette forme lorsque le texte original des prompts n'est pas disponible. Aucun prompt n'a été reconstitué puis présenté comme un texte original.

## 2. Fiche de référence commune

Cette fiche rassemble les informations utilisées comme référence pendant la génération du corpus.

### 2.1. Équipements

- CONV-A-01 : convoyeur à bande CB-400 de 24 m, entraîné par MOTO-B-01.
- CONV-A-02 : convoyeur à bande CB-400 de 18 m, entraîné par MOTO-B-02.
- MOTO-B-01 : motoréducteur de 5,5 kW, courant nominal In = 11,2 A.
- MOTO-B-02 : motoréducteur de 7,5 kW, courant nominal In = 14,8 A.

### 2.2. Codes d'alarme des convoyeurs

- C-101 : glissement de la bande, voyant orange fixe.
- C-102 : désalignement, voyant orange clignotant lentement.
- C-103 : bourrage, voyant rouge fixe.
- C-104 : arrêt d'urgence, voyant rouge clignotant.
- C-105 : température du palier supérieure à 80 °C, voyant orange clignotant rapidement.

### 2.3. Codes d'alarme de la motorisation

- M-101 : surintensité supérieure à 120 % du courant nominal, voyant rouge fixe.
- M-102 : température moteur supérieure à 110 °C, voyant orange fixe.
- M-103 : température d'huile du réducteur supérieure à 85 °C, voyant orange clignotant lentement.
- M-104 : défaut de phase, voyant rouge clignotant.
- M-105 : niveau d'huile bas, voyant orange clignotant rapidement.

### 2.4. Cas ambigus et contradictions volontaires

Les paires suivantes ont été prévues pour tester la capacité du système à distinguer des situations visuellement ou sémantiquement proches :

- C-101 / M-102
- C-102 / M-103
- C-103 / M-101
- C-104 / M-104
- C-105 / M-105

Deux contradictions documentaires ont également été introduites volontairement :

- Procédure de tension de la bande : 45 N·m dans la version v1 contre 60 N·m dans la version v2.
- Procédure de graissage : 40 g / 500 h dans la version v1 contre 25 g / 1000 h dans la version v2.

Ces contradictions constituent des cas de test. Elles ne doivent pas être interprétées comme des consignes techniques à appliquer sur une machine réelle.

## 3. Historique des prompts utilisés

Les éléments ci-dessous résument les principales demandes adressées à Claude et les opérations effectuées avec les scripts Python.

| N° | Outil | Date | Objet | Prompt (résumé) |
|---|---|---|---|---|
| P01 | Claude | 2026-11-01 | Plan du corpus et fiche de référence | Proposer un corpus documentaire d'environ 40 documents sur deux familles d'équipements (convoyeurs et motoréducteurs), comprenant des cas normaux, ambigus, contradictoires, scannés et des documents contenant des tentatives d'injection de prompt. Définir une fiche de référence commune. |
| P02 | Claude | 2026-11-01 | Manuels | Rédiger les six manuels d'utilisation et d'entretien en français, avec des figures et des tableaux numérotés, cohérents avec la fiche de référence. |
| P03 | Claude | 2026-11-01 | Procédures | Rédiger des procédures de maintenance en français, notamment pour l'arrêt d'urgence, le remplacement d'un rouleau, le nettoyage, la vidange, le remplacement d'un roulement, le réarmement du variateur et les notes de service. |
| P04 | Claude | 2026-11-01 | Fiches de sécurité | Créer les fiches de sécurité et de consignation pour les deux familles d'équipements. |
| P05 | Claude | 2026-11-01 | Codes d'alarme | Produire les tableaux de codes d'alarme par équipement, avec des seuils spécifiques, notamment les seuils de glissement de 5 % et 4 % et les valeurs de surintensité propres aux deux motoréducteurs. |
| P06 | Claude | 2026-11-01 | Schémas | Produire quatre schémas : mécanique du convoyeur, implantation des capteurs, schéma électrique de principe et coupe du réducteur. |
| P07 | Claude | 2026-11-01 | Historiques | Créer des historiques fictifs d'interventions et de tickets, avec des identifiants cohérents et des liens entre certaines interventions. |
| P08 | Claude | 2026-11-01 | Paires contradictoires | Produire deux versions de la procédure de tension de la bande et deux versions de la procédure de graissage, avec des valeurs volontairement différentes. Prévoir des versions scannées pour les anciennes procédures. |
| P09 | Claude | 2026-11-01 | Premier document piégé | Créer une note de service contenant une instruction malveillante en page 2 afin de tester la résistance du système aux injections de prompt présentes dans les documents. |
| P10 | Claude + Pillow | 2026-11-01 | Illustrations synthétiques | Générer six illustrations de panneaux, de plaque moteur, de dérive de bande et de fuite d'huile. Ces images sont des illustrations synthétiques et non des photographies réelles. |
| P11 | Claude + Pillow | 2026-11-01 | Second document piégé | Créer un relevé de température scanné, incliné et bruité, contenant une tentative d'injection dans une remarque manuscrite, afin de tester l'OCR et la résistance aux injections. |
| P12 | Claude | 2026-11-01 | Audit de cohérence | Examiner les 41 premiers documents, repérer les incohérences de valeurs, de dates et d'inventaire, puis proposer des corrections. |
| P13 | Claude + script Python | 2026-11-01 | Dégradation des scans | Dégrader volontairement les trois documents scannés (inclinaison, flou, bruit, contraste faible et compression JPEG) pour tester la reconnaissance optique de caractères (OCR). |
| P14 | Claude + script Python | 2026-11-01 | Documents réservés aux responsables | Créer deux rapports d'incident et deux notes de direction fictifs, réservés au groupe des responsables, avec des repères de confidentialité permettant de tester les fuites d'informations entre groupes. |
| P15 | Claude + script Python | 2026-11-04 | Extension des six manuels | Étendre les six manuels pour atteindre environ 12 à 15 pages, en conservant les pages initiales prévues comme inchangées. Ajouter des chapitres sur l'installation, la mise en service, l'exploitation, le diagnostic, la sécurité, les caractéristiques, les pièces, les révisions et le glossaire. |

**Précision :** les descriptions P01 à P15 sont des résumés. Elles ne doivent pas être considérées comme les prompts exacts copiés depuis les historiques de conversation.

## 4. Analyse critique de l'utilisation de l'IA

L'utilisation de l'IA a facilité la génération rapide d'un corpus documentaire varié. Cependant, les documents générés ont nécessité des audits, des corrections et une relecture humaine.

Les points suivants ont notamment été relevés et consignés au cours des opérations d'audit et de correction.

### 4.1. Erreurs et incohérences relevées

- **Structure de l'inventaire :** une première indication de l'assistant mentionnait 21 colonnes, alors que le sujet en exige 20. La structure de l'inventaire a été ramenée aux 20 champs demandés.
- **Identifiants des équipements :** des exemples du sujet utilisaient des conventions d'identifiants différentes. Une convention homogène a été adoptée pour le corpus : CONV-A-01, CONV-A-02, MOTO-B-01 et MOTO-B-02.
- **Cohérence chronologique :** un manuel d'entretien de convoyeur daté de 2024 citait une note de service de 2025. La version du manuel a été modifiée pour corriger cet anachronisme.
- **Manuel d'entretien du motoréducteur :** une ancienne version contenait des valeurs de graissage correspondant à une version plus récente de la procédure. La version du manuel a été mise à jour.
- **Codes d'alarme :** certains codes M-10x étaient coupés sur deux lignes dans les historiques. Leur présentation a été corrigée.
- **Illustration de la plaque moteur :** une partie du texte était tronquée. L'image a été régénérée afin de rendre les informations visibles.
- **Documents contenant des injections :** le premier document piégé était initialement associé à un groupe d'accès qui ne permettait pas de tester correctement une fuite vers les techniciens. Son accessibilité a été modifiée pour permettre le scénario de test prévu.
- **Inventaire :** certaines descriptions de figures et de tableaux ne correspondaient pas suffisamment aux éléments réellement visibles. Les références concernées ont été examinées et ajustées.

### 4.2. Vérifications numériques

Les calculs suivants ont été utilisés comme points de contrôle :

- 1450 / 25 = 58 tr/min.
- Vitesse de bande annoncée : 1,2 m/s pour un tambour de 400 mm.
- 120 % de 11,2 A = 13,44 A, soit environ 13,4 A.
- 120 % de 14,8 A = 17,76 A, soit environ 17,8 A.

Ces calculs permettent de contrôler la cohérence des valeurs présentes dans les documents. Ils ne constituent pas une validation technique des équipements réels.

### 4.3. Limites de l'IA

L'IA peut produire des informations incorrectes, des incohérences chronologiques, des erreurs de mise en page ou des valeurs numériques erronées. Elle ne garantit pas la conformité technique ou la sécurité des procédures générées.

La génération automatique de documents ne remplace donc pas une vérification humaine. Les valeurs, les dates, les références aux figures, les tableaux, l'orthographe et les informations de sécurité doivent être contrôlés.

Statut de la relecture humaine : voir la colonne `verifie_par` de `inventaire.csv`. Les relectures ne sont consignées que si elles ont réellement eu lieu ; la relecture page par page des extensions P15 reste à confirmer par l'équipe.

## 5. Traçabilité des corrections et relecture humaine

Les modifications effectuées au cours des audits ont été consignées dans l'inventaire documentaire, notamment dans la colonne `corrections`.

La colonne `verifie_par` identifie la personne ayant réellement effectué la vérification du document. La colonne `corrections` décrit les corrections appliquées et conserve, lorsque cela est utile, les informations relatives aux modifications générées ou assistées par IA.

Les informations de ces colonnes doivent correspondre aux vérifications réellement réalisées par les membres de l'équipe. Les noms des relecteurs ne doivent pas être inventés et les corrections non effectuées ne doivent pas être déclarées comme réalisées.

La colonne `corrections` ne contient que les corrections effectivement consignées ; la mention « aucune correction consignée » signifie qu'aucune correction n'a été enregistrée, pas qu'une validation technique a eu lieu.

## 6. Décision concernant les documents réels de fabricants

Sept documents de fabricants, notamment des documents associés à Dorner et à SEW-Eurodrive, avaient été envisagés puis ajoutés temporairement.

Ils ont ensuite été retirés du corpus et du dépôt public, car leurs conditions de réutilisation et leurs licences n'avaient pas été vérifiées. Deux diaporamas contenaient également le nom d'une personne réelle.

Le corpus conservé pour le projet repose donc sur des documents synthétiques. Aucun document de fabricant retiré ne doit être présenté comme faisant partie du corpus final.

Toute réintégration future d'un document réel nécessiterait :

- l'identification de son URL d'origine ;
- la vérification des conditions de réutilisation et de la licence ;
- la traçabilité de la recherche et du document ;
- l'anonymisation des données personnelles éventuellement présentes.

## 7. Addendum d'audit — traçabilité et limites (2026-11-06)

L'audit du présent fichier confirme que les entrées P01 à P15 sont des résumés des demandes adressées à l'IA et non des reproductions verbatim des prompts originaux.

Lorsque les historiques originaux ne sont pas disponibles, ces résumés sont conservés avec cette limite explicitement déclarée. Aucun texte n'a été inventé pour être présenté comme un prompt original.

L'analyse critique distingue les erreurs et incohérences relevées lors des audits assistés par IA, les corrections consignées dans l'inventaire et les limites qui nécessitent une validation humaine.

La relecture humaine doit être confirmée par les personnes nommées dans `verifie_par` (voir DECISIONS_A_VALIDER, points 11 et 12).

**Important :** ce corpus est un jeu de données synthétique destiné à l'évaluation d'un assistant documentaire. Il ne constitue pas une documentation constructeur et ne doit pas être utilisé comme source de consignes pour intervenir sur une installation industrielle réelle.



## 8. Photos externes (non générées par IA)

Cinq photographies sous licence Creative Commons (Wikimedia Commons) ont été ajoutées par l'équipe : `photo_ext_*`. Auteurs, licences et URL figurent dans l'inventaire. Elles n'ont pas été produites avec l'IA (`genere_par_ia = non`). Une sixième photo (réducteur miniature de type jouet) a été écartée car non représentative d'un équipement industriel.
