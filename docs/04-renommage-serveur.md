# Étape 4 : renommage du serveur

## Objectif
Je donne au serveur le nom `SRV-DC01` avant d'installer Active Directory. Renommer un contrôleur de domaine après sa promotion est délicat, donc je renomme maintenant, quand le serveur est encore vide.

## Notions utilisées
- **Nom de la VM** : le nom affiché dans VirtualBox. Windows l'ignore.
- **Nom d'ordinateur** : le nom utilisé par Windows et par le réseau. C'est lui que je change.
- **Groupe de travail (`WORKGROUP`)** : mode sans domaine. Le serveur y reste jusqu'à sa promotion en contrôleur de domaine.
- **Convention de nommage** : `SRV` pour serveur, `DC` pour contrôleur de domaine, `01` pour le numéro. Le nom respecte les règles de Windows : 15 caractères maximum, avec des lettres, des chiffres et des tirets.

## Réalisation
1. Je démarre le serveur et j'ouvre la session Administrateur.
2. Je relève le nom actuel dans Gestionnaire de serveur > Serveur local : `WIN-FTJ32S5CIVQ`.
3. Je modifie le nom de l'ordinateur en `SRV-DC01`, en laissant le groupe de travail `WORKGROUP`.
4. Je redémarre le serveur.
5. Je vérifie le résultat après le redémarrage.

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
| Adresse IPv4 | 192.168.10.10, masque 255.255.255.0, pas de passerelle | [04-hostname.png](../screenshots/04-hostname.png) |
| DHCP activé | Non (IP fixe conservée) | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |
| Serveur DNS | 192.168.10.10 | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |
| Adresse MAC | `08-00-27-9F-CB-13`, identique à celle de l'étape 3 | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |
| Suffixe DNS principal | Vide (normal : pas encore de domaine) | [04-ipconfig-all.png](../screenshots/04-ipconfig-all.png) |
| Instantané | `04-serveur-renomme` | [04-snapshot.png](../screenshots/04-snapshot.png) |

Le renommage n'a modifié ni l'adresse IP, ni le DNS, ni la carte réseau.

## Points d'attention
- J'ai noté deux avertissements dans le Gestionnaire de serveur : `Microsoft-Windows-Time-Service` ID 134 et `DNS Client Events` ID 1014. Je n'ai pas de capture de ces avertissements et je n'ai pas agi dessus.

## Prochaine étape
J'installe le rôle AD DS (étape 5), puis je promeus le serveur en contrôleur de domaine (étape 6). Le suffixe DNS se remplit alors avec le nom du domaine.
