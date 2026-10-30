# Étape 2 : création de la VM du serveur

## Objectif
Je crée la machine virtuelle `SRV-DC01`, qui deviendra le contrôleur de domaine du laboratoire. Je prépare le matériel virtuel ; je n'installe pas encore Windows Server.

## Réalisation
1. Je nomme la VM `SRV-DC01` et je sélectionne l'ISO de Windows Server 2019.
2. Je laisse décochée la case « Proceed with Unattended Installation », pour installer Windows manuellement.
3. Je règle la mémoire à 4096 Mo.
4. Je règle le processeur à 1.
5. Je crée un disque de 50 Go en allocation dynamique.

## Caractéristiques de la VM
| Élément | Valeur | Pourquoi |
|---|---|---|
| Nom de la VM | `SRV-DC01` | Convention : serveur, contrôleur de domaine, numéro 01 |
| Système | Windows Server 2019 Standard Evaluation (fr-FR) | Version d'évaluation gratuite |
| Mémoire | 4096 Mo | Suffisant pour AD DS en laboratoire |
| Processeurs | 1 | Avec 2 processeurs, l'installation gelait ; avec 1, elle a abouti |
| Disque | 50 Go (allocation dynamique) | Ne prend que l'espace réellement utilisé |
| Réseau | NAT à la création, puis réseau interne `LAB-AD` à l'étape 3 | Le NAT est provisoire ; le réseau interne isole le laboratoire |

Le nom de la VM dans VirtualBox est distinct du futur nom d'ordinateur Windows, que je configure à l'étape 4.

## Captures d'écran
Fenêtre de création de la VM :

![Création de la VM](../screenshots/04-nouvelle-vm-nom-type.png)

Configuration de la VM créée :

![Configuration de la VM](../screenshots/07-vm-creee-configuration.png)

## Difficultés rencontrées
- J'ai remarqué que la case « Proceed with Unattended Installation » porte un libellé différent selon les versions de VirtualBox. Dans la version 7.2.20, je la laisse décochée pour installer Windows manuellement.
- J'ai constaté que les sections « Specify virtual hardware » et « Specify virtual hard disk » sont repliées par défaut. Le disque a donc gardé la taille proposée (50 Go) au lieu des 60 Go prévus, ce qui reste suffisant pour le laboratoire.
- J'ai observé un gel de l'installation avec 2 processeurs. Je suis passé à 1 processeur, et l'installation a abouti.
- J'ai noté que VirtualBox affiche une tortue verte, signe du mode native API.

## Prochaine étape
Je configure le réseau interne `LAB-AD` et l'adresse IP statique.
