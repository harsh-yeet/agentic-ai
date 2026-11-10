# Day 1 — Professional Python Environment Setup

## Session goal

Set up a repeatable Python development environment, organise the project, verify it with a test, and publish the initial repository to GitHub.

## Environment verified

- Python: `3.11.5`
- Git: `2.45.2.windows.1`
- Virtual environment: `.venv`
- pip in the virtual environment: `26.3`
- Test runner: `pytest 9.2.0`

On Windows, prefer `py -3.11` when choosing an exact installed Python version. After activating `.venv`, use `python -m ...` so commands use the virtual environment's interpreter and packages.

## Virtual environments and dependencies

`.venv` isolates this project's Python packages from other projects and the system Python. This prevents incompatible dependency versions from colliding.

Dependency types:

- Direct dependency: imported and used by this project.
- Transitive dependency: installed because a direct dependency needs it.
- Runtime dependency: required when the application runs.
- Development dependency: required for development or CI, such as `pytest`, but not for normal application use.

`pytest` is declared in the `dev` optional-dependency group and installed with:

```powershell
python -m pip install -e ".[dev]"
```

`-e` means editable installation: Python links the installed package to the working directory, so source-code changes are used without reinstalling.

## Project structure

```text
agentic-ai/
├── src/
│   └── agentic_ai/
│       └── __init__.py
├── tests/
│   └── test_package.py
├── notes/
├── .venv/             # local only; ignored by Git
├── .gitignore
├── pyproject.toml
├── README.md
└── AGENTS.md
```

The repository/distribution name can use a hyphen (`agentic-ai`), but Python import names cannot. Therefore, the package is imported as `agentic_ai`.

The `src` layout keeps source code separate from repository-root files and helps tests validate the installed package rather than accidentally importing from the current folder.

## Configuration files

### `pyproject.toml`

The standard Python project configuration file. It defines:

- the build backend (`setuptools`),
- package metadata such as name and version,
- the minimum Python version (`>=3.11`),
- runtime and optional development dependencies,
- package discovery under `src`.

TOML uses section headers such as `[project]` and assignments such as `name = "agentic-ai"`. It does not use JSON-style braces or colons.

### `.gitignore`

Prevents generated or sensitive files from being added to Git. This project ignores:

- `.venv/`
- `__pycache__/` and `*.pyc`
- `.pytest_cache/`
- `build/`, `dist/`, and `*.egg-info/`
- `.env`

`.gitignore` does not remove a file that has already been committed. If a credential is exposed, rotate or revoke it first, then remove it and handle history cleanup deliberately.

### `README.md`

The human-facing introduction to the project, displayed by GitHub. Markdown headings use `#`, for example `# Agentic AI`.

## Testing and verification

The smoke test in `tests/test_package.py` imports `agentic_ai` and confirms the package can be discovered.

Run tests with:

```powershell
python -m pytest
```

This passed successfully: one test collected and one test passed. Pytest first collects/imports test files, then executes discovered functions beginning with `test_`. A syntax error stops collection before tests run.

## Git and GitHub

- Created a local Git repository on the `main` branch.
- Configured the intended account as this repository's local Git author identity.
- Added and reviewed files in the staging area.
- Created the first local commit: `chore: initialize Python project`.
- Added the public GitHub repository as the `origin` HTTPS remote.
- Cleared the old cached GitHub HTTPS credentials in Windows Credential Manager.
- Pushed `main` to GitHub and configured the upstream branch with `git push -u origin main`.

Git concepts:

- Working directory: current files on disk.
- Staging area: the exact snapshot proposed for the next commit.
- Commit: an immutable snapshot in local Git history.
- Remote: a named URL, such as `origin`, pointing to a GitHub repository.
- Upstream: the default remote branch used by later `git push` and `git pull` commands.

## Mistakes diagnosed and fixed

- Used invalid TOML syntax (`:` and braces) instead of `key = value` sections.
- Used `[tools...]` rather than the recognized `[tool...]` namespace.
- Used JavaScript-style `function` rather than Python's `def` in a pytest test.
- Tried to push with a cached credential for the wrong GitHub account, causing a `403` permission error.

## Revision targets

- Recreate `pyproject.toml` without a template.
- Learn dependency locking and when to use `requirements.txt`, `pip freeze`, or a dedicated lock-file tool.
- Practice managing multiple GitHub accounts on one machine.
- Repeat this setup independently in a fresh practice repository.
