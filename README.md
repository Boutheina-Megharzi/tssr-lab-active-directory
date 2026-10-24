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
| Domaine | `tssrlab.local` (nom NetBIOS `TSSRLAB`) |
| Client | Windows 10 Pro (VM, à créer) |
| Réseau | Réseau interne `LAB-AD`, isolé, adresses IP privées |

## Plan d'adressage
| Machine | Adresse IP | Masque | Passerelle | DNS | État |
|---|---|---|---|---|---|
| SRV-DC01 | 192.168.10.10 | 255.255.255.0 | aucune | lui-même (`127.0.0.1`) | Contrôleur de domaine |
| Client Windows 10 | 192.168.10.20 | 255.255.255.0 | aucune | 192.168.10.10 | Prévu |

Les clients utiliseront `192.168.10.10` comme DNS, car le serveur héberge la zone `tssrlab.local`.

## Structure de l'annuaire

    tssrlab.local
    └── Entreprise
        ├── Direction
        ├── Comptabilité
        ├── Informatique
        ├── Marketing
        ├── Groupes
        └── Ordinateurs

Les utilisateurs et les groupes de sécurité de l'entreprise fictive seront créés à l'étape 8.

## Avancement
- [x] Étape 1 : préparation du laboratoire ([détails](docs/01-preparation.md))
- [x] Étape 2 : création de la VM serveur ([détails](docs/02-creation-vm-serveur.md))
- [x] Étape 3 : réseau isolé et adresses IP ([détails](docs/03-reseau-isole.md))
- [x] Étape 4 : renommage du serveur ([détails](docs/04-renommage-serveur.md))
- [x] Étape 5 : installation du rôle AD DS ([détails](docs/05-installation-adds.md))
- [x] Étape 6 : promotion en contrôleur de domaine, domaine `tssrlab.local` ([détails](docs/06-promotion-controleur-domaine.md))
- [x] Étape 7 : structure d'unités d'organisation ([détails](docs/07-structure-ou.md))
- [x] Étape 8 : utilisateurs et groupes de sécurité ([détails](docs/08-utilisateurs-groupes.md))
- [x] Étape 9 : affectation des utilisateurs aux groupes ([détails](docs/09-affectation-groupes.md))
- [ ] Étape 10 : création du poste client Windows 10 Pro
- [ ] Étape 11 : jonction du client au domaine et tests

## Points de retour (instantanés VirtualBox)
| Instantané | État du serveur |
|---|---|
| `03-reseau-ip-fixe-ok` | Réseau interne et IP fixe configurés |
| `04-serveur-renomme` | Serveur renommé `SRV-DC01` |
| `05-adds-installe` | Rôle AD DS installé, avant la promotion |
| `06-controleur-domaine-ok` | Contrôleur de domaine `tssrlab.local` opérationnel |
| `07-ou-creees` | Structure d'OU créée |
| `08-utilisateurs-groupes-crees` | 8 utilisateurs et 4 groupes de sécurité créés |
| `09-groupes-affectes` | Chaque utilisateur est membre du groupe de son service |

## Difficultés rencontrées
- Erreur PowerShell « chemin introuvable » : il manquait le `\` après `C:`. Résolue en écrivant le chemin complet.
- Installation de Windows 10 dans VirtualBox : erreur « L'ordinateur a redémarré de manière inattendue ». **Non résolue**, en cours d'analyse.
- Installation du serveur gelée avec 2 processeurs : résolue en passant à 1 processeur. VirtualBox fonctionne en mode `native API`, ce qui explique probablement l'instabilité (hypothèse non confirmée).
- Mot de passe Administrateur oublié : VM recréée, mot de passe désormais noté hors GitHub.
- Instantané oublié avant le changement de réseau : pris ensuite, avec un nom qui décrit l'état réel.
- Après la promotion, `nslookup` affiche un délai d'attente sur le serveur DNS tout en résolvant correctement `tssrlab.local`. Cause probable : pas de zone de recherche inversée. Hypothèse non confirmée.

## Choix techniques
- Le nom de domaine `tssrlab.local` convient à un laboratoire isolé. Le suffixe `.local` est déconseillé en production, car il peut entrer en conflit avec d'autres services réseau.
- La protection contre la suppression accidentelle reste cochée sur toutes les OU.

## Compétences travaillées
- Vérifier les ressources d'un PC avant de créer un laboratoire
- Calculer l'empreinte SHA256 d'un fichier avec PowerShell
- Créer et dimensionner une machine virtuelle
- Configurer un réseau interne et une adresse IP statique
- Tester une connectivité avec `ping` et lire `ipconfig /all`
- Renommer un serveur et vérifier le résultat
- Installer un rôle Windows Server (AD DS) avec le Gestionnaire de serveur
- Promouvoir un serveur en contrôleur de domaine et créer un domaine Active Directory
- Vérifier la résolution DNS avec `nslookup`
- Créer une structure d'unités d'organisation et protéger les objets contre la suppression accidentelle
- Diagnostiquer un gel d'installation en changeant un paramètre à la fois
- Créer des comptes utilisateurs et des groupes de sécurité dans Active Directory
-  Affecter des utilisateurs à des groupes de sécurité et vérifier l'appartenance
- Documenter un projet technique sur GitHub

Mis à jour après chaque étape.

