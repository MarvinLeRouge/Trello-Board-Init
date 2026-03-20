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
git clone https://github.com/ton-user/trelloBoardInit.git
cd trelloBoardInit

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

## Format du fichier Markdown

Le fichier est composé d'un **header global** suivi des **cartes individuelles**, chacune séparée par un bloc YAML front matter.

### Header global

```yaml
---
board: Nom du Board
labels:
  - backend                  # couleur assignée automatiquement
  - name: urgent             # couleur assignée automatiquement
  - name: design
    color: purple            # couleur explicite
---
```

**Couleurs disponibles :**
`green` `yellow` `orange` `red` `purple` `blue` `sky` `lime` `pink` `black`

### Cartes

```markdown
---
title: Titre de la carte
labels: [backend, urgent]
---
Description longue et libre en Markdown.

On peut écrire des listes, du **gras**, des liens, des blocs de code, etc.
```

### Exemple complet

```markdown
---
board: Mon Projet
labels:
  - name: backend
    color: blue
  - name: urgent
    color: red
  - frontend
---

---
title: Mettre en place l'authentification
labels: [backend, urgent]
---
Implémenter le système de login/logout avec JWT.

- Endpoint `/auth/login`
- Endpoint `/auth/logout`

---
title: Créer la page d'accueil
labels: [frontend]
---
Concevoir et développer la landing page principale.
```

---

## Utilisation

```bash
# Dry-run uniquement (validation sans rien créer)
python tbi.py tasks.md --dry-run

# Dry-run automatique puis confirmation interactive
python tbi.py tasks.md

# Dry-run automatique puis lancement sans confirmation
python tbi.py tasks.md --force

# Cibler un board existant plutôt qu'en créer un nouveau
python tbi.py tasks.md --board-id ABC123XYZ
```

---

## Déroulement du script

Le script s'exécute toujours en **4 passes**, précédées d'un dry-run automatique :

| Passe | Action |
|-------|--------|
| 0 — Validation | Parsing du fichier, cohérence des labels, détection de doublons |
| 1 — Board | Création ou réutilisation du board + listes par défaut |
| 1.5 — Nettoyage | Suppression des labels vides Trello (board neuf uniquement) |
| 2 — Labels | Création des labels manquants avec leurs couleurs |
| 3 — Cartes | Création des cartes dans `📥 Backlog` avec leurs labels |

---

## Logs

Un fichier de log horodaté est créé dans `logs/` à chaque exécution :

```
logs/tbi_20260303_143000.log
```

Exemple de sortie :

```
2026-03-03 14:30:01  INFO     >>> Running dry-run first...
2026-03-03 14:30:01  INFO     Passe 0 — Parsing & validation
2026-03-03 14:30:01  INFO        Board   : Mon Projet
2026-03-03 14:30:01  INFO        Labels  : ['backend', 'urgent', 'frontend']
2026-03-03 14:30:01  INFO        Cards   : 12 found
2026-03-03 14:30:01  INFO     ✅ Label coherence OK
2026-03-03 14:30:01  INFO     ✅ No duplicate card titles in file.
...
2026-03-03 14:30:04  INFO     ============================================================
2026-03-03 14:30:04  INFO     RÉSUMÉ
2026-03-03 14:30:04  INFO       Board             : created
2026-03-03 14:30:04  INFO       Labels créés      : 3
2026-03-03 14:30:04  INFO       Cartes créées     : 12
2026-03-03 14:30:04  INFO       Erreurs           : 0
2026-03-03 14:30:04  INFO       Durée             : 4.21s
```

---

## Structure du projet

```
trelloBoardInit/
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
