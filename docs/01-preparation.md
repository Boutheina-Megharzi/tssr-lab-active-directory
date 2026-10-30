# Étape 1 : préparation du laboratoire

## Objectif
Je vérifie que mon PC peut héberger le laboratoire, puis je calcule l'empreinte de l'ISO de Windows Server.

## Réalisation
1. Je relève la version de Windows, le processeur, la virtualisation et la mémoire de mon PC.
2. Je note la version de VirtualBox.
3. Je télécharge les deux ISO : Windows Server 2019 et Windows 10.
4. Je calcule l'empreinte SHA256 de l'ISO du serveur avec PowerShell, depuis `C:\tssr-lab-active-directory\iso`.

## Résultats
| Élément | Valeur | Preuve |
|---|---|---|
| Système | Windows 11 Famille, version 25H2 | [01-windows-hote.png](../screenshots/01-windows-hote.png) |
| Processeur | Intel Core Ultra 5 322, 6 cœurs, 6 processeurs logiques | [01-processeur-pc.png](../screenshots/01-processeur-pc.png) |
| Virtualisation | Activée | [01-processeur-pc.png](../screenshots/01-processeur-pc.png) |
| Mémoire | 11,2 Go utilisés, 4,3 Go disponibles | [01-hote-memoire.png](../screenshots/01-hote-memoire.png) |
| VirtualBox | Version 7.2.20 | [01-virtualbox-version.png](../screenshots/01-virtualbox-version.png) |
| ISO du serveur | Windows Server 2019 Standard Evaluation (fr-FR), environ 5,29 Gio | [03-hash-iso.png](../screenshots/03-hash-iso.png) |
| ISO du client | Windows 10 22H2, environ 4,6 Gio | [01-iso-telechargees.png](../screenshots/01-iso-telechargees.png) |

## Calcul de l'empreinte de l'ISO du serveur
Je lance cette commande :

```
Get-FileHash -Path .\*.iso -Algorithm SHA256
```

- `Get-FileHash` calcule l'empreinte d'un fichier.
- `-Path .\*.iso` désigne les fichiers ISO du dossier courant.
- `-Algorithm SHA256` choisit l'algorithme de calcul.

Empreinte obtenue : `6A2B373321408F8509151B00DAF07BEA4FE7FD7D7C3435415B2D0CC0117A11FF`

Preuve : [03-hash-iso.png](../screenshots/03-hash-iso.png)

Je n'ai pas comparé cette empreinte à une valeur de référence. Je la note pour pouvoir la comparer plus tard.

## Pourquoi ces vérifications ?
- La virtualisation doit être activée pour que VirtualBox fonctionne.
- La mémoire doit couvrir le PC et les VM : j'ai prévu 4 Go pour le serveur et 4 Go pour le client.
- Les disques virtuels occupent de la place : 50 Go au maximum par VM.

## Points d'attention
- Lors de la capture, 11,2 Go de mémoire étaient utilisés et 4,3 Go disponibles. Je ferme des applications avant de lancer les deux VM.
- Mon PC est en Windows 11 Famille. Cette édition n'a pas d'effet sur les VM, mais le **client** doit être en Windows 10 **Pro** pour joindre un domaine.

## Difficultés rencontrées
- J'ai obtenu l'erreur « chemin introuvable » dans PowerShell. Il manquait le `\` après `C:` dans `Set-Location`, donc PowerShell est resté dans `C:\Windows\system32\`. J'ai écrit le chemin complet `C:\tssr-lab-active-directory\iso`, et la commande a fonctionné. Preuve : [01-erreur-chemin-powershell.png](../screenshots/01-erreur-chemin-powershell.png)

## Bilan
Mon PC dispose des ressources nécessaires pour le laboratoire.

## Prochaine étape
Je crée la VM du serveur.
