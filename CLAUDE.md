# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project state

This is an early-stage Python project, scaffolded with `uv init --package`, for an agent built on the Anthropic API. The README is empty, and there are no tests, linter or formatter configured yet.

## Commands

The project is managed with [uv](https://docs.astral.sh/uv/). Python 3.14 is required (pinned in `.python-version`).

```bash
uv sync                      # install deps into .venv from uv.lock
uv run main.py               # run the Anthropic API prototype script
uv run tennis-racket-agent   # run the packaged entry point (src/tennis_racket_agent:main)
uv add <package>             # add a dependency (updates pyproject.toml and uv.lock)
```

## Structure

There are two entry points that are not connected yet:

- `main.py` (repo root) is a standalone prototype. It loads `.env` with `python-dotenv`, creates an `anthropic.Anthropic()` client and sends a single `messages.create` call using `claude-haiku-5-5`. This is where the Claude API code currently lives.
- `src/tennis_racket_agent/__init__.py` defines `main()`, which `[project.scripts]` in `pyproject.toml` exposes as the `tennis-racket-agent` command. It is still the uv placeholder that prints a greeting.

Packaged agent code belongs under `src/tennis_racket_agent/` (uv_build src layout), so logic moved out of `main.py` should go there and be reached through `main()`.

## Configuration

`ANTHROPIC_API_KEY` is read from `.env`, which is gitignored. The Anthropic client picks it up from the environment once `load_dotenv()` has run, so any new entry point must call `load_dotenv()` before creating the client.
