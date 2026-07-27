# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Spendly** — a Flask-based expense tracker web application. Currently a learning project with a working landing/auth page skeleton and a placeholder database layer.

## Commands

```bash
# Set up environment
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Run the development server (port 5001)
python app.py

# Run tests
pytest

# Run a single test file
pytest tests/test_foo.py

# Run a specific test
pytest tests/test_foo.py::test_function_name
```

## Architecture

**Stack:** Flask 3.1.3 + Jinja2 templates + plain CSS/JS + SQLite (in progress)

### Routing (`app.py`)
All routes live in a single `app.py`. Current routes: `/`, `/register`, `/login`, `/terms`, `/privacy`. Expense management routes are planned but not yet implemented.

### Templates (`templates/`)
Jinja2 templates extending `base.html`. The base provides a navbar + footer and defines four blocks pages can override:
- `{% block title %}` — page title
- `{% block head %}` — extra `<head>` content (CSS, meta)
- `{% block content %}` — main page body
- `{% block scripts %}` — page-specific JS at bottom

### Database (`database/db.py`)
Placeholder module for SQLite. Will expose `get_db()`, `init_db()`, and `seed_db()`. The database file will be `expense_tracker.db` (git-ignored).

### CSS Design System (`static/css/style.css`)
CSS custom properties defined at `:root`:
- **Colors:** `--ink-*` (dark scale), `--paper-*` (light scale), `--accent-teal` (`#1a472a`), `--accent-orange` (`#c17f24`), `--danger` (`#c0392b`)
- **Typography:** DM Serif Display (headings), DM Sans (body) via Google Fonts
- **Layout:** `--max-width: 1200px`, `--auth-width: 440px`
- **Radius:** `--radius-sm` (6px), `--radius-md` (12px), `--radius-lg` (20px)
