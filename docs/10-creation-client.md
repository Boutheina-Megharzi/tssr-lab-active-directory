# Étape 10 : création du poste client Windows 10 Pro

## Objectif
Installer un poste client Windows 10 Pro dans une machine virtuelle, sur le réseau interne `LAB-AD`, pour le joindre au domaine `tssrlab.local` à l'étape suivante.

## Notions utilisées
- **Poste client et serveur** : le client est l'ordinateur d'un employé. Il applique les règles du domaine et ne l'héberge pas.
- **Édition Pro** : seules les éditions Pro, Entreprise et Éducation peuvent joindre un domaine. L'ISO contient plusieurs éditions, donc je choisis Pro pendant l'installation.
- **Compte local** : compte qui existe seulement sur la machine. Je l'utilise pour installer et dépanner le client avant qu'il rejoigne le domaine.
- **Ordre d'amorçage** : liste des supports sur lesquels la VM essaie de démarrer. Le disque dur doit passer avant le lecteur optique, sinon un appui sur une touche relance l'installateur.

**Pourquoi je crée un compte local d'abord :** J'installe et je teste le client avant de dépendre du serveur. Si la jonction échoue, je garde un compte pour ouvrir la session et dépanner.

## Paramètres de la machine virtuelle
| Paramètre | Valeur | Preuve |
|---|---|---|
| Nom | `CLT-W10-01` | [10-config-vm-client.png](../screenshots/10-config-vm-client.png) |
| Système | Windows 10 (64-bit) | [10-config-vm-client.png](../screenshots/10-config-vm-client.png) |
| Mémoire | 4096 Mo | [10-config-vm-client.png](../screenshots/10-config-vm-client.png) |
| Processeurs | 1 | [10-processeur-client.png](../screenshots/10-processeur-client.png) |
| Disque | 50 Gio (dynamique) | [10-config-vm-client.png](../screenshots/10-config-vm-client.png) |
| ISO | `Win10_22H2_neuve.iso` (4,58 Gio) | [10-config-vm-client.png](../screenshots/10-config-vm-client.png) |
| Réseau | Réseau interne `LAB-AD` | [10-config-vm-client.png](../screenshots/10-config-vm-client.png) |

Les paramètres reprennent ceux du serveur, qui fonctionne avec 1 processeur et 4 Go. L'installation assistée de VirtualBox reste désactivée, pour voir chaque écran.

## Réalisation
1. Arrêt du serveur, fermeture des applications inutiles et redémarrage du PC pour libérer de la mémoire.
2. Création de la machine virtuelle `CLT-W10-01` avec les paramètres ci-dessus.
3. Instantané `10-vm-creee`, avant l'installation.
4. Installation de Windows 10 Pro, en français, région France, sans clavier supplémentaire.
5. Configuration initiale sans Internet (option « Je n'ai pas Internet »), puis création du compte local `adminlocal`.
6. Vérification de l'édition avec `winver`.
7. Arrêt de la VM, puis instantané `10-windows10-installe`.

Le mot de passe du compte local n'est pas publié dans ce dépôt.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Édition (`winver`) | Windows 10 Professionnel, version 22H2, build 19045.3803 | [10-winver.png](../screenshots/10-winver.png) |
| Écran de connexion | Le compte local `adminlocal` s'affiche, champ du mot de passe vide | [10-ecran-connexion.png](../screenshots/10-ecran-connexion.png) |
| Ordre d'amorçage au départ | Disquette, Optique, Disque dur | [10-ordre-amorcage-avant.png](../screenshots/10-ordre-amorcage-avant.png) |
| Ordre d'amorçage corrigé | Disque dur, Optique, Disquette, Réseau décoché | [10-ordre-amorcage-apres.png](../screenshots/10-ordre-amorcage-apres.png) |
| Instantanés | `10-vm-creee` (28/10/2026 à 09:49) et `10-windows10-installe` (10:14) | [10-snapshot.png](../screenshots/10-snapshot.png) |

L'édition a été vérifiée avec `winver` après l'installation. L'écran de choix de l'édition n'a pas été capturé. L'écran de connexion prouve que Windows démarre sur le disque et que le compte local existe. Il ne prouve pas à lui seul le moment où l'ordre d'amorçage a été corrigé.

## Difficultés rencontrées
- **Mémoire insuffisante au départ** : le Gestionnaire des tâches indiquait 5,4 Go disponibles, trop peu pour une VM de 4 Go. Un redémarrage du PC a ramené la mémoire disponible à 8,4 Go. Supprimer des fichiers ne libère pas de mémoire vive.
- **Redémarrage sur l'installateur** : au redémarrage, un appui sur une touche a relancé l'installateur de Windows. Cause probable : l'ordre d'amorçage plaçait le lecteur optique avant le disque dur (hypothèse). L'installation a ensuite abouti jusqu'au bureau, et l'ordre a été corrigé. Preuves : [10-ordre-amorcage-avant.png](../screenshots/10-ordre-amorcage-avant.png) et [10-ordre-amorcage-apres.png](../screenshots/10-ordre-amorcage-apres.png)
- **Ancien échec d'installation** : une première tentative avait échoué avec l'erreur « L'ordinateur a redémarré de manière inattendue ». Elle n'a pas été analysée à l'époque, et la machine concernée a été supprimée. L'installation a réussi avec 1 processeur et 4 Go de mémoire. La cause de l'ancien échec reste inconnue.

## Points d'attention
- Windows 10 n'est plus supporté par Microsoft depuis octobre 2025. Le client reste dans un réseau interne isolé, sans accès à Internet. Une mise à jour vers Windows 11 Pro pourrait être une amélioration.
- Deux machines virtuelles de 4 Go réservent 8 Go sur les 16 Go du PC. Pour l'étape suivante, la mémoire disponible sera surveillée.
- Le client n'est pas encore joint au domaine : l'adresse IP, le nom d'ordinateur et le DNS seront configurés à l'étape 11.

## Prochaine étape
Configurer l'adresse IP et le DNS du client, le renommer, le joindre au domaine `tssrlab.local`, puis ouvrir une session avec un compte du domaine et tester l'accès au dossier partagé.
