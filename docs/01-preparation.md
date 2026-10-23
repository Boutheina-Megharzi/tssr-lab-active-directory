# Étape 1 : préparation du laboratoire

## Objectif
Vérifier que mon PC peut héberger le laboratoire, puis contrôler l'ISO de Windows Server avant de l'utiliser.

## Machine hôte
| Élément | Valeur | Preuve |
|---|---|---|
| Système | Windows 11 Famille, version 25H2 | [01-hote-windows.png](../screenshots/01-hote-windows.png) |
| Processeur | Intel Core Ultra 5 322, 6 cœurs, 6 processeurs logiques | [01-hote-processeur.png](../screenshots/01-hote-processeur.png) |
| Virtualisation | Activée | [01-hote-processeur.png](../screenshots/01-hote-processeur.png) |
| Mémoire vive | 16 Go DDR5 | [01-hote-memoire.png](../screenshots/01-hote-memoire.png) |
| Disque | SSD NVMe, 327 Go libres sur 474 Go | [02-espace-disque.png](../screenshots/02-espace-disque.png) |

## Outils
| Outil | Détail | Preuve |
|---|---|---|
| VirtualBox | Version 7.2.20 | [01-virtualbox-version.png](../screenshots/01-virtualbox-version.png) |
| ISO du serveur | Windows Server 2019 Standard Evaluation (fr-FR), environ 5,29 Gio | [03-hash-iso.png](../screenshots/03-hash-iso.png) |
| ISO du futur client | Windows 10 22H2, environ 4,6 Gio | [01-iso-telechargees.png](../screenshots/01-iso-telechargees.png) |

## Contrôle de l'ISO du serveur
J'ai calculé l'empreinte SHA256 de l'ISO avec PowerShell, depuis le dossier `C:\tssr-lab-active-directory\iso`.

Commande utilisée :

    Get-FileHash -Path .\*.iso -Algorithm SHA256

- `Get-FileHash` calcule l'empreinte d'un fichier.
- `-Path .\*.iso` désigne les fichiers ISO du dossier courant.
- `-Algorithm SHA256` choisit l'algorithme de calcul.

Empreinte obtenue : `6A2B373321408F8509151B00DAF07BEA4FE7FD7D7C3435415B2D0CC0117A11FF`

Preuve : `03-hash-iso.png`

Statut : empreinte calculée et enregistrée.

## Pourquoi ces vérifications ?
- La virtualisation doit être activée pour que VirtualBox fonctionne.
- La mémoire doit couvrir le PC et les VM : 4 Go prévus pour le serveur et 4 Go pour le client.
- Les disques virtuels occupent de la place : 50 Go au maximum par VM.

## Points d'attention
- Lors de la capture, 11,2 Go de mémoire étaient utilisés et 4,3 Go disponibles. Je dois fermer des applications avant de lancer les deux VM.
- Mon PC est en Windows 11 Famille. Les VM doivent être en édition Pro pour joindre un domaine.


## Difficultés rencontrées
- Erreur PowerShell « chemin introuvable » : il manquait le `\` après `C:`. Le chemin correct est `C:\tssr-lab-active-directory\iso`.

## Bilan
Mon PC dispose des ressources nécessaires pour le laboratoire.
