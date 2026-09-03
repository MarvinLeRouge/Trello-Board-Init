[🇫🇷 Version française](README.fr.md) | 🇬🇧 English version

---

# 📋 Trello Board Init

> *Python CLI to scaffold Trello boards from a Markdown file — dry-run, idempotence, timestamped logs.*

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/github/license/MarvinLeRouge/Trello-Board-Init)

## Concept

Automatically imports a Markdown todo list into a Trello board, creating the board, lists, labels and cards via the Trello API.

---

## Features

- Automatic board creation with 4 default lists (`📥 Backlog`, `📅 This week`, `🔄 In progress`, `✅ Done`)
- Label creation with colors (explicit or auto-assigned from the Trello palette)
- All cards are created in `📥 Backlog`
- Automatic dry-run before any real action, with interactive confirmation
- Label coherence check (auto-fixes the file if a label is missing from the header)
- Idempotence: aborts if cards already exist in the Backlog
- Removes empty default labels created by Trello on new boards
- Timestamped logging in `logs/`
- Support for an existing board via `--board-id`

---

## Installation

**Requirements**: Python 3.9+

```bash
# Clone the project
git clone https://github.com/MarvinLeRouge/Trello-Board-Init.git
cd Trello-Board-Init

# Create and activate a virtual environment
uv venv --python 3.9
source .venv/bin/activate

# Install dependencies
uv pip install -r requirements.txt
```

---

## Configuration

Create a `.env` file at the project root (never commit this file):

```bash
TRELLO_API_KEY=your_api_key
TRELLO_TOKEN=your_token
```

**Getting Trello credentials:**

1. API Key: https://trello.com/app-key
2. Token: generate from the following URL (replace `{YOUR_API_KEY}`):

```
https://trello.com/1/authorize?expiration=never&scope=read,write&response_type=token&key={YOUR_API_KEY}
```

A `.env.example` file is provided as a template:
```bash
cp .env.example .env
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

---

## Project structure

```
Trello-Board-Init/
├── tbi.py                 # Main script
├── requirements.txt       # Python dependencies
├── .env.example           # Configuration template
├── .env                   # Credentials (not versioned)
├── .gitignore
├── logs/                  # Log files (not versioned)
└── src/                   # Recommended directory for your .md files
    └── example_tasks.md   # Example file (the only versioned src file)
```

The `src/` directory is the convention used in this project to centralize `.md` files to be processed. It has no functional significance — the script accepts any path as an argument. You are free to store your files elsewhere.

### Recommended `.gitignore`

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

## Dependencies

```
requests
pyyaml
python-dotenv
```

---

## Technical choices

No CI or automated tests: local CLI tool with no complex business logic, the setup cost would be disproportionate to the value delivered.

---

## Known limitations

- All cards land in `📥 Backlog` (no per-card list targeting from the `.md`)
- Trello API is rate-limited to 300 requests / 10 seconds — well within limits for normal use
- The Trello free plan is limited to 10 active boards per workspace

---

## 📋 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
