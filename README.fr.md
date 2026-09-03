🇫🇷 Version française | [🇬🇧 English version](README.md)

---

# 📋 Trello Board Init

> *CLI Python pour générer automatiquement des boards Trello depuis un fichier Markdown — dry-run, idempotence, logs horodatés.*

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/github/license/MarvinLeRouge/Trello-Board-Init)

## Concept

Importe automatiquement une todo list au format Markdown dans un board Trello, en créant le board, les listes, les labels et les cartes via l'API Trello.

---

## Fonctionnalités

- Création automatique du board et des 4 listes par défaut (`📥 Backlog`, `📅 Cette semaine`, `🔄 En cours`, `✅ Done`)
- Création des labels avec couleurs (explicites ou assignées automatiquement depuis la palette Trello)
- Toutes les cartes sont créées dans `📥 Backlog`
- Dry-run automatique avant toute action réelle, avec confirmation interactive
- Vérification de cohérence des labels (auto-correction du fichier si un label manque dans le header)
- Idempotence : bloque si des cartes existent déjà dans le Backlog
- Suppression des labels vides créés par défaut par Trello sur les nouveaux boards
- Logging horodaté dans `logs/`
- Support d'un board existant via `--board-id`

---

## Installation

**Prérequis** : Python 3.9+

```bash
# Cloner le projet
git clone https://github.com/MarvinLeRouge/Trello-Board-Init.git
cd Trello-Board-Init

# Créer et activer un environnement virtuel
uv venv --python 3.9
source .venv/bin/activate

# Installer les dépendances
uv pip install -r requirements.txt
```

---

## Configuration

Créer un fichier `.env` à la racine du projet (ne jamais le committer) :

```bash
TRELLO_API_KEY=ta_clé_api
TRELLO_TOKEN=ton_token
```

**Obtenir les credentials Trello :**

1. Clé API : https://trello.com/app-key
2. Token : générer depuis l'URL suivante (remplacer `{TA_API_KEY}`) :

```
https://trello.com/1/authorize?expiration=never&scope=read,write&response_type=token&key={TA_API_KEY}
```

Un fichier `.env.example` est fourni comme modèle :
```bash
cp .env.example .env
```

---

Pour le format complet des tâches Markdown, des exemples d'utilisation, le déroulement d'exécution et des exemples de logs, voir le [Guide d'utilisation](docs/guides/user_guide.fr.md).

---

## Structure du projet

```
Trello-Board-Init/
├── tbi.py                 # Script principal
├── requirements.txt       # Dépendances Python
├── .env.example           # Modèle de configuration
├── .env                   # Credentials (non versionné)
├── .gitignore
├── logs/                  # Fichiers de log (non versionnés)
└── src/                   # Répertoire recommandé pour vos fichiers .md
    └── example_tasks.md   # Fichier exemple (seul fichier src versionné)
```

Le dossier `src/` est la convention adoptée dans ce projet pour centraliser les fichiers `.md` à traiter. Il n'a aucune valeur fonctionnelle — le script accepte n'importe quel chemin en argument. Vous pouvez stocker vos fichiers ailleurs.

### `.gitignore` recommandé

```
.env
.venv/
logs/
__pycache__/
*.pyc
src/*
!src/example_tasks.md
```

---

## Dépendances

```
requests
pyyaml
python-dotenv
```

---

## Choix techniques

Pas de CI ni de tests automatisés : outil CLI local sans logique métier complexe, le coût de mise en place serait disproportionné par rapport à la valeur apportée.

---

## Limites connues

- Toutes les cartes atterrissent dans `📥 Backlog` (pas de ciblage d'une autre liste depuis le `.md`)
- L'API Trello est limitée à 300 requêtes / 10 secondes — largement suffisant pour un usage normal
- Le plan gratuit Trello est limité à 10 boards actifs par workspace

---

## 📋 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.
