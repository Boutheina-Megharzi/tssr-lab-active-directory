# Étape 6 : promotion en contrôleur de domaine

## Objectif
Créer le domaine `tssrlab.local` et faire de `SRV-DC01` son contrôleur de domaine, avec le DNS intégré.

## Choix du nom de domaine
Le domaine s'appelle `tssrlab.local`, avec le nom NetBIOS `TSSRLAB`. Le suffixe `.local` est courant en formation. Il est déconseillé en production, car il peut entrer en conflit avec d'autres services réseau qui utilisent aussi `.local`. Ce choix convient à un laboratoire isolé.

## Notions utilisées
- **Forêt et domaine** : une forêt regroupe un ou plusieurs domaines. Ici, une seule forêt contient un seul domaine.
- **Nom NetBIOS** : nom court du domaine (`TSSRLAB`), utilisé à la connexion sous la forme `TSSRLAB\Administrateur`.
- **Contrôleur de domaine** : serveur qui vérifie les identités et héberge l'annuaire.
- **Catalogue global** : index de l'annuaire nécessaire à la connexion des utilisateurs.
- **DSRM** : mode de restauration des services d'annuaire, protégé par son propre mot de passe.
- **Délégation DNS** : lien entre une zone DNS et sa zone parente. Elle n'est pas possible ici, car aucune zone parente n'existe dans ce réseau isolé.
- **Pourquoi le DNS accompagne Active Directory** : [réécris ici avec tes mots]

## Réalisation
1. Ouverture de l'assistant depuis le drapeau du Gestionnaire de serveur.
2. Choix de « Ajouter une nouvelle forêt », avec le nom `tssrlab.local`.
3. Réglage des options du contrôleur de domaine et saisie du mot de passe DSRM.
4. Passage des options DNS, du nom NetBIOS, des chemins et du récapitulatif.
5. Vérification de la configuration requise, puis installation.
6. Redémarrage automatique du serveur, puis ouverture de session en `TSSRLAB\Administrateur`.
7. Vérifications et instantané.

Impact : le serveur quitte `WORKGROUP` pour rejoindre le domaine. Le compte Administrateur devient un compte de domaine.

## Paramètres choisis
| Paramètre | Valeur | Preuve |
|---|---|---|
| Opération | Ajouter une nouvelle forêt | [06-deploiement-nouvelle-foret.png](../screenshots/06-deploiement-nouvelle-foret.png) |
| Nom du domaine | `tssrlab.local` | [06-deploiement-nouvelle-foret.png](../screenshots/06-deploiement-nouvelle-foret.png) |
| Niveau fonctionnel (forêt et domaine) | Windows Server 2016 | [06-options-controleur.png](../screenshots/06-options-controleur.png) |
| Serveur DNS | Coché | [06-options-controleur.png](../screenshots/06-options-controleur.png) |
| Catalogue global | Coché | [06-options-controleur.png](../screenshots/06-options-controleur.png) |
| Contrôleur en lecture seule (RODC) | Non | [06-options-controleur.png](../screenshots/06-options-controleur.png) |
| Mot de passe DSRM | Défini, conservé hors GitHub | |
| Délégation DNS | Non créée (zone parente introuvable) | [06-options-dns-delegation.png](../screenshots/06-options-dns-delegation.png) |
| Nom NetBIOS | `TSSRLAB` | [06-netbios.png](../screenshots/06-netbios.png) |
| Récapitulatif | Nouvelle forêt, DNS et catalogue global : oui | [06-examiner-options.png](../screenshots/06-examiner-options.png) |

## Vérification de la configuration requise
L'assistant indique que toutes les vérifications ont donné satisfaction. Il affiche deux avertissements :
- Un paramètre de sécurité par défaut des contrôleurs Windows Server 2019 qui empêche les algorithmes de chiffrement faibles.
- L'impossibilité de créer une délégation DNS, car la zone parente est introuvable.

Aucun des deux ne bloque l'installation. Preuve : [06-verification-prerequis.png](../screenshots/06-verification-prerequis.png)

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Connexion après redémarrage | Compte `TSSRLAB\Administrateur` | [06-ecran-connexion.png](../screenshots/06-ecran-connexion.png) |
| Rôles du serveur | AD DS et DNS ajoutés, « Rôles : 3 », plus d'avertissement sur le drapeau | [06-tableau-de-bord.png](../screenshots/06-tableau-de-bord.png) |
| Domaine du serveur | `tssrlab.local` | [06-serveur-local-domaine.png](../screenshots/06-serveur-local-domaine.png) |
| Suffixe DNS principal | `tssrlab.local` | [06-ipconfig-all.png](../screenshots/06-ipconfig-all.png) |
| IPv4 | `192.168.10.10`, masque `255.255.255.0`, DHCP : Non | [06-ipconfig-all.png](../screenshots/06-ipconfig-all.png) |
| `nslookup tssrlab.local` | Réponse `192.168.10.10` | [06-nslookup.png](../screenshots/06-nslookup.png) |
| Zones DNS | `tssrlab.local` et `_msdcs.tssrlab.local`, intégrées à Active Directory | [06-zones-dns.png](../screenshots/06-zones-dns.png) |
| Console Active Directory | Domaine `tssrlab.local` affiché, sans la croix rouge de l'étape 5 | [06-console-ad.png](../screenshots/06-console-ad.png) |
| Instantané | `06-controleur-domaine-ok` | [06-snapshot.png](../screenshots/06-snapshot.png) |

## Observations
- Après la promotion, `ipconfig /all` affiche les serveurs DNS `::1` et `127.0.0.1` au lieu de `192.168.10.10`. Le serveur s'interroge lui-même. Les clients utiliseront `192.168.10.10`.
- `nslookup` affiche un délai d'attente sur la première ligne (serveur `UnKnown`, adresse `::1`), mais résout correctement `tssrlab.local`. Cause probable : aucune zone de recherche inversée n'est configurée. Hypothèse non confirmée.
- Le Gestionnaire de serveur affiche encore des avertissements d'événements (DNS Client Events 1014, Winlogon 6006). Cause non analysée à ce stade.

## Difficultés rencontrées
[Écris une difficulté réelle, ou « Aucune ».]

## Prochaine étape
Créer les unités d'organisation, les utilisateurs et les groupes de sécurité.
