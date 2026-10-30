# Étape 8 : utilisateurs et groupes de sécurité

## Objectif
Je crée les comptes de l'entreprise fictive dans l'OU de leur service, puis les groupes de sécurité. J'affecte les utilisateurs aux groupes à l'étape 9.

## Notions utilisées
- **Compte de domaine** : compte stocké dans l'annuaire, valable sur toutes les machines du domaine. Un compte local n'existe que sur sa machine.
- **Nom d'ouverture de session** : identifiant de connexion, sans accent ni espace (par exemple `claire.martin`). Le nom complet (Claire Martin) est celui qui s'affiche.
- **Groupe de sécurité global (`GG_`)** : il rassemble les utilisateurs d'un même service. Le préfixe est une convention de nommage.
- **Authentification** : vérifier qui est la personne (identifiant et mot de passe).
- **Autorisation** : décider ce que la personne a le droit de faire, grâce aux groupes et aux permissions.

## Pourquoi les groupes sont dans l'OU `Groupes`
Je range les groupes dans une OU dédiée. Un groupe peut réunir des personnes de plusieurs services, donc je ne le place pas dans l'OU d'un seul service. Je retrouve vite chaque objet : les utilisateurs dans leur service, les groupes dans `Groupes`. Je peux aussi confier la gestion des groupes à une personne sans lui donner accès aux comptes.

## Utilisateurs créés
| OU | Nom complet | Identifiant |
|---|---|---|
| Direction | Claire Martin | `claire.martin` |
| Direction | Paul Durand | `paul.durand` |
| Comptabilité | Sonia Bernard | `sonia.bernard` |
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

J'écris les noms de groupes sans accent, même si l'OU s'appelle `Comptabilité`.

## Réalisation
1. Je fais un clic droit sur l'OU du service, puis **Nouveau** > **Utilisateur**.
2. Je saisis le prénom, le nom et l'identifiant, puis le mot de passe de laboratoire.
3. Je règle les options de compte : « L'utilisateur doit changer le mot de passe à la prochaine ouverture de session » décochée, « Le mot de passe n'expire jamais » cochée.
4. Je crée les 8 utilisateurs, chacun dans l'OU de son service.
5. Je fais un clic droit sur l'OU `Groupes`, puis **Nouveau** > **Groupe**, avec l'étendue Globale et le type Sécurité.
6. Je crée les 4 groupes, puis je vérifie chaque OU.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Menu de création | Clic droit sur l'OU `Direction` | [08-menu-ou-direction.png](../screenshots/08-menu-ou-direction.png) |
| Saisie d'un utilisateur | Claire Martin, identifiant `claire.martin@tssrlab.local`, créée dans `tssrlab.local/Entreprise/Direction` | [08-creation-utilisateur.png](../screenshots/08-creation-utilisateur.png) |
| Récapitulatif | Nom complet Claire Martin, connexion `claire.martin@tssrlab.local`, mot de passe sans expiration | [08-recapitulatif-utilisateur.png](../screenshots/08-recapitulatif-utilisateur.png) |
| OU `Direction` | Claire Martin et Paul Durand | [08-ou-direction.png](../screenshots/08-ou-direction.png) |
| OU `Comptabilité` | Sonia Bernard et Lucas Petit | [08-ou-comptabilite.png](../screenshots/08-ou-comptabilite.png) |
| OU `Informatique` | Amine Roux et Julie Moreau | [08-ou-informatique.png](../screenshots/08-ou-informatique.png) |
| OU `Marketing` | Hugo Lambert (nom tronqué à l'écran) et Emma Girard | [08-ou-marketing.png](../screenshots/08-ou-marketing.png) |
| Menu de création d'un groupe | Clic droit sur l'OU `Groupes` | [08-menu-nouveau-groupe.png](../screenshots/08-menu-nouveau-groupe.png) |
| Création d'un groupe | `GG_Direction`, étendue Globale, type Sécurité, dans `tssrlab.local/Entreprise/Groupes` | [08-creation-groupe.png](../screenshots/08-creation-groupe.png) |
| OU `Groupes` | Quatre groupes de sécurité globaux : `GG_Comptabilite`, `GG_Direction`, `GG_Informatique`, `GG_Marketing` | [08-ou-groupes.png](../screenshots/08-ou-groupes.png) |
| Onglet Compte d'Emma Girard | Identifiant `emma.girard@tssrlab.local`, ancien nom `TSSRLAB\emma.girard`, mot de passe sans expiration, changement obligatoire décoché, expiration du compte : jamais | [08-onglet-compte.png](../screenshots/08-onglet-compte.png) |
| Instantané | `08-utilisateurs-groupes-crees` | [08-snapshot.png](../screenshots/08-snapshot.png) |

## Points d'attention
- Les deux options de mot de passe choisies simplifient les tests du laboratoire. En entreprise, on impose le changement du mot de passe à la première connexion.
- Les comptes de test partagent un mot de passe de laboratoire, différent de celui du compte Administrateur. Je le conserve hors du dépôt. En production, chaque utilisateur a son propre mot de passe.
- L'OU est nommée `Comptabilité` avec un accent, alors que les groupes et les identifiants n'en ont pas, car les accents posent parfois problème dans les scripts.

## Difficultés rencontrées
- J'ai tapé `clair.martin` au lieu de `claire.martin` dans l'identifiant. Je l'ai repéré avant de valider. Le champ « antérieur à Windows 2000 » reprenait la faute, et je l'ai corrigé en même temps. Preuve : [08-erreur-identifiant.png](../screenshots/08-erreur-identifiant.png)

## Prochaine étape
J'ajoute chaque utilisateur au groupe de son service et je vérifie l'appartenance.
