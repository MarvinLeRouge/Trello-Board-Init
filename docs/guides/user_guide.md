[🇫🇷 Version française](user_guide.fr.md) | 🇬🇧 English version

---

# User Guide

Full reference for the Markdown task format, script usage, execution flow, and logging. For installation and quick configuration, see the [main README](../../README.md).

---

## Getting Trello credentials

1. API Key: https://trello.com/app-key
2. Token: generate from the following URL (replace `{YOUR_API_KEY}`):

```
https://trello.com/1/authorize?expiration=never&scope=read,write&response_type=token&key={YOUR_API_KEY}
```

---

## Markdown file format

The file consists of a **global header** followed by **individual cards**, each separated by a YAML front matter block.

### Global header

```yaml
---
board: My Board Name
labels:
  - backend                  # auto-assigned color
  - name: urgent             # auto-assigned color
  - name: design
    color: purple            # explicit color
---
```

**Available colors:**
`green` `yellow` `orange` `red` `purple` `blue` `sky` `lime` `pink` `black`

### Cards

```markdown
---
title: Card title
labels: [backend, urgent]
---
Free-form description in Markdown.

You can write lists, **bold text**, links, code blocks, etc.
```

### Full example

```markdown
---
board: My Project
labels:
  - name: backend
    color: blue
  - name: urgent
    color: red
  - frontend
---

---
title: Set up authentication
labels: [backend, urgent]
---
Implement JWT-based login/logout system.

- Endpoint `/auth/login`
- Endpoint `/auth/logout`

---
title: Create home page
labels: [frontend]
---
Design and develop the main landing page.
```

---

## Usage

```bash
# Dry-run only (validation without creating anything)
python tbi.py tasks.md --dry-run

# Automatic dry-run then interactive confirmation
python tbi.py tasks.md

# Automatic dry-run then run without confirmation
python tbi.py tasks.md --force

# Target an existing board instead of creating a new one
python tbi.py tasks.md --board-id ABC123XYZ
```

---

## How the script works

The script always runs in **4 passes**, preceded by an automatic dry-run:

| Pass | Action |
|------|--------|
| 0 — Validation | File parsing, label coherence check, duplicate detection |
| 1 — Board | Create or reuse board + default lists |
| 1.5 — Cleanup | Remove empty default Trello labels (new boards only) |
| 2 — Labels | Create missing labels with their colors |
| 3 — Cards | Create cards in `📥 Backlog` with their labels |

---

## Logs

A timestamped log file is created in `logs/` on each run:

```
logs/tbi_20260303_143000.log
```

Sample output:

```
2026-03-03 14:30:01  INFO     >>> Running dry-run first...
2026-03-03 14:30:01  INFO     Pass 0 — Parsing & validation
2026-03-03 14:30:01  INFO        Board   : My Project
2026-03-03 14:30:01  INFO        Labels  : ['backend', 'urgent', 'frontend']
2026-03-03 14:30:01  INFO        Cards   : 12 found
2026-03-03 14:30:01  INFO     ✅ Label coherence OK
2026-03-03 14:30:01  INFO     ✅ No duplicate card titles in file.
...
2026-03-03 14:30:04  INFO     ============================================================
2026-03-03 14:30:04  INFO     SUMMARY
2026-03-03 14:30:04  INFO       Board             : created
2026-03-03 14:30:04  INFO       Labels created    : 3
2026-03-03 14:30:04  INFO       Cards created     : 12
2026-03-03 14:30:04  INFO       Errors            : 0
2026-03-03 14:30:04  INFO       Duration          : 4.21s
```
