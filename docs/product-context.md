[🇫🇷 Version française](product-context.fr.md) | 🇬🇧 English version

---

# Product Context - Trello Board Init

## Concept

Planning a project often starts as a Markdown todo list, and copying it into Trello card by card is tedious and error-prone. Trello Board Init reads that list and builds the board in one run: board, default lists, colored labels and one card per task, all in the Backlog. A dry run shows what will happen before anything is created.

## Target user

A single developer turning their own Markdown plan into a Trello board, from the command line, with their own Trello API key and token. No hosted service, no multi-user features.

## Core vocabulary

| Term | Meaning |
|---|---|
| `task file` | The Markdown file describing labels (header) and tasks (cards), see the [user guide](guides/user_guide.md) |
| `Backlog` | The `📥 Backlog` list where every card is created; the other default lists are `📅 This week`, `🔄 In progress`, `✅ Done` |
| `dry run` | Automatic preview of every action, followed by an interactive confirmation |
| `label coherence check` | Verification that every label used by a task is declared in the header, with an automatic fix of the file |

## Features

- **Board scaffolding** - new board with the four default lists, or an existing board through `--board-id`
- **Labels** - created with explicit colors or auto-assigned from the Trello palette; Trello's empty default labels removed
- **Cards** - one per task, all in the Backlog
- **Safety** - automatic dry run and confirmation, idempotence (aborts if the Backlog already has cards)
- **Logs** - timestamped log of each run in `logs/`

## Non-goals

- Syncing the board back to the Markdown file, or updating cards after creation: it is a one-shot import
- Managing cards over time (moving, closing, assigning)
- Other boards than Trello
