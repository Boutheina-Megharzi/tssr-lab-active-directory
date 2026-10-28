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
| Client | Windows 10 Pro (VM `CLT-W10-01`, pas encore joint au domaine) |
| Réseau | Réseau interne `LAB-AD`, isolé, adresses IP privées |
| Client | Windows 10 Pro (VM `CLT-W10-01`, joint au domaine `tssrlab.local`) |

## Plan d'adressage
| Machine | Adresse IP | Masque | Passerelle | DNS | État |
|---|---|---|---|---|---|
| SRV-DC01 | 192.168.10.10 | 255.255.255.0 | aucune | lui-même (`127.0.0.1`) | Contrôleur de domaine |
| Client Windows 10 | 192.168.10.20 | 255.255.255.0 | aucune | 192.168.10.10 | Configuré, joint au domaine |

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

Entreprise fictive de 8 utilisateurs, à raison de deux par service, et 4 groupes de sécurité globaux : `GG_Direction`, `GG_Comptabilite`, `GG_Informatique` et `GG_Marketing`.

## Avancement
- [x] Étape 1 : préparation du laboratoire ([détails](docs/01-preparation.md))
- [x] Étape 2 : création de la VM serveur ([détails](docs/02-creation-vm-serveur.md))
- [x] Étape 3 : réseau isolé et adresses IP ([détails](docs/03-reseau-isole.md))
- [x] Étape 4 : renommage du serveur ([détails](docs/04-renommage-serveur.md))
- [x] Étape 5 : installation du rôle AD DS ([détails](docs/05-installation-adds.md))
- [x] Étape 6 : promotion en contrôleur de domaine, domaine `tssrlab.local` ([détails](docs/06-promotion-controleur-domaine.md))
- [x] Étape 7 : structure d'unités d'organisation ([détails](docs/07-structure-ou.md))
- [x] Étape 8 : utilisateurs et groupes de sécurité ([détails](docs/08-utilisateurs-groupes.md))
- [x] Étape 9 : affectation aux groupes et dossier partagé de test ([détails](docs/09-affectation-groupes.md))
- [x] Étape 10 : création du poste client Windows 10 Pro ([détails](docs/10-creation-client.md))
- [x] Étape 11 : jonction du client au domaine et tests d'accès ([détails](docs/11-jonction-domaine.md))

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
| `09-permissions-ntfs` | Groupes affectés, dossier de test restreint à `GG_Direction` |
| `10-vm-creee` | VM client créée, Windows non installé |
| `10-windows10-installe` | Windows 10 Pro installé, compte local, hors domaine |
| `11-client-joint` (client) | Client Windows 10 Pro joint au domaine |
| `11-client-joint-serveur` (serveur) | Domaine avec un client joint, rangé dans l'OU Ordinateurs |

## Difficultés rencontrées
- Erreur PowerShell « chemin introuvable » : il manquait le `\` après `C:`. Résolue en écrivant le chemin complet.
- Installation de Windows 10 dans VirtualBox : erreur « L'ordinateur a redémarré de manière inattendue ». **Non résolue**, en cours d'analyse.
- Installation du serveur gelée avec 2 processeurs : résolue en passant à 1 processeur. VirtualBox fonctionne en mode `native API`, ce qui explique probablement l'instabilité (hypothèse non confirmée).
- Mot de passe Administrateur oublié : VM recréée, mot de passe désormais noté hors GitHub.
- Instantané oublié avant le changement de réseau : pris ensuite, avec un nom qui décrit l'état réel.
- Après la promotion, `nslookup` affiche un délai d'attente sur le serveur DNS tout en résolvant correctement `tssrlab.local`. Cause probable : pas de zone de recherche inversée. Hypothèse non confirmée.
- Test des permissions : le dossier parent a été partagé par erreur à la place du sous-dossier, détecté grâce au chemin réseau affiché, puis corrigé. Une fenêtre de droits d'OU a aussi été ouverte à la place de celle du dossier, sans modification enregistrée.
- Installation de Windows 10 dans VirtualBox : une première tentative a échoué avec l'erreur « L'ordinateur a redémarré de manière inattendue » (cause non identifiée). L'installation a ensuite réussi avec 1 processeur et 4 Go de mémoire. Un redémarrage sur l'installateur causé par l'ordre d'amorçage a été corrigé.
- Première jonction au domaine échouée (délai dépassé) : le client portait l'adresse IP du serveur, signalée « en double ». Corrigée en `192.168.10.20`, puis jonction réussie.
  
## Choix techniques
- Le nom de domaine `tssrlab.local` convient à un laboratoire isolé. Le suffixe `.local` est déconseillé en production, car il peut entrer en conflit avec d'autres services réseau.
- La protection contre la suppression accidentelle reste cochée sur toutes les OU.
- Les comptes de test partagent un mot de passe de laboratoire, conservé hors du dépôt. En production, chaque utilisateur aurait son propre mot de passe.

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
- Créer des comptes utilisateurs et des groupes de sécurité, puis affecter les utilisateurs aux groupes
- Partager un dossier, configurer des permissions NTFS pour un groupe et vérifier des droits effectifs
- Diagnostiquer un gel d'installation en changeant un paramètre à la fois
- Créer une machine cliente Windows 10, vérifier son édition avec `winver` et régler l'ordre d'amorçage
- Documenter un projet technique sur GitHub
- Configurer un poste client, le joindre à un domaine et ouvrir une session de domaine
- Diagnostiquer une jonction échouée à partir du code d'erreur, du détail DNS et de `ipconfig /all`
- Tester l'accès réel à un dossier partagé avec deux comptes et interpréter un refus d'autorisation

## Limites du laboratoire et améliorations possibles
**Limites**
- Un seul contrôleur de domaine, sans redondance.
- Aucun service DHCP : toutes les adresses sont saisies à la main.
- Aucune GPO ni zone de recherche inversée.
- Windows 10 n'est plus supporté depuis octobre 2025, et le réseau est isolé.
- Mot de passe de laboratoire unique pour les comptes de test.

**Améliorations possibles**
- Ajouter un service DHCP et une zone de recherche inversée.
- Créer des GPO (fond d'écran, restrictions) liées à l'OU `Ordinateurs`.
- Remplacer le client par Windows 11 Pro.
- Ajouter un second contrôleur de domaine.
- Automatiser la création des utilisateurs avec PowerShell, une fois cet outil vu en cours.

Mis à jour après chaque étape.

