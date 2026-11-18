---
bos: project/v1
title: "bSideDesk"
status: review
budget:
deadline:
category: back_office
service_type:
client:
---

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

- [ ] Mesurer latence et stabilité | type:feature | priority:medium
  Définir un protocole de mesure Wi-Fi et USB et consigner les résultats.
- [ ] Vérifier la signature Windows | type:infra | priority:high
  Confirmer la configuration Azure Trusted Signing et tester le setup signé.
- [ ] Étudier la webcam Media Foundation | type:feature | priority:low
  Évaluer une intégration compatible avec les applications Windows concernées.
- [ ] Étudier une distribution App Store | type:feature | priority:low
  Chiffrer les prérequis matériels et la migration vers un iOS récent.
- [ ] Étudier les intégrations Outlook et Teams | type:feature | priority:low
  Évaluer la séparation des privilèges nécessaire à la lecture des données.
- [ ] Préparer le guide de recette | type:documentation | priority:medium
  Définir la matrice appareils, versions et scénarios de validation.
- [ ] Importer la fiche dans bOS | type:documentation | priority:medium
  Vérifier la création de la fiche et la répartition des tâches dans le Kanban.

### En cours

### Review

- [?] Valider le client iPad | type:feature | priority:high
  Code Objective-C présent ; compilation et décodage sur appareil à confirmer.
- [?] Valider l'écran étendu Windows | type:feature | priority:high
  VDD et ffmpeg décrits dans le README ; tester affichage, rotation et veille.
- [?] Valider l'écran virtuel Debian | type:feature | priority:high
  Code Mutter/PipeWire/GStreamer présent ; vérifier sur GNOME Wayland.
- [?] Recetter les modes de saisie | type:feature | priority:high
  Gestes et repli souris documentés ; tester le tactile, la pression et le clavier.
- [?] Recetter la Touch Bar | type:feature | priority:medium
  Raccourcis Debian, y compris terminal, présents dans la source ; tester les contextes des deux hôtes.
- [?] Vérifier le mode multi-PC | type:feature | priority:medium
  Jusqu'à quatre PC selon README ; tester bascule, veille et reconnexion.
- [?] Recetter les transferts et le presse-papiers | type:feature | priority:medium
  Fonctions documentées ; vérifier texte, images, dossiers et erreurs de transfert.
- [?] Recetter webcam, micro et son | type:feature | priority:medium
  Intégration Windows documentée ; tester les applications compatibles et les limites.
- [?] Valider le tableau de bord | type:feature | priority:medium
  Indicateurs et contrôles HTTP/ping décrits ; vérifier exactitude et erreurs réseau.
- [?] Valider l'appairage et TLS | type:infra | priority:high
  Tests de handshake prévus dans le workflow ; vérifier leur résultat et les refus d'accès.
- [?] Valider la migration MirrorPad | type:bug | priority:high
  Migration automatique annoncée ; tester la conservation des réglages et appairages.
- [?] Vérifier installation et mise à jour Windows | type:infra | priority:high
  NSIS et tests de restauration référencés ; vérifier installation et rollback réels.
- [?] Vérifier installation et mise à jour Debian | type:infra | priority:high
  Paquet et polkit documentés ; tester installation, migration et désactivation des mises à jour.
- [?] Vérifier la publication automatisée | type:infra | priority:high
  Workflow présent ; contrôler les résultats CI, le runner SRVHOME et les artefacts.

### Terminé

- [x] Identifier les sources du projet | type:documentation | priority:medium
  README, sources iPad/Debian et workflow consultés sur main ; commit de référence consigné.
- [x] Initialiser le cahier des charges et le Kanban bOS | type:documentation | priority:medium
  Fiche structurée avec périmètre, preuves, limites, recette et tâches ; import non encore vérifié.
<!-- bos:tasks:end -->
