# Étape 11 : jonction du client au domaine et tests d'accès

## Objectif
Je joins `CLT-W10-01` au domaine `tssrlab.local`, j'ouvre des sessions avec des comptes du domaine, puis je teste l'accès réel au dossier partagé `\\SRV-DC01\Direction`.

## Notions utilisées
- **Domaine et groupe de travail** : dans un domaine, le contrôleur centralise les comptes. En groupe de travail, chaque machine gère les siens.
- **Compte local et compte de domaine** : `adminlocal` existe seulement sur le client. `claire.martin` existe dans l'annuaire et fonctionne sur tous les postes du domaine.
- **DNS du client** : le client doit utiliser `192.168.10.10`, car seul le serveur connaît la zone `tssrlab.local`.
- **Authentification et autorisation** : l'authentification vérifie qui est l'utilisateur. L'autorisation décide de ce qu'il peut faire.
- **Objet ordinateur** : la jonction crée un compte pour la machine dans l'annuaire, d'abord dans le conteneur `Computers`.

## Réalisation
1. Je démarre le serveur, puis le client.
2. Je configure l'IP du client : `192.168.10.20`, masque `255.255.255.0`, sans passerelle, DNS `192.168.10.10`.
3. Je renomme le client en `CLT-W10-01`.
4. Je joins le domaine `tssrlab.local` avec le compte `TSSRLAB\Administrateur`, puis je redémarre.
5. J'ouvre une session avec `TSSRLAB\claire.martin`, puis je teste le dossier partagé.
6. Je me déconnecte complètement, j'ouvre une session avec `TSSRLAB\lucas.petit`, puis je refais le même test.
7. Je déplace l'objet ordinateur vers l'OU `Entreprise/Ordinateurs`.
8. J'arrête les deux machines et je prends les instantanés.

Impact : le client crée un compte d'ordinateur dans l'annuaire. Les mots de passe ne sont pas publiés dans ce dépôt.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Diagnostic avant correction : `ipconfig /all` | Adresse `192.168.10.10` signalée « en double » et adresse automatique `169.254.47.126` | [11-client-ipconfig-avant.png](../screenshots/11-client-ipconfig-avant.png) |
| Diagnostic avant correction : `nslookup` et `ping` du serveur | Échec : « No response from server », puis 4 paquets perdus sur 4 | [11-client-nslookup-ping-echec.png](../screenshots/11-client-nslookup-ping-echec.png) |
| Message de jonction (après correction) | « Bienvenue dans le domaine tssrlab.local » | [11-bienvenue-domaine.png](../screenshots/11-bienvenue-domaine.png) |
| IP `192.168.10.20`, DNS et `nslookup tssrlab.local` après correction | Constatés et validés, sans capture conservée | |
| Session de domaine de Claire | `whoami` : `tssrlab\claire.martin`, `%USERDOMAIN%` : `TSSRLAB` | [11-whoami-claire.png](../screenshots/11-whoami-claire.png) |
| Accès de Claire à `\\SRV-DC01\Direction` | Accepté : le fichier `test-claire.txt` a été créé (15:28:08) puis modifié (15:33:56), 11 octets | [11-acces-claire.png](../screenshots/11-acces-claire.png) |
| Session de domaine de Lucas | `whoami` : `tssrlab\lucas.petit` | [11-whoami-lucas.png](../screenshots/11-whoami-lucas.png) |
| Accès de Lucas à `\\SRV-DC01\Direction` | Refusé : « Vous n'avez pas l'autorisation d'accéder à \\SRV-DC01\Direction. Contactez l'administrateur réseau » | [11-acces-lucas.png](../screenshots/11-acces-lucas.png) |
| Objet ordinateur après la jonction | `CLT-W10-01` dans le conteneur `Computers` | [11-client-computers.png](../screenshots/11-client-computers.png) |
| Objet ordinateur après déplacement | `CLT-W10-01` dans `Entreprise/Ordinateurs` | [11-client-ou-ordinateurs.png](../screenshots/11-client-ou-ordinateurs.png) |
| Instantané du client | `11-client-joint` | [11-snapshot-client.png](../screenshots/11-snapshot-client.png) |
| Instantané du serveur | `11-client-joint-serveur` | [11-snapshot-serveur.png](../screenshots/11-snapshot-serveur.png) |

## Conclusion du test
J'ai constaté que les deux comptes sont authentifiés par le domaine avec le même mot de passe de laboratoire. Claire, membre de `GG_Direction`, accède au dossier et peut y écrire. Lucas, membre de `GG_Comptabilite`, est refusé. La différence vient donc de l'autorisation, c'est-à-dire de l'appartenance aux groupes. Ce résultat correspond au calcul des droits effectifs de l'étape 9.

## Difficultés rencontrées
**Échec de la jonction au domaine.** J'ai obtenu l'erreur « Impossible de contacter un contrôleur de domaine Active Directory » (délai dépassé). Le détail indiquait que le serveur DNS `192.168.10.10` ne répondait pas, pour la requête `_ldap._tcp.dc._msdcs.tssrlab.local`. Preuves : [11-erreur-jonction.png](../screenshots/11-erreur-jonction.png) et [11-erreur-jonction-details.png](../screenshots/11-erreur-jonction-details.png).

Mes constats sur le client avant la correction :
- J'avais saisi l'adresse `192.168.10.10`, celle du serveur, au lieu de `192.168.10.20` ([11-client-ip-erronee.png](../screenshots/11-client-ip-erronee.png)).
- `ipconfig /all` signalait cette adresse « en double » et affichait en plus l'adresse automatique `169.254.47.126` ([11-client-ipconfig-avant.png](../screenshots/11-client-ipconfig-avant.png)).
- `nslookup tssrlab.local` répondait « No response from server », et `ping 192.168.10.10` perdait 4 paquets sur 4 ([11-client-nslookup-ping-echec.png](../screenshots/11-client-nslookup-ping-echec.png)).

J'ai corrigé l'adresse du client en `192.168.10.20`, puis j'ai réussi la jonction.

Leçon : deux machines ne peuvent pas partager la même adresse IP. Avant une jonction, je vérifie l'adresse avec `ipconfig /all`, puis le DNS avec `nslookup`.

## Points d'attention
- Mon test d'accès repose sur deux comptes et un seul dossier partagé. Il ne couvre pas les autres services.
- Les comptes de test partagent un mot de passe de laboratoire. En production, chaque utilisateur aurait le sien.
- Aucune stratégie de groupe (GPO) n'est encore appliquée à l'OU `Ordinateurs`.
- Je n'ai pas de capture des vérifications finales de l'IP et de `nslookup`.

## Prochaine étape
Je finalise le README et je vérifie le dépôt.
