# tssr-lab-active-directory

Laboratoire Active Directory sous Windows Server 2019 : domaine, OU, utilisateurs, groupes et poste client.
## C'est quoi Active Directory ?

Active Directory est l'annuaire central d'un réseau Windows. Il stocke les comptes utilisateurs, les groupes et les ordinateurs, et permet de gérer qui peut se connecter et accéder à quoi, depuis un seul endroit.

## Objectifs

- Installer et configurer un serveur Windows Server 2019
- Créer un domaine Active Directory
- Organiser les utilisateurs, groupes et unités d'organisation (OU)
- Joindre un poste client au domaine
- Tester, diagnostiquer et documenter chaque étape

## Environnement

- Hyperviseur : VirtualBox 7.2.20
- Serveur : Windows Server 2019 (VM)
- Client : Windows 10 Pro (VM, à créer)
- Réseau : isolé, adresses IP privées (à configurer)

## Avancement

- [x] Étape 1 : préparation du laboratoire ([détails](docs/01-preparation.md))
- [x] Étape 2 : création de la VM serveur ([détails](docs/02-creation-vm-serveur.md))
- [ ] Étape 3 : réseau isolé et adresses IP

## Difficultés rencontrées

- Erreur « chemin introuvable » dans PowerShell : résolue en ouvrant PowerShell depuis le bon dossier
- Écran bleu à l'installation de Windows 10 dans VirtualBox : en cours d'analyse

## Compétences acquises

- Vérifier les ressources d'un PC avant de créer un laboratoire
- Contrôler un fichier avec son empreinte (SHA256)
- Créer et dimensionner une machine virtuelle
- Utiliser PowerShell pour des vérifications simples
- Documenter un projet technique sur GitHub

*Mis à jour après chaque étape.*
