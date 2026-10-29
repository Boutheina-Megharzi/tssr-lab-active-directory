# tssr-lab-active-directory

Laboratoire Active Directory sous Windows Server 2019 : domaine, OU, utilisateurs, groupes et poste client.

## C'est quoi Active Directory ?
Active Directory est l'annuaire central d'un réseau Windows. Il stocke les comptes, les groupes et les ordinateurs. Il décide qui peut se connecter et accéder à quoi.

## Objectifs
- Installer un serveur Windows Server 2019
- Créer un domaine Active Directory
- Organiser les utilisateurs, groupes et OU
- Joindre un poste client au domaine
- Tester, diagnostiquer et documenter chaque étape

## Environnement
| Élément | Valeur |
|---|---|
| Hyperviseur | VirtualBox 7.2.20 |
| Serveur | Windows Server 2019 Standard Evaluation (VM `SRV-DC01`) |
| Client | Windows 10 Pro (VM `CLT-W10-01`), joint au domaine |
| Domaine | `tssrlab.local` (NetBIOS `TSSRLAB`) |
| Réseau | Réseau interne `LAB-AD`, isolé |

## Plan d'adressage
| Machine | Adresse IP | Masque | Passerelle | DNS |
|---|---|---|---|---|
| `SRV-DC01` | 192.168.10.10 | 255.255.255.0 | aucune | lui-même (127.0.0.1) |
| `CLT-W10-01` | 192.168.10.20 | 255.255.255.0 | aucune | 192.168.10.10 |

Le client utilise le serveur comme DNS, car celui-ci héberge la zone `tssrlab.local`.

## Structure de l'annuaire
```
tssrlab.local
└── Entreprise
    ├── Direction
    ├── Comptabilité
    ├── Informatique
    ├── Marketing
    ├── Groupes
    └── Ordinateurs
```
8 utilisateurs (2 par service) et 4 groupes globaux : `GG_Direction`, `GG_Comptabilite`, `GG_Informatique`, `GG_Marketing`.

## Avancement
- [x] Étape 1 : préparer le laboratoire ([détails](docs/01-preparation.md))
- [x] Étape 2 : créer la VM serveur ([détails](docs/02-creation-vm-serveur.md))
- [x] Étape 3 : configurer le réseau isolé et les adresses IP ([détails](docs/03-reseau-ip.md))
- [x] Étape 4 : renommer le serveur ([détails](docs/04-renommage-serveur.md))
- [x] Étape 5 : installer le rôle AD DS ([détails](docs/05-installation-adds.md))
- [x] Étape 6 : promouvoir le contrôleur de domaine `tssrlab.local` ([détails](docs/06-promotion-controleur-domaine.md))
- [x] Étape 7 : créer la structure d'OU ([détails](docs/07-creation-ou.md))
- [x] Étape 8 : créer les utilisateurs et les groupes de sécurité ([détails](docs/08-utilisateurs-groupes.md))
- [x] Étape 9 : affecter les groupes et partager un dossier de test ([détails](docs/09-affectation-groupes.md))
- [x] Étape 10 : créer le poste client Windows 10 Pro ([détails](docs/10-creation-client.md))
- [x] Étape 11 : joindre le client au domaine et tester les accès ([détails](docs/11-jonction-domaine.md))

## Points de retour (instantanés VirtualBox)
| Instantané | État |
|---|---|
| `03-reseau-ip-fixe-ok` | Réseau et IP fixe configurés |
| `04-serveur-renomme` | Serveur renommé `SRV-DC01` |
| `05-adds-installe` | Rôle AD DS installé |
| `06-controleur-domaine-ok` | Contrôleur de domaine opérationnel |
| `07-ou-creees` | OU créées |
| `08-utilisateurs-groupes-crees` | 8 utilisateurs et 4 groupes créés |
| `09-groupes-affectes` | Utilisateurs affectés aux groupes |
| `09-permissions-ntfs` | Dossier de test restreint à `GG_Direction` |
| `10-vm-creee` | VM client créée, Windows non installé |
| `10-windows10-installe` | Windows 10 installé, hors domaine |
| `11-client-joint` (client) | Client joint au domaine |
| `11-client-joint-serveur` (serveur) | Client rangé dans l'OU Ordinateurs |

## Difficultés rencontrées
- **Serveur gelé à l'installation** : passer de 2 à 1 processeur a résolu le problème. VirtualBox tourne en mode native API, cause probable (non confirmée).
- **Windows 10 : « L'ordinateur a redémarré de manière inattendue »** : cause non identifiée. L'installation a réussi avec 1 processeur et 4 Go. Un redémarrage sur l'installateur venait de l'ordre d'amorçage, corrigé (Disque dur en premier).
- **Mot de passe Administrateur oublié** : recréer la VM, puis noter le mot de passe hors GitHub.
- **Instantané oublié avant le changement de réseau** : le prendre ensuite, avec un nom fidèle à l'état réel.
- **`nslookup` : délai d'attente sur le serveur DNS**, alors que `tssrlab.local` est résolu. Cause probable : pas de zone de recherche inversée (non confirmée).
- **Mauvais dossier partagé** : détecté grâce au chemin réseau, puis corrigé.
- **Jonction au domaine échouée** : le client portait l'IP du serveur (« en double »). Corriger en `192.168.10.20` a permis la jonction.

## Choix techniques
- Utiliser `tssrlab.local` en laboratoire isolé. Éviter `.local` en production.
- Garder la protection contre la suppression accidentelle sur toutes les OU.
- Partager un mot de passe de laboratoire entre comptes de test, conservé hors du dépôt. En production, un mot de passe par utilisateur.

## Compétences travaillées
- Créer et dimensionner une machine virtuelle
- Configurer un réseau interne et une IP statique
- Tester avec `ping`, `ipconfig /all` et `nslookup`
- Installer le rôle AD DS et promouvoir un contrôleur de domaine
- Créer des OU, des utilisateurs et des groupes de sécurité
- Partager un dossier, configurer des permissions NTFS et vérifier les droits effectifs
- Joindre un client au domaine et ouvrir une session de domaine
- Diagnostiquer une jonction échouée à partir de l'erreur, du DNS et de `ipconfig /all`
- Tester l'accès réel avec deux comptes et interpréter un refus
- Documenter un projet sur GitHub

## Limites et améliorations
**Limites :** un seul contrôleur, pas de DHCP, pas de GPO, pas de zone inverse, Windows 10 en fin de support, mot de passe de test unique.

**Améliorations :** ajouter un DHCP et une zone inverse, créer des GPO sur l'OU `Ordinateurs`, passer le client à Windows 11, ajouter un second contrôleur.

