# Étape 9 : affectation aux groupes et dossier partagé

## Objectif
Ajouter chaque utilisateur au groupe de son service, puis préparer un dossier partagé pour tester les permissions d'un groupe.

## Notions utilisées
- **Membre d'un groupe** : l'utilisateur reçoit les permissions données au groupe.
- **Permissions de partage et NTFS** : sur le réseau, la plus restrictive des deux s'applique.
- **Héritage** : un dossier reprend par défaut les droits de son dossier parent.
- **Accès effectif** : calcul des droits d'après les groupes. Ce n'est pas un accès réel depuis un poste client.

**Pourquoi je donne les permissions à un groupe :** Je donne chaque permission une seule fois, au groupe. J'ajoute un compte au groupe quand une personne arrive. Je retire ce compte quand elle part. Je gagne du temps et je limite les erreurs.

## Affectation réalisée
| Groupe | Membres |
|---|---|
| `GG_Direction` | Claire Martin, Paul Durand |
| `GG_Comptabilite` | Sonia Bernard, Lucas Petit |
| `GG_Informatique` | Amine Roux, Julie Moreau |
| `GG_Marketing` | Hugo Lambert, Emma Girard |

## Réalisation
1. Propriétés du groupe, onglet Membres, bouton Ajouter.
2. Saisie des identifiants séparés par un point-virgule, puis Vérifier les noms.
3. Même opération pour les quatre groupes.
4. Création du dossier `C:\Partages\Direction` et partage sous `\\SRV-DC01\Direction`.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Membres de `GG_Direction` | Claire Martin, Paul Durand | [09-membres-gg-direction.png](../screenshots/09-membres-gg-direction.png) |
| `GG_Comptabilite` et `GG_Informatique` | Deux membres chacun, vérifiés de la même façon, captures non publiées | |
| Partage du dossier `Direction` | État « Partagé », chemin réseau `\\SRV-DC01\Direction` | [09-partage-direction.png](../screenshots/09-partage-direction.png) |
| Permissions NTFS d'origine | Héritage de `C:\` actif. Système, Administrateurs et Créateur propriétaire : Contrôle total. Utilisateurs : Lecture et exécution, plus une entrée Spéciale | [09-ntfs-liste-avant.png](../screenshots/09-ntfs-liste-avant.png) |
| Liste NTFS finale | Héritage désactivé. Système et Administrateurs : Contrôle total. Créateur propriétaire : Contrôle total sur les sous-dossiers et les fichiers. `GG_Direction` : Modification. Entrées « Utilisateurs » retirées | [09-ntfs-liste-finale.png](../screenshots/09-ntfs-liste-finale.png) |
| Droits effectifs de `claire.martin` | Droits de lecture, de création et de suppression accordés. Contrôle total, suppression de sous-dossier et fichier, et modification des autorisations refusés (limités par les autorisations de fichiers) | [09-acces-effectif-claire.png](../screenshots/09-acces-effectif-claire.png) |
| Droits effectifs de `lucas.petit` | Aucun droit accordé sur les 13 lignes visibles, toutes limitées par les autorisations de fichiers | [09-acces-effectif-lucas.png](../screenshots/09-acces-effectif-lucas.png) |

## Reste à faire
Retirer l'héritage, restreindre le dossier à `GG_Direction` (Modification), puis relever les droits effectifs de `claire.martin` et de `lucas.petit`. Le test d'accès réel se fera depuis le client Windows 10.

## Difficultés rencontrées
- Le dossier parent `C:\Partages` a été partagé par erreur sous le nom `Direction`. Le chemin réseau affiché a révélé l'erreur. Preuve : [09-partage-erreur-partages.png](../screenshots/09-partage-erreur-partages.png)
- Une première fenêtre ouverte par erreur montrait les droits de l'OU `Direction` dans Active Directory. Les onglets et les groupes affichés (Admins du domaine, SELF) l'ont révélé. L'état de l'OU a été revérifié : héritage actif, bouton Appliquer grisé.

## Points d'attention
- Chaque groupe contient exactement deux membres, issus de l'OU du même service.
- Les mots de passe des comptes de test ne figurent pas dans ce dépôt.

## Prochaine étape
Terminer le test des permissions, puis créer le poste client Windows 10 Pro.
