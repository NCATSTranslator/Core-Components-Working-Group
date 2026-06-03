# Core-Components-Working-Group

This repository contains tooling for the Translator platform's Core Components Working Group.

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `translator-components-diagram/` | Generates Graphviz dependency diagrams from the Translator components Google Sheet. See its own `CLAUDE.md` for full details. |

## Workflow notes for Claude

- After making code changes in `translator-components-diagram/`, do **not** run `uv run generate-diagram` — the user will run the script themselves. Only run `uv run pytest` to check for test failures.
- The active branch for diagram work is `add-translator-components-diagrams-code`; PRs target `main`.
