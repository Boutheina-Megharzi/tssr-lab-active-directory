# Étape 8 : utilisateurs et groupes de sécurité

## Objectif
Créer les comptes de l'entreprise fictive dans l'OU de leur service, puis les groupes de sécurité. L'affectation des utilisateurs aux groupes fait l'objet de l'étape 9.

## Notions utilisées
- **Compte de domaine** : compte stocké dans l'annuaire, valable sur toutes les machines du domaine. Un compte local n'existe que sur sa machine.
- **Nom d'ouverture de session** : identifiant de connexion, sans accent ni espace (par exemple `claire.martin`). Le nom complet (`Claire Martin`) est celui qui s'affiche.
- **Groupe de sécurité global (`GG_`)** : il rassemble les utilisateurs d'un même service. Le préfixe est une convention de nommage.
- **Authentification** : vérifier qui est la personne (identifiant et mot de passe).
- **Autorisation** : décider ce que la personne a le droit de faire, grâce aux groupes et aux permissions.

**Pourquoi les groupes sont dans l'OU `Groupes` :** un groupe n'appartient pas à un seul service, car il peut réunir des utilisateurs de plusieurs OU. Ranger les groupes dans une OU dédiée et les utilisateurs dans les OU de service facilite l'administration et la délégation.

## Utilisateurs créés
| OU | Nom complet | Identifiant |
|---|---|---|
| Direction | Claire Martin | `claire.martin` |
| Direction | Paul Durand | `paul.durand` |
| Comptabilité | Sonia Bernard | `[à vérifier]` |
| Comptabilité | Lucas Petit | `lucas.petit` |
| Informatique | Amine Roux | `amine.roux` |
| Informatique | Julie Moreau | `julie.moreau` |
| Marketing | Hugo Lambert | `hugo.lambert` |
| Marketing | Emma Girard | `emma.girard` |

Les identifiants de `claire.martin` et `emma.girard` sont lisibles sur les captures. Les autres suivent la même règle `prenom.nom`.

## Groupes créés
| Groupe | Étendue | Type | OU |
|---|---|---|---|
| `GG_Direction` | Globale | Sécurité | Groupes |
| `GG_Comptabilite` | Globale | Sécurité | Groupes |
| `GG_Informatique` | Globale | Sécurité | Groupes |
| `GG_Marketing` | Globale | Sécurité | Groupes |

Les noms de groupes sont écrits sans accent, même si l'OU s'appelle `Comptabilité`.

## Réalisation
1. Clic droit sur l'OU du service, puis Nouveau > Utilisateur.
2. Saisie du prénom, du nom et de l'identifiant, puis du mot de passe de laboratoire.
3. Options de compte : « L'utilisateur doit changer le mot de passe à la prochaine ouverture de session » décochée, « Le mot de passe n'expire jamais » cochée.
4. Création des 8 utilisateurs, chacun dans l'OU de son service.
5. Clic droit sur l'OU `Groupes`, puis Nouveau > Groupe, avec l'étendue Globale et le type Sécurité.
6. Création des 4 groupes, puis vérification de chaque OU.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Menu de création | Clic droit sur l'OU `Direction` | [08-menu-ou-direction.png](../screenshots/08-menu-ou-direction.png) |
| Saisie d'un utilisateur | Claire Martin, identifiant `claire.martin@tssrlab.local`, créée dans `tssrlab.local/Entreprise/Direction` | [08-creation-utilisateur.png](../screenshots/08-creation-utilisateur.png) |
| Récapitulatif | Nom complet `Claire Martin`, connexion `claire.martin@tssrlab.local`, mot de passe sans expiration | [08-recapitulatif-utilisateur.png](../screenshots/08-recapitulatif-utilisateur.png) |
| OU Direction | Claire Martin et Paul Durand | [08-ou-direction.png](../screenshots/08-ou-direction.png) |
| OU Comptabilité | Sonia Bernard et Lucas Petit | [08-ou-comptabilite.png](../screenshots/08-ou-comptabilite.png) |
| OU Informatique | Amine Roux et Julie Moreau | [08-ou-informatique.png](../screenshots/08-ou-informatique.png) |
| OU Marketing | Hugo Lambert (nom tronqué à l'écran) et Emma Girard | [08-ou-marketing.png](../screenshots/08-ou-marketing.png) |
| Menu de création d'un groupe | Clic droit sur l'OU `Groupes` | [08-menu-nouveau-groupe.png](../screenshots/08-menu-nouveau-groupe.png) |
| Création d'un groupe | `GG_Direction`, étendue Globale, type Sécurité, dans `tssrlab.local/Entreprise/Groupes` | [08-creation-groupe.png](../screenshots/08-creation-groupe.png) |
| OU Groupes | Quatre groupes de sécurité | [08-ou-groupes.png](../screenshots/08-ou-groupes.png) |
| Onglet Compte d'Emma Girard | Identifiant `emma.girard@tssrlab.local`, ancien nom `TSSRLAB\emma.girard`, mot de passe sans expiration, changement obligatoire décoché, expiration du compte : jamais | [08-onglet-compte.png](../screenshots/08-onglet-compte.png) |

## Points d'attention
- Les deux options de mot de passe choisies simplifient les tests du laboratoire. En entreprise, on impose le changement du mot de passe à la première connexion.
- Les comptes de test partagent un mot de passe de laboratoire unique, conservé hors du dépôt. En production, chaque utilisateur a son propre mot de passe.
- L'OU est nommée `Comptabilité` avec un accent, alors que les groupes et les identifiants n'en ont pas, car les accents posent parfois problème dans les scripts.

## Difficultés rencontrées
- Faute de frappe dans l'identifiant : `clair.martin` au lieu de `claire.martin`, repérée avant de valider. Le champ « antérieur à Windows 2000 » reprenait la faute et a été corrigé en même temps. Preuve : [08-erreur-identifiant.png](../screenshots/08-erreur-identifiant.png)

## Prochaine étape
Ajouter chaque utilisateur au groupe de son service et vérifier l'appartenance.
