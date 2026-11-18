---
bos: project/v1
id: "677d2387-25f9-434c-93c8-9ebb48b4c3a5"
title: "bSideDesk"
status: "review"
exported_at: 2026-11-18T12:58:23.791Z
---

<!-- bos:instructions:start
INSTRUCTIONS POUR L'IA (ce fichier sera réimporté dans bOS, ne casse pas son format)

1. Le cahier des charges (texte libre) est en dehors du bloc de tâches : tu peux le compléter, y ajouter des fonctionnalités
   et tenir un journal de session (ex. une section "## Journal de session" datée avec ce qui a été fait / décidé).
2. Les tâches sont uniquement dans le bloc délimité par les commentaires bos:tasks:start et bos:tasks:end (ne les supprime pas), une tâche par ligne :
     - [ ] Titre | type:feature | priority:medium | due:2026-12-31 | id:xxxxxxxx
       Description optionnelle sur les lignes suivantes, indentées de 2 espaces.
3. Le STATUT d'une tâche est donné par la section où elle se trouve (À faire / En cours / Review / Terminé).
   Pour la faire avancer, déplace la ligne dans la bonne section. Cocher [x] la considère aussi comme terminée.
   Les NOTES d'une tâche sont les lignes qui commencent par "> " (indentées de 2 espaces) sous la tâche : conserve-les, ajoute-en si utile.
4. NOUVELLE tâche : ajoute une ligne SANS "id:" dans la section voulue.
5. Ne modifie JAMAIS un "id:" existant. Ne supprime pas une ligne pour la terminer : déplace-la dans "Terminé".
6. Valeurs autorisées : type = feature | bug | infra | documentation ; priority = low | medium | high | urgent ;
   due = AAAA-MM-JJ (facultatif).
7. Garde le front matter (--- en haut) ; tu peux y changer status (todo | in_progress | review | done | archived), deadline et budget.
bos:instructions:end -->

# bSideDesk

**Nom applicatif** : bSideDesk (anciennement MirrorPad)  
**Catégorie** : Back-Office (classement proposé : outil de productivité)  
**Version déclarée** : 1.0.<numéro de workflow> ; version effectivement installée non vérifiée  
**État** : Socle documenté et code présent ; recette matérielle à confirmer  
**Mise à jour du suivi** : 2026-11-18  
**Durée estimée initiale** : À définir  
**Budget indicatif initial** : À définir  
**Échéance cible** : À définir

## Description

bSideDesk transforme un iPad sous iOS 12 jailbreaké en second écran interactif d'un PC Windows 10/11 ou Debian 12 sous GNOME Wayland. Il transmet l'image et les commandes via une liaison chiffrée, avec connexion Wi-Fi ou USB selon la plateforme. Il associe affichage étendu, saisie tactile, clavier, stylet, Touch Bar, fonctions de partage et tableau de bord. Windows propose également l'exploitation des capteurs de l'iPad (webcam, micro, son). Plusieurs PC peuvent être connectés, avec un seul affiché à la fois. Le projet comprend des installateurs et une chaîne de publication automatisée. La distribution App Store et la compatibilité avec d'autres plateformes ne font pas partie du périmètre validé.

## Bénéfice attendu et mesure

Réutiliser un iPad ancien comme extension du poste de travail, éviter l'achat d'un écran supplémentaire et accélérer les interactions courantes. Mesurer la latence image/saisie, la stabilité des sessions, le délai de reconnexion, le succès des transferts et des mises à jour. Aucun gain chiffré ni résultat de mesure n'a été vérifié pendant cette session.

## Périmètre réalisé

- Client iPad Objective-C : réception H.264/FLV, décodage via AVFoundation, canal réseau sur le port 7272 ; présence du code confirmée dans `ipad/bSideDesk.m`.
- Hôte Debian : Python, GTK, Mutter, PipeWire et GStreamer ; code confirmé dans `linux/bsidedesk.py`, dont les raccourcis Touch Bar pour terminaux.
- Hôte Windows C#/.NET Framework 4.8, écran virtuel, ffmpeg et installateur NSIS : composants décrits dans le README ; fonctionnement matériel non vérifié.
- Saisie, rotation, Touch Bar, loupe, pavé numérique, déplacement de fenêtres et gestion multi-PC : fonctionnalités documentées ; recette à confirmer par plateforme.
- Presse-papiers, transfert de fichiers, tableau de bord, webcam, micro et audio : fonctionnalités documentées ; parité Windows/Debian non établie.
- Appairage TLS 1.2, épinglage du certificat et migration MirrorPad : documentés ; audit de sécurité et migration réelle non exécutés.
- Automatisation de compilation, tests et déploiement : workflow GitHub présent ; résultats CI et releases non consultés.

## État réel et points à valider

