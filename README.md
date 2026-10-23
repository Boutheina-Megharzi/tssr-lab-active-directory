# tssr-lab-active-directory

Laboratoire Active Directory sous Windows Server 2019 : domaine, OU, utilisateurs, groupes et poste client.

## C'est quoi Active Directory ?
Active Directory est l'annuaire central d'un réseau Windows. Il stocke les comptes utilisateurs, les groupes et les ordinateurs. Il permet de gérer qui peut se connecter et accéder à quoi, depuis un seul endroit.

## Objectifs
- Installer et configurer un serveur Windows Server 2019
- Créer un domaine Active Directory
- Organiser les utilisateurs, groupes et unités d'organisation (OU)
- Joindre un poste client au domaine
- Tester, diagnostiquer et documenter chaque étape

## Environnement
| Élément | Valeur |
|---|---|
| Hyperviseur | VirtualBox 7.2.20 |
| Serveur | Windows Server 2019 Standard Evaluation (VM `SRV-DC01`) |
| Client | Windows 10 Pro (VM, à créer) |
| Réseau | Réseau interne `LAB-AD`, isolé, adresses IP privées |

## Plan d'adressage
| Machine | Adresse IP | Masque | Passerelle | DNS | État |
|---|---|---|---|---|---|
| SRV-DC01 | 192.168.10.10 | 255.255.255.0 | aucune | 192.168.10.10 | Configuré |
| Client Windows 10 | 192.168.10.20 | 255.255.255.0 | aucune | 192.168.10.10 | Prévu |

## Avancement
- [x] Étape 1 : préparation du laboratoire ([détails](docs/01-preparation.md))
- [x] Étape 2 : création de la VM serveur ([détails](docs/02-creation-vm-serveur.md))
- [x] Étape 3 : réseau isolé et adresses IP ([détails](docs/03-reseau-isole.md))
- [x] Étape 4 : renommage du serveur ([détails](docs/04-renommage-serveur.md))
- [ ] Étape 5 : installation du rôle AD DS
- [ ] Étape 6 : création du domaine
- [ ] Étape 7 : OU, utilisateurs et groupes
- [ ] Étape 8 : création du poste client Windows 10 Pro
- [ ] Étape 9 : jonction du client au domaine et tests

## Difficultés rencontrées
- Erreur PowerShell « chemin introuvable » : il manquait le `\` après `C:`. Résolue en écrivant le chemin complet.
- Installation de Windows 10 dans VirtualBox : erreur « L'ordinateur a redémarré de manière inattendue ». **Non résolue**, en cours d'analyse.
- Installation du serveur gelée avec 2 processeurs : résolue en passant à 1 processeur.
- Mot de passe Administrateur oublié : VM recréée, mot de passe désormais noté hors GitHub.
- Instantané oublié avant le changement de réseau : pris ensuite, avec un nom qui décrit l'état réel.

## Compétences travaillées
- Vérifier les ressources d'un PC avant de créer un laboratoire
- Calculer l'empreinte SHA256 d'un fichier avec PowerShell
- Créer et dimensionner une machine virtuelle
- Configurer un réseau interne et une adresse IP statique
- Tester une connectivité avec `ping` et lire `ipconfig /all`
- Renommer un serveur et vérifier le résultat
- Diagnostiquer un gel d'installation en changeant un paramètre à la fois
- Documenter un projet technique sur GitHub

Mis à jour après chaque étape.

