# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

MOD CLI (`modcli`) is a command-line tool for MOD Audio plugin developers. It handles authentication with MOD services and publishes LV2 plugin bundles to the MOD platform.

## Development Commands

```bash
# Install for development (using uv)
uv pip install -e ".[dev]"

# Run the CLI
modcli --help

# Run linting
pylint modcli/
```

## Architecture

The CLI is built with Click and organized into command groups:
- `modcli auth` - Authentication (login, login-sso, active-token)
- `modcli bundle` - LV2 bundle operations (publish)
- `modcli config` - Environment management (add-env, set-active-env, list, clear-context)

**Key modules:**
- `cli.py` - Click command definitions and entry point
- `config.py` - `CliContext` and `EnvSettings` classes for managing multi-environment configuration stored in `~/.config/modcli/`
- `auth.py` - Authentication logic including SSO flow with local HTTP callback server
- `bundle.py` - Buildroot package preparation and submission to MOD pipeline API
- `http.py` - Simple HTTP client wrapper using urllib (no external dependencies)
- `settings.py` - Default environment URLs (labs, dev)

**Configuration flow:** `modcli/__init__.py` initializes `context` by reading from `config.py`, which persists JWT tokens and environment settings to `~/.config/modcli/context.json`.

## Code Conventions

- Command names use hyphens (e.g., `login-sso`, `add-env`). The CLI normalizes underscores to hyphens via `token_normalize_func`.
- Python 3.10+ required
- No external HTTP library - uses stdlib `urllib`