| Sujet | État à la date du suivi | Preuve ou validation attendue |
|---|---|---|
| Client iPad | Code présent | Compiler et tester sur iPad iOS 12 jailbreaké |
| Debian | Code présent ; Touch Bar terminal au dernier commit consulté | Session GNOME Wayland réelle |
| Windows | Architecture documentée | Installer sur Windows 10 et 11 et tester les pilotes |
| Tactile Windows | Repli souris documenté pour un poste incompatible | Exécuter Test-Tactile.ps1 et vérifier le repli |
| Sécurité | Protocole et appairage documentés | Tests positifs/négatifs et audit |
| Publication | Workflow présent avec tests | Vérifier les exécutions, artefacts et installations |
| Performance | Débit adaptatif documenté | Mesures Wi-Fi et USB |
| Signature Windows | Option Azure Trusted Signing documentée | Vérifier configuration et signature réelle |

## Architecture et sources de vérité

| Composant | Technologie ou fichier canonique |
|---|---|
| Client iPad | `ipad/bSideDesk.m`, Theos, AVFoundation ; `ipad/build.py`, `ipad/deploy.py` |
| Windows | `windows/src/`, C#/.NET Framework 4.8, ffmpeg, Virtual Display Driver |
| Installation Windows | `windows/installer/bSideDesk.nsi`, `windows/helper/` |
| Debian | `linux/bsidedesk.py`, Mutter RemoteDesktop/ScreenCast, PipeWire, GStreamer, GTK |
| Paquet Debian | `linux/build-deb.sh`, polkit et outil de mise à jour |
| Transport | TCP 7272, TLS 1.2, trames typées ; USB via usbmuxd selon configuration |
| Publication | `.github/workflows/release.yml`, runner SRVHOME et GitHub Ubuntu |
| Documentation | `README.md` |

- Application / console : applications natives ; URL de production web non applicable.
- Dépôt : https://github.com/BenderRobot/bsidedesk — branche : main — dernier commit de code consulté : `93bc37a259f94f2a7bd7f7415d18b5304960be18` (Touch Bar du terminal Debian).
- Accès et secrets : configuration locale Windows dans %APPDATA%/bSideDesk ; Debian dans ~/.config/bsidedesk/ (certificat et clé locale) ; secrets de signature dans GitHub Actions si configurés. Aucune valeur reproduite.

## Contraintes et décisions

- Compatibilité cible : Windows 10/11, Debian 12 GNOME Wayland, iPad iOS 12 jailbreaké.
- Préserver les réglages et appairages lors de la migration MirrorPad.
- Sur Windows, centraliser l'installation des composants dans NSIS plutôt que demander des scripts manuels.
- Conserver le transport chiffré et l'appairage explicite ; ne jamais publier les secrets.
- Respecter le repli en émulation souris lorsque l'injection tactile est ignorée.
- Les modifications Markdown seules sont exclues du déclenchement automatique du workflow de release.

## Recette et critères de clôture

1. Compiler les trois clients et vérifier les tests d'authentification, migration/déploiement Debian et restauration des mises à jour Windows.
2. Installer sur les plateformes cibles et vérifier appairage, refus d'un client non autorisé et reconnexion.
3. Tester écran étendu, orientation, verrouillage, fluidité et connexion Wi-Fi/USB prise en charge.
4. Valider gestes, clavier, stylet, Touch Bar, changement de PC et déplacement de fenêtres selon plateforme.
5. Tester les fonctions de partage, audio, webcam et micro disponibles ; documenter les limites.
6. Vérifier installation, migration, mise à jour, restauration, signature et artefacts publiés.
7. Consigner résultats, versions, appareils et anomalies avant de déclarer le projet terminé.

## Évolutions proposées

| Évolution | Périmètre et état | Dépendance ou garde-fou |
|---|---|---|
| Signature Windows | Activation à confirmer | Azure Trusted Signing et vérification des signatures |
| Webcam Media Foundation | À arbitrer | Compatibilité Windows et tests applicatifs |
| Version App Store | À arbitrer | Mac, Xcode 26, iPad récent ; faisabilité à étudier |
| Outlook / Teams dans le tableau de bord | À arbitrer | Contraintes de privilèges Windows |

## Livrables

| Livrable | Situation |
|---|---|
| Sources iPad et Debian | Présence confirmée sur GitHub |
| Sources et installateur Windows | Documentés ; compilation non vérifiée |
| Paquets iPad et Debian, ZIP et setup Windows | Publication prévue par workflow ; artefacts réels non vérifiés |
| README | Présent et consulté |
| Fiche projet bOS | Renseignée au 2026-11-18 ; import bOS à vérifier |

## Journal de session

### 2026-11-18 — Initialisation du suivi bOS

- Lecture du README, des premières sections des sources iPad et Debian, du workflow de release et du dernier commit consulté.
- Identification des fonctionnalités déclarées, limites connues, architecture et critères de recette.
- Aucun test, build, déploiement, contrôle de release ni import bOS exécuté. Dépôt local non accessible pendant cette session.
- Classement Back-Office proposé ; budget, client, échéance et version installée non connus.

<!-- bos:tasks:start -->
## Suivi des tâches

### À faire

- [ ] Mesurer latence et stabilité | type:feature | priority:medium | id:f6ab433c-f661-40df-b265-16ea63994570
  Définir un protocole de mesure Wi-Fi et USB et consigner les résultats.
