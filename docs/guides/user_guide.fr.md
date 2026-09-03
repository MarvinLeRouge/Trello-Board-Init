🇫🇷 Version française | [🇬🇧 English version](user_guide.md)

---

# Guide d'utilisation

Référence complète du format de fichier Markdown, de l'utilisation du script, du déroulement d'exécution et des logs. Pour l'installation et la configuration rapide, voir le [README principal](../../README.fr.md).

---

## Obtenir les credentials Trello

1. Clé API : https://trello.com/app-key
2. Token : générer depuis l'URL suivante (remplacer `{TA_API_KEY}`) :

```
https://trello.com/1/authorize?expiration=never&scope=read,write&response_type=token&key={TA_API_KEY}
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
    color: purple             # couleur explicite
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
