🇫🇷 Version française | [🇬🇧 English version](CONTRIBUTING.md)

---

# Contribuer à Trello Board Init

Il s'agit avant tout d'un projet personnel. Les contributions externes (rapports de bugs, corrections, petites améliorations) sont bienvenues mais dans une portée limitée.

## Prérequis

Python 3.9+. [uv](https://github.com/astral-sh/uv) est recommandé pour la gestion de l'environnement et des dépendances, mais `venv`/`pip` classiques fonctionnent aussi.

## Installation locale

```bash
git clone https://github.com/MarvinLeRouge/Trello-Board-Init.git
cd Trello-Board-Init
uv venv --python 3.9
source .venv/bin/activate
uv pip install -r requirements.txt
```

## Lancer les tests

Il n'y a pas de suite de tests automatisés. Voir la section [Choix techniques](README.fr.md#choix-techniques) du README pour la raison : c'est un outil CLI local sans logique métier complexe, et le coût de mise en place serait disproportionné par rapport à la valeur apportée. Testez manuellement l'option `--dry-run` sur un board Trello réel ou jetable avant de soumettre une modification.

## Déroulement

1. Forker le dépôt et créer une branche à partir de `main`.
2. Faire la modification, en restant cohérent avec le style existant.
3. Commiter en suivant la convention ci-dessous.
4. Pousser et ouvrir une pull request vers `main`.

## Nommage des branches

| Type | Préfixe |
|---|---|
| Fonctionnalité | `feat/description-courte` |
| Correction | `fix/description-courte` |
| Maintenance | `chore/description-courte` |
| Documentation | `docs/description-courte` |
| Refactoring | `refactor/description-courte` |
| Tests | `test/description-courte` |

Minuscules, kebab-case, sans caractères spéciaux.

## Convention de commit

Suivre [Conventional Commits](https://www.conventionalcommits.org/), impératif, minuscules, sans point final, avec une section `Modified files:` obligatoire :

```
<type>(<scope optionnel>): <résumé court>

Modified files:
- chemin/vers/fichier-a.ext - ce qui a été modifié
- chemin/vers/fichier-b.ext - ce qui a été modifié
```

Types : `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `style`, `perf`, `ci`.

## Style de code

Aucun linter ou formatter automatisé n'est configuré. Suivre le style existant dans `tbi.py` : conventions PEP 8, type hints sur les signatures de fonction, et une docstring de module.

## Code de conduite

Ce projet suit un [Code de conduite](CODE_OF_CONDUCT.fr.md). En participant, vous vous engagez à le respecter.

## Licence

En contribuant, vous acceptez que vos contributions soient distribuées sous la licence du projet (voir [LICENSE](LICENSE)).
