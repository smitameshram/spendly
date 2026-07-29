# Spec: Registration

## Overview
Implement user registration so new visitors can create a Spendly account. This step wires up the `POST /register` handler, adds the necessary DB helpers (`create_user`, `get_user_by_email`), validates form input server-side, hashes the password with werkzeug, and redirects on success. The template and schema already exist from Step 1; this step connects them.

## Depends on
- Step 01 — Database Setup (`users` table, `get_db()`, schema in place)

## Routes
- `GET /register` — render the registration form — public (already exists, needs `methods` updated)
- `POST /register` — process form submission, create user, redirect to login — public

## Database changes
No new tables or columns. Two new helper functions are needed in `database/db.py`:

- `get_user_by_email(email: str) -> sqlite3.Row | None` — returns the matching user row or `None`
- `create_user(name: str, email: str, password_hash: str) -> None` — inserts a new user row

The `users` table already has `UNIQUE NOT NULL` on `email`, so duplicate email is enforced at the DB level.

## Templates
- **Modify:** `templates/register.html` — no HTML changes required; the `{% if error %}` block and all three input `name` attributes (`name`, `email`, `password`) are already correct.

## Files to change
- `app.py` — add `POST` to `/register` route, import `request` and `redirect` and `url_for` from flask, import `generate_password_hash` from werkzeug, import the two new DB helpers
- `database/db.py` — add `get_user_by_email()` and `create_user()`

## Files to create
None.

## New dependencies
No new dependencies.

## Rules for implementation
- No SQLAlchemy or ORMs — raw `sqlite3` only
- Parameterised queries only — never f-strings in SQL
- Passwords hashed with `werkzeug.security.generate_password_hash` before inserting
- Use `abort()` for unexpected HTTP errors, not bare string returns
- All templates extend `base.html`
- Use CSS variables — never hardcode hex values
- DB logic stays in `database/db.py` — the route function only calls helpers
- The route must handle these validation cases server-side (re-render form with `error=`):
  - Any field is empty / whitespace-only
  - Password is fewer than 8 characters
  - Email is already registered (catch the result of `get_user_by_email`)
- On success: redirect to `url_for('login')` — do not render a template
- Do not log the user in automatically after registration (session management is a later step)

## Definition of done
- [ ] `GET /register` still renders the form (no regression)
- [ ] Submitting the form with all valid fields creates a new row in `users` and redirects to `/login`
- [ ] Password stored in DB is a werkzeug hash, never plaintext
- [ ] Submitting with an empty name, email, or password re-renders the form with a visible error message
- [ ] Submitting with a password shorter than 8 characters re-renders the form with a visible error message
- [ ] Submitting with an already-registered email re-renders the form with a visible error message
- [ ] No 405 Method Not Allowed error when the form is submitted
- [ ] `create_user` and `get_user_by_email` live in `database/db.py`, not in `app.py`
