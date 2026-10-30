# Étape 9 : affectation aux groupes et dossier partagé

## Objectif
J'ajoute chaque utilisateur au groupe de son service, puis je teste les permissions d'un groupe sur un dossier partagé.

## Notions utilisées
- **Membre d'un groupe** : l'utilisateur reçoit les permissions données au groupe.
- **Permissions de partage et NTFS** : sur le réseau, la plus restrictive des deux s'applique.
- **Héritage** : un dossier reprend par défaut les droits de son dossier parent.
- **Accès effectif** : calcul des droits d'après les groupes. Ce n'est pas un accès réel depuis un poste client.

## Pourquoi je donne les permissions à un groupe
Je donne chaque permission une seule fois, au groupe. J'ajoute un compte au groupe quand une personne arrive. Je retire ce compte quand elle part. Je gagne du temps et je limite les erreurs.

## Affectation réalisée
| Groupe | Membres |
|---|---|
| `GG_Direction` | Claire Martin, Paul Durand |
| `GG_Comptabilite` | Sonia Bernard, Lucas Petit |
| `GG_Informatique` | Amine Roux, Julie Moreau |
| `GG_Marketing` | Hugo Lambert, Emma Girard |

## Réalisation
1. J'ouvre les propriétés du groupe, onglet **Membres**, bouton **Ajouter**.
2. Je saisis les identifiants séparés par un point-virgule, puis je clique sur **Vérifier les noms**.
3. Je fais la même opération pour les quatre groupes.
4. Je crée le dossier `C:\Partages\Direction` et je le partage sous `\\SRV-DC01\Direction`.
5. Je désactive l'héritage NTFS, je retire les entrées « Utilisateurs » et j'ajoute `GG_Direction` avec la permission Modification.
6. Je relève les droits effectifs de deux comptes.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Saisie et vérification des noms | `claire.martin` et `paul.durand` reconnus | [09-saisie-membres.png](../screenshots/09-saisie-membres.png) |
| Membres de `GG_Direction` | Claire Martin, Paul Durand | [09-membres-gg-direction.png](../screenshots/09-membres-gg-direction.png) |
| Membres de `GG_Comptabilite` | Lucas Petit, Sonia Bernard | [09-membres-gg-comptabilite.png](../screenshots/09-membres-gg-comptabilite.png) |
| Membres de `GG_Informatique` | Amine Roux, Julie Moreau | [09-membres-gg-informatique.png](../screenshots/09-membres-gg-informatique.png) |
| Membres de `GG_Marketing` | Emma Girard, Hugo Lambert | [09-membres-gg-marketing.png](../screenshots/09-membres-gg-marketing.png) |
| Partage du dossier `Direction` | État « Partagé », chemin réseau `\\SRV-DC01\Direction` | [09-partage-direction.png](../screenshots/09-partage-direction.png) |
| Permissions NTFS d'origine | Héritage de `C:\` actif. Système, Administrateurs et Créateur propriétaire : Contrôle total. Utilisateurs : Lecture et exécution, plus une entrée Spéciale | [09-ntfs-liste-avant.png](../screenshots/09-ntfs-liste-avant.png) |
| Liste NTFS finale | Héritage désactivé. Système et Administrateurs : Contrôle total. Créateur propriétaire : Contrôle total sur les sous-dossiers et les fichiers. `GG_Direction` : Modification. Entrées « Utilisateurs » retirées | [09-ntfs-liste-finale.png](../screenshots/09-ntfs-liste-finale.png) |
| Droits effectifs de `claire.martin` | Droits de lecture, de création et de suppression accordés. Contrôle total, suppression de sous-dossier et fichier, et modification des autorisations refusés (limités par les autorisations de fichiers) | [09-acces-effectif-claire.png](../screenshots/09-acces-effectif-claire.png) |
| Droits effectifs de `lucas.petit` | Aucun droit accordé sur les 13 lignes visibles, toutes limitées par les autorisations de fichiers | [09-acces-effectif-lucas.png](../screenshots/09-acces-effectif-lucas.png) |
| Instantané | `09-permissions-ntfs` | [09-snapshot.png](../screenshots/09-snapshot.png) |

## Conclusion du test
J'ai constaté que le calcul des droits effectifs donne des résultats différents pour deux comptes du même domaine : `claire.martin`, membre de `GG_Direction`, obtient des droits de modification sur le dossier, alors que `lucas.petit`, membre de `GG_Comptabilite`, n'obtient aucun droit. Le calcul se base sur les groupes et ne tient pas compte des permissions de partage. J'ai ensuite fait le test d'accès réel depuis le client Windows 10 joint au domaine : voir l'[étape 11](11-jonction-domaine.md).

## Difficultés rencontrées
- J'ai partagé par erreur le dossier parent `C:\Partages` sous le nom `Direction`. Le chemin réseau affiché m'a révélé l'erreur. J'ai supprimé le partage du dossier parent, puis j'ai partagé le bon dossier. Preuves : [09-partage-erreur-partages.png](../screenshots/09-partage-erreur-partages.png) et [09-partage-corrige.png](../screenshots/09-partage-corrige.png)
- J'ai ouvert par erreur une fenêtre qui montrait les droits de l'OU `Direction` dans Active Directory, et non ceux du dossier. Les onglets et les groupes affichés (Admins du domaine, SELF) me l'ont révélé. J'ai revérifié l'état de l'OU : héritage actif, bouton Appliquer grisé. Je n'ai pas conservé de capture de cette fenêtre.

## Points d'attention
- Chaque groupe contient exactement deux membres, issus de l'OU du même service.
- Les mots de passe des comptes de test ne figurent pas dans ce dépôt.

## Prochaine étape
Je crée le poste client Windows 10 Pro, puis je teste l'accès réel au dossier partagé.
