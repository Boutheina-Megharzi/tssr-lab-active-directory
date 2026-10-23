# Étape 3 : réseau isolé et adresses IP

## Objectif
Placer le serveur dans un réseau privé isolé, avec une adresse IP fixe. Un contrôleur de domaine doit toujours garder la même adresse, car les postes clients s'en servent pour le retrouver.

## Notions utilisées
- **Réseau interne** : switch virtuel privé de VirtualBox. Seules les VM qui portent le même nom de réseau communiquent entre elles.
- **Adresse IP statique** : adresse saisie à la main, qui ne change jamais.
- **DNS** : service qui traduit un nom en adresse IP. Active Directory en dépend.
- **DHCP** : service qui distribue les adresses automatiquement. Il n'y en a pas dans ce laboratoire.

## Plan d'adressage
| Machine | Adresse IP | Masque | Passerelle | DNS | État |
|---|---|---|---|---|---|
| SRV-DC01 | 192.168.10.10 | 255.255.255.0 | aucune | 192.168.10.10 | Configuré |
| Client Windows 10 | 192.168.10.20 | 255.255.255.0 | aucune | 192.168.10.10 | Prévu, pas encore créé |

La passerelle reste vide : le réseau est isolé, il n'y a aucun routeur. Le serveur sert de DNS à lui-même et servira de DNS au client.

## Réalisation
1. Serveur éteint, passage de l'adaptateur 1 du mode NAT au mode **Réseau interne**, nom `LAB-AD`.
2. Démarrage du serveur, puis configuration d'IPv4 : adresse, masque, DNS. La passerelle est laissée vide.
3. Vérification des détails de la carte et test avec `ping`.
4. Prise d'un instantané `03-reseau-ip-fixe-ok`.

Impact : le serveur n'a plus accès à Internet. Ce n'est pas nécessaire pour installer AD DS.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Mode réseau de la carte | Réseau interne `LAB-AD` | [03-virtualbox-reseau-interne.png](../screenshots/03-virtualbox-reseau-interne.png) |
| Saisie de l'IP, du masque et du DNS | 192.168.10.10 / 255.255.255.0 / 192.168.10.10 | [03-ip-saisie.png](../screenshots/03-ip-saisie.png) |
| Détails de la carte | DHCP activé : Non, IP et DNS conformes | [03-ip-details.png](../screenshots/03-ip-details.png) |
| `ping 192.168.10.10` | 4 envoyés, 4 reçus, 0 % de perte | [03-ping-serveur.png](../screenshots/03-ping-serveur.png) |
| Instantané | `03-reseau-ip-fixe-ok` | [03-snapshot.png](../screenshots/03-snapshot.png) |

L'adresse MAC `08-00-27-9F-CB-13` est identique dans VirtualBox et dans Windows : c'est bien la même carte.

## Limites
- Le ping vers sa propre adresse prouve que la configuration du serveur est correcte. Il ne prouve pas que deux machines se voient. Ce test sera fait quand le client sera créé.
- Le message « Réseau non identifié » est normal : il n'y a ni passerelle ni domaine pour l'instant.

## Difficultés rencontrées
- Le snapshot n'avait pas été pris avant le changement de réseau. Il a été pris ensuite, avec un nom qui décrit l'état réel (`03-reseau-ip-fixe-ok`) au lieu de « avant réseau ».

## Prochaine étape
Renommer le serveur, puis installer le rôle AD DS.
