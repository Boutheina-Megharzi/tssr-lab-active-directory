# Étape 9 : affectation des utilisateurs aux groupes

## Objectif
Ajouter chaque utilisateur au groupe de sécurité de son service, puis vérifier l'appartenance. Les groupes étaient vides à la fin de l'étape 8.

## Notions utilisées
- **Membre d'un groupe** : l'utilisateur reçoit les permissions données au groupe.
- **Utilisateurs du domaine** : groupe principal dans lequel tout compte du domaine se trouve déjà.
- **Délai d'effet** : le serveur lit les groupes d'un utilisateur à l'ouverture de session. Un utilisateur déjà connecté doit se déconnecter, puis se reconnecter.

**Pourquoi je donne les permissions à un groupe :** Je donne chaque permission une seule fois, au groupe. J'ajoute un compte au groupe quand une personne arrive. Je retire ce compte quand elle part. Je gagne du temps, je limite les erreurs et je garde une vue claire des droits.

## Affectation réalisée
| Groupe | Membres | OU d'origine des membres |
|---|---|---|
| `GG_Direction` | Claire Martin, Paul Durand | Direction |
| `GG_Comptabilite` | Sonia Bernard, Lucas Petit | Comptabilité |
| `GG_Informatique` | Amine Roux, Julie Moreau | Informatique |
| `GG_Marketing` | Hugo Lambert, Emma Girard | Marketing |

## Réalisation
1. Ouverture des propriétés du groupe, depuis l'OU `Groupes`.
2. Onglet Membres, puis bouton Ajouter.
3. Saisie des deux identifiants séparés par un point-virgule, puis Vérifier les noms.
4. Validation avec OK, deux fois.
5. Même opération pour chacun des quatre groupes.
6. Vérification des membres de chaque groupe et de l'onglet Membre de de deux utilisateurs.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Propriétés de `GG_Direction` | Étendue globale, type sécurité | [09-groupe-proprietes-general.png](../screenshots/09-groupe-proprietes-general.png) |
| Membres avant l'ajout | Liste vide | [09-membres-gg-direction-avant.png](../screenshots/09-membres-gg-direction-avant.png) |
| Saisie des identifiants | `claire.martin;paul.durand` | [09-saisie-membres.png](../screenshots/09-saisie-membres.png) |
| Vérification des noms | Claire Martin (`claire.martin@tssrlab.local`) et Paul Durand (`paul.durand@tssrlab.local`) reconnus | [09-verification-noms.png](../screenshots/09-verification-noms.png) |
| Membres de `GG_Direction` | Claire Martin et Paul Durand, dans `tssrlab.local/Entreprise/Direction` | [09-membres-gg-direction.png](../screenshots/09-membres-gg-direction.png) |
| Membres de `GG_Comptabilite` | Lucas Petit et Sonia Bernard, dans `tssrlab.local/Entreprise/Comptabilité` | [09-membres-gg-comptabilite.png](../screenshots/09-membres-gg-comptabilite.png) |
| Membres de `GG_Informatique` | Amine Roux et Julie Moreau, dans `tssrlab.local/Entreprise/Informatique` | [09-membres-gg-informatique.png](../screenshots/09-membres-gg-informatique.png) |
| Membres de `GG_Marketing` | Emma Girard et Hugo Lambert, dans `tssrlab.local/Entreprise/Marketing` | [09-membres-gg-marketing.png](../screenshots/09-membres-gg-marketing.png) |
| Onglet Membre de de Claire Martin | Utilisateurs du domaine et `GG_Direction` | [09-membre-de-claire-martin.png](../screenshots/09-membre-de-claire-martin.png) |
| Onglet Membre de de Lucas Petit | Utilisateurs du domaine et `GG_Comptabilite` | [09-membre-de-lucas-petit.png](../screenshots/09-membre-de-lucas-petit.png) |
| Instantané | `09-groupes-affectes` | [09-snapshot.png](../screenshots/09-snapshot.png) |

## Points d'attention
- Chaque groupe contient exactement deux membres, tous issus de l'OU du même service.
- Les groupes ne donnent aucune permission pour l'instant. Aucun dossier partagé ni stratégie de groupe n'existe encore.
- Les mots de passe des comptes de test ne figurent pas dans ce dépôt.

## Difficultés rencontrées
Aucune difficulté technique.

## Prochaine étape
Créer la machine virtuelle du poste client Windows 10 Pro.
