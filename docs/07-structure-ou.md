# Étape 7 : structure d'unités d'organisation

## Objectif
Organiser l'annuaire `tssrlab.local` comme une petite entreprise fictive, en créant une arborescence d'unités d'organisation (OU).

## Entreprise fictive
Petite société avec quatre services : Direction, Comptabilité, Informatique et Marketing. Les utilisateurs et les groupes de sécurité seront créés à l'étape suivante.

## Notions utilisées
- **OU (unité d'organisation)** : dossier de l'annuaire. Elle sert à ranger les objets et à leur appliquer des stratégies de groupe (GPO).
- **Groupe de sécurité** : liste de membres à laquelle on donne des permissions.
- **Conteneur par défaut** : les dossiers `Users` et `Computers` existent déjà, mais on ne peut pas leur appliquer de GPO. D'où l'intérêt de créer ses propres OU.
- **Protection contre la suppression accidentelle** : case cochée par défaut, qui empêche de supprimer une OU par erreur.

| | OU | Groupe de sécurité |
|---|---|---|
| Rôle | Ranger et appliquer des GPO | Donner des permissions |
| Un utilisateur peut être dans… | Une seule OU | Plusieurs groupes |

**Pourquoi un utilisateur n'est que dans une OU, mais dans plusieurs groupes :** une OU est un dossier, et un objet n'est rangé qu'à un seul endroit. Un groupe est une liste de membres, et une personne peut figurer sur plusieurs listes. Par exemple, un technicien est dans l'OU Informatique et peut être membre de plusieurs groupes.

## Structure créée

    tssrlab.local
    └── Entreprise
        ├── Direction
        ├── Comptabilité
        ├── Informatique
        ├── Marketing
        ├── Groupes
        └── Ordinateurs

- `Entreprise` : OU racine, qui regroupe tout et sépare les objets des conteneurs par défaut.
- Une OU par service : elle contiendra les utilisateurs du service.
- `Groupes` : elle contiendra les groupes de sécurité.
- `Ordinateurs` : elle accueillera le poste client.

## Réalisation
1. Ouverture de Outils > Utilisateurs et ordinateurs Active Directory.
2. Clic droit sur `tssrlab.local` > Nouveau > Unité d'organisation : création de `Entreprise`.
3. Clic droit sur `Entreprise` > Nouveau > Unité d'organisation : création des six OU enfants.
4. Protection contre la suppression laissée cochée pour chaque OU.
5. Activation des fonctionnalités avancées dans le menu Affichage pour lire l'onglet Objet.
6. Vérifications et test de suppression.

## Tests et résultats
| Test | Résultat | Preuve |
|---|---|---|
| Console avant les OU | Console ouverte sur `tssrlab.local` | [07-console-avant.png](../screenshots/07-console-avant.png) |
| Menu de création | Nouveau > Unité d'organisation disponible sur le domaine | [07-menu-nouvelle-ou.png](../screenshots/07-menu-nouvelle-ou.png) |
| OU racine | `Entreprise` créée dans `tssrlab.local`, protection cochée | [07-creation-ou-entreprise.png](../screenshots/07-creation-ou-entreprise.png) |
| Création d'une OU enfant | `Comptabilité` créée dans `tssrlab.local/Entreprise`, protection cochée | [07-creation-ou-comptabilite.png](../screenshots/07-creation-ou-comptabilite.png) |
| Arborescence | Six OU sous `Entreprise` | [07-arborescence.png](../screenshots/07-arborescence.png) |
| Liste des OU | Six objets de type « Unité d'organisation » | [07-liste-ou.png](../screenshots/07-liste-ou.png) |
| Onglet Objet | Nom canonique `tssrlab.local/Entreprise/Comptabilité`, créée le 23/10/2026 à 23:03:24, protection cochée | [07-proprietes-objet.png](../screenshots/07-proprietes-objet.png) |
| Test de suppression de `Marketing` | Confirmation demandée | [07-suppression-confirmation.png](../screenshots/07-suppression-confirmation.png) |
| Résultat du test | Suppression refusée | [07-suppression-refus.png](../screenshots/07-suppression-refus.png) |
| Instantané | `07-ou-creees` | [07-snapshot.png](../screenshots/07-snapshot.png) |

## Points d'attention
- L'OU est nommée `Comptabilité` avec un accent. Cela fonctionne dans l'interface graphique. Les accents posent parfois problème dans les scripts, donc les identifiants des utilisateurs et des groupes seront écrits sans accent.
- Le message de refus cite deux causes possibles : privilèges insuffisants ou objet protégé. La session était ouverte en Administrateur et la case de protection était cochée à la création. La protection est donc la cause probable. Hypothèse non confirmée par un second test.
- Les dossiers supplémentaires visibles dans l'arborescence (`Keys`, `LostAndFound`, `System`, etc.) apparaissent à cause des fonctionnalités avancées.

## Difficultés rencontrées
Aucune difficulté technique. Écart avec le plan : l'OU `Comptabilité` a été créée avec un accent, alors que `Comptabilite` était prévu (voir Points d'attention).

## Prochaine étape
Créer les utilisateurs et les groupes de sécurité.
