# Étape 5 : installation du rôle AD DS

## Objectif
Installer le rôle AD DS (Active Directory Domain Services) sur `SRV-DC01`. Le serveur n'est pas encore contrôleur de domaine : la promotion fait l'objet de l'étape 6.

## Notions utilisées
- **Rôle** : fonction que Windows Server peut remplir, installée comme un logiciel.
- **Installer le rôle AD DS** : Windows reçoit les programmes d'Active Directory. Aucun domaine n'existe à ce stade, et le serveur reste en `WORKGROUP`.
- **Promouvoir le serveur** : l'assistant crée le domaine et l'annuaire, puis le serveur devient contrôleur de domaine.
- **Domaine et groupe de travail** : en groupe de travail, chaque machine gère ses propres comptes. Dans un domaine, un annuaire central gère les comptes.

## Réalisation
1. Ouverture du Gestionnaire de serveur, puis Gérer > Ajouter des rôles et fonctionnalités.
2. Type d'installation : installation basée sur un rôle ou une fonctionnalité.
3. Sélection du serveur `SRV-DC01`.
4. Case « Services AD DS » cochée, avec ajout des fonctionnalités requises.
5. Installation, puis fermeture de l'assistant **sans** cliquer sur « Promouvoir ce serveur en contrôleur de domaine ».
6. Arrêt du serveur, puis instantané `05-adds-installe`.
7. Redémarrage et vérifications.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Rôle sélectionné | « Services AD DS » coché dans l'assistant | [05-assistant-role-adds.png](../screenshots/05-assistant-role-adds.png) |
| Installation | « Installation réussie sur SRV-DC01 », avec la mention « Configuration requise » | [05-assistant-resultat.png](../screenshots/05-assistant-resultat.png) |
| Menu et drapeau | AD DS dans le menu de gauche, drapeau d'avertissement jaune | [05-serveur-local-adds.png](../screenshots/05-serveur-local-adds.png) |
| Rôles après redémarrage | « Rôles : 2 » : AD DS et Services de fichiers et de stockage (déjà présent avant) | [05-tableau-de-bord-roles.png](../screenshots/05-tableau-de-bord-roles.png) |
| Outils disponibles | Utilisateurs et ordinateurs Active Directory, Domaines et approbations, Sites et services, Gestion des stratégies de groupe, Modification ADSI | [05-menu-outils.png](../screenshots/05-menu-outils.png) |
| Console AD | S'ouvre avec une croix rouge sur la racine, probablement parce qu'aucun domaine n'existe encore | [05-console-utilisateurs-ad.png](../screenshots/05-console-utilisateurs-ad.png) |
| Instantané | `05-adds-installe`, pris le 23/10/2026 à 21:09 | [05-snapshot.png](../screenshots/05-snapshot.png) |

Les instantanés forment une chaîne : `03-reseau-ip-fixe-ok`, puis `04-serveur-renomme`, puis `05-adds-installe`.

## Différence entre installer et promouvoir
[Réécris ici ta réponse, avec tes propres mots.]

## Points d'attention
- Les avertissements d'événements (Time-Service 134 et DNS Client Events 1014) sont toujours présents dans le Gestionnaire de serveur. Cause probable : le réseau est isolé, sans Internet. À confirmer.
- Le drapeau jaune est normal : il signale que la promotion reste à faire.

## Difficultés rencontrées
[Écris une difficulté réelle, ou « Aucune » si tu n'en as pas eu.]

## Prochaine étape
Promouvoir `SRV-DC01` en contrôleur de domaine et créer le domaine.
