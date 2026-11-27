🇫🇷 Version française | [🇬🇧 English version](product-context.md)

---

# Contexte produit - Trello Board Init

## Concept

La planification d'un projet commence souvent par une todo list en Markdown, et la recopier carte par carte dans Trello est fastidieux et source d'erreurs. Trello Board Init lit cette liste et construit le board en une seule exécution : board, listes par défaut, labels colorés et une carte par tâche, toutes dans le Backlog. Un dry run montre ce qui va se passer avant toute création.

## Utilisateur cible

Un seul développeur qui transforme son propre plan Markdown en board Trello, en ligne de commande, avec sa propre clé d'API et son propre token Trello. Pas de service hébergé, pas de fonctionnalités multi-utilisateur.

## Vocabulaire

| Terme | Sens |
|---|---|
| `task file` | Le fichier Markdown qui décrit les labels (en-tête) et les tâches (cartes), voir le [guide utilisateur](guides/user_guide.fr.md) |
| `Backlog` | La liste `📥 Backlog` où toutes les cartes sont créées ; les autres listes par défaut sont `📅 This week`, `🔄 In progress`, `✅ Done` |
| `dry run` | Aperçu automatique de chaque action, suivi d'une confirmation interactive |
| `label coherence check` | Vérification que chaque label utilisé par une tâche est déclaré dans l'en-tête, avec correction automatique du fichier |

## Fonctionnalités

- **Création du board** - nouveau board avec les quatre listes par défaut, ou board existant via `--board-id`
- **Labels** - créés avec des couleurs explicites ou attribuées automatiquement dans la palette Trello ; les labels vides créés par défaut par Trello sont supprimés
- **Cartes** - une par tâche, toutes dans le Backlog
- **Sécurité** - dry run et confirmation automatiques, idempotence (abandon si le Backlog contient déjà des cartes)
- **Logs** - journal horodaté de chaque exécution dans `logs/`

## Hors périmètre

- Synchroniser le board vers le fichier Markdown, ou mettre à jour les cartes après création : c'est un import unique
- Gérer les cartes dans le temps (déplacer, clore, assigner)
- D'autres outils que Trello
