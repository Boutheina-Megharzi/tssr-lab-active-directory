# Étape 11 : jonction du client au domaine et tests d'accès

## Objectif
Joindre `CLT-W10-01` au domaine `tssrlab.local`, ouvrir des sessions avec des comptes du domaine, puis tester l'accès réel au dossier partagé `\\SRV-DC01\Direction`.

## Notions utilisées
- **Domaine et groupe de travail** : dans un domaine, le contrôleur centralise les comptes. En groupe de travail, chaque machine gère les siens.
- **Compte local et compte de domaine** : `adminlocal` existe seulement sur le client. `claire.martin` existe dans l'annuaire et fonctionne sur tous les postes du domaine.
- **DNS du client** : le client doit utiliser `192.168.10.10`, car seul le serveur connaît la zone `tssrlab.local`.
- **Authentification et autorisation** : l'authentification vérifie qui est l'utilisateur. L'autorisation décide de ce qu'il peut faire.
- **Objet ordinateur** : la jonction crée un compte pour la machine dans l'annuaire, d'abord dans le conteneur `Computers`.

## Réalisation
1. Démarrage du serveur, puis du client.
2. Configuration IP du client : `192.168.10.20`, masque `255.255.255.0`, sans passerelle, DNS `192.168.10.10`.
3. Renommage du client en `CLT-W10-01`.
4. Jonction au domaine `tssrlab.local` avec le compte `TSSRLAB\Administrateur`, puis redémarrage.
5. Ouverture de session avec `TSSRLAB\claire.martin`, puis test du dossier partagé.
6. Déconnexion complète, ouverture de session avec `TSSRLAB\lucas.petit`, puis même test.
7. Déplacement de l'objet ordinateur vers l'OU `Entreprise/Ordinateurs`.
8. Arrêt des deux machines et instantanés.

Impact : le client crée un compte d'ordinateur dans l'annuaire. Les mots de passe ne sont pas publiés dans ce dépôt.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Message de jonction | « Bienvenue dans le domaine tssrlab.local » | [11-bienvenue-domaine.png](../screenshots/11-bienvenue-domaine.png) |
| IP et DNS du client après correction, `nslookup tssrlab.local` | Constatés et validés, sans capture conservée | |
| Session de domaine de Claire | `whoami` : `tssrlab\claire.martin`, `%USERDOMAIN%` : `TSSRLAB` | [11-whoami-claire.png](../screenshots/11-whoami-claire.png) |
| Accès de Claire à `\\SRV-DC01\Direction` | Accepté : le fichier `test-claire.txt` a été créé (15:28:08) puis modifié (15:33:56), 11 octets | [11-acces-claire.png](../screenshots/11-acces-claire.png) |
| Session de domaine de Lucas | `whoami` : `tssrlab\lucas.petit` | [11-whoami-lucas.png](../screenshots/11-whoami-lucas.png) |
| Accès de Lucas à `\\SRV-DC01\Direction` | Refusé : « Vous n'avez pas l'autorisation d'accéder à \\SRV-DC01\Direction. Contactez l'administrateur réseau » | [11-acces-lucas.png](../screenshots/11-acces-lucas.png) |
| Objet ordinateur après la jonction | `CLT-W10-01` dans le conteneur `Computers` | [11-client-computers.png](../screenshots/11-client-computers.png) |
| Objet ordinateur après déplacement | `CLT-W10-01` dans `Entreprise/Ordinateurs` | [11-client-ou-ordinateurs.png](../screenshots/11-client-ou-ordinateurs.png) |
| Instantané du client | `11-client-joint` | [11-snapshot-client.png](../screenshots/11-snapshot-client.png) |
| Instantané du serveur | `11-client-joint-serveur` | [11-snapshot-serveur.png](../screenshots/11-snapshot-serveur.png) |

## Conclusion du test
Les deux comptes sont authentifiés par le domaine avec le même mot de passe de laboratoire. Claire, membre de `GG_Direction`, accède au dossier et peut y écrire. Lucas, membre de `GG_Comptabilite`, est refusé. La différence vient donc de l'autorisation, c'est-à-dire de l'appartenance aux groupes. Ce résultat correspond au calcul des droits effectifs de l'étape 9.

## Difficultés rencontrées
- **Échec de la première jonction** : l'erreur « Impossible de contacter un contrôleur de domaine Active Directory » (délai dépassé) est apparue. Le détail indiquait que le serveur DNS `192.168.10.10` ne répondait pas, pour la requête `_ldap._tcp.dc._msdcs.tssrlab.local`. `ipconfig /all` a montré que le client portait par erreur l'adresse du serveur (`192.168.10.10`, signalée « en double »), ce qui avait déclenché une adresse automatique en `169.254`. L'adresse a été corrigée en `192.168.10.20`, puis la jonction a réussi. Preuves : [11-erreur-jonction.png](../screenshots/11-erreur-jonction.png), [11-erreur-jonction-details.png](../screenshots/11-erreur-jonction-details.png) et [11-client-ipconfig-avant.png](../screenshots/11-client-ipconfig-avant.png)

## Points d'attention
- Le test d'accès repose sur deux comptes et un seul dossier partagé. Il ne couvre pas les autres services.
- Les comptes de test partagent un mot de passe de laboratoire. En production, chaque utilisateur aurait le sien.
- Aucune stratégie de groupe (GPO) n'est encore appliquée à l'OU `Ordinateurs`.

## Prochaine étape
Bilan du laboratoire, tests complémentaires (DNS, pare-feu) et améliorations possibles.
