# tssr-lab-active-directory

Laboratoire Active Directory sous Windows Server 2019 : domaine, OU, utilisateurs, groupes et poste client.

## C'est quoi Active Directory ?
Active Directory est l'annuaire central d'un réseau Windows. Il stocke les comptes, les groupes et les ordinateurs. Il décide qui peut se connecter et accéder à quoi.

## Objectifs
- J'installe un serveur Windows Server 2019.
- Je crée un domaine Active Directory.
- J'organise les utilisateurs, les groupes et les OU.
- Je joins un poste client au domaine.
- Je teste, je diagnostique et je documente chaque étape.

## Méthode de travail
1. Je prépare : je lis l'objectif et je vérifie les prérequis.
2. Je configure : j'applique un seul changement à la fois.
3. Je vérifie : je teste avec `ping`, `nslookup` ou `ipconfig /all`.
4. Je protège : je prends un instantané nommé selon l'état réel.
5. Je diagnostique : je relève l'erreur, je corrige, puis je confirme.
6. Je documente : je capture la preuve et j'écris ce que j'ai constaté.

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
| `10-vm-creee` (client) | VM client créée, Windows non installé |
| `10-windows10-installe` (client) | Windows 10 installé, hors domaine |
| `11-client-joint` (client) | Client joint au domaine |
| `11-client-joint-serveur` (serveur) | Client rangé dans l'OU `Ordinateurs` |

## Difficultés rencontrées
- **Jonction au domaine échouée** : j'ai obtenu l'erreur « Impossible de contacter un contrôleur de domaine ». J'ai constaté que le client portait l'adresse du serveur, signalée « en double ». J'ai corrigé en `192.168.10.20`, puis j'ai réussi la jonction. ([détails](docs/11-jonction-domaine.md))
- **Redémarrage sur l'installateur** : le client relançait l'installation de Windows 10. J'ai placé **Disque dur** en premier dans l'ordre de démarrage. ([détails](docs/10-creation-client.md))
- **Mauvais dossier partagé** : j'ai partagé `Partages` au lieu de `Direction`. J'ai supprimé ce partage, puis créé le bon. ([détails](docs/09-affectation-groupes.md))
- **Faute de frappe dans un identifiant** : j'ai tapé `clair.martin` au lieu de `claire.martin`. Je l'ai repéré avant de valider et corrigé aussi le champ « antérieur à Windows 2000 ». ([détails](docs/08-utilisateurs-groupes.md))
- **Instantané oublié avant le changement de réseau** : je l'ai pris ensuite, avec un nom fidèle à l'état réel : `03-reseau-ip-fixe-ok`. ([détails](docs/03-reseau-ip.md))
- **Erreur de chemin dans PowerShell** : il manquait le `\` après `C:`. J'ai écrit le chemin complet. ([détails](docs/01-preparation.md))

## Choix techniques
- J'utilise `tssrlab.local` en laboratoire isolé. J'évite `.local` en production.
- Je garde la protection contre la suppression accidentelle sur toutes les OU.
- Les comptes de test partagent un mot de passe de laboratoire, conservé hors du dépôt. En production, chaque utilisateur aurait le sien.

## Compétences travaillées
- Créer et dimensionner une machine virtuelle
- Calculer l'empreinte SHA256 d'un ISO
- Configurer un réseau interne et une IP statique
- Tester avec `ping`, `ipconfig /all` et `nslookup`
- Installer le rôle AD DS et promouvoir un contrôleur de domaine
- Créer des OU, des utilisateurs et des groupes de sécurité
- Partager un dossier, configurer des permissions NTFS et vérifier les droits effectifs
- Créer un poste client Windows 10 Pro et régler l'ordre d'amorçage
- Joindre un client au domaine et ouvrir une session de domaine
- Diagnostiquer une jonction échouée à partir de l'erreur, du DNS et de `ipconfig /all`
- Tester l'accès réel avec deux comptes et interpréter un refus
- Documenter un projet technique sur GitHub
