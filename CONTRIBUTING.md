[🇫🇷 Version française](CONTRIBUTING.fr.md) | 🇬🇧 English version

---

# Contributing to Trello Board Init

This is primarily a personal project. External contributions (bug reports, fixes, small improvements) are welcome but limited in scope.

## Prerequisites

Python 3.9+. [uv](https://github.com/astral-sh/uv) is recommended for environment and dependency management, but plain `venv`/`pip` also work.

## Local setup

```bash
git clone https://github.com/MarvinLeRouge/Trello-Board-Init.git
cd Trello-Board-Init
uv venv --python 3.9
source .venv/bin/activate
uv pip install -r requirements.txt
```

## Running tests

There is no automated test suite. See [Technical choices](README.md#technical-choices) in the README for why: this is a local CLI tool with no complex business logic, and the setup cost would be disproportionate to the value delivered. Manually exercise the `--dry-run` flag against a real or throwaway Trello board before submitting a change.

## Workflow

1. Fork the repository and create a branch off `main`.
2. Make your change, keeping it consistent with the existing style.
3. Commit following the convention below.
4. Push and open a pull request against `main`.

## Branch naming

| Type | Prefix |
|---|---|
| Feature | `feat/short-description` |
| Bug fix | `fix/short-description` |
| Chore | `chore/short-description` |
| Documentation | `docs/short-description` |
| Refactor | `refactor/short-description` |
| Tests | `test/short-description` |

Use lowercase kebab-case. No special characters.

## Commit convention

Follow [Conventional Commits](https://www.conventionalcommits.org/), imperative mood, lowercase summary, no trailing period, with a mandatory `Modified files:` section:

```
<type>(<optional scope>): <short summary>

Modified files:
- path/to/file-a.ext - what was changed
- path/to/file-b.ext - what was changed
```

Types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `style`, `perf`, `ci`.

## Code style

No automated linter or formatter is configured. Follow the existing style in `tbi.py`: PEP 8 conventions, type hints on function signatures, and a module-level docstring.

## Code of Conduct

This project follows a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold it.

## License

By contributing, you agree that your contributions will be licensed under the project's license (see [LICENSE](LICENSE)).