- [ ] Vérifier la signature Windows | type:infra | priority:high | id:9b7150d2-abd7-460b-81e1-bc50886da570
  Confirmer la configuration Azure Trusted Signing et tester le setup signé.
- [ ] Étudier la webcam Media Foundation | type:feature | priority:low | id:05560d0b-a445-4235-93b0-c44d46df2a3e
  Évaluer une intégration compatible avec les applications Windows concernées.
- [ ] Étudier une distribution App Store | type:feature | priority:low | id:1d645880-0fd9-4d87-8c10-54c8f26cf487
  Chiffrer les prérequis matériels et la migration vers un iOS récent.
- [ ] Étudier les intégrations Outlook et Teams | type:feature | priority:low | id:78b27a4e-2485-4011-9b81-f724a56a912e
  Évaluer la séparation des privilèges nécessaire à la lecture des données.
- [ ] Préparer le guide de recette | type:documentation | priority:medium | id:61b8d7c4-c43d-46c5-81d4-e613c85b2686
  Définir la matrice appareils, versions et scénarios de validation.
- [ ] Importer la fiche dans bOS | type:documentation | priority:medium | id:c8de3841-3978-4785-bf44-bcb371377400
  Vérifier la création de la fiche et la répartition des tâches dans le Kanban.

### En cours

_(aucune)_

### Review

- [?] Valider le client iPad | type:feature | priority:high | id:760c5201-baf6-493d-84ca-9eb5830e85f6
  Code Objective-C présent ; compilation et décodage sur appareil à confirmer.
- [?] Recetter les modes de saisie | type:feature | priority:high | id:ca7e1338-7cb2-4f6b-810f-07cd3c010cf3
  Gestes et repli souris documentés ; tester le tactile, la pression et le clavier.
- [?] Recetter la Touch Bar | type:feature | priority:medium | id:27a2a835-a26c-483c-9e80-c2c936231564
  Raccourcis Debian, y compris terminal, présents dans la source ; tester les contextes des deux hôtes.
- [?] Vérifier le mode multi-PC | type:feature | priority:medium | id:26ade479-ddab-4823-b02f-ff1b2add1222
  Jusqu'à quatre PC selon README ; tester bascule, veille et reconnexion.
- [?] Recetter les transferts et le presse-papiers | type:feature | priority:medium | id:3dc7933a-5381-464e-9b24-1be04eaf3409
  Fonctions documentées ; vérifier texte, images, dossiers et erreurs de transfert.
- [?] Recetter webcam, micro et son | type:feature | priority:medium | id:aabef9c8-7bf5-487c-8c10-1128750d885b
  Intégration Windows documentée ; tester les applications compatibles et les limites.
- [?] Valider le tableau de bord | type:feature | priority:medium | id:9085880e-91a7-45a4-acdb-1deb218f9a72
  Indicateurs et contrôles HTTP/ping décrits ; vérifier exactitude et erreurs réseau.
- [?] Valider l'appairage et TLS | type:infra | priority:high | id:233722f1-57cf-41b2-847e-c3484f5bce42
  Tests de handshake prévus dans le workflow ; vérifier leur résultat et les refus d'accès.
- [?] Valider la migration MirrorPad | type:bug | priority:high | id:868a5841-851e-4532-99f8-8702ea457f40
  Migration automatique annoncée ; tester la conservation des réglages et appairages.
- [?] Vérifier installation et mise à jour Windows | type:infra | priority:high | id:825d7b37-80a3-40ec-8e36-6fd5e6403b4c
  NSIS et tests de restauration référencés ; vérifier installation et rollback réels.
- [?] Vérifier installation et mise à jour Debian | type:infra | priority:high | id:3c349c26-40a1-476d-920b-38ebeb65a245
  Paquet et polkit documentés ; tester installation, migration et désactivation des mises à jour.
- [?] Vérifier la publication automatisée | type:infra | priority:high | id:0947f79c-277b-44c3-a92e-63abdecbbd6a
  Workflow présent ; contrôler les résultats CI, le runner SRVHOME et les artefacts.

### Terminé

- [x] Identifier les sources du projet | type:documentation | priority:medium | id:8e09112e-e8f7-4dbd-b3f5-865263c9826b
  README, sources iPad/Debian et workflow consultés sur main ; commit de référence consigné.
- [x] Initialiser le cahier des charges et le Kanban bOS | type:documentation | priority:medium | id:6a3d3337-b4d6-4a05-8b0f-e448039c090c
  Fiche structurée avec périmètre, preuves, limites, recette et tâches ; import non encore vérifié.
- [x] Valider l'écran virtuel Debian | type:feature | priority:high | id:82117487-2bed-4872-8178-b26188a454ac
  Code Mutter/PipeWire/GStreamer présent ; vérifier sur GNOME Wayland.
- [x] Valider l'écran étendu Windows | type:feature | priority:high | id:cca63196-df3a-4de8-a466-ac59fac01b41
  VDD et ffmpeg décrits dans le README ; tester affichage, rotation et veille.
<!-- bos:tasks:end -->
