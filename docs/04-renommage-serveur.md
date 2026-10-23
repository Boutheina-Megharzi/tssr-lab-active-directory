# Étape 4 : renommage du serveur

## Objectif
Donner au serveur le nom `SRV-DC01` avant d'installer Active Directory. Renommer un contrôleur de domaine après sa promotion est délicat, donc le renommage se fait maintenant, quand le serveur est encore vide.

## Notions utilisées
- **Nom de la VM** : le nom affiché dans VirtualBox. Windows l'ignore.
- **Nom d'ordinateur** : le nom utilisé par Windows et par le réseau. C'est lui que l'on change.
- **Groupe de travail (`WORKGROUP`)** : mode sans domaine. Le serveur y reste jusqu'à sa promotion en contrôleur de domaine.
- **Convention de nommage** : `SRV` pour serveur, `DC` pour contrôleur de domaine, `01` pour le numéro. Le nom respecte les règles de Windows : 15 caractères maximum, avec des lettres, des chiffres et des tirets.

## Réalisation
1. Démarrage du serveur et ouverture de la session Administrateur.
2. Relevé du nom actuel dans Gestionnaire de serveur > Serveur local : `WIN-FTJ32S5CIVQ`.
3. Modification du nom de l'ordinateur en `SRV-DC01`, le groupe de travail restant `WORKGROUP`.
4. Redémarrage du serveur.
5. Vérifications après redémarrage.

Impact : le serveur redémarre. Aucune donnée n'est en jeu, car il est encore vide.

## Commandes utilisées
| Commande | Rôle |
|---|---|
| `hostname` | Affiche le nom de l'ordinateur |
| `ipconfig` | Affiche l'adresse IP, le masque et la passerelle |
| `ipconfig /all` | Affiche aussi l'adresse MAC, l'état du DHCP et le serveur DNS |

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Nom avant renommage | `WIN-FTJ32S5CIVQ` | [04-nom-avant.png](../screenshots/04-nom-avant.png) |
| Nom après renommage | `SRV-DC01` | [04-nom-apres.png](../screenshots/04-nom-apres.png) |
| `hostname` | `SRV-DC01` | [04-hostname.png](../screenshots/04-hostname.png) |
| Adresse IPv4 | `192.168.10.10`, masque `255.255.255.0`, pas de passerelle | [04-hostname.png](../screenshots/04-hostname.png) |
| DHCP activé | Non (IP fixe conservée) | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |
| Serveur DNS | `192.168.10.10` | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |
| Adresse MAC | `08-00-27-9F-CB-13`, identique à celle de l'étape 3 | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |
| Suffixe DNS principal | Vide (normal : pas encore de domaine) | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |

Le renommage n'a modifié ni l'adresse IP, ni le DNS, ni la carte réseau.

## Points d'attention
- Le Gestionnaire de serveur affiche des avertissements (Microsoft-Windows-Time-Service ID 134 et DNS Client Events ID 1014). Ils sont probablement liés à l'absence d'Internet dans ce réseau isolé. Cette hypothèse reste à confirmer.
- Moteur d'exécution de VirtualBox : `native API`, relevé à l'écran sans capture. Il explique probablement les lenteurs et l'instabilité avec plusieurs processeurs, mais ce lien n'est pas démontré.

## Prochaine étape
Installer le rôle AD DS et promouvoir le serveur en contrôleur de domaine. Le suffixe DNS sera alors rempli avec le nom du domaine.
