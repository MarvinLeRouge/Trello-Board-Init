[🇫🇷 Version française](SECURITY.fr.md) | 🇬🇧 English version

---

# Security Policy

## Reporting a vulnerability

This is a small personal project with no dedicated security team. If you find a security issue, please open a [GitHub issue](https://github.com/MarvinLeRouge/Trello-Board-Init/issues) describing the problem. Avoid including real Trello API keys, tokens, or other secrets in the report.

There is no fixed response time guarantee, but reports will be reviewed and addressed as soon as possible.

## Credential handling

This tool reads Trello API credentials (`TRELLO_API_KEY`, `TRELLO_TOKEN`) from a local `.env` file, loaded via `python-dotenv`. To keep them safe:

- Never commit `.env` — it is excluded by `.gitignore`; only `.env.example` (a template with no real values) is tracked.
- Treat your Trello token like a password: it grants read/write access to your boards. Regenerate it from your [Trello account settings](https://trello.com/app-key) if it is ever exposed.
- Avoid pasting real credentials into scratch notes, chat logs, or any file that isn't already covered by `.gitignore`.

## Scope

This project only talks to the official Trello REST API over HTTPS via the `requests` library. It does not run a server and does not expose any network-facing endpoint of its own.
